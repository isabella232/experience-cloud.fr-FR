---
title: Création d’une propriété de lancement
description: Découvrez comment vous connecter à l’interface de lancement et créer une propriété de lancement. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-description: null
seo-title: Création d’une propriété de lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Création d’une propriété de lancement

Dans cette leçon, vous allez créer votre première propriété de lancement.

Une propriété est essentiellement un conteneur que vous remplissez avec des extensions, des règles, des éléments de données et des bibliothèques lorsque vous déployez des balises sur votre site.

## Conditions préalables 

Pour terminer les leçons suivantes, vous devez disposer des autorisations nécessaires pour développer, approuver, publier, gérer les extensions et gérer les environnements au lancement. Si vous ne parvenez pas à effectuer l’une de ces étapes, car les options de l’interface utilisateur ne sont pas disponibles, contactez votre administrateur Experience Cloud pour demander l’accès. For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Connexion à l’interface utilisateur de lancement
* Création d’une propriété de lancement
* Configuration d’une propriété Launch

## Accéder à Launch

**Pour accéder au lancement**

1. Connexion à [Adobe Experience Cloud](https://experiencecloud.adobe.com)

1. Cliquez sur l'icône ![du sélecteur de](images/launch-solutionSwitcher.png) solution pour ouvrir le sélecteur de solution.

1. Sélectionnez **[!UICONTROL Lancer]** dans le menu ![Ouvrez le sélecteur de solution à l’aide de l’icône et cliquez sur Activation.](images/launch-solutionSwitcherActivation.png)

1. Sous **[!UICONTROL Adobe Experience Cloud Launch]**, cliquez sur le bouton **[!UICONTROL Atteindre le lancement]** .

   ![Cliquez sur le bouton Lancer](images/launch-goToLaunch.png)

You should now see the `Properties` screen (if no properties have ever been created in the account, this screen might be empty):

![Écran Propriétés](images/launch-propertiesScreen.png)

Si vous utilisez Lancer fréquemment, vous pouvez également mettre en signet l’URL suivante et vous connecter directement [https://launch.adobe.com](https://launch.adobe.com)

## Création d’une propriété

Une propriété est essentiellement un conteneur que vous remplissez avec des extensions, des règles, des éléments de données et des bibliothèques lorsque vous déployez des balises sur votre site. Une propriété peut être n’importe quel regroupement d’un ou de plusieurs domaines et sous-domaines. Vous pouvez gérer ces ressources et en effectuer le suivi de manière similaire. Par exemple, supposons que vous disposez de plusieurs sites web reposant sur un modèle et que vous souhaitez effectuer le suivi des mêmes ressources sur tous les sites. Vous pouvez appliquer une propriété à plusieurs domaines. Pour plus d’informations sur la création de propriétés, voir ["Entreprises et propriétés"](https://docs.adobe.com/content/help/en/launch/using/reference/admin/companies-and-properties.html) dans la documentation du produit.

**Pour créer une propriété**

1. Cliquez sur le bouton **[!UICONTROL Nouvelle propriété]** :

   ![Cliquez sur Nouvelle propriété](images/launch-addNewProperty.png)

1. Nommez votre propriété (par ex. `Launch Tutorial` ou `Daniel's Launch Tutorial`)
1. En tant que domaine, entrez `enablementadobe.com` puisqu’il s’agit du domaine où le site de démonstration Luma est hébergé. Bien que le champ "Domaine" soit obligatoire, la propriété Launch fonctionne sur n’importe quel domaine où elle est implémentée. Ce champ a pour principal objectif de préremplir les options de menu du créateur de règles.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** .

   ![Créer une propriété](images/launch-newProperty.png)

Votre nouvelle propriété doit s’afficher dans la page Propriétés. Note that if you check the box next to the property name, options to **[!UICONTROL Configure]** or **[!UICONTROL Delete]** the property appear above the property list. Cliquez sur le nom de votre propriété (p. ex. `Launch Tutorial`) pour ouvrir l’ `Overview` écran.
![Cliquez sur le nom de la propriété pour l’ouvrir.](images/launch-openProperty.png)

[Suivant : "Ajouter le code incorporé de lancement" &gt;](launch-add-embed.md)
