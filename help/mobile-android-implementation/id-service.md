---
title: Mise en oeuvre d’Adobe Experience Platform Identity Service avec lancement
description: Découvrez comment ajouter l’extension Adobe Experience Platform Identity Service et utiliser l’action Définir les ID de client pour collecter les ID de client. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications mobiles Android.
seo-description: null
seo-title: Mise en oeuvre d’Adobe Experience Platform Identity Service avec lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Ajout du service d’identité Adobe Experience Platform

Le service [d’identité de la plate-forme](https://docs.adobe.com/content/help/en/id-service/using/home.html) Adobe Experience Platform définit un identifiant visiteur commun à toutes les solutions Adobe afin d’alimenter les fonctionnalités d’Experience Cloud, telles que le partage d’audience entre les solutions.  Vous pouvez également envoyer vos propres identifiants de client au Service pour activer le ciblage et les intégrations inter-périphériques avec votre système de gestion de la relation client (CRM).

Le service Identity Service fait partie de l'extension Mobile Core, ***donc vous l'avez déjà implémentée lorsque vous avez installé le kit SDK*** Mobile ! Pour le moment, ce didacticiel n’inclut pas d’instructions sur la définition des ID de client. Consultez la documentation pour plus d’informations sur [la définition des identifiants](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference)de client.

## Etapes de validation

Pour valider les appels vers Identity Service, exécutez l’exemple d’application dans Android Studio, ouvrez la console de débogage et recherchez la requête et la réponse :

1. **Demande au service** d’identité (filtrer Logcat pour `IdentityExtension`) Dans cet exemple, l’ID (`d_mid`) a déjà été défini et est simplement de nouveau signalé)

   ```java
   03-14 17:01:18.526 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Sending request (https://dpm.demdex.net/id?d_mid=59651426340521082405908216148091920022&d_ver=2&d_orgid=7ABB3E6A5A7491460A495D61%40AdobeOrg)
   ```

1. **Réponse du service** d’identité (filtrez Logcat à `IdentityExtension`). Notez comment la `mid` valeur correspond à la `d_mid` valeur dans la requête ci-dessus :

   ```java
   03-14 17:01:19.033 7743-7803/com.adobe.busbooking D/ADBMobile: IdentityExtension - Received ID response (mid: 59651426340521082405908216148091920022, blob: j8Odv6LonN4r3an7LhD3WZrU1bUpAkFkkiY1ncBR96t2PTI, hint: 9, ttl: 604800
   ```

[Suivant "Ajouter la prise en charge du compositeur d’expérience visuelle Adobe Target" &gt;](target-vec.md)
