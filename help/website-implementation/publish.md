---
title: Publication de la propriété Launch
description: Découvrez comment publier la propriété Launch de l’environnement de développement vers les environnements d’évaluation et de production. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les sites web avec Launch.
seo-description: null
seo-title: Publication de la propriété Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: aacd691c176da6ae5b247a19051de57289c128c5

---


# Publication de la propriété Launch

Maintenant que vous avez mis en œuvre certaines solutions clés d’Adobe Experience Cloud dans votre environnement de développement, il est temps d’apprendre le processus de publication.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

1. publier une bibliothèque de développement dans l’environnement d’évaluation ;
1. faire correspondre une bibliothèque d’évaluation à votre site web de production à l’aide du débogueur ;
1. publier une bibliothèque d’évaluation dans l’environnement de production.

## Publication dans l’environnement d’évaluation

Maintenant que vous avez créé et validé votre bibliothèque dans l’environnement de développement, il est temps de la publier dans l’environnement d’évaluation.

1. Accédez à la page **[!UICONTROL Publication]**.

1. Dans la liste déroulante en regard de votre bibliothèque, sélectionnez **[!UICONTROL Envoyer pour approbation]**.

   ![Envoyer pour approbation](images/publishing-submitForApproval.png)

1. Cliquez sur le bouton **[!UICONTROL Envoyer]** dans la boîte de dialogue :

   ![Clic sur Envoyer dans le modal](images/publishing-submit.png)

1. Votre bibliothèque apparaît désormais dans la colonne [!UICONTROL Envoyé] à l’état non créée :

1. Ouvrez la liste déroulante et sélectionnez **[!UICONTROL Créer à des fins d’évaluation]** :

   ![Créer à des fins d’évaluation](images/publishing-buildForStaging.png)

1. Lorsque l’icône en forme de point vert s’affiche, la bibliothèque peut être prévisualisée dans l’environnement d’évaluation.

Dans un scénario réel, l’étape suivante du processus consisterait à demander aux membres de votre équipe d’assurance qualité de valider les modifications dans la bibliothèque d’évaluation. Cela est possible à l’aide du débogueur.

**Validation des modifications dans la bibliothèque d’évaluation**

1. Dans la propriété Launch, ouvrez la page [!UICONTROL Environnements].

1. Sur la ligne [!UICONTROL Évaluation], cliquez sur l’icône Installer ![icône Installer](images/launch-installIcon.png) pour ouvrir le modal.

   ![Accès à la page Environnements et clic pour ouvrir le modal](images/publishing-getStagingCode.png)

1. Cliquez sur l’icône Copier ![icône Copier](images/launch-copyIcon.png) pour copier le code incorporé dans le presse-papiers.

1. Cliquez sur **[!UICONTROL Fermer]** pour fermer le modal.

   ![Icône Installer](images/publishing-copyStagingCode.png)

1. Ouvrez le [site de démonstration Luma](https://luma.enablementadobe.com/content/luma/us/en.html) dans votre navigateur Chrome.

1. Ouvrez l’[extension Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) en cliquant sur l’icône ![icône Debugger](images/icon-debugger.png).

   ![Clic sur l’icône Debugger](images/switchEnvironments-openDebugger.png)

1. Accédez à l’onglet Outils.

1. Cliquez sur le bouton **[!UICONTROL Adobe Launch &gt; Insérer Launch de façon dynamique &gt; Code incorporé]** pour ouvrir le champ de saisie de texte (l’URL de votre code incorporé de développement est peut-être renseignée dans le champ) :

   ![Adobe Launch &gt; Insérer Launch de façon dynamique &gt; Code incorporé](images/switchEnvironments-debugger-editEmbedCode.png)

1. Collez le code incorporé d’évaluation qui se trouve dans le presse-papiers.

1. Cliquez sur l’icône de disquette pour enregistrer.

   ![Environnement de Launch affiché dans Debugger](images/switchEnvironments-debugger-save.png)

1. Actualisez et consultez l’onglet Résumé du débogueur. Sous la section Launch, vous devriez constater que votre propriété d’évaluation est mise en œuvre et voir le nom de votre propriété (« Lancer un tutoriel » ou autre selon le nom donné) !

   ![Environnement de Launch affiché dans Debugger](images/publishing-debugger-staging.png)

En situation réelle, une fois que l’équipe d’assurance qualité a examiné et validé les modifications dans l’environnement d’évaluation, il est temps de publier la propriété dans l’environnement de production.

## Publication dans l’environnement de production

1. Accédez à la page [!UICONTROL Publication].

1. Dans la liste déroulante, cliquez sur **[!UICONTROL Approuver pour publication]** :

   ![Approuver pour publication](images/publishing-approveForPublishing.png)

1. Cliquez sur le bouton **[!UICONTROL Approuver]** dans la boîte de dialogue :

   ![Clic sur Approuver](images/publishing-approve.png)

1. La bibliothèque apparaît désormais dans la colonne [!UICONTROL Approuvé] à l’état non créée (point jaune) :

1. Dans la liste déroulante, sélectionnez **[!UICONTROL **Créer et publier dans l’environnement de production]** :

   ![Créer et publier dans l’environnement de production](images/publishing-buildAndPublishToProduction.png)

1. Cliquez sur **[!UICONTROL Publier]** dans la boîte de dialogue :

   ![Clic sur Publier.](images/publishing-publish.png)

1. La bibliothèque s’affiche désormais dans la colonne [!UICONTROL Publié] :

   ![Publié](images/publishing-published.png)

Vous avez terminé. Vous avez terminé ce tutoriel et publié votre première propriété dans Launch !