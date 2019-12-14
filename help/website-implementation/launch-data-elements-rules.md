---
title: Ajouter un élément de données, une règle et une bibliothèque
description: Découvrez comment créer des éléments de données, des règles et une bibliothèque au lancement. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-description: null
seo-title: Ajouter un élément de données, une règle et une bibliothèque
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajouter un élément de données, une règle et une bibliothèque

Dans cette leçon, vous allez créer votre premier élément de données, votre première règle et votre première bibliothèque.

Les éléments de données et les règles constituent les éléments de base du lancement. Les éléments de données stockent les attributs que vous souhaitez envoyer à vos solutions marketing et publicitaires, tandis que les règles déclenchent les requêtes vers ces solutions dans les bonnes conditions.  Les bibliothèques sont les fichiers JavaScript qui se chargent sur la page pour effectuer tout le travail. Dans cette leçon, vous utiliserez les trois pour faire quelque chose sur notre page d'exemple.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Créer un élément de données
* Créer une règle
* Créer une bibliothèque
* Ajouter des modifications à une bibliothèque
* Valider le chargement de la bibliothèque dans votre navigateur web
* Utiliser la fonction "Bibliothèque de travail" pour travailler plus efficacement

## Création d’un élément de données pour le nom de page

Les éléments de données sont la version de lancement d’une couche de données. Ils peuvent stocker les valeurs de votre propre objet de couche de données, des cookies, des objets de stockage local, des paramètres de chaîne de requête, des éléments de page, des balises meta, etc. Au cours de cet exercice, vous allez créer un élément de données pour le nom de page, que vous utiliserez ultérieurement dans vos implémentations Target et Analytics.

**Pour créer un élément de données**

1. Dans le volet de navigation supérieur, cliquez sur Eléments **[!UICONTROL de données.]**

1. Comme vous n’avez encore créé aucun élément de données dans cette propriété, une courte vidéo s’affiche avec des informations supplémentaires sur cette rubrique. Regardez cette vidéo si vous le souhaitez.

1. Click the **[!UICONTROL Create New Data Element]** button:

   ![Cliquez sur le bouton Create New Data Element (Créer un élément de données)](images/launch-newDataElement.png)

1. Name the data element, e.g. `Page Name`

1. Use the [!UICONTROL JavaScript Variable] Data Element type to point to a value in your sample page's data layer: `digitalData.page.pageInfo.pageName`

1. Use "`not available`" as the [!UICONTROL Default Value]. The [!UICONTROL Default Value] tells Launch what value to use for the data element if your JavaScript Variable specified above is not found.

1. Cochez les cases pour **[!UICONTROL Forcer la valeur minuscule]** et **[!UICONTROL Nettoyer le texte]** pour normaliser la casse et supprimer les espaces superflus

1. Laissez **[!UICONTROL Aucun]** comme paramètre Durée **[!UICONTROL de]** stockage, car cette valeur est généralement différente sur chaque page.

1. Cliquez sur le bouton **[!UICONTROL Enregistrer]** pour enregistrer l’élément de données.

   ![Créez l’élément](images/launch-dataElement.png)de données Nom de page.

>[!NOTE]**** Migrateurs DTM : De nouveaux types d’éléments de données ont été ajoutés au lancement, qui n’existait pas dans la gestion dynamique des balises. Parmi les nouveaux types d’éléments de données il y a : un stockage local, un stockage de session, des informations sur la page et des nombres aléatoires
<!-- -->
>[!NOTE]Les fonctionnalités des éléments de données _peuvent être étendues avec les extensions_. Par exemple, l’extension ContextHub vous permet d’ajouter des éléments de données à l’aide des fonctionnalités de l’extension.

## Créer une règle

Vous utiliserez ensuite cet élément de données dans une règle simple. Les règles sont l’une des fonctionnalités les plus puissantes du lancement et vous permettent de spécifier ce qui doit se produire lorsque le visiteur interagit avec votre site Web. Lorsque les critères décrits dans vos règles sont satisfaits, la règle déclenche l’action que vous avez spécifiée.

Vous allez créer une règle qui renvoie la valeur de l’élément de données Nom de page dans la console du navigateur.

**Pour créer une règle**

1. In the top navigation, click **[!UICONTROL Rules]**

1. Comme vous n’avez encore créé aucune règle dans cette propriété, une courte vidéo s’affiche avec des informations supplémentaires sur la rubrique. Regardez cette vidéo si vous le souhaitez.

1. Click the **[!UICONTROL Create New Rule]** button:

   ![Cliquez sur le bouton Create New Rule (Créer une règle)](images/launch-newRule.png)

1. Name the Rule `All Pages - Library Loaded`. Cette convention d’affectation de nom indique où et quand la règle se déclenchera, ce qui facilite l’identification et la réutilisation à mesure que votre propriété Launch arrive à maturité.

