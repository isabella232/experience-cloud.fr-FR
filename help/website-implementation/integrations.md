---
title: Mise en œuvre des intégrations Experience Cloud avec Launch
description: Découvrez comment valider les intégrations d’Audiences, d’A4T et des attributs du client dans votre mise en œuvre d’Adobe Experience Cloud. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les sites web avec Launch.
seo-description: null
seo-title: Mise en œuvre des intégrations Experience Cloud avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: e9dee6d0aa3b775d0ce617e2b2c57b56de491b8d

---


# Intégrations Experience Cloud

Dans cette leçon, vous examinerez les intégrations clés des solutions que vous venez de mettre en œuvre. La bonne nouvelle, c’est qu’en suivant les leçons précédentes, vous avez déjà mis en œuvre les aspects des intégrations en lien avec le code. Vous n’avez pas besoin d’en faire plus pour cette leçon, hormis lire et valider.

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

1. expliquer les cas d’utilisation de base pour les intégrations du partage des audiences, d’Analytics for Target (A4T) et des attributs du client ;
1. valider les aspects de base de la mise en œuvre de ces intégrations côté client.

## Conditions préalables

Vous devez suivre les leçons précédentes de ce tutoriel avant de suivre les instructions de cette leçon.

>[!NOTE] Il existe de nombreuses exigences d’autorisation des utilisateurs, ainsi que des étapes de configurations de compte et d’attribution des privilèges d’accès requises pour utiliser pleinement ces intégrations et qui vont au-delà de ce tutoriel. Si vous n’utilisez pas déjà ces intégrations dans votre mise en œuvre actuelle d’Experience Cloud, tenez compte des points suivants :
>
> * Examinez toutes les exigences relatives aux [intégrations des services principaux](https://docs.adobe.com/content/help/fr-FR/core-services/interface/about-core-services/core-services.html).
> * Examinez toutes les exigences relatives à l’[intégration Analytics for Target](https://docs.adobe.com/content/help/fr-FR/target/using/integrate/a4t/before-implement.html).
> * Demandez à un administrateur de votre organisation Experience Cloud [ de demander l’attribution des privilèges d’accès de ces intégrations](https://www.adobe.com/go/audiences)


## Audiences

[Audiences](https://docs.adobe.com/content/help/en/core-services/interface/audiences/audience-library.htm) fait partie du service principal People et vous permet de partager des audiences entre différentes solutions. Par exemple, vous pouvez créer une audience dans Audience Manager et l’utiliser pour diffuser du contenu personnalisé avec Target.

Les principales exigences pour la mise en œuvre d’A4T (ce que vous avez déjà fait) sont les suivantes :

1. La mise en œuvre d’Adobe Experience Platform Identity Service
1. La mise en œuvre d’Audience Manager
1. La mise en œuvre d’autres solutions desquelles vous souhaitez recevoir ou créer des audiences, comme Target et Analytics

### Validation de l’intégration d’Audiences

La meilleure façon de valider l’intégration d’Audiences consiste à créer une audience, à la partager avec une autre solution, puis à l’utiliser pleinement dans l’autre solution (par exemple, en confirmant qu’un visiteur admissible pour un segment AAM peut être admissible pour une activité Target ciblée pour ce segment). Toutefois, cela va au-delà de la portée de ce tutoriel.

Ces étapes de validation portent sur la partie essentielle visible dans la mise en œuvre côté client : l’ID visiteur.

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Accédez à l’onglet Réseau du débogueur.

1. Cliquez sur **[!UICONTROL Effacer toutes les requêtes]** pour nettoyer les éléments.

1. Chargez à nouveau la page Luma, en veillant à afficher les requêtes Target et Analytics dans le débogueur.

1. Chargez encore une fois la page Luma.

1. Vous devriez maintenant avoir quatre requêtes dans l’onglet Réseau du débogueur : deux pour Target et deux pour Analytics.

1. Regardez dans la ligne « Identifiant visiteur Experience Cloud ». Les ID de chaque requête pour chaque solution doivent toujours être identiques.

   ![Confirmation des SDID correspondants](images/integrations-matchingECIDs.png)

1. Les ID sont propres à chaque visiteur, ce que vous pouvez vérifier en demandant à un collègue de répéter ces étapes.

## Analytics for Target (A4T)

L’intégration [Analytics for Target (A4T)](https://docs.adobe.com/content/help/fr-FR/target/using/integrate/a4t/a4t.html) vous permet d’exploiter vos données Analytics en tant que source des mesures de création de rapports dans Target.

Les principales exigences pour la mise en œuvre d’A4T (ce que vous avez déjà fait) sont les suivantes :

1. La mise en œuvre d’Adobe Experience Platform Identity Service
1. Le déclenchement de la requête pendant le chargement de la page Target avant celui de la balise de page vue Analytics

A4T fonctionne en assemblant une requête côté serveur de Target à Analytics avec la balise de page vue Analytics, c’est l’« assemblage d’accès ». L’assemblage d’accès nécessite que la requête Target qui envoie l’activité (ou incrémente une mesure d’objectif basée sur Target) ait un paramètre correspondant à un paramètre dans la balise de page vue Analytics. Ce paramètre est appelé ID de données supplémentaire (SDID).

### Validation de la mise en œuvre d’A4T

Le meilleur moyen de valider l’intégration A4T est de créer une activité Target à l’aide d’A4T et de valider les données de création de rapports. Toutefois, cela va au-delà de la portée de ce tutoriel. Ce tutoriel montre comment vérifier que les identifiants de données supplémentaires correspondent entre les appels de solution.

**Validation des SDID**

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Accédez à l’onglet Réseau du débogueur.

1. Cliquez sur **[!UICONTROL Effacer toutes les requêtes]** pour nettoyer les éléments.

1. Chargez à nouveau la page Luma, en veillant à afficher les requêtes Target et Analytics dans le débogueur.

1. Chargez encore une fois la page Luma.

1. Vous devriez maintenant avoir quatre requêtes dans l’onglet Réseau du débogueur : deux pour Target et deux pour Analytics.

1. Regardez dans la ligne intitulée « ID de données supplémentaire ». Les identifiants du premier chargement de page doivent correspondre entre Target et Analytics. Les identifiants du second chargement de page doivent également correspondre, mais être différents par rapport au premier chargement de page.

   ![Confirmation des SDID correspondants](images/integrations-matchingSDIDs.png)

Si vous effectuez d’autres requêtes Target dans le cadre d’un chargement de page (et non d’applications d’une seule page) qui font partie des activités A4T, il est conseillé de leur attribuer des noms uniques (pas target-global-mbox) afin qu’elles possèdent les mêmes SDID des requêtes Target et Analytics initiales.

## Attributs du client

Les [attributs du client](https://docs.adobe.com/content/help/fr-FR/core-services/interface/customer-attributes/attributes.html) font partie du service principal People qui vous permet de charger des données à partir de votre base de données de gestion de la relation client (GRC) et de les exploiter dans Adobe Analytics et Adobe Target.

Les principales exigences pour la mise en œuvre des attributs du client ce que vous avez déjà fait sont les suivantes :

1. La mise en œuvre d’Adobe Experience Platform Identity Service
1. La définition des ID de client par le biais du service d’identité *avant que* Target et Analytics ne déclenchent leurs requêtes (ce que vous avez exécuté à l’aide de la fonction d’agencement des règles dans Launch)

### Validation de la mise en œuvre des attributs du client

Vous avez déjà vérifié que les ID de client sont transmis à Identity Service et à Target dans les leçons précédentes. Vous pouvez également valider l’ID de client dans l’accès à Analytics.
Actuellement, l’ID de client est l’un des rares paramètres qui ne s’affichent pas dans l’Experience Cloud Debugger, vous devez donc utiliser la console JavaScript du navigateur pour l’afficher.

1. Ouvrez le site Luma.
1. Ouvrez les outils de développement de votre navigateur.
1. Accédez à l’onglet Réseau.
1. Dans le champ de filtre, saisissez `b/ss`. Cela limite ce que vous voyez aux requêtes Adobe Analytics.

   ![Ouverture de l’onglet des outils de développement et de filtrage du réseau pour afficher uniquement les requêtes Analytics](images/aam-openTheJSConsole.png)

1. Cliquez sur le lien **[!UICONTROL Connexion]** dans le coin supérieur droit du site.

   ![Clic sur Connexion dans le volet de navigation supérieur](images/idservice-loginNav.png)

1. Saisissez `test@adobe.com` comme nom d’utilisateur.
1. Saisissez `test` comme mot de passe.
1. Cliquez sur le bouton **[!UICONTROL Connexion]**.

   ![Saisie des informations d’identification et clic sur Connexion](images/idservice-login.png)

1. Cela devrait vous renvoyer à la page d’accueil et déclencher une balise visible dans les outils de développement. Si vous êtes renvoyé vers la page d’informations du compte, cliquez sur le logo WE.RETAIL pour retourner à la page d’accueil.
1. Cliquez sur la requête et sélectionnez l’onglet En-têtes.
1. Faites défiler l’écran vers le bas jusqu’à ce que certains paramètres imbriqués s’affichent :
   1. cid : le délimiteur standard pour la partie ID de client de la requête
   1. crm_id : le code d’intégration personnalisé, que vous avez spécifié dans la leçon Identity Service
   1. id : la valeur d’ID de client provenant de votre élément de données `Email (Hashed)`
   1. as : l’état d’authentification, avec « 1 » signifiant connecté
   ![Validation de l’ID de client Analytics](images/integrations-analyticsCustomerIDValidation.png)

[Suite : « Publication de la propriété » &gt;](publish.md)