---
title: Mise en œuvre de Target avec Launch
description: Découvrez comment mettre en œuvre Adobe Target à l’aide de Launch avec at.js, une requête de chargement de page, des paramètres, une requête de commande et un code d’en-tête/de pied de page personnalisé. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les sites web avec Launch.
seo-title: Mise en œuvre de Target avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajout d’Adobe Target

Dans cette leçon, nous allons mettre en œuvre [l’extension Adobe Target](https://docs.adobe.com/content/help/fr-FR/launch/using/extensions-ref/adobe-extension/target-extension/overview.html) avec une requête de chargement de page et des paramètres personnalisés.

[Adobe Target](https://docs.adobe.com/content/help/fr-FR/target/using/target-home.html) est la solution d’Adobe Experience Cloud qui fournit tout ce dont vous avez besoin pour personnaliser l’expérience de vos clients afin de maximiser les recettes de vos sites web et mobiles, de vos applications, de vos médias sociaux et d’autres canaux numériques.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* ajouter le fragment de code de masquage préalable servant à gérer le scintillement lors de l’utilisation de Target avec des codes intégrés à Launch asynchrones ;
* ajouter l’extension Target v2 ;
* déclencher la requête de chargement de page (anciennement appelée « mbox générale » ou « mbox globale ») ;
* ajouter des paramètres à la requête de chargement de page ;
* expliquer comment ajouter des paramètres de profil et d’entité à la requête de chargement de page ;
* déclencher la requête de confirmation de commande avec les paramètres requis ;
* expliquer comment ajouter des configurations avancées telles que le code d’en-tête de bibliothèque et de pied de page de bibliothèque ;
* valider une mise en œuvre de Target.

## Conditions préalables

Pour terminer les exercices de cette section, vous devez d’abord suivre les leçons [Configuration de Launch](launch.md) et [Ajout d’Identity Service](id-service.md).

## Ajout du fragment de code de masquage préalable Target

Avant de commencer, nous devons faire une simple mise à jour des codes intégrés à Launch. Lorsque les codes intégrés à Launch sont chargés de manière asynchrone, la page peut terminer le rendu avant que la bibliothèque Target ne soit complètement chargée et ait effectué le remplacement de son contenu. Cela peut donner lieu à ce que l’on qualifie de « scintillement », où le contenu par défaut s’affiche brièvement avant d’être remplacé par le contenu personnalisé spécifié par Target. Si vous souhaitez éviter ce scintillement, nous vous recommandons vivement de figer un fragment de code de masquage préalable spécifique juste avant les codes intégrés asynchrones de Launch.

Cela a déjà été fait sur le site Luma, mais nous pouvons le faire sur la page d’exemple pour que vous compreniez cette manipulation. Copiez les lignes de code suivantes :

```html
<script>
    //prehiding snippet for Adobe Target with asynchronous Launch deployment
    (function(g,b,d,f){(function(a,c,d){if(a){var e=b.createElement("style");e.id=c;e.innerHTML=d;a.appendChild(e)}})(b.getElementsByTagName("head")[0],"at-body-style",d);setTimeout(function(){var a=b.getElementsByTagName("head")[0];if(a){var c=b.getElementById("at-body-style");c&&a.removeChild(c)}},f)})(window,document,"body {opacity: 0 !important}",3E3);
</script>
```

Ouvrez la page d’exemple et collez-les juste avant votre code intégré à Launch, comme illustré ci-dessous (ne vous inquiétez pas si les numéros de ligne sont différents) :
![Passez la souris sur l’extension](images/target-prehidingSnippet.png)

Rechargez votre page d’exemple. Vous remarquerez que la page sera masquée pendant trois secondes avant de s’afficher. Ce comportement est temporaire et disparaîtra une fois Target déployé. Ce comportement de masquage préalable est contrôlé par deux configurations à la fin du fragment de code, qui peuvent être personnalisées, mais qu’il convient généralement de conserver avec les paramètres par défaut :

* `body {opacity: 0 !important}` spécifie la définition CSS à utiliser pour le masquage préalable jusqu’au chargement de Target. Par défaut, la totalité du corps sera masquée. Si vous disposez d’une structure DOM cohérente avec un élément de conteneur facilement identifiable encapsulant tout le contenu sous votre navigation, par exemple, et que vous ne souhaitez jamais tester ou personnaliser votre navigation, vous pouvez utiliser ce paramètre pour limiter le masquage préalable à cet élément de conteneur.
* `3E3` spécifie le délai d’expiration pour le masquage préalable. Par défaut, si Target ne s’est pas chargé en trois secondes, la page s’affiche. Cette situation devrait être extrêmement rare.

Pour plus d’informations et pour obtenir le fragment de code de masquage préalable complet, voir [Extension Adobe Target avec un déploiement asynchrone](https://docs.adobe.com/content/help/fr-FR/launch/using/extensions-ref/adobe-extension/target-extension/overview.html#adobe-target-extension-with-an-asynchronous-deployment).

## Ajout de l’extension Target

L’extension Adobe Target prend en charge les mises en œuvre côté client à l’aide du SDK JavaScript de Target pour le Web moderne, at.js. Les clients qui utilisent encore l’ancienne bibliothèque Target, mbox.js, [doivent effectuer la mise à niveau vers at.js 2.x](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/mbox-implement/migrate-mbox/target-atjs-implementation.html) pour pouvoir utiliser Launch.

L’extension Target v2 se compose de deux parties principales :

1. La configuration de l’extension, qui gère les paramètres de bibliothèque principaux
1. Les actions de règle, qui effectuent les opérations suivantes :
   1. Charger Target (at.js 2.x)
   1. Ajouter des paramètres aux requêtes de chargement de page
   1. Ajouter des paramètres à toutes les requêtes
   1. Déclencher la requête de chargement de page

Dans ce premier exercice, nous allons ajouter l’extension et examiner les configurations. Dans les exercices ultérieurs, nous utiliserons les actions.

**Ajout de l’extension**

1. Accédez à **[!UICONTROL Extensions &gt; Catalogue]**.
1. Saisissez `target` dans le filtre pour localiser rapidement les extensions Adobe Target. Il existe deux extensions : Adobe Target et Adobe Target v2. Ce tutoriel utilisera la version v2 de l’extension, qui utilise la dernière version d’at.js (actuellement 2.x), idéale pour les sites web traditionnels et les applications d’une seule page (SPA).
1. Cliquez sur **[!UICONTROL Installer]**.

   ![Installation de l’extension Target v2](images/target-installExtension.png)

1. Comme illustré ci-dessous, lorsque vous ajoutez l’extension, elle importe de nombreux paramètres at.js de l’interface de Target,. Un paramètre qui n’est pas importé est Délai d’expiration, qui est toujours de 3 000 ms après l’ajout de l’extension. Pour le tutoriel, conservez les paramètres par défaut. Notez que la version d’at.js fournie avec la version actuelle de l’extension est indiquée sur le côté gauche.

1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et créer]**.

   ![Enregistrement de l’extension](images/target-saveExtension.png)

À ce stade, Target ne fait encore rien, il n’y a donc rien à valider.

>[!NOTE] Chaque version de l’extension Target est fournie avec une version spécifique d’at.js, indiquée dans la description de l’extension. En mettant à jour l’extension Target, vous mettez à jour la version d’at.js.

## Chargement de Target et déclenchement d’une requête de chargement de page

Les spécialistes du marketing utilisent Target pour contrôler l’expérience du visiteur sur la page lors du test et du ciblage du contenu. En raison de ce rôle important dans l’affichage de la page, Target doit être chargé le plus tôt possible afin de minimiser l’impact sur la visibilité de la page. Dans cette section, nous allons charger la bibliothèque JavaScript Target (at.js) et déclencher la requête de chargement de page (appelée « mbox générale » ou « mbox globale » dans les versions antérieures d’at.js).

Vous pouvez utiliser la règle `All Pages - Library Loaded` que vous avez créée dans la leçon « [Ajouter des éléments de données, des règles et des bibliothèques](launch-data-elements-rules.md) » pour mettre en œuvre Target, car il est déjà déclenché le plus tôt possible au chargement de la page.

**Chargement de Target**

1. Accédez à **[!UICONTROL règles]** dans le volet de navigation supérieur, puis cliquez sur `All Pages - Library Loaded` pour ouvrir l’éditeur de règles

   ![Ouverture de la règle chargée Bibliothèque - pages les Toutes](images/target-editRule.png)

1. Sous Actions, cliquez sur l’icône ![Plus](images/icon-plus.png) pour ajouter une nouvelle action.

   ![Cliquer sur l’icône Plus pour ajouter une nouvelle action](images/target-addLoadTargetAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Target v2]**.

1. Sélectionnez **[!UICONTROL Target Charger &gt; d’action Type]**.

1. Cliquez sur **[!UICONTROL modifications les Conserver]**.

   ![Cliquez sur modifications les Conserver](images/target-addLoadTargetAction-keepChanges.png)

Target Charger`Load Target` ajoutée, at.js se charge sur la page. Toutefois, aucune requête Target ne se déclenche tant que nous n’avons pas ajouté l’action `Fire Page Load Request`.

**Déclenchement de la requête de chargement de page**

1. Sous Actions, cliquez sur l’icône ![Plus](images/icon-plus.png) pour ajouter à nouveau une autre action.

   ![Cliquer sur l’icône Plus pour ajouter une autre action](images/target-addGlobalMboxAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Target v2]**.

1. Sélectionnez **[!UICONTROL page de chargement de requête la de Déclenchement &gt; d’action Type]**.

1. Il existe certaines configurations disponibles pour la requête de chargement de page associées au fait de masquer ou non la page et le sélecteur CSS à utiliser pour le masquage préalable. Ces paramètres fonctionnent conjointement avec le fragment de code de masquage préalable codé en dur sur la page. Conservez les paramètres par défaut.

1. Cliquez sur **[!UICONTROL Conserver les modifications]**.

   ![Déclenchement de la requête de chargement de page](images/target-fireGlobalMbox.png)

1. La nouvelle action est ajoutée dans l’ordre après `Load Target`et les actions s’exécuteront dans cet ordre. Vous pouvez glisser et déposer les actions pour réorganiser l’ordre, mais dans cette situation, `Load Target` doit être placé avant `Fire Page Load Request`.

1. Cliquez sur **[!UICONTROL créer et bibliothèque la dans Enregistrer]**.

   ![Enregistrement et création](images/target-fireGlobalMbox-saveAndBuild.png)

### Validation de la requête de chargement de page

Maintenant que vous avez ajouté l’extension Target v2 et déclenché les actions `Load Target` et `Fire Page Load Request`, une requête de chargement de page doit avoir été envoyée sur toutes les pages sur lesquelles votre propriété Launch est utilisée.

**Validation des actions Charger Target et Déclenchement de la requête de chargement de page**

1. Rechargez votre page d’exemple. Vous ne devriez plus voir de délai de trois secondes avant que la page ne soit visible. Si vous chargez la page d’exemple à l’aide du protocole `file://`, vous devez effectuer cette étape dans un navigateur Firefox ou Safari, car Chrome ne déclenche pas de requête Target lors de l’utilisation du protocole `file://`.

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Accédez à l’onglet Résumé du débogueur.

1. Dans la section `Launch`, vérifiez que `Target` s’affiche sous le titre `Extensions`.

1. Dans la section `Target`, vérifiez que la version de la bibliothèque at.js s’affiche.

   ![Vérifiez que Target apparaît dans l’onglet Résumé du débogueur.](images/target-summaryTab.png)

1. Enfin, accédez à l’onglet `Target`, développez votre code client et vérifiez que votre requête de chargement de page s’affiche :

   ![Vérifiez que la demande de chargement de page a été effectuée](images/target-debugger-globalMbox.png)

Félicitations ! Vous avez mis en œuvre Target !

## Ajout de paramètres

Le transfert de paramètres dans la requête Target ajoute de grandes possibilités à vos activités de ciblage, de test et de personnalisation. L’extension Launch propose deux actions permettant de transférer des paramètres :

1. `Add Params to Page Load Request` , qui ajoute des paramètres aux requêtes de chargement de page (équivalent à la méthode [targetPageParams()](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/cmp-atjs-functions.html))

1. `Add Params to All Requests` , qui ajoute des paramètres à toutes les requêtes Target, par exemple la requête de chargement de page plus les requêtes supplémentaires effectuées à partir d’actions de code personnalisé ou codées en dur sur votre site (équivalent à la méthode [targetPageParamsAll()](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/cmp-atjs-functions.html))

Ces actions peuvent être utilisées *avant* l’action `Load Target` et peuvent définir différents paramètres sur différentes pages en fonction des configurations de vos règles. Utilisez la fonction d’agencement des règles que vous avez utilisée lors de la définition des ID de client avec Identity Service pour définir des paramètres supplémentaires sur l’événement `Library Loaded` avant la règle qui déclenche la requête de chargement de page.
>[!TIP] Comme la plupart des mises en œuvre utilisent la requête de chargement de page pour la diffusion des activités, il suffit généralement d’utiliser l’action `Add Params to Page Load Requests`.

### Paramètres de requête (mbox)

Les paramètres servent à transmettre des données personnalisées à Target, enrichissant ainsi vos possibilités de personnalisation. Ils sont parfaits pour les attributs qui changent fréquemment au cours d’une session de navigation, tels que le nom de la page, le modèle, etc., et ne sont pas persistants.

Ajoutons l’élément de données `Page Name` que nous avons créé plus tôt dans la leçon [Ajout d’éléments de données, de règles et de bibliothèques](launch-data-elements-rules.md) comme paramètre de requête.

**Ajout du paramètre de requête**

1. Accédez aux **[!UICONTROL règles]** dans le volet de navigation supérieur, puis cliquez sur `All Pages - Library Loaded` pour ouvrir l’éditeur de règles.

   ![Toutes les pages - Bibliothèque chargée](images/target-editRule.png)

1. Sous Actions, cliquez sur l’icône ![Plus](images/icon-plus.png) pour ajouter une nouvelle action.

   ![Cliquer sur l’icône Plus pour ajouter une nouvelle action](images/target-addParamsAction.png)

1. Sélectionnez **[!UICONTROL Extension &gt; Adobe Target v2]**.

1. Sélectionnez **[!UICONTROL page de chargement de requête la à paramètres de Ajout &gt; d’action Type]**.

1. Saisissez `pageName` dans le champ **[!UICONTROL Nom]**.

1. Cliquez sur ![l’icône d’élément de données](images/icon-dataElement.png) pour ouvrir le modal des éléments de données.

1. Cliquez sur l’élément de données `Page Name`.

1. Cliquez sur le bouton **[!UICONTROL Sélectionner]**.

   ![Cliquez sur le bouton « Sélectionner »](images/target-mboxParam-pageName.png)

1. Cliquez sur **[!UICONTROL modifications les Conserver]**.

   ![Conserver les modifications](images/target-addPageName-keepChanges.png)

1. Cliquez et faites glisser sur le bord gauche l’action `Add Params to Page Load Request` de façon à ce qu’elle soit avant l’action `Fire Page Load Request`. Elle peut être avant ou après `Load Target`.

1. Cliquez sur **[!UICONTROL créer et bibliothèque la dans Enregistrer]**.

   ![Enregistrer dans la bibliothèque et créer](images/target-rearrangeActions.png).

#### Validation des paramètres de requête

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

**Validation du paramètre de requête pageName**

1. Chargez à nouveau le site Luma en veillant à ce qu’il soit mappé sur votre propre propriété Launch.
1. Ouvrez les outils de développement de votre navigateur.
1. Cliquez sur l’onglet Réseau.
1. Filtrez les requêtes sur `tt.omtrdc` (ou sur votre domaine CNAME pour les requêtes Target).
1. Développez la section `Headers` &gt; `Request Payload` &gt; `execute.pageLoad.parameters` pour valider le paramètre `pageName` et sa valeur.
   ![Paramètre pageName dans le débogueur](images/target-debugger-pageName-browser.png)

<!--Now go to the **[!UICONTROL Target]** tab in the Debugger. Expand your client code and look at the requests. You should see the new `pageName` parameter passed in the request:

![pageName parameter in the debugger](images/target-debugger-pageName.png)-->

### Paramètres de profil

Comme pour les paramètres de requête, les paramètres de profil sont transmis par le biais de la requête Target. Cependant, les paramètres de profil sont stockés dans la base de données du profil du visiteur Target et sont conservés pendant [toute la durée du profil du visiteur](https://docs.adobe.com/content/help/fr-FR/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html). Vous pouvez les définir sur une page de votre site et les utiliser dans des activités Target sur une autre page. Voici un exemple issu d’un site web automobile. Lorsqu’un visiteur accède à la page d’un véhicule, vous pouvez transmettre un paramètre de profil « profile.lastViewed=sportscar » pour enregistrer son intérêt pour ce véhicule particulier. Lorsque le visiteur accède à d’autres pages ne se rapportant pas à un véhicule, vous pouvez cibler le contenu en fonction du dernier véhicule qu’il a consulté.  Les paramètres de profil sont idéaux pour les attributs qui changent rarement ou ne sont disponibles que sur certaines pages.

Vous ne transmettrez aucun paramètre de profil dans ce tutoriel, mais le processus est presque identique à celui que vous avez réalisé au moment de transmettre le paramètre `pageName`. La seule différence est que vous devez attribuer un préfixe `profile.` aux paramètres de nom du profil. Voici à quoi ressemble un paramètre de profil appelé « userType » dans l’action `Add Params to Page Load Request`:

![Définition d’un paramètre de profil](images/target-profileParameter.png)

### Paramètres d’entité

Les paramètres d’entité sont des paramètres spéciaux utilisés dans les [Mises en œuvre des recommandations](https://docs.adobe.com/content/help/fr-FR/target/using/recommendations/plan-implement.html) pour trois raisons principales :

1. Comme clé pour déclencher des recommandations de produit. Par exemple, lorsque vous utilisez un algorithme de recommandations comme « Les personnes qui ont consulté le produit X ont également consulté Y », « X » est la « clé » de la recommandation. Il s’agit généralement du SKU (`entity.id`) ou de la catégorie (`entity.categoryId`) du produit que le visiteur est en train de consulter.
1. Pour collecter le comportement des visiteurs afin d’alimenter les algorithmes de recommandations, tels que « Produits récemment affichés » ou « Produits les plus consultés ».
1. Pour renseigner le catalogue des recommandations. Ce catalogue contient une base de données de tous les produits ou articles de votre site web, afin qu’ils puissent être diffusés dans l’offre de recommandations. Par exemple, lorsque vous recommandez des produits, vous souhaitez généralement afficher des attributs tels que le nom (`entity.name`) et l’image (`entity.thumbnailUrl`) du produit. Certains clients remplissent leur catalogue à l’aide de flux d’arrière-plan, mais ils peuvent également le faire à l’aide de paramètres d’entité dans des requêtes Target.

Vous n’avez pas besoin de transmettre de paramètres de profil dans ce tutoriel, mais le processus est identique à ce que vous avez fait précédemment lors de la transmission du paramètre de requête `pageName`. Il vous suffit de donner au paramètre un nom avec le préfixe « entity. » et de le mapper sur l’élément de données approprié. Notez que certaines entités courantes ont des noms réservés qui doivent être utilisés (par exemple, entity.id pour le SKU du produit). Voici à quoi pourrait ressembler la définition des paramètres d’entité dans l’action `Add Params to Page Load Request` :

![Ajout de paramètres d’entité](images/target-entityParameters.png)

### Ajout de paramètres d’ID de client

La collecte des ID de client avec Adobe Experience Platform Identity Service facilite l’importation de données depuis un CRM dans Target à l’aide de la fonctionnalité [Attributs du client](https://docs.adobe.com/content/help/fr-FR/target/using/audiences/visitor-profiles/working-with-customer-attributes.html) d’Adobe Experience Cloud. Elle favorise également [la connexité des visiteurs d’un appareil à l’autre](https://docs.adobe.com/content/help/fr-FR/target/using/integrate/experience-cloud-device-co-op.html), ce qui vous permet de maintenir une expérience utilisateur cohérente lorsque vos clients passent d’un ordinateur portable à leur appareil mobile.

Il est absolument nécessaire de définir l’ID de client dans l’action `Set Customer IDs` d’Identity Service avant de déclencher la demande de chargement de page. Pour ce faire, assurez-vous de disposer des fonctionnalités suivantes sur votre site :

* L’ID de client doit être disponible sur la page avant le code intégré à Launch.
* L’extension Adobe Experience Platform Identity Service doit être installée.
* Vous devez utiliser l’action `Set Customer IDs` dans une règle qui se déclenche à l’événement « (page la de Haut chargée Bibliothèque) ».
* Utilisez l’action `Fire Page Load Request` dans une règle qui se déclenche *après l’action* « client de ID des Définition ».

Dans la leçon précédente, [Ajout d’Adobe Experience Platform Identity Service](id-service.md), vous avez créé la règle `All Pages - Library Loaded - Authenticated - 10` pour déclencher l’action «c lient de ID des Définition ». Comme cette règle a un paramètre `Order` de `10`, les ID de client sont définis avant que notre demande de chargement de page ne se déclenche à partir de la règle `All Pages - Library Loaded` avec son paramètre `Order` de `50`. Vous avez donc déjà mis en œuvre la collecte des ID de client pour Target !

#### Validation de l’ID de client

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

**Validation de l’ID de client**

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Connectez-vous au site Luma à l’aide des informations d’identification suivantes : `test@adobe.com`/`test`
1. Revenez à la [page d’accueil de Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Ouvrez les outils de développement de votre navigateur.
1. Cliquez sur l’onglet Réseau.
1. Filtrez les requêtes sur `tt.omtrdc` (ou sur votre domaine CNAME pour les requêtes Target).
1. Développez la section `Headers` &gt; `Request Payload` &gt; `id.customerIds.0` pour valider les paramètres d’ID de client et leur valeur :
   ![Paramètres d’ID de client dans le débogueur](images/target-debugger-customerId-browser.png)
<!--
1. Open the Debugger
1. Go to the Target tab
1. Expand your client code
1. You should see parameters in the latest Target request for `vst.crm_id.id` and `vst.crm_id.authState`. `vst.crm_id.id` should have a value of the hashed email address and `vst.crm_id.authState` should have a value of `1` to represent `authenticated`. Note that `crm_id` is the `Integration Code` you specified in the Identity Service configuration and must align with the key you use in your [Customer Attributes data file](https://docs.adobe.com/content/help/en/core-services/interface/customer-attributes/t-crs-usecase.html):

![The Customer Id details should be visible as custom parameters in the Target request](images/target-debugger-customerId.png)-->

>[!WARNING] Adobe Experience Platform Identity Service vous permet d’envoyer plusieurs identifiants au service, mais seul le premier sera envoyé à Target.

### Ajout du paramètre de jeton de propriété

>[!NOTE] Ceci est un exercice facultatif pour les clients Target Premium.

Le jeton de propriété est un paramètre réservé utilisé avec la fonctionnalité Target Premium [Autorisations des utilisateurs Enterprise](https://docs.adobe.com/content/help/fr-FR/target/using/administer/manage-users/enterprise/property-channel.html). Il sert à définir différentes propriétés numériques afin que différents membres d’une organisation Experience Cloud puissent se voir attribuer différentes autorisations pour chaque propriété. Par exemple, vous souhaitez peut-être qu’un groupe d’utilisateurs puisse configurer des activités Target sur votre site web, mais pas sur votre application mobile.

Les propriétés Target sont similaires aux propriétés Launch et aux suites de rapports Analytics. Une entreprise qui possède plusieurs marques, sites web et équipes marketing peut utiliser une propriété Target, une propriété Launch et une suite de rapports Analytics différentes pour chaque site web ou application mobile. Les propriétés Launch se différencient par leur code intégré, les suites de rapports Analytics par leur identifiant de suite de rapports et les propriétés Target par leur paramètre de jeton de propriété.

Le jeton de propriété est mis en œuvre comme un paramètre de requête. Attribuez simplement au paramètre le nom « at_property » et collez-le dans la valeur fournie dans l’interface Target.  Si vous mettez en œuvre plusieurs sites avec une seule propriété Launch, vous pouvez gérer la valeur at_property par le biais d’un élément de données.

Voici un exercice facultatif, si vous êtes un client Target Premium et si vous souhaitez mettre en œuvre un jeton de propriété dans votre propriété de tutoriel :

1. Dans un onglet distinct, ouvrez l’interface utilisateur de Target.

1. Accédez à **[!UICONTROL Propriétés &gt; Configuration]**.

1. Identifiez la propriété à utiliser et cliquez sur **[!UICONTROL &lt;/&gt;]** (ou créez une nouvelle propriété).

1. Copiez la valeur `at_property` dans votre presse-papiers.

   ![Obtention du jeton de propriété à partir de l’interface Adobe Target](images/target-addATProperty-targetProperties.png)

1. Dans votre onglet Launch, accédez à **[!UICONTROL Règles]** dans le volet de navigation supérieur, puis cliquez sur `All Pages - Library Loaded` pour ouvrir l’éditeur de règles.

   ![Toutes les pages - Bibliothèque chargée](images/target-editRule.png)

1. Sous Actions, cliquez sur l’action `Adobe Target - Add Params to Page Load Request` pour ouvrir `Action Configuration`.

   ![Ajout de paramètres à la requête de chargement de page](images/target-openParamsAction.png)

1. Sous le paramètre `pageName`, cliquez sur le bouton **[!UICONTROL Ajouter]**.

   ![Ajout de paramètres à la requête de chargement de page](images/target-addATProperty.png)

1. Nommez le paramètre `at_property` et collez-y la valeur que vous avez copiée à partir de l’interface de Target.

1. Cliquez sur **[!UICONTROL modifications les Conserver]**.

   ![Conserver les modifications](images/target-addATProperty-keepChanges.png)

1. Cliquez sur **[!UICONTROL créer et bibliothèque la dans Enregistrer]**.
   ![Enregistrer dans la bibliothèque et créer](images/target-addATProperty-save.png)

#### Validation du jeton de propriété

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

**Validation du paramètre de jeton de propriété**

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).
1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Ouvrez les outils de développement de votre navigateur.
1. Cliquez sur l’onglet Réseau.
1. Filtrez les requêtes sur `tt.omtrdc` (ou sur votre domaine CNAME pour les requêtes Target).
1. Développez la section `Headers` &gt; `Request Payload` &gt; `property.token` pour valider la valeur.
   ![Le jeton de propriété doit être visible en tant que paramètre at_property dans chaque requête](images/target-debugger-atProperty-browser.png)

<!--
1. Go to the `Target` tab
1. Expand your client code
1. You should see the parameter for "at_property" in every page load request request as you browse the site:

![The Property Token should be visible as the at_property parameter in every request](images/target-debugger-atProperty.png)-->

## Ajout de requêtes personnalisées

### Ajout d’une requête de confirmation de commande

La requête de confirmation de commande est un type spécial de requête utilisé pour envoyer les détails de la commande à Target. L’inclusion de trois paramètres de requête spécifiques orderId, orderTotal et productPurchasedId transforme une requête Target ordinaire en une requête de commande. Outre la création de rapports de chiffre d’affaires, la requête de commande effectue également les opérations suivantes :

1. Elle déduplique les renvois de commande accidentels.
1. Elle filtre les commandes extrêmes (toute commande dont le total était supérieur à trois écarts types par rapport à la moyenne).
1. Elle utilise un algorithme différent en arrière-plan pour calculer la fiabilité statistique.
1. Elle crée un rapport d’audit spécial et téléchargeable des détails de commande individuels.

Il est recommandé d’utiliser une requête de confirmation de commande dans tous les entonnoirs de commande, même sur des sites non liés à la vente. Par exemple, les sites de génération de piste comportent généralement des entonnoirs de piste avec un « ID de piste » unique généré à la fin. Ces sites doivent mettre en œuvre une requête de commande à l’aide d’une valeur statique (telle que « 1 ») pour orderTotal.

Les clients qui utilisent Analytics for Target (A4T) pour la plupart de leurs rapports doivent également mettre en œuvre la requête de commande, étant donné que A4T n’est pas encore compatible avec les types d’activités tels que l’affectation automatique, la personnalisation automatisée et le ciblage automatique. De plus, la requête de commande est un élément essentiel des mises en œuvre des recommandations, qui optimise les algorithmes en fonction du comportement d’achat.

La requête de confirmation de commande doit se déclencher à partir d’une règle qui n’est déclenchée que sur la page de confirmation de votre commande ou de votre événement. Il est souvent possible de la combiner avec une règle définissant l’événement d’achat Adobe Analytics. Elle doit être configurée à l’aide de l’action Code personnalisé de l’extension Core, à l’aide des éléments de données appropriés pour définir les paramètres orderId, orderTotal et productPurchasedId.

Ajoutons les éléments de données et la règle dont nous avons besoin pour déclencher une requête de confirmation de commande sur le site Luma. Comme vous avez déjà créé plusieurs éléments de données, ces instructions sont abrégées.

**Création de l’élément de données pour l’ID de commande**

1. Cliquez sur **[!UICONTROL données de Éléments]** dans la barre de navigation supérieure.
1. Cliquez sur **[!UICONTROL données de élément un Ajouter]**.
1. Nommez l’élément de données `Order Id`.
1. Sélectionnez **[!UICONTROL Type d’élément de données &gt; Variable JavaScript]**.
1. Utilisez `digitalData.cart.orderId` comme `JavaScript variable name`.
1. Cochez l’option `Clean text`.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque]**.
(Nous ne créerons pas la bibliothèque tant que nous n’aurons pas apporté toutes les modifications à la requête de confirmation de commande.)

**Création de l’élément de données pour le montant du panier**

1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**.
1. Nommez l’élément de données `Cart Amount`.
1. Sélectionnez **[!UICONTROL Type d’élément de données &gt; Variable JavaScript]**.
1. Utilisez `digitalData.cart.cartAmount` comme `JavaScript variable name`.
1. Cochez l’option `Clean text`.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque]**.

**Création d’un élément de données pour les SKU du panier (Target)**

1. Cliquez sur **[!UICONTROL Ajouter un élément de données]**.
1. Nommez l’élément de données `Cart SKUs (Target)`.
1. Sélectionnez **[!UICONTROL Type d’élément de données &gt; Code personnalisé]**.
1. Pour Target, les SKU doivent être représentés par une liste séparée par des virgules. Ce code personnalisé formate à nouveau le tableau de couche de données dans le format approprié. Dans l’éditeur de code personnalisé, collez les éléments suivants :

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

1. Cochez l’option `Force lowercase value`.
1. Cochez l’option `Clean text`.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque]**.

Nous devons maintenant créer une règle pour déclencher la requête de confirmation de commande avec ces éléments de données comme paramètres sur la page de confirmation de commande.

**Création de la règle pour la page de confirmation de commande**

1. Cliquez sur **[!UICONTROL Règles]** dans le volet de navigation supérieur.
1. Cliquez sur **[!UICONTROL Ajouter une règle]**.
1. Donnez à la règle le nom `Order Confirmation Page - Library Loaded - 60`.
1. Cliquez sur **[!UICONTROL Événements &gt; Ajouter]**.
   1. Sélectionnez **[!UICONTROL Type d’événement &gt; Bibliothèque chargée (Haut de page)]**.
   1. Remplacez la valeur `Order` par `60` afin que le déclenchement survienne après l’action `Load Target`, (qui se trouve dans notre règle `All Pages - Library Loaded` où la valeur `Order` est définie sur `50`).
   1. Cliquez sur **[!UICONTROL Conserver les modifications]**.
1. Cliquez sur **[!UICONTROL Conditions &gt; Ajouter]**.
   1. Sélectionnez **[!UICONTROL Type de condition &gt; Chemin sans chaîne de requête]**.
   1. Pour le champ `Path equals`, saisissez `thank-you.html`.
   1. Activez l’option Regex pour modifier la logique de `equals` à `contains`. Vous pouvez utiliser la fonction `Test` pour confirmer que le test réussira avec l’URL `https://luma.enablementadobe.com/content/luma/us/en/user/checkout/order/thank-you.html`.

      ![Saisissez des valeurs aléatoires pour le prénom et le nom](images/target-orderConfirm-test.png)

   1. Cliquez sur **[!UICONTROL Conserver les modifications]**.
1. Cliquez sur **[!UICONTROL Actions &gt; Ajouter]**.
   1. Sélectionnez **[!UICONTROL Type d’action &gt; Code personnalisé]**.
   1. Cliquez sur **[!UICONTROL Ouvrir l’éditeur]**.
   1. Collez le code suivant dans le modal `Edit Code`.

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

   1. Cliquez sur **[!UICONTROL Enregistrer]** pour enregistrer le code personnalisé.
   1. Cliquez sur **[!UICONTROL Conserver les modifications]** pour conserver l’action.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et créer]**.

#### Validation de la requête de confirmation de commande

Pour le moment, les paramètres personnalisés transmis avec les requêtes at.js 2.x ne sont pas facilement visibles dans le débogueur. Nous utiliserons donc les outils de développement du navigateur.

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Parcourez le site et ajoutez plusieurs produits à votre panier.
1. Passez à la caisse.
1. Au cours du processus de paiement, les seuls champs obligatoires sont `First Name` et `Last Name`.

   ![Saisissez des valeurs aléatoires pour le prénom et le nom](images/target-testOrderCart.png)

1. Sur la page de vérification de la commande, veillez à cliquer sur le bouton `Place Order`.
1. Ouvrez les outils de développement de votre navigateur.
1. Cliquez sur l’onglet Réseau.
1. Filtrez les requêtes sur `tt.omtrdc` (ou sur votre domaine CNAME pour les requêtes Target).
1. Cliquez sur la seconde requête.
1. Développez la section `Headers` &gt; `Request Payload` &gt; `execute.mboxes.0` pour valider le nom de la requête et les paramètres de commande :
   ![Paramètres de requête de commande dans le débogueur](images/target-debugger-orderConfirmPage-browser.png)
<!--
1. Look in the Debugger
1. Go to the Target tab
1. Expand your client code
1. You should see the `orderConfirmPage` request as the latest Target request with the orderId, orderTotal, and productPurchasedId parameters populated with the details of your order

   ![orderConfirmPage request with required parameters](images/target-debugger-orderConfirmPage.png)-->

### Requêtes personnalisées

En de rares occasions, vous devez adresser d’autres requêtes que la requête de chargement de page et de confirmation de commande. Par exemple, il arrive que des données importantes que vous souhaitez utiliser pour la personnalisation ne soient pas définies sur la page avant les codes intégrés à Launch. Elles peuvent être codées en dur au bas de la page ou être renvoyées à partir d’une requête d’API asynchrone. Ces données peuvent toujours être envoyées vers Target à l’aide d’une requête supplémentaire, bien qu’il ne soit pas recommandé d’utiliser cette requête pour la diffusion de contenu, puisque la page est déjà visible. Elles peuvent être utilisées pour enrichir le profil du visiteur en vue d’une utilisation ultérieure (à l’aide des paramètres de profil) ou pour compléter le catalogue des recommandations.

Dans ces circonstances, utilisez l’action Code personnalisé dans l’extension Core pour déclencher une requête à l’aide des méthodes
[getOffer()](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/adobe-target-getoffer.html)/[applyOffer()](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/adobe-target-applyoffer.html) et [trackEvent()](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/adobe-target-trackevent.html). Cette manipulation est similaire à ce que vous avez effectué dans l’exercice [Requête de confirmation de commande](#order-confirmation-request), mais vous utilisez un autre nom de requête et non des paramètres de commande spéciaux. Veillez à utiliser l’action **[!UICONTROL Charger Target]** avant d’adresser des requêtes Target à partir du code personnalisé.

## En-tête et pied de page de bibliothèque

L’écran Modifier at.js de l’interface utilisateur de Target contient des emplacements dans lesquels vous pouvez coller le code JavaScript personnalisé qui s’exécute immédiatement avant ou après le fichier at.js. L’en-tête de bibliothèque est parfois utilisé pour remplacer les paramètres at.js par le biais de la fonction [targetGlobalSettings()](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/targetgobalsettings.html) ou pour transmettre des données provenant de tiers en utilisant la fonction [Fournisseurs de données](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html). Le pied de page de la bibliothèque est parfois utilisé pour ajouter des détecteurs [d’événements personnalisés at.js](https://docs.adobe.com/content/help/fr-FR/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html).

Pour profiter de cette fonctionnalité dans Launch, utilisez l’action Code personnalisé dans l’extension Core et séquencez l’action avant En-tête de bibliothèque ou après Pied de page de bibliothèque l’action Charger Target. Cette action peut être effectuée dans la même règle que l’action `Load Target`, comme illustré ci-dessous, ou dans des règles distinctes avec des événements ou des paramètres de commande qui se déclenchent de manière fiable avant ou après la règle contenant `Load Target` :

![En-tête et pied de page de bibliothèque dans la séquence d’actions](images/target-libraryHeaderFooter.png)

Pour en savoir plus sur les cas d’utilisation pour les en-têtes et pieds de page personnalisés, consultez les ressources suivantes :

* [Utilisation de dataProviders pour intégrer des données tierces dans Adobe Target](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [Implémentation de dataProviders pour intégrer des données tierces dans Adobe Target](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
* [Utilisation de jetons de réponse et d’événements personnalisés at.js avec Adobe Target](https://docs.adobe.com/content/help/en/target-learn/tutorials/integrations/use-response-tokens-and-atjs-custom-events.html)

[Suite : « Ajout d’Adobe Analytics » &gt;](analytics.md)
