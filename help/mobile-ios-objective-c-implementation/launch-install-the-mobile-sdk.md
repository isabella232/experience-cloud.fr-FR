---
title: Installation du SDK Mobile Adobe dans une application mobile Objective-C pour iOS
description: Découvrez comment obtenir les codes intégrés de votre propriété Launch et comment les mettre en œuvre dans votre site web. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS.
seo-description: null
seo-title: Installation du SDK Mobile Adobe dans une application mobile Objective-C pour iOS
solution: Experience Cloud
translation-type: ht
source-git-commit: bd91e0e5f11b94f9a343ab60e7d4e6e25e181f9d

---


# Installation du SDK Mobile

Dans cette leçon, vous allez mettre en œuvre le SDK Mobile avec les extensions et les paramètres correspondant à l’environnement de développement de la propriété Launch.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* obtenir les instructions d’installation de votre propriété Launch mobile ;
* différencier environnement de développement, d’évaluation et de production ;
* créer et modifier le fichier de capsule ;
* importer le SDK Mobile dans votre fichier AppDelegate ;
* vérifier que le SDK a bien été mis en œuvre.

## Obtention des instructions d’installation

Les instructions d’installation pour les propriétés mobiles de Launch sont une collection de fragments de code que vous exécutez dans votre terminal ou que vous ajoutez à des emplacements spécifiques dans votre application mobile.

Cliquez sur l’onglet `Environments` dans la barre de navigation supérieure pour accéder à la page des environnements. Notez que les environnements de développement, d’évaluation et de production ont déjà été créés pour vous. Ils correspondent aux environnements typiques du processus de développement et de mise à jour du code. Le code est d’abord écrit par un développeur dans un environnement de développement. Lorsqu’ils ont terminé leur travail, les développeurs envoient le code à un environnement d’évaluation où il est révisé par des équipes d’assurance qualité et d’autres équipes. Une fois que les équipes d’assurance qualité et les autres équipes sont satisfaites, le code est publié dans l’environnement de production, qui est l’environnement que voient les visiteurs lorsqu’ils téléchargent votre application.

Launch permet de créer des environnements de développement supplémentaires, ce qui est utile aux grandes équipes où plusieurs développeurs travaillent sur différents projets simultanément.

Les environnements de développement, d’évaluation et de production sont les seuls environnements nécessaires pour compléter le tutoriel.

![Clic sur Environnements dans la barre de navigation supérieure](images/mobile-launch-environments.png)

Sur la ligne **[!UICONTROL Développement]**, cliquez sur l’icône Installer ![icône Installer](images/mobile-launch-installIcon.png) pour ouvrir le modal de code intégré.

![Clic sur l’icône pour ouvrir le modal de code intégré](images/mobile-launch-openEmbedCode.png)

Passons en revue les instructions étape par étape.

## Création du fichier de capsule et installation des capsules

Si vous avez déjà utilisé Launch dans des sites web, l’une des premières choses que vous remarquerez est qu’il y a beaucoup plus d’informations dans ce modal que pour les propriétés web.

Le SDK Mobile Adobe pour iOS utilise les CocoaPods pour gérer les dépendances entre ses différents composants. Si vous n’avez pas encore installé [CocoaPods](https://cocoapods.org/) dans votre environnement de développement, suivez les instructions d’installation de leur site web. En outre, si vous n’avez pas encore téléchargé l’application [Bus Booking](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps), enregistrez-la sur votre ordinateur en local et extrayez l’archive zip sur votre bureau.

**Création du fichier de capsule**

1. Ouvrez l’application `Terminal` sur votre Mac®.

1. Accédez au dossier du projet dans lequel vous avez enregistré l’application Objectif-C Bus Booking (par ex. `cd Desktop/busbooking-mobileapps-master/Objective-C/`).

   ![Accès au répertoire du projet](images/ios/objective-c/mobile-launch-install-goToProjectDirectory.png)

1. Dans l’interface de Launch, modifiez le système d’exploitation pour `iOS`.

1. Copiez la première instruction iOS `pod init`, en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png).

   ![Copie de la capsule init dans le presse-papiers dans l’interface de Launch](images/ios/mobile-launch-install-copyPodInit.png)

1. Dans votre application Terminal, exécutez la commande `pod init` et attendez qu’elle soit terminée.

   ![Exécution de la capsule init](images/ios/objective-c/mobile-launch-install-runPodInit.png)

1. Dans votre application Terminal, ouvrez le fichier de capsule à l’aide de la commande `open podfile`.

   ![Exécution d’un fichier de capsule ouvert](images/ios/objective-c/mobile-launch-install-openPodfile.png)

1. Votre ordinateur peut ouvrir une boîte de dialogue vous demandant avec quelle application vous souhaitez ouvrir le fichier de capsule. Choisissez n’importe quel éditeur de texte, par exemple `TextEdit`.

