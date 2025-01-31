---
title: Mise à l’échelle automatique
description: Découvrez comment Adobe Commerce sur les infrastructures cloud peut s’adapter à la demande en ressources.
feature: Cloud, Auto Scaling
topic: Architecture
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Mise à l’échelle automatique

La mise à l’échelle automatique ajoute ou supprime automatiquement des ressources à l’infrastructure cloud afin de maintenir des performances optimales et des coûts raisonnables. Actuellement, cette fonctionnalité n’est disponible que pour les projets configurés avec une [architecture à l’échelle](scaled-architecture.md).

## Nœuds de serveur Web

Le [niveau web](scaled-architecture.md#web-tier) s’adapte à l’augmentation des demandes de traitement et aux exigences de trafic plus élevées. Actuellement, la fonction de mise à l’échelle automatique ne se met à l’échelle horizontale qu’en ajoutant ou en supprimant des nœuds de serveur web.

Un événement de mise à l’échelle automatique se produit lorsque l’utilisation et le trafic CPU atteignent un seuil prédéfini :

- **Nœuds ajoutés** : les processeurs/cœurs de tous les nœuds web actifs ont une capacité de 75 % pendant 1 minute et le trafic augmente de 20 % pendant 5 minutes consécutives.
- **Nœuds supprimés** : les processeurs/cœurs de tous les nœuds web actifs sont chargés à 60 % pendant 20 minutes. Les nœuds sont supprimés dans l’ordre dans lequel ils ont été ajoutés.

Les seuils minimum et maximum sont déterminés et fixés en fonction des limites de ressources contractuelles de chaque commerçant, ce qui réduit le risque de mise à l&#39;échelle infinie.

## Surveillance des seuils avec New Relic

Vous pouvez utiliser le service [New Relic](../monitor/new-relic-service.md) pour surveiller certains seuils, tels que le nombre d&#39;hôtes et l&#39;utilisation de CPU. Les requêtes New Relic suivantes utilisent une notation de variable à des fins d’`cluster-id` uniquement.

>[!TIP]
>
>Pour plus d’informations sur la création de requêtes, voir [syntaxe, clauses et fonctions NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) dans la documentation de _New Relic_.
>Utilisez vos requêtes pour créer un tableau de bord [New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Nombre d’hôtes

L’exemple de requête New Relic suivant montre le nombre d’hôtes dans l’environnement :

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

Dans la capture d’écran suivante, **hôtes APM vus** fait référence au nombre d’hôtes avec des transactions consignées pendant la période sélectionnée.

![Nombre d’hôtes New Relic](../../assets/new-relic/host-count.png)

### Utilisation de CPU

L’exemple de requête New Relic suivant montre l’utilisation de CPU pour les nœuds web :

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Utilisation du CPU des nœuds web New Relic](../../assets/new-relic/web-node-cpu-usage.png)

## Activer la mise à l’échelle automatique

Pour activer ou désactiver la mise à l’échelle automatique pour votre projet d’infrastructure cloud Adobe Commerce, [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Sélectionnez les raisons suivantes dans le ticket :

- **Motif du contact** : demande de modification de l’infrastructure
- **Motif de contact de l’infrastructure Adobe Commerce** : autre demande de modification de l’infrastructure

>[!IMPORTANT]
>
>La fonction de mise à l’échelle automatique capture les événements inattendus. Même si la mise à l’échelle automatique est activée, Adobe vous recommande de continuer à [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) si vous prévoyez un événement à venir.

### Test de charge

Adobe active d’abord la mise à l’échelle automatique sur votre cluster de projet cloud _staging_. Une fois que vous avez effectué et terminé les tests de chargement dans votre environnement, Adobe active la mise à l’échelle automatique sur votre cluster de production. Pour obtenir des conseils sur les tests de chargement, voir [Tests de performance](../launch/checklist.md#performance-testing).

### IP, place sur la liste autorisée

Après l’activation de la mise à l’échelle automatique, le trafic sortant des nœuds web provient des adresses IP des nœuds de service. Si vous utilisez une liste autorisée avec un service tiers qui n’est pas fourni avec votre projet d’infrastructure cloud Adobe Commerce, vérifiez les adresses IP dans la liste autorisée du service tiers.

Par exemple :

- Si la liste autorisée contient les adresses IP de vos nœuds de service (1, 2 et 3), aucune action n’est requise.
- Si la place sur la liste autorisée contient les adresses IP de vos nœuds de service (1, 2 et 3) et de vos nœuds web (4, 5 et 6) (dans ce cas, les six nœuds), aucune action n’est requise.
- Si la place sur la liste autorisée contient les adresses IP _uniquement_ pour vos nœuds web (4, 5 et 6), vous devez mettre à jour la liste autorisée afin d’inclure les adresses IP des nœuds de service.