1. Sous Événements, cliquez sur **[!UICONTROL Ajouter]**. L’événement indique au lancement le moment où la règle doit se déclencher et peut être de nombreux éléments, notamment un chargement de page, un clic, un événement JavaScript personnalisé, etc.

   ![Attribution d’un nom à la règle et ajout d’un événement](images/launch-addEventToRule.png)

   1. En tant que type d’événement, sélectionnez **[!UICONTROL Chargé par bibliothèque Haut de page]**. Notez que lorsque vous sélectionnez le type d’événement, le paramètre Lancer préremplit le nom de l’événement à l’aide de votre sélection. Notez également que l’ordre par défaut de l’événement est 50. La commande est une puissante fonctionnalité du lancement qui vous permet de contrôler précisément la séquence d’actions lorsque plusieurs règles sont déclenchées par le même événement. Vous utiliserez cette fonctionnalité plus loin dans le didacticiel.

   1. Cliquez sur le bouton **[!UICONTROL Conserver les modifications]**
   ![Sélection d’un événement](images/launch-ruleSelectEvent.png)

1. Puisque cette règle doit se déclencher sur toutes les pages, laissez **[!UICONTROL Conditions]** vides. Si vous ouvrez le modal Conditions, vous verrez que les conditions peuvent ajouter des restrictions aussi bien que des exclusions en fonction d’une grande variété d’options, y compris les URL, les valeurs d’élément de données, les plages de dates, etc.

1. Under Actions, click **[!UICONTROL Add]**

1. Sélectionnez **[!UICONTROL Action Type &gt; Code]** personnalisé, qui est actuellement la seule option. Plus loin dans le didacticiel, au fur et à mesure que vous ajouterez des extensions, plus d’options deviennent disponibles.

1. Sélectionnez **[!UICONTROL &lt;/&gt; Ouvrir l’éditeur]** pour ouvrir l’éditeur de code.

   ![Sélection d’une action](images/launch-selectAction.png)

1. Ajoutez les éléments suivants à l’éditeur de code. Ce code génère la valeur de l’élément de données Nom de page dans la console du navigateur afin que vous puissiez confirmer son fonctionnement :

   ```javascript
   console.log('The page name is '+_satellite.getVar('Page Name'));
   ```

1. Enregistrer l’éditeur de code

   ![Saisie d’un code personnalisé](images/launch-customCodeAction.png)

1. On the Action configuration screen click **[!UICONTROL Keep Changes]**

1. Click **[!UICONTROL Save]** to save the rule

>[!NOTE]**** Migrateurs DTM : Dans Lancement, des règles sont requises pour déclencher la plupart des pixels marketing. Par exemple, pour déclencher la balise Adobe Analytics, vous devez utiliser une règle pour indiquer au lancement de le faire.
>
> Le créateur de règles a été considérablement repensé et reconstruit dans Launch.
> Voici quelques-unes des principales modifications :
>
> * Il y a un seul créateur de règles. Les types de règle de la gestion dynamique des balises tels que "Bas de page", "Clic" et "Appel direct" sont tous des types d’événement dans le créateur de règles. Cela facilite la mise à jour d’une règle si vous devez modifier le trigger, par exemple, d’un événement prêt pour DOM à un événement personnalisé.
> * Il existe un nouveau type d’événement "Code personnalisé".
> * Les extensions peuvent ajouter de nouveaux types d’événements au créateur de règles. Par exemple, l’extension Target peut ajouter une prise en charge intégrée pour ses [événements personnalisés at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/functions-overview/atjs-custom-events.html), de sorte que le code personnalisé n’est pas nécessaire pour utiliser cette fonctionnalité.
> * Les extensions peuvent ajouter de nouvelles actions au créateur de règles, ce qui réduit les problèmes en rendant obsolète la dépendance à l’égard du code personnalisé. Vous utiliserez bon nombre de ces actions d’extension dans ce didacticiel.
> * Des règles sont nécessaires pour déclencher les requêtes associées à la plupart des outils marketing. Cela nécessitera un ajustement de l’état d’esprit, notamment pour les éléments tels que la définition des identifiants client, le déclenchement des balises Analytics et le déclenchement des requêtes Target.


## Enregistrer vos modifications dans une bibliothèque

Après avoir configuré une collection d’extensions, d’éléments de données et de règles dans l’interface de lancement, vous devez regrouper ces fonctionnalités et cette logique dans un ensemble de code JavaScript que vous pouvez déployer sur votre site Web afin que les balises marketing se déclenchent lorsque les visiteurs se rendent sur le site. Une bibliothèque est l'ensemble du code JavaScript qui fera cela.

