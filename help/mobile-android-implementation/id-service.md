---
title: Mise en œuvre d’Adobe Experience Platform Identity Service avec Launch
description: Découvrez comment ajouter l’extension Adobe Experience Platform Identity Service et utiliser l’action Définition des ID de client pour collecter les ID de client. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles pour Android.
seo-description: null
seo-title: Mise en œuvre d’Adobe Experience Platform Identity Service avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout d’Adobe Experience Platform Identity Service

[Adobe Experience Platform Identity Service](https://docs.adobe.com/content/help/fr-FR/id-service/using/home.html) définit un identifiant visiteur commun à toutes les solutions Adobe afin d’alimenter les fonctionnalités d’Experience Cloud telles que le partage d’audience entre les solutions.  Vous pouvez aussi envoyer vos propres ID de client au service pour permettre un ciblage entre appareils et des intégrations supplémentaires avec votre système de gestion de la relation client (CRM).

Identity Service fait partie de l’extension Mobile Core, ***vous l’avez donc déjà mis en œuvre lorsque vous avez installé le SDK Mobile***. Pour le moment, ce tutoriel n’inclut pas d’instructions pour définir des ID de client. Consultez la documentation pour plus d’informations sur [la définition des ID de client](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference).

## Étapes de validation

Pour valider les appels vers Identity Service, exécutez l’exemple d’application dans Android Studio, ouvrez la console de débogage et recherchez la requête et la réponse :

1. **Requête à Identity Service** (filtrez Logcat sur `IdentityExtension`) Dans cet exemple, l’ID (`d_mid`) a déjà été défini et est juste signalé de nouveau)

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **Réponse d’Identity Service** (filtrez Logcat sur `IdentityExtension`). Notez comment la valeur `mid` correspond à la valeur `d_mid` dans la requête ci-dessus :

   ```java
   03-14 17:01:19.033 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Received ID response (mid: 59651426340521082405908216148091920022, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: 604800
   ```

[Suite : « Ajout de la prise en charge du VEC d’Adobe Target » &gt;](target-vec.md)
