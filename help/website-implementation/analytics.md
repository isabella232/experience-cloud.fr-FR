---
title: Mise en oeuvre d’Adobe Analytics avec le lancement
description: Découvrez comment implémenter Adobe Analytics à l’aide de l’extension de lancement d’Adobe Analytics, envoyer la balise de page vue, ajouter des variables, suivre les événements et ajouter des modules externes. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-description: null
seo-title: Mise en oeuvre d’Adobe Analytics avec le lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajout d’Adobe Analytics

Dans cette leçon, vous allez mettre en oeuvre l’extension [](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/analytics-extension/overview.html) Adobe Analytics et créer des règles pour envoyer des données à Adobe Analytics.

[Adobe Analytics](https://docs.adobe.com/content/help/en/analytics/landing/home.html) est une solution de pointe qui vous permet de comprendre vos clients en tant que personnes et d’orienter votre activité grâce aux renseignements sur vos clients.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

1. Ajout de l’extension Adobe Analytics
1. Définir des variables globales à l’aide de l’extension
1. Ajouter la balise de page vue
1. Ajouter des variables supplémentaires à l’aide de règles
1. Ajouter le suivi des clics et d’autres balises basées sur un événement
1. Ajout de modules externes Analytics

Il existe de nombreux éléments qui pourraient être implémentés pour Analytics dans le lancement. Cette leçon n’est pas exhaustive, mais elle devrait vous donner un aperçu complet des principales techniques que vous devrez mettre en oeuvre sur votre propre site.

## Conditions préalables 

You should have already completed the lessons in [Configure Launch](launch.md) and [Add the Identity Service](id-service.md).

De plus, vous aurez besoin d’au moins un identifiant de suite de rapports et de votre serveur de suivi. Si vous ne disposez pas d’une suite de rapports test/développement que vous pouvez utiliser pour ce didacticiel, créez-en une. Si vous n’êtes pas sûr de la marche à suivre, consultez [la documentation](https://docs.adobe.com/content/help/en/analytics/admin/manage-report-suites/new-report-suite/new-report-suite.html). Vous pouvez récupérer votre serveur de suivi à partir de votre implémentation actuelle, de votre conseiller Adobe ou de votre représentant du service à la clientèle.

## Ajouter l’extension Analytics

L’extension Analytics se compose de deux parties principales :

1. La configuration de l’extension, qui gère les paramètres principaux de la bibliothèque AppMeasurement.js et qui peut définir des variables globales.
1. Les actions de règle effectuent les opérations suivantes :
   1. Définir des variables
   1. Effacer des variables
   1. Envoyer la balise Analytics

**Pour ajouter l’extension Analytics**

1. Accéder à **[!UICONTROL Extensions &gt; Catalogue]**
1. Localisation de l’extension Adobe Analytics
1. Cliquez sur **[!UICONTROL Installer]**

   ![Installation de l’extension Analytics](images/analytics-catalog-install.png)

1. Sous Gestion des [!UICONTROL bibliothèques &gt; Report Suites], saisissez les identifiants de suite de rapports que vous souhaitez utiliser avec chaque environnement de lancement. Notez que lorsque vous commencez à saisir du texte dans la zone, une liste de toutes vos suites de rapports est préremplie. (Il est possible d’utiliser une Report Suite pour tous les environnements dans ce didacticiel, mais dans la vie réelle, vous souhaiterez utiliser des Report Suites distinctes, comme illustré dans l’illustration ci-dessous)

   ![Entrer les identifiants des suites de rapports](images/analytics-config-reportSuite.png)

   >[!TIP] Nous vous recommandons d’utiliser l’option [!UICONTROL Gérer la bibliothèque pour moi] en tant que paramètre de gestion [!UICONTROL des] `AppMeasurement.js` bibliothèques, car il est beaucoup plus facile de tenir la bibliothèque à jour.

1. Sous [!UICONTROL Général &gt; Serveur]de suivi, entrez votre serveur de suivi, par ex. "`tmd.sc.omtrdc.net`." Entrez votre serveur de suivi SSL si votre site prend en charge `https://`

   ![Entrer les serveurs de suivi](images/analytics-config-trackingServer.png)

1. Dans la section [!UICONTROL Variables]globales, définissez la variable Nom [!UICONTROL de] page à l’aide de votre `Page Name` élément de données. Cliquez sur l’icône ![de l’élément de](images/icon-dataElement.png) données pour ouvrir le module et choisir l’élément de `Page Name` données de la page.)

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Définissez la variable de nom de page et enregistrez](images/analytics-extension-pageName.png)

