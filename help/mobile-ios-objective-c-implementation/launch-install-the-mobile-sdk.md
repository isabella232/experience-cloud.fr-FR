---
title: Installation du SDK Adobe Mobile dans une application Mobile iOS Objective-C
description: Découvrez comment obtenir les codes incorporés de votre propriété Launch et les implémenter dans votre site Web. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications iOS Objective-C mobiles.
seo-description: null
seo-title: Installation du SDK Adobe Mobile dans une application Mobile iOS Objective-C
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: bd91e0e5f11b94f9a343ab60e7d4e6e25e181f9d

---


# Installation du SDK Mobile

Dans cette leçon, vous allez mettre en oeuvre le kit Mobile SDK avec les extensions et les paramètres correspondant à l’environnement de développement de la propriété Launch.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Suivez les instructions d’installation de la propriété Lancement mobile.
* Comprendre la différence entre environnement de développement, d’évaluation et de production
* Création et modification du fichier de contenu
* Importation du kit SDK Mobile dans votre fichier AppDelegate
* Vérifiez que le SDK a bien été implémenté

## Obtention des instructions d’installation

Les instructions d’installation pour le lancement mobile sont une collection de fragments de code que vous exécutez dans votre terminal ou ajoutez à des emplacements spécifiques dans votre application mobile.

Cliquez sur l’ `Environments` onglet de la navigation supérieure pour accéder à la page des environnements. Notez que les environnements de développement, de test et de production ont été précréés pour vous. Elles correspondent aux environnements typiques du processus de développement et de publication du code. Le code est d’abord écrit par un développeur dans un environnement de développement. Lorsqu’ils ont terminé leur travail, les développeurs envoient le code à un environnement d’évaluation où il est révisé par des équipes d’assurance qualité et d’autres équipes. Une fois l’assurance qualité et les autres équipes satisfaites, le code est ensuite publié dans l’environnement Production, qui est l’environnement public que vos visiteurs voient lorsqu’ils téléchargent votre application.

Le lancement permet d’autres environnements de développement, ce qui est utile dans les grandes entreprises où plusieurs développeurs travaillent simultanément sur différents projets.

Le développement, le test et la production sont les seuls environnements nécessaires pour compléter le didacticiel.

![Cliquez sur Environnements dans la barre de navigation supérieure.](images/mobile-launch-environments.png)

Dans la ligne **[!UICONTROL Développement]** , cliquez sur l’icône ![](images/mobile-launch-installIcon.png) Installer pour ouvrir le mode de code incorporé.

![Cliquez sur l’icône pour ouvrir le mode de code incorporé](images/mobile-launch-openEmbedCode.png)

Passons en revue les instructions étape par étape.

## Création du fichier Podfile et installation des modules

Si vous avez déjà utilisé Launch dans des sites Web, l’une des premières choses que vous remarquerez est qu’il y a beaucoup plus d’informations dans ce module que pour les propriétés Web.

Le SDK mobile Adobe pour iOS utilise les CocoaPods pour gérer les dépendances entre ses différents composants. Si vous n’avez pas encore [CocoaPods](https://cocoapods.org/) installé dans votre environnement de développement, suivez les instructions d’installation sur leur site Web. En outre, si vous n’avez pas encore téléchargé l’application [de réservation de](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)bus, enregistrez-la sur votre ordinateur local et extrayez l’archive zip sur votre bureau.

**Pour créer le fichier de contenu**

1. Ouvrez l’ `Terminal` application sur votre Mac®.

1. Accédez au dossier du projet dans lequel vous avez enregistré l'application Objectif-C de réservation de bus (ex. `cd Desktop/busbooking-mobileapps-master/Objective-C/`)

   ![accéder au répertoire du projet](images/ios/objective-c/mobile-launch-install-goToProjectDirectory.png)

1. Dans l’interface de lancement, modifiez le système d’exploitation en `iOS`

1. Copiez la première instruction iOS `pod init`, en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png)

   ![Copiez la capsule dans votre presse-papiers dans l’interface de lancement.](images/ios/mobile-launch-install-copyPodInit.png)

1. Dans votre application Terminal Server, exécutez la `pod init` commande et attendez qu’elle soit terminée.

   ![Exécuter la capsule init](images/ios/objective-c/mobile-launch-install-runPodInit.png)

1. Dans votre application Terminal Server, ouvrez le fichier podfile à l’aide de la `open podfile` commande

   ![Exécution d’un fichier podfile ouvert](images/ios/objective-c/mobile-launch-install-openPodfile.png)

1. Votre ordinateur peut ouvrir une boîte de dialogue vous demandant avec quelle application vous souhaitez ouvrir le fichier de module. Choisissez n’importe quel éditeur de texte, par exemple `TextEdit`

