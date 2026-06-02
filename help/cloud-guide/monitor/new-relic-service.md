---
title: Service New Relic
description: Découvrez le service New Relic disponible avec votre projet d’infrastructure cloud Adobe Commerce.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00.000Z
exl-id: 10966241-311d-4b68-804d-4c9569bf933d
TQID: https://experienceleague.adobe.com/TzgxuxAgzcExYd8BsFh5FTsgFAMy5-qjAOmUdlwqMME
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 456
ht-degree: 0%

---

# Présentation du service New Relic

Tous les projets Adobe Commerce sur les infrastructures cloud incluent l’accès au service New Relic pour surveiller les performances et enquêter sur les événements de l’application [!DNL Commerce] et de l’infrastructure cloud.

Les fonctionnalités New Relic suivantes sont disponibles pour une utilisation avec les environnements de production et d’évaluation :

- [New Relic APM](#new-relic-apm) (Pro et Starter)
- [Infrastructure ](#new-relic-infrastructure) (Pro uniquement)
- [Gestion des journaux ](#new-relic-log-management) (Pro uniquement)

>[!INFO]
>
>Les autres fonctionnalités de New Relic ne sont pas disponibles dans les projets Adobe Commerce.
>
>Les clients Adobe Commerce on Cloud ne peuvent pas intégrer ni envoyer directement de données à partir de serveurs externes dans leur compte New Relic configuré. Le service New Relic se limite à la surveillance de l’environnement d’application Commerce Cloud. Seuls les services tiers appelés par l’application elle-même (par exemple, les API externes ou les services appelés pendant l’exécution) peuvent être suivis et surveillés dans New Relic.

## NEW RELIC APM

[New Relic pour la gestion des performances des applications (APM)](https://docs.newrelic.com/introduction-apm/) est un produit d&#39;analyse logicielle qui vous aide à analyser et à améliorer les interactions entre les applications. L’API New Relic est disponible pour tous les projets d’infrastructure cloud d’Adobe Commerce et offre les fonctionnalités suivantes :

- **Concentrez-vous sur des transactions spécifiques**—Notez et surveillez activement les principales actions du client sur votre site, telles que l&#39;ajout au panier, la vérification ou le traitement d&#39;un paiement.
- **Surveillance des requêtes de base de données** : localisez et surveillez les requêtes de base de données qui affectent les performances.
- **App Map** : permet d’afficher toutes les dépendances d’applications au sein de votre site, de vos extensions et de vos services externes.
- **[!DNL Apdex]les scores**—Évaluez les performances et créez des alertes qui identifient les problèmes et vous informent lorsqu&#39;ils se produisent, par exemple les performances du site affectées par une vente flash ou un événement web. Voir [Score Apdex](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Alertes gérées pour Adobe Commerce**-utilisez cette stratégie d’alerte New Relic pour surveiller les performances des applications et de l’infrastructure en fonction des bonnes pratiques du secteur. Voir [ Surveillance des performances avec la politique d’alerte Alertes gérées pour Adobe Commerce](investigate-performance.md#monitor-performance-with-managed-alerts).
- **Suivi des déploiements** : surveillez les événements de déploiement et analysez l’impact du déploiement sur les performances globales. Voir [ Suivi des déploiements ](track-deployments.md).

Votre projet d’infrastructure cloud d’Adobe Commerce comprend le logiciel pour le service New Relic APM ainsi qu’une clé de licence. Vous n&#39;avez pas besoin d&#39;acheter ou d&#39;installer un logiciel supplémentaire.

## Infrastructure New Relic

Les projets professionnels incluent le service [New Relic Infrastructure (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/), qui se connecte automatiquement aux données de l&#39;application et à l&#39;analyse des performances pour fournir une surveillance dynamique du serveur. Ce service est disponible dans les environnements de production et d’évaluation Pro.

## Gestion des journaux New Relic

Tous les projets d’infrastructure cloud incluent la [gestion des journaux ](log-management.md). Le service est préconfiguré pour agréger toutes les données de journal de vos environnements d’évaluation et de production et les afficher dans un tableau de bord de gestion des journaux centralisé.
