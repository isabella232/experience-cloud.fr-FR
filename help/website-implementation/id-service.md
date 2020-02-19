---
title: Mise en œuvre d’Adobe Experience Platform Identity Service avec Launch
description: Découvrez comment ajouter l’extension Adobe Experience Platform Identity Définition des ID de client pour collecter les ID de client. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les sites web avec Launch.
seo-description: null
seo-title: Mise en œuvre d’Adobe Experience Platform Identity Service avec Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajout d’Adobe Experience Platform Identity Service

Cette leçon vous guidera tout au long des étapes requises pour mettre en œuvre l’[extension Adobe Experience Platform Identity Service](https://docs.adobe.com/content/help/fr-FR/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html) et envoyer des ID de client.

[Adobe Experience Platform Identity Service](https://docs.adobe.com/content/help/fr-FR/id-service/using/home.html) définit un identifiant visiteur commun à toutes les solutions Adobe de façon à alimenter les fonctionnalités d’Experience Cloud, telles que le partage d’audience entre solutions.  Vous pouvez également envoyer vos propres ID au service pour permettre un ciblage entre les appareils et des intégrations supplémentaires avec votre système de gestion de la relation client (CRM).

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

* ajouter l’extension Identity Service ;
* créer un élément de données pour recueillir vos ID de client ;
* créer une règle qui utilise l’action « Définition des ID de client » pour envoyer les ID de client à Adobe;
* utiliser la fonction d’agencement des règles pour séquencer des règles qui se déclenchent sur le même événement.

## Conditions préalables

Vous devez avoir terminé les leçons de la section [Configuration de Launch](launch.md).

## Ajout de l’extension Identity Service

Puisqu’il s’agit de la première extension que vous ajoutez, voici un résumé rapide de ce que sont les extensions. Les extensions sont l’une des principales caractéristiques de Launch. Une extension est une intégration construite par Adobe, ses partenaires ou ses clients et qui ajoute un nombre illimité de nouvelles options pour les balises que vous pouvez déployer sur vos sites web. Si vous considérez Launch comme un système d’exploitation, les extensions sont les applications que vous installez afin que le système puisse effectuer les actions dont vous avez besoin.

**Ajout de l’extension Identity Service**

1. Dans la barre de navigation supérieure, cliquez sur **[!UICONTROL Extensions]**.

1. Cliquez sur **[!UICONTROL Catalogue]** pour accéder au catalogue des extensions.

   ![Consulter le catalogue des extensions](images/extensions-goToExtensionsCatalog.png)

1. Notez que de nombreuses extensions sont disponibles dans le catalogue.

1. Dans la partie supérieure, dans les filtres, saisissez « id » pour filtrer les extensions.

1. Sur la carte d’Adobe Experience Platform Identity Service, cliquez sur **[!UICONTROL Installer]**.

   ![Installation de l’extension Identity Service](images/idservice-install.png)

1. Notez que votre ID d’organisation Experience Cloud a été automatiquement détecté.

1. Laissez tous les paramètres par défaut et cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et créer]**.

   ![Enregistrement de l’extension](images/idservice-save.png)

>[!NOTE] Chaque version de l’extension Identity Service comprend une version spécifique de VisitorAPI.js qui est indiquée dans la description de l’extension. La version de VisitorAPI.js est mise à jour en même temps que l’extension.

### Validation de l’extension

L’extension Identity Service est l’une des rares extensions de Launch qui émet une requête sans avoir à utiliser une action de règle. L’extension émet automatiquement une requête à Identity Service lors du premier chargement de page à l’occasion de la première consultation d’un site web. Une fois l’identifiant demandé, il est stocké dans un cookie propriétaire commençant par « AMCV_ ».

**Validation de l’extension Identity Service**

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

1. Dans l’onglet Synthèse du débogueur, la section Launch doit indiquer que l’extension Adobe Experience Platform Identity Service est mise en œuvre.

1. De même, dans l’onglet Synthèse, la section Identity Service doit renseigner le même ID d’organisation que celui affiché sur votre écran de configuration de l’extension dans l’interface de Launch :

   ![Vérifiez que l’extension Adobe Experience Platform Identity Service est mise en œuvre.](images/idservice-debugger-summary.png)

1. La demande initiale de récupération de l’identifiant visiteur peut apparaître dans l’onglet Identity Service du débogueur. Il se peut néanmoins qu’il ait déjà été demandé, donc ne vous inquiétez pas si vous ne le voyez pas :
   ![Vérifiez s’il existe une demande adressée à Identity Service avec votre ID d’organisation.](images/idservice-idRequest.png)

1. Après la demande initiale visant à récupérer l’identifiant visiteur, l’ID est stocké dans un cookie dont le nom commence par `AMCV_`. Vous pouvez vérifier que le cookie a été défini en procédant comme suit :
   1. Ouvrez les outils de développement de votre navigateur.
   1. Accédez à l’onglet `Application`.
   1. Développez l’élément `Cookies` sur la gauche
   1. Cliquez sur le domaine `https://luma.enablementadobe.com`
   1. Recherchez le cookie AMCV_ sur la droite. Vous pouvez en voir plusieurs, puisque le site Luma a été chargé à l’aide de sa propriété Launch codée en dur et mappée sur la vôtre.
      ![Vérifiez le cookie AMCV_](images/idservice-AMCVCookie.png)

Vous avez terminé. Vous avez ajouté votre première extension ! Pour plus d’informations sur les options de configuration d’Identity Service, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/id-service/using/id-service-api/configurations/function-vars.html).