1. Dans l’interface de lancement, copiez la liste des dépendances en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png) . Notez qu’une ligne correspond à chacune des extensions que vous avez ajoutées dans la leçon précédente. Chaque extension possède son propre jeu de code qui repose sur l’extension Mobile Core et ne peut être ajouté ou supprimé qu’avec une mise à jour d’application :

   ![Copier les dépendances dans le Presse-papiers dans l’interface de lancement](images/ios/mobile-launch-install-copyDependencies.png)

1. Dans l’éditeur de texte, collez les dépendances du presse-papiers juste après la ligne. `# Pods for BusBookingObjectiveC`

1. Enregistrez les mises à jour dans le fichier de module dans votre éditeur de texte.

   ![Ajouter des dépendances et enregistrer](images/ios/objective-c/mobile-launch-install-addDependenciesAndSave.png)

1. Vous pouvez désormais fermer votre éditeur de texte

1. Dans l’interface de lancement, copiez la prochaine instruction iOS `pod repo update`en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png) .

   ![Copier la mise à jour du référentiel de capsule](images/ios/mobile-launch-install-copyPodRepoUpdate.png)

1. Dans votre application Terminal Server, exécutez la `pod repo update` commande et attendez qu’elle se termine (cela peut prendre quelques minutes).

   ![Exécution de la mise à jour du référentiel de capsule](images/ios/objective-c/mobile-launch-install-podRepoUpdate.png)

1. Dans l’interface de lancement, copiez la prochaine instruction iOS `pod install`en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png) .

   ![Copier l’installation de la capsule dans le Presse-papiers dans l’interface de lancement](images/ios/mobile-launch-install-copyPodInstall.png)

1. Dans votre application Terminal Server, exécutez la `pod install` commande et attendez qu’elle soit terminée.

   ![Exécution de l’installation du module](images/ios/objective-c/mobile-launch-install-podInstall.png)

1. Vous pouvez désormais fermer la fenêtre Terminal

1. Ouvrez une fenêtre du Finder, accédez au dossier dans lequel vous avez enregistré l'application de réservation de bus et vérifiez que le fichier BusBookingObjectiveC.xcworkspace, le fichier Podfile, le fichier Podfile.lock et le dossier Pods ont été créés.

   ![Confirmation des capsules dans l’outil de recherche](images/ios/objective-c/mobile-launch-install-podsInFinder.png)

## Mise à jour d’AppDelegate

Il est maintenant temps de mettre à jour l’application pour importer le SDK :

1. Ouvrir le `BusBookingObjectiveC.xcworkspace` fichier dans XCode
1. Ouvrir le `AppDelegate.m` fichier

   ![Ouverture du fichier AppDelegate](images/ios/objective-c/mobile-launch-install-openAppDelegate.png)

1. Dans l’interface de lancement, accédez à la section **[!UICONTROL Ajouter un code]** d’initialisation et sélectionnez **[!UICONTROL Objectif C]** comme langue iOS utilisée.
1. Copiez les instructions d’importation en cliquant sur la première icône ![Copier](images/mobile-launch-copyIcon.png) de la section **[!UICONTROL Ajouter un code]** d’initialisation :

   ![Copier les instructions d’importation dans le Presse-papiers](images/ios/objective-c/mobile-launch-install-copyImports.png)

1. Dans XCode, collez ces instructions d’importation dans le `AppDelegate.m` fichier après l’importation pour la variable `AppDelegate.h`

   ![Collez les instructions d’importation dans votre fichier AppDelegate.](images/ios/objective-c/mobile-launch-install-pasteImports.png)

1. Dans l’interface de lancement, copiez les deux lignes liées à l’extension Core en cliquant sur la deuxième icône ![Copier](images/mobile-launch-copyIcon.png) dans la section **[!UICONTROL Ajouter un code]** d’initialisation. La première ligne active les instructions de journalisation de la console (les options disponibles sont "debug", "verbose", "warning" et "error"). La deuxième ligne pointe vers l’identifiant unique de l’environnement de lancement. C’est important, car vous devrez mettre à jour cette valeur lorsque nous serons prêts à déployer l’application dans l’environnement de production.

   ![Copiez les instructions Core dans le Presse-papiers](images/ios/objective-c/mobile-launch-install-copyCore.png)

1. Dans XCode, collez les instructions de base suivantes dans le fichier AppDelegate en haut de la `application(_:didFinishLaunchingWithOptions:)` méthode :

   ![Collez les instructions Core dans votre fichier AppDelegate](images/ios/objective-c/mobile-launch-install-pasteCore.png)

1. Dans l’interface de lancement, copiez les instructions d’extension en cliquant sur la troisième icône ![Copier](images/mobile-launch-copyIcon.png) de la section [!UICONTROL Ajouter un code] d’initialisation :

   ![Copier les instructions Extension dans le Presse-papiers](images/ios/objective-c/mobile-launch-install-copyExtensions.png)

1. Dans XCode, collez ces instructions d’extension dans le fichier AppDelegate juste avant la `return true` ligne de la `application(_:didFinishLaunchingWithOptions:)` méthode :

   ![Collez les instructions Extension dans votre fichier AppDelegate](images/ios/objective-c/mobile-launch-install-pasteExtension.png)

