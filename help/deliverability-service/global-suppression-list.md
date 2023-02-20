---
title: Liste de suppression globale
description: Découvrez la liste de suppression globale
hide: true
source-git-commit: a946cfb1027896f6e45aaf88d25ad7114d6b5ac6
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 52%

---

# Liste de suppression globale {#global-suppression-list}

Une liste de suppression se compose d’adresses email que les clients souhaitent exclure de leurs diffusions, car l’envoi à ces contacts peut nuire à leur réputation d’envoi et à leurs taux de diffusion. Actuellement, Adobe tient à jour une liste des adresses email erronées connues qui se sont avérées préjudiciables à l’engagement et à la réputation du publipostage, et s’assure que les emails ne leur sont pas remis. Cette liste est gérée dans une liste de suppression globale qui est commune à tous les clients Adobe. Les adresses et les noms de domaine contenus dans la liste de suppression globale sont masqués. Seul le nombre de destinataires exclus est indiqué dans les rapports de diffusion.

Il est désormais possible de gérer la liste de suppression globale à partir d’une interface disponible en interne. Cette liste sera gérée par les consultants en délivrabilité uniquement. La liste de suppression globale peut inclure des adresses électroniques ou de domaine.

## Accès à la liste de suppression globale

Pour accéder à la liste détaillée des adresses email et domaines exclus, accédez à **[!UICONTROL Liste de suppression globale]**.

>[!CAUTION]
>
>Les autorisations d’affichage, d’exportation et de gestion de la liste de suppression globale dépendent de la liste de distribution à laquelle vous êtes affecté. En savoir plus

Deux onglets s’affichent : **[!UICONTROL Email]** et **[!UICONTROL Domaine]**.

Des filtres sont disponibles pour vous aider à parcourir la liste.

## Ajout d’adresses et de domaines

Actuellement, il existe deux façons d’ajouter des adresses à la liste de suppression globale :

* Liste gérée par l’équipe en charge de la délivrabilité.
* Liste fournie par le fournisseur tiers Blackbox.

Vous pouvez ajouter des adresses e-mail ou des domaines [un par un](#add-one-address-or-domain) ou [en masse](#upload-csv-file) par le biais du téléchargement d’un fichier CSV.

Pour cela, cliquez sur le bouton **[!UICONTROL Ajouter un e-mail ou un domaine]**, puis suivez l’une des méthodes ci-dessous.

### Ajout d’une adresse ou d’un domaine {#add-one-address-or-domain}

1. Sélectionnez l’option **[!UICONTROL Un par un]**.

1. Choisissez le type d&#39;adresse : **[!UICONTROL Adresse e-mail]** ou **[!UICONTROL Adresse de domaine]**.

1. Saisissez l&#39;adresse e-mail ou le domaine que vous souhaitez exclure de votre envoi.

   >[!NOTE]
   >
   >Veillez à saisir une adresse e-mail (par exemple abc@company) ou un domaine valide (par exemple abc.company.com).

1. Indiquez un motif si nécessaire.

   >[!NOTE]
   >
   >Tous les caractères ASCII imprimables compris entre 32 et 126 sont autorisés dans ce champ. La liste complète se trouve sur [cette page](https://en.wikipedia.org/wiki/Wikipedia:ASCII#ASCII_printable_characters){target="_blank"} par exemple.

1. Cliquez sur **[!UICONTROL Envoyer]** pour confirmer.

### Chargement d’un fichier CSV {#upload-csv-file}

1. Sélectionnez l’option **[!UICONTROL Télécharger CSV]**.

1. Téléchargez le modèle CSV à utiliser, qui comprend les colonnes et le format ci-dessous :

   ```
   TYPE,VALUE,COMMENT
   EMAIL,abc@somedomain.com,Comment
   DOMAIN,somedomain.com,Comment
   ```
   >[!CAUTION]
   >
   >Ne modifiez pas les noms des colonnes dans le modèle CSV.
   >
   >La taille du fichier ne doit pas dépasser 1 Mo.

1. Remplissez le modèle CSV avec les adresses e-mail et/ou les domaines à ajouter à la liste de suppression.

   >[!NOTE]
   >
   >Tous les caractères ASCII compris entre 32 et 126 sont autorisés dans la colonne **Commentaire**. La liste complète se trouve sur [cette page](https://en.wikipedia.org/wiki/Wikipedia:ASCII#ASCII_printable_characters){target="_blank"} par exemple.

1. Une fois l’opération terminée, faites glisser et déposez votre fichier CSV, puis cliquez sur **[!UICONTROL Envoyer]** pour confirmer.

>[!NOTE]
>
>Une fois le chargement terminé, assurez-vous qu’il a réussi en vérifiant son statut depuis l’interface. [Voici comment procéder](#recent-uploads)

### Vérification du statut des téléchargements récents {#recent-uploads}

Cliquez sur le bouton **[!UICONTROL Chargements récents]** pour vérifier l’état des derniers fichiers CSV que vous avez téléchargés.

Les statuts possibles sont les suivants :

* **[!UICONTROL En attente]** : le téléchargement du fichier est en cours de traitement.
* **[!UICONTROL Erreur]** : le processus de téléchargement du fichier a échoué en raison d’un problème technique ou d’un format de fichier incorrect.
* **[!UICONTROL Terminé]** : le processus de téléchargement du fichier est terminé.

Pendant le chargement, si certaines adresses ne sont pas au format correct, elles ne sont pas ajoutées à la liste de suppression globale.

Dans ce cas, une fois le téléchargement terminé, un rapport associé est émis. Vous pouvez le télécharger pour vérifier les erreurs rencontrées.

## Supprimer des adresses

Pour supprimer une adresse de la liste de suppression globale, utilisez le **[!UICONTROL Supprimer]** bouton .

>[!CAUTION]
>
>Les adresses ou les domaines automatiquement ajoutés par le fournisseur tiers Blackbox ne peuvent pas être supprimés par les consultants via l’interface. Cette opération peut uniquement être effectuée via un ticket de serveur principal.

