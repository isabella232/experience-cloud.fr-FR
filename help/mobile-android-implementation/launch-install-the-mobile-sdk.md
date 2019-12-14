---
title: Installation du SDK Adobe Mobile dans une application Mobile Android
description: Découvrez comment obtenir les codes incorporés de votre propriété Launch et les implémenter dans votre site Web. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications mobiles Android.
seo-description: null
seo-title: Installation du SDK Adobe Mobile dans une application Mobile Android
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: d669338b0e298c44c1ef2ed1a4491b8451d45f7e

---


# Installation du SDK Mobile

Dans cette leçon, vous allez mettre en oeuvre le kit Mobile SDK avec les extensions et les paramètres correspondant à l’environnement de développement de la propriété Launch.

## Conditions préalables 

Dans cette leçon, nous allons commencer à ajouter du code à l'application de réservation de bus, donc si vous ne l'avez pas déjà fait :

1. Téléchargement et installation d’ [Android Studio](https://developer.android.com/studio)
1. Télécharger l'application de réservation de [bus](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Suivez les instructions d’installation de la propriété Lancement mobile.
* Comprendre la différence entre environnement de développement, d’évaluation et de production
* Mettez à jour votre `build.gradle` fichier pour ajouter Mobile SDK
* Importation du kit Mobile SDK dans votre application
* Vérifiez que le SDK a bien été implémenté
* Activation des mesures de cycle de vie dans l’application

## Obtention des instructions d’installation

Les instructions d’installation pour le lancement mobile sont une collection de fragments de code que vous ajoutez à des emplacements spécifiques de votre application mobile.

Cliquez sur l’ `Environments` onglet de la navigation supérieure pour accéder à la page des environnements. Notez que les environnements de développement, de test et de production ont été précréés pour vous. Elles correspondent aux environnements typiques du processus de développement et de publication du code. Le code est d’abord écrit par un développeur dans un environnement de développement. Lorsqu’ils ont terminé leur travail, les développeurs envoient le code à un environnement d’évaluation où il est révisé par des équipes d’assurance qualité et d’autres équipes. Une fois l’assurance qualité et les autres équipes satisfaites, le code est ensuite publié dans l’environnement Production, qui est l’environnement public que vos visiteurs voient lorsqu’ils téléchargent votre application.

Le lancement permet d’autres environnements de développement, ce qui est utile dans les grandes entreprises où plusieurs développeurs travaillent simultanément sur différents projets.

Le développement, le test et la production sont les seuls environnements nécessaires pour compléter le didacticiel.

![Cliquez sur Environnements dans la barre de navigation supérieure.](images/mobile-launch-environments.png)

Dans la ligne **[!UICONTROL Développement]** , cliquez sur l’icône ![](images/mobile-launch-installIcon.png) Installer pour ouvrir le mode de code incorporé.

![Cliquez sur l’icône pour ouvrir le mode de code incorporé](images/mobile-launch-openEmbedCode.png)

Passons en revue les instructions étape par étape.

## Mise à jour du fichier build.gradle

Si vous avez déjà utilisé Launch dans des sites Web, l’une des premières choses que vous remarquerez est qu’il y a beaucoup plus d’instructions d’installation pour les applications mobiles que pour les sites Web.

Le SDK mobile Adobe pour Android utilise Gradle pour gérer les dépendances entre ses différents composants. L’une des premières choses que nous ferons est d’ajouter les dépendances du SDK mobile Adobe au fichier build.gradle de l’application de réservation de bus.

**Pour mettre à jour le fichier build.gradle**

1. Ouvrir Android Studio
1. Sélectionnez "Ouvrir un projet Android Studio existant".

   ![Sélectionnez "Ouvrir un projet Android Studio existant".](images/android/mobile-launch-install-openProject.png)

1. Ouvrez le fichier build.gradle à la racine du dossier Android de réservation de bus :

   ![Ouvrez le fichier build.gradle à la racine du dossier Android de réservation de bus.](images/android/mobile-launch-install-openApp.png)

1. Ouvrez la liste déroulante et passez à la vue Projet.

   ![Sélectionnez "Projet" dans la liste déroulante.](images/android/mobile-launch-install-openProjectView.png)

1. Ouvrez le fichier **Android &gt; bus &gt; build.gradle** .

   ![Ouvrez Android &gt; bus &gt; build.gradel](images/android/mobile-launch-install-openGradle.png)

1. Dans l’interface de lancement, assurez-vous que le système d’exploitation est défini sur `Android`

1. Copiez les dépendances dans le Presse-papiers en cliquant sur l’icône ![Copier](images/mobile-launch-copyIcon.png) .

   ![Copier les dépendances dans le Presse-papiers dans l’interface de lancement](images/android/mobile-launch-install-copyDependencies.png)

1. Dans Android Studio, collez les dépendances de votre presse-papiers juste après les dépendances existantes (mais avant la fermeture `}`).
1. De plus, si vous installez l’extension VEC Adobe Target, vous devez également ajouter les dépendances suivantes :

   ```java
   implementation 'com.google.code.gson:gson:2.8.2'
   implementation 'android.arch.lifecycle:extensions:1.1.1'
   implementation 'io.github.sac:SocketclusterClientJava:1.7.5'
   implementation 'com.android.support:support-annotations:28.0.0'
   implementation 'com.android.support:support-compat:28.0.0'
   implementation 'com.android.support:design:28.0.0'
   ```

1. Cliquez sur le lien Synchroniser maintenant pour synchroniser le projet.

   ![Coller les dépendances dans pour build.gradle](images/android/mobile-launch-install-pasteDependencies.png)

## Mise à jour de l’application

Il est maintenant temps de mettre à jour l’application pour importer le SDK

**Pour importer le SDK**

1. Ouvrez le fichier Application principale dans l’application de réservation de bus, qui se trouve sous **Android &gt; bus &gt; src &gt; main &gt; java &gt; com.adobe.busreservation &gt; DemoApplication**

   ![Ouvrir DemoApplication](images/android/mobile-launch-install-openDemoApplication.png)

1. Dans l’interface de lancement, accédez à la section **[!UICONTROL Ajouter un code]** d’initialisation.
1. Copiez les instructions d’importation en cliquant sur la première icône ![Copier](images/mobile-launch-copyIcon.png) de la section **[!UICONTROL Ajouter un code]** d’initialisation :

   ![Copier les instructions d’importation dans le Presse-papiers](images/android/mobile-launch-install-copyImports.png)

1. Dans Android Studio, collez ces instructions d’importation *avant* les importations existantes dans le `DemoApplication` fichier. Notez que l’extension Core inclut des bibliothèques pour prendre en charge les rappels, le service d’identité, les mesures de cycle de vie et la journalisation de la console, entre autres fonctionnalités.

   ![Collez les instructions d’importation dans votre fichier DemoApplication.](images/android/mobile-launch-install-pasteImports.png)

1. Dans l’interface de lancement, copiez les deux lignes liées à l’extension Core en cliquant sur la deuxième icône ![Copier](images/mobile-launch-copyIcon.png) dans la section **[!UICONTROL Ajouter un code]** d’initialisation. La deuxième ligne active les instructions de journalisation de la console (les options disponibles sont "DEBUG", "VERBOSE", "WARNING" et "ERROR").

   ![Copiez les instructions Core dans le Presse-papiers](images/android/mobile-launch-install-copyCore.png)

1. Dans Android Studio, collez ces instructions Core dans le `DemoApplication` fichier juste après `super.onCreate()`
1. Supprimez les `//` commentaires avant les `try` et les `catch` lignes

   ![Collez les instructions Core dans votre fichier DemoApplication.](images/android/mobile-launch-install-pasteCore.png)

1. Dans l’interface de lancement, copiez les instructions d’extension en cliquant sur la troisième icône ![Copier](images/mobile-launch-copyIcon.png) de la section [!UICONTROL Ajouter un code] d’initialisation.

   ![Copier les instructions Extension dans le Presse-papiers](images/android/mobile-launch-install-copyExtensions.png)

1. Dans Android Studio, collez ces instructions d’extension dans la `try` section. Notez que `MobileCore.configureWithAppID` contient l’identifiant de l’environnement de développement Launch de votre propriété. C’est important, car vous devrez mettre à jour cette valeur lorsque nous serons prêts à déployer l’application dans l’environnement de production.

   ![Collez les instructions Extension dans votre fichier DemoApplication](images/android/mobile-launch-install-pasteExtensions.png)

>[!NOTE] Les instructions d’installation de Mobile fournies dans l’interface de lancement incluent les instructions d’importation et d’enregistrement pour les extensions d’identité, de cycle de vie et de signal, ainsi que l’initialisation des mesures de cycle de vie. Ces extensions sont considérées comme faisant partie de l’extension Mobile Core. Si vous ne souhaitez pas utiliser ces extensions dans votre application, vous n’avez pas besoin d’importer, d’enregistrer ou de mettre en oeuvre un autre code associé à ces extensions.
>
> D’autres options d’implémentation doivent également être prises en compte lors de l’utilisation de ces extensions (par exemple, vous pouvez suspendre/redémarrer la collection Lifecycle lorsque l’utilisateur met l’application en arrière-plan/en avant-plan). Pour en savoir plus, consultez [la documentation de l’extension Mobile Core.](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core)

## Vérification de l’implémentation

1. Enregistrement de votre projet Android Studio
1. Exécutez l’application et lancez-la dans l’émulateur. Si aucun périphérique d’émulateur n’est configuré, configurez-en un maintenant, en veillant à configurer un périphérique exécutant Android 4.1 (API 16) ou une version ultérieure.

   ![Exécutez l’application et lancez-la dans l’émulateur.](images/android/mobile-launch-install-buildAndLaunch.png)

1. Patientez jusqu’à ce que l’émulateur démarre et ouvre complètement l’application sur l’écran de réservation (cela peut prendre quelques minutes).

   ![Attendez que l’application s’ouvre complètement.](images/android/mobile-launch-install-simulator.png)

1. Confirmez que des appels sont envoyés aux serveurs Adobe dans le journal Android Studio Logcat.

   ![Attendez que l’application s’ouvre complètement.](images/android/mobile-launch-install-console.png)

Voici quelques exemples d’appels spécifiques que vous pouvez rechercher :

1. **Appels pour récupérer la configuration** de lancement (filtrer Logcat à `adobedtm.com`). Notez les configurations d’extension que vous avez saisies dans la leçon précédente. Bien que l’ajout de l’extension nécessite une mise à jour de l’application, ces paramètres peuvent être gérés en externe au lancement et modifiés à tout moment :

   ```java
   03-14 16:30:29.484 24869-24930/com.adobe.busbooking D/ADBMobile: ConfigurationExtension - Cached configuration loaded.
    {"target.propertyToken":"","target.timeout":5,"global.privacy":"optedin","analytics.backdatePreviousSessionInfo":true,"analytics.offlineEnabled":true,"build.environment":"dev","rules.url":"https://assets.adobedtm.com/launch-EN360aefc739b04410816f751a95861744-development-rules.zip","experienceCloud.org":"7ABB3E6A5A7491460A495D61@AdobeOrg","target.clientCode":"techmarketingdemos","target.autoFetch":true,"target.fetchBackground":false,"lifecycle.sessionTimeout":300,"target.environmentId":"busbookingapp","analytics.server":"tmd.sc.omtrdc.net","analytics.rsids":"tmd-mobile-dev1","analytics.batchLimit":0,"property.id":"PRb4881271498b4f2cbaf67d38a8f3891a","global.ssl":true,"analytics.aamForwardingEnabled":true}
   ```

1. **Demande au service** d’identité (filtrer Logcat pour `IdentityExtension`) Dans cet exemple, l’ID (`d_mid`) a déjà été défini et est simplement de nouveau signalé)

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **Requête** Analytics (filtrer Logcat `AnalyticsExtension`)

   ```java
   03-14 17:01:18.509 7743-7777/com.adobe.busbooking D/ADBMobile: AnalyticsExtension - Sending Analytics ID call (https://tmd.sc.omtrdc.net/id?mcorgid=7ABB3E6A5A7491460A495D61%40AdobeOrg&mid=59651426340521082405908216148091920022)
   ```

