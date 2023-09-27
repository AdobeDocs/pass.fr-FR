---
title: Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.6.0
description: Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.6.0
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Notes de mise à jour d’Adobe Pass Concurrency Monitoring 2.6.0 {#cm-260}


Cette page décrit les nouvelles fonctionnalités, les modifications et les problèmes connus de cette version :



## Date de publication : 11/10/2016



## Nouvelles fonctionnalités

Cette version permet de mettre fin à un ou plusieurs flux existants afin de permettre au flux actuel de démarrer (c’est-à-dire de tuer le flux).



**Arrêt à distance**

* Dans une réponse de conflit 409, chaque session répertoriée dans le champ &quot;conflits&quot; du conseil porte un attribut terminationCode .
* L’utilisateur peut être invité à saisir la liste des sessions en conflit et être autorisé à choisir celle ou les sessions à supprimer.
* Les sessions distantes ne peuvent être interrompues qu’en transmettant un en-tête de requête X-Terminate (avec les codes d’arrêt sélectionnés comme valeurs) au sein d’une tentative d’initialisation de session.
* Un nouveau type de &quot;conseil&quot; a été défini pour la réponse 410 Gone pour indiquer la session qui a tué la session actuelle.


Pour plus d’informations, voir la documentation mise à jour .



>[!NOTE]
>
>La définition de session utilisée pour répertorier les sessions actives a été mise à jour afin d’inclure le nom de l’application et le nom de l’appareil (au lieu de l’ID de l’application).




## Correctifs {#bug-fixes}

Suppression des en-têtes en double dans la réponse du serveur (le correctif implique à la fois les en-têtes CORS et la date 1).




## Problèmes connus {#known-issues}

N/A