## Envoi des ID de client

Ensuite, vous enverrez un [ID de client](https://docs.adobe.com/content/help/fr-FR/id-service/using/reference/authenticated-state.html) à Identity Service. Cela vous permet d’[intégrer votre CRM](https://docs.adobe.com/content/help/fr-FR/core-services/interface/customer-attributes/attributes.html) à Experience Cloud et de suivre les visiteurs sur différents périphériques.

Dans une leçon précédente, [Ajout d’éléments de données, de règles et de bibliothèques](launch-data-elements-rules.md), vous avez créé un élément de données et vous l’avez utilisé dans une règle. Maintenant, vous allez utiliser ces mêmes techniques pour envoyer un ID de client lorsque le visiteur est authentifié.

### Créer des éléments de données pour des ID de client

Commencez par créer deux éléments de données :

1. `Authentication State` : pour savoir si le visiteur est connecté ou non.
1. `Email (Hashed)` : pour capturer la version hachée de l’adresse électronique (utilisée comme ID de client) à partir de la couche de données.

**Création de l’élément de données pour l’état d’authentification**

1. Cliquez sur **[!UICONTROL Éléments de données]** dans la barre de navigation supérieure.
1. Cliquez sur le bouton **[!UICONTROL Ajouter un élément de données]**.

   ![Cliquez sur « Ajouter un élément de données »](images/idservice-addDataElement1.png).

1. Nommez l’élément de données `Authentication State`.
1. Pour le champ **[!UICONTROL Type d’élément de données]**, sélectionnez **[!UICONTROL Code personnalisé]**.
1. Cliquez sur le bouton **[!UICONTROL Ouvrir l’éditeur]**.

   ![Ouvrez l’éditeur pour ajouter le code personnalisé de l’élément de données.](images/idservice-authenticationState.png)

1. Dans la fenêtre [!UICONTROL Modifier le code], utilisez le code suivant pour renvoyer les valeurs de « connecté » ou « déconnecté » en fonction d’un attribut de la couche de données du site Luma :

   ```javascript
   if (digitalData.user[0].profile[0].attributes.loggedIn)
       return "logged in"
   else
       return "logged out"
   ```

1. Cliquez sur **[!UICONTROL Enregistrer]** pour enregistrer le code personnalisé.

   ![Enregistrez le code personnalisé](images/idservice-authenticationCode.png)

1. Conservez tous les autres paramètres à leurs valeurs par défaut.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et créer]** pour enregistrer l’élément de données et revenir à la page des éléments de données.

   ![Enregistrez l’élément de données](images/idservice-authenticationStateFinalSave.png)

En connaissant l’état d’authentification de l’utilisateur, vous savez quand un ID de client doit exister sur la page à envoyer à Identity Service. L’étape suivante consiste à créer un élément de données pour l’ID de client lui-même. Sur le site de démonstration Luma, vous allez utiliser la version hachée de l’adresse électronique du visiteur.

**Ajout de l’élément de données pour le courrier électronique haché**

1. Cliquez sur le bouton **[!UICONTROL Ajouter un élément de données]**.

   ![Ajouter un élément de données](images/idservice-addDataElement2.png)

