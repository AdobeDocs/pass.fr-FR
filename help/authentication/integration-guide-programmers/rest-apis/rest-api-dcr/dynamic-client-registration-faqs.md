---
title: FAQ sur Dynamic Client Registration (DCR)
description: FAQ sur Dynamic Client Registration (DCR)
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# FAQ sur Dynamic Client Registration (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence Adobe. Aucune utilisation non autorisée n’est autorisée.

Ce document fournit des réponses générales aux questions les plus fréquentes sur l’adoption de l’enregistrement client dynamique (DCR) d’Adobe Pass Authentication.

Pour plus d’informations sur l’enregistrement client dynamique (DCR) dans son ensemble, consultez la documentation [ Présentation de l’enregistrement client dynamique ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Questions fréquentes d’ordre général {#general-faqs}

Commencez par cette section si vous travaillez sur une application qui doit intégrer l’enregistrement client dynamique (DCR), qu’il s’agisse d’une nouvelle application ou d’une application existante qui migre depuis l’un des mécanismes précédents.

>[!MORELIKETHIS]
>
> * [FAQ sur l’API REST v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### FAQ sur l’accès à l’API REST V2 {#rest-api-v2-access-faqs}

Questions fréquentes sur l’accès à l’API +++REST V2

#### 1. Quel est l&#39;objectif de la phase d&#39;enregistrement ? {#rest-api-v2-access-faq1}

La phase d’enregistrement a pour but d’enregistrer l’application cliente par rapport à l’authentification Adobe Pass par le biais du processus [Enregistrement client dynamique (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

Le processus d’enregistrement client dynamique (DCR) nécessite que l’application cliente obtienne une paire d’informations d’identification client et récupère un jeton d’accès en tant qu’objectif final de la phase d’enregistrement.

Pour plus d’informations, consultez la documentation [ Présentation de l’enregistrement client dynamique ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. La phase d’enregistrement est-elle obligatoire ? {#rest-api-v2-access-faq2}

La phase d’enregistrement est obligatoire, mais l’application cliente peut ignorer cette phase si elle dispose d’une paire d’informations d’identification client mises en cache et d’un jeton d’accès toujours valides.

#### 3. Qu&#39;est-ce qu&#39;un énoncé de logiciel et combien de temps est-il valide ? {#rest-api-v2-access-faq3}

L&#39;instruction logicielle est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement).

L’instruction du logiciel se compose d’un jeton Web JSON (JWT) qui peut être généré et téléchargé à partir du tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre entreprise ou par un représentant Adobe Pass Authentication agissant pour votre compte.

L’instruction du logiciel est valide pour une durée illimitée, mais vous pouvez choisir de demander à un représentant Adobe Pass Authentication de la révoquer à tout moment.

L’application cliente doit stocker l’instruction du logiciel et l’utiliser lorsque vous avez besoin de récupérer les informations d’identification du client.

Pour plus d’informations, consultez la documentation [ Présentation de l’enregistrement client dynamique ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 4. Comment générer et télécharger une déclaration de logiciel ? {#rest-api-v2-access-faq4}

Cette opération peut être effectuée via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation [Guide de l’utilisateur des canaux du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou [Guide de l’utilisateur des programmeurs du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

#### 5. Que se passe-t-il si une déclaration de logiciel est révoquée ? {#rest-api-v2-access-faq5}

Lorsque l’instruction du logiciel est révoquée, il existe une conséquence importante à prendre en compte :

* Les applications clientes qui utilisent l’instruction logicielle révoquée ne pourront plus passer par les flux [droits](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement), ce qui signifie que les utilisateurs seront bloqués pour lire le contenu.

#### 6. Que sont les informations d’identification du client et pendant combien de temps sont-elles valides ? {#rest-api-v2-access-faq6}

Les informations d’identification du client sont un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials).

Les informations d’identification client se composent d’un identifiant client et d’une paire secret client qui peuvent être récupérées à partir du point d’entrée du registre client.

Les informations d’identification du client sont valides pour une durée illimitée.

L’application cliente doit stocker les informations d’identification du client et les utiliser indéfiniment lorsqu’elle a besoin de récupérer un jeton d’accès.

Pour plus d’informations, consultez la documentation [Récupération des informations d’identification client](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

#### 7. Comment gérer les informations d’identification du client ? {#rest-api-v2-access-faq7}

Nous recommandons à l’application cliente de gérer une paire unique d’informations d’identification client pour chaque instance d’application utilisateur dans le cas des intégrations client à serveur et serveur à serveur avec l’authentification Adobe Pass.

#### 8. L’application cliente doit-elle mettre en cache les informations d’identification du client dans un stockage persistant ? {#rest-api-v2-access-faq8}

L’application cliente doit stocker les informations d’identification du client et les utiliser indéfiniment lorsqu’elle a besoin de récupérer un jeton d’accès.

#### 9. Que se passe-t-il si les informations d’identification du client mises en cache sont perdues ? {#rest-api-v2-access-faq9}

Lorsque les informations d’identification du client mises en cache sont perdues, trois conséquences importantes doivent être prises en compte :

* L’application cliente doit obtenir une nouvelle paire d’informations d’identification client.
* L’application cliente doit obtenir un nouveau jeton d’accès à l’aide de la nouvelle paire d’informations d’identification client.
* L’application cliente devra demander à l’utilisateur de s’authentifier à nouveau, car elle perdra l’accès aux profils authentifiés obtenus précédemment.

#### 10. Qu’est-ce qu’un jeton d’accès et combien de temps est-il valide ? {#rest-api-v2-access-faq10}

Le jeton d’accès est un terme défini dans la documentation [Glossaire](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token).

Le jeton d’accès se compose d’un [jeton porteur](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) qui peut être récupéré à partir du point d’entrée du jeton client.

Le jeton d’accès est valide pendant une période limitée et courte spécifiée au moment de l’émission.

L’application cliente doit stocker le jeton d’accès et l’utiliser jusqu’à son expiration lors du ciblage de l’API REST V2.

L’application cliente doit obtenir un nouveau jeton d’accès avant l’expiration du jeton actuel pour éviter les requêtes non autorisées.

Pour plus d’informations, consultez la documentation [Récupérer le jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

#### 11. L’application cliente doit-elle mettre en cache le jeton d’accès dans un stockage persistant ? {#rest-api-v2-access-faq11}

L’application cliente doit stocker et utiliser le jeton d’accès jusqu’à son expiration, puis le supprimer et en obtenir un nouveau.

#### 12. Comment l’application cliente peut-elle actualiser un jeton d’accès ? {#rest-api-v2-access-faq12}

L’application cliente doit actualiser un jeton d’accès de la même manière que pour récupérer un nouveau jeton d’accès, mais en utilisant les informations d’identification du client mises en cache.

L’application cliente ne doit pas se réenregistrer pour actualiser un jeton d’accès, mais doit utiliser les informations d’identification client stockées, sinon les utilisateurs et utilisatrices devraient s’authentifier à nouveau.

Pour plus d’informations, consultez la documentation [Récupérer le jeton d’accès](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

+++

## FAQ sur la migration {#migration-faqs}

Passez à cette section si vous travaillez sur une application qui doit migrer une application existante pour utiliser l’enregistrement client dynamique (DCR).

>[!MORELIKETHIS]
>
> * [FAQ sur l’API REST v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### FAQ sur la migration vers la version 2 de l’API REST {#rest-api-v2-migration-faqs}

FAQ sur la migration vers +++REST API V2

#### 1. L’application cliente peut-elle réutiliser les applications enregistrées existantes (déclarations de logiciel) ? {#rest-api-v2-migration-faq1}

L’application cliente ne peut pas réutiliser les applications enregistrées existantes (instructions logicielles). Elle doit donc générer et télécharger une nouvelle application enregistrée (instructions logicielles) dédiée à l’utilisation de l’API REST V2.

Cette opération peut être effectuée via le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) par l’un des administrateurs de votre organisation ou par un représentant Adobe Pass Authentication agissant en votre nom.

Pour plus d’informations, reportez-vous à la documentation [Guide de l’utilisateur des canaux du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou [Guide de l’utilisateur des programmeurs du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

Pour le moment, vous devrez demander à un représentant de l’authentification Adobe Pass d’activer l’utilisation de l’API REST V2 pour vos nouvelles applications enregistrées (instructions logicielles), jusqu’à ce que le tableau de bord Adobe Pass [TVE Dashboard](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) soit mis à jour pour permettre l’auto-gestion de cette opération.

Afin de distinguer les applications enregistrées (instructions logicielles) utilisées dans les applications clientes utilisant l’API REST V2, vous devez ajouter un suffixe spécifique au nom de l’application enregistrée, tel que « RESTV2 ».

#### 2. L’application cliente peut-elle réutiliser les schémas personnalisés existants ? {#rest-api-v2-migration-faq2}

L’application cliente peut réutiliser les schémas personnalisés existants générés via le tableau de bord Adobe Pass [TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard).

Pour plus d’informations, reportez-vous à la documentation [Guide de l’utilisateur des canaux du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) ou [Guide de l’utilisateur des programmeurs du tableau de bord TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes).

+++
