---
title: Création d’une propriété de lancement pour les applications mobiles
description: Découvrez comment vous connecter à l’interface de lancement et créer une propriété de lancement mobile. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications mobiles Android.
seo-description: null
seo-title: Création d’une propriété de lancement pour les applications mobiles
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Création d’une propriété de lancement

Adobe Experience Platform Launch est la prochaine génération de fonctionnalités de SDK mobile et de gestion des balises de site Web. Le lancement offre aux clients un moyen simple de déployer et de gérer toutes les solutions d’analyse, de marketing et de publicité nécessaires pour générer des expériences client pertinentes. Il n'y a pas de frais supplémentaires pour le lancement. Il est disponible pour tout client Adobe Experience Cloud.

Dans cette leçon, vous allez créer une propriété Launch pour les applications mobiles.

## Conditions préalables 

Pour terminer les leçons suivantes, vous devez disposer des autorisations nécessaires pour développer, approuver, publier, gérer les extensions et gérer les environnements au lancement. Si vous ne parvenez pas à effectuer l’une de ces étapes, car les options de l’interface utilisateur ne sont pas disponibles, contactez votre administrateur Experience Cloud pour demander l’accès. For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Connexion à l’interface utilisateur de lancement
* Création d’une propriété Launch mobile
* Configuration d’une propriété Launch mobile

## Accéder à Launch

**Pour accéder au lancement**

1. Connexion à [Adobe Experience Cloud](https://experiencecloud.adobe.com)

1. Cliquez sur l'icône ![du sélecteur de](images/mobile-launch-solutionSwitcher.png) solution pour ouvrir le sélecteur de solution.

1. Select **[!UICONTROL Launch]** from the menu

   ![Ouvrez le sélecteur de solution à l’aide de l’icône et cliquez sur Activation.](images/mobile-launch-solutionSwitcherActivation.png)

1. Sous **[!UICONTROL Adobe Experience Cloud Launch]**, cliquez sur le bouton **[!UICONTROL Atteindre le lancement]** .

   ![Cliquez sur le bouton Lancer](images/mobile-launch-goToLaunch.png)

You should now see the `Properties` screen (if no properties have ever been created in the account, this screen might be empty):

![Écran Propriétés](images/mobile-launch-propertiesScreen.png)

Si vous utilisez Lancer fréquemment, vous pouvez également mettre en signet l’URL suivante et vous connecter directement [https://launch.adobe.com](https://launch.adobe.com)

## Création d’une propriété

Une propriété est essentiellement un conteneur que vous renseignez avec des extensions, des règles, des éléments de données et des bibliothèques lorsque vous déployez des balises dans votre application. Une propriété mobile unique peut être utilisée sur plusieurs plates-formes d’applications (iOS et Android, par exemple) à condition que les applications contiennent des fonctionnalités similaires et nécessitent les mêmes solutions à implémenter.  Pour plus d’informations sur la création de propriétés, voir ["Configuration d’une propriété mobile"](https://aep-sdks.gitbook.io/docs/getting-started/create-a-mobile-property) dans la documentation du produit.

**Pour créer une propriété**

1. Cliquez sur le bouton **[!UICONTROL Nouvelle propriété]** :

   ![Cliquez sur Nouvelle propriété](images/mobile-launch-addNewProperty.png)

1. Nommez votre propriété (ex. `Mobile Tutorial`)
1. En tant que plateforme, cliquez sur **[!UICONTROL Mobile.]**
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** .

   ![Créer une propriété](images/mobile-launch-newProperty.png)

Votre nouvelle propriété doit s’afficher sur la page Propriétés. Note that if you check the box next to the property name, options to **[!UICONTROL Configure]** or **[!UICONTROL Delete]** the property appear above the property list. Cliquez sur le nom de votre propriété (p. ex. `Mobile Tutorial`) pour ouvrir l’ `Overview` écran.
![Cliquez sur le nom de la propriété pour l’ouvrir.](images/mobile-launch-openProperty.png)

[Ajouter des extensions &gt;](launch-add-extensions.md)
