---
title: Création d’une propriété Launch
description: Découvrez comment vous connecter à l’interface Launch et créer une propriété Launch. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les sites web avec Launch.
seo-description: null
seo-title: Création d’une propriété Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Création d’une propriété Launch

Au cours de cette leçon, vous allez créer votre première propriété Launch.

Une propriété est essentiellement un conteneur que vous remplissez avec des extensions, des règles, des éléments de données et des bibliothèques lorsque vous déployez des balises sur votre site.

## Conditions préalables

Vous devez disposer des autorisations nécessaires pour développer, approuver, publier, gérer les extensions et gérer les environnements au sein de Launch pour pouvoir suivre les leçons suivantes. Si vous ne parvenez pas à effectuer l’une de ces étapes parce que vous n’avez pas accès aux options de l’interface utilisateur, contactez votre administrateur Experience Cloud pour demander l’accès à ces options. Pour plus d’informations sur les autorisations dans Launch, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/user-permissions.html).

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* vous connecter à l’interface utilisateur de Launch ;
* créer une propriété Launch ;
* configurer un propriété Launch.

## Accès à Launch

**Accès à Launch**

1. Connectez-vous à [Adobe Experience Cloud](https://experiencecloud.adobe.com)

1. Cliquez sur l’icône ![icône du sélecteur de solution](images/launch-solutionSwitcher.png) pour ouvrir le sélecteur de solution.

1. Sélectionnez **[!UICONTROL Launch]** dans le menu. ![Ouverture du sélecteur de solution via l’icône et clic sur Activation](images/launch-solutionSwitcherActivation.png).

1. Sous **[!UICONTROL Adobe Experience Cloud Launch]**, cliquez sur le bouton **[!UICONTROL Accéder à Launch]**.

   ![Clic sur le bouton Launch.](images/launch-goToLaunch.png)

L’écran `Properties` devrait s’afficher (si aucune propriété n’a été créée pour ce compte, cet écran peut être vide) :

![Écran Propriétés](images/launch-propertiesScreen.png)

Si vous utilisez Launch fréquemment, vous pouvez également mettre l’URL suivante en signet et vous connecter directement [https://launch.adobe.com](https://launch.adobe.com).

## Création d’une propriété

Une propriété est essentiellement un conteneur que vous remplissez avec des extensions, des règles, des éléments de données et des bibliothèques lorsque vous déployez des balises sur votre site. Une propriété peut être n’importe quel regroupement d’un ou de plusieurs domaines et sous-domaines. Vous pouvez gérer ces ressources et en effectuer le suivi de manière similaire. Par exemple, supposons que vous disposez de plusieurs sites web reposant sur un modèle et que vous souhaitez effectuer le suivi des mêmes ressources sur tous les sites. Vous pouvez appliquer une propriété à plusieurs domaines. Pour en savoir plus sur la création de propriétés, consultez [« Entreprises et propriétés »](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/companies-and-properties.html) dans la documentation du produit.

**Création d’une propriété**

1. Cliquez sur le bouton **[!UICONTROL Nouvelle propriété]** :

   ![Clic sur Nouvelle propriété](images/launch-addNewProperty.png)

1. Nommez votre propriété (par ex. `Launch Tutorial` ou `Daniel's Launch Tutorial`).
1. Saisissez `enablementadobe.com` comme domaine. Il s’agit du domaine sur lequel le site de démonstration Luma est hébergé. Bien que le champ « Domaine » soit obligatoire, la propriété Launch fonctionne sur n’importe lequel domaine où elle est implémentée. La fonction principale de ce champ est de préremplir les options de menu du créateur de règles.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]**.

   ![Création d’une propriété ](images/launch-newProperty.png)

Votre nouvelle propriété devrait s’afficher sur la page Propriétés. Notez que si vous cochez la case en regard du nom de la propriété, les options pour **[!UICONTROL Configurer]** ou **[!UICONTROL Supprimer]** la propriété apparaissent au-dessus de la liste des propriétés. Cliquez sur le nom de votre propriété (p. ex. `Launch Tutorial`) pour ouvrir l’écran `Overview`.
![Clic sur le nom de la propriété pour l’ouvrir](images/launch-openProperty.png)

[Suite : « Ajout du code incorporé Launch »&gt;](launch-add-embed.md)
