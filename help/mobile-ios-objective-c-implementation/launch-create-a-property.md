---
title: Création d’une propriété Launch pour les applications mobiles
description: Découvrez comment vous connecter à l’interface de Launch et créer une propriété Launch mobile. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS.
seo-description: null
seo-title: Création d’une propriété Launch pour les applications mobiles
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Création d’une propriété Launch

Adobe Experience Platform Launch est la nouvelle génération de fonctionnalités de gestion de SDK mobile et de balises de sites web. Launch offre aux clients un moyen simple de déployer et de gérer toutes les solutions d’analyse, de marketing et de publicité nécessaires pour proposer des expériences client pertinentes. Launch n’entraîne aucuns frais supplémentaires. Il est disponible pour chaque client d’Adobe Experience Cloud.

Dans cette leçon, vous allez créer une propriété Launch pour les applications mobiles.

## Conditions préalables

Vous devez disposer des autorisations nécessaires pour développer, approuver, publier, gérer les extensions et gérer les environnements au sein de Launch pour pouvoir suivre les leçons suivantes. Si vous ne parvenez pas à effectuer l’une de ces étapes parce que vous n’avez pas accès aux options de l’interface utilisateur, contactez votre administrateur Experience Cloud pour demander l’accès à ces options. Pour plus d’informations sur les autorisations dans Launch, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/user-permissions.html).

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* vous connecter à l’interface utilisateur de Launch ;
* créer une propriété Launch mobile ;
* configurer une propriété Launch mobile.

## Accès à Launch

**Accès à Launch**

1. Connectez-vous à [Adobe Experience Cloud](https://experiencecloud.adobe.com)

1. Cliquez sur l’icône ![icône du sélecteur de solution](images/mobile-launch-solutionSwitcher.png) pour ouvrir le sélecteur de solution.

1. Sélectionnez **[!UICONTROL Launch]** dans le menu.

   ![Ouverture du sélecteur de solution à l’aide de l’icône et clic sur Activation](images/mobile-launch-solutionSwitcherActivation.png)

1. Sous **[!UICONTROL Adobe Experience Cloud Launch]**, cliquez sur le bouton **[!UICONTROL Accéder à Launch]**.

   ![Clic sur le bouton Launch.](images/mobile-launch-goToLaunch.png)

L’écran `Properties` devrait s’afficher (si aucune propriété n’a été créée pour ce compte, cet écran peut être vide) :

![Écran Propriétés](images/mobile-launch-propertiesScreen.png)

Si vous utilisez Launch fréquemment, vous pouvez également mettre l’URL suivante en signet et vous connecter directement [https://launch.adobe.com](https://launch.adobe.com).

## Création d’une propriété

Une propriété est un conteneur qui se remplit d’extensions, de règles, d’éléments de données et de bibliothèques lors du déploiement des balises sur l’application. Une propriété mobile unique peut être utilisée sur plusieurs plateformes d’applications (par exemple iOS et Android), à condition que ces applications contiennent des fonctionnalités similaires et nécessitent la mise en œuvre des mêmes solutions.  Pour plus d’informations sur la création de propriétés, voir [« Configuration d’une propriété mobile »](https://aep-sdks.gitbook.io/docs/getting-started/create-a-mobile-property) dans la documentation du produit.

**Création d’une propriété**

1. Cliquez sur le bouton **[!UICONTROL Nouvelle propriété]** :

   ![Clic sur Nouvelle propriété](images/mobile-launch-addNewProperty.png)

1. Nommez votre propriété (par exemple `Mobile Tutorial`). 
1. Pour la plateforme, cliquez sur **[!UICONTROL Mobile]**.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]**.

   ![Création d’une propriété](images/mobile-launch-newProperty.png)

Votre nouvelle propriété devrait s’afficher sur la page Propriétés. Notez que si vous cochez la case en regard du nom de la propriété, les options **[!UICONTROL Configurer]** ou **[!UICONTROL Supprimer]** relatives à la propriété apparaissent au-dessus de la liste des propriétés. Cliquez sur le nom de votre propriété (p. ex. `Mobile Tutorial`) pour ouvrir l’écran `Overview`.
![Clic sur le nom de la propriété pour l’ouvrir](images/mobile-launch-openProperty.png)

[Suite : « Ajout d’extensions » &gt;](launch-add-extensions.md)
