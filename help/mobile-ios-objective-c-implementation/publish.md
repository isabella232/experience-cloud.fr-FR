---
title: Publier votre propriété Launch
description: Découvrez comment publier votre propriété Launch de l’environnement de développement vers les environnements de test et de production. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications iOS Objective-C avec lancement.
seo-description: null
seo-title: Publier votre propriété Launch
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Publier votre propriété Launch

Maintenant que vous avez mis en oeuvre des solutions clés d’Adobe Experience Cloud dans votre environnement de développement, il est temps de découvrir le processus de publication.

## Conditions préalables 

Votre compte utilisateur Lancer doit être autorisé à "Approuver" et "Publier" pour terminer cette leçon. Si vous ne parvenez pas à effectuer l’une de ces étapes, car les options de l’interface utilisateur ne sont pas disponibles, contactez votre administrateur Experience Cloud pour y accéder. For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

1. Publier une bibliothèque de développement dans l’environnement d’évaluation
1. Mettez à jour votre application pour charger différents environnements de lancement
1. Publier une bibliothèque d’évaluation dans l’environnement de production

## Publication dans l’environnement d’évaluation

Maintenant que vous avez créé et validé votre bibliothèque dans l’environnement de développement, il est temps de la publier sur le site de test.

1. Go to the **[!UICONTROL Publishing]** page

1. Ouvrez la liste déroulante en regard de votre bibliothèque et sélectionnez **[!UICONTROL Envoyer pour approbation.]**

   ![Submit for Approval (Soumettre pour approbation)](images/mobile-publishing-submitForApproval.png)

1. Cliquez sur le bouton **[!UICONTROL Envoyer]** dans la boîte de dialogue :

   ![Cliquez sur Envoyer dans le modèle.](images/mobile-publishing-submit.png)

1. Votre bibliothèque apparaît désormais dans la colonne [!UICONTROL Envoi] dans un état non généré :

1. Ouvrez la liste déroulante et sélectionnez **[!UICONTROL Créer pour le test]**:

   ![Build for Staging (Créer à des fins d’évaluation)](images/mobile-publishing-buildForStaging.png)
1. Lorsque l’icône de point vert s’affiche, la bibliothèque peut être prévisualisée dans l’environnement d’évaluation.

Dans un scénario réel, l’étape suivante du processus consisterait à demander aux membres de votre équipe d’assurance qualité de valider les modifications dans la bibliothèque d’évaluation.

**Pour valider les modifications dans la bibliothèque d’évaluation**

1. Dans la propriété Launch, ouvrez la page [!UICONTROL Environments] .

1. Dans la ligne [!UICONTROL d’évaluation] , cliquez sur l’icône Installer

   ![Icône](images/mobile-launch-installIcon.png) d’installation pour ouvrir le module
   ![Accédez à la page Environnements et cliquez sur pour ouvrir le module](images/ios/objective-c/mobile-publishing-getStagingCode.png)

Si vous utilisez un autre espace de travail pour votre application d’évaluation, vous devez vous assurer que cet espace de travail comporte tous les modules et mises à jour d’application que vous avez effectués tout au long de ce didacticiel. A ce stade, la seule différence dans les instructions d’installation par rapport à votre environnement de développement est la référence de lancement dans la configuration de base, comme indiqué dans la capture d’écran ci-dessus. Vous devez mettre à jour la ligne correspondante dans votre fichier AppDelegate.h et recréer votre application.

Dans la vie réelle, une fois que votre équipe d’assurance qualité a signé en examinant les modifications de l’environnement d’évaluation, il est temps de publier en production.

## Publication dans l’environnement de production

1. Go to the [!UICONTROL Publishing] page

1. Dans la liste déroulante, cliquez sur **[!UICONTROL Approuver pour publication]**:

   ![Approve for Publishing (Approuver pour publication)](images/mobile-publishing-approveForPublishing.png)

1. Cliquez sur le bouton **[!UICONTROL Approuver]** dans la boîte de dialogue :

   ![Cliquez sur Approuver](images/mobile-publishing-approve.png)

1. La bibliothèque apparaît désormais dans la colonne [!UICONTROL Approuvé] dans l’état non généré (point jaune) :

1. Ouvrez la liste déroulante et sélectionnez **[!UICONTROL Créer et Publier vers Production]**:

   ![Cliquez sur Créer et publier en production](images/mobile-publishing-buildAndPublishToProduction.png)

1. Cliquez sur **[!UICONTROL Publier]** dans la boîte de dialogue :

   ![Cliquez sur Publier](images/mobile-publishing-publish.png)

1. La bibliothèque s’affiche désormais dans la colonne [!UICONTROL Publié] :

   ![Published](images/mobile-publishing-published.png) (Publié)

Notez à nouveau que l’environnement Production utilise une référence de lancement dans la configuration de base, comme indiqué dans la capture d’écran ci-dessous.  Si vous utilisez un autre espace de travail pour votre application d’évaluation, vous devez vous assurer que cet espace de travail comporte tous les modules et mises à jour d’application que vous avez effectués tout au long de ce didacticiel.
![Accédez à la page Environnements et cliquez sur pour ouvrir le module](images/ios/objective-c/mobile-publishing-getProductionCode.png)

>[!WARNING] La prochaine fois que vous apporterez des modifications à votre configuration de lancement, vous devrez créer une nouvelle bibliothèque dans l’environnement de développement. N’oubliez pas que l’ajout et la suppression d’extensions nécessiteront des mises à jour de l’application elle-même. Veillez à synchroniser les environnements de lancement et le code d’application les uns avec les autres afin d’éviter les problèmes.

C’est tout ! Vous avez terminé le didacticiel et publié votre première propriété mobile dans Launch!