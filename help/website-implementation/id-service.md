---
title: Mise en oeuvre d’Adobe Experience Platform Identity Service avec lancement
description: Découvrez comment ajouter l’extension Adobe Experience Platform Identity Service et utiliser l’action Définir les ID de client pour collecter les ID de client. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les sites Web avec lancement.
seo-description: null
seo-title: Mise en oeuvre d’Adobe Experience Platform Identity Service avec lancement
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Ajout du service d’identité Adobe Experience Platform

Cette leçon vous guidera tout au long des étapes requises pour implémenter l’extension [](https://docs.adobe.com/content/help/en/launch/using/extensions-ref/adobe-extension/id-service-extension/overview.html) Adobe Experience Platform Identity Service et envoyer les identifiants de client.

Le service [d’identité de la plate-forme](https://docs.adobe.com/content/help/en/id-service/using/home.html) Adobe Experience Platform définit un identifiant visiteur commun à toutes les solutions Adobe afin d’alimenter les fonctionnalités d’Experience Cloud, telles que le partage d’audience entre les solutions.  Vous pouvez également envoyer vos propres identifiants de client au Service pour activer le ciblage et les intégrations inter-périphériques avec votre système de gestion de la relation client (CRM).

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Ajout de l’extension Identity Service
* Créer un élément de données pour collecter les identifiants de client
* Créez une règle qui utilise l’action "Définir les ID de client" pour envoyer les ID de client à Adobe.
* Utiliser la fonction d’agencement des règles pour séquencer des règles qui se déclenchent sur le même événement

## Conditions préalables

You should have already completed the lessons in the [Configure Launch](launch.md) section.

## Ajout de l’extension Identity Service

Puisqu'il s'agit de la première extension que vous ajoutez, voici un aperçu rapide des extensions. Les extensions sont l’une des principales fonctionnalités de Launch. Une extension est une intégration créée par Adobe, un partenaire Adobe ou tout client Adobe qui ajoute de nouvelles options sans fin pour les balises que vous pouvez déployer sur votre site Web. Si vous considérez le lancement comme un système d’exploitation, les extensions sont les applications que vous installez, de sorte que Launch puisse faire ce que vous avez besoin de faire.

**Pour ajouter l’extension Identity Service**

1. In the top navigation, click **[!UICONTROL Extensions]**

1. Cliquez sur **[!UICONTROL Catalogue]** pour accéder à la page Catalogue des extensions.

   ![Accéder au catalogue des extensions](images/extensions-goToExtensionsCatalog.png)

1. Notez la diversité des extensions disponibles dans le catalogue

1. Dans le filtre en haut, tapez "id" pour filtrer le catalogue.

1. Sur la carte du service Adobe Experience Platform Identity Service, cliquez sur **[!UICONTROL Installer.]**

   ![Installation de l’extension Identity Service](images/idservice-install.png)

1. Notez que votre ID d’organisation Experience Cloud a été automatiquement détecté.

1. Laissez tous les paramètres par défaut et cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer.]**

   ![Enregistrer l’extension](images/idservice-save.png)

>[!NOTE] Chaque version de l’extension Identity Service est fournie avec une version spécifique de VisitorAPI.js qui est indiquée dans la description de l’extension. Vous mettez à jour la version de VisitorAPI.js en mettant à jour l’extension Identity Service.

### Validation de l’extension

L’extension Identity Service est l’une des rares extensions Launch à effectuer une requête sans avoir à utiliser une action de règle. L'extension envoie automatiquement une requête au service d'identité lors du premier chargement de page de la première visite sur un site Web. Une fois l’identifiant demandé, il est stocké dans un cookie propriétaire commençant par "AMCV_".

**Pour valider l’extension Identity Service**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md).

1. Dans l’onglet Résumé du débogueur, la section Lancement doit indiquer que l’extension Adobe Experience Platform Identity Service est implémentée.

1. En outre, dans l’onglet Summary (Résumé), la section Identity Service doit être renseignée avec le même ID d’organisation que celui qui se trouvait sur l’écran de configuration de l’extension dans l’interface Launch :

   ![Vérifiez que l’extension Adobe Experience Platform Identity Service est implémentée](images/idservice-debugger-summary.png)

