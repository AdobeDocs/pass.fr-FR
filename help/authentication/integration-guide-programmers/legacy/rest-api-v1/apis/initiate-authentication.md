---
title: Lancer l’authentification
description: Lancement de l’authentification
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# Lancer l’authentification {#initiate-authentication}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par le [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points de terminaison de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Description {#description}

Lance le processus d’authentification en informant un événement de sélection MVPD. Crée un enregistrement sur la base de données d’authentification Adobe Pass, qui est réconcilié lorsqu’une réponse réussie est reçue du MVPD.



| Point d’entrée | Appelé </br> | Entrée   </br> Params | Méthode HTTP </br> | Réponse | Réponse HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | Module AuthN | 1. requestor_id (obligatoire)</br>2.  mso_id (obligatoire)</br>3.  reg_code (obligatoire)</br>4.  domain_name (obligatoire)</br>5.  noflash=true - </br>    (Obligatoire, paramètre résiduel)</br>6.  no_iframe=true (obligatoire, paramètre résiduel)</br>7.  paramètres supplémentaires (facultatif)</br>8.  redirect_url (obligatoire) | GET | L’application web de connexion est redirigée vers la page de connexion MVPD. | 302 pour les mises en oeuvre de redirection complètes |

{style="table-layout:auto"}


| Paramètre d’entrée | Description |
| --- | --- |
| requestor_id | Demandeur de programmeur pour lequel cette opération est valide. |
| mso_id | Identifiant MVPD pour lequel cette opération est valide. |
| reg_code | Code d’enregistrement généré par le service Reggie. |
| domain_name | Domaine d’origine. |
| redirect_url | URL de redirection de l’application web de connexion une fois l’authentification terminée. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Important : Paramètres obligatoires -** Quel que soit l’implémentation côté client, tous les paramètres ci-dessus sont obligatoires.
>
>
>Exemple :
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Important : Paramètres facultatifs**
>
>L&#39;appel peut également contenir des paramètres facultatifs qui permettent d&#39;accéder à d&#39;autres fonctionnalités, telles que :
>
> * generic\_data : permet l’utilisation de [TempPass promotionnel](/help/authentication/integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notes** {#notes}

* La valeur du paramètre `domain_name` doit être définie sur l’un des noms de domaine enregistrés avec l’authentification Adobe Pass. Pour plus d&#39;informations, voir [Enregistrement et initialisation](/help/authentication/kickstart/programmer-overview.md).

* [Évitez d’utiliser &#39;&amp;&#39;reg\_code dans /authenticate request (note technique)](/help/authentication/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* Le paramètre `redirect_url` doit être le dernier dans l’ordre.

* La valeur du paramètre `redirect_url` doit être codée au format URL.
