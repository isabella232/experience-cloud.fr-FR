---
title: Publication de la propriété Launch
description: Découvrez comment publier la propriété Launch de l’environnement de développement vers les environnements d’évaluation et de production. Cette leçon fait partie du tutoriel Mise en œuvre d’Experience Cloud dans les applications mobiles Swift pour iOS avec Launch.
seo-description: null
seo-title: Publication de la propriété Launch
solution: Experience Cloud
translation-type: ht
source-git-commit: 7166d2933cc99bcbbd4713fba8f87fb0826b711e

---


# Publication de la propriété Launch

Maintenant que vous avez mis en œuvre certaines solutions clés d’Adobe Experience Cloud dans votre environnement de développement, il est temps d’apprendre le processus de publication.

## Conditions préalables

Pour pouvoir suivre cette leçon, votre compte utilisateur Launch doit disposer des autorisations « Approuver » et « Publier ». Si vous ne parvenez pas à effectuer l’une de ces étapes parce que vous n’avez pas accès aux options de l’interface utilisateur, contactez votre administrateur Experience Cloud pour demander l’accès à ces options. Pour plus d’informations sur les autorisations dans Launch, consultez [la documentation](https://docs.adobe.com/content/help/fr-FR/launch/using/reference/admin/user-permissions.html).

## Objectifs d’apprentissage

À la fin de cette leçon, vous saurez comment :

1. publier une bibliothèque de développement dans l’environnement d’évaluation ;
1. mettre à jour votre application pour charger différents environnements Launch ;
1. publier une bibliothèque d’évaluation dans l’environnement de production.

## Publication dans l’environnement d’évaluation

Maintenant que vous avez créé et validé votre bibliothèque dans l’environnement de développement, il est temps de la publier dans l’environnement d’évaluation.

1. Accédez à la page **[!UICONTROL Publication]**.

1. Dans la liste déroulante en regard de votre bibliothèque, sélectionnez **[!UICONTROL Envoyer pour approbation**].

   ![Envoyer pour approbation](images/mobile-publishing-submitForApproval.png)

1. Cliquez sur le bouton **[!UICONTROL Envoyer]** dans la boîte de dialogue :

   ![Clic sur Envoyer dans le modal](images/mobile-publishing-submit.png)

1. Votre bibliothèque apparaît désormais dans la colonne [!UICONTROL Envoyé] à l’état non créée :

1. Ouvrez la liste déroulante et sélectionnez **[!UICONTROL Créer à des fins d’évaluation]** :

   ![Créer à des fins d’évaluation](images/mobile-publishing-buildForStaging.png)
1. Lorsque l’icône en forme de point vert s’affiche, la bibliothèque peut être prévisualisée dans l’environnement d’évaluation.

Dans un scénario réel, l’étape suivante du processus consisterait à demander aux membres de votre équipe d’assurance qualité de valider les modifications dans la bibliothèque d’évaluation.

**Validation des modifications dans la bibliothèque d’évaluation**

1. Dans la propriété Launch, ouvrez la page [!UICONTROL Environnements].

1. Sur la ligne [!UICONTROL Évaluation], cliquez sur l’icône Installer

   ![icône Installer](images/mobile-launch-installIcon.png) pour ouvrir le modal.
   ![Accès à la page Environnements et clic pour ouvrir le modal](images/ios/swift/mobile-publishing-getStagingCode.png)

Si vous utilisez un autre espace de travail pour votre application d’évaluation, vous devez vous assurer que cet espace de travail comporte toutes les capsules et toutes les mises à jour d’application que vous avez effectuées tout au long de ce tutoriel. À ce stade, la seule différence dans les instructions d’installation par rapport à votre environnement de développement est la référence de Launch dans la configuration de base, comme indiqué dans la copie d’écran ci-dessus. Vous devez mettre à jour la ligne correspondante dans votre fichier AppDelegate.swift et recréer votre application.

En situation réelle, une fois que l’équipe d’assurance qualité a examiné et validé les modifications dans l’environnement d’évaluation, il est temps de publier la propriété dans l’environnement de production.

## Publication dans l’environnement de production

1. Accédez à la page [!UICONTROL Publication].

1. Dans la liste déroulante, cliquez sur **[!UICONTROL Approuver pour publication]** :

   ![Approuver pour publication](images/mobile-publishing-approveForPublishing.png)

1. Cliquez sur le bouton **[!UICONTROL Approuver]** dans la boîte de dialogue :

   ![Clic sur Approuver](images/mobile-publishing-approve.png)

1. La bibliothèque apparaît désormais dans la colonne [!UICONTROL Approuvé] à l’état non créée (point jaune) :

1. Dans la liste déroulante, sélectionnez **[!UICONTROL Créer et publier dans l’environnement de production]** :

   ![Créer et publier dans l’environnement de production](images/mobile-publishing-buildAndPublishToProduction.png)

1. Cliquez sur **[!UICONTROL Publier]** dans la boîte de dialogue :

   ![Clic sur Publier.](images/mobile-publishing-publish.png)

1. La bibliothèque s’affiche désormais dans la colonne [!UICONTROL Publié] :

   ![Publié](images/mobile-publishing-published.png)

Là encore, notez que l’environnement de production utilise une référence de Launch dans la configuration Core, comme indiqué dans la copie d’écran ci-dessous. Si vous utilisez un autre espace de travail pour votre application d’évaluation, vous devez vous assurer que cet espace de travail comporte toutes les capsules et toutes les mises à jour d’application que vous avez effectuées tout au long de ce tutoriel.
![Accès à la page Environnements et clic pour ouvrir le modal](images/ios/swift/mobile-publishing-getProductionCode.png)

>[!WARNING] La prochaine fois que vous apporterez des modifications à votre configuration Launch, vous devrez créer une nouvelle bibliothèque dans l’environnement de développement. N’oubliez pas que l’ajout et la suppression d’extensions nécessitent la mise à jour de l’application elle-même. Veillez à ce que vos environnements Launch et votre code d’application soient synchronisés entre eux pour éviter tout problème.

Vous avez terminé. Vous avez terminé ce tutoriel et publié votre première propriété dans Launch !