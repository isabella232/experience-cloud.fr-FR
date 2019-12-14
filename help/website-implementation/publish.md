---
title: Publier votre propriété Launch
description: Découvrez comment publier votre propriété Launch de l’environnement de développement vers les environnements de test et de production. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-description: null
seo-title: Publier votre propriété Launch
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: aacd691c176da6ae5b247a19051de57289c128c5

---


# Publier votre propriété Launch

Maintenant que vous avez mis en oeuvre des solutions clés d’Adobe Experience Cloud dans votre environnement de développement, il est temps de découvrir le processus de publication.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

1. Publier une bibliothèque de développement dans l’environnement d’évaluation
1. Association d’une bibliothèque d’évaluation à votre site Web de production à l’aide du débogueur
1. Publier une bibliothèque d’évaluation dans l’environnement de production

## Publication dans l’environnement d’évaluation

Maintenant que vous avez créé et validé votre bibliothèque dans l’environnement de développement, il est temps de la publier sur le site de test.

1. Go to the **[!UICONTROL Publishing]** page

1. Ouvrez la liste déroulante en regard de votre bibliothèque et sélectionnez **[!UICONTROL Envoyer pour approbation.]**

   ![Submit for Approval (Soumettre pour approbation)](images/publishing-submitForApproval.png)

1. Cliquez sur le bouton **[!UICONTROL Envoyer]** dans la boîte de dialogue :

   ![Cliquez sur Envoyer dans le modèle.](images/publishing-submit.png)

1. Votre bibliothèque apparaît désormais dans la colonne [!UICONTROL Envoi] dans un état non généré :

1. Ouvrez la liste déroulante et sélectionnez **[!UICONTROL Créer pour le test]**:

   ![Build for Staging (Créer à des fins d’évaluation)](images/publishing-buildForStaging.png)

1. Lorsque l’icône de point vert s’affiche, la bibliothèque peut être prévisualisée dans l’environnement d’évaluation.

Dans un scénario réel, l’étape suivante du processus consisterait à demander aux membres de votre équipe d’assurance qualité de valider les modifications dans la bibliothèque d’évaluation. Ils peuvent le faire à l’aide du débogueur.

**Pour valider les modifications dans la bibliothèque d’évaluation**

1. Dans la propriété Launch, ouvrez la page [!UICONTROL Environments] .

1. Sur la ligne [!UICONTROL d’évaluation] , cliquez sur l’icône ![](images/launch-installIcon.png) Installer pour ouvrir le module modal.

   ![Accédez à la page Environnements et cliquez sur pour ouvrir le module](images/publishing-getStagingCode.png)

1. Cliquez sur l’icône Copier ![Copier pour copier le code incorporé dans le Presse-papiers](images/launch-copyIcon.png) .

1. Click **[!UICONTROL Close]** to close the modal

   ![Icône Installer](images/publishing-copyStagingCode.png)

1. Open the [Luma demo site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser

1. Ouvrez l’extension [du débogueur](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) Experience Cloud en cliquant sur l’icône de l’icône ![du](images/icon-debugger.png) débogueur

   ![Cliquez sur l’icône Débogueur](images/switchEnvironments-openDebugger.png)

1. Accéder à l’onglet Outils

1. Cliquez sur **[!UICONTROL Adobe Launch &gt; Dynamic Insert Launch &gt; Embed Code]** button pour ouvrir le champ de saisie de texte (l’URL de votre code incorporé de développement est peut-être actuellement disponible) :

   ![Cliquez sur le bouton Lancer Adobe &gt; Insérer dynamiquement le lancement &gt; Code incorporé.](images/switchEnvironments-debugger-editEmbedCode.png)

1. Collez le code incorporé d’évaluation qui se trouve dans le Presse-papiers.

1. Cliquez sur l’icône de disquette pour enregistrer

   ![Environnement de lancement affiché dans le débogueur](images/switchEnvironments-debugger-save.png)

1. Rechargez et cochez l’onglet Résumé du débogueur. Dans la section Lancement, vous devriez maintenant voir votre propriété d’évaluation implémentée, avec le nom de votre propriété (c.-à-d. "Didacticiel de lancement" ou tout ce que vous avez appelé votre propriété)!

   ![Environnement de lancement affiché dans le débogueur](images/publishing-debugger-staging.png)

Dans la vie réelle, une fois que votre équipe d’assurance qualité a signé en examinant les modifications de l’environnement d’évaluation, il est temps de publier en production.

## Publication dans l’environnement de production

1. Go to the [!UICONTROL Publishing] page

1. Dans la liste déroulante, cliquez sur **[!UICONTROL Approuver pour publication]**:

   ![Approve for Publishing (Approuver pour publication)](images/publishing-approveForPublishing.png)

1. Cliquez sur le bouton **[!UICONTROL Approuver]** dans la boîte de dialogue :

   ![Cliquez sur Approuver](images/publishing-approve.png)

1. La bibliothèque apparaît désormais dans la colonne [!UICONTROL Approuvé] dans l’état non généré (point jaune) :

1. Ouvrez la liste déroulante et sélectionnez **[!UICONTROL **Créer et publier vers Production]**:

   ![Cliquez sur Créer et publier en production](images/publishing-buildAndPublishToProduction.png)

1. Cliquez sur **[!UICONTROL Publier]** dans la boîte de dialogue :

   ![Cliquez sur Publier](images/publishing-publish.png)

1. La bibliothèque s’affiche désormais dans la colonne [!UICONTROL Publié] :

   ![Published](images/publishing-published.png) (Publié)

C’est tout ! Vous avez terminé le tutoriel et publié votre première propriété dans Launch!