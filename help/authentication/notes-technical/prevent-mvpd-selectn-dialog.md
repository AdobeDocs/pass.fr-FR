---
title: Empêcher l’affichage des MVPD dans la boîte de dialogue de sélection
description: Empêcher l’affichage des MVPD dans la boîte de dialogue de sélection
exl-id: 20faf501-c006-45e2-a725-fb1273ecaffe
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 0%

---

# Empêcher l’affichage des MVPD dans la boîte de dialogue de sélection

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Problème {#issue-prevent-mvpd-sel-dialog}

Vous devez empêcher l’affichage de MVPD spécifiques à (&quot;liste bloquée&quot;) dans le sélecteur MVPD.


## Solution {#solution-prevent-mvpd-sel-dialog}

La solution consiste à créer une liste bloquée lorsque `displayProviderDialog()` est appelé.

Par exemple, si vous souhaitez que CableCompany_1 et CableCompany_2 ne s’affichent pas dans le sélecteur MVPD, vous effectuez une opération similaire à celle présentée dans l’exemple suivant.

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