1. Nommez l’élément de données `Email (Hashed)`.
1. Pour le champ **[!UICONTROL Type d’élément de données]**, sélectionnez **[!UICONTROL Variable JavaScript]**.
1. Pour le champ **[!UICONTROL Nom de variable JavaScript]**, utilisez le pointeur suivant pour indiquer une variable dans la couche de données du site Luma : `digitalData.user.0.profile.0.attributes.username`.
1. Conservez tous les autres paramètres à leurs valeurs par défaut.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et créer]** pour enregistrer l’élément de données.

   ![Enregistrez l’élément de données](images/idservice-emailHashed.png)

### Ajout d’une règle pour envoyer des ID de client

Adobe Experience Platform Identity Service transmet les ID de client dans les règles à l’aide d’une action appelée « Définition des ID de client ».  Vous allez maintenant créer une règle pour déclencher cette action lorsque le visiteur est authentifié.

**Ajout d’une règle pour envoyer des ID de client**

1. Dans la barre de navigation supérieure, cliquez sur **[!UICONTROL Règles]**.
1. Cliquez sur **[!UICONTROL Ajouter une règle]** pour ouvrir le créateur de règles.

   ![Ajouter une règle](images/idservice-addRule.png)

1. Donnez à la règle le nom `All Pages - Library Loaded - Authenticated - 10`.

   >[!TIP] Cette convention d’affectation des noms indique que vous déclenchez cette règle en haut de toutes les pages lorsque l’utilisateur est authentifié et qu’elle aura une commande de « 10 ». L’utilisation d’une convention d’affectation des noms comme celle-ci au lieu de l’affecter aux solutions déclenchées dans les actions vous permettra de minimiser le nombre total de règles nécessaires à votre mise en œuvre.

1. Sous **[!UICONTROL Événements]**, cliquez sur **[!UICONTROL Ajouter]**.

   ![Ajout d’un événement](images/idservice-customerId-addEvent.png)

   1. Dans le champ **[!UICONTROL Type d’événement]**, sélectionnez **[!UICONTROL Chargé par bibliothèque, haut de page]**.
   1. Dans le champ **[!UICONTROL Commande]**, saisissez `10`. La commande contrôle la séquence de règles déclenchées par le même événement. Les règles dont la commande est plus faible se déclenchent avant les règles dont la commande est plus élevée. Dans ce cas, vous souhaitez définir l’ID de client avant de déclencher la requête, ce que vous allez faire lors de la leçon suivante avec une règle dont la commande est de `50`.
   1. Cliquez sur le bouton **[!UICONTROL Conserver les modifications]** pour revenir au créateur de règles.
   ![Enregistrement de l’événement](images/idservice-customerId-saveEvent.png)

1. Sous **[!UICONTROL Conditions]**, cliquez sur **[!UICONTROL Ajouter]**.

   ![Ajout d’une condition à la règle](images/idservice-customerId-addCondition.png)

   1. Pour le champ **[!UICONTROL Type de condition]**, sélectionnez **[!UICONTROL Comparaison des valeurs]**.
   1. Cliquez sur l’icône ![icône d’élément de données](images/icon-dataElement.png) pour ouvrir le modal d’élément de données.

      ![Ouvrir le modal d’élément de données](images/idservice-customerId-valueComparison.png)

   1. Dans le modal d’élément de données, cliquez sur **[!UICONTROL État d’authentification]**, puis sur **[!UICONTROL Sélectionner]**.

      ![Définir l’état d’authentification](images/idservice-customerId-authStateCondition.png)

1. Assurez-vous que l’opérateur est `Equals`.
1. Saisissez « connecté » dans le champ de texte, ce qui entraîne le déclenchement de la règle lorsque l’élément de données « État d’authentification » a la valeur « connecté ».

1. Cliquez sur **[!UICONTROL Conserver les modifications]**.

   ![Enregistrement de la condition](images/idservice-customerId-loggedIn.png)

