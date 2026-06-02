---
title: Ingestion des données
description: Découvrez comment afficher et gérer l’ingestion des données Commerce dans New Relic.
feature: Cloud, Observability
exl-id: b457b4de-deeb-4e92-b95a-c2b89d6f7a05
TQID: https://experienceleague.adobe.com/60hhI0IvazUSrw6cCRoXfbGjA4fkLLPXb8Q4Q08AqeE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 209
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

Les données de journal contribuent à une grande partie de l’ingestion. Découvrez comment [Afficher et analyser les données de journal](log-management.md#view-and-analyze-log-data) et collaborer avec votre représentant Adobe pour élaborer une stratégie concernant les besoins en matière d’ingestion et de conservation des données. Pour en savoir plus sur la [gestion de l’ingestion des données](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/), consultez la documentation de New Relic __.
