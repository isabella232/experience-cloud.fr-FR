---
title: Ajout d’extensions à une propriété Launch mobile
description: Découvrez comment ajouter des extensions à une propriété Launch mobile. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles pour Android.
seo-description: null
seo-title: Ajout d’extensions à une propriété Launch mobile
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajout d’extensions

Dans cette leçon, vous allez ajouter des extensions à votre propriété Launch.

Launch est une plateforme qui permet à Adobe et aux fournisseurs tiers de créer des extensions pour faciliter le déploiement de leurs solutions via Launch. Une extension est un module de code qui étend l’interface de Launch et les fonctionnalités du client. Les extensions vous permettent de choisir uniquement les parties du SDK Mobile Adobe Experience Platform dont vous avez besoin pour votre application. Launch peut être considéré comme un système d’exploitation, et les extensions comme les applications utilisées pour exécuter les tâches.

Puisque vous allez mettre en œuvre les solutions Adobe (par exemple, Target, Analytics et Audience Manager), vous devez ajouter les extensions nécessaires pour les prendre en charge.

>[!CAUTION] Pour ajouter et supprimer des extensions dans des propriétés mobiles pour Lauch, vous devez mettre à jour votre application. Cela diffère des propriétés Launch web, dans lesquelles vous pouvez ajouter ou supprimer les extensions à tout moment, sans avoir à mettre à jour votre site web.

## Conditions préalables

Votre compte utilisateur Launch doit disposer de l’autorisation « Gérer les extensions » pour terminer cette leçon. Si vous ne parvenez pas à effectuer l’une de ces étapes parce que vous n’avez pas accès aux options de l’interface utilisateur, contactez votre administrateur Experience Cloud pour demander l’accès à ces options. Pour plus d’informations sur les autorisations dans Launch, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/user-permissions.html).

Vous aurez besoin des informations suivantes sur la solution :

* Un identifiant de suite de rapports Analytics. Si vous ne disposez pas d’une suite de rapports de test/développement que vous pouvez utiliser pour ce tutoriel, créez-en une. Si vous n’êtes pas sûr de la marche à suivre, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html).

* Votre serveur de suivi Analytics. Vous pouvez récupérer votre serveur de suivi à partir de votre mise en œuvre actuelle, ou auprès de votre consultant ou de votre représentant de l’Assistance clientèle Adobe.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* ajouter des extensions à une propriété Launch mobile ;
* configurer l’extension Analytics ;
* configurer des extensions Target et Target VEC.

>[!NOTE] Vous pouvez mettre en œuvre Adobe Audience Manager par le biais d’une configuration dans l’extension Analytics. Vous n’aurez donc pas besoin d’ajouter l’extension Audience Manager dans ce tutoriel.

## Vérification des extensions préinstallées

1. Cliquez sur l’onglet **[!UICONTROL Extensions]** pour accéder à la page des extensions.
1. Notez que les extensions Mobile Core et `Profile` sont préinstallées dans votre nouvelle propriété mobile.
1. Cliquez sur le bouton **[!UICONTROL Configurer]** de l’extension Core pour en examiner les paramètres.

   ![Accès à l’onglet Extensions](images/mobile-extensions-installed-default.png)

1. L’extension Mobile Core constitue le SDK Mobile Adobe Experience Platform de base requis pour toute implémentation d’application. Elle contient un ensemble commun de fonctionnalités et de structures, telles qu’Experience Cloud Identity Service, un centre d’événements de données, un moteur de règles, une mise en réseau réutilisable, des routines d’accès au disque, etc., requises par toutes les extensions Adobe et tierces. Pour plus d’informations sur l’extension Mobile Core, consultez [la documentation](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core).

   1. Notez que votre ID d’organisation Experience Cloud est détecté automatiquement et prérenseigné.
   1. Le champ Serveur Experience Cloud vous permet de spécifier un point de terminaison personnalisé pour les requêtes du service d’identification des visiteurs. Utilisez le paramètre par défaut (laissez le champ vide) pour ce tutoriel.
   1. Le champ Délai d’expiration de la session vous permet de spécifier le moment auquel la session de cycle de vie des applications doit expirer. Par défaut, il expire lorsque l’application est restée en arrière-plan pendant 300 secondes. Utilisez le paramètre par défaut pour ce tutoriel.

