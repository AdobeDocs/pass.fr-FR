---
title: Programmeurs
description: Découvrez les programmeurs et leurs configurations dans le tableau de bord TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '717'
ht-degree: 0%

---

# Programmeurs {#programmers}

>[!NOTE]
>
>Le contenu de cette page est fourni à titre d’information uniquement. L’utilisation de cette API nécessite une licence actuelle de Adobe. Aucune utilisation non autorisée n’est autorisée.

La variable **Programmeurs** La section du tableau de bord TVE vous permet d’afficher et de gérer les paramètres du [programmeurs](/help/authentication/glossary.md#programmer) liés à vos droits de compte. Vous pouvez également [ajouter un nouveau programmeur](#add-new-programmer) selon vos besoins.

La variable **Programmeurs** dans le panneau de gauche affiche une liste des programmeurs existants avec les détails suivants :

* **Identifiant du programmeur**: identifiant de la société de médias dans le système.
* **Canaux**: nombre de canaux associés liés à un programmeur.

![Liste des programmeurs existants](assets/programmers-list.png)

*Liste des programmeurs existants*

Saisissez le nom du programmeur dans le champ **Rechercher** au-dessus de la liste pour en savoir plus sur un programmeur.

## Gestion des configurations de programmeur {#manage-programmer-conf}

Suivez ces étapes pour gérer différents paramètres d’un programmeur spécifique.

1. Sélectionnez la variable **Programmeurs** dans le panneau de gauche.
1. Sélectionnez un programmeur dans la liste.
1. Sélectionnez l’un des onglets suivants pour afficher et modifier les paramètres correspondants du programmeur sélectionné :

   * [Canaux](#channels)
   * [Certificats](#certificates)
   * [Applications enregistrées](#registered-applications)
   * [Schémas personnalisés](#custom-schemes)

   ![Paramètres du programmeur](assets/programmer-settings.png)

   *Paramètres du programmeur*

>[!IMPORTANT]
>
> Affichage [Révision et notification push des modifications](/help/authentication/tve-dashboard-review-push-changes.md) pour plus d’informations sur l’activation des modifications de configuration.

### Canaux {#channels}

Cet onglet affiche la liste des canaux associés à un programmeur actuel. Sélectionnez un canal spécifique de cette liste pour accéder aux informations détaillées de la section [Canaux](/help/authentication/tve-dashboard-channels.md) .

Pour ajouter un nouveau canal pour le programmeur sélectionné, sélectionnez **Ajouter un nouveau canal** dans le coin supérieur droit de **Canaux disponibles** . Formation [comment ajouter un nouveau canal](/help/authentication/tve-dashboard-channels.md#add-new-channel).

![Ajouter un nouveau canal](assets/programmers-channels.png)

*Ajouter un nouveau canal*

### Certificats {#certificates}

Cet onglet affiche une liste de [certificats disponibles](#available-certificates) utilisé dans les flux de chiffrement des métadonnées utilisateur. Il affiche des détails sur chaque certificat qui comprend :

* État (activé ou non pour **cryptage des métadonnées utilisateur** usage ou non)
* Numéro de série
* Nom de l’organisation émettrice
* Nom de l’organisation concernée
* Date d’émission
* Date d’expiration
* Un menu déroulant pour chiffrer les métadonnées utilisateur (si vous sélectionnez **Oui**, le certificat chiffrera les informations utilisateur sensibles, telles que les valeurs de code postal).

#### Certificats disponibles {#available-certificates}

Ces certificats servent de clés privées ou publiques et sont utilisés pour le chiffrement des métadonnées utilisateur. Tous les canaux associés à la même société de médias peuvent utiliser ces certificats.

Vous pouvez apporter les modifications suivantes aux certificats disponibles :

* [Ajouter un nouveau certificat](#add-new-certificate)
* [Suppression de certificat](#delete-certificate)

##### Ajouter un nouveau certificat {#add-new-certificate}

Pour ajouter un nouveau certificat, procédez comme suit.

1. Sélectionner **Ajouter un nouveau certificat** dans le coin supérieur droit du **Certificats disponibles** .

   ![Ajouter un nouveau certificat](assets/programmer-add-new-certificate.png)

   *Ajouter un nouveau certificat*

1. Collez la clé publique de votre certificat dans le **Nouveau certificat** de la boîte de dialogue
1. Sélectionner **Ajout d’un certificat**.

   Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser le nouveau certificat répertorié dans la variable **Certificats disponibles** , passez à la section [révision et notification push des modifications](/help/authentication/tve-dashboard-review-push-changes.md) flux.

1. Localisez le nouveau certificat dans la liste de **Certificats disponibles**.

   >[!IMPORTANT]
   >
   > Assurez-vous que vos systèmes sont à jour et prêts à utiliser le nouveau certificat.

1. Sélectionner **Oui** de **Utilisé pour les métadonnées utilisateur chiffrées** pour activer un nouveau certificat.

##### Suppression de certificat {#delete-certificate}

Pour supprimer un certificat, procédez comme suit.

1. Pointez sur le certificat que vous souhaitez supprimer de la liste de **Certificats disponibles**.
1. Sélectionner **Supprimer**.

   ![Supprimer le certificat sélectionné](assets/programmer-remove-certificate.png)

   *Supprimer le certificat sélectionné*

1. Sélectionner **Supprimer** sur le **Suppression de certificat** de la boîte de dialogue

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Le certificat sera supprimé de la **Certificats disponibles** section uniquement après [révision et notification push des modifications](/help/authentication/tve-dashboard-review-push-changes.md).

### Applications enregistrées {#registered-applications}

Cet onglet fournit une liste d’inscriptions aux applications. Affichage [Gestion dynamique de l&#39;enregistrement des clients](/help/authentication/dynamic-client-registration-management.md), pour plus d’informations.

### Schémas personnalisés {#custom-schemes}

Cet onglet affiche une liste des schémas personnalisés. Affichage [Enregistrement de l’application iOS/tvOS](/help/authentication/iostvos-application-registration.md) et [Gestion dynamique de l&#39;enregistrement des clients](/help/authentication/dynamic-client-registration-management.md), pour plus d’informations.

## Ajouter un nouveau programmeur {#add-new-programmer}

Pour ajouter une nouvelle entité de programmeur, procédez comme suit.

1. Sélectionnez la variable **Programmeurs** dans le panneau de gauche.
1. Sélectionner **Ajouter un nouveau programmeur** dans le coin supérieur droit du **Programmeurs** .

   ![Ajout d’un nouveau programmeur](assets/add-new-programmer.png)

   *Ajout d’un nouveau programmeur*

1. Saisissez l’identifiant de la société multimédia dans **Identifiant du programmeur** dans le **Nouveau programmeur** de la boîte de dialogue
1. Saisissez un nom de marque commerciale à afficher dans la console sous **Nom d’affichage**.
1. Sélectionner **Ajout d’un programmeur**.

Un nouveau changement de configuration a été créé et est prêt pour la mise à jour du serveur. Pour utiliser le nouveau programmeur répertorié dans le **Programmeurs** , passez à la section [révision et notification push des modifications](/help/authentication/tve-dashboard-review-push-changes.md) flux.

