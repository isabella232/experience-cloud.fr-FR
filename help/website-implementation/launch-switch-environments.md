---
title: Basculer les environnements de lancement avec le débogueur Adobe Experience Cloud
description: Découvrez comment utiliser le débogueur Experience Cloud pour charger différents codes d’intégration de lancements. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-description: null
seo-title: Basculer les environnements de lancement avec le débogueur Adobe Experience Cloud
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 79d5274d09d66b80a49aba5db3e0a997d0f0ff61

---


# Basculer les environnements de lancement avec le débogueur Experience Cloud

In this lesson you will use the [Adobe Experience Cloud Debugger extension](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) to replace the Launch property hardcoded on the [Luma demo site](https://luma.enablementadobe.com/content/luma/us/en.html) with your own property.

Cette technique s’appelle le changement d’environnement et sera utile ultérieurement lorsque vous travaillez avec Launch sur votre propre site Web. Vous pourrez charger votre site Web de production dans votre navigateur, mais avec votre environnement *de lancement de développement* . Cela vous permet d’effectuer et de valider en toute confiance les modifications de lancement indépendamment des versions de code régulières.  Après tout, cette séparation entre les versions de balises marketing et les versions de code régulières est l’une des principales raisons pour lesquelles les clients utilisent le lancement en premier lieu !

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Utilisation du débogueur pour charger un autre environnement de lancement
* Utilisez le débogueur pour vérifier que vous avez chargé un autre environnement de lancement.

## Obtenir l’URL de votre environnement de développement

1. In your Launch property, open the `Environments` page

1. In the **[!UICONTROL Development]** row, click the Install icon ![Install icon](images/launch-installIcon.png) to open the modal

1. Cliquez sur l’icône Copier ![Copier pour copier le code incorporé dans le Presse-papiers](images/launch-copyIcon.png) .

1. Click **[!UICONTROL Close]** to close the modal

   ![Icône Installer](images/launch-copyInstallCode.png)

## Remplacez l’URL de lancement sur le site de démonstration Luma.

1. Open the [Luma demo site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser

1. Ouvrez l’extension [du débogueur](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) Experience Cloud en cliquant sur l’icône de l’icône ![du](images/icon-debugger.png) débogueur

   ![Cliquez sur l’icône Débogueur](images/switchEnvironments-openDebugger.png)

1. Notez que la propriété Launch actuellement implémentée s’affiche dans l’onglet Summary (Résumé).

   ![Environnement de lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail-prod.png)

1. Accéder à l’onglet Outils

1. Accédez à la section **[!UICONTROL Remplacer le code incorporé du lancement]**

1. Assurez-vous que l’onglet Chrome avec le site Luma est placé derrière le débogueur (et non l’onglet avec ce didacticiel ou l’onglet avec l’interface de lancement).  Collez le code incorporé figurant dans le Presse-papiers dans le champ d’entrée.

1. Activez la fonction "Appliquer sur luma.enablement.adobe.com" afin que toutes les pages du site Luma correspondent à votre propriété Launch.

1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** .

   ![Environnement de lancement affiché dans le débogueur](images/switchEnvironments-debugger-save.png)

1. Rechargez le site Luma et vérifiez l’onglet Résumé du débogueur. Sous la section Lancement, vous devriez maintenant voir que votre propriété de développement est utilisée. Vérifiez que le Nom de la propriété correspond à la vôtre et que l’Environnement indique "développement".

   ![Environnement de lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

>[!NOTE] Le débogueur enregistrera cette configuration et remplacera les codes d’intégration de lancement chaque fois que vous reviendrez sur le site Luma. Elle n’aura aucun impact sur les autres sites que vous visitez dans d’autres onglets ouverts. To stop the Debugger from replacing the embed code, click the **[!UICONTROL Remove]** button next to the embed code in the Tools tab of the Debugger.

Au fur et à mesure que vous poursuivez le didacticiel, vous utiliserez cette technique de mappage du site Luma sur votre propre propriété Launch pour valider votre implémentation Launch. Lorsque vous commencez à utiliser Launch sur votre site Web de production, vous pouvez utiliser cette même technique pour valider les modifications.

[Suivant : "Ajouter le service d’identité Adobe Experience Platform" &gt;](id-service.md)