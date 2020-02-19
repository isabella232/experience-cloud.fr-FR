---
title: Installation du SDK Adobe Mobile dans une application mobile pour Android
description: Découvrez comment obtenir les codes intégrés de votre propriété Launch et comment les mettre en œuvre dans votre site web. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles pour Android.
seo-description: null
seo-title: Installation du SDK Adobe Mobile dans une application mobile pour Android
solution: Experience Cloud
translation-type: ht
source-git-commit: d669338b0e298c44c1ef2ed1a4491b8451d45f7e

---


# Installation du SDK Mobile

Dans cette leçon, vous allez mettre en œuvre le SDK Mobile avec les extensions et les paramètres correspondant à l’environnement de développement de la propriété Launch.

## Conditions préalables

Dans cette leçon, nous allons commencer à ajouter du code à l’application Bus Booking, donc si cela n’est pas déjà fait, vous devez :

1. Télécharger et installer [Android Studio](https://developer.android.com/studio)
1. Télécharger l’application [Bus Booking](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* obtenir les instructions d’installation de votre propriété Launch mobile ;
* différencier environnement de développement, d’évaluation et de production ;
* mettre à jour votre fichier `build.gradle` pour ajouter le SDK Mobile ;
* importer le SDK Mobile dans votre application ;
* vérifier que le SDK a bien été mis en œuvre ;
* activer les mesures de cycle de vie dans l’application.

## Obtention des instructions d’installation

Les instructions d’installation des propriétés Launch mobiles sont une collection d’extraits de code que vous ajoutez à des emplacements spécifiques de votre application mobile.

Cliquez sur l’onglet `Environments` de la barre de navigation supérieure pour accéder à la page des environnements. Notez que les environnements de développement, d’évaluation et de production ont déjà été créés pour vous. Ils correspondent aux environnements typiques du processus de développement et de mise à jour du code. Le code est d’abord écrit par un développeur dans un environnement de développement. Lorsqu’ils ont terminé leur travail, les développeurs envoient le code à un environnement d’évaluation où il est révisé par des équipes d’assurance qualité et d’autres équipes. Une fois que les équipes d’assurance qualité et les autres équipes sont satisfaites, le code est publié dans l’environnement de production, qui est l’environnement que voient les visiteurs lorsqu’ils téléchargent votre application.

Launch permet de créer des environnements de développement supplémentaires, ce qui est utile aux grandes équipes où plusieurs développeurs travaillent sur différents projets simultanément.

Les environnements de développement, d’évaluation et de production sont les seuls environnements nécessaires pour compléter le tutoriel.

![Clic sur Environnements dans la barre de navigation supérieure](images/mobile-launch-environments.png)

Sur la ligne **[!UICONTROL Développement]**, cliquez sur l’icône Installer ![icône Installer](images/mobile-launch-installIcon.png) pour ouvrir le modal de code intégré.

![Clic sur l’icône pour ouvrir le modal de code intégré](images/mobile-launch-openEmbedCode.png)

Passons en revue les instructions étape par étape.

## Mise à jour du fichier build.gradle

Si vous avez déjà utilisé Launch pour des sites web, l’une des premières choses que vous remarquez est qu’il y a beaucoup plus d’instructions d’installation pour les applications mobiles que pour les sites web.

Le SDK Adobe Mobile pour Android utilise Gradle pour gérer les dépendances entre ses divers composants. L’une des premières choses à faire est d’ajouter les dépendances du SDK Adobe Mobile au fichier build.gradle de l’application Bus Booking.

**Mise à jour du fichier build.gradle**

1. Ouvrez Android Studio
1. Sélectionnez « Ouvrir un projet Android Studio existant »

   ![Sélectionnez « Ouvrir un projet Android Studio existant »](images/android/mobile-launch-install-openProject.png)

1. Ouvrez le fichier build.gradle à la racine du dossier Android Bus Booking :

   ![Ouvrez le fichier build.gradle à la racine du dossier Android Bus Booking](images/android/mobile-launch-install-openApp.png)

1. Sélectionnez la vue Projet dans la liste déroulante

   ![Sélectionnez la vue « Projet » dans la liste déroulante](images/android/mobile-launch-install-openProjectView.png)

1. Ouvrez le fichier **Android &gt; bus &gt; build.gradle**

   ![Ouvrez Android &gt; bus &gt; build.gradle](images/android/mobile-launch-install-openGradle.png)

1. Dans l’interface Launch, assurez-vous que le système d’exploitation est défini sur `Android`

1. Copiez les dépendances dans le presse-papiers en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png)

   ![Copie des dépendances dans le presse-papiers dans l’interface Launch](images/android/mobile-launch-install-copyDependencies.png)

1. Dans Android Studio, collez les dépendances de votre presse-papiers juste après les dépendances existantes (mais avant la `}` fermante)
1. Si vous installez l’extension Adobe Target VEC, vous devez aussi ajouter les dépendances suivantes :

   ```java
   implementation 'com.google.code.gson:gson:2.8.2'
   implementation 'android.arch.lifecycle:extensions:1.1.1'
   implementation 'io.github.sac:SocketclusterClientJava:1.7.5'
   implementation 'com.android.support:support-annotations:28.0.0'
   implementation 'com.android.support:support-compat:28.0.0'
   implementation 'com.android.support:design:28.0.0'
   ```

1. Cliquez sur le lien « Synchroniser maintenant » pour synchroniser le projet

   ![Collez les dépendances dans build.gradle](images/android/mobile-launch-install-pasteDependencies.png)

## Mise à jour de l’application

Il est maintenant temps de mettre à jour l’application pour importer le SDK.

**Importation du SDK**

1. Ouvrez le principal fichier de l’application Bus Booking situé sous **Android &gt; bus &gt; src &gt; main &gt; java &gt; com.adobe.busbooking &gt; DemoApplication**

   ![Ouvrez DemoApplication](images/android/mobile-launch-install-openDemoApplication.png)

1. Dans l’interface Launch, faites défiler l’écran jusqu’à la section **[!UICONTROL Ajouter le code d’initialisation]**.
1. Copiez les instructions d’importation en cliquant sur la première icône ![Copier](images/mobile-launch-copyIcon.png) dans la section **[!UICONTROL Ajouter le code d’initialisation]** :

   ![Copie des instructions d’importation dans votre presse-papiers](images/android/mobile-launch-install-copyImports.png)

1. Dans Android Studio, collez ces instructions d’importation *avant* les importations existantes dans le fichier `DemoApplication`. Notez que l’extension Core inclut des bibliothèques pour prendre en charge les rappels, Identity Service, les mesures de cycle de vie et la connexion de la console, entre autres fonctionnalités.

   ![Collez les instructions d’importation dans le fichier DemoApplication](images/android/mobile-launch-install-pasteImports.png)

1. Dans l’interface de Launch, copiez les deux lignes liées à l’extension Core en cliquant sur la deuxième icône ![Copier](images/mobile-launch-copyIcon.png) dans la section **[!UICONTROL Ajouter un code d’initialisation]**. La deuxième ligne active les instructions de connexion de la console (les options disponibles sont « DEBUG », « VERBOSE », « WARNING » et « ERROR »).

   ![Copie des instructions Core dans le presse-papiers](images/android/mobile-launch-install-copyCore.png)

1. Dans Android Studio, collez ces instructions Core dans le fichier `DemoApplication`, juste après `super.onCreate()`
1. Supprimez les commentaires `//` avant les lignes `try` et `catch`

   ![Collez les instructions Core dans le fichier DemoApplication](images/android/mobile-launch-install-pasteCore.png)

1. Dans l’interface Launch, copiez les instructions d’extension en cliquant sur la troisième icône ![Copier](images/mobile-launch-copyIcon.png) de la section [!UICONTROL Ajouter le code d’initialisation].

   ![Copie des instructions d’extension dans le presse-papiers](images/android/mobile-launch-install-copyExtensions.png)

1. Dans Android Studio, collez ces instructions d’extension dans la section `try`. Notez que `MobileCore.configureWithAppID` contient l’identifiant de l’environnement de développement Launch de votre propriété. C’est important, car vous devrez mettre à jour cette valeur lorsque nous serons prêts à déployer l’application dans l’environnement de production.

   ![Collez les instructions d’extension dans votre fichier DemoApplication](images/android/mobile-launch-install-pasteExtensions.png)

>[!NOTE] Les instructions d’installation pour mobile fournies dans l’interface de Launch incluent les instructions d’importation et d’enregistrement pour les extensions d’identité, de cycle de vie et de signal, ainsi que l’initialisation des mesures de cycle de vie. Ces extensions sont considérées comme partie intégrante de l’extension Mobile Core. Si vous ne souhaitez pas utiliser ces extensions dans votre application, il est inutile d’importer, d’enregistrer ou de mettre en œuvre un autre code associé à ces extensions.
>
> D’autres options de mise en œuvre doivent aussi être prises en compte lors de l’utilisation de ces extensions (par exemple, vous pouvez mettre en pause/redémarrer la collection du cycle de vie lorsque l’utilisateur met l’application en arrière-plan/au premier plan). Pour en savoir plus, consultez [la documentation de l’extension Mobile Core](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)

## Vérification de la mise en œuvre

1. Enregistrez le projet Android Studio
1. Exécutez l’application et ouvrez-la dans l’émulateur. Si aucun terminal d’émulateur n’est configuré sur votre appareil, configurez-en un maintenant en vous assurant qu’il exécute Android 4.1 (API 16) ou une version ultérieure.

   ![Exécution de l’application et son lancement dans l’émulateur](images/android/mobile-launch-install-buildAndLaunch.png)

1. Attendez que l’émulateur démarre et ouvre complètement l’application et que l’écran Bus Booking s’affiche (cela peut prendre plusieurs minutes)

   ![Attendez que l’application s’ouvre complètement](images/android/mobile-launch-install-simulator.png)

1. Vérifiez que des appels sont envoyés aux serveurs Adobe dans l’Android Studio Logcat

   ![Attendez que l’application s’ouvre complètement](images/android/mobile-launch-install-console.png)

Voici quelques exemples d’appels spécifiques que vous pouvez rechercher :

1. **Appels pour récupérer la configuration Launch** (filtrer Logcat sur `adobedtm.com`). Notez les configurations d’extension que vous avez saisies dans la leçon précédente. Bien que l’ajout de l’extension nécessite une mise à jour de l’application, ces paramètres peuvent être gérés en externe dans Launch et modifiés à tout moment :

   ```java
   03-14 16:30:29.484 24869-24930/com.adobe.busbooking D/ADBMobile: ConfigurationExtension - Cached configuration loaded.
    {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.clientCode":"techmarketingdemos","target.autoFetch":true,"target.fetchBackground":false,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **Requête à Identity Service** (filtrer Logcat sur `IdentityExtension`) Dans cet exemple, l’ID (`d_mid`), déjà défini, est juste signalé de nouveau)

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **Requête Analytics** (filtrer Logcat sur `AnalyticsExtension`)

   ```java
   03-14 17:01:18.509 7743-7777/com.adobe.busbooking D/ADBMobile: AnalyticsExtension - Sending Analytics ID call (https://tmd.sc.omtrdc.net/id?mcorgid=7ABB3E6A5A7491460A495D61%40AdobeOrg&mid=59651426340521082405908216148091920022)
   ```

Félicitations, vous avez ajouté le SDK à une application mobile !

## Activation des mesures de cycle de vie dans l’application

Les mesures de cycle de vie sont des mesures et des dimensions basées sur l’environnement qui peuvent être facilement activées dans une application à l’aide du SDK Mobile Experience Platform. Comme elles peuvent être utilisées par plusieurs solutions Experience Cloud, nous les activerons à cette étape avant d’aborder plus en détail chacune des solutions. C’est aussi simple que d’ajouter quelques lignes de code au bon endroit dans l’application.

### Importation de la bibliothèque Core dans le fichier BusBookingActivity

Pour effectuer des appels d’API via le SDK Mobile Adobe Experience Platform, vous devez importer les bibliothèques dans les fichiers appropriés. Dans ce cas, nous devons importer la bibliothèque Core pour utiliser l’appel d’API Lifecycle.

1. Ouvrez votre application dans Android Studio, puis ouvrez le fichier BusBookingActivity, qui se trouve dans le même répertoire que le fichier DemoApplication dans lequel vous avez travaillé.
1. Dans la partie supérieure du fichier, ajoutez l’instruction d’importation MobileCore suivante afin de pouvoir utiliser les appels d’API associés
   `import com.adobe.marketing.mobile.MobileCore;`

![Importez la bibliothèque Mobile Core](images/android/mobile-launch-install-importMobileCore.png)

### Ajout du code Lifecycle

Vous allez maintenant ajouter le code Lifecycle à la fonction principale onResume() de l’application afin de déclencher les fonctions Lifecycle.

1. Ouvrez le fichier BusBookingActivity
1. Faites défiler le fichier vers le bas jusqu’à trouver la fonction onResume()
1. Ajoutez les deux lignes de code suivantes sous la ligne `super.onResume()` :

   ```java
    MobileCore.setApplication(getApplication());
    MobileCore.lifecycleStart(null);
   ```

![Insérez le code Lifecycle](images/android/mobile-launch-install-lifecycle.png)

### Validation de l’accès à Lifecycle

Lorsque vous exécutez l’application, vous devriez recevoir un ou plusieurs messages de Lifecycle dans la section de débogage d’Android Studio.

1. Exécutez une version, puis choisissez un simulateur pour exécuter l’application
1. Une fois le simulateur en cours d’exécution, cliquez sur la section « Exécuter » du débogueur dans Android Studio
1. Lancez une recherche pour `internalaction=Lifecycle`
1. Vous remarquez qu’il existe des lignes qui incluent cette paire clé/valeur ainsi que les autres mesures de cycle de vie.

Notez que les lignes que vous voyez sont en fait des appels Analytics avec des mesures de cycles de vie.

![Validez Lifecycle](images/android/mobile-launch-install-validateLifecycle.png)

[Suite : « Ajout d’Adobe Experience Platform Identity Service » &gt;](id-service.md)