1. Dans l’interface de Launch, copiez la liste des dépendances en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png). Vous observerez qu’il y a une ligne pour chacune des extensions que vous avez ajoutées dans la leçon précédente. Chaque extension possède son propre jeu de code qui repose sur l’extension Mobile Core et ne peut être ajouté ou supprimé qu’avec une mise à jour de l’application :

   ![Copie des dépendances dans le presse-papiers dans l’interface de Launch](images/ios/mobile-launch-install-copyDependencies.png)

1. Dans l’éditeur de texte, collez les dépendances depuis le presse-papiers juste après la ligne `# Pods for BusBookingObjectiveC`.

1. Enregistrez les mises à jour dans le fichier de capsule dans votre éditeur de texte.

   ![Ajout de dépendances et enregistrement](images/ios/objective-c/mobile-launch-install-addDependenciesAndSave.png)

1. Vous pouvez désormais fermer votre éditeur de texte.

1. Dans l’interface de Launch, copiez l’instruction iOS `pod repo update` suivante en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png).

   ![Copie de la mise à jour du référentiel de capsule](images/ios/mobile-launch-install-copyPodRepoUpdate.png)

1. Dans votre application Terminal, exécutez la commande `pod repo update` et attendez qu’elle soit terminée (cette opération peut durer plusieurs minutes).

   ![Exécution de la mise à jour du référentiel de capsule](images/ios/objective-c/mobile-launch-install-podRepoUpdate.png)

1. Dans l’interface de Launch, copiez l’instruction iOS `pod install` suivante en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png).

   ![Copie de l’installation de la capsule dans le presse-papiers dans l’interface de Launch](images/ios/mobile-launch-install-copyPodInstall.png)

1. Dans votre application Terminal, exécutez la commande `pod install` et attendez qu’elle soit terminée.

   ![Exécution de l’installation de la capsule](images/ios/objective-c/mobile-launch-install-podInstall.png)

1. Vous pouvez désormais fermer la fenêtre Terminal.

1. Ouvrez une fenêtre Finder, accédez au dossier dans lequel vous avez enregistré l’application Bus Booking, puis vérifiez que le fichier BusBookingObjectiveC.xcworkspace, le fichier de capsule, le fichier Podfile.lock et le dossier Pods ont été créés.

   ![Confirmation des capsules dans l’outil de recherche](images/ios/objective-c/mobile-launch-install-podsInFinder.png)

## Mise à jour d’AppDelegate

Il est maintenant temps de mettre à jour l’application pour importer le SDK :

1. Ouvrir le fichier `BusBookingObjectiveC.xcworkspace` dans Xcode.
1. Ouvrez le fichier `AppDelegate.m`.

   ![Ouverture du fichier AppDelegate](images/ios/objective-c/mobile-launch-install-openAppDelegate.png)

1. Dans l’interface de Launch, faites défiler l’écran jusqu’à la section **[!UICONTROL Ajouter un code d’initialisation]**, puis sélectionnez **[!UICONTROL Objective C]** comme langage iOS.
1. Copiez les instructions d’importation en cliquant sur la première icône ![Copier](images/mobile-launch-copyIcon.png) de la section **[!UICONTROL Ajouter un code d’initialisation]** :

   ![Copie des instructions d’importation dans votre presse-papiers](images/ios/objective-c/mobile-launch-install-copyImports.png)

1. Dans Xcode, collez ces instructions d’importation dans le fichier `AppDelegate.m` après l’importation pour le `AppDelegate.h`.

   ![Collage des instructions d’importation dans le fichier AppDelegate](images/ios/objective-c/mobile-launch-install-pasteImports.png)

1. Dans l’interface de Launch, copiez les deux lignes liées à l’extension Core en cliquant sur la deuxième icône ![Copier](images/mobile-launch-copyIcon.png) dans la section **[!UICONTROL Ajouter un code d’initialisation]**. La première ligne active les instructions de journalisation de la console (les options disponibles sont « debug », « verbose », « warning » et « error »). La deuxième ligne pointe vers l’identifiant unique de l’environnement de Launch. Il est maintenant temps de mettre à jour l’application pour importer le SDK.

   ![Copie des instructions Core dans le presse-papiers](images/ios/objective-c/mobile-launch-install-copyCore.png)

1. Dans Xcode, collez les instructions Core suivantes dans le fichier AppDelegate au-dessus de la méthode `application(_:didFinishLaunchingWithOptions:)` :

   ![Collage des instructions Core dans le fichier AppDelegate](images/ios/objective-c/mobile-launch-install-pasteCore.png)

1. Dans l’interface de Launch, copiez les instructions d’extension en cliquant sur la troisième icône ![Copier](images/mobile-launch-copyIcon.png) de la section [!UICONTROL Ajouter un code d’initialisation] :

   ![Copie des instructions d’extension dans le presse-papiers](images/ios/objective-c/mobile-launch-install-copyExtensions.png)

1. Dans Xcode, collez ces instructions d’extension dans le fichier AppDelegate juste avant la ligne `return true` de la méthode `application(_:didFinishLaunchingWithOptions:)` :

   ![Collage des instructions d’extension dans le fichier AppDelegate](images/ios/objective-c/mobile-launch-install-pasteExtension.png)