1. Sous **[!UICONTROL Actions]**, cliquez sur **[!UICONTROL Ajouter]**.

   ![Ajout d’une nouvelle action](images/idservice-customerId-addAction.png)

   1. Pour le champ **[!UICONTROL Extension]**, sélectionnez **[!UICONTROL Adobe Experience Platform Identity Service]**.
   1. Pour le champ **[!UICONTROL Type d’action]**, sélectionnez **[!UICONTROL Définition des ID de client]**.
   1. Pour le champ **[!UICONTROL Code d’intégration]**, saisissez `crm_id`.
   1. Pour le champ **[!UICONTROL Valeur]**, saisissez l’option d’ouverture du modal du sélecteur d’élément de données et sélectionnez `Email (Hashed)`.
   1. Pour le champ **[!UICONTROL État d’authentification]**, sélectionnez **[!UICONTROL Authentifié]**.
   1. Cliquez sur le bouton **[!UICONTROL Conserver les modifications]** pour enregistrer l’action et revenir au créateur de règles.

      ![Configuration de l’action et enregistrement des modifications](images/idservice-customerId-action.png)

1. Cliquez sur le bouton **[!UICONTROL Enregistrer dans la bibliothèque et créer]** pour enregistrer la règle.

   ![Enregistrement de la règle](images/idservice-customerId-saveRule.png)

Vous avez maintenant créé une règle qui enverra l’ID de client sous forme de variable `crm_id` lorsque le visiteur est authentifié. Puisque vous avez spécifié une commande de `10`, cette règle se déclenchera avant votre règle `All Pages - Library Loaded` créée dans la leçon [Ajout d’éléments de données, de règles et de bibliothèques](launch-data-elements-rules.md), qui utilise la commande par défaut de `50`.

### Validation des ID de client

Pour valider votre travail, vous devez vous connecter au site Luma pour confirmer le comportement de la nouvelle règle.

**Connexion au site Luma**

1. Ouvrez le [site Luma](https://luma.enablementadobe.com/content/luma/us/en.html).

1. Veillez à ce que le débogueur mappe la propriété Launch à *votre* environnement de développement, comme décrit dans la [leçon précédente](launch-switch-environments.md).

   ![Votre environnement de développement Launch affiché dans Debugger](images/switchEnvironments-debuggerOnWeRetail.png)

1. Cliquez sur **[!UICONTROL CONNEXION]** dans le coin supérieur droit du site Luma.

   ![Cliquez sur Connexion dans le volet de navigation supérieur](images/idservice-loginNav.png)

1. Saisissez `test@adobe.com` comme nom d’utilisateur.
1. Saisissez `test` comme mot de passe.
1. Cliquez sur le bouton **[!UICONTROL CONNEXION]**.

   ![Entrez les informations d’identification et cliquez sur le bouton de connexion](images/idservice-login.png)

1. Revenez à la page d’accueil.

Vérifiez ensuite que l’ID de client est envoyé au service à l’aide de l’extension Debugger.

**Vérifier qu’Identity Service transmet l’ID de client**

1. Assurez-vous que l’onglet avec le site Luma est bien ciblé.
1. Dans Debugger, accédez à l’onglet Adobe Experience Platform Identity Service.
1. Développez votre ID d’organisation.
1. Cliquez sur la cellule avec la valeur `Customer ID - crm_id`.
1. Dans le modal, notez la valeur de l’ID de client et vérifiez que l’état `AUTHENTICATED` est reflété :

   ![Vérification de l’ID de client dans Debugger](images/idservice-debugger-confirmCustomerId.png)

1. Notez que vous pouvez confirmer la valeur de l’e-mail haché en affichant le code source de la page Luma et en observant la propriété username. Elle doit correspondre à la valeur affichée dans Debugger :

   ![Adresse électronique hachée dans le code source](images/idservice-customerId-inSourceCode.png)

### Autres conseils de validation

Launch dispose également de fonctions de journalisation de la console enrichies. Pour les activer, accédez à l’onglet **[!UICONTROL Outils]** de Debugger et activez l’option **[!UICONTROL Journalisation de la console Launch]**.

![Activation de la journalisation de la console Launch](images/idservice-debugger-logging.png)

Cela permet d’activer la journalisation de la console, à la fois dans la console de votre navigateur et dans l’onglet Journaux du débogueur. Vous devriez voir la journalisation de toutes les règles que vous avez créées jusqu’à présent. Notez que de nouvelles entrées de journal sont ajoutées dans la partie supérieure de la liste. Dès lors, votre règle « Toutes les pages - Bibliothèque chargée - Authentifiée - 10 » doit se déclencher avant la règle « Toutes les pages - Bibliothèque chargée » et apparaît en dessous de celle-ci dans la journalisation de la console du débogueur :

![Onglet Journaux du débogueur](images/idservice-debugger-loggingStatements.png)

[Suite : « Ajout d’Adobe Target » &gt;](target.md)