>Les variables globales [!NOTE] peuvent être définies dans la configuration de l’extension ou dans les actions de règle. Be aware that when setting variables in the extension configuration, the data layer must be defined *before* the Launch embed codes.

## Envoyer la balise Page vue

Vous allez maintenant créer une règle pour déclencher la balise Analytics, qui enverra la variable Nom [!UICONTROL de] page définie dans la configuration de l’extension.

You have already created an "All Pages - Library Loaded" rule in the [Add a Data Element, a Rule and a Library](launch-data-elements-rules.md) lesson of this tutorial, which is triggered on every page when the Launch library loads. Vous *pouvez* également utiliser cette règle pour Analytics. Toutefois, cette configuration nécessite que tous les attributs de couche de données utilisés dans la balise Analytics soient définis avant les codes d’intégration Lancer. Pour plus de flexibilité dans la collecte de données, vous allez créer une nouvelle règle "toutes les pages" déclenchée sur DOM Ready pour déclencher la balise Analytics.

**Pour envoyer la balise Page vue**

1. Accédez à la section **[!UICONTROL Règles]** dans le volet de navigation supérieur, puis cliquez sur **[!UICONTROL Ajouter une règle.]**

   ![Ajouter une règle](images/target-addRule.png)

1. Attribuez un nom à la règle `All Pages - DOM Ready`
1. Click **[!UICONTROL Events &gt; Add]** to open the `Event Configuration` screen

   ![Attribuez un nom à la règle et ajoutez l’événement](images/analytics-domReady-nameAddAnalyticsEvent.png)

1. Sélectionnez Type d’ **[!UICONTROL événement &gt; Prêt]** pour le modèle DOM (Notez que l’ordre de la règle est "50").
1. Click **[!UICONTROL Keep Changes]**
   ![Configuration de l’événement](images/analytics-configureEventDomReady.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![Cliquez sur l’icône Plus pour ajouter une nouvelle action.](images/analytics-ruleAddAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Analytics.]**

1. Sélectionnez **[!UICONTROL Action Type &gt; Envoyer la balise]**

1. Laissez Suivi défini sur `s.t()`. Note that if you wanted to make an `s.tl()` call in a click-event rule you could do that using the Send Beacon action, as well.

1. Cliquez sur le bouton **[!UICONTROL Conserver les modifications]**

   ![Cliquez sur l’icône Plus pour ajouter une nouvelle action.](images/analytics-sendBeacon.png)

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Cliquez sur Enregistrer dans la bibliothèque et créer](images/analytics-saveToLibraryAndBuild.png)

### Validation de la balise Page vue

