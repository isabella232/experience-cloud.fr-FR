---
title: Mise en œuvre d’Adobe Audience Manager avec Launch
description: Découvrez comment mettre en œuvre Adobe Audience Manager sur votre site web à l’aide du transfert côté serveur et de Launch. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS.
seo-description: null
seo-title: Mise en œuvre d’Adobe Audience Manager avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout d’Adobe Audience Manager

Cette leçon vous guidera tout au long des étapes de mise en œuvre d’Adobe Audience Manager dans le SDK Mobile Experience Platform à l’aide du transfert côté serveur.

[Adobe Audience Manager](https://docs.adobe.com/content/help/fr-FR/audience-manager/user-guide/aam-home.html) (AAM) fournit des services de pointe pour la gestion des données d’audience en ligne, dotant les publicitaires et les éditeurs en numérique des outils dont ils ont besoin pour contrôler leurs ressources de données et en tirer profit afin de stimuler leurs ventes.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

1. décrire les deux principales méthodes de mise en œuvre d’Audience Manager dans une application mobile ;
1. ajouter Audience Manager à l’aide du transfert côté serveur de la balise Analytics ;
1. valider la mise en œuvre d’Audience Manager.

## Conditions préalables

Pour pouvoir suivre cette leçon, vous devez :

1. avoir terminé les leçons de la section Configuration de Launch, à savoir [Création d’une propriété Launch](launch-create-a-property.md), [Ajout d’extensions](launch-add-extensions.md), [Création d’une bibliothèque](launch-create-a-library.md) et [Installation du SDK Mobile](launch-install-the-mobile-sdk.md) ;

1. disposer d’un accès à Adobe Analytics pour activer le transfert côté serveur pour la suite de rapports que vous utilisez pour ce tutoriel. Vous pouvez également demander à un administrateur existant de votre organisation de le faire à votre place, en suivant les instructions ci-dessous.

Si vous n’avez pas encore mis en œuvre Audience Manager, suivez ces instructions pour [obtenir votre sous-domaine Audience Manager](https://docs.adobe.com/content/help/en/audience-manager-learn/tutorials/web-implementation/how-to-identify-your-partner-id-or-subdomain.html).

## Options de mise en œuvre

Il existe deux manières de mettre en œuvre Audience Manager dans une application :

* Transfert côté serveur (SSF) : pour les clients avec Adobe Analytics, il s’agit de la méthode de mise en œuvre la plus simple et recommandée. Adobe Analytics transfère les données vers AAM sur le serveur principal d’Adobe, de sorte que vous n’ayez pas à envoyer de requêtes de l’application directement à Audience Manager. Cette solution permet également de bénéficier de fonctionnalités d’intégration clés et se conforme à nos bonnes pratiques en matière de mise en œuvre et de déploiement du code Audience Manager.

* DIL côté client : cette approche est destinée aux clients qui ne disposent pas d’Adobe Analytics. Les méthodes d’Audience Manager dans l’application envoient directement les données à Audience Manager. Dans ce cas, vous utiliserez l’extension Audience Manager dans Launch au moment de la configuration de votre propriété Launch mobile.

Lorsque vous avez précédemment configuré l’extension Analytics dans la section [Ajout d’extensions](launch-add-extensions.md) de ce tutoriel, vous avez coché la case pour lancer le transfert côté serveur des données d’Analytics vers Audience Manager. Cette opération insère de manière dynamique le code nécessaire pour gérer à la réponse des segments d’Audience Manager dans votre application. Nous n’ajouterons pas l’extension Audience Manager à ce tutoriel, car, encore une fois, celle-ci sert uniquement dans le cas où vous ne disposez PAS d’Adobe Analytics.

## Activation du transfert côté serveur

Voici les deux étapes principales d’une mise en œuvre avec transfert côté serveur :

1. Cocher une case dans l’Admin Console Analytics pour transférer les données depuis Analytics vers Audience Manager *par suite de rapports*.
1. Mettre le code du SDK en place, **ce que vous avez fait** via Launch simplement en cochant la case de l’extension Analytics pour transférer les données vers AAM.

### Activation du transfert côté serveur dans l’Admin Console d’Analytics

Il est nécessaire d’effectuer une configuration dans l’Admin Console d’Adobe Analytics pour lancer le transfert de données d’Adobe Analytics vers Adobe Audience Manager. Veuillez noter que le système peut prendre jusqu’à quatre heures pour commencer à transférer les données. Souvenez-vous donc de ce point lorsque vous résolvez les problèmes de transfert.

#### Activation du transfert côté serveur dans l’Admin Console d’Analytics

1. Connectez-vous à Analytics via l’interface utilisateur d’Experience Cloud. Si vous ne disposez pas d’un accès d’administrateur à Analytics, contactez votre administrateur Experience Cloud ou Analytics pour qu’il vous octroie un accès ou qu’il effectue ces étapes pour vous.

   ![Connexion à l’interface utilisateur d’Adobe Analytics](images/mobile-aam-logIntoAnalytics.png)

1. Dans le volet de navigation supérieur d’Analytics, choisissez **[!UICONTROL Admin &gt; Suite de rapports]**, puis, dans la liste, sélectionnez la ou les suites de rapports à transférer vers Audience Manager.

   ![Clic sur l’Admin Console](images/mobile-aam-analyticsAdminConsoleReportSuites.png)

1. Sur l’écran Suite de rapports et une fois les suites de rapports sélectionnées, choisissez **[!UICONTROL Modifier les paramètres &gt; Général &gt; Transfert côté serveur]**.

   ![Sélection du menu de transfert côté serveur](images/mobile-aam-selectSSFmenu.png)

   >[!WARNING] Comme indiqué ci-dessus, vous devez disposer de droits d’administrateur pour afficher cet élément de menu.

1. Une fois sur la page Server-Side Forwarding (Transfert côté serveur), lisez les informations et cochez la case **[!UICONTROL Activer le transfert côté serveur]** pour la ou les suites de rapports.

1. Cliquez sur **[!UICONTROL Enregistrer]**.

   ![Fin de la configuration du transfert côté serveur](images/mobile-aam-enableSSFcomplete.png)

>[!NOTE] Comme le transfert côté serveur doit être activé par suite de rapports, pensez à répéter cette étape pour vos véritables suites de rapports lorsque vous déployez le transfert côté serveur dans la suite de rapports de votre véritable application.
>
>De plus, si l’option Transfert côté serveur est grisée, vous devrez mapper la ou les suites de rapports à votre organisation Experience Cloud afin d’activer l’option. Cela est expliqué dans [la documentation](https://docs.adobe.com/content/help/fr-FR/core-services/interface/about-core-services/report-suite-mapping.html).

L’activation de cette case démarre le transfert réel des données vers AAM, à condition qu’Adobe Experience Platform Identity Service soit mis en œuvre. Le reste de la mise en œuvre du transfert côté serveur se produit dans le code, qui a été géré dans Launch lorsque vous avez coché la case dans l’extension Analytics pour effectuer le transfert vers AAM.

Le code de transfert côté serveur est maintenant mis en œuvre pour votre application.

### Validation du transfert côté serveur

La principale manière de vérifier que le transfert côté serveur est opérationnel consiste à examiner la réponse à l’un de vos accès Adobe Analytics provenant de l’application.

Si vous n’effectuez pas de transfert côté serveur (SSF) des données depuis Analytics vers Audience Manager, alors aucune réponse n’est apportée à la balise Analytics (hormis un pixel 2x2). Toutefois, une fois le transfert côté serveur activé, vous pouvez vérifier certains éléments dans la requête et la réponse Analytics qui vous permettent ensuite de savoir qu’elle fonctionne correctement.

Comme la console Xcode n’affiche pas la réponse aux balises, vous devez utiliser un autre débogueur/renifleur de paquets qui affiche la réponse, comme Charles Proxy, par exemple (vous le verrez dans la copie d’écran ci-dessous).

1. Ouvrez votre débogueur et appliquez un filtre sur `b/ss`, ce qui limitera les éléments visibles des requêtes Adobe Analytics.
1. Créez et exécutez un exemple d’application à partir des exercices précédents.
1. Pour toutes vos requêtes Analytics, consultez la réponse. Elle doit contenir un paramètre `dcs_region`, un paramètre `uuid`, et également un objet « stuff ». Cet objet est l’endroit où les identifiants de segment AAM sont renvoyés au navigateur (pour tout segment auquel appartient l’utilisateur, qui est affecté dans AAM à une destination de cookie). Si vous obtenez l’objet « stuff », c’est que le transfert côté serveur fonctionne.

   ![Réponse AA - objet « stuff »](images/mobile-aam-AAresponseCharles.png)

>[!WARNING] Attention aux faux messages « succès » : si vous avez une réponse et que tout semble fonctionner, **assurez-vous** d’avoir cet objet « stuff ». Dans le cas contraire, un message peut s’afficher dans la réponse et indiquer « status » : « SUCCESS ». Aussi incroyable que cela puisse paraître, c’est pourtant bien la preuve qu’il ne fonctionne **PAS** correctement. Si vous voyez ce message, cela signifie que vous avez terminé l’étape dans Launch pour effectuer le transfert vers AAM, mais celui-ci n’est pas encore terminé dans l’Admin Console d’Analytics. Dans ce cas, vous devez vous assurer d’avoir activé le transfert côté serveur dans l’Admin Console d’Analytics. Si tel est le cas, mais que le délai de quatre heures n’est pas encore passé, soyez patient.

![Réponse AA : faux message « succès »](images/mobile-aam-unsuccessful-SSF.png)

[Suite : « Publication de la propriété » &gt;](publish.md)