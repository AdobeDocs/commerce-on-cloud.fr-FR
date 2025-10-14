---
title: Architecture cloud pour Commerce
description: Découvrez le contraste des architectures de projet Starter et Pro pour Commerce sur les infrastructures cloud.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 7c1e895d-0f88-4f11-919a-b3b5748ca5f0
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# Architecture cloud pour Commerce

Adobe Commerce sur les infrastructures cloud dispose d’un plan Starter et Pro. Chaque plan possède une architecture unique pour piloter votre processus de développement et de déploiement d’Adobe Commerce. Les architectures Starter et Pro Plan déploient des bases de données, des serveurs web et des serveurs de mise en cache dans plusieurs environnements pour des tests de bout en bout tout en prenant en charge l’intégration continue.

À titre de comparaison, chaque plan comprend les fonctionnalités d’infrastructure et les produits pris en charge suivants.

|          | Démarreur | Pro |
| -------- | --------------------| ------------------ |
| Fonctionnalités principales | <ul><li>[Toutes les fonctionnalités d’Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>Outil d’intégration PayPal</li><li>[Rapports Commerce](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Toutes les fonctionnalités d’Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>Outil d’intégration PayPal</li><li>[Rapports Commerce](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[Module B2B](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infrastructure et déploiement | <ul><li>Outils d’intégration cloud continue avec un nombre illimité d’utilisateurs</li><li>Fast Content Delivery Network (CDN), Optimisation des images (IO) et sécurité accrue avec de larges bandes passantes. Le service Pare-feu d’application web (WAF) est disponible uniquement dans les environnements de production.</li><li>[New Relic](../monitor/new-relic-service.md) APM (Performance Monitoring) sur 3 branches : `master` et 2 de votre choix<br>environnements de production, d’évaluation et de développement de Platform as a service (PaaS) (4 environnements actifs au total) optimisés pour Adobe Commerce</li><li>Filtrage des sorties (pare-feu sortant)</li></ul> | <ul><li>Outils d’intégration cloud continue avec un nombre illimité d’utilisateurs</li><li>Fast Content Delivery Network (CDN), Optimisation des images (IO) et sécurité accrue avec de larges bandes passantes. Le service Pare-feu d’application web (WAF) est disponible uniquement dans les environnements de production.</li><li>[New Relic &#x200B;](../monitor/new-relic-service.md) Infrastructure en production + APM (surveillance des performances) en évaluation et en production. La politique [&#x200B; Alertes gérées &#x200B;](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) pour Adobe Commerce met en œuvre les bonnes pratiques de surveillance afin de vous informer de manière proactive des problèmes d’application et d’infrastructure affectant les performances du site.</li><li>Environnements basés sur Platform as a service (PaaS) [développement de l’intégration](pro-architecture.md#integration-environment) (2 environnements actifs au total) optimisés pour Adobe Commerce</li><li>Infrastructure en tant que service (IaaS) : infrastructure virtuelle dédiée aux environnements d’évaluation et de production</li></ul> |
| Infrastructure à haute disponibilité | | [Architecture haute disponibilité](pro-architecture.md#redundant-hardware) avec une configuration à trois serveurs dans l’infrastructure en tant que service (IaaS) sous-jacente pour offrir une fiabilité et une disponibilité de niveau entreprise |
| Matériel dédié | | Matériel isolé et dédié dans l’infrastructure en tant que service (IaaS) sous-jacente pour offrir des niveaux encore plus élevés de fiabilité et de disponibilité |
| Assistance par e-mail 24h/24 et 7j/7 | surveillance et prise en charge par e-mail 24h/24 et 7j/7 de l’application principale et de l’infrastructure cloud | surveillance et prise en charge par e-mail 24h/24 et 7j/7 de l’application principale et de l’infrastructure cloud |
| Un conseiller technique client dédié (CTA) | | Gestion de compte technique dédié pour la période de lancement initiale, à partir de votre abonnement jusqu’au lancement initial de votre site |
| Modules complémentaires | <ul><li>[Module B2B](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Moyennant des frais supplémentaires_

## Projets de démarrage

L’[architecture du plan de démarrage](starter-architecture.md) comporte quatre environnements :

- **Intégration** : l’environnement d’intégration fournit deux environnements testables. Chaque environnement comprend une branche Git active, une base de données, un serveur web, la mise en cache, certains services, des variables d’environnement et des configurations.

- **Staging** : à mesure que le code et les extensions réussissent vos tests, vous pouvez fusionner votre branche `integration` dans l’environnement d’évaluation, qui devient votre environnement de test de pré-production. Il comprend la branche principale `staging`, la base de données, le serveur web, la mise en cache, les services tiers, les variables d’environnement, les configurations et les services, tels que Fastly et New Relic.

- **Production** : lorsque le code est prêt et testé, il est fusionné pour `master` le déploiement sur le site de production actif. Cet environnement comprend votre branche de `master` active, votre base de données, votre serveur web, la mise en cache, des services tiers, des variables d’environnement et des configurations.

- **Inactif**—Vous disposez d&#39;un nombre illimité de branches inactives.

## Projets professionnels

L&#39;architecture du plan [Pro](pro-architecture.md) a une `master` globale avec trois environnements :

- **Intégration** : l’environnement d’intégration fournit un environnement testable qui comprend une base de données, un serveur web, la mise en cache, certains services, des variables d’environnement et des configurations. Vous pouvez développer, déployer et tester votre code avant de le fusionner dans l’environnement d’évaluation.

   - _Inactive_ : vous pouvez avoir un nombre illimité de branches inactives en fonction de l’environnement `integration`, mais une seule branche active (sans inclure `integration` ).

- **Staging** : l’environnement d’évaluation est destiné aux tests de pré-production et comprend une base de données, un serveur web, la mise en cache, des services tiers, des variables d’environnement, des configurations et des services, tels que Fastly.

- **Production** : l’environnement de production comprend une architecture haute disponibilité à trois nœuds pour vos données, services, mises en cache et magasins. La production correspond à votre environnement de magasin public en ligne, avec des variables d’environnement, des configurations et des services tiers.

## Logiciels et services pris en charge

Adobe Commerce sur les infrastructures cloud utilise :

- Système d&#39;exploitation : Debian GNU/Linux
- Serveur web : Nginx
- Base de données : MySQL (MariaDB)
- Réseau de diffusion de contenu (CDN) : Fast CDN

Vous pouvez configurer les services suivants :

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [ActiveMQ](../services/activemq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>Voir [Configuration requise](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) dans le _Guide d’installation_ pour obtenir les versions recommandées.

Le module Fastly CDN est utilisé pour les services de réseau CDN et de mise en cache sur les environnements d’évaluation et de production. Voir [Configuration des services Fastly](../cdn/fastly.md).

Pour plus d’informations sur la configuration des versions logicielles à utiliser dans votre implémentation, consultez les fichiers de configuration d’Adobe Commerce sur l’infrastructure cloud suivants :

- [Configuration de l’application (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Configuration de l’environnement (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Configuration des itinéraires (routes.yaml)](../routes/routes-yaml.md)
- [Configuration des services (services.yaml)](../services/services-yaml.md)