1. La demande initiale de récupération de l’identifiant visiteur peut apparaître dans l’onglet Service d’identité du débogueur. Il se peut néanmoins qu’il ait déjà été demandé, donc ne vous inquiétez pas si vous ne le voyez pas:
   ![Vérifiez s’il existe une demande adressée au service d’identité avec votre ID d’organisation.](images/idservice-idRequest.png)

1. Après la demande initiale visant à récupérer l’identifiant visiteur, l’ID est stocké dans un cookie dont le nom commence par `AMCV_`. Vous pouvez vérifier que le cookie a été défini en procédant comme suit :
   1. Ouvrez les outils de développement de votre navigateur
   1. Go to the `Application` tab
   1. Expand `Cookies` on the left side
   1. Click on the domain `https://luma.enablementadobe.com`
   1. Recherchez le cookie AMCV_ sur le côté droit. Vous pouvez en voir plusieurs depuis le chargement du site Luma à l’aide de sa propriété Launch codée en dur et mappée sur la vôtre.
      ![Vérification du cookie AMCV_](images/idservice-AMCVCookie.png)

Vous avez terminé. Vous avez ajouté votre première extension ! Pour plus d'informations sur les options de configuration d'Identity Service, consultez [la documentation](https://docs.adobe.com/content/help/en/id-service/using/id-service-api/configurations/function-vars.html).

## Envoyer des ID de client

Ensuite, vous enverrez un ID [de](https://docs.adobe.com/content/help/en/id-service/using/reference/authenticated-state.html) client au service d’identité. This will allow you to [integrate your CRM](https://docs.adobe.com/content/help/en/core-services/interface/customer-attributes/attributes.html) with the Experience Cloud as well as track visitors across devices.

In the earlier lesson, [Add Data Elements, Rules, and Libraries](launch-data-elements-rules.md) you created a data element and used it in a rule. Désormais, vous utiliserez ces mêmes techniques pour envoyer un ID de client lorsque le visiteur est authentifié.

### Créer des éléments de données pour des ID de client

Commencez par créer deux éléments de données :

1. `Authentication State`—pour savoir si le visiteur est connecté ou non
1. `Email (Hashed)`: pour capturer la version hachée de l’adresse électronique (utilisée comme ID client) à partir de la couche de données.

**Pour créer l’élément de données pour l’état d’authentification**

1. Cliquez sur Eléments **[!UICONTROL de]** données dans la barre de navigation supérieure.
1. Cliquez sur le bouton **[!UICONTROL Ajouter un élément]** de données

   ![Cliquez sur "Ajouter un élément de données".](images/idservice-addDataElement1.png)

1. Nommez l’élément de données `Authentication State`
1. Pour le type **[!UICONTROL d’élément de]** données, sélectionnez Code **[!UICONTROL personnalisé.]**
1. Cliquez sur le bouton **[!UICONTROL Ouvrir l’éditeur]** .

   ![Ouvrez l’éditeur pour ajouter le code personnalisé de l’élément de données.](images/idservice-authenticationState.png)

1. Dans la fenêtre [!UICONTROL Modifier le code] , utilisez le code suivant pour renvoyer les valeurs de "connecté" ou "déconnecté" en fonction d’un attribut de la couche de données du site Luma :

   ```javascript
   if (digitalData.user[0].profile[0].attributes.loggedIn)
       return "logged in"
   else
       return "logged out"
   ```

1. Click **[!UICONTROL Save]** to save the custom code

   ![Enregistrez le code personnalisé](images/idservice-authenticationCode.png)

1. Conservez tous les autres paramètres à leurs valeurs par défaut.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer]** pour enregistrer l’élément de données et revenir à la page des éléments de données.

   ![Enregistrez l’élément de données](images/idservice-authenticationStateFinalSave.png)

En connaissant l'état d'authentification de l'utilisateur, vous savez quand un ID de client doit exister sur la page à envoyer au service d'identité. L’étape suivante consiste à créer un élément de données pour l’identifiant client lui-même. Sur le site de démonstration Luma, vous utiliserez la version hachée de l’adresse électronique du visiteur.

**Pour ajouter l’élément de données pour le courrier électronique haché**

1. Cliquez sur le bouton **[!UICONTROL Ajouter un élément]** de données

   ![ajouter un élément de données ;](images/idservice-addDataElement2.png)

1. Nommez l’élément de données `Email (Hashed)`
1. Pour le type **[!UICONTROL d’élément de]** données, sélectionnez Variable **[!UICONTROL JavaScript.]**
1. En tant que nom **[!UICONTROL de variable]** JavaScript, utilisez le pointeur suivant pour définir une variable dans la couche de données du site Luma : `digitalData.user.0.profile.0.attributes.username`
1. Conservez tous les autres paramètres à leurs valeurs par défaut.
1. Cliquez sur **[!UICONTROL Enregistrer dans la bibliothèque et Créer]** pour enregistrer l’élément de données.

   ![Enregistrez l’élément de données](images/idservice-emailHashed.png)

### Ajouter une règle pour envoyer des ID de client

Le service d’identité d’Adobe Experience Platform transmet les ID de client dans les règles à l’aide d’une action appelée "Définir les ID de client".  Vous allez maintenant créer une règle pour déclencher cette action lorsque le visiteur est authentifié.

**Pour créer une règle pour envoyer les ID de client**

1. In the top navigation, click **[!UICONTROL Rules]**
1. Cliquez sur **[!UICONTROL Ajouter une règle]** pour ouvrir le Créateur de règles.

   ![Ajouter une règle](images/idservice-addRule.png)

1. Attribuez un nom à la règle `All Pages - Library Loaded - Authenticated - 10`

   >[!TIP] Cette convention d’affectation de nom indique que vous déclenchez cette règle en haut de toutes les pages lorsque l’utilisateur est authentifié et qu’elle aura la valeur "10". L’utilisation d’une convention d’affectation de nom comme celle-ci (au lieu de l’attribuer aux solutions déclenchées dans les actions) vous permettra de minimiser le nombre total de règles nécessaires à votre implémentation.

1. Under **[!UICONTROL Events]** click **[!UICONTROL Add]**

   ![Ajouter un événement](images/idservice-customerId-addEvent.png)

   1. For the **[!UICONTROL Event Type]** select **[!UICONTROL Library Loaded (Page Top)]**
   1. For the  **[!UICONTROL Order]** enter `10`. La commande contrôle la séquence de règles déclenchées par le même événement. Les règles avec un ordre inférieur se déclenchent avant les règles avec un ordre supérieur. In this case, you want to set the customer ID before you fire the Target request, which you will do in the next lesson with a rule with an order of `50` .
   1. Cliquez sur le bouton **[!UICONTROL Conserver les modifications]** pour revenir au Créateur de règles.
   ![Enregistrer l’événement](images/idservice-customerId-saveEvent.png)

1. Sous **[!UICONTROL Conditions]** , cliquez sur **[!UICONTROL Ajouter.]**

   ![Ajouter une condition à la règle](images/idservice-customerId-addCondition.png)

   1. Pour le type **[!UICONTROL de]** condition, sélectionnez Comparaison des **[!UICONTROL valeurs.]**
   1. Click the ![data element icon](images/icon-dataElement.png) icon to open the Data Element modal

      ![ouvrir le module de l’élément de données](images/idservice-customerId-valueComparison.png)

   1. Dans le modèle de l’élément de données, cliquez sur État **[!UICONTROL d’]** authentification, puis sur **[!UICONTROL Sélectionner.]**

      ![définir l’état d’authentification](images/idservice-customerId-authStateCondition.png)

1. Make sure `Equals` is the operator
1. Tapez "connecté" dans le champ de texte, ce qui entraîne le déclenchement de la règle lorsque l’élément de données "État d’authentification" a la valeur "connecté".

1. Click **[!UICONTROL Keep Changes]**

   ![Enregistrer la condition](images/idservice-customerId-loggedIn.png)

1. Sous **[!UICONTROL Actions]** , cliquez sur **[!UICONTROL Ajouter.]**

   ![Ajouter une nouvelle action](images/idservice-customerId-addAction.png)

   1. Pour l’ **[!UICONTROL extension]** , sélectionnez **[!UICONTROL Adobe Experience Platform Identity Service.]**
   1. Pour le type **[!UICONTROL d’]** action **[!UICONTROL , sélectionnezDéfinir les ID de client.]**
   1. Pour le code **[!UICONTROL d’]** intégration, saisissez `crm_id`
   1. Pour la **[!UICONTROL valeur]** , saisissez l’option d’ouverture modale du sélecteur d’éléments de données et sélectionnez le `Email (Hashed)`
   1. Pour l’état **** Auth, sélectionnez **[!UICONTROL Authentifié.]**
   1. Click the **[!UICONTROL Keep Changes]** button to save the action and return to the Rule Builder

      ![Configurez l’action et enregistrez les modifications](images/idservice-customerId-action.png)

1. Cliquez sur le bouton **[!UICONTROL Enregistrer dans la bibliothèque et Créer]** pour enregistrer la règle.

   ![Enregistrer la règle](images/idservice-customerId-saveRule.png)

Vous avez maintenant créé une règle qui enverra l’ID de client sous forme de variable `crm_id` lorsque le visiteur est authentifié. Puisque vous avez spécifié la Commande comme `10` cette règle se déclenchera avant votre `All Pages - Library Loaded` règle créée dans la leçon [Ajouter des éléments de données, des règles et des bibliothèques](launch-data-elements-rules.md) qui utilise la valeur de commande par défaut de `50`.

### Validation des ID de client

Pour valider votre travail, vous vous connecterez au site Luma pour confirmer le comportement de la nouvelle règle.

**Pour vous connecter au site Luma**

1. Open the [Luma site](https://luma.enablementadobe.com/content/luma/us/en.html)

1. Make sure the Debugger is mapping the Launch property to *your* Development environment, as described in the [earlier lesson](launch-switch-environments.md)

   ![L’environnement de développement de votre lancement affiché dans le débogueur](images/switchEnvironments-debuggerOnWeRetail.png)

1. Cliquez sur le lien **[!UICONTROL CONNEXION]** dans le coin supérieur droit du site Luma.

   ![Cliquez sur Connexion dans la barre de navigation supérieure.](images/idservice-loginNav.png)

1. Entrez `test@adobe.com` comme nom d'utilisateur
1. Entrez `test` le mot de passe
1. Cliquez sur le bouton **[!UICONTROL CONNEXION]** .

   ![Entrez les informations d’identification et cliquez sur Se connecter.](images/idservice-login.png)

1. Revenez à la page d’accueil

Maintenant, confirmez que l’ID client est envoyé au service à l’aide de l’extension Débogueur.

**Pour vérifier que le service d’identité transmet l’identifiant du client**

1. Assurez-vous que l'onglet du site Luma est activé
1. Dans le débogueur, accédez à l’onglet Adobe Experience Platform Identity Service.
1. Développez votre ID d’organisation.
1. Click on the cell with the `Customer ID - crm_id` value
1. In the modal, note the customer id value and that the `AUTHENTICATED` state is reflected:

   ![Confirmation de l’ID de client dans le débogueur](images/idservice-debugger-confirmCustomerId.png)

1. Notez que vous pouvez confirmer la valeur de courrier électronique haché en affichant le code source de la page Luma et en observant la propriété username. Elle doit correspondre à la valeur affichée dans le débogueur :

   ![adresse électronique hachée dans le code source](images/idservice-customerId-inSourceCode.png)

### Autres conseils de validation

Launch dispose également de fonctions de journalisation de la console riches. Pour les activer, accédez à l'onglet **[!UICONTROL Outils]** du débogueur et activez la bascule Connexion **[!UICONTROL à la console de]** lancement.

![Basculer sur la journalisation de la console du lancement](images/idservice-debugger-logging.png)

La connexion à la console s’active dans la console de votre navigateur et dans l’onglet Journaux du débogueur. Vous devriez voir la journalisation de toutes les règles que vous avez créées jusqu’à présent! Notez que de nouvelles entrées de journal sont ajoutées en haut de la liste. Par conséquent, votre règle "Toutes les pages - Bibliothèque chargée - Authentifiée - 10" doit se déclencher avant la règle "Toutes les pages - Bibliothèque chargée" et apparaître en dessous dans la journalisation de la console du débogueur :

![Onglet Journaux du débogueur](images/idservice-debugger-loggingStatements.png)

[Suivant : "Ajouter Adobe Target" &gt;](target.md)
