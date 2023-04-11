---
title: Créer et gérer les Triggers Experience Cloud
description: Découvrir l’interface utilisateur des Triggers Adobe Experience Cloud
hide: true
hidefromtoc: true
source-git-commit: ce0faf9fab45c931feb666ac0c77f5ab5c231746
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 27%

---

# Création d’un trigger Experience Cloud {#create-triggers}

>[!NOTE]
>
> La nouvelle interface utilisateur de Triggers Experience Cloud offre une expérience intuitive de gestion des comportements des consommateurs et de personnalisation des expériences utilisateur. Pour revenir à l’interface précédente, cliquez sur le bouton **[!UICONTROL Accéder au mode classique]** bouton .

Créez un trigger et configurez les conditions correspondantes. Vous pouvez par exemple indiquer les critères des règles d’un trigger pendant une visite, comme des mesures telles que Abandon du panier ou des dimensions telles que le nom du produit. Lorsque les règles sont satisfaites, le trigger s’exécute.

1. Dans l’Experience Cloud, sélectionnez le menu avancé, puis Déclencheurs.

   ![](assets/triggers_7.png)

1. Sur la page d&#39;accueil de Trigger, cliquez sur **[!UICONTROL Créer un trigger]**, puis spécifiez le type de déclencheur.

   Trois types de déclencheurs sont disponibles :

   * **[!UICONTROL Abandon]**: Vous pouvez créer un trigger qui se déclenche lorsqu’un visiteur consulte un produit mais n’ajoute rien au panier.

   * **[!UICONTROL Action]**: Vous pouvez créer des triggers, par exemple, pour qu’ils se déclenchent après des inscriptions à des newsletters, des abonnements par e-mail ou des demandes de cartes de crédit (confirmations). Si vous êtes un détaillant, vous pouvez créer un trigger pour un visiteur qui s’inscrit à un programme de fidélité. Dans le secteur des médias et du divertissement, créez des triggers pour les visiteurs qui regardent un programme en particulier et qui doivent répondre à une enquête.

   * **[!UICONTROL Début et fin de session]**: Créez un déclencheur pour les événements de début et de fin de session.

   ![](assets/triggers_1.png)

1. Ajouter un **[!UICONTROL Nom]** et un **[!UICONTROL Description]** à votre déclencheur.

1. Sélectionnez le **[!UICONTROL Suite de rapports]** utilisé pour ce déclencheur. Ce paramètre identifie les données de rapport à utiliser.

   [En savoir plus sur la suite de rapports](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite.html?lang=fr).

1. Choisissez la **[!UICONTROL Déclencher après aucune action pour]** période de validité.

1. Dans la **[!UICONTROL Visite doit inclure]** et **[!UICONTROL Visite ne doit pas inclure]** catégories, vous pouvez définir des critères ou des comportements de visiteur que vous souhaitez ou non voir se produire. Vous pouvez indiquer **Et** ou **Ou** logique au sein ou entre les conditions, selon les critères que vous déterminez.

   Par exemple, les règles pour un trigger d’abandon de panier simple peuvent ressembler à celles-ci :

   * **[!UICONTROL Visite doit inclure]**: `Carts (metric) Is greater or equal to 1` pour cibler les visiteurs avec au moins un article dans leur panier.
   * **[!UICONTROL Visite ne doit pas inclure]**: `Checkout (metric) Exists.` pour supprimer les visiteurs qui ont acheté des articles dans leur panier.

   ![](assets/triggers_2.png)

1. Cliquez sur **[!UICONTROL Conteneur]** pour établir et enregistrer des règles, des conditions ou des filtres qui définissent un déclencheur. Pour que des événements se produisent en même temps, vous devez les placer dans le même conteneur.

   Chaque conteneur est traité indépendamment au niveau de l’accès, ce qui signifie que si deux conteneurs sont associés à la variable **[!UICONTROL Et]** , les règles ne sont éligibles que si deux accès répondent aux exigences.

1. Dans la **[!UICONTROL Métadonnées]** champ, cliquez sur **[!UICONTROL + Dimension]** pour sélectionner une dimension Campaign particulière ou des variables pertinentes pour le comportement d’un visiteur.

   ![](assets/triggers_3.png)

1. Cliquez sur **[!UICONTROL Enregistrer]**.

1. Sélectionnez votre **[!UICONTROL Déclencheur]** de la liste pour accéder au rapport détaillé de votre déclencheur.

   ![](assets/triggers_4.png)

1. Dans la vue détaillée de votre déclencheur, vous pouvez accéder aux rapports sur le nombre de déclencheurs déclenchés. Si nécessaire, vous pouvez modifier le déclencheur à l’aide de l’icône en forme de crayon.

   ![](assets/triggers_5.png)
