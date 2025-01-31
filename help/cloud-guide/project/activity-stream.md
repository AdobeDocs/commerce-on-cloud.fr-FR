---
title: Flux d’activité
description: Découvrez comment lire le flux d’activités dans l [!DNL Cloud Console] interface de ligne de commande ou dans l’interface de ligne de commande cloud pour Adobe Commerce sur les infrastructures cloud.
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Flux d’activité

La vue principale de chaque environnement affiche une liste **Activité** d’événements historiques semblable à un journal Git. La liste des activités est un flux des événements récents pour les environnements actifs. Voici une liste des types d’activités et de leurs icônes qui s’affichent dans le flux d’activités :

![Types d’activité](../../assets/activity-types.svg){width="500" align="center"}

## Afficher les journaux

Dans la liste Activité , cliquez sur l’icône de statut d’une activité pour afficher le journal. Vous pouvez également cliquer sur le menu ![Plus](../../assets/icon-more.png){width="32"} (_plus_) pour accéder à d’autres options de gestion de l’activité. Vous trouverez ci-dessous un bref journal de création de sauvegarde. Vous pouvez [utiliser l’interface de ligne de commande Cloud](#activity-stream-with-cloud-cli) pour afficher le même journal.

![Vue du journal](../../assets/log-view.png)

## Gestion d’une activité

Certaines activités ont le statut _en cours d’exécution_ ou _en attente_. Vous pouvez agir sur une activité en cours d’exécution, par exemple en annulant un déploiement en cours d’exécution. Les onglets suivants présentent deux méthodes d’annulation d’une activité : l’interface de ligne de commande [!DNL Cloud Console] ou l’interface de ligne de commande Cloud.

>[!BEGINTABS]

>[!TAB  Console ]

**Pour annuler une activité dans le[!DNL Cloud Console]** :

Vous pouvez agir sur une activité en cours d’exécution en accédant au menu ![Plus](../../assets/icon-more.png){width="32"} (_plus_) et en sélectionnant une action, telle que `Cancel` ou `View log`. Pour cet exemple, sélectionnez l’option **Annuler** pour arrêter l’activité en cours d’exécution.

Toutes les activités n’ont pas l’option d’annulation. Par exemple, l’option permettant d’annuler le déploiement de l’application s’affiche uniquement pendant la phase _build_. Une fois l’application déplacée dans la phase _déploiement_, vous ne pouvez plus annuler l’activité. Voir [Processus de déploiement](../deploy/process.md) pour en savoir plus sur les différentes phases.

![Annuler l’activité](../../assets/activity-icons/cancel-activity.png){width="450" align="center"}

Si un terminal exécute l’activité de déploiement, l’annulation dans le [!DNL Cloud Console] entraîne l’annulation dans le terminal :

![Activité annulée dans le terminal](../../assets/activity-icons/activity-cancelled.png){width="300"}

>[!TAB CLI]

**Pour annuler une activité dans l’interface de ligne de commande Cloud** :

1. Identifiez les activités en cours d’exécution et sélectionnez un ID d’activité.

   ```bash
   magento-cloud activity:list --state=in_progress
   ```

1. Annulez l’activité à l’aide de l’identifiant d’activité :

   ```bash
   magento-cloud activity:cancel wvl5wm7s5vkhy
   ```

>[!ENDTABS]

## Flux d’activité de filtre

La possibilité de filtrer la liste des activités est utile lorsque vous recherchez quelque chose de spécifique, comme une sauvegarde ou un événement de fusion.

**Pour filtrer la liste des activités dans le[!DNL Cloud Console]** :

1. Sélectionnez un environnement et choisissez la vue **[!UICONTROL All]** d’activité pour inclure l’historique complet des événements.

1. Cliquez sur ![Filtrer par](../../assets/icon-filterby.png){width="32"} et sélectionnez les options **[!UICONTROL Filter by]** :

   ![Filtrer les activités](../../assets/activity-filter.png)

1. Choisissez la vue **[!UICONTROL Recent]** d’activité et réinitialisez la liste.

## Afficher le flux avec l’interface de ligne de commande du cloud

L’interface de ligne de commande `magento-cloud` offre la plupart des fonctionnalités de la [!DNL Cloud Console]. La commande `activity` permet d’effectuer les opérations suivantes :

- `list` du flux d’activités pour un environnement
- `get` d’informations sur une activité spécifique
- afficher les `log` d’une activité spécifique
- `cancel` une activité

**Pour afficher le flux d’activités avec l’interface de ligne de commande Cloud** :

1. Répertorier les activités de l’environnement actuel.

   ```bash
   magento-cloud activity:list
   ```

1. Chaque activité possède un identifiant unique. Sélectionnez un ID dans la liste précédente et affichez les détails de cette activité.

   ```bash
   magento-cloud activity:get wvl5wm7s5vkhy
   ```

1. Affichez le journal complet de cette activité.

   ```bash
   magento-cloud activity:log wvl5wm7s5vkhy
   ```

   Exemple de réponse :

   ```bash
   Activity ID: wvl5wm7s5vkhy
   Type: environment.backup
   Description: User created a backup of Master
   Created: 2023-09-08T14:03:33+00:00
   State: complete
   Log:
   Creating backup of master
   Created backup eg5pu63egt2dcojkljalzjdopa
   ```
