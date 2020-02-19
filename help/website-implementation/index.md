---
title: Mise en œuvre d’Experience Cloud dans les sites web avec Adobe Experience Platform Launch
description: Mise en œuvre d’Experience Cloud dans les sites web avec Launch constitue le point de départ idéal pour les développeurs front-end ou les spécialistes du marketing technique qui souhaitent apprendre à mettre en œuvre les solutions Adobe Experience Cloud sur leur site web.
seo-description: null
seo-title: Mise en œuvre d’Experience Cloud dans les sites web avec Adobe Experience Platform Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Aperçu

_Mise en œuvre d’Experience Cloud dans les sites web avec Launch_ constitue le point de départ idéal pour les développeurs front-end ou les spécialistes du marketing technique qui souhaitent apprendre à mettre en œuvre les solutions Adobe Experience Cloud sur leur site web.

Chaque leçon comprend des exercices pratiques et des informations essentielles pour vous aider à mettre en œuvre Experience Cloud et à comprendre sa valeur. Des légendes sont fournies pour mettre en évidence les informations qui peuvent être utiles aux clients effectuant la migration depuis Dynamic Tag Management, l’ancien gestionnaire de balises. Les sites de démonstration vous permettent de suivre le tutoriel afin que vous puissiez apprendre les techniques sous-jacentes dans un environnement sûr. Après avoir suivi ce tutoriel, vous serez prêt à commencer à mettre en œuvre toutes vos solutions marketing par le biais de Launch sur votre propre site web.

Après avoir terminé cette formation, vous saurez comment :

* créer une propriété Launch ;

* installer une propriété Launch sur un site web ;

* ajouter les solutions Adobe Experience Cloud suivantes :
   * **[Adobe Experience Platform Identity Service](id-service.md)**
   * **[Adobe Target](target.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* créer des règles et des éléments de données pour envoyer des données aux solutions Adobe ;

* valider votre mise en œuvre à l’aide du débogueur Adobe Experience Cloud Debugger ;

* publier des modifications dans Launch via les environnements de développement, d’évaluation et de production.

>[!NOTE] Des tutoriels multisolutions similaires sont également disponibles pour les plateformes suivantes :
>
> * [Mise en œuvre d’Experience Cloud dans les applications mobiles Swift™ pour iOS](/help/mobile-ios-swift-implementation/index.md)
* [Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS](/help/mobile-ios-objective-c-implementation/index.md)
* [Mise en œuvre d’Experience Cloud dans les applications mobiles pour Android](/help/mobile-android-implementation/index.md)


## Conditions préalables

Dans ces leçons, il est attendu que vous disposiez d’un ID Adobe et des autorisations requises pour effectuer les exercices. Dans le cas contraire, vous devrez peut-être contacter votre administrateur Experience Cloud pour demander l’accès.

* Pour Launch, vous devez disposer des autorisations Développer, Approuver, Publier, Gérer les extensions et Gérer les environnements. Pour plus d’informations sur les autorisations dans Launch, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/user-permissions.html).
* Pour Adobe Analytics, vous devez connaitre votre serveur de suivi et les suites de rapports que vous utiliserez pour terminer ce tutoriel.
* Pour Audience Manager, vous devez connaitre votre Subdomain (Sous-domaine) Audience Manager, également appelé Nom de partenaire, Partner Identifiant de partenaire ou Sous-domaine de partenaire.

En outre, vous êtes censé connaitre les langages de développement d’interfaces tels que HTML et JavaScript. Vous n’avez pas besoin d’être un expert de ces langages pour terminer les leçons, mais vous assimilerez plus d’informations si vous pouvez facilement lire et comprendre le code.

## À propos de Launch

Conçu par Adobe, Adobe Experience Platform Launch est la nouvelle génération de fonctionnalités de gestion de SDK mobile et de balises de sites web. Launch offre aux clients un moyen simple de déployer et gérer toutes les solutions d’analyse, de marketing et de publicité nécessaires pour proposer des expériences client pertinentes. Launch n’entraîne aucuns frais supplémentaires. Il est disponible pour tout client Adobe Experience Cloud.

Launch pour les sites web vous permet de gérer de manière centralisée l’ensemble du code JavaScript lié aux solutions d’analyse, de marketing et de publicité utilisées sur vos sites pour ordinateurs et appareils mobiles. Si, par exemple, vous déployez Adobe Analytics, Launch gèrera la bibliothèque JavaScript AppMeasurement, renseignera les variables et déclenchera les demandes.

La balise conteneur Launch est 60 % plus légère que DTM et 40 % plus légère que Google Tag Manager. Le contenu de votre conteneur est réduit, y compris votre code personnalisé. Tout est modulaire. Si vous n’avez pas besoin d’un élément, celui-ci n’est pas inclus dans votre bibliothèque. Vous obtenez ainsi une mise en œuvre rapide et compacte.

Launch est également une plateforme qui permet aux fournisseurs tiers de créer des extensions pour faciliter le déploiement de leurs solutions via Launch. Une extension est un module de code (JavaScript, HTML et CSS) qui étend l’interface utilisateur de Launch et les fonctionnalités du client. Launch peut être considéré comme un système d’exploitation, et les extensions comme les applications utilisées pour exécuter les tâches.

## À propos des leçons

Dans ces leçons, vous allez mettre en œuvre Adobe Experience Cloud dans un faux site web de vente au détail appelé Luma. Le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html) possède une couche de données et des fonctionnalités riches qui vous permettront de faire une mise en œuvre réaliste. Vous allez créer votre propre propriété Launch, dans votre propre organisation Experience Cloud, et la mapper à notre site Luma hébergé à l’aide du débogueur Experience Cloud Debugger.

[![Site web Luma](images/overview-luma.png)](https://luma.enablementadobe.com/content/luma/us/en.html)

## Obtention des outils

1. Puisque vous utiliserez certaines extensions spécifiques au navigateur, nous vous recommandons de suivre le tutoriel en utilisant le [navigateur web Chrome](https://www.google.com/intl/fr/chrome/)
1. Ajoutez l’extension [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) à votre navigateur Chrome.
1. Téléchargez la [page HTML d’exemple](https://www.enablementadobe.com/multi/web/basic-sample.html) (cliquez avec le bouton droit sur ce lien et cliquez sur « Enregistrer le lien sous »).
1. Utilisez un éditeur de texte dans lequel vous pouvez modifier la page HTML d’exemple. (Si vous n’en avez pas, nous vous recommandons d’essayer [Brackets](http://brackets.io/)).
1. Ajoutez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html) à vos favoris.

>[!NOTE] Il vous sera sans doute plus facile de suivre ce tutoriel avec le site Luma ouvert dans Chrome. Vous pourrez lire le tutoriel et tout en suivant les étapes dans l’interface de Launch dans un autre navigateur.

C’est parti !

[Suite : « Création d’une propriété Launch » &gt;](launch.md)