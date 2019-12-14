---
title: Mise en oeuvre d’Adobe Analytics avec le lancement
description: Découvrez comment implémenter Adobe Analytics à l’aide de l’extension de lancement d’Adobe Analytics, envoyer une balise d’affichage d’écran, ajouter des variables, suivre les événements et ajouter des modules externes. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications iOS Objective-C mobiles.
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

Les mesures de cycle de vie sont des mesures et des dimensions basées sur l’environnement qui peuvent être facilement activées dans une application à l’aide du SDK Experience Platform Mobile. En fait, vous les avez déjà ajoutés !

Vous avez déjà activé les mesures de cycle de vie lorsque vous avez ajouté l’extension Core à votre propriété et que vous avez suivi les instructions d’installation Mobile fournies dans l’interface. Ces mesures et dimensions, y compris les mesures propres à l’environnement et à l’application, telles que la version de l’application, le nombre d’utilisateurs actifs, la version du système d’exploitation, la division du temps, les jours depuis la dernière utilisation, etc. peut s’avérer très utile dans l’analyse de votre application, en particulier lorsque vous créez des segments Analytics à partir d’eux pour les appliquer à tous vos rapports. La liste complète des mesures est disponible dans la [documentation](https://docs.adobe.com/content/help/en/mobile-services/ios/metrics.html).

### Affichage de l’accès au cycle de vie d’Analytics

Bien que vous puissiez voir les accès de cycle de vie dans n’importe quel programme de débogage/renifleur de paquets, nous allons simplement les afficher dans la console de débogage Xcode.

1. Générez et exécutez votre projet dans Xcode afin qu’il lance le simulateur.
1. Dans la console de débogage Xcode, entrez `lifecycle` dans le filtre en bas pour limiter ce qui s’affiche, puis faites défiler les entrées vers le bas.
1. Remarquez la `Analytics request was sent with body` section
1. Les mesures de cycle de vie comprennent des éléments tels que AppID, CarrierName, DayOfWeek, DaysSinceFirstUse et d’autres mesures/dimensions répertoriées dans la [documentation.](https://docs.adobe.com/content/help/en/mobile-services/ios/metrics.html)

   ![Débogage des accès au cycle de vie](images/ios/objective-c/mobile-analytics-lifecycleHitDebugging.png)

## Importation de la bibliothèque ACPCore

Dans les exercices suivants, vous utiliserez les API pour effectuer le suivi des états ("trackState") et des actions ("trackAction") dans votre application. Pour utiliser ces API, vous devez importer la bibliothèque qui les contient.  Dans le nouveau SDK mobile de la plateforme Experience Cloud, les API trackState et trackAction ont été déplacées de la bibliothèque Analytics vers la bibliothèque principale, ce qui permet d’exploiter ces API à des fins autres que le suivi d’Adobe Analytics.

Dans ce didacticiel, vous n’effectuerez le suivi que d’un seul état. Toutefois, dans votre application, vous souhaiterez effectuer le suivi de plusieurs états.

**Pour importer la bibliothèque ACPCore**

1. Ouvrir le fichier BookingViewController.m dans Xcode
1. Dans la partie supérieure du fichier (généralement à côté d’autres instructions d’importation), ajoutez `#import "ACPCore.h"`
1. Enregistrez
1. Vous êtes maintenant prêt à utiliser les API trackState ou trackAction dans ce fichier

   <!--![Import ACPCore to File](images/ios/objective-c/mobile-analytics-importACPCoreToFile.png)-->

## Suivi des états

Dans votre application, vous pouvez disposer de nombreux écrans de contenu différents que vous fournissez à vos utilisateurs. Ce sont les équivalents des pages d'un site Web. Adobe Analytics fournit une méthode pour envoyer ces "accès aux pages vues" et les afficher dans les rapports auxquels vous êtes habitué pour vos propriétés Web. Cette méthode est appelée "trackState".

Dans ce didacticiel, vous allez placer le code d’un appel trackState dans un seul écran (page) de votre application. Dans la vie réelle, vous allez répliquer cela sur tous les autres écrans/états de votre application. Vous allez également explorer plusieurs manières d’envoyer des données (paires clé/valeur) avec l’accès.

Vous trouverez ci-dessous la syntaxe et un exemple de code de la documentation que vous pouvez copier-coller dans ce didacticiel ou dans votre propre application.

**Syntaxe :**

```objective-c
+ (void) trackState: (nullable NSString*) state data: (nullable NSDictionary*) data;
```

**Exemple :**

```objective-c
[ACPCore trackState:@"state name" data:@{@"key":@"value"}];
```

### Suivi d’un état sans données

1. L'exemple d'application étant ouvert dans Xcode, accédez à BookingViewController.m et, dans la `viewDidLoad()` fonction, ajoutez un appel de méthode trackState.
1. Définissez le paramètre `state name` sur "Ecran d’accueil".
1. Au lieu d’ajouter d’autres données, ajoutez-les `null` en tant qu’espace réservé dans l’appel de méthode.
1. Ou copiez-collez dans les éléments suivants :

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:nil];
   ```

   ![Appel trackState de base](images/ios/objective-c/mobile-analytics-basicTrackState2.png)

>[!NOTE] Si vous avez terminé les leçons pour implémenter le compositeur d’expérience visuelle de Target, vous disposez d’un code supplémentaire dans la fonction viewDidLoad() qui n’est pas affiché dans les captures d’écran de cet exercice. On s'attend à ce que cette mesure soit prise pour mettre l'accent sur la tâche à accomplir.

**Pour valider trackState**

1. Enregistrer, créer et exécuter le projet
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console Xcode.
1. Filtrez la console aux entrées avec "accueil" et regardez l’entrée inférieure qui montre que la variable `Analytics request was sent with body`
1. Notez que la variable pageName est définie sur `Home Screen`et qu’il n’existe aucune autre paire de données personnalisée. Bien que, techniquement, vous définissiez un "nom d’état" et non un "nom de page", le nom du paramètre utilisé est utilisé `pageName` pour assurer la cohérence avec les implémentations du site Web.

   ![Résultat trackState de base](images/ios/objective-c/mobile-analytics-basicTrackStateResult1.png)

### Suivi d’un état avec des données

1. Revenez dans BookingViewController.m, et dans la `viewDidLoad()` fonction, placez en commentaire (ou supprimez) l’appel trackState de base (aucune donnée ajoutée) à partir du dernier exercice.
1. Ajoutez un nouvel appel de méthode trackState, cette fois avec des données, en utilisant `key1` comme clé et `value1` comme valeur
1. Laissez le `state name` comme "Ecran d’accueil"
1. Ou copier et coller dans :

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1"}];
   ```

   ![Appel trackState de base](images/ios/objective-c/mobile-analytics-trackStateWithData2.png)

**Pour valider trackState avec des données**

1. Enregistrez, générez et exécutez à nouveau le projet.
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console Xcode.
1. Laissez le filtre comme "accueil" et examinez l’entrée inférieure qui indique que la variable `Analytics request was sent with body`
1. Maintenant, vous voyez qu'en plus de la variable pageName en cours de définition, vous avez également la paire clé/valeur qui a été envoyée sur l'accès

   ![Résultat trackState de base](images/ios/objective-c/mobile-analytics-trackStateWithDataResult1.png)

>[!NOTE] Si vous connaissez les "props and eVars" dans Analytics, vous constaterez que ces noms de variable ne figurent pas dans le SDK. Toutes les données de clé/valeur provenant du SDK seront envoyées sous forme de variables [](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html)contextData et devront donc être mappées sur des props ou des eVars (ou d’autres variables) à l’aide de règles [de](https://docs.adobe.com/content/help/en/analytics/admin/admin-tools/processing-rules/processing-rules.html) traitement dans l’interface utilisateur d’Analytics.

### Options d’envoi de données supplémentaires

Dans les deux exercices précédents, vous avez fait deux requêtes, l'une avec des données supplémentaires et l'autre sans. Cependant, que se passe-t-il si vous souhaitez envoyer plusieurs points de données à Analytics avec un écran ou un chargement d’état ? Vous trouverez ci-dessous deux options.

#### Option 1 : Plusieurs paires clé/valeur

Dans l’appel trackState, vous avez la possibilité d’envoyer plusieurs paires clé/valeur, simplement par une virgule les séparant dans le jeu de données. Par exemple :

```objective-c
[ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1",@"key2":@"value2",@"key3":@"value3"}];
```

#### Option 2 : Objet Dictionary

Vous pouvez également définir un dictionnaire dans votre code, puis l’envoyer avec trackState. Bien entendu, si vous avez déjà défini des objets de dictionnaire dans votre code et souhaitez les envoyer dans Analytics, cette option peut être idéale pour vous. Par exemple :

```objective-c
NSDictionary *theStuff = @{@"key1": @"value1",@"key2": @"value2"};
[ACPCore trackState:@"Home Screen" data:theStuff];
```

**Crédit** supplémentaire Allez de l'avant et essayez ces deux options dans votre code, en affichant les résultats dans la console de débogage Xcode. Vous pouvez utiliser le même filtre qu’auparavant et vérifier les résultats pour vous assurer que les variables et les valeurs sont bien présentes.

## Suivi des actions

Tout comme pour le suivi des actions hors chargement de page sur un site Web, vous souhaitez souvent effectuer le suivi d’une action qu’un utilisateur effectue dans votre application ; par exemple, les clics sur des éléments qui ne chargent pas un autre écran. Ceci est géré de manière très similaire à la propriété trackState utilisée ci-dessus, sauf que cette méthode est appelée `trackAction`.

Vous trouverez ci-dessous la syntaxe et un exemple de code de la documentation que vous pouvez copier-coller dans ce didacticiel ou dans votre propre application.

**Syntaxe :**

```objective-c
+ (void) trackAction: (nullable NSString*) action data: (nullable NSDictionary*) data;
```

**Exemple :**

```objective-c
[ACPCore trackAction:@"action name" data:@{@"key":@"value"}];
```

### Suivi de l’interaction avec la case à cocher "Pas d’arrêt"

Dans cet exemple d’application de réservation de bus, il existe une case à cocher qui permet aux utilisateurs de décider s’ils veulent limiter leurs résultats de recherche aux options. Vous avez décidé de suivre l’interaction avec cette case à cocher dans Adobe Analytics.

![Case à cocher Non arrêté](images/mobile-analytics-nonstopCheckbox.png)

Cette case à cocher est contrôlée dans le fichier BookingViewController.m du projet exemple. Dans cet exercice, vous enverrez un accès trackAction chaque fois que des personnes cochent ou décochent la case.

#### Définition du code trackAction

1. L'exemple de projet étant ouvert dans Xcode, accédez à BookingViewController.m et localisez la fonction "nonStopButtonToggled".
1. Dans l’ `if` instruction, la première section désélectionne la zone si elle est déjà sélectionnée. Dans ce scénario, vous souhaitez envoyer un accès avec la valeur "off" à l’aide du code suivant :

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"off"}];
   ```

1. Dans la section suivante (la section "else"), il vérifie la case si elle n’est pas déjà cochée. Dans ce scénario, vous souhaitez envoyer un accès avec la valeur "on" à l’aide du code suivant :

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"on"}];
   ```

