---
title: Mise en œuvre d’Adobe Analytics avec Launch
description: Découvrez comment mettre en œuvre Adobe Analytics à l’aide de l’extension Launch d’Adobe Analytics, envoyer une balise d’affichage d’écran, ajouter des variables, suivre les événements et ajouter des plug-ins. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS.
seo-description: null
seo-title: Mise en œuvre d’Adobe Analytics avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout d’Adobe Analytics

Dans cette leçon, vous apprendrez à activer le suivi Adobe Analytics dans votre application.

[Adobe Analytics](https://docs.adobe.com/content/help/fr-FR/analytics/landing/home.html) est une solution de pointe qui vous permet de comprendre vos clients en tant que personnes et d’orienter votre activité grâce aux renseignements sur vos clients.

Dans les leçons [Ajout d’extensions](launch-add-extensions.md) et [Installation du SDK Mobile](launch-install-the-mobile-sdk.md), vous avez ajouté l’extension Adobe Analytics à votre propriété Launch et l’avez importée dans l’exemple d’application.  Maintenant, il vous suffit d’ajouter du code pour effectuer le suivi des états et actions de votre application.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* vérifier que les mesures de cycle de vie sont envoyées à Adobe Analytics ;
* ajouter du code pour effectuer le suivi des états dans votre application avec des données supplémentaires ;
* ajouter du code pour effectuer le suivi des actions dans votre application avec des données supplémentaires.

De nombreux éléments peuvent être mis en œuvre pour Analytics dans Launch. Cette leçon n’est pas exhaustive, mais elle devrait vous donner une vue d’ensemble adéquate des techniques principales dont vous aurez besoin pour effectuer la mise en œuvre sur votre propre application.

## Conditions préalables

Vous devez avoir terminé les leçons de la section [Configuration de Launch](launch-create-a-property.md). Dans cette section, vous avez ajouté l’extension Analytics et configuré votre serveur de suivi et vos identifiants de suite de rapports.

## Mesures de cycle de vie et Adobe Analytics

Les mesures de cycle de vie sont des mesures et des dimensions basées sur l’environnement qui peuvent être facilement activées dans une application à l’aide du SDK Mobile Experience Platform. En réalité, vous les avez déjà ajoutées !

Vous avez déjà activé les mesures de cycle de vie lorsque vous avez ajouté l’extension Core à votre propriété et que vous avez suivi les instructions d’installation Mobile fournies dans l’interface. Ces mesures et dimensions, parmi lesquelles les mesures propres à l’environnement et à l’application telles que la version de l’application, le nombre d’utilisateurs actifs, la version du système d’exploitation, la division du temps, le nombre de jours depuis la dernière utilisation, etc. peuvent s’avérer très utiles dans l’analyse de votre application, en particulier lorsque vous les utilisez pour créer des segments Analytics en vue de les appliquer à tous vos rapports. La liste complète des mesures est disponible dans la [documentation](https://docs.adobe.com/content/help/fr-FR/mobile-services/ios/metrics.html).

### Affichage de l’accès au cycle de vie d’Analytics

Bien que vous puissiez voir les accès au cycle de vie dans n’importe quel programme de débogage ou renifleur de paquets, nous allons simplement les afficher dans la console de débogage Xcode.

1. Compilez et exécutez votre projet dans Xcode afin qu’il lance le simulateur.
1. Dans la console de débogage Xcode, entrez `lifecycle` dans le filtre en bas pour limiter ce qui s’affiche, puis faites défiler les entrées vers le bas.
1. Observez la section `Analytics request was sent with body`.
1. Les mesures de cycle de vie comprennent des éléments comme AppID, CarrierName, DayOfWeek, DaysSinceFirstUse, et d’autres mesures/dimensions répertoriées dans la [documentation](https://docs.adobe.com/content/help/fr-FR/mobile-services/ios/metrics.html).

   ![Débogage des accès au cycle de vie](images/ios/objective-c/mobile-analytics-lifecycleHitDebugging.png)

## Importation de la bibliothèque ACPCore

Dans les exercices suivants, vous utiliserez les API pour effectuer le suivi des états (« trackState ») et des actions (« trackAction ») dans votre application. Pour utiliser ces API, vous devez importer la bibliothèque qui les contient.  Dans le nouveau SDK Mobile Experience Cloud Platform, les API trackState et trackAction ont été déplacées de la bibliothèque Analytics vers la bibliothèque Core, ce qui permet d’exploiter ces API à des fins autres que le suivi d’Adobe Analytics.

Dans ce tutoriel, vous n’effectuerez le suivi que d’un seul état. Toutefois, dans votre application, vous souhaiterez effectuer le suivi de plusieurs d’entre eux.

**Importation de la bibliothèque ACPCore**

1. Ouvrir le fichier BookingViewController.m dans Xcode.
1. Dans la partie supérieure du fichier généralement à côté d’autres instructions d’importation, ajoutez `#import "ACPCore.h"`.
1. Enregistrez.
1. Vous êtes maintenant prêt à utiliser les API trackState ou trackAction dans ce fichier.

   <!--![Import ACPCore to File](images/ios/objective-c/mobile-analytics-importACPCoreToFile.png)-->

## Suivi des états

Dans votre application, vous pouvez disposer de plusieurs écrans de contenu différents que vous fournissez à vos utilisateurs. Ils sont l’équivalent des pages d’un site web. Adobe Analytics propose une méthode permettant d’envoyer ces « accès aux pages consultées » et de les afficher dans les rapports auxquels vous êtes habitué pour vos propriétés web. Cette méthode est appelée « trackState ».

Dans ce tutoriel, vous allez placer le code d’un appel trackState dans un seul écran (page) de votre application. En pratique, vous allez répliquer cela sur tous les autres écrans/états de votre application. Vous allez également explorer plusieurs manières d’envoyer des données (paires clé/valeur) avec l’accès.

Vous trouverez ci-dessous la syntaxe et un exemple de code de la documentation que vous pouvez copier-coller dans ce tutoriel ou dans votre propre application.

**Syntaxe :**

```objective-c
+ (void) trackState: (nullable NSString*) state data: (nullable NSDictionary*) data;
```

**Exemple :**

```objective-c
[ACPCore trackState:@"state name" data:@{@"key":@"value"}];
```

### Suivi d’un état sans données

1. L’application test étant ouverte dans Xcode, accédez à BookingViewController.m et, dans la fonction `viewDidLoad()`, ajoutez un appel de méthode trackState
1. Définissez le paramètre `state name` sur « Home Screen ».
1. Au lieu d’ajouter d’autres données, ajoutez `null` en tant qu’espace réservé dans l’appel de méthode.
1. Vous pouvez aussi copier-coller les éléments suivants :

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:nil];
   ```

   ![Appel trackState de base](images/ios/objective-c/mobile-analytics-basicTrackState2.png)

>[!NOTE] Si vous avez terminé les leçons visant à mettre en œuvre le VEC de Target, vous disposez d’un code supplémentaire dans la fonction viewDidLoad() qui n’est pas affiché dans les copies d’écran de cet exercice. Cela vise à mettre l’accent sur la tâche à accomplir.

**Validation de la méthode trackState**

1. Enregistrez, compilez et exécutez le projet.
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console Xcode.
1. Filtrez la console sur les entrées avec le filtre « home » et regardez l’entrée inférieure où apparaît le message suivant : `Analytics request was sent with body`.
1. Notez que la variable pageName est définie sur `Home Screen` et qu’il n’existe aucune autre paire de données personnalisée. Bien que, techniquement, vous définissiez un « nom d’état » et pas un « nom de page », le nom du paramètre utilisé est `pageName` pour assurer la cohérence avec les mises en œuvre du site web.

   ![Résultat trackState de base](images/ios/objective-c/mobile-analytics-basicTrackStateResult1.png)

### Suivi d’un état avec des données

1. Revenez dans BookingViewController.m puis, dans la fonction `viewDidLoad()`, commentez (ou supprimez) l’appel trackState de base (aucune donnée ajoutée) du dernier exercice.
1. Ajoutez un nouvel appel de méthode trackState, cette fois avec des données, en utilisant `key1` comme clé et `value1` comme valeur.
1. Laissez `state name` sur « Home Screen ».
1. Ou effectuez un copier-coller dans :

   ```objective-c
   [ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1"}];
   ```

   ![Appel trackState de base](images/ios/objective-c/mobile-analytics-trackStateWithData2.png)

**Validation de trackState avec des données**

1. Enregistrez, compilez et exécutez à nouveau le projet.
1. Lorsque le simulateur s’exécute et ouvre l’écran d’accueil de l’application, affichez la console Xcode.
1. Laissez le filtre sur « home » et regardez l’entrée inférieure où apparaît le message suivant : `Analytics request was sent with body`.
1. Maintenant, vous constatez qu’en plus de la variable pageName en cours de définition, vous avez également la paire clé/valeur qui a été envoyée lors de l’accès.

   ![Résultat trackState de base](images/ios/objective-c/mobile-analytics-trackStateWithDataResult1.png)

>[!NOTE] Si vous connaissez les « props et eVars » dans Analytics, vous constaterez que ces noms de variable ne figurent pas dans le SDK. Toutes les données de clé/valeur provenant du SDK seront envoyées sous forme de [variables contextData](https://docs.adobe.com/content/help/fr-FR/analytics/implementation/javascript-implementation/variables-analytics-reporting/context-data-variables.html) et devront donc être mappées sur des props ou des eVars (ou d’autres variables) à l’aide de [règles de traitement](https://docs.adobe.com/content/help/fr-FR/analytics/admin/admin-tools/processing-rules/processing-rules.html) dans l’interface utilisateur d’Analytics.

### Options supplémentaires d’envoi de données

Dans les deux exercices précédents, vous avez effectué deux requêtes, l’une avec des données supplémentaires et l’autre sans. Cependant, que se passe-t-il si vous souhaitez envoyer plusieurs points de données à Analytics avec un écran ou un chargement d’état ? Vous trouverez ci-dessous deux options.

#### Option 1 : plusieurs paires clé/valeur

Lors de l’appel trackState, vous avez la possibilité d’envoyer plusieurs paires clé/valeur, en les séparant simplement par une virgule dans le jeu de données. Par exemple :

```objective-c
[ACPCore trackState:@"Home Screen" data:@{@"key1":@"value1",@"key2":@"value2",@"key3":@"value3"}];
```

#### Option 2 : objet de dictionnaire

Vous pouvez également définir un dictionnaire dans votre code, puis l’envoyer avec la méthode trackState. Bien entendu, si vous avez déjà défini des objets de dictionnaire dans votre code, et que vous souhaitez les envoyer dans Analytics, cette option peut être idéale pour vous. Par exemple :

```objective-c
NSDictionary *theStuff = @{@"key1": @"value1",@"key2": @"value2"};
[ACPCore trackState:@"Home Screen" data:theStuff];
```

**Crédit supplémentaire**
Essayez ces deux options dans votre code, en affichant les résultats dans la console de débogage Xcode. Vous pouvez utiliser le même filtre que précédemment, et vérifier les résultats pour vous assurer que les variables et les valeurs sont bien présentes.

## Suivi des actions

Tout comme pour le suivi des actions n’impliquant pas de chargement de page sur un site web, vous souhaitez souvent effectuer le suivi d’une action qu’un utilisateur effectue dans votre application – par exemple, les clics sur des éléments qui ne chargent pas d’autre écran. Ce paramètre est géré de manière très similaire à la propriété trackState utilisée ci-dessus, sauf que cette méthode est appelée `trackAction`.

Vous trouverez ci-dessous la syntaxe et un exemple de code de la documentation que vous pouvez copier-coller dans ce tutoriel ou dans votre propre application.

**Syntaxe :**

```objective-c
+ (void) trackAction: (nullable NSString*) action data: (nullable NSDictionary*) data;
```

**Exemple :**

```objective-c
[ACPCore trackAction:@"action name" data:@{@"key":@"value"}];
```

### Suivi de l’interaction avec la case à cocher « Aucun arrêt »

Dans cet exemple d’application de réservation de tickets de bus, il y a une case à cocher qui permet aux utilisateurs de décider s’ils veulent limiter leurs résultats de recherche aux options. Vous avez décidé de suivre l’interaction avec cette case à cocher dans Adobe Analytics.

![Case à cocher NonStop](images/mobile-analytics-nonstopCheckbox.png)

Cette case à cocher est contrôlée dans le fichier BookingViewController.m du projet test. Dans cet exercice, vous enverrez un accès trackAction chaque fois que des utilisateurs cocheront ou décocheront la case.

#### Définition du code trackAction

1. Une fois le projet test ouvert dans Xcode, accédez à BookingViewController.m et localisez la fonction « nonStopButtonToggled ».
1. Dans l’instruction `if`, la première section désélectionne la case si elle est déjà sélectionnée. Dans ce scénario, vous souhaitez envoyer un accès avec la valeur « décochée » à l’aide du code suivant :

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"off"}];
   ```

1. Dans la section suivante (section « else » [autre]), la case est cochée si ce n’était pas déjà le cas. Dans ce scénario, vous souhaitez envoyer un accès avec la valeur « cochée » à l’aide du code suivant :

   ```objective-c
   [ACPCore trackAction:@"NonStop Button Interaction" data:@{@"NonStop":@"on"}];
   ```

Observez les autres personnalisations dans le code :

* Vous définissez le paramètre `action name` sur « Interaction avec le bouton NonStop ». Cette valeur renseigne le paramètre « action » de la requête et le rapport/la dimension de lien personnalisé dans Adobe Analytics.
* Le nom de la `key` que vous utilisez est « NonStop ». Il s’agit du nom de la clé que vous pouvez rechercher dans les règles de traitement de l’Admin Console d’Analytics, afin de pouvoir mapper ces valeurs à une prop ou à une eVar.

La fonction se présente désormais comme suit :

![Case à cocher NonStop](images/ios/objective-c/mobile-analytics-nonStopButtonCode2.png)

#### Validation du code trackAction

1. Après avoir ajouté le code, enregistrez le projet, compilez-le et exécutez-le.
1. Cliquez sur l’icône de la corbeille pour vider la console.
1. Cochez la case dans le simulateur, et vous constaterez que deux requêtes s’affichent dans la console. La dernière concerne l’envoi des données à Adobe Analytics à partir du code que vous venez d’ajouter.
1. Notez que les paramètres action et pev2 sont définis sur « Interaction avec le bouton NonStop » (avec espaces codés).
1. Notez que la paire clé/valeur « NonStop=on » est présente et peut ensuite être attribuée à une prop/eVar dans les règles de traitement.
1. Observez la clé/valeur « pe=lnk_o », indiquant qu’il s’agit d’un accès « lien personnalisé », déclenché par la méthode trackAction.

   ![Résultat trackAction dans Debugger](images/ios/objective-c/mobile-analytics-trackActionResult1.png)

Beau travail ! Vous avez terminé la leçon sur Analytics. Bien entendu, vous pouvez faire bien d’autres choses pour améliorer notre mise en œuvre d’Analytics, mais nous espérons que vous avez ainsi acquis certaines des compétences de base qui vous seront nécessaires pour répondre au reste de vos besoins.

## Avantages supplémentaires de trackState et trackAction

Lors des derniers exercices, vous avez pu envoyer des données de l’application vers Adobe Analytics à l’aide des API trackState et trackAction. Étant donné que le SDK Mobile Experience Platform est intégré à Launch, vous pouvez réaliser de nombreuses autres actions dans l’interface Launch en exploitant le code que vous venez d’ajouter.

Dans Launch, vous pouvez créer des règles déclenchées par les API trackState et trackAction et leur demander d’exécuter des actions supplémentaires, comme envoyer des requêtes à d’autres solutions Adobe ou à des partenaires externes.

[Suite : « Ajout d’Adobe Audience Manager » &gt;](audience-manager.md)
