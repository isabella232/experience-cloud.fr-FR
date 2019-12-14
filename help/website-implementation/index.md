---
title: Implémentation d’Adobe Experience Cloud dans les sites Web avec le lancement d’Adobe Experience Platform
description: La mise en oeuvre d’Experience Cloud dans les sites Web avec lancement constitue le point de départ idéal pour les développeurs frontaux ou les spécialistes du marketing technique qui souhaitent apprendre à mettre en oeuvre les solutions Adobe Experience Cloud sur leur site Web.
seo-description: null
seo-title: Implémentation d’Adobe Experience Cloud dans les sites Web avec le lancement d’Adobe Experience Platform
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Aperçu

_La mise en oeuvre d’Experience Cloud dans les sites Web avec lancement_ constitue le point de départ idéal pour les développeurs frontaux ou les spécialistes du marketing technique qui souhaitent apprendre à mettre en oeuvre les solutions Adobe Experience Cloud sur leur site Web.

Chaque leçon contient des exercices pratiques et des informations fondamentales pour vous aider à mettre en oeuvre Experience Cloud et à comprendre sa valeur.  Les légendes sont fournies pour mettre en surbrillance des informations utiles aux clients qui migrent depuis notre ancien gestionnaire de balises, la gestion dynamique des balises. Les sites de démonstration vous permettent de suivre le tutoriel afin que vous puissiez apprendre les techniques sous-jacentes dans un environnement sûr. Après avoir suivi ce didacticiel, vous devez être prêt à commencer à implémenter toutes vos solutions marketing par le biais de Launch sur votre propre site Web.

Après avoir terminé, vous pourrez :

* Création d’une propriété de lancement

* Installation d’une propriété de lancement sur un site Web

* Ajoutez les solutions Adobe Experience Cloud suivantes :
   * **[Service Adobe Experience Platform Identity](id-service.md)**
   * **[Adobe Target](target.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* Créez des règles et des éléments de données pour envoyer des données aux solutions Adobe.

* Validation de l’implémentation à l’aide du débogueur Adobe Experience Cloud

* Publier les modifications dans les environnements de développement, d’évaluation et de production

>[!NOTE] Des didacticiels multisolutions similaires sont également disponibles pour les plateformes suivantes :
>
> * [Mise en oeuvre d’Experience Cloud dans les applications mobiles iOS Swift™](/help/mobile-ios-swift-implementation/index.md)
* [Mise en oeuvre d’Experience Cloud dans les applications iOS mobiles Objective-C](/help/mobile-ios-objective-c-implementation/index.md)
* [Mise en oeuvre d’Experience Cloud dans les applications mobiles Android](/help/mobile-android-implementation/index.md)


## Conditions préalables 

Dans ces leçons, on suppose que vous disposez d’un ID Adobe et des autorisations requises pour terminer les exercices. Dans le cas contraire, vous devrez peut-être contacter votre administrateur Experience Cloud pour demander l’accès.

* Pour le lancement, vous devez être autorisé à développer, approuver, publier, gérer les extensions et gérer les environnements. For more information on Launch permissions, see [the documentation](https://docs.adobe.com/content/help/en/launch/using/reference/admin/user-permissions.html).
* Pour Adobe Analytics, vous devez connaître votre serveur de suivi et les suites de rapports que vous utiliserez pour terminer ce didacticiel
* Pour Audience Manager, vous devez connaître votre sous-domaine Audience Manager (également appelé "Nom du partenaire" "ID du partenaire" ou "Sous-domaine du partenaire").

En outre, vous êtes censé connaitre les langages de développement d’interfaces tels que HTML et JavaScript. Vous n’avez pas besoin d’être un expert de ces langages pour terminer les leçons, mais vous assimilerez plus d’informations si vous pouvez facilement lire et comprendre le code.

## À propos de Launch

Adobe Experience Platform Launch est la prochaine génération de fonctionnalités de gestion des balises de site Web et des kits SDK mobiles d’Adobe. Le lancement offre aux clients un moyen simple de déployer et de gérer toutes les solutions d’analyse, de marketing et de publicité nécessaires pour générer des expériences client pertinentes. Il n'y a pas de frais supplémentaires pour le lancement. Il est disponible pour tout client Adobe Experience Cloud.

Le lancement pour les sites Web vous permet de gérer de manière centralisée l’ensemble du code JavaScript lié aux solutions d’analyse, de marketing et de publicité utilisées sur vos sites pour postes de travail et mobiles. Si, par exemple, vous déployez Adobe Analytics, Launch gère la bibliothèque JavaScript AppMeasurement, renseigne les variables et déclenche les demandes.

La balise conteneur Lancer est 60 % plus légère que la gestion dynamique des balises et 40 % plus légère que le gestionnaire de balises Google. Le contenu de votre conteneur est réduit, y compris votre code personnalisé. Tout est modulaire. Si vous n’avez pas besoin d’un élément, celui-ci n’est pas inclus dans votre bibliothèque. Vous obtenez ainsi une mise en œuvre rapide et compacte.

Launch est également une plate-forme qui permet aux fournisseurs tiers de créer des extensions pour faciliter le déploiement de leurs solutions via Launch. Une extension est un module de code (JavaScript, HTML et CSS) qui étend l’interface utilisateur de Launch et les fonctionnalités du client. Launch peut être considéré comme un système d’exploitation, et les extensions comme les applications utilisées pour exécuter les tâches.

## À propos des leçons

Dans ces leçons, vous allez mettre en oeuvre Adobe Experience Cloud dans un faux site Web de vente au détail appelé Luma. Le site [](https://luma.enablementadobe.com/content/luma/us/en.html) Luma possède une couche de données riche et une fonctionnalité qui vous permettra de construire une implémentation réaliste. Vous allez créer votre propre propriété Launch, dans votre propre organisation Experience Cloud, et la mapper à notre site Luma hébergé à l’aide du débogueur Experience Cloud.

[![Site Web de Luma](images/overview-luma.png)](https://luma.enablementadobe.com/content/luma/us/en.html)

## Obtenir les outils

1. Puisque vous utiliserez certaines extensions spécifiques au navigateur, nous vous recommandons de suivre le tutoriel en utilisant le [navigateur web Chrome](https://www.google.com/chrome/)
1. Add the [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj) extension to your Chrome browser
1. Download the [sample html page](https://www.enablementadobe.com/multi/web/basic-sample.html) (right-click on this link and click "Save Link As")
1. Obtenez un éditeur de texte dans lequel vous pouvez apporter des modifications à l’exemple de page HTML. (Si vous n’en avez pas, nous vous recommandons d’essayer [Brackets](http://brackets.io/))
1. Mettre en signet le site [Luma](https://luma.enablementadobe.com/content/luma/us/en.html)

>[!NOTE] Vous pouvez trouver plus facile de terminer ce didacticiel avec le site Luma ouvert dans Chrome, pendant que vous lisez ce didacticiel et terminez les étapes de l’interface de lancement dans un autre navigateur.

Commençons !

[Suivant : "Créer une propriété de lancement" &gt;](launch.md)