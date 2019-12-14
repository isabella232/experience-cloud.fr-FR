---
title: Mise en oeuvre de Target avec lancement
description: Découvrez comment implémenter Adobe Target à l’aide de Launch with at.js, d’une requête de chargement de page, de paramètres, d’une requête de commande et d’un code d’en-tête/de pied de page personnalisé. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-title: Mise en oeuvre de Target avec lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajout d’Adobe Target

Dans cette leçon, nous allons mettre en oeuvre l’extension [](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/target-extension/overview.html) Adobe Target avec une requête de chargement de page et des paramètres personnalisés.

[Adobe Target](https://docs.adobe.com/content/help/en/target/using/target-home.html) est la solution Adobe Marketing Cloud qui fournit tout ce dont vous avez besoin pour personnaliser l’expérience de vos clients, afin que vous puissiez optimiser les recettes sur vos sites Web et mobiles, vos applications, vos médias sociaux et d’autres canaux numériques.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Ajoutez le fragment de code prémasqué utilisé pour gérer le scintillement lors de l’utilisation de Target avec des codes incorporés de lancement asynchrones.
* Ajout de l’extension Target v2
* Déclenchez la demande de chargement de page (anciennement appelée "mbox globale").
* Ajout de paramètres à la requête de chargement de page
* Expliquer comment ajouter des paramètres de profil et d’entité à la demande de chargement de page
* Lancer la demande de confirmation de commande avec les paramètres requis
* Explique comment ajouter des configurations avancées telles que l’en-tête de bibliothèque et le code de pied de page de bibliothèque
* Validation d’une implémentation Target

## Conditions préalables 

To complete the lessons in this section, you must first complete the lessons in [Configure Launch](launch.md) and [Add the Identity Service](id-service.md).

## Ajout du fragment de code de prémasquage de Target

Avant de commencer, nous devons mettre à jour légèrement les codes d’intégration de lancement. Lorsque les codes d’intégration de lancement sont chargés de manière asynchrone, il se peut que la page termine le rendu avant que la bibliothèque Target ne soit complètement chargée et ait effectué l’échange de contenu. Cela peut conduire à ce que l’on appelle le "scintillement", où le contenu par défaut s’affiche brièvement avant d’être remplacé par le contenu personnalisé spécifié par Target. Si vous souhaitez éviter ce scintillement, nous vous recommandons vivement de figer un fragment de code prémasqué spécial juste avant les codes incorporés asynchrones de Launch.

Cela a déjà été fait sur le site Luma, mais allons de l'avant et faisons-le sur l'exemple de page pour que vous compreniez l'implémentation. Copiez les lignes de code suivantes :

```html
<script>
    //prehiding snippet for Adobe Target with asynchronous Launch deployment
    (function(g,b,d,f){(function(a,c,d){if(a){var e=b.createElement("style");e.id=c;e.innerHTML=d;a.appendChild(e)}})(b.getElementsByTagName("head")[0],"at-body-style",d);setTimeout(function(){var a=b.getElementsByTagName("head")[0];if(a){var c=b.getElementById("at-body-style");c&&a.removeChild(c)}},f)})(window,document,"body {opacity: 0 !important}",3E3);
</script>
```

Ouvrez l’exemple de page et collez-le juste avant votre code d’intégration Lancer comme illustré ci-dessous (ne vous inquiétez pas si les numéros de ligne sont différents) :
Passez![la souris sur l’extension](images/target-prehidingSnippet.png)

Rechargez votre exemple de page. Vous remarquerez que la page sera masquée pendant trois secondes avant de s’afficher. Ce comportement est temporaire et disparaîtra une fois Target déployé. Ce comportement de prémasquage est contrôlé par deux configurations à la toute fin du fragment de code, qui peuvent être personnalisées mais qui sont généralement préférables dans les paramètres par défaut :

* `body {opacity: 0 !important}` spécifie la définition CSS à utiliser pour le prémasquage jusqu’au chargement de Target. Par défaut, le corps entier sera masqué. Si vous disposez d’une structure DOM cohérente avec un élément conteneur facilement identifiable enveloppant tout le contenu sous votre navigation, par exemple, et que vous ne vouliez jamais tester ou personnaliser votre navigation, vous pouvez utiliser ce paramètre pour limiter le prémasquage à cet élément conteneur.
* `3E3` qui spécifie le paramètre de délai d’expiration pour le prémasquage. Par défaut, si Target n’a pas été chargé dans trois secondes, la page s’affiche. Cela devrait être extrêmement rare.

For more details and to obtain the un-minified pre-hiding snippet, please see [the Adobe Target extension with an asynchronous deployment&#x200B;](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/target-extension/overview.html#adobe-target-extension-with-an-asynchronous-deployment).

## Ajout de l’extension Target

L’extension Adobe Target prend en charge les implémentations côté client à l’aide du SDK JavaScript de Target pour le Web moderne, at.js. Customers still using Target's older library, mbox.js, [should upgrade to at.js 2.x](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/mbox-implement/migrate-mbox/target-atjs-implementation.html) in order to use Launch.

L’extension Target v2 se compose de deux parties principales :

1. La configuration de l’extension, qui gère les paramètres de bibliothèque principaux
1. Les actions de règle effectuent les opérations suivantes :
   1. Charger Target (at.js 2.x)
   1. Ajouter des paramètres aux requêtes de chargement de page
   1. Ajout de paramètres à toutes les requêtes (Ajout de paramètres à toutes les requêtes)
   1. Fire Page Load Request (Déclenchement de la requête pendant le chargement de la page)

Dans ce premier exercice, nous allons ajouter l'extension et examiner les configurations. Dans les exercices ultérieurs, nous utiliserons les actions.

**Pour ajouter l’extension**

1. Accéder à **[!UICONTROL Extensions &gt; Catalogue]**
1. Entrez `target` dans le filtre pour localiser rapidement les extensions Adobe Target. Il existe deux extensions : Adobe Target et Adobe Target v2. Ce didacticiel utilisera la version v2 de l’extension qui utilise la dernière version d’at.js (actuellement 2.x), idéale pour les sites Web traditionnels et les applications d’une seule page (SPA).
1. Cliquez sur **[!UICONTROL Installer]**

   ![Installation de l’extension Target v2](images/target-installExtension.png)

1. Lorsque vous ajoutez l’extension, elle importera de nombreux paramètres at.js, mais pas tous, de l’interface de Target, comme illustré ci-dessous. L’un des paramètres qui ne sera pas importé est le délai d’expiration, qui sera toujours de 3 000 ms après l’ajout de l’extension. Pour le tutoriel, conservez les paramètres par défaut. Notez que sur le côté gauche, il affiche la version d’at.js fournie avec la version actuelle de l’extension.

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Enregistrer l’extension](images/target-saveExtension.png)

À ce stade, Target ne fait vraiment rien, il n’y a donc rien à valider.

>[!NOTE] Chaque version de l’extension Target est fournie avec une version spécifique d’at.js, répertoriée dans la description de l’extension. Vous mettez à jour la version d’at.js en mettant à jour l’extension Target.

## Charger Target et déclencher la requête de chargement de page

Les marketeurs utilisent Target pour contrôler l’expérience des visiteurs sur la page lors du test et du ciblage du contenu. En raison de cet important rôle dans l’affichage de la page, vous devez charger Target le plus tôt possible afin de minimiser l’impact sur la visibilité de la page. Dans cette section, nous allons charger la bibliothèque JavaScript Target (at.js) et déclencher la demande de chargement de page (appelée "mbox globale" dans les versions antérieures d’at.js).

Vous pouvez utiliser la `All Pages - Library Loaded` règle que vous avez créée dans la leçon "[Ajouter des éléments de données, des règles et des bibliothèques](launch-data-elements-rules.md)" pour implémenter Target, car il est déjà déclenché le plus tôt possible au chargement de la page.

**Pour charger Target**

1. Accédez aux **[!UICONTROL règles]** dans le volet de navigation supérieur, puis cliquez sur `All Pages - Library Loaded` pour ouvrir l’éditeur de règles

   ![Ouvrir toutes les pages - Règle de chargement de bibliothèque](images/target-editRule.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![Cliquez sur l’icône Plus pour ajouter une nouvelle action.](images/target-addLoadTargetAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Target v2.]**

1. Sélectionnez **[!UICONTROL Action Type &gt; Charger la cible.]**

1. Click **[!UICONTROL Keep Changes]**

   ![Cliquez sur Conserver les modifications](images/target-addLoadTargetAction-keepChanges.png)

With the `Load Target` action added, at.js will load on the page. Toutefois, aucune requête Target ne se déclenchera tant que nous n’aurons pas ajouté l’ `Fire Page Load Request` action.

**Pour déclencher une demande de chargement de page**

1. Sous Actions, cliquez de nouveau sur l’icône ![](images/icon-plus.png) Cliquez de nouveau sur Plus pour ajouter une autre action.

   ![Cliquez sur l’icône Plus pour ajouter une autre action.](images/target-addGlobalMboxAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Target v2.]**

1. Sélectionnez Type **[!UICONTROL d’action &gt; Demande de chargement de page d’incendie]**

1. Certaines configurations sont disponibles pour la demande de chargement de page en fonction du masquage de la page et du sélecteur CSS à utiliser pour le prémasquage. Ces paramètres fonctionnent conjointement avec le code en dur du fragment de code caché en amont sur la page. Conservez les paramètres par défaut.

1. Click **[!UICONTROL Keep Changes]**

   ![Action de demande de chargement de page d’incendie](images/target-fireGlobalMbox.png)

1. La nouvelle action est ajoutée dans l’ordre après l’ `Load Target` action et les actions s’exécuteront dans cet ordre. Vous pouvez faire glisser et déposer les actions pour réorganiser l’ordre, mais dans ce scénario, `Load Target` vous devez le faire avant `Fire Page Load Request`.

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Enregistrer et créer](images/target-fireGlobalMbox-saveAndBuild.png)

### Valider la requête de chargement de page

Maintenant que vous avez ajouté l’extension Target v2 et déclenché les actions `Load Target` et `Fire Page Load Request` les actions, une demande de chargement de page doit être envoyée sur toutes les pages sur lesquelles votre propriété Launch est utilisée.

**Pour valider les actions Charger la cible et Charger la page d’incendie**

1. Rechargez votre exemple de page. Vous ne devriez plus voir de délai de trois secondes avant que la page ne soit visible. If you are loading the sample page using the `file://` protocol, you should do this step in Firefox or Safari browsers since Chrome will not fire a Target request when using the `file://` protocol.

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![L’environnement de développement de votre lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

1. Accédez à l’onglet Résumé du débogueur

1. In the `Launch` section, confirm that `Target` appears under the `Extensions` heading

1. Dans la `Target` section, vérifiez que la version de la bibliothèque at.js s’affiche.

   ![Vérifiez que Target apparaît dans l’onglet Résumé du débogueur.](images/target-summaryTab.png)

1. Enfin, accédez à l’ `Target` onglet, développez votre code client et vérifiez que votre demande de chargement de page s’affiche :

   ![Confirmer que la demande de chargement de page a été effectuée](images/target-debugger-globalMbox.png)

Félicitations ! Vous avez implémenté Target !

## Ajouter des paramètres

La transmission de paramètres dans la requête Target ajoute des fonctionnalités puissantes à vos activités de ciblage, de test et de personnalisation. L’extension Launch propose deux actions pour transmettre des paramètres :

1. `Add Params to Page Load Request`, qui ajoute des paramètres aux requêtes de chargement de page (équivalent à la méthode [targetPageParams()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/cmp-atjs-functions.html) )

1. `Add Params to All Requests`, qui ajoute des paramètres dans toutes les requêtes Target, par exemple la requête de chargement de page plus les requêtes supplémentaires effectuées à partir d’actions de code personnalisé ou codées en dur sur votre site (équivalent à la méthode [targetPageParamsAll()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/cmp-atjs-functions.html) )

These actions can be used *before* the `Load Target` action and can set different parameters on different pages based on your rule configurations. Utilisez la fonction d’ordre des règles que vous avez utilisée lors de la définition des ID de client avec Identity Service pour définir des paramètres supplémentaires sur l’ `Library Loaded` événement avant que la règle ne déclenche la demande de chargement de page.
>[!TIP] Comme la plupart des implémentations utilisent la requête de chargement de page pour la diffusion de l’activité, il suffit généralement d’utiliser l’ `Add Params to Page Load Requests` action.

### Paramètres de requête (mbox)

Les paramètres servent à transmettre des données personnalisées à Target, enrichissant ainsi vos capacités de personnalisation. Elles sont idéales pour les attributs qui changent fréquemment au cours d’une session de navigation, tels que le nom de la page, le modèle, etc. et ne persistent pas.

Ajoutons l’élément de `Page Name` données que nous avons créé plus tôt dans la leçon [Ajouter des éléments de données, des règles et des bibliothèques](launch-data-elements-rules.md) comme paramètre de requête.

**Pour ajouter le paramètre de requête**

1. Accédez aux **[!UICONTROL règles]** dans le volet de navigation supérieur, puis cliquez sur `All Pages - Library Loaded` pour ouvrir l’éditeur de règles.

   ![Ouvrir toutes les pages - Règle de chargement de bibliothèque](images/target-editRule.png)

1. Under Actions, click the ![Click the Plus icon](images/icon-plus.png) to add a new action

   ![Cliquez sur l’icône Plus pour ajouter une nouvelle action.](images/target-addParamsAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Target v2.]**

1. Sélectionnez **[!UICONTROL Action Type &gt; Ajouter des paramètres à la requête de chargement de page]**

1. Saisissez `pageName` comme **[!UICONTROL nom]**

1. Click the ![data element icon](images/icon-dataElement.png) to open the data element modal

1. Cliquez sur l’élément `Page Name` de données

1. Cliquez sur le bouton **[!UICONTROL Sélectionner]** .

   ![Cliquez sur le bouton Sélectionner](images/target-mboxParam-pageName.png)

1. Click **[!UICONTROL Keep Changes]**

   ![Cliquez sur Conserver les modifications](images/target-addPageName-keepChanges.png)

1. Cliquez et faites glisser sur le bord gauche de l’ `Add Params to Page Load Request` action pour réorganiser les actions qu’elle est avant ou après l’ `Fire Page Load Request` action (elle peut être avant ou après `Load Target`).

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Cliquez sur Enregistrer dans la bibliothèque et créer](images/target-rearrangeActions.png)

#### Validation des paramètres de requête

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

**Pour valider le paramètre de requête pageName**

1. Rechargez le site Luma, en veillant à ce qu’il soit mappé sur votre propre propriété Launch.
1. Ouvrez les outils de développement de votre navigateur
1. Cliquez sur l’onglet Réseau
1. Filtrez les requêtes vers `tt.omtrdc` (ou votre domaine CNAME pour les requêtes Target)
1. Développez la section `Headers` &gt; `Request Payload` &gt; `execute.pageLoad.parameters` pour valider le `pageName` paramètre et la valeur
   ![paramètre pageName dans le débogueur](images/target-debugger-pageName-browser.png)

<!--Now go to the **[!UICONTROL Target]** tab in the Debugger. Expand your client code and look at the requests. You should see the new `pageName` parameter passed in the request:

![pageName parameter in the debugger](images/target-debugger-pageName.png)-->

### Paramètres de profil

Tout comme les paramètres de requête, les paramètres de profil sont également transmis via la requête Target. However, profile parameters get stored in Target's visitor profile database and will persist for the [duration of the visitor's profile](https://docs.adobe.com/content/help/en/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html). Vous pouvez les définir sur une page de votre site et les utiliser dans les activités Target sur une autre page. Voici un exemple issu d’un site web automobile. Lorsqu’un visiteur accède à la page d’un véhicule, vous pouvez transmettre un paramètre de profil "profile.lastViewed=sportscar" afin d’enregistrer son intérêt pour ce véhicule particulier. Lorsque le visiteur accède à d’autres pages, autres que des pages de véhicules, vous pouvez cibler le contenu en fonction de son dernier véhicule consulté.  Les paramètres de profil sont idéaux pour les attributs qui changent rarement ou ne sont disponibles que sur certaines pages

You won't pass any profile parameters in this tutorial, but the workflow is almost identical to what you just did when passing the `pageName` parameter. La seule différence est que vous devez attribuer un préfixe `profile.` aux paramètres de nom du profil. This is what a profile parameter called "userType" would look like in the `Add Params to Page Load Request` action:

![Définition d’un paramètre de profil](images/target-profileParameter.png)

### Paramètres d’entité

Les paramètres d’entité sont des paramètres spéciaux utilisés dans les [Mises en œuvre des recommandations](https://docs.adobe.com/content/help/en/target/using/recommendations/plan-implement.html) pour trois raisons principales :

1. Comme clé pour déclencher des recommandations de produit. Par exemple, lorsque vous utilisez un algorithme de recommandations comme « Les personnes qui ont consulté le produit X ont également consulté Y », « X » est la « clé » de la recommandation. Il s’agit généralement du SKU (`entity.id`) ou de la catégorie (`entity.categoryId`) du produit que le visiteur est en train de consulter.
1. Pour collecter le comportement des visiteurs dans des algorithmes de recommandations d’alimentation, tels que "Produits récemment consultés" ou "Produits les plus consultés"
1. Pour renseigner le catalogue des recommandations. Recommendations contient une base de données de tous les produits ou articles de votre site Web, de sorte qu’ils puissent être diffusés dans l’offre de recommandation. Par exemple, lorsque vous recommandez des produits, vous souhaitez généralement afficher des attributs tels que le nom (`entity.name`) et l’image (`entity.thumbnailUrl`) du produit. Certains clients renseignent leur catalogue à l’aide de flux d’arrière-plan, mais ils peuvent également être renseignés à l’aide de paramètres d’entité dans les demandes Target.

Vous n’avez pas besoin de transmettre de paramètres de profil dans ce didacticiel, mais le flux de travail est identique à ce que vous avez fait précédemment lors de la transmission du paramètre de `pageName` requête. Il vous suffit de donner au paramètre un nom précédé de "entity". et faites-le correspondre à l’élément de données approprié. Notez que certaines entités courantes ont des noms réservés qui doivent être utilisés (par exemple entity.id pour le sku de produit). This is what it would look like to set entity parameters in the `Add Params to Page Load Request` action:

![Ajout de paramètres d’entité](images/target-entityParameters.png)

### Ajouter des paramètres d’identifiant client

La collecte des identifiants de client avec le service d’identité Adobe Experience Platform facilite l’importation de données de gestion de la relation client dans Target à l’aide de la fonctionnalité Attributs [du](https://docs.adobe.com/content/help/en/target/using/audiences/visitor-profiles/working-with-customer-attributes.html) client d’Adobe Experience Cloud. It also enables [cross-device visitor stitching](https://docs.adobe.com/content/help/en/target/using/integrate/experience-cloud-device-co-op.html), allowing you to maintain a consistent user experience as your customers switch between their laptops and their mobile devices.

Il est impératif de définir l'ID du client dans l' `Set Customer IDs` action du service d'identité avant de déclencher la demande de chargement de page. Pour ce faire, assurez-vous de disposer des fonctionnalités suivantes sur votre site :

* L’ID de client doit être disponible sur la page avant le code incorporé de lancement.
* L’extension Adobe Experience Platform Identity Service doit être installée.
* You must use the `Set Customer IDs` action in a rule that fires at the "Library Loaded (Page Top)" event
* Use the `Fire Page Load Request` action in a rule that fires *after* the "Set Customer IDs" action

Dans la leçon précédente, [ajoutez le service](id-service.md)d’identité Adobe Experience Platform, vous avez créé la `All Pages - Library Loaded - Authenticated - 10` règle pour déclencher l’action "Définir l’ID du client". Comme cette règle a un `Order` paramètre de `10`, les identifiants de client sont définis avant que notre demande de chargement de page ne se déclenche de la `All Pages - Library Loaded` règle avec son `Order` paramètre de `50`. Vous avez donc déjà implémenté la collecte des identifiants de client pour Target !

#### Validation de l’ID de client

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

**Pour valider l’ID de client**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![L’environnement de développement de votre lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

1. Connectez-vous au site Luma à l’aide des informations d’identification `test@adobe.com`/`test`
1. Return to the [Luma homepage](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Ouvrez les outils de développement de votre navigateur
1. Cliquez sur l’onglet Réseau
1. Filtrez les requêtes vers `tt.omtrdc` (ou votre domaine CNAME pour les requêtes Target)
1. Développez la section `Headers` &gt; `Request Payload` &gt; `id.customerIds.0` pour valider les paramètres et la valeur d’ID de client :
   ![paramètres d’ID de client dans le débogueur](images/target-debugger-customerId-browser.png)
<!--
1. Open the Debugger
1. Go to the Target tab
1. Expand your client code
1. You should see parameters in the latest Target request for `vst.crm_id.id` and `vst.crm_id.authState`. `vst.crm_id.id` should have a value of the hashed email address and `vst.crm_id.authState` should have a value of `1` to represent `authenticated`. Note that `crm_id` is the `Integration Code` you specified in the Identity Service configuration and must align with the key you use in your [Customer Attributes data file](https://docs.adobe.com/content/help/en/core-services/interface/customer-attributes/t-crs-usecase.html):

![The Customer Id details should be visible as custom parameters in the Target request](images/target-debugger-customerId.png)-->

>[!WARNING] Le service d’identité Adobe Experience Platform vous permet d’envoyer plusieurs identifiants au service, mais seul le premier sera envoyé à Target.

### Ajout du paramètre de jeton de propriété

>[!NOTE] Il s’agit d’un exercice facultatif pour les clients Target Premium.

The property token is a reserved parameter used with the Target Premium [Enterprise User Permissions](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/property-channel.html) feature. Elle permet de définir différentes propriétés numériques afin que différents membres d’une organisation Experience Cloud puissent se voir attribuer des autorisations différentes sur chaque propriété. Par exemple, vous souhaitez peut-être qu’un groupe d’utilisateurs puisse configurer des activités Target sur votre site Web, mais pas dans votre application mobile.

Les propriétés de Target sont similaires aux propriétés de lancement et aux suites de rapports Analytics. Une entreprise dotée de plusieurs marques, sites Web et équipes marketing peut utiliser une propriété Target, une propriété de lancement et une suite de rapports Analytics différentes pour chaque site Web ou application mobile. Les propriétés de lancement sont différenciées par leur code incorporé, les suites de rapports Analytics par leur identifiant de suite de rapports et les propriétés Target sont différenciées par leur paramètre de jeton de propriété.

Le jeton de propriété est implémenté comme un paramètre de requête. Attribuez simplement un nom au paramètre "at_property" et collez-le dans la valeur fournie dans l’interface Target.  Si vous implémentez plusieurs sites avec une seule propriété Launch, vous pouvez gérer la valeur at_property au moyen d’un élément de données.

Voici un exercice facultatif, si vous êtes un client Target Premium et souhaitez implémenter un jeton de propriété dans votre propriété Didacticiel :

1. Dans un onglet distinct, ouvrez l’interface utilisateur de Target.

1. Accédez à **[!UICONTROL Configuration &gt; Propriétés]**

1. Identifiez la propriété à utiliser et cliquez sur le **[!UICONTROL &lt;/&gt;]** (ou créez une nouvelle propriété).

1. Copy the `at_property` value to your clipboard

   ![Obtention du jeton de propriété à partir de l’interface Adobe Target](images/target-addATProperty-targetProperties.png)

1. In your Launch tab, go to the **[!UICONTROL Rules]** in the top navigation and then click on `All Pages - Library Loaded` to open the rule editor.

   ![Ouvrir toutes les pages - Règle de chargement de bibliothèque](images/target-editRule.png)

1. Sous Actions, cliquez sur l’ `Adobe Target - Add Params to Page Load Request` action pour ouvrir le `Action Configuration`

   ![Ouvrez l’action Ajouter des paramètres à la demande de chargement de page](images/target-openParamsAction.png)

1. Sous le `pageName` paramètre, cliquez sur le bouton **[!UICONTROL Ajouter]** .

   ![Ouvrez l’action Ajouter des paramètres à la demande de chargement de page](images/target-addATProperty.png)

1. Name the parameter `at_property` and paste in the value you copied from the Target interface

1. Click **[!UICONTROL Keep Changes]**

   ![Cliquez sur Conserver les modifications](images/target-addATProperty-keepChanges.png)

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**
   ![Cliquez sur Enregistrer et créer dans la bibliothèque](images/target-addATProperty-save.png)

#### Valider le jeton de propriété

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

**Pour valider le paramètre Jeton de propriété**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)
1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![L’environnement de développement de votre lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

1. Ouvrez les outils de développement de votre navigateur
1. Cliquez sur l’onglet Réseau
1. Filtrez les requêtes vers `tt.omtrdc` (ou votre domaine CNAME pour les requêtes Target)
1. Développez la section `Headers` &gt; `Request Payload` &gt; `property.token` pour valider la valeur.
   ![Le jeton de propriété doit être visible en tant que paramètre at_property dans chaque requête.](images/target-debugger-atProperty-browser.png)

<!--
1. Go to the `Target` tab
1. Expand your client code
1. You should see the parameter for "at_property" in every page load request request as you browse the site:

![The Property Token should be visible as the at_property parameter in every request](images/target-debugger-atProperty.png)-->

## Ajouter des requêtes personnalisées

### Ajouter une demande de confirmation de commande

La demande de confirmation de commande est un type spécial de demande utilisé pour envoyer les détails de la commande à Target. L’inclusion de trois paramètres de requête spécifiques (orderId, orderTotal et productPurchasedId) transforme une requête Target ordinaire en une requête de commande. Outre la génération de rapports sur les recettes, la demande de commande effectue les opérations suivantes :

1. Déduplique les envois de commandes accidentels
1. Elle filtre les commandes extrêmes (toute commande dont le total était supérieur à trois écarts types par rapport à la moyenne).
1. Elle utilise un algorithme différent en arrière-plan pour calculer la fiabilité statistique.
1. Elle crée un rapport d’audit spécial et téléchargeable des détails de commande individuels.

La meilleure pratique consiste à utiliser et à commander une demande de confirmation dans tous les entonnoirs de commande, même sur les sites non commerciaux. Par exemple, les sites de génération de piste comportent généralement des entonnoirs de piste avec un « ID de piste » unique généré à la fin. Ces sites doivent implémenter une demande de commande, en utilisant une valeur statique (ex. "1") pour le paramètre orderTotal.

Les clients qui utilisent l’intégration Analytics pour Target (A4T) pour la plupart de leurs rapports doivent également mettre en oeuvre la demande de commande, car A4T n’est pas encore compatible avec les types d’activité tels que l’affectation automatique, la personnalisation automatisée et le ciblage automatique. En outre, la demande de commande est un élément essentiel dans les implémentations de Recommendations, qui permet d’activer les algorithmes en fonction du comportement d’achat.

La demande de confirmation de commande doit se déclencher à partir d’une règle qui n’est déclenchée que sur votre page ou événement de confirmation de commande. Il est souvent possible de le combiner avec une règle définissant l’événement d’achat Adobe Analytics. Il doit être configuré à l’aide de l’action Code personnalisé de l’extension de base, en utilisant les éléments de données appropriés pour définir les paramètres orderId, orderTotal et productPurchasedId.

Ajoutons les éléments de données et la règle dont nous avons besoin pour déclencher une demande de confirmation de commande sur le site Luma. Comme vous avez déjà créé plusieurs éléments de données, ces instructions seront abrégées.

**Pour créer l’élément de données pour l’ID de commande**

1. Cliquez sur Eléments **[!UICONTROL de]** données dans la barre de navigation supérieure.
1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**
1. Nommez l’élément de données `Order Id`
1. Sélectionnez **[!UICONTROL Data Element Type &gt; JavaScript Variable]**
1. Utilisez `digitalData.cart.orderId` comme `JavaScript variable name`
1. Check the `Clean text` option
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque]**(Nous ne créerons pas la bibliothèque tant que nous n'aurons pas apporté toutes les modifications à la demande de confirmation de commande).

**Pour créer l’élément de données pour le montant du panier**

1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**
1. Nommez l’élément de données `Cart Amount`
1. Sélectionnez **[!UICONTROL Data Element Type &gt; JavaScript Variable]**
1. Utilisez `digitalData.cart.cartAmount` comme `JavaScript variable name`
1. Check the `Clean text` option
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque]**

**Pour créer l’élément de données pour les SKU de panier (Target)**

1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**
1. Nommez l’élément de données `Cart SKUs (Target)`
1. Sélectionnez **[!UICONTROL Data Element Type &gt; Code personnalisé.]**
1. Pour Target, le SKU doivent être représentés par une liste séparée par des virgules. Ce code personnalisé reformate le tableau de couche de données dans le format approprié. Dans l’éditeur de code personnalisé, collez les éléments suivants :

   ```javascript
   var targetProdSkus="";
   for (var i=0; i<digitalData.cart.cartEntries.length; i++) {
     if(i>0) {
       targetProdSkus = targetProdSkus + ",";
     }
     targetProdSkus = targetProdSkus + digitalData.cart.cartEntries[i].sku;
   }
   return targetProdSkus;
   ```

1. Check the `Force lowercase value` option
1. Check the `Clean text` option
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque]**

Nous devons maintenant créer une règle pour déclencher la demande de confirmation de commande avec ces éléments de données comme paramètres sur la page de confirmation de commande.

**Pour créer la règle pour la page de confirmation de commande**

1. Click **[!UICONTROL Rules]** in the top navigation
1. Cliquez sur **[!UICONTROL Ajouter une règle]**
1. Attribuez un nom à la règle `Order Confirmation Page - Library Loaded - 60`
1. Cliquez sur **[!UICONTROL Evénements &gt; Ajouter.]**
   1. Sélectionnez **[!UICONTROL Type d’événement &gt; Bibliothèque chargée (Haut de page).]**
   1. Remplacez le `Order` par `60` afin qu’il se déclenche après l’ `Load Target` action (qui se trouve dans notre `All Pages - Library Loaded` règle où `Order` est défini sur `50`).
   1. Click **[!UICONTROL Keep Changes]**
1. Cliquez sur **[!UICONTROL Conditions &gt; Ajouter.]**
   1. Sélectionnez Type **[!UICONTROL de condition &gt; Chemin sans chaîne de requête.]**
   1. Pour `Path equals` entrer `thank-you.html`
   1. Activez l’option Régex pour modifier la logique de `equals` à `contains` (vous pouvez utiliser la `Test` fonction pour confirmer que le test réussira avec l’URL). `https://luma.enablementadobe.com/content/luma/us/en/user/checkout/order/thank-you.html`

      ![Saisir des valeurs factices pour le prénom et le nom](images/target-orderConfirm-test.png)

   1. Click **[!UICONTROL Keep Changes]**
1. Cliquez sur **[!UICONTROL Actions &gt; Ajouter.]**
   1. Sélectionnez **[!UICONTROL Action Type &gt; Code personnalisé]**
   1. Click **[!UICONTROL Open Editor]**
   1. Paste the following code into the `Edit Code` modal

      ```javascript
      adobe.target.getOffer({
        "mbox": "orderConfirmPage",
        "params":{
           "orderId": _satellite.getVar('Order Id'),
           "orderTotal": _satellite.getVar('Cart Amount'),
          "productPurchasedId": _satellite.getVar('Cart SKUs (Target)')
        },
        "success": function(offer) {
          adobe.target.applyOffer({
            "mbox": "orderConfirmPage",
            "offer": offer
          });
        },
        "error": function(status, error) {
          console.log('Error', status, error);
        }
      });
      ```

   1. Click **[!UICONTROL Save]** to save the custom code
   1. Cliquez sur **[!UICONTROL Conserver les modifications]** pour conserver l’action.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

#### Valider la demande de confirmation de commande

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![L’environnement de développement de votre lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

1. Parcourez le site et ajoutez plusieurs produits à votre panier
1. Passez à la caisse
1. During the checkout process the only required fields are `First Name` and `Last Name`

   ![Saisir des valeurs factices pour le prénom et le nom](images/target-testOrderCart.png)

1. On the Review Order page, be sure to click the `Place Order` button
1. Ouvrez les outils de développement de votre navigateur
1. Cliquez sur l’onglet Réseau
1. Filtrez les requêtes vers `tt.omtrdc` (ou votre domaine CNAME pour les requêtes Target)
1. Cliquez sur la seconde requête
1. Développez la section `Headers` &gt; `Request Payload` &gt; `execute.mboxes.0` pour valider le nom de la requête et les paramètres de commande :
   ![paramètres de demande de commande dans le débogueur](images/target-debugger-orderConfirmPage-browser.png)
<!--
1. Look in the Debugger
1. Go to the Target tab
1. Expand your client code
1. You should see the `orderConfirmPage` request as the latest Target request with the orderId, orderTotal, and productPurchasedId parameters populated with the details of your order

   ![orderConfirmPage request with required parameters](images/target-debugger-orderConfirmPage.png)-->

### Requêtes personnalisées

Il arrive parfois que vous deviez effectuer des requêtes Target autres que le chargement de la page et la demande de confirmation de commande. Par exemple, il arrive que des données importantes que vous souhaitez utiliser pour la personnalisation ne soient pas définies sur la page avant le lancement des codes incorporés. Elles peuvent être codées en dur au bas de la page ou être renvoyées à partir d’une demande d’API asynchrone. Ces données peuvent toujours être envoyées à Target à l’aide d’une requête supplémentaire, bien qu’il ne soit pas optimal d’utiliser cette requête pour la diffusion de contenu puisque la page sera déjà visible. Il peut être utilisé pour enrichir le profil du visiteur en vue d’une utilisation ultérieure (à l’aide de paramètres de profil) ou pour renseigner le catalogue de recommandations.

In these circumstances, use the Custom Code action in the Core extension to fire a request using the
[getOffer()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/adobe-target-getoffer.html)/[applyOffer()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/adobe-target-applyoffer.html) and [trackEvent()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/adobe-target-trackevent.html)
methods. This is very similar to what you just did in the [Order
Confirmation request](#order-confirmation-request) exercise, but you will just use a different request name and will not use the special order parameters. Be sure to use the **[!UICONTROL Load Target]** action before making Target requests from custom code.

## En-tête de bibliothèque et pied de page de bibliothèque

L’écran Modifier at.js de l’interface utilisateur de Target comporte des emplacements dans lesquels vous pouvez coller du code JavaScript personnalisé qui s’exécutera immédiatement avant ou après le fichier at.js. The Library Header is sometimes used to override at.js settings via the
[targetGlobalSettings()](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html) function or pass data from third parties using the [Data Providers](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html) feature. Le pied de page de la bibliothèque est parfois utilisé pour ajouter des écouteurs d’événement [personnalisé](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html) at.js.

Pour répliquer cette fonctionnalité dans Lancement, il vous suffit d’utiliser l’action Code personnalisé dans l’extension principale et de séquencer l’action avant (En-tête de bibliothèque) ou après (Pied de page de bibliothèque) l’action Charger la cible. This can be done in the same rule as the `Load Target` action (as pictured below) or in separate rules with events or order settings that will reliably fire before or after the rule containing `Load Target`:

![En-tête et pied de page de bibliothèque dans la séquence d’actions](images/target-libraryHeaderFooter.png)

Pour en savoir plus sur les cas d’utilisation pour les en-têtes et pieds de page personnalisés, consultez les ressources suivantes :

* [Utilisation de dataProviders pour intégrer des données tierces dans Adobe Target](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [Implémentation de dataProviders pour intégrer des données tierces dans Adobe Target](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
* [Utilisation de jetons de réponse et d’événements personnalisés at.js avec Adobe Target](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-response-tokens-and-atjs-custom-events.html)

[Suivant : "Ajouter Adobe Analytics" &gt;](analytics.md)