1. Puisque vous n’avez modifié aucun des paramètres, cliquez sur **[!UICONTROL Annuler]** pour quitter la configuration de l’extension.

   ![Clic sur Annuler pour quitter la configuration](images/mobile-extensions-core-cancel.png)

1. L’extension Profile permet au SDK de stocker des données dans un profil côté client. Elle ne propose aucune configuration, il n’y a donc rien à analyser. Pour plus d’informations sur l’extension Profile, consultez [la documentation](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/profile).

## Ajout d’extensions aux solutions

Il est maintenant temps de passer à la partie amusante et de commencer à ajouter les extensions pour les solutions que vous allez mettre en œuvre dans ce tutoriel. Lorsque vous utilisez Launch avec des applications mobiles, l’application doit être mise à jour à chaque fois qu’une extension est ajoutée ou supprimée. Afin de gagner du temps par la suite, nous allons ajouter toutes les extensions dans cette leçon. Ignorez les solutions pour lesquelles votre entreprise n’a pas de licence.

### Ajout de l’outil Adobe Analytics

>[!NOTE] Si vous n’avez pas de licence pour Adobe Analytics, vous pouvez ignorer cette section. L’extension Analytics pour les propriétés mobiles est pour l’instant utilisée uniquement pour gérer les paramètres du SDK et n’ajoute pas d’options d’interface à Launch, telles que les actions de règle.

**Ajout de l’extension**

1. Cliquez sur l’onglet Catalogue pour afficher les extensions _non installées_.

1. Recherchez l’extension **[!UICONTROL Adobe Analytics]** et cliquez sur **[!UICONTROL Installer]**.

   ![Accès au catalogue des extensions et clic sur Installer pour ajouter l’extension Analytics](images/mobile-extensions-catalog-installAnalytics.png)

1. Sélectionnez vos **[!UICONTROL suites de rapports]** dans les listes préremplies. Il s’agit des suites de rapports auxquelles l’application enverra des données. Vous pouvez sélectionner différentes suites de rapports pour vos environnements de développement, d’évaluation et de production.
1. Votre **[!UICONTROL Serveur de suivi Analytics]** peut être prérenseigné ou vous devrez peut-être le sélectionner dans une liste prérenseignée ou le saisir manuellement. Il s’agit du domaine auquel les balises seront envoyées, généralement au format `yoursite.sc.omtrdc.net`.
1. Cochez la case **[!UICONTROL Connexion hors ligne activée]**. Quand la case Connexion hors ligne activée est cochée, les accès Analytics sont placés en file d’attente lorsque votre appareil est hors ligne. Ils sont envoyés ultérieurement, une fois que votre périphérique est de nouveau en ligne. Pour utiliser le suivi hors ligne, **vérifiez** que l’horodatage de votre suite de rapports est activé. Pour plus d’informations, consultez la [documentation ](https://docs.adobe.com/content/help/fr-FR/analytics/implementation/javascript-implementation/offline-tracking.html).
1. Cochez la case **[!UICONTROL Transfert Audience Manager]**. Les données Analytics seront transférées vers Audience Manager, de sorte que vous n’aurez pas à effectuer d’appel supplémentaire de l’application vers Audience Manager. Dans cet exercice, nous supposons que vous disposez d’Audience Manager et que vous transmettez donc les données d’Analytics. Si vous ne disposez pas d’Audience Manager, ne cochez pas cette case lorsque vous configurerez Analytics pour votre propre mise en œuvre.
1. Cochez la case **[!UICONTROL Antidater les informations de la session précédente]**.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]**.

   ![Accès au catalogue des extensions et clic sur Installer pour ajouter l’extension Analytics](images/mobile-extensions-analytics-settings.png)

### Ajout de l’extension Target

Il existe deux extensions officielles Adobe Target : l’extension Adobe Target et l’extension Adobe Target VEC. Adobe Target prend en charge toutes les API connues des utilisateurs de nos SDK mobiles antérieurs. L’extension Adobe Target VEC prend en charge le compositeur d’expérience visuelle de Target, ce qui permet aux spécialistes marketing de créer des activités simples qui modifient les éléments d’image et de texte de la page dans une interface WYSIWYG (What-You-See-Is-What-You-Get). Dans le cadre de ce tutoriel, vous utiliserez les deux.

