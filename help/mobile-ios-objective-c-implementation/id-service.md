---
title: Mise en œuvre d’Adobe Experience Platform Identity Service avec Launch
description: Découvrez comment ajouter l’extension Adobe Experience Platform Identity Service et utiliser l’action Définition des ID de client pour collecter les ID de client. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles Objective-C pour iOS.
seo-description: null
seo-title: Mise en œuvre d’Adobe Experience Platform Identity Service avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout d’Adobe Experience Platform Identity Service

[Adobe Experience Platform Identity Service](https://docs.adobe.com/content/help/fr-FR/id-service/using/home.html) définit un identifiant visiteur commun à toutes les solutions Adobe afin d’alimenter les fonctionnalités d’Experience Cloud telles que le partage d’audience entre les solutions.  Vous pouvez également envoyer vos propres ID au service pour permettre un ciblage entre les appareils et des intégrations supplémentaires avec votre système de gestion de la relation client (CRM).

Identity Service fait partie de l’extension Mobile Core, ***vous l’avez donc déjà mis en œuvre lorsque vous avez installé le SDK Mobile***. Pour le moment, ce tutoriel n’inclut pas d’instructions sur la définition des ID de client. Consultez la documentation pour plus d’informations sur [la définition des ID de client](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference).

## Étapes de validation

Pour valider les appels à Identity Service, exécutez l’exemple d’application dans Xcode ou Developer Studio, ouvrez la console de débogage et recherchez la requête et la réponse :

1. **Requête à Identity Service** (filtrez votre console sur `demdex.net`) Dans cet exemple, l’ID (`d_mid`) a déjà été défini et est simplement de nouveau signalé.

```objective-c
2019-03-13 16:53:26.655908-0400 BusBookingObjectiveC[56630:3854937] [AMSDK DEBUG <com.adobe.module.identity>]:Sending request (https://dpm.demdex.net/id?d_rtbd=json&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61@AdobeOrg&d_mid=67027929491180584128922600814231770586)
```

1. **Réponse d’Identity Service** (filtrez votre console sur `ID Service`). Notez comment la valeur `mid` correspond à la valeur `d_mid` dans la requête ci-dessus :

```objective-c
2019-03-13 16:53:27.397048-0400 BusBookingObjectiveC[56630:3854937] [AMSDK DEBUG <com.adobe.module.identity>]: ID Service - Got ID Response (mid: 67027929491180584128922600814231770586, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: "604800000 ms")
```

[Suite : « Ajout de la prise en charge du VEC d’Adobe Target » &gt;](target-vec.md)
