---
title: Mise en oeuvre d’Adobe Audience Manager avec le lancement
description: Découvrez comment implémenter Adobe Audience Manager sur votre site Web à l’aide du transfert et du lancement côté serveur. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications iOS Objective-C mobiles.
seo-description: null
seo-title: Mise en oeuvre d’Adobe Audience Manager avec le lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajouter Adobe Audience Manager

Cette leçon vous guidera tout au long des étapes de mise en oeuvre d’Adobe Audience Manager dans le SDK Experience Platform Mobile à l’aide du transfert côté serveur.

[Adobe Audience Manager](https://docs.adobe.com/content/help/en/audience-manager/user-guide/aam-home.html) (AAM) fournit des services de pointe en matière de gestion des données d’audience en ligne, offrant aux publicitaires et aux éditeurs numériques les outils dont ils ont besoin pour contrôler et exploiter leurs ressources de données afin de contribuer au succès des ventes.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

1. Décrivez les deux principales manières d’implémenter Audience Manager dans une application mobile.
1. Ajout d’Audience Manager à l’aide du transfert côté serveur de la balise Analytics
1. Validation de l’implémentation d’Audience Manager

## Conditions préalables 

Pour terminer cette leçon, vous devez :

1. Pour avoir terminé les leçons de la section Configuration du lancement, à savoir [Créer une propriété](launch-create-a-property.md)de lancement, [Ajouter des extensions](launch-add-extensions.md), [Créer une bibliothèque](launch-create-a-library.md)et [Installer le SDK mobile.](launch-install-the-mobile-sdk.md)

1. Admin de l’accès à Adobe Analytics afin que vous puissiez activer le transfert côté serveur pour la suite de rapports que vous utilisez pour ce didacticiel. Vous pouvez également demander à un administrateur existant de votre organisation de le faire à votre place, en suivant les instructions ci-dessous.

Si vous n’avez pas encore mis en oeuvre Audience Manager, suivez ces instructions pour [obtenir votre sous-domaine](https://docs.adobe.com/content/help/en/audience-manager-learn/tutorials/web-implementation/how-to-identify-your-partner-id-or-subdomain.html)Audience Manager.

## Options d’implémentation

Il existe deux manières de mettre en oeuvre Audience Manager dans une application :

* Transfert côté serveur (SSF) : pour les clients qui utilisent Adobe Analytics, il s’agit de la méthode de mise en oeuvre la plus simple et la plus recommandée. Adobe Analytics transfère les données vers AAM sur le serveur principal d’Adobe, de sorte que vous n’ayez pas à envoyer de requêtes de l’application directement à Audience Manager. Cela permet également d’intégrer des fonctionnalités clés et est conforme à nos meilleures pratiques pour l’implémentation et le déploiement du code Audience Manager.

* DIL côté client : cette approche est destinée aux clients qui ne disposent pas d’Adobe Analytics. Les méthodes d’Audience Manager dans l’application envoient directement les données à Audience Manager. Dans ce cas, vous utiliserez l’extension Audience Manager dans Launch lorsque vous configurez votre propriété Launch mobile.

Lorsque vous avez précédemment configuré l’extension Analytics dans la section [Ajouter des extensions](launch-add-extensions.md) de ce didacticiel, vous avez coché la case pour lancer le transfert côté serveur des données d’Analytics vers Audience Manager. Cette opération insère de manière dynamique le code nécessaire pour gérer à nouveau la réponse des segments d’Audience Manager dans votre application. Nous n’ajouterons pas l’extension Audience Manager dans ce didacticiel, car, encore une fois, il s’agit uniquement du cas d’utilisation lorsque vous n’avez PAS Adobe Analytics.

## Activer le transfert côté serveur

Deux étapes principales permettent d’effectuer une implémentation SSF :

1. Activez un "commutateur" dans la console d’administration Analytics pour transférer les données d’Analytics vers Audience Manager *par suite* de rapports.
1. Placez le code du SDK en place, **ce que vous avez fait** via Launch simplement en cochant la case de l’extension Analytics pour transférer les données vers AAM.

### Activer le transfert côté serveur dans l’Admin Console Analytics

Une configuration dans la console d’administration Adobe Analytics est requise pour commencer à transférer des données d’Adobe Analytics vers Adobe Audience Manager. Soyez averti que le système peut prendre jusqu’à quatre heures pour commencer à transmettre les données. N’oubliez donc pas cela lorsque vous dépassez le transfert.

#### Pour activer SSF dans la console d’administration Analytics

1. Connectez-vous à Analytics via l’interface utilisateur d’Experience Cloud. Si vous n’avez pas accès à Analytics par l’administrateur, contactez votre administrateur Experience Cloud ou Analytics pour vous y affecter ou suivez ces étapes.

   ![Connexion à l’interface utilisateur d’Adobe Analytics](images/mobile-aam-logIntoAnalytics.png)

1. Dans le volet de navigation supérieur d’Analytics, choisissez **[!UICONTROL Admin &gt; Report Suites]**, puis, dans la liste, sélectionnez (ou sélectionnez plusieurs fois) la ou les suites de rapports à transférer vers Audience Manager.

   ![Cliquez sur dans la console d’administration.](images/mobile-aam-analyticsAdminConsoleReportSuites.png)

1. Dans l’écran Report Suites et avec les suites de rapports sélectionnées, choisissez **[!UICONTROL Modifier les paramètres &gt; Général &gt; Transfert côté serveur]**.

   ![Sélection du menu SSF](images/mobile-aam-selectSSFmenu.png)

   >[!WARNING] Comme indiqué ci-dessus, vous devez disposer de droits d’administrateur pour afficher cet élément de menu.

1. Une fois sur la page Transfert côté serveur, lisez les informations et cochez la case **[!UICONTROL Activer le transfert]** côté serveur pour les suites de rapports.

1. Cliquez sur **[!UICONTROL Enregistrer]**

   ![Configuration SSF terminée](images/mobile-aam-enableSSFcomplete.png)

>[!NOTE] Puisque le SSF doit être activé par suite de rapports, n’oubliez pas de répéter cette étape pour vos suites de rapports réelles lorsque vous déployez le SSF dans la suite de rapports de votre application.
>
>En outre, si l’option SSF est grisée, vous devrez "mapper les suites de rapports avec votre organisation Experience Cloud pour activer cette option. Cela est expliqué dans [la documentation](https://docs.adobe.com/content/help/en/core-services/interface/about-core-services/report-suite-mapping.html).

Ce commutateur démarre le transfert réel des données vers AAM, tant que le service d’identité Adobe Experience Platform est mis en oeuvre. Le reste de l’implémentation SSF se produit dans le code, qui était géré dans Lancement lorsque vous avez coché la case dans l’extension Analytics pour transférer vers AAM.

Le code de transfert côté serveur est maintenant mis en oeuvre pour votre application !

### Valider le transfert côté serveur

Le principal moyen de vérifier que le transfert côté serveur est en cours d’exécution consiste à examiner la réponse à l’un de vos accès Adobe Analytics provenant de l’application.

Si vous n’effectuez pas de transfert côté serveur (SSF) des données d’Analytics vers Audience Manager, il n’existe vraiment aucune réponse à la balise Analytics (en dehors d’un pixel de 2x2). Cependant, une fois que nous activons SSF, il existe des éléments que vous pouvez vérifier dans la requête et la réponse Analytics afin de vous informer qu’elle fonctionne correctement.

Comme la console Xcode n'affiche pas la réponse aux balises, vous devez utiliser un autre débogueur/renifleur de paquets qui n'affiche pas la réponse, comme Charles Proxy, par exemple (ce que je vais montrer dans ma capture d'écran ci-dessous).

1. Ouvrez votre débogueur et filtrez-le pour `b/ss`, ce qui limitera ce que vous voyez aux requêtes Adobe Analytics.
1. Création et exécution d’un exemple d’application à partir des exercices précédents
1. Pour toutes vos requêtes Analytics, consultez la réponse. Il doit contenir un `dcs_region` paramètre, un `uuid` paramètre et un objet "stuff". Cet objet est l’endroit où les ID de segment AAM sont renvoyés au navigateur (pour tout segment auquel appartient l’utilisateur, qui est affecté dans AAM à une destination de cookie). Si vous avez l'objet "machin", SSF fonctionne !

   ![Réponse AA - Objet de texte](images/mobile-aam-AAresponseCharles.png)

>[!WARNING] Attention au faux "Succès" - S'il y a une réponse, et que tout semble marcher, **assurez** -vous que vous avez cet objet "truc". Si vous ne le faites pas, vous verrez peut-être un message dans la réponse qui indique "status":"SUCCESS". Aussi fou que cela puisse paraître, c'est en fait la preuve qu'il **ne fonctionne pas** correctement. Si vous voyez cela, cela signifie que vous avez terminé l’étape du lancement pour transférer vers AAM, mais que le transfert dans la console d’administration Analytics n’est pas encore terminé. Dans ce cas, vous devez vérifier que vous avez activé SSF dans la console d’administration Analytics. Si vous l'avez fait, et ça n'a pas encore été 4 heures, soyez patient.

![Réponse AA - faux succès](images/mobile-aam-unsuccessful-SSF.png)

[Suivant : "Publier votre propriété" &gt;](publish.md)