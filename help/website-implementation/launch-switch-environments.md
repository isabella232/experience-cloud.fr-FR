---
title: Changement d’environnements Launch avec l’Adobe Experience Cloud Debugger
description: Découvrez comment utiliser l’Experience Cloud Debugger pour charger des codes incorporés Launch. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les sites web avec Launch.
seo-description: null
seo-title: Changement d’environnements Launch avec l’Adobe Experience Cloud Debugger
solution: Experience Cloud
translation-type: ht
source-git-commit: 79d5274d09d66b80a49aba5db3e0a997d0f0ff61

---


# Changement d’environnements Launch avec l’Experience Cloud Debugger

Au cours de cette leçon, vous allez utiliser l’[extension Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) pour remplacer la propriété codée Launch en dur sur le [site de démonstration Luma](https://luma.enablementadobe.com/content/luma/us/en.html) par votre propre propriété.

Cette technique s’appelle le changement d’environnement et vous sera utile lorsque vous travaillerez avec Launch sur votre propre site web. Vous pourrez charger votre site web de production dans votre navigateur avec votre environnement de *développement* Launch. Cela vous permet d’effectuer et de valider des modifications dans Launch en toute confiance, indépendamment de vos mises à jour de code standard. Après tout, l’une des principales raisons pour lesquelles les clients utilisent Launch, c’est cette séparation entre les mises à jour des balises marketing et les mises à jour de code standard.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* utiliser le débogueur (Debugger) pour charger un autre environnement ;
* utiliser le débogueur (Debugger) pour confirmer le chargement d’un autre environnement Launch.

## Obtention de l’URL de votre environnement de développement

1. Dans les propriétés Launch, ouvrez la page `Environments`.

1. Sur la ligne **[!UICONTROL Développement]**, cliquez sur l’icône Installer ![icône Installer](images/launch-installIcon.png) pour ouvrir le modal.

1. Cliquez sur l’icône Copier ![icône Copier](images/launch-copyIcon.png) pour copier le code incorporé dans le presse-papiers.

1. Cliquez sur **[!UICONTROL Fermer]** pour fermer le modal.

   ![Icône Installer](images/launch-copyInstallCode.png)

## Remplacement de l’URL Launch sur le site de démonstration Luma

1. Ouvrez le [site de démonstration Luma](https://luma.enablementadobe.com/content/luma/us/en.html) dans votre navigateur Chrome.

1. Ouvrez l’[extension Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) en cliquant sur l’icône ![icône Debugger](images/icon-debugger.png).

   ![Clic sur l’icône Debugger](images/switchEnvironments-openDebugger.png)

1. Notez que la propriété Launch actuellement mise en œuvre s’affiche dans l’onglet Résumé.

   ![Environnement de Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail-prod.png)

1. Accédez à l’onglet Outils.

1. Accédez à la section **[!UICONTROL Remplacement du code incorporé Launch]**.

1. Assurez-vous que c’est l’onglet Chrome avec le site Luma qui est mis en évidence derrière le débogueur (et non l’onglet avec ce tutoriel ou l’onglet avec l’interface Launch). Collez le code incorporé présent dans le presse-papiers dans le champ de saisie.

1. Activez la fonction « Appliquer sur luma.enablementadobe.com », afin que toutes les pages du site Luma soient associées à votre propriété Launch.

1. Cliquez sur le bouton **[!UICONTROL Enregistrer]**.

   ![Environnement de Launch affiché dans Debugger](images/switchEnvironments-debugger-save.png)

1. Chargez à nouveau le site Luma et vérifiez l’onglet Résumé du débogueur. Sous la section Launch, vous devriez constater que votre propriété de développement est en cours d’utilisation. Vérifiez que le nom de la propriété correspond à celui de la vôtre et que l’environnement indique « development ».

   ![Environnement de Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

>[!NOTE] Le débogueur enregistrera cette configuration et remplacera les codes incorporés Launch chaque fois que vous reviendrez sur le site Luma. Cela n’a aucune incidence sur les autres sites que vous visitez dans les autres onglets ouverts. Pour empêcher le débogueur de remplacer le code incorporé, cliquez sur le bouton **[!UICONTROL Supprimer]** en regard du code incorporé dans l’onglet Outils du débogueur.

Dans la suite de ce tutoriel, vous utiliserez cette technique de mappage du site Luma sur votre propre propriété Launch pour valider votre mise en œuvre de Launch. Lorsque vous commencerez à utiliser Launch sur votre site web de production, vous pourrez utiliser la même technique pour valider les modifications.

[Suite : « Ajout d’Adobe Experience Platform Identity Service » &gt;](id-service.md)