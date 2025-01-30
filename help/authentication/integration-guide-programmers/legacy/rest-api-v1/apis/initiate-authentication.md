---
title: Lancer l’authentification
description: Lancer l’authentification
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# (Hérité) Lancer l’authentification {#initiate-authentication}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

>[!NOTE]
>
> L’implémentation de l’API REST est limitée par [mécanisme de limitation](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Points d’entrée de l’API REST {#clientless-endpoints}

&lt;REGGIE_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN> :

* Production - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Évaluation - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Description {#description}

Lance le processus d’authentification en signalant un événement de sélection MVPD. Crée un enregistrement sur la base de données d’authentification d’Adobe Pass, qui est réconcilié lorsqu’une réponse réussie est reçue de MVPD.



| Point d’entrée | Appelé </br>Par | Entrée   </br>Params | HTTP </br>Méthode | Réponse | HTTP </br>Réponse |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | Module AuthN | 1. requestor_id (obligatoire)</br>2.  mso_id (obligatoire)</br>3.  reg_code (obligatoire)</br>4.  domain_name (obligatoire)</br>5.  noflash=true - </br>    (Obligatoire, paramètre résiduel)</br>6.  no_iframe=true (obligatoire, paramètre résiduel)</br>7.  paramètres supplémentaires (facultatif)</br>8.  redirect_url (obligatoire) | GET | L’application web de connexion est redirigée vers la page de connexion de MVPD. | 302 pour les implémentations de redirection complètes |

{style="table-layout:auto"}


| Paramètre d’entrée | Description |
| --- | --- |
| requestor_id | Demandeur programmeur pour lequel cette opération est valide. |
| mso_id | Identifiant MVPD pour lequel cette opération est valide. |
| reg_code | Code d’enregistrement généré par le service Reggie. |
| domain_name | Domaine d’origine. |
| redirect_url | URL de redirection de l’application Web de connexion une fois l’authentification terminée. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Important : paramètres obligatoires -** Quelle que soit l’implémentation côté client, tous les paramètres ci-dessus sont obligatoires.
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
>**Important : paramètres facultatifs**
>
>L’appel peut également contenir des paramètres facultatifs qui activent d’autres fonctionnalités telles que :
>
> * generic\_data - active l’utilisation de [Promotional TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notes** {#notes}

* La valeur du paramètre `domain_name` doit être définie sur l’un des noms de domaine enregistrés avec l’authentification Adobe Pass.

* [Évitez d’utiliser le code ’&amp;’reg\_code dans la requête /authenticate (remarque technique)](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* Le paramètre `redirect_url` doit être le dernier dans l’ordre

* La valeur du paramètre `redirect_url` doit être encodée en URL
