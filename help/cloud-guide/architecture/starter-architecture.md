---
title: Architecture de démarrage
description: Découvrez les environnements pris en charge par l’architecture Starter.
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '956'
ht-degree: 0%

---

# Architecture de démarrage

Votre architecture de démarrage d’Adobe Commerce sur l’infrastructure cloud prend en charge jusqu’à **quatre** environnements, y compris un environnement `master` qui contient le code du projet initial, l’environnement d’évaluation et jusqu’à deux environnements d’intégration.

Tous les environnements se trouvent dans des conteneurs PaaS (Platform as a service). Ces conteneurs sont déployés dans des conteneurs hautement restreints sur une grille de serveurs. Ces environnements sont en lecture seule et acceptent les modifications de code déployé des branches transmises à partir de votre espace de travail local. Chaque environnement fournit une base de données et un serveur web.

Vous pouvez utiliser n’importe quelle méthodologie de développement et d’embranchement. Lorsque vous obtenez l’accès initial à votre projet, créez un environnement `staging` à partir de l’environnement `master`. Créez ensuite l’environnement `integration` en embranchant à partir de `staging`.

## Architecture de l’environnement de démarrage

Le diagramme suivant montre les relations hiérarchiques des environnements de démarrage.

![Vue de haut niveau du projet de démarrage](../../assets/starter/architecture.png)

## Environnement de production

L’environnement de production fournit le code source pour déployer Adobe Commerce vers l’infrastructure cloud qui exécute vos storefronts de sites uniques et multiples accessibles au public. L’environnement de production utilise le code de la branche `master` pour configurer et activer le serveur web, la base de données, les services configurés et le code de votre application.

Comme l’environnement `production` est en lecture seule, utilisez l’environnement `integration` pour apporter des modifications au code, effectuer un déploiement dans l’architecture du `integration` au `staging`, et enfin dans l’environnement `production`. Voir [Déploiement de votre boutique](../deploy/staging-production.md) et [Lancement du site](../launch/overview.md).

Adobe recommande d’effectuer des tests complets dans votre branche `staging` avant d’effectuer un transfert vers la branche `master`, qui se déploie vers l’environnement `production`.

## Environnement d’évaluation

Adobe recommande de créer une branche appelée `staging` à partir de `master`. La branche `staging` déploie le code dans l’environnement d’évaluation afin de fournir un environnement de préproduction pour tester le code, les modules et les extensions, les passerelles de paiement, l’expédition, les données de produit, etc. Cet environnement fournit la configuration pour que tous les services correspondent à l’environnement de production, y compris Fastly, New Relic APM et Search.

Des sections supplémentaires de ce guide fournissent des instructions pour les déploiements de code finaux et le test des interactions au niveau de la production dans un environnement d’évaluation sécurisé. Pour des tests de performances et de fonctionnalités optimaux, répliquez votre base de données dans l’environnement d’évaluation.

>[!WARNING]
>
>Adobe recommande de tester chaque interaction client et commerçant dans l’environnement d’évaluation avant le déploiement dans l’environnement de production. Voir [Déploiement de votre magasin](../deploy/staging-production.md) et [Test du déploiement](../test/staging-and-production.md).

## Environnement d’intégration

Les développeurs utilisent l’environnement `integration` pour développer, déployer et tester :

- Code de l’application Adobe Commerce

- Code personnalisé

- Extensions

- Services tertiaires

**Cas d’utilisation recommandés :**

Les environnements d’intégration sont conçus pour des tests et un développement limités. Par exemple, vous pouvez utiliser l’environnement d’intégration pour effectuer les tâches suivantes :

- Assurez-vous que les modifications apportées aux processus d’intégration continue (CI) sont compatibles avec le cloud

- Testez les workflows critiques sur des pages clés telles que l’accueil, la catégorie, la page de détails du produit (PDP), le passage en caisse et l’administrateur.

Pour de meilleures performances dans l’environnement d’intégration, suivez ces bonnes pratiques :

- Limiter la taille du catalogue : à titre de référence, les données d’exemple contiennent environ 2 048 produits. Essayez de réduire la taille de votre catalogue à environ 4 000 à 5 000 produits.
Pour vérifier le nombre de produits dans le catalogue, exécutez la requête MySQL suivante :

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Réduire le nombre de groupes de clients et clientes : un trop grand nombre de groupes de clients et clientes peut affecter les performances d’indexation et les performances globales.

- Limiter l’utilisation à un ou deux utilisateurs simultanés

