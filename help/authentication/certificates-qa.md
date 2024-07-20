---
title: Questions et réponses sur les certificats
description: Questions et réponses sur les certificats
exl-id: d4e493b0-4467-42b1-9758-16c5941d8051
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# Questions et réponses sur les certificats {#certificates-q}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

</br>

**Q1 :** Est-il possible d’enregistrer des certificats dans iOS et Android ?

**A :** Le certificat pour iOS et Android est le même dans la configuration actuelle. Le certificat natif est utilisé pour les deux plateformes.

</br>

**Q2 :** Les mêmes certificats iOS peuvent-ils être utilisés comme certificats de Principal et de sauvegarde dans les environnements de production et d’évaluation ? Si ce n’est pas recommandé, pouvez-vous fournir une explication ?

**A :** La configuration du même certificat ne sert à rien que le certificat de Principal et le certificat de sauvegarde. Nous avons le concept de certificats de Principal et de sauvegarde afin de pouvoir configurer plusieurs certificats pour un programmeur en cas d’expiration ou de révocation du certificat du Principal. Disposer d’un certificat de sauvegarde donnera aux programmeurs le temps de modifier le Principal sans affecter l’environnement de publication. Vous pouvez toutefois utiliser le même ensemble de certificats de Principal et de sauvegarde pour les profils de production et d’évaluation.

</br>

**Q3 :** Un nouveau certificat est-il nécessaire pour les pages web qui utiliseront le nouveau TempPass flexible ?

**A :** Le certificat (et tout certificat en fait) est configuré au niveau de Media Company &amp; Programmer. FlexibleTempPass est un MVPD. Vous n’avez pas besoin de configurer de certificat pour celui-ci. Par conséquent, si vous intégrez un programmeur existant avec un TempPass flexible, le certificat qui est déjà configuré au niveau de Programmer/Media Company sera utilisé.
