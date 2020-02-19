---
title: Mise en œuvre d’Experience Cloud dans les applications mobiles Swift pour iOS
description: La mise en œuvre d’Experience Cloud dans les applications mobiles Swift pour iOS est le point de départ idéal pour les développeurs d’applications mobiles qui souhaitent apprendre à mettre en œuvre les solutions Adobe Experience Cloud dans leurs applications mobiles Swift pour iOS.
seo-description: null
seo-title: 'Mise en œuvre d’Experience Cloud dans les applications mobiles Swift pour iOS '
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Présentation

_La mise en œuvre d’Experience Cloud dans les applications mobiles Swift™ pour iOS_ est le point de départ idéal pour les développeurs d’applications mobiles qui souhaitent apprendre à mettre en œuvre les solutions Adobe Experience Cloud dans leurs applications Swift pour iOS.

![Logo Swift](images/ios/swift/Swift_logo_horz_lockup_color_rgb.png)

Chaque leçon comprend des exercices pratiques et des informations essentielles pour vous aider à mettre en œuvre Experience Cloud et à comprendre sa valeur.  Une application de démonstration Swift vous permet de suivre le tutoriel afin que vous puissiez apprendre les techniques sous-jacentes dans un environnement sûr. Après avoir suivi ce tutoriel, vous devriez être prêt à commencer à mettre en œuvre toutes vos solutions Experience Cloud dans votre propre application Swift pour iOS.

À la fin de ce tutoriel, vous saurez comment :

* créer une propriété Launch mobile ;

* installer une propriété Launch dans une application Swift ;

* mettre en œuvre les solutions Adobe Experience Cloud suivantes :
   * **[Adobe Experience Platform Identity Service](id-service.md)**
   * **[Adobe Target](target-vec.md)**
   * **[Adobe Analytics](analytics.md)**
   * **[Adobe Audience Manager](audience-manager.md)**

* publier des modifications dans Launch via les environnements de développement, d’évaluation et de production.

>[!NOTE] Des tutoriels multisolutions similaires sont également disponibles pour les plateformes suivantes :
>
> * [Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS](/help/mobile-ios-objective-c-implementation/index.md)
> * [Mise en œuvre d’Experience Cloud dans les applications mobiles pour Android™](/help/mobile-android-implementation/index.md)
* [Mise en œuvre d’Experience Cloud dans les sites web pour Launch](/help/website-implementation/index.md)


## Conditions préalables

Dans ces leçons, il est attendu que vous disposiez d’un ID Adobe et des autorisations requises pour effectuer les exercices. Dans le cas contraire, vous devrez peut-être contacter votre administrateur Experience Cloud pour demander l’accès.

* Pour Launch, vous devez disposer des autorisations Développer, Approuver, Publier, Gérer les extensions et Gérer les environnements. Pour plus d’informations sur les autorisations dans Launch, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/user-permissions.html).
* Pour Target, vous devez disposer d’un accès de niveau Approbateur à l’interface d’Adobe Target.
* Pour Adobe Analytics, vous devez connaitre votre serveur de suivi et les suites de rapports que vous utiliserez pour terminer ce tutoriel.

De plus, nous partons du principe que vous avez une bonne connaissance du développement iOS dans Swift. Vous n’avez pas besoin d’être un expert de Swift pour terminer les leçons, mais vous assimilerez plus d’informations si vous pouvez facilement lire et comprendre le code.

## À propos des leçons

Dans ces leçons, vous allez mettre en œuvre Adobe Experience Cloud dans une application de démonstration appelée « [Bus Booking](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps) » à l’aide de votre propre organisation Experience Cloud. L’application dispose de fonctionnalités simples qui vous permettront d’effectuer une mise en œuvre mobile de base d’Experience Cloud avant de le faire dans votre propre application.

[![Application Bus Booking](images/mobile-busBookingApp.png)](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps)

## Obtention des outils

1. Vous devez utiliser un Mac® pour suivre ce tutoriel.
1. Téléchargez [Xcode](https://developer.apple.com/xcode/).
1. Téléchargez l’[application Bus Booking](https://github.com/Adobe-Marketing-Cloud/busbooking-mobileapps).
1. Installez [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).

C’est parti !

[Suite : « Création d’une propriété Launch » &gt;](launch-create-a-property.md)

_Apple, le logo Apple, l’iPad, l’iPhone, l’iPod et l’iPod touch sont des marques déposées d’Apple Inc., enregistrées aux États-Unis et dans d’autres pays. Swift et le logo Swift sont des marques déposées d’Apple Inc._