Maintenant que vous avez créé une règle pour envoyer une balise Analytics, vous devriez être en mesure de voir la requête dans le débogueur Experience Cloud.

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser
1. Cliquez sur l’icône Débogueur ![Ouvrez le débogueur](images/analytics-debuggerIcon.png) Experience Cloud pour ouvrir le débogueur **[!UICONTROL Adobe Experience Cloud.]**
1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![L’environnement de développement de votre lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

1. Cliquez pour ouvrir l’onglet Analytics.
1. Développez le nom de votre suite de rapports pour afficher toutes les requêtes qui lui sont envoyées.
1. Confirmer que la requête a été déclenchée avec la variable et la valeur Nom de page

   ![Validation de l’accès à la page](images/analytics-validatePageHit.png)

>[!NOTE] Si Page Name (Nom de page) ne s’affiche pas, revenez à la procédure décrite sur cette page pour vous assurer que vous n’avez rien manqué.

## Ajouter des variables à l’aide de règles

When you configured the Analytics Extension, you populated the `pageName` variable in the extension configuration. Il s’agit d’un emplacement idéal pour renseigner d’autres variables globales, telles que les eVars et les props, à condition que la valeur soit disponible sur la page avant le chargement du code intégré à Launch.

Les règles qui utilisent l’ `Set Variables` action offrent un emplacement plus souple pour définir des variables (ainsi que des événements). Les règles vous permettent de définir différentes variables et événements Analytics dans différentes conditions. For example, you could set the `prodView` only on product detail pages and the `purchase` event only on order confirmation pages. Cette section explique comment définir des variables à l’aide de règles.

### Exemple d’utilisation

Les pages Détails du produit (PDP) sont des points importants pour la collecte de données sur des sites de vente au détail. En règle générale, vous souhaitez qu’Analytics enregistre qu’une consultation de produit s’est produite et que le produit a été consulté. Cela s’avère utile pour déterminer quels produits sont prisés par vos clients. Sur un site multimédia, les pages d’articles ou de vidéos peuvent utiliser des techniques de suivi similaires à celles que vous utiliserez dans cette section.  When you load a Product Detail Page, you might want to put that value into a "Page Type" `eVar`, as well as set some events and the product id. Cela nous permettra de voir ce qui suit dans notre analyse :

1. Nombre de chargements de pages de détails du produit
1. Quels produits spécifiques sont consultés et combien de fois ?
1. Comment d'autres facteurs (campagnes, recherches, etc.) affectent le nombre de personnes chargées par le PDP

### Créer un élément de données pour le type de page

Vous devez tout d'abord identifier les pages qui sont les pages Détails du produit. Vous le ferez avec un élément de données.

**Pour créer l’élément de données pour le type de page**

1. Cliquez sur Eléments **[!UICONTROL de]** données dans la barre de navigation supérieure.
1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**

   ![Ajouter un nouvel élément de données](images/analytics-addDataElement.png)

1. Nommez l’élément de données `Page Type`
1. Sélectionnez **[!UICONTROL Data Element Type &gt; JavaScript Variable]**
1. Utilisez `digitalData.page.category.type` comme `JavaScript variable name`
1. Check the `Clean text` and `Force Lower Case` options
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Ajouter un nouvel élément de données pour le type de page](images/analytics-PageTypeDataElement.png)

### Créer un élément de données pour l’ID de produit

Ensuite, vous allez collecter l’ID de produit de la page Détails du produit actuelle avec un élément de données.

**Pour créer l’élément de données pour l’ID de produit**

1. Cliquez sur Eléments **[!UICONTROL de]** données dans la barre de navigation supérieure.
1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**

   ![Ajouter un nouvel élément de données](images/analytics-addDataElement.png)

1. Nommez l’élément de données `Product Id`
1. Sélectionnez **[!UICONTROL Data Element Type &gt; JavaScript Variable]**
1. Utilisez `digitalData.product.0.productInfo.sku` comme `JavaScript variable name`
1. Check the `Force lowercase value` option
1. Check the `Clean text` option
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Ajouter un nouvel élément de données pour le type de page](images/analytics-ProductIdDataElement.png)

### Ajouter l’extension de chaîne de produit Adobe Analytics

If you are already familiar with Adobe Analytics implementations, you are probably familiar with the [products variable](https://docs.adobe.com/content/help/en/analytics/components/variables/dimensions-reports/reports-products.html). La variable products a une syntaxe très spécifique et est utilisée de différentes manières selon le contexte. Pour faciliter la variable de population des produits dans Launch, trois extensions supplémentaires ont déjà été créées sur le marché Launch Extension ! Dans cette section, vous allez ajouter une extension créée par Adobe Consulting pour l’utiliser dans la page Détails du produit.

**Pour ajouter l’`Adobe Analytics Product String`extension**

1. Accédez à la page [!UICONTROL Extensions &gt; Catalogue] .
1. Recherchez l’ `Adobe Analytics Product String` extension auprès d’Adobe Consulting Services et cliquez sur **[!UICONTROL Installer.]**
   ![Ajout de l’extension de chaîne de produit Adobe Analytics par Adobe Consulting](images/analytics-addProductStringExtension.png)
1. Prenez quelques instants pour lire les instructions
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Enregistrez l’extension et générez-la dans votre bibliothèque.](images/analytics-addProductStringExtensionSave.png)