Dans une précédente leçon, vous avez mis en œuvre le code incorporé de votre environnement de développement sur la page d’exemple. Lorsque vous avez chargé l’exemple de page, une erreur 404 a été renvoyée pour l’URL du code incorporé, car une bibliothèque de lancement n’avait pas encore été créée et affectée à l’environnement. Vous allez maintenant placer votre nouvel élément de données et votre nouvelle règle dans une bibliothèque afin que votre exemple de page puisse faire quelque chose.

**Pour ajouter et créer une bibliothèque**

1. Go to the [!UICONTROL Publishing] tab

1. Click **[!UICONTROL Add New Library]**

   ![Ajouter une bibliothèque](images/launch-addNewLibrary.png)

1. Nommer la bibliothèque "Configuration initiale"

1. Sélectionnez **[!UICONTROL Environnement &gt; Développement]**

1. Cliquez sur **[!UICONTROL Ajouter toutes les ressources modifiées]**

   ![Ajouter toutes les ressources modifiées](images/launch-addAllChangedResources.png)

1. Notez qu’après avoir cliqué sur **[!UICONTROL Ajouter toutes les ressources]** modifiées, le lancement résume les modifications que vous venez d’apporter.

1. Cliquez sur **[!UICONTROL Enregistrer et générer pour le développement]**

   ![Sauver et construire pour le développement](images/launch-saveAndBuild.png)

Au bout de quelques instants, le point d’état devient vert pour indiquer que la bibliothèque a été créée avec succès.

![Bibliothèque construite](images/launch-libraryBuilt.png)

## Valider votre travail

Vérifiez maintenant que votre règle fonctionne comme prévu.

Rechargez votre exemple de page. Si vous consultez l’onglet Outils de développement -&gt; Réseau, vous devriez maintenant voir une réponse de 200 pour votre bibliothèque de lancement !

![Chargements de bibliothèque avec 200 réponses](images/samplepage-200.png)

Si vous consultez la console Outils de développement -&gt; Console, le texte "Le nom de la page est la page d’accueil" s’affiche.

![Message de la console](images/samplepage-console.png)

Félicitations, vous avez créé votre premier élément de données et règle et créé votre première bibliothèque de lancement !

## Utilisation de la fonction Bibliothèque de travail

Lorsque vous apportez de nombreuses modifications au lancement, il n’est pas pratique d’accéder à l’onglet Publication, d’ajouter des modifications et de créer la bibliothèque chaque fois que vous souhaitez voir le résultat.  Maintenant que vous avez créé votre bibliothèque de configuration initiale, vous pouvez utiliser une fonctionnalité appelée "Bibliothèque de travail" pour enregistrer rapidement vos modifications et recréer la bibliothèque en une seule étape.

Apportez une petite modification à votre règle "Toutes les pages - Bibliothèque chargée". Dans le volet de navigation supérieur, cliquez sur **[!UICONTROL Règles]** , puis sur la `All Pages - Library Loaded` règle pour l’ouvrir.

![Rouvrir la règle](images/launch-reopenRule.png)

Sur la `Edit Rule` page, cliquez sur la liste déroulante Bibliothèque ***[!UICONTROL de]*** travail et sélectionnez votre `Initial Setup` bibliothèque.

![Sélectionner la configuration initiale comme bibliothèque de travail](images/launch-setWorkingLibrary.png)

Après avoir sélectionné la bibliothèque, vous verrez que le bouton **[!UICONTROL Enregistrer]** est désormais **[!UICONTROL Enregistrer dans la bibliothèque et Créer]** par défaut. Lorsque vous effectuez une modification dans Lancer, vous pouvez utiliser cette option pour ajouter automatiquement la modification directement à votre bibliothèque de travail et la recréer.

Testez-le. Ouvrez l’action Code personnalisé et ajoutez simplement un deux-points après le texte "Le nom de la page est" afin que le bloc de code complet se lise comme suit :

```javascript
console.log('The page name is: '+_satellite.getVar('Page Name'));
```

Enregistrez le code, conservez les modifications dans l’action, puis cliquez sur le bouton **[!UICONTROL Enregistrer dans la bibliothèque et Créer]** .

![L’option Enregistrer et créer existe désormais](images/launch-workingLibrary-saveAndBuild.png)

Patientez un moment jusqu’à ce que le point vert réapparaisse en regard de la liste déroulante Bibliothèque [!UICONTROL de] travail. Maintenant, rechargez votre exemple de page et vous devriez voir votre modification reflétée dans le message de la console (vous devrez peut-être vider le cache de votre navigateur et le recharger pour voir la modification de la page) :

![Message de la console avec deux points](images/samplepage-consoleWithColon.png)

C'est une façon beaucoup plus rapide de travailler et vous utiliserez cette approche pour le reste du tutoriel.

[Suivant "Changer d’environnement avec le débogueur Experience Cloud" &gt;](launch-switch-environments.md)
