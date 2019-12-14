---
title: Ajout d’extensions à une propriété Lancement mobile
description: Découvrez comment ajouter des extensions à une propriété de lancement mobile. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications iOS Objective-C mobiles.
seo-description: null
seo-title: Ajout d’extensions à une propriété Lancement mobile
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajouter des extensions

Dans cette leçon, vous allez ajouter des extensions à votre propriété Launch.

Launch est une plate-forme qui permet à Adobe et aux fournisseurs tiers de créer des extensions pour faciliter le déploiement de leurs solutions via Launch. Une extension est un package de code qui étend l’interface de lancement et la fonctionnalité client. Les extensions vous permettent de choisir uniquement les parties du SDK mobile Adobe Experience Platform dont vous avez besoin pour votre application spécifique.  Launch peut être considéré comme un système d’exploitation, et les extensions comme les applications utilisées pour exécuter les tâches.

Puisque vous implémenterez les solutions Adobe (par exemple, Target, Analytics et Audience Manager), vous ajouterez les extensions nécessaires pour les prendre en charge.

>[!WARNING] Pour ajouter et supprimer des extensions dans les propriétés de lancement mobile, vous devez mettre à jour votre application. Cela diffère des propriétés de lancement Web, dans lesquelles vous pouvez ajouter ou supprimer des extensions à tout moment, sans avoir à mettre à jour votre site Web.

## Conditions préalables 

Votre compte utilisateur Lancer doit être autorisé à "Gérer les extensions" pour terminer cette leçon. Si vous ne parvenez pas à effectuer l’une de ces étapes, car les options de l’interface utilisateur ne sont pas disponibles, contactez votre administrateur Experience Cloud pour y accéder. For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).

Vous aurez besoin des détails suivants sur la solution :

* Au moins un identifiant de suite de rapports Analytics. Les rapports [d’application doivent être activés](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/mobile-management.html)dans la suite de rapports. If you don't have a test/dev report suite that you can use for this tutorial, please [create one](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html). Une suite de rapports suffit pour ce didacticiel, mais dans la réalité, vous souhaiterez utiliser différentes suites de rapports pour vos environnements de développement, de test et de production.

* Votre serveur de suivi Analytics. Vous pouvez récupérer votre serveur de suivi à partir de votre implémentation actuelle, de votre conseiller Adobe ou de votre représentant du service à la clientèle.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Ajout d’extensions à une propriété de lancement mobile
* Configuration de l’extension Analytics
* Configuration des extensions du compositeur d’expérience visuelle de Target et de Target

>[!NOTE] Adobe Audience Manager peut être implémenté via une configuration dans l’extension Analytics. Vous n’aurez donc pas besoin d’ajouter l’extension Audience Manager dans ce didacticiel

## Vérification des extensions préinstallées

1. Cliquez sur l’onglet **[!UICONTROL Extensions]** pour accéder à la page Extensions.
1. Notez que Mobile Core et les `Profile` extensions sont préinstallés dans votre nouvelle propriété mobile.
1. Cliquez sur le bouton **[!UICONTROL Configurer]** de l’extension Core pour examiner ses paramètres.

   ![Accéder à l’onglet Extensions](images/mobile-extensions-installed-default.png)

1. L’extension Mobile Core représente le SDK mobile Adobe Experience Platform de base requis pour toute implémentation d’application. Le noyau contient un ensemble commun de fonctionnalités et de structures, telles qu’un service Experience Cloud Identity, un concentrateur d’événements de données, un moteur de règles, une mise en réseau réutilisable, des routines d’accès au disque, etc., requises par toutes les extensions Adobe et tierces.  Pour plus d’informations sur l’extension Mobile Core, voir [la documentation](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core).

   1. Notez que votre ID d’organisation Experience Cloud est détecté automatiquement et prérempli.
   1. Le champ Serveur Experience Cloud vous permet de spécifier un point de fin personnalisé pour les demandes du service d’identification des visiteurs. Utilisez le paramètre par défaut (laissez ce champ vide) pour ce didacticiel.
   1. Le champ Délai d’expiration de la session vous permet de spécifier le moment auquel une session de cycle de vie d’application doit expirer. Par défaut, il expirera si l’application est en arrière-plan pendant 300 secondes. Utilisez le paramètre par défaut de ce didacticiel.