### Créer la règle pour les pages Détails du produit

Vous allez maintenant utiliser vos nouveaux éléments de données et votre extension pour créer votre règle de page Détails du produit. Pour cette fonctionnalité, vous allez créer une autre règle de chargement de page, déclenchée par DOM Ready. However, you will use a condition so that it only fires on the Product Detail pages and the order setting so that it fires _before_ the rule that sends the beacon.

**Pour créer la règle de page Détails du produit**

1. Accédez à la section **[!UICONTROL Règles]** dans le volet de navigation supérieur, puis cliquez sur **[!UICONTROL Ajouter une règle.]**

   ![Ajouter une règle](images/target-addRule.png)

1. Attribuez un nom à la règle `Product Details - DOM Ready - 40`
1. Click **[!UICONTROL Events &gt; Add]** to open the `Event Configuration` screen

   ![Attribuez un nom à la règle et ajoutez l’événement](images/analytics-domReadyAddEvent.png)

1. Sélectionnez **[!UICONTROL Event Type &gt; DOM Ready]**
1. Set the **[!UICONTROL Order]** to 40, so that the rule will run *before* the rule containing the Analytics &gt; Send Beacon action
1. Click **[!UICONTROL Keep Changes]**
   ![Configuration de l’événement](images/analytics-configDOMReadyEvent.png)

