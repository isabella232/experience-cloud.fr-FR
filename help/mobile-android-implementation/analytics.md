---
title: Mise en œuvre d’Adobe Analytics avec Launch
description: Découvrez comment mettre en œuvre Adobe Analytics à l’aide de l’extension Launch d’Adobe Analytics, envoyer une balise d’affichage d’écran, ajouter des variables, suivre les événements et ajouter des plug-ins. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles pour Android.
seo-description: null
seo-title: Mise en œuvre d’Adobe Analytics avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout d’Adobe Analytics

Dans cette leçon, vous apprendrez à activer le suivi Adobe Analytics dans votre application.

[Adobe Analytics](https://docs.adobe.com/content/help/fr-FR/analytics/landing/home.html) est une solution de pointe qui vous permet de comprendre vos clients en tant que personnes et d’orienter votre activité grâce aux renseignements sur vos clients.

Dans les leçons [Ajout d’extensions](launch-add-extensions.md) et [Installation du SDK Mobile](launch-install-the-mobile-sdk.md), vous avez ajouté l’extension Adobe Analytics à la propriété Launch et l’avez importée dans l’exemple d’application. Il vous suffit maintenant d’ajouter du code pour suivre les états et actions de l’application.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* vérifier que les mesures de cycle de vie sont envoyées à Adobe Analytics ;
* ajouter du code pour effectuer le suivi des états dans votre application avec des données supplémentaires ;
* ajouter du code pour effectuer le suivi des actions dans votre application avec des données supplémentaires.

De nombreux éléments peuvent être mis en œuvre pour Analytics dans Launch. Cette leçon n’est pas exhaustive, mais elle devrait vous donner une vue d’ensemble adéquate des techniques principales dont vous aurez besoin pour effectuer la mise en œuvre sur votre propre application.

## Conditions préalables

Vous devez avoir terminé les leçons de la section [Configuration de Launch](launch-create-a-property.md). Dans cette section, vous avez ajouté l’extension Analytics et configuré votre serveur de suivi et vos identifiants de suite de rapports.

## Mesures de cycle de vie et Adobe Analytics

Les mesures de cycle de vie sont des mesures et des dimensions basées sur l’environnement qui peuvent être facilement activées dans une application à l’aide du SDK Mobile Experience Platform.

Vous avez déjà activé les mesures de cycle de vie lorsque vous avez ajouté l’extension Core à votre propriété et que vous avez suivi les instructions d’installation mobile fournies dans l’interface. Ces mesures et dimensions, parmi lesquelles les mesures propres à l’environnement et à l’application telles que la version de l’application, le nombre d’utilisateurs actifs, la version du système d’exploitation, la division du temps, le nombre de jours depuis la dernière utilisation, etc. peuvent s’avérer très utiles dans l’analyse de votre application, en particulier lorsque vous les utilisez pour créer des segments Analytics en vue de les appliquer à tous vos rapports. La liste complète des mesures est disponible dans la [documentation](https://docs.adobe.com/content/help/fr-FR/mobile-services/android/metrics.html).

## Importation de la bibliothèque ACPCore

Dans la leçon précédente intitulée [« Installation du SDK Mobile »](launch-install-the-mobile-sdk.md), vous avez ajouté une instruction d’importation afin de rendre la bibliothèque AdobeCore disponible dans le fichier BusBookingActivity. Cette même bibliothèque sera utilisée pour des appels API supplémentaires dans les activités de cette leçon. Dans les exercices suivants, vous utiliserez des API pour suivre des états (« trackState ») et des actions (« trackAction ») dans l’application, qui sont définis dans la bibliothèque AdobeCore. Dans le nouveau SDK Mobile Experience Cloud Platform, les API trackState et trackAction ont été déplacées de la bibliothèque Analytics vers la bibliothèque Core, ce qui permet d’exploiter ces API à des fins autres que le suivi d’Adobe Analytics.

## Suivi des états

Dans votre application, vous pouvez disposer de plusieurs écrans de contenu différents que vous fournissez à vos utilisateurs. Ils sont l’équivalent des pages d’un site web. Adobe Analytics propose une méthode permettant d’envoyer ces « accès aux pages consultées » et de les afficher dans les rapports auxquels vous êtes habitué pour vos propriétés web. Cette méthode est appelée « trackState ».

Dans ce tutoriel, vous allez placer le code d’un appel trackState dans un seul écran (page) de votre application. En pratique, vous allez répliquer cela sur tous les autres écrans/états de votre application.

Vous trouverez ci-dessous la syntaxe et un exemple de code de la documentation que vous pouvez copier-coller dans ce tutoriel ou dans votre propre application.

**Syntaxe :**

```java
public static void trackState(final String state, final Map<String, String> contextData)
```

**Exemple :**

```java
HashMap cData = new HashMap<String, String>();
contextData.put("key", "value");
MobileCore.trackState("state name",contextData);
```

### Suivi d’un état sans données

1. L’exemple d’application étant ouvert dans Android Studio, accédez à BusBookingActivity, puis faites défiler l’écran vers le bas jusqu’à la fonction onResume
1. Ajoutez un appel de méthode trackState.
1. Dans `state name`, indiquez « Booking Screen »
1. Au lieu d’ajouter des données, indiquez `null` comme espace réservé dans l’appel d’API
1. Vous pouvez aussi copier-coller les éléments suivants :

   ```java
   MobileCore.trackState("Booking Screen", null);
   ```

![Appel trackState de base](images/android/mobile-analytics-basicTrackStateCall2.png)

**Validation de trackState**

1. Enregistrez, compilez et exécutez le projet.
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console de débogage Logcat d’Android Studio
1. Dans la console, lancez une recherche pour `pageName=Booking%20Screen`
1. Notez que la variable pageName est définie sur `Booking Screen` (avec %20 comme espace codé) et qu’il n’existe aucune autre paire de données personnalisées. Bien que, techniquement, vous définissiez un « nom d’état » et pas un « nom de page », le nom du paramètre utilisé est `pageName` pour assurer la cohérence avec les mises en œuvre du site web.

   ![Résultat trackState de base](images/android/mobile-analytics-basicTrackStateResult.png)

### Suivi d’un état avec des données

1. Revenez dans BusBookingActivity et ajoutez une importation en haut du fichier `import java.util.HashMap;`, sous les importations existantes
1. Dans la fonction `onResume()`, placez en commentaire (ou supprimez) l’appel trackState de base du dernier exercice
1. Ajoutez un nouvel appel de méthode trackState, cette fois avec des données, en créant et en nommant un HashMap, en utilisant la commande « put » pour inclure certaines paires clé/valeur, puis en appelant ce HashMap dans l’appel à trackState
1. Laissez « Booking Screen » au niveau de `state name`
1. Ou effectuez un copier-coller dans :

   ```java
   HashMap cData = new HashMap<String, String>();
   cData.put("cd.section", "Bus Booking");
   cData.put("cd.subSection", "Booking");
   cData.put("cd.conversionType", "Landing");
   MobileCore.trackState("Booking Screen", cData);
   ```

   ![Appel trackState avec données](images/android/mobile-analytics-trackStateWithData.png)

**Validation de trackState avec des données**

1. Enregistrez, compilez et exécutez à nouveau le projet.
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console de débogage Logcat d’Android Studio
1. Recherchez `subSection` (ou l’une des clés ou valeurs que vous avez saisies dans le code)
1. Vous constatez maintenant qu’en plus de la variable pageName en cours de définition, les paires clé/valeur ont été envoyées sur le même accès

   ![trackState avec résultat de données](images/android/mobile-analytics-trackStateWithDataResult.png)

>[!NOTE] Si vous connaissez les « props et eVars » dans Analytics, vous constatez que ces noms de variable ne figurent pas dans le SDK. Toutes les données de clé/valeur provenant du SDK sont envoyées sous forme de [variables contextData](https://docs.adobe.com/content/help/fr-FR/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html) et doivent donc être associées à des props ou à des eVars (ou à d’autres variables) à l’aide de [règles de traitement](https://docs.adobe.com/content/help/fr-FR/analytics/admin/admin-tools/processing-rules/processing-rules.html) dans l’interface utilisateur d’Analytics.

## Suivi des actions

Tout comme pour le suivi des actions n’impliquant pas de chargement de page sur un site web, vous souhaitez souvent effectuer le suivi d’une action qu’un utilisateur effectue dans votre application – par exemple, les clics sur des éléments qui ne chargent pas d’autre écran. Ce suivi est géré de manière très similaire à la propriété trackState utilisée ci-dessus, mais cette méthode s’appelle `trackAction`.

Vous trouverez ci-dessous la syntaxe et un exemple de code dans la documentation.

**Syntaxe :**

```java
public static void trackAction(final String action, final Map<String, String> contextData) data;
```

**Exemple :**

```java
HashMap<String, String> contextData = new HashMap<String, String>();
contextData.put("key", "value");
MobileCore.trackAction("action taken", contextData);
```

### Interaction de suivi avec le sélecteur de destination

Dans l’exemple d’application Bus Booking, vous pouvez échanger ville d’origine et ville de destination en cliquant sur la flèche entre ces deux valeurs. Vous avez décidé de suivre l’interaction avec cette fonctionnalité dans Adobe Analytics.

![Sélecteur de destination](images/android/mobile-analytics-destinationSwitcher.png)

Ce sélecteur est contrôlé dans le fichier BusBookingActivity de l’exemple de projet. Dans cet exercice, vous enverrez un accès trackAction chaque fois que des utilisateurs cliquent dessus.

#### Ajout du code trackAction

1. Avec l’exemple de projet ouvert dans Android Studio, accédez à BusBookingActivity
1. Localisez la fonction « mBtnFlip.setOnClickListener », sur la ligne 57 ou près de celle-ci
1. Si nécessaire, développez la fonction pour afficher tout le code
1. Dans la fonction onClick, sous l’appel à `flipSourceDesti()`, ajoutez un appel `trackAction()`
1. Nommez l’action « Flip Destination » et indiquez « null » pour le paramètre contextData (car il n’est pas vraiment nécessaire d’envoyer des informations supplémentaires cette fois)
1. Vous pouvez copier et coller le code suivant

   ```java
   MobileCore.trackAction("Flip Destination", null);
   ```

La fonction se présente désormais comme suit :

![Code du sélecteur de destination](images/android/mobile-analytics-destinationSwitcherCode.png)

#### Validation du code trackAction

1. Après avoir ajouté le code, enregistrez le projet, compilez-le et exécutez-le.
1. Cliquez sur l’icône de la corbeille pour effacer la console Logcat
1. Cliquez sur la flèche du sélecteur de destination dans le simulateur et notez qu’une nouvelle requête (ou plusieurs) s’affiche(nt) dans la console.
1. Recherchez `Flip%20Destination` dans Logcat
1. Notez que les paramètres action et pev2 retournent%20Destination (avec espace codé)
1. Notez la clé/valeur `pe=lnk_o` sur la même ligne, indiquant qu’il s’agit d’un accès à « lien personnalisé » déclenché par trackAction.

   <!--![trackAction Result in Debugger](images/android/mobile-analytics-trackActionResult1.png)-->

Beau travail ! Vous avez terminé la leçon sur Analytics. Bien entendu, vous pouvez faire bien d’autres choses pour améliorer notre mise en œuvre d’Analytics, mais nous espérons que vous avez ainsi acquis certaines des compétences de base qui vous seront nécessaires pour répondre au reste de vos besoins.

## Avantages supplémentaires de trackState et trackAction

Lors des derniers exercices, vous avez pu envoyer des données de l’application vers Adobe Analytics à l’aide des API trackState et trackAction. Étant donné que le SDK Mobile Experience Platform est intégré à Launch, vous pouvez réaliser de nombreuses autres actions dans l’interface Launch en exploitant le code que vous venez d’ajouter.

Dans Launch, vous pouvez créer des règles déclenchées par les API trackState et trackAction et leur demander d’exécuter des actions supplémentaires, comme envoyer des requêtes à d’autres solutions Adobe ou à des partenaires externes.

[Suite : « Ajout d’Adobe Audience Manager » &gt;](audience-manager.md)
