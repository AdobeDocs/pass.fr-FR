---
title: Fonctionnalité Métadonnées utilisateur
description: Fonctionnalité Métadonnées utilisateur
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '1654'
ht-degree: 1%

---

# Métadonnées utilisateur {#user-metadata}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>
</br>

## Introduction {#intro}

La fonction Métadonnées utilisateur permet aux programmeurs d’accéder à différents types de données spécifiques à l’utilisateur gérées par les MVPD.  Les types de métadonnées utilisateur incluent des codes postaux, des évaluations parentales, des ID utilisateur, etc.  Les métadonnées *User* sont une extension des métadonnées *static* disponibles précédemment (jeton d’authentification TTL, jeton d’autorisation TTL et ID d’appareil).


Points clés des métadonnées utilisateur :

- Transmis à l’application du programmeur pendant les flux d’authentification et d’autorisation
- Les valeurs sont enregistrées dans le jeton
- Les valeurs peuvent être normalisées si différents MVPD fournissent des données dans différents formats.
- Certains paramètres peuvent être chiffrés à l’aide de la clé du programmeur (par exemple, le code postal). Voir [Certificat de métadonnées utilisateur pour le chiffrement](./user-metadata-certificate.md) pour la génération du certificat de chiffrement.
- Des valeurs spécifiques sont disponibles par Adobe, via un changement de configuration.

## Obtention des métadonnées utilisateur {#obtaining}

Les métadonnées utilisateur sont disponibles pour les programmeurs via la fonction AccessEnabler `getMetadata()` et via le point d’entrée `/usermetadata` de l’API sans client.  Pour plus d’informations sur l’utilisation de `getMetadata()` et de son rappel correspondant `setMetadataStatus()`, reportez-vous à la documentation de l’API de plateforme (ou pour les points de terminaison et paramètres utilisés dans l’API sans client).

Les programmeurs obtiennent des métadonnées en fournissant une clé pour le type de métadonnées qu’ils souhaitent obtenir : `getMetadata(key)`.

Les métadonnées sont renvoyées comme suit : `setMetadataStatus(key, encrypted, data)` :

| Paramètre | Type | Description |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | Chaîne | Spécifie le type de métadonnées demandé |
| `encrypted` | Booléen | Indicateur booléen indiquant si la &quot;valeur&quot; est chiffrée ou non. Si la valeur est &quot;true&quot;, alors &quot;value&quot; sera en fait une représentation JSON Web Encrypted de la valeur réelle. |
| `data` | Objet | Objet JSON contenant la représentation des métadonnées |



La structure du paramètre de données, les valeurs varient selon les types :

