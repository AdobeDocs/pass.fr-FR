---
title: Empêcher l'affichage des fichiers MVPD dans la boîte de dialogue de sélection
description: Empêcher l'affichage des fichiers MVPD dans la boîte de dialogue de sélection
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 0%

---

# (Hérité) Empêcher l’affichage des fichiers MVPD dans la boîte de dialogue de sélection

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

## Problème {#issue-prevent-mvpd-sel-dialog}

Vous devez empêcher l’affichage de MVPD spécifiques (« liste bloquée ») dans le sélecteur MVPD.


## Solution {#solution-prevent-mvpd-sel-dialog}

La solution consiste à effectuer une liste bloquée lorsque `displayProviderDialog()` est appelé.

Par exemple, si vous souhaitez que CableCompany_1 et CableCompany_2 ne s’affichent pas dans le sélecteur MVPD, procédez comme illustré dans l’exemple suivant.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if (!isBlocklisted(currentMvpd.ID)) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isBlocklisted(mvpdID) {
    // Implement block-listing on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
} 
```

<!--
**Related Information**

* [Allow MVPDs in the Selection Dialog](/help/authentication/allow-mvpd-selectn-dialog.md)
* **Code samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
