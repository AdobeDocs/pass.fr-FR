---
title: API REST V2 - Présentation
description: API REST V2 - Présentation
source-git-commit: 94fcb4e8c94330561596cd4006738c4f4d75e371
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 0%

---

# API REST V2 - Présentation {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

Voulez-vous améliorer le rapport coût-efficacité de vos applications TVE ?

Voulez-vous réduire le temps et les ressources de développement requis pour la prise en charge des applications TVE sur plusieurs plateformes ?

Voulez-vous garantir une expérience utilisateur cohérente sur toutes les plateformes ?

Voulez-vous réduire les efforts de maintenance et simplifier la fourniture de mises à jour, correctifs de bogues ou améliorations ?

## Présentation de l’API REST V2

L’authentification Adobe Pass est ravie d’annoncer le lancement de l’API REST V2, conçue pour améliorer l’expérience utilisateur et simplifier l’intégration avec les services Pass.

Nous sommes ravis des possibilités offertes par notre nouvelle API REST, qui représente un grand pas en avant pour notre plate-forme et ouvre la porte à de nouvelles fonctionnalités et à de nouveaux flux d’applications.

## Nouveautés

Mise en oeuvre unique sur toutes les plateformes

Les applications des clients peuvent désormais utiliser la même mise en oeuvre sur toutes les plateformes, ce qui facilite le lancement de nouvelles fonctionnalités ou la maintenance d’applications actives.

### SSO multi-appareils

L’API REST V2 permet à une session d’authentification d’être transmise en toute sécurité entre différents appareils. En transmettant simplement la session entre les appareils, les utilisateurs peuvent s’authentifier sur leur appareil mobile et diffuser des vidéos sur un appareil connecté à la télévision sans nouvelle authentification.

### Plusieurs sessions d’authentification actives

Différentes sessions MVPD actives sont désormais possibles, et les clients peuvent choisir de basculer entre l’intégration TempPass et l’intégration MVPD standard si nécessaire.

### Mécanisme de sécurité amélioré

L’enregistrement dynamique de client est le mécanisme de sécurité utilisé dans tous les flux et fonctionnalités. Il permet un contrôle plus sécurisé et plus granulaire des applications des clients et peut enregistrer des applications sur toutes les plateformes.

### Amélioration des performances pour des temps de réponse plus rapides

Les mécanismes de mise en cache améliorés permettent de réduire le trafic vers les MVPD, d’améliorer les temps de réponse et de réduire la latence. Dans l’ensemble, le nombre d’appels d’API est réduit jusqu’au démarrage de la vidéo.

### Amélioration des codes d’erreur pour tous les flux

Les codes d’erreur avancés sont désormais disponibles sur tous les flux de transmission au même format. Des détails supplémentaires guident les applications afin d’améliorer l’expérience globale de l’utilisateur.

### Contrôle amélioré de toutes les sessions d’authentification

La nouvelle API REST V2 permet d’agir simultanément sur plusieurs sessions authentifiées.

### Réduction des coûts de maintenance

Toutes les réponses et les informations d’erreur sont désormais normalisées.

## Et après ?

Tous les clients qui utilisent actuellement nos API via des SDK ou des appels REST peuvent continuer à le faire, car nous prévoyons de continuer à fournir une assistance jusqu’à fin 2025.

Cependant, tous les développements futurs seront basés sur l’API REST V2. Il est vivement recommandé de lancer le processus de migration pour bénéficier des dernières fonctionnalités d’Adobe Pass.

## Vous souhaitez en savoir plus ?

Pour commencer, consultez notre documentation publique pour accéder à [description du flux](./flows/rest-api-v2-flows-overview.md) et [référence d’API](./apis/rest-api-v2-apis-overview.md).

Notre équipe d’assistance dédiée est également à votre disposition pour toute question ou assistance technique dont vous avez besoin.

## Vous souhaitez essayer l’API REST V2 ?

Vous pouvez désormais explorer l’API REST V2 via notre page dédiée aux produits sur le site web [Adobe Developer](https://developer.adobe.com/adobe-pass/).
