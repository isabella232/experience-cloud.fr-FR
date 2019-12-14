---
title: Ajout du compositeur d’expérience visuelle (VEC) d’Adobe Target
description: Découvrez comment mettre en oeuvre le compositeur d’expérience visuelle (VEC) de Target avec des paramètres personnalisés, le combiner avec votre périphérique et créer une activité. Cette leçon fait partie du didacticiel Mise en oeuvre d’Experience Cloud dans les applications mobiles Android.
seo-description: null
seo-title: Ajout du compositeur d’expérience visuelle (VEC) d’Adobe Target
solution: Experience Cloud
translation-type: tm+mt
source-git-commit: 79d5274d09d66b80a49aba5db3e0a997d0f0ff61

---


# Ajout du compositeur d’expérience visuelle (VEC) d’Adobe Target

Dans cette leçon, vous allez activer le compositeur d’expérience visuelle Target (VEC) pour les applications mobiles.

[Adobe Target](https://docs.adobe.com/content/help/en/target/using/target-home.html) est la solution Adobe Experience Cloud qui fournit tout ce dont vous avez besoin pour personnaliser l’expérience de vos clients, afin que vous puissiez optimiser les recettes sur vos sites Web et mobiles, vos applications, vos médias sociaux et d’autres canaux numériques.

L’application Visual Experience Composer (VEC) pour applications mobiles natives vous permet de créer des activités et de personnaliser du contenu sur les applications mobiles natives à votre propre manière sans dépendances de développement continu et de cycles de commercialisation d’application.

Dans la leçon [Ajouter des extensions](launch-add-extensions.md), vous avez ajouté l’extension Target VEC à votre propriété Launch. Dans la leçon [Installer le SDK](launch-install-the-mobile-sdk.md) mobile, vous avez importé l’extension dans l’exemple d’application. Seules quelques mises à jour mineures sont requises pour démarrer la configuration des activités dans le compositeur d’expérience visuelle mobile de Target !

>[!IMPORTANT] Les extensions de lancement du compositeur d’expérience visuelle Target et Target sont requises pour utiliser le compositeur d’expérience visuelle Target dans votre application mobile.

## Objectifs d’apprentissage

À la fin de ce tutoriel, vous serez en mesure :

* Activation de l’exemple d’application pour le compositeur d’expérience visuelle Target
* Ajout de paramètres à la requête Target VEC
* Association de votre périphérique au compositeur d’expérience visuelle
* Création d’une activité à l’aide du compositeur d’expérience visuelle

## Conditions préalables 

Pour terminer les leçons de cette section, vous devez :

* Suivez les leçons de la section [Configuration du lancement](launch-create-a-property.md) .
* Disposer d’un accès de niveau approbateur à l’interface d’Adobe Target

## Demande de chargement d’application

Target déclenchera une demande de "chargement d’application" lors du premier chargement de l’application en raison des paramètres sélectionnés lors de la configuration de l’extension du compositeur d’expérience visuelle Target. Cette requête prérécupère toutes les activités du compositeur d’expérience visuelle Target que vous avez créées pour votre application.

Dans Android Studio, filtrez Logcat sur "Target r" pour afficher les requêtes et réponses Target. Notez les paramètres du nom et de la version de l’application. Toutes les activités du compositeur d’expérience visuelle Target que vous créez sont automatiquement ciblées sur ces propriétés.

![Afficher les requêtes du compositeur d’expérience visuelle Target](images/android/mobile-targetvec-viewLogs.png)

## Ajouter des paramètres

Comme vous l’avez vu lors du dernier exercice, les mesures de cycle de vie des applications sont automatiquement incluses en tant que paramètres dans la requête du compositeur d’expérience visuelle Target. Vous pouvez également ajouter des paramètres personnalisés aux requêtes, globalement ou pour des vues spécifiques dans l’application.

**Pour ajouter des paramètres personnalisés globalement**

1. Dans Android Studio, ouvrez `DemoApplication` le fichier.
1. Importez l’extension du compositeur d’expérience visuelle Target en ajoutant `import ACPTargetVEC` sous l’importation existante.
1. Ajoutez l’exemple de code suivant dans la `onCreate()` fonction, avant l’enregistrement des extensions. Cet exemple de code montre comment ajouter des paramètres réguliers, des paramètres de profil, des paramètres de produit (ou d’entité) et des paramètres de commande à la requête TargetVEC. Cet exemple utilise des valeurs statiques, alors que dans votre application réelle, vous souhaiterez probablement utiliser des variables dynamiques pour renseigner ces valeurs. Et bien sûr, vous ne voudriez renseigner que les paramètres pertinents pour toutes les vues :

   ```java
   Map<String, String>targetParams = new HashMap<>(); //params
   targetParams.put( "param1", "value1");
   Map<String, String>taregtProfileParams = new HashMap<>(); //profile params
   taregtProfileParams.put("profilekey1","profilevalue1");
   
   TargetVEC.setGlobalRequestParameters(new TargetParameters.Builder()
            .parameters(targetParams)
            .profileParameters(taregtProfileParams)
            .product(new TargetProduct("1234", "furniture"))
            .order(new TargetOrder("12343", 123.45, Arrays.asList("100", "200")))
            .build());
   ```

1. Vous pouvez constater des erreurs dans Android Studio, puisque le code de paramètre ci-dessus nécessite les importations suivantes, que vous devez ajouter au fichier :

   ```java
   import com.adobe.marketing.mobile.TargetOrder;
   import com.adobe.marketing.mobile.TargetProduct;
   import com.adobe.marketing.mobile.TargetParameters;
   import java.util.Arrays;
   import java.util.Map;
   import java.util.HashMap;
   ```

   ![Ajout de paramètres à la requête TargetVEC](images/android/mobile-targetvec-addParameters.png)

Maintenant que vous avez ajouté des paramètres à l’application, il est temps de confirmer qu’ils sont transmis dans la requête.

**Pour vérifier les paramètres**

1. Enregistrement du projet Android Studio
1. Recréez l’application et attendez qu’elle soit rouverte dans l’émulateur.
1. Ouvrez le volet Logcat d’Android Studio.
1. Filtrer pour afficher toutes les instructions avec "Target r"
1. Les paramètres personnalisés que vous venez d’ajouter doivent être visibles dans la requête.

   ![Vérification des paramètres de la requête TargetVEC](images/android/mobile-targetvec-verifyParams.png)

Pour plus d’informations et d’informations sur la manière de transmettre des paramètres avec des vues spécifiques, voir [la documentation](https://docs.adobe.com/content/help/en/target/using/implement-target/mobile-apps/composer/mobile-visual-experience-composer-android.html#parameters).

## Association de l’application mobile à l’interface Target

Pour créer des activités du compositeur d’expérience visuelle dans l’interface de Target, vous devez d’abord associer Target à votre application. Ce couplage est réalisé avec l'utilisation de liens profonds.

### Création du lien profond

Android prend en charge l’utilisation de liens [profonds et de liens](https://developer.android.com/training/app-links/deep-linking) d’application Android pour créer des URL qui accèdent directement à des emplacements spécifiques de votre application. Vous les utilisez probablement déjà dans votre application. Si tel est le cas, vous pouvez utiliser votre structure d’URL existante pour établir une paire avec Target. Dans ce didacticiel, vous passerez en revue le lien profond prédéfini dans l’application de réservation de bus, vous confirmerez qu’il fonctionne, puis vous l’utiliserez pour associer votre application au compositeur d’expérience visuelle de Target pour les applications mobiles.

**Pour consulter la configuration des liens profonds**

1. Dans Android Studio, ouvrez le fichier AndroidManifest.xml.
1. Notez qu'il existe déjà un filtre de mode configuré pour le schéma de lien profond de l'application de réservation de bus
1. Notez que les `Host` et `Scheme` sont déjà définis sur `com.adobe.example.busbooking` et `http`, respectivement. Cela signifie qu’une URL telle qu’une URL `http://com.adobe.example.busbooking` s’ouvre dans l’émulateur doit ouvrir automatiquement l’exemple d’application.

   ![Enregistrement du modèle d’URL](images/android/mobile-targetvec-registerScheme.png)

L'étape suivante consiste à confirmer que le schéma des liens profonds fonctionne.

### Vérifier le lien profond

Assurez-vous maintenant que le lien profond ouvrira l’application dans l’émulateur. Vous pouvez choisir la méthode d’exécution des commandes adb que vous pouvez utiliser.

**Pour vérifier le lien profond à l’aide d’adb (Mac®)**

1. Vérifiez que l’émulateur Android est en cours d’exécution.
1. Fermez l'application de réservation de bus si elle est déjà ouverte
1. Ouvrir une fenêtre de terminal
1. Accédez au répertoire des outils de la plate-forme Android : `cd Library/Android/sdk/platform-tools/`
1. Vérifiez que votre émulateur est attaché : `./adb devices`
1. Ouvrez le shell adb : `./adb shell`
1. Dans le shell adb, testez le lien profond : `am start -W -a android.intent.action.VIEW -d "http://com.adobe.example.busbooking" "com.adobe.busbooking"`
1. Confirmer que l'application de réservation de bus a été lancée dans l'émulateur

   ![Vérifier le lien profond](images/android/mobile-targetvec-verifyDeepLink.png)

Maintenant que votre structure de liens profonds est configurée, vous êtes prêt à utiliser le compositeur d’expérience visuelle Target pour configurer des activités !

## Création d’une activité dans le compositeur d’expérience visuelle mobile

Créons maintenant une activité dans l’interface de Target.

**Pour créer une activité avec le compositeur d’expérience visuelle de Target**

1. Connexion à [Adobe Experience Cloud](https://experiencecloud.adobe.com)
1. Utiliser le sélecteur de solution pour accéder à Target

   ![Vérifier le lien profond](images/mobile-targetvec-goToTarget.png)

1. Lancer Target

   ![Vérifier le lien profond](images/mobile-targetvec-launchTarget.png)

1. Cliquez sur le bouton **[!UICONTROL Créer l’activité]** et sélectionnez Test **[!UICONTROL A/B.]**
1. Sélectionner une application **[!UICONTROL mobile]**
1. Assurez-vous que **[!UICONTROL Visual]** est sélectionné sous **[!UICONTROL Sélectionner un compositeur d’expérience.]**
1. Cliquez sur le bouton **[!UICONTROL Suivant]** .

   ![Création d’une activité A/B à l’aide du compositeur d’expérience visuelle mobile](images/mobile-targetvec-createActivity.png)

1. Dans l’écran **[!UICONTROL Sélectionner une application à utiliser]** , cliquez sur **[!UICONTROL Ajouter une nouvelle application.]**

   ![Ajouter une nouvelle application](images/mobile-targetvec-addNewApp.png)

1. Entrez le modèle d'URL que vous venez de définir dans le champ **[!UICONTROL Entrer le modèle]** d'URL, par ex. `http://com.adobe.example.busbooking/`
1. Cliquez sur **[!UICONTROL Créer un lien profond]**

   ![Entrez votre modèle d’URL et créez un lien profond](images/android/mobile-targetvec-enterURLScheme.png)

   >[!NOTE] Vous disposez de quelques options pour envoyer le lien profond vers l’application. Vous pouvez :
   >
   >   1. Envoyez le lien profond à une adresse électronique valide, puis ouvrez le lien avec une application de messagerie sur le périphérique.
   >   1. Prenez une photo du code QR de votre périphérique Android (dans notre didacticiel, le périphérique doit être lié à Android Studio).
   >   1. Copiez le lien profond de l’interface Target et envoyez-le au périphérique comme vous le souhaitez.


1. Cliquez sur l'onglet **[!UICONTROL Copier et envoyer le lien]** .
1. Cliquez sur l’URL générée (notez que cliquer sur l’URL le copiera automatiquement dans le Presse-papiers).

   ![Copier l’URL](images/android/mobile-targetvec-copyURL.png)

1. Ouvrez une fenêtre Terminal (ou revenez-y si vous l'avez toujours ouverte).
1. Accédez au répertoire des outils de la plate-forme Android (vous êtes peut-être déjà ici) : `cd Library/Android/sdk/platform-tools/`
1. Vérifiez que votre émulateur est attaché : `./adb devices`
1. Ouvrez le shell adb : `./adb shell`
1. Dans le shell adb, remplacez [YOUR_TARGET_URL_WITH_TOKEN] dans la commande suivante par l’URL que vous venez de copier dans le Presse-papiers : `am start -W -a android.intent.action.VIEW -d "[YOUR_TARGET_URL_WITH_TOKEN]" "com.adobe.busbooking"`
1. Une fois l’application chargée, revenez à l’onglet du navigateur dans lequel Target est ouvert. Votre application doit être chargée dans le compositeur d’expérience visuelle.
1. Cliquez sur des fichiers de texte et d’image dans votre application et vous devriez voir des options pour les modifier et les remplacer !

   ![Chargements d’application dans le compositeur d’expérience visuelle](images/android/mobile-targetvec-devicePaired.png)

   > [!TIP] Si le compositeur d’expérience visuelle mobile ne s’ouvre pas automatiquement dans l’interface Target après l’ouverture du lien profond dans le périphérique mobile, voici quelques éléments à essayer :
   >
   >   1. Vérifiez que vous utilisez exactement la même URL dans l’interface de Target et que vous n’avez accidentellement coupé aucun caractère. Lors de l’exécution de la commande dans le shell adb, assurez-vous que l’URL est entre guillemets.
      >
      >
      >   

   1. Confirmez que vous avez ajouté au fichier build.gradle les dépendances supplémentaires requises par le compositeur d’expérience visuelle de Target. Ces dépendances auraient dû être ajoutées lors de la leçon [Installation du kit SDK Mobile](https://docs.adobe.com/content/help/en/experience-cloud/implementing-in-mobile-android-apps-with-launch/configure-launch/launch-install-the-mobile-sdk.html#update-the-buildgradle-file)
      >
      >
      >   

   1. Essayez d’effacer les données stockées dans l’application, comme indiqué dans l’illustration ci-dessous.
      >
      >       
      ![Chargements d’application dans le compositeur d’expérience visuelle](images/android/mobile-targetvec-pairing-clearData.png)


1. Apportez quelques modifications au premier écran de votre application.
1. Positionnez maintenant l’émulateur en regard du navigateur avec le compositeur d’expérience visuelle ouvert.
1. Accédez à un autre écran de l’application et remarquez la mise à jour du compositeur d’expérience visuelle avec l’émulateur !
1. Vous pouvez mettre à jour plusieurs vues dans votre application, dans une seule activité !

   ![Chargements d’application dans le compositeur d’expérience visuelle](images/android/mobile-targetvec-navigateTheApp.png)

1. Vous pouvez également ajouter visuellement des mesures de suivi des clics !
1. Enregistrez et approuvez votre activité et vérifiez que vous pouvez l’afficher dans l’exemple d’application.

L’association du périphérique au compositeur d’expérience visuelle est une action unique. Lorsque vous créez d’autres activités dans le futur sur le même périphérique, vous pourrez simplement sélectionner le périphérique dans une liste, comme illustré ci-dessous :

![Utilisation d'un périphérique enregistré](images/android/mobile-targetvec-useSavedApp.png)

>[!TIP] Si un périphérique est ouvert, mais qu’il est "Non disponible" dans le menu de sélection, essayez de fermer et de rouvrir l’application sur l’émulateur ou le périphérique.

## Création d’audiences sur la base de mesures de cycle de vie

Mesures de cycle de vie intégrées relatives à l’utilisation par le visiteur de votre application, qui sont automatiquement incluses dans les appels effectués par le SDK mobile Adobe. Vous pouvez facilement créer des audiences dans Target en fonction de ces mesures.

**Pour créer une audience**

1. Dans l’interface Target, cliquez sur **Audiences** dans le volet de navigation supérieur.
1. Click the **Create Audience** button

   ![Créer une audience](images/mobile-targetvec-audienceList.png)

1. Name the Audience `Launches < 5`
1. Click **Add Rule &gt; Custom**

   ![Sélectionner l’option Personnalisé](images/mobile-targetvec-custom.png)

1. Dans la première liste déroulante, sélectionnez le paramètre **a.Launches** . Tous les paramètres de mesure de cycle de vie commencent par "a".préfixe. Nous allons cibler le contenu en fonction du nombre de lancements d’application dont dispose l’utilisateur, ce qui constitue un excellent moyen de cibler les utilisateurs de votre application pour la première fois avec une expérience pédagogique, FTUE (First-user-experience).
1. Dans la liste déroulante suivante, sélectionnez **est inférieur à**
1. Dans la troisième liste déroulante, saisissez **5**
1. Cliquez sur **Enregistrer**

   ![Audience pour lancements &lt;= 4](images/mobile-targetvec-LaunchesLessThan5.png)

Notez qu’il existe une grande variété d’options prêtes à l’emploi pour la création d’audiences dans Target. De plus, vous pouvez envoyer des données personnalisées dans la demande Target pour la création d’audiences, utiliser des audiences partagées à partir d’autres solutions Experience Cloud, telles qu’Audience Manager et Analytics, et des données de gestion de la relation client partagées à Target à l’aide de la fonction Attributs du client du service principal People.

[Suivant : "Ajouter Adobe Target" &gt;](target.md)