Remarquez les autres personnalisations du code :

* Vous définissez le paramètre sur `action name` Interaction de bouton NonStop. Cette valeur renseigne le paramètre "action" de la requête et le rapport/dimension de lien personnalisé dans Adobe Analytics.
* Le nom du `key` système que vous utilisez est "NonStop". Il s’agit du nom de clé que vous pouvez rechercher dans les règles de traitement dans la console d’administration Analytics, afin de mapper ces valeurs à une prop ou une eVar.

La fonction se présente désormais comme suit :

![Case à cocher Non arrêté](images/ios/objective-c/mobile-analytics-nonStopButtonCode2.png)

#### Pour valider le code trackAction

1. Après avoir ajouté le code, enregistrez le projet, exécutez et générez
1. Cliquez sur l’icône de transparence pour effacer la console.
1. Cochez la case du simulateur, en remarquant que deux requêtes dans la console s’affichent. La dernière est l’envoi des données à Adobe Analytics à partir du code que vous venez d’ajouter.
1. Notez que les paramètres action et pev2 sont définis sur "Interaction de bouton non stop" (avec espaces codés).
1. Notez que la paire clé/valeur "NonStop=on" est présente et peut ensuite être affectée à une prop/eVar dans les règles de traitement.
1. Remarquez la clé/valeur "pe=lnk_o", indiquant qu’il s’agit d’un accès "lien personnalisé", déclenché par trackAction.

   ![Résultat trackAction dans le débogueur](images/ios/objective-c/mobile-analytics-trackActionResult1.png)

Beau travail ! Vous avez terminé la leçon Analytics. Bien sûr, vous pouvez faire bien d’autres choses pour améliorer notre mise en oeuvre Analytics, mais j’espère que cela vous a donné certaines des compétences essentielles dont vous aurez besoin pour répondre au reste de vos besoins.

## Avantages supplémentaires de trackState et trackAction

Lors de ces derniers exercices, vous avez pu envoyer des données de l’application vers Adobe Analytics à l’aide des API trackState et trackAction. Comme le SDK Experience Platform Mobile est ancré dans Launch, vous pouvez faire bien d’autres choses dans l’interface Launch en exploitant le code que vous venez d’ajouter.

Dans Launch, vous pouvez créer des règles déclenchées par les API trackState et trackAction et leur demander d’exécuter d’autres actions, telles que des demandes à d’autres solutions Adobe ou à des partenaires externes.

[Suivant : "Ajouter Adobe Audience Manager" &gt;](audience-manager.md)