1. Sous **[!UICONTROL Conditions]**, cliquez sur l’icône ![](images/icon-plus.png) Cliquez sur Plus pour ouvrir l’ `Condition Configuration` écran.
   ![Cliquez sur l’icône Plus pour ajouter une nouvelle condition.](images/analytics-PDPRuleAddCondition.png)

   1. Sélectionnez Type de **[!UICONTROL condition &gt; Comparaison des valeurs.]**
   1. Use the data element picker, choose `Page Type` in the first field
   1. Sélectionnez **[!UICONTROL Contient]** dans la liste déroulante des opérateurs de comparaison.
   1. Dans le type de champ suivant `product-page` (c’est la partie unique de la valeur de type de page extraite de la couche de données sur les PDP)
   1. Click **[!UICONTROL Keep Changes]**

      ![Définir la condition](images/analytics-PDP-condition.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![Cliquez sur l’icône Plus pour ajouter une nouvelle action.](images/analytics-PDPAddAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Analytics.]**
1. Sélectionnez Type **[!UICONTROL d’action &gt; Définir des variables.]**
1. Sélectionnez **[!UICONTROL eVar1 &gt; Définir comme]** et saisissez `product detail page`
1. Set **[!UICONTROL event1]**, leaving the optional values blank
1. Sous Evénements, cliquez sur le bouton **[!UICONTROL Ajouter un autre]** .
1. Set the **[!UICONTROL prodView]** event, leaving the optional values blank
1. Click **[!UICONTROL Keep Changes]**

   ![Définition des variables Analytics dans la règle PDP](images/analytics-PDPsetVariables.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![Ajouter une autre action pour la chaîne de produit](images/analytics-PDPaddProductStringAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Chaîne de produit Adobe Analytics.]**
1. Sélectionnez Type **[!UICONTROL d’action &gt; Définir s.products.]**

1. Dans la section Evénement **[!UICONTROL de commerce électronique]** Analytics, sélectionnez **[!UICONTROL prodView.]**

1. Dans la section Variables de couche **[!UICONTROL de données pour les données]** de produit, utilisez le sélecteur d’élément de données pour choisir l’élément de `Product Id` données.

1. Click **[!UICONTROL Keep Changes]**

   ![Ajout de la variable de chaîne de produit à l’aide de l’extension de chaîne de produit Adobe Analytics](images/analytics-PDPaddProductString.png)

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Enregistrez la règle](images/analytics-PDP-saveRule.png)

### Valider les données de la page Détails du produit

Vous venez de créer une règle qui définit les variables avant l’envoi de la balise. Vous devriez maintenant être en mesure de voir les nouvelles données sortir dans l’accès dans le débogueur d’Experience Cloud.

**Pour valider les données de la page Détails du produit**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser
1. Accédez à une page de détails du produit
1. Cliquez sur l’icône Débogueur ![Ouvrez le débogueur](images/analytics-debuggerIcon.png) Experience Cloud pour ouvrir votre débogueur **[!UICONTROL Adobe Experience Cloud.]**
1. Cliquez sur l’onglet Analytics.
1. Développer votre suite de rapports
1. Notice the Product Detail Variables that are now in the debugger, namely that `eVar1` has been set to "product detail page", that the `Events` variable has been set to "event1" and "prodView", that the products variable is set with the product id of the product you are viewing, and that your Page Name is still set by the Analytics extension

   ![Validation de l’accès à la page](images/analytics-validatePDPvars.png)

## Envoyer une balise de lien de suivi

Lors du chargement d’une page, vous déclenchez généralement une balise de chargement de page déclenchée par la fonction `s.t()`. This automatically increments a `page view` metric for the page listed in the `pageName` variable.

Cependant, il arrive que vous ne souhaitiez pas incrémenter les pages vues sur votre site, car l’action en cours est "plus petite" (ou peut-être simplement différente) qu’une page vue. In this case, you will use the `s.tl()` function, which is commonly referred to as a "track link" request. Bien qu’elle soit qualifiée de requête de lien de suivi, il n’est pas nécessaire de la déclencher via un clic sur les liens. It can be triggered by *any* of the events that are available to you in the Launch rule builder, including your own custom JavaScript.

Dans ce didacticiel, vous déclencherez un `s.tl()` appel à l’aide de l’un des événements JavaScript les plus cool, un `Enters Viewport` événement.

### Le cas d’utilisation

Pour ce cas d'utilisation, vous voulez savoir si les gens font défiler la page d'accueil Luma assez loin pour voir la section *Nouveautés* de notre page. Notre société est en désaccord interne sur le point de savoir si les gens voient cette section ou non. Vous souhaitez donc utiliser Analytics pour déterminer la vérité.

### Création de la règle au lancement

1. Accédez à la section **[!UICONTROL Règles]** dans le volet de navigation supérieur, puis cliquez sur **[!UICONTROL Ajouter une règle.]**
   ![Ajouter une règle](images/target-addRule.png)
1. Attribuez un nom à la règle `Homepage - New Arrivals enters Viewport`
1. Click **[!UICONTROL Events &gt; Add]** to open the `Event Configuration` screen

   ![Ajouter une nouvelle règle d'arrivée](images/analytics-newArrivalsRuleAdd2.png)

1. Sélectionnez **[!UICONTROL Type d’événement &gt; Entrent dans la fenêtre d’affichage]**. Cela ouvre un champ dans lequel vous devez entrer dans le sélecteur CSS qui identifie l’élément de votre page qui doit déclencher la règle lorsqu’il entre dans la vue dans le navigateur.
1. Revenez à la page d'accueil de Luma et faites défiler la page jusqu'à la section Nouveautés.
1. Cliquez avec le bouton droit de la souris sur l’espace entre le titre « NEW ARRIVALS » et les éléments de cette section, puis sélectionnez `Inspect` dans le menu contextuel. Ça vous rapprochera de ce que vous voulez.
1. Tout autour, peut-être juste sous la section sélectionnée, vous cherchez une div avec `class="we-productgrid aem-GridColumn aem-GridColumn--default--12"`. Localisez cet élément.
1. Cliquez avec le bouton droit sur cet élément et sélectionnez **[!UICONTROL Copier &gt; Copier le sélecteur]**

   ![Configuration de l’événement Enters Viewport](images/analytics-copyElementSelector.png)

1. Revenez à Lancer, puis collez cette valeur du Presse-papiers dans le champ intitulé `Elements matching the CSS selector`.
   1. D’un côté, c’est à vous de décider comment identifier les sélecteurs CSS. Cette méthode est un peu fragile, car certaines modifications de la page peuvent rompre ce sélecteur. Tenez compte de ce point lors de l’utilisation de sélecteurs CSS au lancement.
1. Click **[!UICONTROL Keep Changes]**
   ![Configuration de l’événement Enters Viewport](images/analytics-configEntersViewportEvent.png)

1. Sous Conditions, cliquez sur l’icône ![](images/icon-plus.png) Cliquez sur Plus pour ajouter une nouvelle condition.
1. Sélectionnez Type de **[!UICONTROL condition &gt; Comparaison des valeurs.]**
1. Use the data element picker, choose `Page Name` in the first field
1. Sélectionnez **[!UICONTROL Est égal]** dans la liste déroulante des opérateurs de comparaison.
1. Dans le type de champ suivant `content:we-retail:us:en` (il s’agit du nom de page de la page d’accueil tel qu’il est extrait de la couche de données - nous voulons seulement que cette règle s’exécute sur la page d’accueil)
1. Click **[!UICONTROL Keep Changes]**

   ![Configuration de la condition de page d'accueil](images/analytics-configHomepageCondition.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action
1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Analytics.]**
1. Sélectionnez Type **[!UICONTROL d’action &gt; Définir des variables.]**
1. Set `eVar3` to `Home Page - New Arrivals`
1. Set `prop3` to `Home Page - New Arrivals`
1. Set the `Events` variable to `event3`
1. Click **[!UICONTROL Keep Changes]**

   ![Configuration de l’événement Enters Viewport](images/analytics-configViewportAction.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add another new action

   ![Ajouter une action Envoyer la balise](images/analytics-newArrivalsSendBeacon2.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Analytics.]**
1. Sélectionnez **[!UICONTROL Action Type &gt; Envoyer la balise]**
1. Choose the **[!UICONTROL s.tl()]** tracking option
1. Dans le champ Nom **[!UICONTROL du]** lien, saisissez `Scrolled down to New Arrivals`. Cette valeur sera placée dans le rapport Liens personnalisés dans Analytics.
1. Click **[!UICONTROL Keep Changes]**

   ![Configurer la balise des nouvelles arrivées](images/analytics-configEntersViewportBeacon.png)

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Enregistrez la règle et générez](images/analytics-saveCustomLinkRule.png)

### Validation de la balise de lien de suivi

Vous devez maintenant vous assurer que cet accès se déroule lorsque vous faites défiler l’écran jusqu’à la section Nouveautés de la page d’accueil de notre site. Lorsque vous chargez la page d’accueil pour la première fois, la requête ne doit pas être effectuée, mais lorsque vous faites défiler la page et que la section apparaît, l’accès doit se déclencher avec nos nouvelles valeurs.

1. Ouvrez le site [](https://luma.enablementadobe.com/content/luma/us/en.html) Luma dans votre navigateur Chrome et assurez-vous que vous êtes en haut de la page d’accueil.
1. Cliquez sur l’icône **[!UICONTROL de]** débogueur ![Ouvrez le débogueur](images/analytics-debuggerIcon.png) Experience Cloud pour ouvrir votre débogueur [!UICONTROL Adobe Experience Cloud.]
1. Cliquez sur l’onglet Analytics.
1. Développer l’accès à votre suite de rapports
1. Notez l’accès normal à la page vue pour la page d’accueil avec le nom de la page, etc. (mais rien dans eVar3 ou prop3).

   ![Débogueur avec une page vue](images/analytics-debuggerPageView.png)

1. En laissant le débogueur ouvert, faites défiler votre site jusqu’à ce que vous puissiez voir la section Nouveautés
1. Affichez de nouveau le débogueur et un autre accès Analytics aurait dû s’afficher. Les paramètres associés à l’accès s.tl() que vous avez configuré doivent être les suivants :
   1. `LinkType = "link_o"` (cela signifie que l’accès est un accès à un lien personnalisé, pas un accès à une page vue)
   1. `LinkName = "Scrolled down to New Arrivals"`
   1. `prop3 = "Home Page - New Arrivals"`
   1. `eVar3 = "Home Page - New Arrivals"`
   1. `Events = "event3"`

      ![Débogueur avec une page vue](images/analytics-debuggerEntersViewport.png)

## Ajout d’un module externe

Un module externe est un élément de code JavaScript que vous pouvez ajouter à votre implémentation pour exécuter une fonction spécifique qui n’est pas intégrée au produit. Les modules externes peuvent être créés par vous, par d’autres clients/partenaires Adobe ou par le service de conseil d’Adobe.

Pour mettre en oeuvre des plug-ins, il existe essentiellement trois étapes :

1. Inclure la fonction doPlugins, où le module sera référencé
1. Ajout du code de fonction principal du module externe
1. Inclure le code qui appelle la fonction et qui définit des variables, etc.

### Rendre l’objet Analytics globalement accessible

Si vous souhaitez ajouter la fonction doPlugins (ci-dessous) et utiliser des plug-ins, vous devez cocher une case pour rendre l’objet "s" Analytics disponible globalement dans la mise en oeuvre Analytics.

1. Accédez à **[!UICONTROL Extensions &gt; Installé]**

1. Dans l’extension Adobe Analytics, cliquez sur **[!UICONTROL Configurer.]**

   ![Configuration d’Analytics](images/analytics-configureExtension.png)

1. Sous Gestion des **[!UICONTROL bibliothèques]**, sélectionnez la zone `Make tracker globally accessible`. Comme vous pouvez le voir dans la bulle d'aide, le suivi sera étendu globalement sous window.s, ce qui sera important lorsque vous y faites référence dans le code JavaScript de votre client.

### Inclusion de la fonction doPlugins

Pour ajouter des plug-ins, vous devez ajouter une fonction appelée doPlugins. Cette fonction n’est pas ajoutée par défaut, mais une fois ajoutée, elle est gérée par la bibliothèque AppMeasurement et appelée en dernier lorsqu’un accès est envoyé dans Adobe Analytics. Par conséquent, vous pouvez utiliser cette fonction pour exécuter certains codes JavaScript afin de définir des variables plus facilement définies de cette manière.

1. While still in the Analytics extension, scroll down and expand the section titled `Configure Tracker Using Custom Code.`
1. Click **[!UICONTROL Open Editor]**
1. Collez le code suivant dans l’éditeur de code :

   ```javascript
   /* Plugin Config */
   s.usePlugins=true
   s.doPlugins=function(s) {
   /* Add calls to plugins here */
   }
   ```

1. Laissez cette fenêtre ouverte pour l’étape suivante

### Ajouter un code de fonction pour le plug-in

Vous allez en fait appeler deux plug-ins dans ce code, mais l'un d'eux est intégré dans la bibliothèque AppMeasurement, donc pour celui-ci vous n'avez pas besoin d'ajouter la fonction à appeler. Cependant, pour la seconde, vous devez également ajouter le code de fonction. Cette fonction s’appelle getValOnce().

### Module getValOnce()

Ce plug-in a pour but de préserver la duplication erronée des valeurs dans le code lorsqu’un visiteur actualise une page ou utilise le bouton Retour du navigateur pour revenir à une page où une valeur a été définie. Dans cette leçon, vous l’utiliserez pour empêcher la duplication de l’ `clickthrough` événement.

Le code de ce plug-in est disponible dans la [documentation d’Analytics](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/plugins/getvalonce.html), mais il est inclus ici pour faciliter le copier-coller.

1. Copiez le code suivant.

   ```javascript
   /*
   * Plugin: getValOnce_v1.11
   */
   s.getValOnce=new Function("v","c","e","t",""
   +"var s=this,a=new Date,v=v?v:'',c=c?c:'s_gvo',e=e?e:0,i=t=='m'?6000"
   +"0:86400000,k=s.c_r(c);if(v){a.setTime(a.getTime()+e*i);s.c_w(c,v,e"
   +"==0?0:a);}return v==k?'':v");
   ```

1. Collez-le dans la fenêtre de code de l’extension Analytics (si vous ne l’avez pas encore ouvert, rouvrez-le selon l’étape précédente), **complètement sous** la fonction doPlugins (et non à l’intérieur).

   ![Ajouter un code de module externe](images/analytics-doPluginsAndGeValOnceCode.png)

Vous pouvez maintenant appeler ce module depuis doPlugins.

### Appel de plug-ins au sein de doPlugins

Maintenant que le code est présent et qu’il peut être référencé, vous pouvez appeler les modules externes dans la fonction doPlugins.

Tout d’abord, vous appellerez un module externe qui a été intégré à la bibliothèque AppMeasurement et qui est donc appelé "utilitaire". It is referred to as `s.Util.getQueryParam`, because it is part of the s object, is a built-in utility, and will grab values (based on a parameter) from the query string in the URL.

1. Copiez le code suivant :

   ```javascript
   s.campaign = s.Util.getQueryParam("cid");
   ```

1. Collez-le dans la fonction doPlugins. This will look for a parameter called `cid` in the current page URL and place it into the s.campaign variable.
1. Appelez maintenant la fonction getValOnce en copiant le code suivant et en le collant juste en dessous de l’appel à getQueryParam :

   ```javascript
   s.campaign=s.getValOnce(s.campaign,'s_cmp',30);
   ```

   Ce code s’assure que la même valeur n’est pas envoyée plus d’une fois de suite pendant 30 jours (voir la documentation pour savoir comment personnaliser ce code selon vos besoins).

   ![Appeler des plug-ins dans doPlugins](images/analytics-doPluginsWithPlugins.png)

1. Enregistrez la fenêtre de code
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Appeler des plug-ins dans doPlugins](images/analytics-saveExtensionAndBuild.png)

### Validation des plug-ins

Vous pouvez désormais vous assurer que les plug-ins fonctionnent.

**Pour valider les modules externes**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html) in your Chrome browser
1. Cliquez sur l’icône Débogueur ![Ouvrez le débogueur](images/analytics-debuggerIcon.png) Experience Cloud pour ouvrir le débogueur **[!UICONTROL Adobe Experience Cloud.]**
1. Cliquez sur l’onglet Analytics.
1. Développer votre suite de rapports
1. Notez que l’accès Analytics ne comporte pas de variable de campagne.
1. Leaving the Debugger open, go back to the Luma site and add  `?cid=1234` to the URL and hit Enter to refresh the page with that query string included

   ![Ajouter une chaîne de requête](images/analytics-cidAdded.png)

1. Check the Debugger and confirm that there is a second Analytics request with a Campaign variable set to `1234`

   ![getQueryParam, étape 1](images/analytics-getQueryParam1.png)

1. Revenez en arrière et actualisez à nouveau la page Luma, avec la chaîne de requête toujours dans l’URL.
1. Vérifiez l’accès suivant dans le débogueur et la variable Campagne **ne doit pas** être présente, car le module getValOnce s’est assuré qu’il n’est pas dupliqué et qu’il ressemble à une autre personne issue du code de suivi de campagne.

   ![getQueryParam, étape 1](images/analytics-getQueryParam2.png)

1. BONUS: You can test this over and over by changing the value of the `cid` parameter in the query string. The Campaign variable should only be there if it is the **first** time you run the page with the value. If you are not seeing the Campaign value in the debugger, simply change the value of the `cid` in the query string of the URL, hit enter, and you should see it again in the debugger.

   >[!NOTE] Il existe en fait plusieurs manières de retirer un paramètre de la chaîne de requête de l’URL, y compris dans la configuration de l’extension Analytics. Cependant, dans ces autres options non plug-in, elles ne permettent pas d'arrêter la duplication inutile, comme vous l'avez fait ici avec le plug-in getValOnce. Il s’agit de la méthode préférée de l’auteur, mais vous devez déterminer quelle méthode fonctionne le mieux pour vous et vos besoins.

Beau travail ! Vous avez terminé la leçon Analytics. Bien sûr, vous pouvez faire bien d’autres choses pour améliorer notre mise en oeuvre Analytics, mais j’espère que cela vous a donné certaines des compétences essentielles dont vous aurez besoin pour répondre au reste de vos besoins.

[Suivant : "Ajouter Adobe Audience Manager" &gt;](audience-manager.md)