1. Puisque vous n’avez modifié aucun des paramètres, cliquez sur **[!UICONTROL Annuler]** pour quitter la configuration de l’extension.

   ![Cliquez sur Annuler pour quitter la configuration.](images/mobile-extensions-core-cancel.png)

1. L’extension de profil permet au SDK de stocker des données dans un profil client. Il n'a aucune configuration, il n'y a donc rien à regarder. Pour plus d’informations sur l’extension de profil, voir [la documentation](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/profile).

## Ajout des extensions de solution

Il est maintenant temps de passer à la partie amusante et de commencer à ajouter les extensions pour les solutions que vous allez implémenter dans ce didacticiel. Lors de l’utilisation de Launch avec les applications mobiles, l’application doit être mise à jour chaque fois qu’une extension est ajoutée ou supprimée. Afin de gagner du temps plus tard, nous ajouterons toutes les extensions de cette leçon. Ignorez les solutions que votre entreprise n'a pas de licence.

### Ajout de l’extension Adobe Analytics

>[!NOTE] Si vous n’avez pas de licence pour Adobe Analytics, vous pouvez ignorer cette section. Actuellement, l’extension Analytics pour les propriétés mobiles est utilisée uniquement pour gérer les paramètres du SDK et n’ajoute pas d’options d’interface au lancement, telles que les actions de règle.

**Pour ajouter l’extension**

1. Cliquez sur l’onglet Catalogue pour afficher les extensions _désinstallées_ .

1. Recherchez l’extension **[!UICONTROL Adobe Analytics]** et cliquez sur **[!UICONTROL Installer.]**

   ![Accédez au catalogue des extensions et cliquez sur Installer pour ajouter l’extension Analytics.](images/mobile-extensions-catalog-installAnalytics.png)

1. Sélectionnez vos suites **[!UICONTROL de]** rapports dans les listes préremplies. Il s’agit des suites de rapports auxquelles l’application enverra des données. Vous pouvez sélectionner différentes suites de rapports pour vos environnements de développement, de test et de production.
1. Votre serveur **[!UICONTROL de suivi]** Analytics peut être prérempli ou vous devrez peut-être le sélectionner dans une liste préremplie ou le saisir manuellement. Il s’agit du domaine auquel les balises seront envoyées, généralement au format `yoursite.sc.omtrdc.net`.
1. Cochez la case **[!UICONTROL Hors connexion activée]**. Lorsque la case à cocher Activé hors ligne est activée, les accès Analytics sont placés en file d’attente lorsque votre périphérique est hors ligne et sont envoyés ultérieurement lorsque votre périphérique est de nouveau en ligne. Pour utiliser le suivi hors ligne, **vérifiez** que l’horodatage de votre suite de rapports est activé. Pour plus d’informations, consultez la [documentation ](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/offline-tracking.html).
1. Cochez la case Transfert **[!UICONTROL d’Audience Manager]**. Les données Analytics seront transférées à Audience Manager, de sorte que vous n’aurez pas à effectuer un appel supplémentaire de l’application vers Audience Manager. Dans cet exercice, nous supposons que vous disposez d’Audience Manager et que vous transmettez donc les données d’Analytics. Si vous ne disposez pas d’Audience Manager, ne cochez pas cette case lorsque vous configurez Analytics pour votre propre mise en oeuvre.
1. Cochez la case pour **[!UICONTROL antidater les informations de session précédente.]**
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** .

   ![Accédez au catalogue des extensions et cliquez sur Installer pour ajouter l’extension Analytics.](images/mobile-extensions-analytics-settings.png)

### Ajouter l’extension Target

Adobe Target comporte deux extensions officielles, l’extension Adobe Target et l’extension Adobe Target VEC. Adobe Target prend en charge toutes les API familières aux utilisateurs de nos SDK mobiles antérieurs. L’extension du compositeur d’expérience visuelle d’Adobe Target prend en charge le compositeur d’expérience visuelle de Target, ce qui permet aux spécialistes du marketing de créer des activités simples qui modifient les éléments d’image et de texte de la page dans une interface WYSIWYG (What-You-See-Is-What-You-Get). Dans ce didacticiel, vous utiliserez les deux.

