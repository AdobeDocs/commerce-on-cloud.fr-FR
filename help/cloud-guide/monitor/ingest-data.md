---
title: Ingestion des données
description: Découvrez comment afficher et gérer l’ingestion des données Commerce dans New Relic.
feature: Cloud, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Ingestion des données

New Relic dépend de données riches pour fournir une surveillance et une analyse efficaces, mais des jeux de données volumineux peuvent affecter les résultats, les performances et la conformité en temps opportun. Cette rubrique fournit quelques conseils sur la gestion de l’ingestion des données et des stratégies pour affiner vos données afin qu’elles soient plus efficaces.

New Relic fournit une vue _Gestion des données_ qui résume l’utilisation de votre plan par source de données.

**Pour afficher les données et les sources d’ingestion** :

1. Dans le menu utilisateur de New Relic, cliquez sur **[!UICONTROL Manage your data]**.
1. Cliquez sur **[!UICONTROL Data management]** dans la liste _Administration_.

   ![Gestion des données](../../assets/new-relic/data-ingestion.png)

   L’onglet **[!UICONTROL Data ingestion]** affiche les données ingérées pour la journée et la source des données.
L’onglet Conservation des données affiche et contrôle la durée de stockage des données.

1. Sélectionnez l’onglet **[!UICONTROL Limits]** et affichez les limites de votre compte.

Les sources de données pour Adobe Commerce sont les suivantes :

- **Événements APM**—Données d&#39;événement utilisées dans les graphiques et les tableaux de bord
- **Infrastructure**—métriques de processus et d&#39;hôte, telles que CPU, stockage, mise en réseau
- **Journalisation** : journaux pour le réseau CDN, APM et le serveur d’applications

Les données de journal contribuent à une grande partie de l’ingestion. Découvrez comment [Afficher et analyser les données de journal](log-management.md#view-and-analyze-log-data) et collaborer avec votre représentant d’Adobe pour élaborer une stratégie concernant les besoins en matière d’ingestion et de conservation des données. Pour en savoir plus sur la [gestion de l’ingestion des données](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/), consultez la documentation de New Relic __.
