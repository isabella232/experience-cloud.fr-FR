---
title: Mise en oeuvre d’Adobe Analytics avec le lancement
description: Découvrez comment implémenter Adobe Analytics à l’aide de l’extension de lancement d’Adobe Analytics, envoyer une balise d’affichage d’écran, ajouter des variables, suivre les événements et ajouter des modules externes. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications mobiles Android.
seo-description: null
seo-title: Mise en oeuvre d’Adobe Analytics avec le lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout d’Adobe Analytics

Dans cette leçon, vous allez activer le suivi Adobe Analytics dans votre application.

[Adobe Analytics](https://docs.adobe.com/content/help/en/analytics/landing/home.html) est une solution de pointe qui vous permet de comprendre vos clients en tant que personnes et d’orienter votre activité grâce aux renseignements sur vos clients.

Dans les leçons [Ajouter des extensions](launch-add-extensions.md) et [Installer le kit SDK](launch-install-the-mobile-sdk.md) mobile, vous avez ajouté l’extension Adobe Analytics à votre propriété Launch et l’avez importée dans l’exemple d’application.  Maintenant, il vous suffit d'ajouter du code pour suivre les états et actions de votre application !

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Vérifier que les mesures de cycle de vie sont envoyées à Adobe Analytics
* Ajouter du code pour effectuer le suivi des états dans votre application avec des données supplémentaires
* Ajouter du code pour effectuer le suivi des actions dans votre application avec des données supplémentaires

Il existe de nombreux éléments qui pourraient être implémentés pour Analytics dans le lancement. Cette leçon n’est pas exhaustive, mais elle doit vous donner un aperçu complet des principales techniques dont vous aurez besoin pour mettre en oeuvre votre propre application.

## Conditions préalables 

You should have already completed the lessons in the [Configure Launch](launch-create-a-property.md) section. Dans cette section, vous avez ajouté l’extension Analytics et configuré votre serveur de suivi et vos identifiants de suite de rapports.

## Mesures de cycle de vie et Adobe Analytics

Les mesures de cycle de vie sont des mesures et des dimensions basées sur l’environnement qui peuvent être facilement activées dans une application à l’aide du SDK Experience Platform Mobile.

Vous avez déjà activé les mesures de cycle de vie lorsque vous avez ajouté l’extension Core à votre propriété et que vous avez suivi les instructions d’installation Mobile fournies dans l’interface. Ces mesures et dimensions, y compris les mesures propres à l’environnement et à l’application, telles que la version de l’application, le nombre d’utilisateurs actifs, la version du système d’exploitation, la division du temps, les jours depuis la dernière utilisation, etc. peut s’avérer très utile dans l’analyse de votre application, en particulier lorsque vous créez des segments Analytics à partir d’eux pour les appliquer à tous vos rapports. La liste complète des mesures est disponible dans la [documentation](https://docs.adobe.com/content/help/en/mobile-services/android/metrics.htmlh).

## Importation de la bibliothèque ACPCore

Dans la leçon précédente intitulée ["Install the Mobile SDK"](launch-install-the-mobile-sdk.md), vous avez ajouté une instruction d’importation afin de rendre la bibliothèque AdobeCore disponible dans le fichier BusBookingActivity. Cette même bibliothèque sera utilisée pour les appels d’API supplémentaires dans les activités de cette leçon. Dans les exercices suivants, vous utiliserez les API pour effectuer le suivi des états ("trackState") et des actions ("trackAction") dans votre application, qui sont définis dans la bibliothèque AdobeCore.  Dans le nouveau SDK mobile de la plateforme Experience Cloud, les API trackState et trackAction ont été déplacées de la bibliothèque Analytics vers la bibliothèque principale, ce qui permet d’exploiter ces API à des fins autres que le suivi d’Adobe Analytics.

## Suivi des états

Dans votre application, vous pouvez disposer de nombreux écrans de contenu différents que vous fournissez à vos utilisateurs. Ce sont les équivalents des pages d'un site Web. Adobe Analytics fournit une méthode pour envoyer ces "accès aux pages vues" et les afficher dans les rapports auxquels vous êtes habitué pour vos propriétés Web. Cette méthode est appelée "trackState".

Dans ce didacticiel, vous allez placer le code d’un appel trackState dans un seul écran (page) de votre application. Dans la vie réelle, vous allez répliquer cela sur tous les autres écrans/états de votre application.

Vous trouverez ci-dessous une syntaxe et un exemple de code issus de la documentation que vous pouvez copier-coller dans ce didacticiel ou dans votre propre application.

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

1. L’exemple d’application étant ouvert dans Android Studio, accédez à BusBookingActivity, puis faites défiler l’écran vers le bas jusqu’à la fonction onResume.
1. Ajouter un appel de méthode trackState
1. Définir `state name` sur "Écran de réservation"
1. Au lieu d’ajouter des données supplémentaires, ajoutez `null` comme espace réservé dans l’appel d’API.
1. Ou copiez-collez dans les éléments suivants :

   ```java
   MobileCore.trackState("Booking Screen", null);
   ```

![Appel trackState de base](images/android/mobile-analytics-basicTrackStateCall2.png)

**Pour valider trackState**

1. Enregistrer, créer et exécuter le projet
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console de débogage Android Studio Logcat.
1. Recherchez dans la console `pageName=Booking%20Screen`
1. Notez que la variable pageName est définie sur `Booking Screen` (avec %20 comme espace codé) et qu’il n’existe aucune autre paire de données personnalisée. Bien que, techniquement, vous définissiez un "nom d’état" et non un "nom de page", le nom du paramètre utilisé est utilisé `pageName` pour assurer la cohérence avec les implémentations du site Web.

   ![Résultat trackState de base](images/android/mobile-analytics-basicTrackStateResult.png)

### Suivi d’un état avec des données

1. Revenez dans BusBookingActivity et ajoutez une importation en haut du fichier `import java.util.HashMap;` sous les importations existantes.
1. Dans la `onResume()` fonction, placez en commentaire (ou supprimez) l’appel trackState de base du dernier exercice.
1. Ajoutez un nouvel appel de méthode trackState, cette fois avec des données en créant et en nommant un HashMap, en utilisant la commande "put" pour inclure certaines paires clé/valeur, puis en appelant ce HashMap dans l'appel à trackState
1. Laissez `state name` comme "Écran de réservation"
1. Ou copier et coller dans :

   ```java
   HashMap cData = new HashMap<String, String>();
   cData.put("cd.section", "Bus Booking");
   cData.put("cd.subSection", "Booking");
   cData.put("cd.conversionType", "Landing");
   MobileCore.trackState("Booking Screen", cData);
   ```

   ![Appel trackState avec données](images/android/mobile-analytics-trackStateWithData.png)

**Pour valider trackState avec des données**

1. Enregistrez, générez et exécutez à nouveau le projet.
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console de débogage Android Studio Logcat.
1. Rechercher `subSection` (ou l’une des clés ou valeurs que vous avez saisies dans le code)
1. Maintenant, voyez qu'en plus de la variable pageName en cours de définition, vous avez également les paires clé/valeur qui ont été envoyées sur l'accès.

   ![trackState avec résultat de données](images/android/mobile-analytics-trackStateWithDataResult.png)

>[!NOTE] Si vous connaissez les "props and eVars" dans Analytics, vous constaterez que ces noms de variable ne figurent pas dans le SDK. Toutes les données de clé/valeur provenant du SDK seront envoyées sous forme de variables [](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html)contextData et devront donc être mappées sur des props ou des eVars (ou d’autres variables) à l’aide de règles [de](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/processing-rules/processing-rules.html) traitement dans l’interface utilisateur d’Analytics.

## Suivi des actions

Tout comme pour le suivi des actions hors chargement de page sur un site Web, vous souhaitez souvent effectuer le suivi d’une action qu’un utilisateur effectue dans votre application ; par exemple, les clics sur des éléments qui ne chargent pas un autre écran. Ceci est géré de manière très similaire à la propriété trackState utilisée ci-dessus, sauf que cette méthode est appelée `trackAction`.

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

### Interaction de suivi avec le commutateur de destination

Dans cet exemple d’application de réservation de bus, vous pouvez changer la ville d’origine avec la ville de destination en cliquant sur la flèche entre ces deux valeurs. Vous avez décidé de suivre l’interaction avec cette fonctionnalité dans Adobe Analytics.

![Commutateur de destination](images/android/mobile-analytics-destinationSwitcher.png)

Ce commutateur est contrôlé dans le fichier BusBookingActivity de l'exemple de projet. Dans cet exercice, vous enverrez un accès trackAction chaque fois que des personnes cliquent dessus.

#### Pour ajouter le code trackAction

1. Avec l'exemple de projet ouvert dans Android Studio, accédez à BusBookingActivity
1. Localisez la fonction "mBtnFlip.setOnClickListener", à la ligne 57 ou aux alentours.
1. Développez la fonction, si nécessaire, pour afficher tout le code.
1. Dans la fonction onClick, sous l’appel à `flipSourceDesti()`, ajoutez un `trackAction()` appel.
1. Définissez le nom de l’action sur "Retourner la destination" et ajoutez "null" pour le paramètre contextData (car nous n’avons pas vraiment besoin d’envoyer d’informations supplémentaires cette fois).
1. Vous pouvez copier et coller le code suivant

   ```java
   MobileCore.trackAction("Flip Destination", null);
   ```

La fonction se présente désormais comme suit :

![Code du commutateur de destination](images/android/mobile-analytics-destinationSwitcherCode.png)

#### Pour valider le code trackAction

1. Après avoir ajouté le code, enregistrez le projet, exécutez et générez
1. Cliquez sur l'icône de transparence pour effacer la console Logcat.
1. Cliquez sur la flèche du sélecteur de destination dans le simulateur, et notez qu’une nouvelle requête (ou plus) s’affiche dans la console.
1. Rechercher `Flip%20Destination` dans Logcat
1. Remarquez que les paramètres action et pev2 retournent%20Destination (avec espace codé)
1. Notez la `pe=lnk_o` clé/valeur sur la même ligne, indiquant qu’il s’agit d’un accès "lien personnalisé", déclenché par trackAction.

   <!--![trackAction Result in Debugger](images/android/mobile-analytics-trackActionResult1.png)-->

Beau travail ! Vous avez terminé la leçon Analytics. Bien sûr, vous pouvez faire bien d’autres choses pour améliorer notre mise en oeuvre Analytics, mais j’espère que cela vous a donné certaines des compétences essentielles dont vous aurez besoin pour répondre au reste de vos besoins.

## Avantages supplémentaires de trackState et trackAction

Lors de ces derniers exercices, vous avez pu envoyer des données de l’application vers Adobe Analytics à l’aide des API trackState et trackAction. Comme le SDK Experience Platform Mobile est ancré dans Launch, vous pouvez faire bien d’autres choses dans l’interface Launch en exploitant le code que vous venez d’ajouter.

Dans Launch, vous pouvez créer des règles déclenchées par les API trackState et trackAction et leur demander d’exécuter d’autres actions, telles que des demandes à d’autres solutions Adobe ou à des partenaires externes.

[Suivant : "Ajouter Adobe Audience Manager" &gt;](audience-manager.md)