Félicitations, vous avez ajouté le SDK à une application mobile !

## Activation des mesures de cycle de vie dans l’application

Les mesures de cycle de vie sont des mesures et des dimensions basées sur l’environnement qui peuvent être facilement activées dans une application à l’aide du SDK Experience Platform Mobile. Puisqu’elles peuvent être utilisées par plusieurs solutions Experience Cloud, nous les activerons ici, avant d’approfondir les différentes solutions. C'est aussi simple que d'ajouter quelques lignes de code à notre application au bon endroit.

### Importer la bibliothèque principale dans le fichier BusBookingActivity

Pour effectuer des appels d’API via le SDK mobile d’Adobe Experience Platform, vous devez importer les bibliothèques dans les fichiers appropriés. Dans ce cas, pour utiliser l’appel de l’API Lifecycle, nous devons importer la bibliothèque principale.

1. Lorsque votre application est ouverte dans Android Studio, ouvrez le fichier BusBookingActivity, qui se trouve dans le même répertoire que le fichier DemoApplication dans lequel vous avez travaillé.
1. Dans la partie supérieure du fichier, ajoutez l’instruction d’importation MobileCore suivante afin que vous puissiez utiliser les appels d’API associés.
   `import com.adobe.marketing.mobile.MobileCore;`

![Importation de la bibliothèque principale mobile](images/android/mobile-launch-install-importMobileCore.png)

