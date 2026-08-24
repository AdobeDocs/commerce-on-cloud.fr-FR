---
title: Mise à l’échelle automatique
description: Découvrez comment Adobe Commerce sur les infrastructures cloud peut s’adapter à la demande en ressources.
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: a542dac902dc0de7c0836c1e5e4aece40fc6cbee
workflow-type: tm+mt
source-wordcount: 979
ht-degree: 0%

---

# Mise à l’échelle automatique

La mise à l’échelle automatique ajoute ou supprime automatiquement des ressources à l’infrastructure cloud afin de maintenir des performances optimales et des coûts raisonnables. Adobe propose deux types de mise à l’échelle automatique pour les projets [!DNL Adobe Commerce on cloud infrastructure] :

- [Mise à l’échelle automatique horizontale](#horizontal-auto-scaling) (disponible pour l’architecture mise à l’échelle uniquement) — Ajoute ou supprime des nœuds de serveur web pour les projets d’architecture mise à l’échelle.
- [Mise à l’échelle automatique verticale](#vertical-auto-scaling) (disponible pour l’architecture standard Pro ou l’architecture mise à l’échelle) : redimensionne la capacité CPU des nœuds existants pour s’adapter aux changements de la demande.


## Activer la mise à l’échelle automatique

Pour activer ou désactiver la mise à l’échelle automatique horizontale ou verticale de votre projet [!DNL Adobe Commerce on cloud infrastructure], [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Sélectionnez les raisons suivantes dans le ticket :

- **Motif du contact** : demande de modification de l’infrastructure
- **Motif de contact de l’infrastructure Adobe Commerce** : autre demande de modification de l’infrastructure

>[!IMPORTANT]
>
>La fonction de mise à l’échelle automatique capture les événements inattendus. Même si la mise à l’échelle automatique est activée, Adobe vous recommande de continuer à [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) si vous prévoyez un événement à venir.

### Test de charge

Adobe active d’abord la mise à l’échelle automatique sur votre cluster de projet cloud _staging_. Une fois que vous avez effectué et terminé les tests de chargement dans votre environnement, Adobe active la mise à l’échelle automatique sur votre cluster de production. Pour obtenir des conseils sur les tests de chargement, voir [Tests de performance](../launch/checklist.md#performance-testing).

## Mise à l’échelle automatique horizontale

Actuellement, cette fonctionnalité n’est disponible que pour les projets configurés avec une [architecture à l’échelle](scaled-architecture.md).

La mise à l’échelle automatique horizontale ajoute ou supprime les nœuds de serveur web pour les projets d’architecture mise à l’échelle. Vous pouvez également utiliser la mise à l’échelle automatique verticale [vertical auto scaling](#vertical-auto-scaling) pour redimensionner la capacité CPU des nœuds existants afin de répondre aux modifications de la demande.

### Nœuds de serveur Web

Le [niveau web](scaled-architecture.md#web-tier) s’adapte à l’augmentation des demandes de traitement et aux exigences de trafic plus élevées. Actuellement, la fonction de mise à l’échelle automatique ne se met à l’échelle horizontale qu’en ajoutant ou en supprimant des nœuds de serveur web.

Un événement de mise à l’échelle automatique se produit lorsque l’utilisation et le trafic CPU atteignent un seuil prédéfini :

- **Nœuds ajoutés** — Les processeurs/cœurs de tous les nœuds web actifs ont une capacité de 75 % pendant 1 minute et le trafic augmente de 20 % pendant 5 minutes consécutives.
- **Nœuds supprimés** — Les processeurs/cœurs de tous les nœuds Web actifs sont chargés à 60 % pendant 20 minutes. Les nœuds sont supprimés dans l’ordre dans lequel ils ont été ajoutés.

Les seuils minimum et maximum sont déterminés et fixés en fonction des limites de ressources contractuelles de chaque commerçant, ce qui réduit le risque de mise à l&#39;échelle infinie.

### Surveillance des seuils avec New Relic

Vous pouvez utiliser le service [&#128279;](../monitor/new-relic-service.md) pour surveiller certains seuils, tels que le nombre d&#39;hôtes et l&#39;utilisation de CPU. Les requêtes New Relic suivantes utilisent une notation de variable à des fins d’`cluster-id` uniquement.

>[!TIP]
>
>Pour plus d’informations sur la création de requêtes, voir [syntaxe, clauses et fonctions NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) dans la documentation de _New Relic_.
>Utilisez vos requêtes pour créer un tableau de bord [&#128279;](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

#### Nombre d’hôtes

L’exemple de requête New Relic suivant montre le nombre d’hôtes dans l’environnement :

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

Dans la capture d’écran suivante, **hôtes APM vus** fait référence au nombre d’hôtes avec des transactions consignées pendant la période sélectionnée.

![Nombre d’hôtes &#x200B;](../../assets/new-relic/host-count.png)

#### Utilisation de CPU

L’exemple de requête New Relic suivant montre l’utilisation de CPU pour les nœuds web :

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Utilisation du CPU des nœuds web New Relic](../../assets/new-relic/web-node-cpu-usage.png)

### IP, place sur la liste autorisée

Après l’activation de la mise à l’échelle automatique, le trafic sortant des nœuds web provient des adresses IP des nœuds de service. Si vous utilisez une liste autorisée avec un service tiers qui n’est pas fourni avec votre projet d’infrastructure cloud Adobe Commerce, vérifiez les adresses IP dans la liste autorisée du service tiers.

Par exemple :

- Si la liste autorisée contient les adresses IP de vos nœuds de service (1, 2 et 3), aucune action n’est requise.
- Si la place sur la liste autorisée contient les adresses IP de vos nœuds de service (1, 2 et 3) et de vos nœuds web (4, 5 et 6) (dans ce cas, les six nœuds), aucune action n’est requise.
- Si la place sur la liste autorisée contient les adresses IP _uniquement_ pour vos nœuds web (4, 5 et 6), vous devez mettre à jour la liste autorisée afin d’inclure les adresses IP des nœuds de service.

## Mise à l’échelle automatique verticale

En plus de la mise à l’échelle automatique horizontale traditionnelle [horizontal auto scaling](#auto-scaling), [!DNL Adobe Commerce on cloud infrastructure] propose également la mise à l’échelle automatique verticale pour les projets d’architecture pro standard et d’architecture mise à l’échelle.

Au lieu d’ajouter ou de supprimer des nœuds, la mise à l’échelle automatique verticale redimensionne la capacité CPU des nœuds existants pour s’adapter aux changements de la demande. Cela complète la mise à l’échelle automatique horizontale, qui ajoute ou supprime des nœuds de serveur web pour les projets d’architecture mise à l’échelle.

- **Nœuds ajoutés** : sans objet. La mise à l’échelle automatique verticale redimensionne les nœuds existants au lieu d’en ajouter de nouveaux.
- **Upsize de nœud** : un nœud est redimensionné à la taille d’instance supérieure suivante lorsque la pression de la mémoire dépasse le seuil défini. Une seule augmentation de taille est appliquée par événement de mise à l’échelle.
- **Réduction de la taille des nœuds** : les nœuds sont automatiquement réduits une fois la demande diminuée. Les tailles minimale et maximale sont définies en fonction du modèle d’utilisation de chaque projet et des limites de ressources sous-traitées, ce qui réduit le risque d’une mise à l’échelle inutile.

### Seuils de mise à l’échelle automatique

Les événements de mise à l’échelle automatique verticale sont déclenchés à l’aide des informations de blocage de la pression (PSI) pour la mémoire sous Linux, qui mesure le temps passé par un système bloqué en raison de la pression de la mémoire. Adobe définit des seuils en fonction des limites de ressources sous-traitées et des schémas d’utilisation de votre projet. Pour le moment, les commerçants ne peuvent pas les configurer.

### Surveillance des seuils avec New Relic

Vous pouvez utiliser le service [!DNL New Relic] pour surveiller les détails de l’instance d’infrastructure, notamment la taille et le type de l’instance. Configurez des alertes dans New Relic pour recevoir une notification chaque fois qu’un événement de mise à l’échelle automatique verticale modifie la taille ou le type d’une instance.

### Impact sur votre environnement

La mise à l’échelle automatique verticale a l’impact suivant sur votre environnement :

- **Temps d’arrêt** : aucun temps d’arrêt n’est prévu lorsqu’un nœud est redimensionné.
- **Planning** : le redimensionnement d’un nœud prend généralement entre 20 et 30 minutes. Le nœud est temporairement retiré de l’équilibreur de charge pendant que le redimensionnement est en cours.