| Clé | Type de valeur | Exemple | Description |
| --- | --- | --- | --- |
| `zip` | Tableau JSON | \[&quot;77754&quot;, &quot;12345&quot;\] | Code postal |
| `householdID` | chaîne JSON | &quot;1o7241p&quot; | Identifiant du foyer. Si le MVPD ne prend pas en charge les sous-comptes, cela sera identique à `userID` |
| `maxRating` | Objet JSON | { MPAA: &quot;NR&quot;, <br> VCHIP: &quot;X&quot;, <br> URL: &quot;http://manage.my/parental&quot; } | Note parentale maximale pour l’utilisateur |
| `userID` | chaîne JSON | &quot;1o7241p&quot; | Identifiant de l’utilisateur. Dans le cas où un MVPD prend en charge des sous-comptes et que l’utilisateur n’est pas le compte principal, `userID` sera différent de `householdID`. |
| `channelID` | Tableau JSON | \[&quot;channel-1&quot;, &quot;channel-2&quot; \] | Liste des canaux qu’un utilisateur est autorisé à afficher |
| `is_hoh` | chaîne JSON | &quot;1&quot; | Indicateur qui identifie si un utilisateur est chef de famille |
| `encryptedZip` | chaîne JSON | &quot;&quot; | Comcast expose le code postal chiffré |
| `typeID` | chaîne JSON | Principal | Indicateur qui identifie si le compte utilisateur est un compte principal/secondaire. |
| `primaryOID` | chaîne JSON | &quot;uuid1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Identifiant du foyer. Si `typeID` est Principal, contient la valeur de la propriété `userID` |
| hba_status | Booléen | &quot;true&quot; &quot;false&quot; | Indicateur booléen indiquant si l’authentification par domicile est activée pour une intégration particulière |
| allowMirroring | Booléen | &quot;true&quot; &quot;false&quot; | Indique si la mise en miroir d’écran est autorisée pour cet appareil ou non |

>[!NOTE]
>
> **Remarque :** Si le paramètre de données est chiffré, comme c’est généralement le cas pour la **clé zip**, la représentation de la clé de métadonnées sera une chaîne JSON au lieu d’un tableau ou d’un objet.


**Important :** Les métadonnées utilisateur réelles disponibles pour un programmeur dépendent de ce qu’un MVPD rend disponible.  Les conventions légales doivent être signées avec les MVPD pour que les métadonnées utilisateur sensibles (comme le code postal) soient mises à disposition dans l’environnement de production.

</br>


| Nom | Détails | Nécessite un chiffrement | Commentaires |
| --- | --- | --- | --- |
| Identifiant utilisateur | Tel que fourni par le MVPD | Non | Il s’agit de la valeur qui est ensuite hachée par Adobe et exposée dans le jeton média et dans le rappel sendTrackingData() .<br><br>Dans ce cas, le hachage a été effectué pour des raisons historiques.<br><br>Cet identifiant peut être un identifiant de foyer ou un identifiant de sous-compte. Cela n’est généralement pas spécifié, il s’agit simplement de l’identifiant associé à la connexion qui était utilisé à l’époque (qui peut être un compte principal ou secondaire). |
| Identifiant utilisateur en amont | Fourni par le MVPD pour être utilisé uniquement pour les flux de surveillance de simultanéité | Non | Cette valeur est utilisée lors de l’application des limites de simultanéité sur les sites et applications MVPD et Programmer. <br><br>L’ID peut également contenir des stratégies de surveillance de simultanéité<br><br>Pour la plupart des MVPD, cette valeur est égale à l’ID utilisateur |
| Identifiant utilisateur du foyer | Fourni par le MVPD pour être utilisé principalement pour les flux de contrôle parental | Non | Identifiant qui permet aux programmeurs de comprendre l’utilisation du foyer par rapport au sous-compte.<br><br>Il est parfois utilisé comme substitut des contrôles parentaux si de vrais évaluations ne sont pas disponibles (si l’utilisateur a été connecté avec le compte du foyer qu’il peut regarder, sinon le contenu noté ne s’affiche pas)<br><br>Il y a beaucoup de variations entre les distributeurs multicanaux de programmes de développement de contenu pour la manière dont cela est représenté : identifiant utilisateur, identifiant du foyer, drapeau du chef de foyer, etc. |
| Chef de foyer | Indicateur signalant si le compte courant est chef de famille ou non | Non | voir ci-dessus |
| ID de type / ID de Principal | Identifiants de compte du foyer | Non | Indicateurs spécifiques AT&amp;T pour le chef de famille.<br><br>ID de type = Indicateur qui identifie si le compte utilisateur est un compte principal/secondaire<br><br>OID de Principal = Identifiant du foyer. Si TypeID est Principal, contient la valeur de userID. |
| Note maximale | Évaluation maximale autorisée pour le compte actuel | Non | Permet aux programmeurs de filtrer le contenu non adapté au compte.<br><br> Comporte des évaluations MPAA ou VCHIP |
| Ligne de canal vers le haut | Liste des canaux disponibles dans le package de l’utilisateur. | Non | Utilisé pour autoriser/supprimer rapidement divers canaux des portails qui agrégent plusieurs réseaux</br></br> *Notez que l’autorisation de contrôle en amont offre généralement plus de flexibilité pour ce cas d’utilisation et doit être utilisée à la place.* <br><br>La spécification OLCA permet cela en tant qu’AttributeStatement dans la réponse AuthN. |
| État de l’adaptateur | Indique si l’authentification a eu lieu via l’adaptateur de bus hôte | Non |     |
| Code postal | Code postal de facturation de l’utilisateur | Oui | Utilisé pour la diffusion ou les événements sportifs<br><br>Peut également être fourni avec la réponse AuthZ pour les mises à jour rapides<br><br>Données sensibles, nécessite des accords juridiques MVPD |
| Code postal chiffré | Code postal de facturation de l’utilisateur (Comcast) | Oui | Comme ci-dessus mais crypté par Comcast |
| Langue | Paramètres de langue de l’utilisateur | Non | Utilisé pour afficher les messages en fonction des préférences de l’utilisateur |
| Autoriser la mise en miroir | Indique si la mise en miroir d’écran est autorisée pour cet appareil ou non | Non |     |



## Métadonnées disponibles {#available_metadata}

Le tableau suivant présente l’état actuel des métadonnées utilisateur dans l’écosystème d’authentification Adobe Pass :


|     | **Legal **<br><br>**Agreement **<br><br>**Signed (zip uniquement)** | **ID utilisateur **<br><br>**sur AuthN** | **Code postal **<br><br>**sur AuthN/Z** | **Évaluation **<br><br>**sur AuthN/Z** | **Ménage **<br><br>**ID sur AuthN/Z** | **ID de canal sur AuthN** | **Chef de foyer sur AuthN** | **ID de type sur AuthN** | **Principal OID sur AuthN** | Langue | ID utilisateur amont **sur AuthN** | État de l’adaptateur | OnNet | inHome | Autoriser la mise en miroir sur AuthZ | **Notes** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Nom officiel** | n/a | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | language | amontUserID | hba_status | onNet | inHome | allowMirroring | 1. Pour AuthN : l’analyseur OiosamlMetadataParser doit être modifié de sorte que tous les analyseurs aient ce nouvel attribut activé <br>2.  Pour AuthZ : il n’existe pas de méthode générique, car l’implémentation des autorisations est spécifique au MVPD. |
| **Chiffrement requis** | n/a | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** |     |
| **Sensitive** | n/a | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** |     |     |
| **Adobe IdP** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui** | **Oui** | **Oui** | **Oui** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | Aucun accord juridique n’est nécessaire. Il peut être activé. |
| **Synacor** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Contrat juridique ne couvrant pas tous les MVPD proxy.**   <br>Il s’agit d’une prise en charge générique de Synacor, qui peut ne pas être cumulée à tous les MVPD. |
| Disposition | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | Partage la même liste que tous les MVPD de syntaxe, plus l’amontUserID. |
| Comcast | **Non** | **Oui** | **Non** | **Oui (AuthZ uniquement)** | **Oui (AuthZ uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** |     |
| **AT&amp;T** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Oui** | **Oui** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | Accord juridique signé. |
| **Cablevision** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | Accord juridique signé. |
| **HTC** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| **Proxy Massilon** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | Accord juridique signé. |
| **Proxy Clearleap** | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthZ uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** | **Non** | Accord juridique signé. |
| Rogers | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| RCN | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| Charte | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| Verizon | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Non** |     |
| Eastlink | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| Proxy GLDS | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| DTV | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| COX | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| Cogeco | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** |     |
| Vidéotron | **Non** | **Oui** | **Oui (AuthN uniquement)** | **Non** | **Oui*** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | Expose le paramètre householdID avec la même valeur que le paramètre userID |
| Spectrum | **Oui** | **Oui** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Oui (AuthN uniquement)** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Oui** | **Non** | **Non** | **Oui** |     |
| **Tous les autres **<br><br>**MVPD** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Non** | **Oui** | **Non** | **Non** | **Non** | **Non** | **Aucun accord juridique pour le moment, métadonnées sensibles non disponibles pour la production.** <br>Pour tous les MVPD, l’ID utilisateur est disponible sans travail supplémentaire. |


La liste des types de métadonnées utilisateur sera étendue à mesure que de nouveaux types seront disponibles et ajoutés au système d’authentification Adobe Pass.

## Exemples de code {#code_samples}

- [Exemple de code 1](#code_sample1)
- [Exemple de code 2 (Mock getMetadata)](#code_sample2)


### Exemple de code 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Exemple de code 2 (Mock getMetadata) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