>[!NOTE] Les instructions d’installation de Mobile fournies dans l’interface de lancement incluent les instructions d’importation et d’enregistrement pour les extensions d’identité, de cycle de vie et de signal, ainsi que l’initialisation des mesures de cycle de vie. Ces extensions sont considérées comme faisant partie de l’extension Mobile Core. Si vous ne souhaitez pas utiliser ces extensions dans votre application, vous n’avez pas besoin d’importer, d’enregistrer ou de mettre en oeuvre un autre code associé à ces extensions.
>
> D’autres options d’implémentation doivent également être prises en compte lors de l’utilisation de ces extensions (par exemple, vous pouvez suspendre/redémarrer la collection Lifecycle lorsque l’utilisateur met l’application en arrière-plan/en avant-plan). Pour en savoir plus, consultez [la documentation de l’extension Mobile Core.](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)

## Vérification de l’implémentation

1. Enregistrer votre projet XCode
1. Exécutez l’application et lancez-la dans le simulateur. Si aucun simulateur n’est configuré, configurez-en un maintenant, en veillant à configurer un périphérique exécutant iOS 10+. Nous aimons utiliser un simulateur iPhone 8 car il est facile de cliquer sur le `Home` bouton avec la souris.

   ![Exécutez l’application et lancez-la dans l’émulateur.](images/ios/objective-c/mobile-launch-install-buildAndLaunch.png)

1. Attendez que le simulateur se lance et ouvre complètement l’application sur l’écran de réservation (cela peut prendre quelques minutes).

   ![Attendez que l’application s’ouvre complètement.](images/ios/mobile-launch-install-simulator.png)

1. Confirmez que des appels sont envoyés aux serveurs Adobe dans la console XCode.

   ![Rechercher des appels dans la console](images/ios/objective-c/mobile-launch-install-console.png)

Voici quelques exemples d’appels spécifiques que vous pouvez rechercher :

1. **Appels pour récupérer la configuration** de lancement (filtrez votre console vers `adobedtm.com`). Notez les configurations d’extension que vous avez saisies dans la leçon précédente. Bien que l’ajout de l’extension nécessite une mise à jour de l’application, ces paramètres peuvent être gérés en externe au lancement et modifiés à tout moment :

   ```objective-c
   2019-03-13 16:53:26.633816-0400 BusBookingObjectiveC[56630:3854917] [AMSDK DEBUG <RulesDownloader>]: Successfully downloaded Rules from 'https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip'
   
   {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.clientCode":"techmarketingdemos","target.autoFetch":true,"target.fetchBackground":false,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **Demande au service** d’identité (filtrez votre console vers `demdex.net`) Dans cet exemple, l’ID (`d_mid`) a déjà été défini et est simplement de nouveau signalé)

   ```objective-c
   2019-03-13 16:53:26.655908-0400 BusBookingObjectiveC[56630:3854937] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   Sending request (https://dpm.demdex.net/id?d_rtbd=json&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61@AdobeOrg&d_mid=67027929491180584128922600814231770586)
   ```

1. **Réponse du service** d’identité (filtrez votre console vers `ID Service`). Notez comment la `mid` valeur correspond à la `d_mid` valeur dans la requête ci-dessus :

   ```objective-c
   2019-03-13 16:53:27.397048-0400 BusBookingObjectiveC[56630:3854937] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   ID Service - Got ID Response (mid: 67027929491180584128922600814231770586, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: "604800000 ms")
   ```

1. **Requête** Analytics (filtrer votre console vers `Analytics request`)

   ```objective-c
   2019-03-13 16:53:27.689061-0400 BusBookingObjectiveC[56630:3855024] [AMSDK DEBUG <AnalyticsHitDatabase>]: Analytics request was sent with body
   
   (ndh=1&c.&a.&AppID=BusBookingObjectiveC%201%20%281.0%29&CarrierName=%28null%29&DailyEngUserEvent=DailyEngUserEvent&DayOfWeek=4&DeviceName=x86_64&HourOfDay=16&InstallDate=3%2F13%2F2019&InstallEvent=InstallEvent&LaunchEvent=LaunchEvent&Launches=1&MonthlyEngUserEvent=MonthlyEngUserEvent&OSVersion=iOS%2012.1&Resolution=750x1334&RunMode=Application&TimeSinceLaunch=1&internalaction=Lifecycle&locale=en-US&.a&.c&ce=UTF-8&cp=foreground&mid=67027929491180584128922600814231770586&pageName=BusBookingObjectiveC%201%20%281.0%29&pe=lnk_o&pev2=ADBINTERNAL%3ALifecycle&t=00%2F00%2F0000%2000%3A00%3A00%200%20240&ts=1552510406)
   ```

Félicitations, vous avez ajouté le SDK à une application mobile !

[Suivant : "Ajouter le service d’identité Adobe Experience Platform" &gt;](id-service.md)