>[!NOTE] Si vous n’avez pas de licence pour Adobe Target, vous pouvez ignorer cette section. Actuellement, l’extension Target pour les propriétés mobiles est utilisée uniquement pour gérer les paramètres du SDK et n’ajoute pas d’options d’interface à Launch, telles que les actions de règle.

**Ajout de l’extension**

1. Cliquez sur l’onglet Catalogue pour afficher les extensions _non installées_.

1. Recherchez l’extension **[!UICONTROL Adobe Target]** et cliquez sur **[!UICONTROL Installer]**.

   ![Accès au catalogue des extensions et clic sur Installer pour ajouter l’extension Target](images/mobile-extensions-catalog-installTarget.png)

1. Votre **[!UICONTROL code client]** sera prérenseigné.
1. Ne renseignez pas d’**[!UICONTROL ID d’environnement]**. Ce paramètre est utilisé conjointement avec la fonction [Hôtes](https://docs.adobe.com/help/fr-FR/target/using/administer/hosts.html) d’Adobe Target, qui vous permet d’envoyer les données à différents environnements de création de rapports (par exemple, Développement, Évaluation, Production). Par défaut, les données sont envoyées à l’environnement de production.
1. Ne renseignez pas de **[!UICONTROL propriété d’espace de travail Target]**. Ce paramètre est utilisé conjointement avec la fonction [Autorisations des utilisateurs Enterprise](https://docs.adobe.com/content/help/fr-FR/target/using/administer/manage-users/enterprise/property-channel.html) Target Premium.
1. Laissez le **[!UICONTROL délai d’expiration]** défini sur 5 secondes. Ce paramètre contrôle la durée pendant laquelle l’application doit attendre la réponse de Target avant d’afficher le contenu par défaut.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]**.

   ![Configuration des paramètres de Target](images/mobile-extensions-target-settings.png)

### Ajout de l’extension Target VEC

Maintenant que l’extension Target a été ajoutée, vous pouvez ajouter l’extension Target VEC.

>[!NOTE] Si vous n’avez pas de licence pour Adobe Target, vous pouvez ignorer cette section. L’extension Target VEC pour les propriétés mobiles est pour l’instant utilisée uniquement pour gérer les paramètres du SDK et n’ajoute pas d’options d’interface à Launch, telles que les actions de règle.

**Ajout de l’extension**

1. Cliquez sur l’onglet Catalogue pour afficher les extensions _non installées_.

1. Recherchez l’extension **[!UICONTROL Adobe Target VEC]** et cliquez sur **[!UICONTROL Installer]**.

   ![Accès au catalogue des extensions et clic sur Installer pour ajouter l’extension Target](images/mobile-extensions-catalog-installTargetVEC.png)

1. Définissez la fonction **[!UICONTROL Récupérer automatiquement les campagnes Target]** sur `ON`. Cette opération permet de récupérer au préalable toutes les activités de Target lors du premier chargement de l’application, réduisant ainsi le nombre de requêtes à effectuer.
1. Laissez le champ **[!UICONTROL Récupérer en arrière-plan]** sur `OFF`. Ce paramètre s’affiche uniquement lorsque vous utilisez `Auto-Fetch Target Campaigns`. Si vous laissez ce paramètre sur `OFF`, vous pourrez exécuter des activités du VEC sur l’écran d’accueil de l’application, mais vous ajouterez également un délai au démarrage de l’application pour vous assurer que la demande Target est terminée ou expirée avant l’affichage de l’écran d’accueil. Nous vous recommandons de laisser ce paramètre sur `OFF` lorsque vous exécutez des activités sur l’écran d’accueil, et de le basculer sur `ON` lorsque vous n’en exécutez pas. Ce paramètre peut être modifié à tout moment dans l’interface de Launch sans mettre à jour votre application.
1. Cliquez sur le bouton **[!UICONTROL Enregistrer]**.

   ![Configuration des paramètres du VEC de Target](images/mobile-extensions-targetVEC-settings.png)

Vous avez terminé. Maintenant que vous avez ajouté les extensions à votre propriété, vous pouvez les ajouter à une bibliothèque :

[Suite : « Création d’une bibliothèque » &gt;](launch-create-a-library.md)