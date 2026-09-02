---
title: Déployer les variables
description: Consultez la liste des variables d’environnement qui contrôlent les actions dans la phase de déploiement d’Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
TQID: https://experienceleague.adobe.com/TNuUxXzCiXnKefww0DmKbjfJygEz2HFG-0PjCsCy2nA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: bdc2bedd2696e7dde0ffb55f846a8bced2dbd25d
workflow-type: tm+mt
source-wordcount: 3106
ht-degree: 0%

---

# Déployer les variables

Les variables _deploy_ suivantes contrôlent les actions lors de la phase de déploiement et peuvent hériter des valeurs de [Variables globales](variables-global.md) et les remplacer. Insérez ces variables dans l’étape `deploy` du fichier `.magento.env.yaml` :

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

Pour plus d’informations sur la personnalisation du processus de création et de déploiement :

- [Configuration du déploiement](configure-env-yaml.md)
- [Processus de déploiement](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Par défaut**—_Non défini_

Utilisez `CACHE_CONFIGURATION` pour fusionner ou remplacer les options frontales et principales du cache générées lors du déploiement.

Pour Adobe Commerce sur les infrastructures cloud, ne modifiez pas directement les `app/etc/env.php`. Le package de `ece-tools` génère la configuration de déploiement à partir des `.magento.env.yaml`, des relations de service et des variables de déploiement prises en charge.

Utilisez `VALKEY_BACKEND` ou `REDIS_BACKEND` pour sélectionner le cache pris en charge ou l’implémentation L2 pour la version exacte d’Adobe Commerce. Utilisez des `CACHE_CONFIGURATION` pour personnaliser des options telles que les reprises de connexion, les délais de lecture, les préfixes de cache ou les clés de préchargement.

La combinaison back-end et cache-service prise en charge dépend de la version de Commerce et du niveau de correctif. Redis n’est pas pris en charge pour Adobe Commerce 2.4.9 ou pour les versions de correctif ultérieures à 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 et 2.4.8-p4. Utilisez Valkey pour les versions où la [configuration requise](https://experienceleague.adobe.com/fr/docs/commerce-operations/installation-guide/system-requirements) le requiert.

>[!NOTE]
>
>Pour obtenir des conseils plus détaillés sur la configuration des services Redis et Valkey, voir [&#x200B; Bonnes pratiques pour la configuration des services Valkey et Redis &#x200B;](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)

Par défaut, le processus de déploiement remplace la configuration de cache correspondante. Pour fusionner les valeurs spécifiées avec la configuration générée, définissez `_merge` sur `true` :

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            connect_retries: 3
          remote_backend_options:
            read_timeout: 10
```

Pour remplacer la configuration existante avec les valeurs spécifiées dans `CACHE_CONFIGURATION`, définissez `_merge` sur `false`.

>[!IMPORTANT]
>
> Ne copiez pas les options de `bin/magento setup:config:set` sur site, telles que `cm_cache_backend_redis`, directement dans `CACHE_CONFIGURATION`. Dans les projets cloud, `ece-tools` obtient les détails de connexion au service à partir des relations configurées. Utilisez la structure décrite pour la version de Commerce sélectionnée et l’implémentation du cache.

L&#39;exemple suivant fusionne les affectations de base de données dans une configuration de cache existante. Utilisez ce type de remplacement uniquement lorsque le serveur principal et la version Commerce sélectionnés le prennent en charge. Appliquez les paramètres frontend à `symfony_l2` uniquement si la documentation actuelle de Symfony L2 prend explicitement en charge l’option.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

L’exemple suivant utilise la fonction de préchargement [Redis](https://experienceleague.adobe.com/fr/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache#redis-preload-feature) telle que définie dans le _Guide de configuration_. Utilisez les instructions Valkey correspondantes pour les versions qui utilisent Valkey.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

Pour utiliser un modèle [REDIS_BACKEND](#redis_backend) personnalisé qui n’est pas dans la liste autorisée, définissez `_custom_redis_backend` sur `true` afin que ece-tools applique la validation appropriée :

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **Default**—`true`

Active ou désactive le nettoyage [fichiers de contenu statique](https://experienceleague.adobe.com/fr/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment) généré pendant la phase de build ou de déploiement. Utilisez la valeur par défaut _true_ en développement comme bonne pratique.

- **`true`** : supprime tout le contenu statique existant avant de déployer le contenu statique mis à jour.
- **`false`** : le déploiement ne remplace les fichiers de contenu statique existants que si le contenu généré contient une version plus récente.

Si vous modifiez du contenu statique par le biais d’un processus distinct, définissez la valeur sur _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Si vous ne nettoyez pas les fichiers d&#39;affichage statique avant de les déployer, vous risquez de rencontrer des problèmes si vous déployez des mises à jour sur des fichiers existants sans supprimer les versions précédentes. En raison des règles [static file fallback](https://developer.adobe.com/commerce/frontend-core/guide/css/preprocess#clean-static-view-files), les opérations de secours peuvent afficher le mauvais fichier si le répertoire contient plusieurs versions du même fichier.

## `CRON_CONSUMERS_RUNNER`

- **Default**—`cron_run = false`, `max_messages = 1000`

Utilisez cette variable d’environnement pour confirmer que les files d’attente de messages sont en cours d’exécution après un déploiement.

- `cron_run` : valeur booléenne qui active ou désactive la tâche cron `consumers_runner`. La valeur par défaut est `false`.
- `max_messages` : nombre maximal de messages que chaque client traite avant de s&#39;arrêter. La valeur par défaut est `1000`. Pour empêcher le client de s’arrêter, définissez-le sur `0`.
- `consumers` : tableau de chaînes spécifiant les noms des consommateurs à exécuter. Un tableau vide s’exécute _tous_ les consommateurs.
- `multiple_processes`-Nombre de processus à générer pour chaque client. Cette option est prise en charge dans Adobe Commerce 2.4.4 et les versions ultérieures.

>[!NOTE]
>
>Pour répertorier les clients de file d’attente de messages disponibles, exécutez la commande `./bin/magento queue:consumers:list` dans l’environnement distant.

L’exemple suivant exécute les consommateurs sélectionnés et lance plusieurs processus pour chacun d’eux :

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
       example_consumer_1
       example_consumer_2
      multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

L’exemple suivant exécute tous les consommateurs :

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Par défaut, le processus de déploiement remplace les paramètres correspondants dans le fichier `env.php`. Voir [Gérer les files d’attente de messages](https://experienceleague.adobe.com/fr/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues) dans le _Guide de configuration de Commerce_ pour Adobe Commerce On-Premise.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Default**—`false`

Configurez le traitement `consumers` messages de la file d&#39;attente des messages en choisissant l&#39;une des options suivantes :

- `false` : `Consumers` traiter les messages disponibles, fermer la connexion TCP et s&#39;arrêter quelle que soit la limite de `max_messages` spécifiée dans la variable de déploiement `CRON_CONSUMERS_RUNNER`.

- `true`—`Consumers` continuer à traiter les messages de la file d&#39;attente des messages jusqu&#39;à atteindre le nombre maximal de messages (`max_messages`) spécifié dans la variable de déploiement `CRON_CONSUMERS_RUNNER` avant de fermer la connexion TCP et d&#39;arrêter le traitement du client. Si la file d’attente se vide avant d’atteindre `max_messages`, le client attend l’arrivée d’autres messages.

>[!WARNING]
>
>Si vous utilisez des programmes de travail pour exécuter `consumers` au lieu d’utiliser une tâche cron, définissez cette variable sur true.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Par défaut**—_Non défini_

>[!WARNING]
>
>Pour éviter d’exposer la clé dans le référentiel de code source, définissez la valeur `CRYPT_KEY` via le [!DNL Cloud Console] au lieu du fichier `.magento.env.yaml`. Voir [Définition des variables d’environnement et de projet](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/project/overview#configure-environment).

Lorsque vous déplacez la base de données d’un environnement à un autre sans processus d’installation, vous avez besoin des informations cryptographiques correspondantes. Adobe Commerce utilise la valeur de la clé de chiffrement définie dans le [!DNL Cloud Console] comme valeur `crypt/key` dans le fichier `env.php`.

## `DATABASE_CONFIGURATION`

- **Par défaut**—_Non défini_

Si vous avez défini une base de données dans la propriété [relations](../application/properties.md#relationships) du fichier `.magento.app.yaml`, vous pouvez personnaliser vos connexions à la base de données pour le déploiement.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

L’exemple suivant fusionne de nouvelles valeurs dans une configuration existante :

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

Vous pouvez également configurer un préfixe de tableau.

>[!WARNING]
>
>Si vous n’utilisez pas l’option de fusion avec le préfixe de tableau, vous devez fournir les paramètres de connexion par défaut ou le déploiement échoue lors de la validation.

L’exemple suivant utilise le préfixe de tableau `ece_` avec les paramètres de connexion par défaut au lieu d’utiliser l’option `_merge` :

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

Exemple de sortie :

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **Par défaut**—_Non défini_

Conserve les paramètres de service [!DNL Elastic Suite] personnalisés entre les déploiements et les utilise dans la section « system/default/smile_elasticsuite_core_base_settings » de la configuration [!DNL Elastic Suite] principale. Si le package du compositeur de [!DNL Elastic Suite] est installé, il est configuré automatiquement.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>Sur un cluster d’évaluation/de production Pro qui comporte trois nœuds (ou trois nœuds de service sur [Scaled Architecture](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)), le `indices_settings` doit être défini comme suit :
>
>```yaml
>           indices_settings:
>               number_of_shards: 1
>               number_of_replicas: 2
>```

{{merge-options}}

L’exemple suivant fusionne une nouvelle valeur avec la configuration existante :

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 2
      _merge: true
```

**Limites connues** :

- La modification du moteur de recherche en un type autre que `elasticsuite` entraîne un échec du déploiement accompagné d’une erreur de validation appropriée
- La suppression du service Elasticsearch entraîne un échec du déploiement accompagné d’une erreur de validation appropriée

>[!NOTE]
>
>Pour plus d’informations sur l’utilisation ou la résolution des problèmes du plug-in [!DNL Elastic Suite] avec Adobe Commerce, consultez la [[!DNL Elastic Suite] documentation](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **Default**—`false`

Active et désactive Google Analytics lors du déploiement dans les environnements d’évaluation et d’intégration. Par défaut, Google Analytics est défini uniquement sur l’environnement de production. Pour activer Google Analytics dans les environnements d’évaluation et d’intégration, définissez cette valeur sur `true`.

- **`true`** : active Google Analytics dans les environnements d’évaluation et d’intégration.
- **`false`** : désactive Google Analytics dans les environnements d&#39;évaluation et d&#39;intégration.

Ajoutez la variable d’environnement `ENABLE_GOOGLE_ANALYTICS` à l’étape `deploy` dans le fichier `.magento.env.yaml` :

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>Le processus de déploiement active toujours Google Analytics dans les environnements de production.

## `FORCE_UPDATE_URLS`

- **Default**—`true`

Lors du déploiement dans des environnements d’évaluation et de production Pro ou Starter, cette variable remplace les URL de base d’Adobe Commerce dans la base de données par les URL de projet spécifiées par la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) . Pour remplacer le comportement par défaut de la variable de déploiement [UPDATE_URLS](#update_urls), utilisez ce paramètre.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Par défaut**— Dans les environnements de production et d’évaluation, la valeur par défaut est `file` et ne peut pas être modifiée. Pour l&#39;intégration Pro et les environnements de démarrage, la valeur par défaut est `db`.

Le fournisseur de verrous empêche l’exécution des tâches et des groupes cron en double. Adobe Commerce on Cloud prend en charge les fournisseurs de verrous `file` et `db`.

Dans les environnements d’évaluation et de production Pro, `MAGENTO_CLOUD_LOCKS_DIR` configure le fournisseur de `file`. Impossible de remplacer ce paramètre. Dans les environnements Pro Integration et Starter, `ece-tools` définit le fournisseur de `db` par défaut. Pour optimiser les performances locales et refléter l’architecture de production, définissez le fournisseur sur `file` dans ces environnements.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: 'file'
```

## `MYSQL_USE_SLAVE_CONNECTION`

- **Default**—`false`

>[!TIP]
>
>La variable `MYSQL_USE_SLAVE_CONNECTION` est prise en charge uniquement sur les clusters Adobe Commerce on cloud infrastructure Staging et Production Pro. Il n’est pas pris en charge sur les projets de démarrage.

Adobe Commerce peut lire plusieurs bases de données de manière asynchrone. Définissez cette variable sur `true` pour utiliser automatiquement une connexion _lecture seule_ à la base de données afin de recevoir le trafic en lecture seule sur un nœud non principal. Cette connexion améliore les performances grâce à l’équilibrage de charge, car un seul nœud gère le trafic en lecture-écriture. Pour supprimer un tableau de connexion en lecture seule existant du fichier `env.php`, définissez sur `false`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Lorsque la variable `MYSQL_USE_SLAVE_CONNECTION` est définie sur `true`, le système définit le paramètre `synchronous_replication` sur `true` par défaut dans le fichier `env.php` dans les environnements d’évaluation et de production Pro. Lorsque la `MYSQL_USE_SLAVE_CONNECTION` est définie sur `false`, le paramètre `synchronous_replication` n’est pas configuré.

## `QUEUE_CONFIGURATION`

- **Par défaut**—_Non défini_

Utilisez cette variable d’environnement pour conserver les paramètres personnalisés du service de file d’attente entre les déploiements. Cette variable prend en charge les protocoles AMQP (pour RabbitMQ) et STOMP (pour ActiveMQ Artemis). Par exemple, si vous préférez utiliser un service de file d’attente de messages existant au lieu de vous fier à l’infrastructure cloud pour le créer, utilisez la variable d’environnement `QUEUE_CONFIGURATION` pour le connecter à votre site :

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

Pour les artéfacts ActiveMQ utilisant le protocole STOMP :

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      stomp:
        host: activemq.host
        port: 61616
        user: username
        password: password
```

{{merge-options}}

L’exemple suivant fusionne de nouvelles valeurs dans une configuration existante :

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **Default**—`Cm_Cache_Backend_Redis`

Spécifie la configuration du modèle principal pour le cache Redis.

Le cache Redis n’est pas pris en charge pour Adobe Commerce 2.4.9 ou pour les versions de correctif ultérieures à 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 et 2.4.8-p4. Pour ces versions, utilisez Valkey et la configuration `VALKEY_BACKEND` correspondante. Vérifiez toujours le service de cache pris en charge dans la [configuration requise](https://experienceleague.adobe.com/fr/docs/commerce-operations/installation-guide/system-requirements).

Pour les versions prises en charge par Redis, les modèles principaux disponibles sont les suivants :

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

L’exemple suivant active le serveur principal du cache synchronisé à distance et le cache L2 :

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
> Lorsque `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` est sélectionné, `ece-tools` génère automatiquement la configuration du cache L2. Pour personnaliser la configuration générée, utilisez [`CACHE_CONFIGURATION`](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Default**—`false`

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION` est pris en charge uniquement sur les clusters Adobe Commerce on Cloud Staging et Production Pro. Il n’est pas pris en charge sur les projets de démarrage.

Adobe Commerce peut lire plusieurs instances Redis de manière asynchrone. Définissez cette variable sur `true` pour utiliser une connexion en lecture seule à un réplica Redis pour le trafic de lecture tandis que l’instance principale gère le trafic de lecture-écriture. Pour supprimer un tableau de connexion en lecture seule existant de `env.php`, définissez-le sur `false`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Un service [Redis](../services/redis.md) doit être configuré dans les fichiers `.magento.app.yaml` et `services.yaml`.

[ECE-Tools version 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) et versions ultérieures utilisent des réglages plus tolérants aux pannes. Si Adobe Commerce ne peut pas lire les données du réplica Redis, il retourne à l’instance Redis principale.

La connexion en lecture seule n’est pas disponible dans l’environnement d’intégration. Si vous utilisez [`CACHE_CONFIGURATION`](#cache_configuration), fusionnez les modifications dans la configuration générée et vérifiez que la configuration obtenue conserve la connexion de réplica.

## `VALKEY_BACKEND`

- **Default**—`Cm_Cache_Backend_Redis`
- **Version** : versions d’Adobe Commerce prenant en charge Valkey.

`VALKEY_BACKEND` spécifie le modèle principal pour la configuration du cache Valkey. La valeur par défaut utilise un nom de classe hérité compatible avec Redis ; cela ne signifie pas que le service doit être Redis.

Pour les versions d’Adobe Commerce antérieures à la version 2.4.9 qui prennent en charge Valkey, les modèles principaux incluent :

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Adobe Commerce version 2.4.9 et ultérieure prend également en charge `symfony_l2`, l’implémentation L2 basée sur le cache de Symfony. `symfony_l2` est pris en charge avec Valkey uniquement.

### Configuration du cache synchronisé à distance

Pour Adobe Commerce 2.4.8, utilisez la configuration suivante lorsque l’implémentation du cache synchronisé à distance est appropriée :

```yaml
stage:
  deploy:
    VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

La spécification du serveur principal synchronisé à distance active le cache L2 et `ece-tools` génère automatiquement la configuration du cache. Voir [exemple de fichier de configuration](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#customize-the-symfony-l2-cache-configuration). Pour personnaliser la configuration générée, utilisez [`CACHE_CONFIGURATION`](#cache_configuration).

### Configurer l’implémentation moderne du cache L2 de Symfony

Pour Adobe Commerce version 2.4.9 et ultérieure, utilisez l’implémentation de Symfony L2 :

```yaml
stage:
  deploy:
    VALKEY_BACKEND: 'symfony_l2'
```

La spécification de `symfony_l2` comme modèle principal Valkey active le cache L2 et génère `ece-tools` automatiquement la configuration de cache L2 à partir des détails de connexion au service Valkey, y compris les fronts `default` et `stale_cache_enabled`. Définissez des `CACHE_CONFIGURATION` uniquement lorsque vous devez personnaliser les options principales prises en charge, telles que le répertoire du cache local. Voir [Implémentation du cache Symfony L2](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#configure-symfony-l2-cache){target="_blank"} dans le _Guide de configuration d’Adobe Commerce_.

>[!NOTE]
>
>Adobe Commerce 2.4.9 comprend des améliorations du cache Symfony L2 (notamment le stockage, l’invalidation et la compression des balises de cache) avec le correctif ACP2E-5132, ce qui réduit les E/S de disque, élimine les entrées de cache obsolètes et réduit la mémoire et la surcharge réseau.

## `VALKEY_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **Version**—Adobe Commerce 2.4.8 et versions ultérieures

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION` est pris en charge uniquement sur les clusters Adobe Commerce on Cloud Staging et Production Pro. Il n’est pas pris en charge sur les projets de démarrage.

Adobe Commerce peut lire plusieurs instances Valkey de manière asynchrone. Définissez `VALKEY_USE_SLAVE_CONNECTION` sur `true` pour utiliser une connexion _lecture seule_ à un réplica Valkey pour le trafic en lecture seule, tandis que l’instance principale gère le trafic en lecture-écriture. Cette connexion améliore les performances grâce à l’équilibrage de charge, car un seul nœud gère le trafic en lecture-écriture. Pour supprimer un tableau de connexion en lecture seule existant de `env.php`, définissez-le sur `false`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Vous devez avoir configuré un service [Valkey](../services/valkey.md) en `.magento.app.yaml` et `.magento/services.yaml`. La disponibilité d’une connexion de réplication dépend de la topologie du projet et de la version de `ece-tools` installée.

Avant de vous fier à ce paramètre, examinez la valeur de `MAGENTO_CLOUD_RELATIONSHIPS` décodée et confirmez qu’une relation de réplication est présente. Par exemple :

```bash
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

Par `symfony_l2`, la prise en charge des réplications nécessite les mises à jour appropriées des correctifs `ece-tools` et Cloud. Effectuez la mise à jour vers la dernière version de `ece-tools` avant d’activer ce paramètre. Si aucune relation de réplication n’est présente après le redéploiement, contactez l’assistance Adobe Commerce.

Lors de l’utilisation de [`CACHE_CONFIGURATION`](#cache_configuration), fusionner les remplacements pris en charge dans la configuration générée au lieu de remplacer la structure de connexion générée.

## `RESOURCE_CONFIGURATION`

- **Par défaut** : non défini

Mappe un nom de ressource à une connexion à la base de données. Cette configuration correspond à la section `resource` du fichier `env.php`.

{{merge-options}}

L’exemple suivant fusionne de nouvelles valeurs dans une configuration existante :

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **Default**—`4`

Indique le niveau de compression [gzip](https://www.gnu.org/software/gzip) (`0` à `9`) à utiliser lors de la compression du contenu statique. Définissez-le sur `0` pour désactiver la compression.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Default**—`600`

Lorsque le temps nécessaire à la compression des ressources statiques dépasse le délai d’expiration de la compression, le processus de déploiement est interrompu. Définissez la durée d’exécution maximale, en secondes, de la commande de compression de contenu statique.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Par défaut**—_Non défini_

Vous pouvez configurer plusieurs paramètres régionaux par thème. Cette personnalisation accélère le processus de déploiement en réduisant le nombre de fichiers de thème inutiles. Par exemple, vous pouvez déployer le thème _magento/backend_ en anglais et un thème personnalisé dans d’autres langues.

L’exemple suivant déploie le thème `Magento/backend` avec trois paramètres régionaux :

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Vous pouvez également choisir de ne _pas déployer_ thème :

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Par défaut**—_Non défini_

Permet d’augmenter le temps d’exécution maximal attendu pour le déploiement de contenu statique.

Par défaut, Adobe Commerce définit l’exécution maximale prévue sur 900 secondes, mais certains scénarios nécessitent plus de temps pour terminer le déploiement du contenu statique pour un projet cloud.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Default**—`false`

Lors de la phase de déploiement, définissez `SCD_NO_PARENT: true` afin que la génération de contenu statique pour les thèmes parents ne se produise pas pendant la phase de déploiement. Ce paramètre réduit le temps de déploiement et évite les temps d’arrêt du site qui peuvent se produire si la création de contenu statique échoue pendant le déploiement. Voir [Déploiement de contenu statique](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Default**—`quick`

Permet de personnaliser la [stratégie de déploiement](https://experienceleague.adobe.com/fr/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy) pour le contenu statique. Voir [&#x200B; Déploiement de fichiers de vue statiques &#x200B;](https://experienceleague.adobe.com/fr/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment).

Utilisez ces options _uniquement_ si vous disposez de plusieurs paramètres régionaux :

- `standard` : déploie tous les fichiers de vue statiques pour tous les packages.
- `quick`—(_par défaut_) réduit le temps de déploiement.
- `compact` : permet de conserver de l&#39;espace disque sur le serveur.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default**—Automatique

Définit le nombre de threads pour le déploiement de contenu statique. La valeur par défaut est définie en fonction du nombre de threads CPU détectés et ne dépasse pas une valeur de 4. L’augmentation du nombre de threads accélère le déploiement de contenu statique. La diminution du nombre de threads le ralentit. Vous pouvez définir la valeur du thread, par exemple :

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Pour réduire davantage le temps de déploiement, utilisez [Gestion de la configuration](../store/store-settings.md) avec la commande `scd-dump` pour déplacer le déploiement statique vers la phase de création.

## `SEARCH_CONFIGURATION`

- **Par défaut**—_Non défini_

Utilisez cette variable d’environnement pour conserver les paramètres de service de recherche personnalisés entre les déploiements. Par exemple :

Configuration d’Elasticsearch :

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

Configuration d’OpenSearch (pour Commerce 2.4.6 et versions ultérieures) :

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

L’exemple suivant fusionne une nouvelle valeur avec la configuration existante :

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Par défaut**—_Non défini_

Utilisez `SESSION_CONFIGURATION` pour configurer le stockage de session. L’exemple ci-dessous utilise la structure de configuration de session compatible avec Redis. Utilisez-le uniquement avec la combinaison de noms et de services de stockage de sessions prise en charge par la version exacte de Commerce. Pour les sessions soutenues par Valkey, suivez l’exemple [Valkey session-storage example](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration#apply-all-best-practice-recommendations).

Ne supposez pas que les variables de cache telles que `VALKEY_BACKEND` ou `REDIS_BACKEND` configurent des sessions. Le cache et la configuration de session sont indépendants. Sur les projets cloud, utilisez la relation de service et la configuration générée si possible ; ne codez pas en dur des valeurs spécifiques à un environnement sans remplacer l’exemple d’hôte et de port.

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: 'redis.internal'
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

Remplacez `redis.internal` et `6379` par l’hôte et le port du service de session pour l’environnement cible lorsque la configuration de déploiement nécessite des détails de connexion explicites.

{{merge-options}}

L’exemple suivant fusionne une nouvelle valeur avec la configuration existante :

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Par défaut**— _Non défini_

Définissez sur `true` pour ignorer le déploiement du contenu statique pendant la phase de déploiement.

Lors de la phase de déploiement, définissez `SKIP_SCD: true` afin que la création de contenu statique ne se produise pas pendant la phase de déploiement. Ce paramètre réduit le temps de déploiement et évite les temps d’arrêt du site qui peuvent se produire si la création de contenu statique échoue pendant le déploiement. Voir [Déploiement de contenu statique](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Default**—`true`

Lors du déploiement, remplacez les URL de base d’Adobe Commerce dans la base de données par les URL de projet spécifiées par la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) . Cette configuration est utile pour le développement local, où les URL de base sont configurées pour votre environnement local. Lorsque vous effectuez un déploiement dans un environnement cloud, les URL sont mises à jour afin que vous puissiez accéder à votre storefront et à votre administrateur à l’aide des URL du projet.

Si vous devez mettre à jour des URL lors d’un déploiement dans des environnements d’évaluation et de production Pro ou Starter, utilisez la variable [`FORCE_UPDATE_URLS`](#force_update_urls) .

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `USE_LUA`

- **Default**—`false`
- **Version**—Adobe Commerce 2.4.7 et versions ultérieures

Contrôle l’option du serveur principal du cache `use_lua` dans `env.php` pour le serveur principal du cache par défaut (et, lors de l’utilisation du serveur principal `symfony_l2`, les options du serveur principal distant du serveur principal du `stale_cache_enabled`). Cette option n’est pas appliquée au serveur frontal `page_cache`.

Utilisez la valeur par défaut `false` sauf indication contraire explicite de la part de la prise en charge d’Adobe.

```yaml
stage:
  deploy:
    USE_LUA: false
```

>[!WARNING]
>
>Sur Adobe Commerce 2.4.7 et 2.4.8, la définition de `USE_LUA: true` peut entraîner des problèmes de corruption du cache et d’absence du cache GraphQL.
>
>À partir d’Adobe Commerce 2.4.9, utilisez les conseils de configuration du cache Valkey pour votre version de Commerce et ne comptez pas sur `USE_LUA` pour les nouveaux déploiements.

## `LUA_KEY`

La variable `LUA_KEY` est obsolète. Si `LUA_KEY` est inclus dans `.magento.env.yaml`, supprimez-le lors de la migration. Utilisez plutôt les variables `USE_LUA` et `USE_LUA_ON_GC` .

## `USE_LUA_ON_GC`

- **Default**—`true`
- **Version**—Adobe Commerce 2.4.8 et versions ultérieures

Contrôle l’option du serveur principal du cache `use_lua_on_gc` dans `env.php` pour le serveur principal du cache par défaut (et, lors de l’utilisation du serveur principal `symfony_l2`, les options du serveur principal distant du serveur principal `stale_cache_enabled`) pour le nettoyage. Cette option n’est pas appliquée au serveur frontal `page_cache`.

Utilisez l’`true` de valeur par défaut pour conserver le nettoyage atomique des balises de cache pendant la tâche cron `backend_clean_cache`.

```yaml
stage:
  deploy:
    USE_LUA_ON_GC: true
```

>[!WARNING]
>
>Sous Adobe Commerce 2.4.8, la définition de `USE_LUA_ON_GC: false` peut entraîner l’échec silencieux de l’invalidation du cache basé sur les balises et nécessiter un vidage complet du cache pour la récupération.
>
>Sur 2.4.9 et les versions ultérieures, suivez les [conseils de service de cache](https://experienceleague.adobe.com/fr/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache) correspondant à la version installée.

## `VERBOSE_COMMANDS`

- **Par défaut**—_Non défini_

Activez ou désactivez le niveau de détail de débogage [Symfony](https://symfony.com/doc/current/console/verbosity.html) pour `bin/magento` commandes d’interface de ligne de commande exécutées lors de la phase de déploiement.

>[!NOTE]
>
>`bin/magento` Pour utiliser le paramètre VERBOSE_COMMANDS afin de contrôler le détail dans la sortie de commande pour les commandes CLI réussies et échouées, vous devez définir [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Choisissez le niveau de détail fourni dans les logs :

- `-v`= sortie normale
- `-vv`= sortie plus détaillée
- `-vvv` = sortie détaillée idéale pour le débogage

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