- Désactivez les tâches cron et exécutez-les manuellement si nécessaire.

Vous pouvez avoir jusqu’à **deux** environnements d’intégration actifs. Vous créez un environnement d’intégration en créant une branche à partir de la branche `staging` . Lorsque vous créez un environnement d’intégration, le nom de l’environnement correspond au nom de la branche. Un environnement d’intégration comprend un serveur web et une base de données. Il n’inclut pas tous les services ; par exemple, Fastly CDN et New Relic ne sont pas disponibles.

Vous pouvez disposer d’un nombre illimité de branches inactives pour le stockage du code. Pour accéder, afficher et tester une branche inactive, vous devez l’activer

{{enhanced-integration-envs}}

## Pile technologique de production et d’évaluation

Les environnements de production et d’évaluation incluent les technologies suivantes. Vous pouvez modifier et configurer ces technologies via le fichier [`.magento.app.yaml`](../application/configure-app-yaml.md).

- Fastly pour la mise en cache HTTP et le réseau CDN
- Serveur web Nginx parlant à PHP-FPM, une instance avec plusieurs workers
- Serveur Redis
- Elasticsearch de recherche catalogue pour Adobe Commerce 2.2 à 2.4.3-p2
- OpenSearch pour la recherche catalogue dans Adobe Commerce 2.3.7-p3, 2.4.3-p2, 2.4.4 et versions ultérieures
- Filtrage des sorties (pare-feu sortant)

### Services tertiaires

Adobe Commerce sur les infrastructures cloud prend actuellement en charge les services suivants : PHP, MySQL (MariaDB), Elasticsearch (Adobe Commerce 2.2 à 2.4.3-p2), OpenSearch (2.3.7-p3, 2.4.3-p2, 2.4.4 et versions ultérieures), Redis et [!DNL RabbitMQ].

Chaque service s’exécute dans un conteneur sécurisé distinct. Les conteneurs sont gérés ensemble dans le projet. Certains services sont standard, comme les suivants :

- Routeur HTTP (gestion des requêtes entrantes, mais aussi mise en cache et redirections)

- Serveur applicatif PHP

- Git

- Secure Shell (SSH)

### Versions logicielles

Adobe Commerce sur les infrastructures cloud utilise le système d&#39;exploitation Debian GNU/Linux et le serveur web NGINX. Vous ne pouvez pas mettre à niveau ce logiciel, mais vous pouvez configurer des versions pour les éléments suivants :

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

Dans les environnements d’évaluation et de production, vous utilisez Fastly pour le CDN et la mise en cache. La dernière version de l’extension Fastly CDN s’installe lors de l’approvisionnement initial de votre projet. Vous pouvez mettre à niveau l’extension pour obtenir les derniers correctifs et améliorations. Voir [Module de réseau CDN Fastly pour Magento 2](https://github.com/fastly/fastly-magento2). Vous avez également accès à [New Relic](../monitor/account-management.md) pour le suivi des performances.

Utilisez les fichiers suivants pour configurer les versions de logiciels que vous souhaitez utiliser dans votre implémentation.

- [`.magento.app.yaml`](../application/configure-app-yaml.md)

- [`routes.yaml`](../routes/routes-yaml.md)

- [`services.yaml`](../services/services-yaml.md)

### Sauvegarde et reprise après sinistre

Vous pouvez créer une sauvegarde de votre base de données et de votre système de fichiers à l’aide du [!DNL Cloud Console] ou de l’interface de ligne de commande. Voir [&#x200B; Gestion des sauvegardes &#x200B;](../storage/snapshots.md).

## Préparation au développement

Le workflow suivant résume le processus pour créer une branche de code, développer et déployer votre magasin :

1. Configuration de votre environnement local

1. Clonez la branche `master` vers votre environnement local.

1. Créer une branche `staging` à partir de `master`

1. Créer des branches pour le développement à partir d’`staging`

1. Code push vers Git qui crée et déploie dans un environnement à des fins de test

Consultez les sections suivantes pour obtenir des instructions détaillées et des explications sur le développement, le test et le déploiement de votre boutique :

- [Démarrer le workflow de développement et de déploiement](starter-develop-deploy-workflow.md)

- [Développement Docker](../dev-tools/cloud-docker.md) (environnement de développement local activé par Cloud Docker pour Commerce)

- [Gestion des branches](../project/console-branches.md)

- [Déploiement de votre boutique](../deploy/staging-production.md)

- [Lancement du site](../launch/overview.md)
