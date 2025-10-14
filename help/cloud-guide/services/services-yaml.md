---
title: Configuration des services
description: Découvrez comment configurer les services utilisés par Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# Configuration des services

Le fichier `services.yaml` définit les services pris en charge et utilisés par Adobe Commerce sur les infrastructures cloud, telles que MySQL, Redis et Elasticsearch ou OpenSearch. Vous n’avez pas besoin de vous abonner à des fournisseurs de services externes.

>[!NOTE]
>
>Le fichier `.magento/services.yaml` est géré localement dans le répertoire `.magento` de votre projet. La configuration est accessible pendant le processus de création afin de définir les versions de service requises dans l’environnement d’intégration uniquement. Elle est supprimée une fois le déploiement terminé. Vous ne les trouverez donc pas sur le serveur.


Le script de déploiement utilise les fichiers de configuration du répertoire `.magento` pour fournir à l’environnement les services configurés. Un service devient disponible pour votre application s’il est inclus dans la propriété [`relationships`](../application/properties.md#relationships) du fichier `.magento.app.yaml`. Le fichier `services.yaml` contient les valeurs _type_ et _disk_. Le type de service définit le service _nom_ et _version_.

La modification d’une configuration de service entraîne la mise en service de l’environnement par un déploiement avec les services mis à jour, ce qui affecte les environnements suivants :

- Tous les environnements de démarrage, y compris les `master` de production
- Environnements d’intégration Pro

{{pro-update-service}}

## Services par défaut et pris en charge

L’infrastructure cloud prend en charge et déploie les services suivants :

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

Vous pouvez afficher les versions par défaut et les valeurs de disque dans le fichier [&#x200B; actuel `services.yaml`par défaut](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). L’exemple suivant montre les services `mysql`, `redis`, `opensearch` ou `elasticsearch`, `rabbitmq` et `activemq-artemis` définis dans le fichier de configuration `services.yaml` :

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## Valeurs de service

Vous devez fournir l’ID de service et l’`type: <name>:<version>` de configuration du type de service. Si le service utilise le stockage persistant, vous devez fournir une valeur de disque.

Utilisez le format suivant :

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

La valeur `service-id` identifie le service dans le projet. Vous pouvez uniquement utiliser des caractères alphanumériques en minuscules : `a` à `z` et `0` à `9`, comme `redis`.

Cette valeur _service-id_ est utilisée dans la propriété [`relationships`](../application/properties.md#relationships) du fichier de configuration `.magento.app.yaml` :

```yaml
relationships:
    redis: "<name>:redis"
```

Vous pouvez nommer plusieurs instances de chaque type de service. Par exemple, vous pouvez utiliser plusieurs instances Redis, une pour la session et une pour le cache.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

Renommer un service dans le fichier `services.yaml` **supprime définitivement** les éléments suivants :

- Le service existant avant de créer un service avec le nouveau nom que vous spécifiez.
- Toutes les données existantes pour le service sont supprimées. Adobe vous recommande vivement de [sauvegarder votre environnement de démarrage](../storage/snapshots.md) avant de modifier le nom d’un service existant.

### `type`

La valeur `type` spécifie le nom et la version du service. Par exemple :

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

La valeur `disk` spécifie la taille de l’espace de stockage disque persistant (en Mo) à allouer au service. Les services qui utilisent le stockage persistant, tels que MySQL, doivent fournir une valeur de disque. Les services qui utilisent la mémoire au lieu du stockage persistant, tels que Redis, ne nécessitent pas de valeur de disque.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

La quantité de stockage par défaut actuelle par projet est de 5 Go, soit 512 Mo. Vous pouvez répartir ce montant entre votre application et chacun de ses services.

## Relations de service

Dans Adobe Commerce sur les projets d’infrastructure cloud, les services [relations](../application/properties.md#relationships) configurés dans le fichier `.magento.app.yaml` déterminent les services disponibles pour votre application.

Vous pouvez récupérer les données de configuration pour toutes les relations de service à partir de la variable d’environnement [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md). Les données de configuration incluent le nom, le type et la version du service, ainsi que tous les détails de connexion requis tels que le numéro de port et les informations d’identification.

**Pour vérifier les relations dans l’environnement local** :

1. Dans votre environnement local, afficher les relations pour l’environnement actif.

   ```bash
   magento-cloud relationships
   ```

1. Confirmez les `service` et `type` à partir de la réponse. La réponse fournit des informations de connexion, telles que l’adresse IP et le numéro de port.

   >Exemple de réponse abrégée

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**Pour vérifier les relations dans les environnements distants** :

1. Utilisez SSH pour vous connecter à l’environnement distant.

1. Répertorier les données de configuration des relations pour tous les services configurés dans l’environnement.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou utilisez la commande `ece-tools` suivante pour afficher les relations :

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Confirmez les `service` et `type` à partir de la réponse. La réponse fournit des informations de connexion, telles que l’adresse IP et le numéro de port, ainsi que les informations d’identification de nom d’utilisateur et de mot de passe requises.

## Versions des services

La prise en charge des versions de service et de la compatibilité pour Adobe Commerce sur l’infrastructure cloud est déterminée par les versions déployées et testées sur l’infrastructure cloud et diffère parfois des versions prises en charge par les déploiements sur site d’Adobe Commerce. Consultez [Configuration requise](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=fr) dans le guide _Installation_ pour obtenir une liste des dépendances logicielles tierces qu’Adobe a testées avec des versions spécifiques d’Adobe Commerce et de Magento Open Source.

### Vérifications de fin de vie du logiciel

Pendant le processus de déploiement, le package `ece-tools` vérifie les versions de service installées par rapport aux dates de fin de vie (EOL) de chaque service.

- Si une version de service est en cours depuis moins de trois mois, une notification s’affiche dans le journal de déploiement.
- Si la date de fin de vie est passée, une notification d’avertissement s’affiche.

Pour maintenir la sécurité du magasin, mettez à jour les versions logicielles installées avant qu&#39;elles n&#39;atteignent leur fin de vie. Vous pouvez consulter les dates de fin de vie dans le fichier [&#x200B; de `eol.yaml`ece-tools](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Migrer vers OpenSearch

{{elasticsearch-support}}

Pour Adobe Commerce version 2.4.4 et ultérieure, voir [Configuration du service OpenSearch](opensearch.md).

## Modifier la version du service

Vous pouvez mettre à niveau la version de service installée pour des raisons de compatibilité avec la version d’Adobe Commerce déployée dans votre environnement cloud.

Vous ne pouvez pas rétrograder directement la version du service pour un service installé. Cependant, vous pouvez créer un service avec la version requise. Voir [Version du service de rétrogradation](#downgrade-version).

### Mettre à niveau la version du service installé

Vous pouvez mettre à niveau la version du service installée en mettant à jour la configuration du service dans le fichier `services.yaml`.

1. Modifiez la valeur [`type`](#type) pour le service dans le fichier `.magento/services.yaml` :

   > Définition du service d’origine

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > Définition du service mise à jour

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Version de rétrogradation

Vous ne pouvez pas rétrograder directement un service installé. Vous disposez de deux options :

1. Renommez un service existant avec la nouvelle version, ce qui supprime le service et les données existants et en ajoute une nouvelle.

1. Créez un service et enregistrez les données du service existant.

Lorsque vous modifiez la version du service, vous devez mettre à jour la configuration du service dans le fichier `services.yaml` et mettre à jour les relations dans le fichier `.magento.app.yaml`.

**Pour rétrograder une version de service en renommant un service existant** :

1. Renommez le service existant dans le fichier `.magento/services.yaml` et modifiez la version.

   >[!WARNING]
   >
   >Renommer un service existant le remplace et supprime toutes les données. Si vous devez conserver les données, créez un service au lieu de renommer le service existant.

   Par exemple, pour rétrograder la version MariaDB du service _mysql_ de la version 10.4 à la version 10.3, modifiez la configuration _service-id_ et _type_ existante.

   > Définition de l’`services.yaml` d’origine

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Nouvelle définition de `services.yaml`

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Mettez à jour les relations dans le fichier `.magento.app.yaml`.

   > Configuration d’origine du `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Mise à jour de la configuration `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Ajouter, valider et transmettre vos modifications de code.

**Pour rétrograder un service en créant un service** :

1. Ajoutez une définition de service au fichier `services.yaml` pour votre projet avec la spécification de version rétrogradée. Voir _mysql2_ dans l’exemple suivant :

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Modifiez la configuration des relations dans le fichier `.magento.app.yaml` pour utiliser le nouveau service.

   > Configuration d’origine du `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Nouvelle configuration `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Ajouter, valider et transmettre vos modifications de code.