>[!NOTE] Les instructions d’installation pour mobile fournies dans l’interface de Launch incluent les instructions d’importation et d’enregistrement pour les extensions d’identité, de cycle de vie et de signal, ainsi que l’initialisation des mesures de cycle de vie. Ces extensions sont considérées comme partie intégrante de l’extension Mobile Core. Si vous ne souhaitez pas utiliser ces extensions dans votre application, il est inutile d’importer, d’enregistrer ou de mettre en œuvre un autre code associé à ces extensions.
>
> D’autres options de mise en œuvre doivent aussi être prises en compte lors de l’utilisation de ces extensions (par exemple, vous pouvez mettre en pause/redémarrer la collection du cycle de vie lorsque l’utilisateur met l’application en arrière-plan/au premier plan). Pour en savoir plus, consultez [la documentation de l’extension Mobile Core](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core).

## Vérification de la mise en œuvre

1. Enregistrez votre projet Xcode.
1. Exécutez l’application et lancez-la dans le simulateur. Si aucun simulateur n’est configuré, configurez-en un maintenant, en veillant à configurer un appareil fonctionnant sous iOS 10+. Le simulateur de l’iPhone 8 fonctionne parfaitement, car il est facile de cliquer sur le bouton `Home` avec la souris.

   ![Exécution de l’application et son lancement dans l’émulateur](images/ios/objective-c/mobile-launch-install-buildAndLaunch.png)

1. Attendez que le simulateur se lance et ouvre complètement l’application sur l’écran de réservation (cette opération peut prendre quelques minutes).

   ![Attente de l’ouverture complète de l’application](images/ios/mobile-launch-install-simulator.png)

1. Confirmez que des appels sont envoyés aux serveurs Adobe dans la console Xcode.

   ![Recherche des appels dans la console](images/ios/objective-c/mobile-launch-install-console.png)

Voici quelques exemples d’appels spécifiques que vous pouvez rechercher :

1. **Appels pour récupérer la configuration de Launch** (filtrez votre console sur `adobedtm.com`). Notez les configurations d’extension que vous avez saisies dans la leçon précédente. Bien que l’ajout de l’extension nécessite une mise à jour de l’application, ces paramètres peuvent être gérés en externe dans Launch et modifiés à tout moment :

   ```objective-c
   2019-03-13 16:53:26.633816-0400 BusBookingObjectiveC[56630:3854917] [AMSDK DEBUG <RulesDownloader>]: Successfully downloaded Rules from 'https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip'
   
   {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.clientCode":"techmarketingdemos","target.autoFetch":true,"target.fetchBackground":false,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **Requête à Identity Service** (filtrez votre console sur `demdex.net`) Dans cet exemple, l’ID (`d_mid`) a déjà été défini et est simplement de nouveau signalé.

   ```objective-c
   2019-03-13 16:53:26.655908-0400 BusBookingObjectiveC[56630:3854937] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   Sending request (https://dpm.demdex.net/id?d_rtbd=json&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61@AdobeOrg&d_mid=67027929491180584128922600814231770586)
   ```

1. **Réponse d’Identity Service** (filtrez votre console sur `ID Service`). Notez comment la valeur `mid` correspond à la valeur `d_mid` dans la requête ci-dessus :

   ```objective-c
   2019-03-13 16:53:27.397048-0400 BusBookingObjectiveC[56630:3854937] [AMSDK DEBUG <com.adobe.module.identity>]:
   
   ID Service - Got ID Response (mid: 67027929491180584128922600814231770586, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: "604800000 ms")
   ```

1. **Requête Analytics** (filtrer votre console sur `Analytics request`)

   ```objective-c
   2019-03-13 16:53:27.689061-0400 BusBookingObjectiveC[56630:3855024] [AMSDK DEBUG <AnalyticsHitDatabase>]: Analytics request was sent with body
   
   (ndh=1&c.&a.&AppID=BusBookingObjectiveC%201%20%281.0%29&CarrierName=%28null%29&DailyEngUserEvent=DailyEngUserEvent&DayOfWeek=4&DeviceName=x86_64&HourOfDay=16&InstallDate=3%2F13%2F2019&InstallEvent=InstallEvent&LaunchEvent=LaunchEvent&Launches=1&MonthlyEngUserEvent=MonthlyEngUserEvent&OSVersion=iOS%2012.1&Resolution=750x1334&RunMode=Application&TimeSinceLaunch=1&internalaction=Lifecycle&locale=en-US&.a&.c&ce=UTF-8&cp=foreground&mid=67027929491180584128922600814231770586&pageName=BusBookingObjectiveC%201%20%281.0%29&pe=lnk_o&pev2=ADBINTERNAL%3ALifecycle&t=00%2F00%2F0000%2000%3A00%3A00%200%20240&ts=1552510406)
   ```

Félicitations, vous avez ajouté le SDK à une application mobile !

[Suite : « Ajout d’Adobe Experience Platform Identity Service » &gt;](id-service.md)