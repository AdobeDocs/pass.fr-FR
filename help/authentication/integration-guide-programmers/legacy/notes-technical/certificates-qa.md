---
title: Questions/Réponses Sur Les Certificats
description: Questions/Réponses Sur Les Certificats
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Questions/réponses sur les certificats (hérités) {#certificates-q}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe actuelle. Aucune utilisation non autorisée n’est autorisée.

>[!IMPORTANT]
>
> Veillez à rester informé des dernières annonces de produits Authentification Adobe Pass et des délais de désactivation agrégés dans la page [Annonces de produits](/help/authentication/product-announcements.md).

</br>

**Q1:** Est-il possible d’enregistrer des certificats dans iOS et Android ?

**A :** le certificat pour iOS et Android est le même dans la configuration actuelle. Le certificat natif est utilisé pour les deux plateformes.

</br>

**Q2:** Les mêmes certificats iOS peuvent-ils être utilisés comme certificats de Principal et de sauvegarde dans les environnements de production et d’évaluation ? Si ce n&#39;est pas recommandé, pouvez-vous nous donner une explication?

**A :** il n’est pas utile de configurer le même certificat que le certificat de Principal et le certificat de sauvegarde. Nous avons le concept de certificats de Principal et de sauvegarde afin que nous puissions configurer plusieurs certificats pour un programmeur au cas où le certificat de Principal expire ou est révoqué. Disposer d’un certificat de sauvegarde permet aux programmeurs de modifier le certificat de Principal sans affecter l’environnement de la version. Cependant, vous pouvez utiliser le même ensemble de certificats de Principal et de sauvegarde pour les profils de production et d’évaluation.

</br>

**Q3:** Un nouveau certificat est-il nécessaire pour les pages web qui utiliseront le nouveau TempPass flexible ?

**A :** le certificat (et tout certificat en effet) est configuré au niveau de l’entreprise de médias et du programmeur. FlexibleTempPass est un MVPD, vous n&#39;avez pas besoin de configurer de certificat pour lui, donc si vous intégrez un Programmeur existant avec le TempPass flexible, le certificat qui est déjà configuré au niveau du Programmeur / Media Company sera utilisé.