### Ajout du code de cycle de vie

Vous allez maintenant ajouter le code de cycle de vie à la fonction principale onResume() de l’application, afin de déclencher les fonctions de cycle de vie.

1. Ouverture du fichier BusBookingActivity
1. Faites défiler le fichier vers le bas et recherchez la fonction onResume()
1. Ajoutez les deux lignes de code suivantes sous la `super.onResume()` ligne :

   ```java
    MobileCore.setApplication(getApplication());
    MobileCore.lifecycleStart(null);
   ```

![Insérer un code de cycle de vie](images/android/mobile-launch-install-lifecycle.png)

### Validation de l’accès au cycle de vie

Lorsque vous exécutez votre application, vous devez maintenant recevoir un ou plusieurs messages du cycle de vie dans la section de débogage d’Android Studio.

1. Exécutez une compilation, puis choisissez un simulateur pour exécuter l’application.
1. Une fois le simulateur en cours d’exécution, cliquez sur la section "Exécuter" du débogueur dans Android Studio.
1. Rechercher `internalaction=Lifecycle`
1. Notez qu’il existe des lignes qui incluent cette paire clé/valeur, ainsi que les autres mesures de cycle de vie.

Notez que les lignes que vous verrez sont en fait des appels Analytics avec des mesures de cycle de vie.

![Valider le cycle de vie](images/android/mobile-launch-install-validateLifecycle.png)

[Suivant : "Ajouter le service d’identité Adobe Experience Platform" &gt;](id-service.md)