>[!NOTE] Si vous n’avez pas de licence pour Adobe Target, vous pouvez ignorer cette section. Actuellement, l’extension Target pour les propriétés mobiles sert uniquement à gérer les paramètres du SDK et n’ajoute pas d’options d’interface au lancement, telles que les actions de règle.

**Pour ajouter l’extension**

1. Cliquez sur l’onglet Catalogue pour afficher les extensions _désinstallées_ .

1. Recherchez l’extension **[!UICONTROL Adobe Target]** et cliquez sur **[!UICONTROL Installer.]**

   ![Accédez au catalogue des extensions et cliquez sur Installer pour ajouter l’extension Target.](images/mobile-extensions-catalog-installTarget.png)

1. Votre code **** client sera prérempli.
1. Laissez l’ID **** d’environnement vide. Ce paramètre est utilisé conjointement avec la fonction [Hôtes](https://docs.adobe.com/help/en/target/using/administer/hosts.html) d’Adobe Target, qui vous permet d’envoyer les données à différents environnements de création de rapports (par exemple, développement, test, production). Par défaut, les données sont envoyées à l’environnement Production.
1. Laissez la propriété **[!UICONTROL de l’espace de travail]** Target vide. Ce paramètre est utilisé conjointement avec la fonction d’autorisation [utilisateur de Target Premium](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/property-channel.html) Enterprise.
1. Laissez le **[!UICONTROL délai d’expiration]** défini sur 5 secondes. Ce paramètre contrôle la durée pendant laquelle l’application doit attendre la réponse Target avant d’afficher le contenu par défaut.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** .

   ![Configuration des paramètres de Target](images/mobile-extensions-target-settings.png)

### Ajout de l’extension Target VEC

Maintenant que l’extension Target a été ajoutée, vous pouvez ajouter l’extension Target VEC.

>[!NOTE] Si vous n’avez pas de licence pour Adobe Target, vous pouvez ignorer cette section. Actuellement, l’extension du compositeur d’expérience visuelle de Target pour les propriétés mobiles est utilisée uniquement pour gérer les paramètres du SDK et n’ajoute pas d’options d’interface au lancement, telles que les actions de règle.

**Pour ajouter l’extension**

1. Cliquez sur l’onglet Catalogue pour afficher les extensions _désinstallées_ .

1. Recherchez l’extension **[!UICONTROL Adobe Target VEC]** et cliquez sur **[!UICONTROL Installer.]**

   ![Accédez au catalogue des extensions et cliquez sur Installer pour ajouter l’extension Target.](images/mobile-extensions-catalog-installTargetVEC.png)

1. Activez la fonction **[!UICONTROL Récupérer automatiquement les campagnes]**`ON` cibles. Cette opération prérécupère toutes les activités Target au premier chargement de l’application, réduisant ainsi le nombre de demandes à effectuer.
1. Laissez **[!UICONTROL Récupérer En Arrière-Plan]**`OFF`. Ce paramètre s’affiche uniquement lorsque `Auto-Fetch Target Campaigns` vous l’utilisez.  Si vous laissez ce paramètre, vous `OFF` pourrez exécuter des activités du compositeur d’expérience visuelle sur l’écran d’accueil de l’application, mais vous ajouterez également un délai au démarrage de l’application pour vous assurer que la demande Target est terminée ou expirée avant l’affichage de l’écran d’accueil. Nous vous recommandons de conserver ce paramètre `OFF` lorsque vous exécutez des activités sur l’écran d’accueil et de le désactiver `ON` lorsque vous ne le faites pas.  Ce paramètre peut être modifié à tout moment dans l’interface de lancement sans mettre à jour votre application.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** .

   ![Configuration des paramètres du compositeur d’expérience visuelle de Target](images/mobile-extensions-targetVEC-settings.png)

Vous avez terminé. Maintenant que vous avez ajouté les extensions à votre propriété, vous pouvez les ajouter à une bibliothèque :

[Suivant : "Créer une bibliothèque" &gt;](launch-create-a-library.md)