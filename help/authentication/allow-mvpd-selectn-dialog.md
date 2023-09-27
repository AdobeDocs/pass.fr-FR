---
title: Autorisation des MVPD dans la boîte de dialogue de sélection
description: Autorisation des MVPD dans la boîte de dialogue de sélection
exl-id: 2c0e0f06-ddc6-4bea-90dc-d7ef8e78d27e
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 0%

---

# Autorisation des MVPD dans la boîte de dialogue de sélection {#allow-mvpds-selection-dialog}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

## Problème {#issue}

Le programmeur peut vouloir tester ou vérifier l’expérience utilisateur des nouvelles intégrations MVPD avant de passer publiquement aux utilisateurs finaux.

## Solution {#solution}

Dans le `displayProviderDialog()` callback, l’authentification Adobe Pass renvoie tous les MVPD intégrés au programmeur sélectionné (ID de demandeur). Mais le programmeur peut appliquer un filtre sur le tableau de retour des MVPD et afficher uniquement ceux qui se trouvent dans les deux listes.

## Exemple {#example}

Cet exemple montre comment afficher uniquement CableCompany_1 et CableCompany_2 dans la boîte de dialogue du sélecteur MVPD et ne pas afficher CableCompany_NewIntegration.

```C
function displayProviderDialog(mvpdList) {
    var allowlisted = new Array();
    for (var i = 0; i < mvpdList.length; i = i + 1) {
        var currentMvpd = mvpdList[i];
        if ( isAllowListed(currentMvpd.ID) ) {
            allowlisted.push(currentMvpd);
        }
    }
    displayAllowlisted(allowlisted);
}

function isAllowListed(mvpdID) {
    // Implement allowlisting on MVPD IDs.
    return (mvpdID === 'CableCompany_1' || mvpdID === 'CableCompany_2');
}

function displayAllowlisted(list) {
    // TODO: Implement site-specific logic here.
}
```

<!--
**Related Information**
* [Prevent MVPDs from appearing in the Selection Dialog](/help/authentication/prevent-mvpd-selectn-dialog.md)
* **Code Samples**
* [Programmer integration guide](/help/authentication/programmer-integration-guide-overview.md)
-->
