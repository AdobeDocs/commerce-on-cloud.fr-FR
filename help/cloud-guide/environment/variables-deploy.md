---
title: Déployer les variables
description: Consultez la liste des variables d’environnement qui contrôlent les actions dans la phase de déploiement d’Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 275a2a5c58b7c5db12f8f31bffed85004e77172f
workflow-type: tm+mt
source-wordcount: '2483'
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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Configurez la page Redis et la mise en cache par défaut. Lors de la définition du paramètre `cm_cache_backend_redis` , vous devez spécifier les options `server`, `port` et `database`.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

L’exemple suivant fusionne de nouvelles valeurs dans une configuration existante :

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

L’exemple suivant utilise la fonction de préchargement [Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html?lang=fr#redis-preload-feature) telle que définie dans le _Guide de configuration_ :

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

Pour utiliser un modèle [REDIS_BACKEND](#redis_backend) personnalisé (et pas seulement à partir de la liste autorisée de données), définissez l’option `_custom_redis_backend` sur `true` afin d’activer la validation correcte, comme dans l’exemple suivant :

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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Active ou désactive le nettoyage [fichiers de contenu statique](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=fr) généré pendant la phase de build ou de déploiement. Utilisez la valeur par défaut _true_ en développement comme bonne pratique.

- **`true`** : supprime tout le contenu statique existant avant de déployer le contenu statique mis à jour.
- **`false`** : le déploiement ne remplace les fichiers de contenu statique existants que si le contenu généré contient une version plus récente.

Si vous modifiez du contenu statique par le biais d’un processus distinct, définissez la valeur sur _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

Si vous ne nettoyez pas les fichiers d&#39;affichage statique avant de les déployer, vous risquez de rencontrer des problèmes si vous déployez des mises à jour sur des fichiers existants sans supprimer les versions précédentes. En raison des règles [static file fallback](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache), les opérations de secours peuvent afficher le mauvais fichier si le répertoire contient plusieurs versions du même fichier.

## `CRON_CONSUMERS_RUNNER`

- **Default**—`cron_run = false`, `max_messages = 1000`
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Utilisez cette variable d’environnement pour confirmer que les files d’attente de messages sont en cours d’exécution après un déploiement.

- `cron_run` : valeur booléenne qui active ou désactive la tâche cron `consumers_runner` (par défaut = `false`).
- `max_messages` : nombre spécifiant le nombre maximal de messages que chaque client doit traiter avant de s&#39;arrêter (par défaut = `1000`). Vous pouvez définir la valeur sur `0` pour empêcher l’arrêt du client.
- `consumers` : tableau de chaînes spécifiant les consommateurs à exécuter. Un tableau vide s’exécute _tous_ les consommateurs.

- `multiple_processes`-Nombre spécifiant le nombre de processus à générer pour chaque client. Pris en charge dans Commerce **2.4.4** ou version ultérieure.

>[!NOTE]
>
>Pour renvoyer une liste des `consumers` de file d&#39;attente de messages, exécutez la commande `./bin/magento queue:consumers:list` dans l&#39;environnement distant.

Exemple de tableau qui exécute des `consumers` spécifiques et le `multiple_processes` à générer pour chaque client :

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Exemple de tableau vide qui exécute toutes les `consumers` :

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Par défaut, le processus de déploiement remplace tous les paramètres du fichier `env.php`. Voir [Gérer les files d’attente de messages](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html?lang=fr) dans le _Guide de configuration de Commerce_ pour Adobe Commerce On-Premise.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Default**—`false`
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Configurez le traitement `consumers` messages de la file d&#39;attente des messages en choisissant l&#39;une des options suivantes :

- `false` : `Consumers` traiter les messages disponibles dans la file d&#39;attente, fermer la connexion TCP et terminer. `Consumers` n’attendez pas que d’autres messages rejoignent la file d’attente, même si le nombre de messages traités est inférieur à la valeur `max_messages` spécifiée dans la variable de déploiement `CRON_CONSUMERS_RUNNER` .

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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

>[!WARNING]
>
>Définissez la valeur de `CRYPT_KEY` via le [!DNL Cloud Console] au lieu du fichier `.magento.env.yaml` pour éviter d’exposer la clé dans le référentiel de code source pour votre environnement. Voir [Définition des variables d’environnement et de projet](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html?lang=fr#configure-environment).

Lorsque vous déplacez la base de données d’un environnement à un autre sans processus d’installation, vous avez besoin des informations cryptographiques correspondantes. Adobe Commerce utilise la valeur de la clé de chiffrement définie dans le [!DNL Cloud Console] comme valeur `crypt/key` dans le fichier `env.php`.

## `DATABASE_CONFIGURATION`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

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
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

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
>Sur un cluster Pro Staging/Production qui comporte trois nœuds (ou trois nœuds de service sur [Scaled Architecture](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier), la `indices_settings` doit être définie comme suit :
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

L’exemple suivant fusionne une nouvelle valeur avec la configuration existante :

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Active et désactive Google Analytics lors du déploiement dans les environnements d’évaluation et d’intégration. Par défaut, Google Analytics est défini uniquement sur l’environnement de production. Définissez cette valeur sur `true` pour activer Google Analytics dans les environnements d’évaluation et d’intégration.

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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Lors du déploiement dans des environnements d’évaluation et de production Pro ou Starter, cette variable remplace les URL de base d’Adobe Commerce dans la base de données par les URL de projet spécifiées par la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) . Utilisez ce paramètre pour remplacer le comportement par défaut de la variable de déploiement [UPDATE_URLS](#update_urls), qui est ignorée lors du déploiement dans les environnements d’évaluation ou de production.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Default**—`file`
- **Version**—Adobe Commerce 2.2.5 et versions ultérieures

Le fournisseur de verrous empêche le lancement de tâches et de groupes cron en double. Utilisez le fournisseur de verrou `file` dans l’environnement de production. Les environnements de démarrage et l&#39;environnement d&#39;intégration Pro n&#39;utilisent pas la variable [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md). Par conséquent, `ece-tools` applique automatiquement le fournisseur de verrous `db`.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Voir [Configuration du verrou](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html?lang=fr) dans le _Guide d’installation_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

>[!TIP]
>
>La variable `MYSQL_USE_SLAVE_CONNECTION` est prise en charge uniquement sur Adobe Commerce dans les environnements de cluster d’évaluation et de production Pro de l’infrastructure cloud et n’est pas prise en charge sur les projets de démarrage.

Adobe Commerce peut lire plusieurs bases de données de manière asynchrone. Définissez cette valeur sur `true` pour utiliser automatiquement une connexion _lecture seule_ à la base de données afin de recevoir le trafic en lecture seule sur un nœud non principal. Cette connexion améliore les performances grâce à l’équilibrage de charge, car un seul nœud gère le trafic en lecture-écriture. Définissez sur `false` pour supprimer tout tableau de connexion en lecture seule existant du fichier `env.php`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Lorsque la variable `MYSQL_USE_SLAVE_CONNECTION` est définie sur `true`, le paramètre `synchronous_replication` est défini sur `true` par défaut dans le fichier `env.php` dans les environnements d’évaluation et de production Pro. Lorsque la `MYSQL_USE_SLAVE_CONNECTION` est définie sur `false`, le paramètre `synchronous_replication` n’est pas configuré.

## `QUEUE_CONFIGURATION`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Utilisez cette variable d’environnement pour conserver les paramètres de service AMAP personnalisés entre les déploiements. Par exemple, si vous préférez utiliser un service de file d’attente de messages existant au lieu de vous fier à l’infrastructure cloud pour le créer, utilisez la variable d’environnement `QUEUE_CONFIGURATION` pour le connecter à votre site :

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
- **Version**—Adobe Commerce 2.3.0 et versions ultérieures

Spécifie la configuration du modèle principal pour le cache Redis.

Les versions 2.3.0 et ultérieures d’Adobe Commerce incluent les modèles principaux suivants :

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

Exemple de définition de `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Si vous spécifiez `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` comme modèle principal Redis pour activer le cache [L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=fr), `ece-tools` génère automatiquement la configuration du cache. Consultez un exemple [fichier de configuration](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=fr#configuration-example) dans le _Guide de configuration d’Adobe Commerce_. Pour remplacer la configuration de cache générée, utilisez la variable de déploiement [CACHE_CONFIGURATION](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **Version**—Adobe Commerce 2.1.16 et versions ultérieures

>[!WARNING]
>
>N’activez _pas_ cette variable sur un projet [architecture à l’échelle](../architecture/scaled-architecture.md). Cela entraîne des erreurs de connexion Redis. Les esclaves Redis sont toujours actifs mais ne sont pas utilisés pour les lectures Redis. Adobe recommande également d’utiliser Adobe Commerce version 2.3.5 ou ultérieure, d’implémenter une nouvelle configuration principale de Redis et d’implémenter la mise en cache L2 pour Redis.

>[!TIP]
>
>La variable `REDIS_USE_SLAVE_CONNECTION` est prise en charge uniquement sur Adobe Commerce dans les environnements de cluster d’évaluation et de production Pro de l’infrastructure cloud et n’est pas prise en charge sur les projets de démarrage.

Adobe Commerce peut lire plusieurs instances Redis de manière asynchrone. Définissez cette variable sur `true` pour utiliser automatiquement une connexion _lecture seule_ à une instance Redis afin de recevoir le trafic en lecture seule sur un nœud non maître. Cette connexion améliore les performances grâce à l’équilibrage de charge, car un seul nœud gère le trafic en lecture-écriture. Définissez sur `false` pour supprimer tout tableau de connexion en lecture seule existant du fichier `env.php`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

Un service Redis doit être configuré dans le fichier `.magento.app.yaml` et dans le fichier `services.yaml`.

[ECE-Tools version 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) et versions ultérieures utilisent des réglages plus tolérants aux pannes. Si Adobe Commerce ne peut pas lire les données de l’instance Redis _esclave_, il lit les données de l’instance Redis _maître_.

La connexion en lecture seule n’est pas disponible pour être utilisée dans l’environnement d’intégration ou si vous utilisez la variable [`CACHE_CONFIGURATION`](#cache_configuration).

## `VALKEY_BACKEND`

- **Default**—`Cm_Cache_Backend_Redis`
- **Version**—Adobe Commerce 2.8.0 et versions ultérieures

`VALKEY_BACKEND` spécifie la configuration du modèle principal pour le cache Valkey.

Les versions 2.8.0 et ultérieures d’Adobe Commerce incluent les modèles principaux suivants :

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

L’exemple suivant décrit comment définir `VALKEY_BACKEND` :

```yaml
stage:
  deploy:
    VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Si vous spécifiez `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` comme modèle principal Valkey pour activer le cache [L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=fr), `ece-tools` génère automatiquement la configuration du cache. Consultez un exemple [fichier de configuration](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html?lang=fr#configuration-example) dans le _Guide de configuration d’Adobe Commerce_. Pour remplacer la configuration de cache générée, utilisez la variable de déploiement [CACHE_CONFIGURATION](#cache_configuration).

## `VALKEY_USE_SLAVE_CONNECTION`

- **Default**—`false`
- **Version**—Adobe Commerce 2.4.8 et versions ultérieures

>[!WARNING]
>
>N’activez _pas_ cette variable sur un projet [architecture à l’échelle](../architecture/scaled-architecture.md). Cela entraîne des erreurs de connexion Valkey. Les esclaves Redis sont toujours actifs mais ne sont pas utilisés pour les lectures Redis. Adobe recommande également d’utiliser Adobe Commerce 2.4.8 ou une version ultérieure, d’implémenter une nouvelle configuration principale Valkey et d’implémenter la mise en cache L2 pour Valkey.

>[!TIP]
>
>La variable `VALKEY_USE_SLAVE_CONNECTION` est prise en charge uniquement sur Adobe Commerce dans les environnements de cluster d’évaluation et de production Pro de l’infrastructure cloud et n’est pas prise en charge sur les projets de démarrage.

Adobe Commerce peut lire plusieurs instances Redis de manière asynchrone. `VALKEY_USE_SLAVE_CONNECTION` défini sur `true` pour utiliser automatiquement une connexion _lecture seule_ à une instance Redis afin de recevoir le trafic en lecture seule sur un nœud non maître. Cette connexion améliore les performances grâce à l’équilibrage de charge, car un seul nœud gère le trafic en lecture-écriture. Définissez `VALKEY_USE_SLAVE_CONNECTION` sur `false` pour supprimer tout tableau de connexion en lecture seule existant du fichier `env.php`.

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

Un service Redis doit être configuré dans le fichier `.magento.app.yaml` et dans le fichier `services.yaml`.

[ECE-Tools version 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) et versions ultérieures utilisent des réglages plus tolérants aux pannes. Si Adobe Commerce ne peut pas lire les données de l’instance Valkey _esclave_, il lit les données de l’instance Redis _maître_.

La connexion en lecture seule n’est pas disponible pour être utilisée dans l’environnement d’intégration ou si vous utilisez la variable [`CACHE_CONFIGURATION`](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Par défaut** : non défini
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Indique le niveau de compression [gzip](https://www.gnu.org/software/gzip) (`0` à `9`) à utiliser lors de la compression du contenu statique ; `0` désactive la compression.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Default**—`600`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Lorsque le temps nécessaire à la compression des ressources statiques dépasse le délai d’expiration de la compression, le processus de déploiement est interrompu. Définissez la durée d’exécution maximale, en secondes, de la commande de compression de contenu statique.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

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
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Permet d’augmenter le temps d’exécution maximal attendu pour le déploiement de contenu statique.

Par défaut, Adobe Commerce définit l’exécution maximale prévue sur 900 secondes, mais dans certains scénarios, vous aurez peut-être besoin de plus de temps pour terminer le déploiement du contenu statique pour un projet cloud.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Default**—`false`
- **Version**—Adobe Commerce 2.4.2 et versions ultérieures

Lors de la phase de déploiement, définissez `SCD_NO_PARENT: true` afin que la génération de contenu statique pour les thèmes parents ne se produise pas pendant la phase de déploiement. Ce paramètre réduit le temps de déploiement et évite les temps d’arrêt du site qui peuvent se produire si la création de contenu statique échoue pendant le déploiement. Voir [Déploiement de contenu statique](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Default**—`quick`
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Permet de personnaliser la [stratégie de déploiement](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html?lang=fr) pour le contenu statique. Voir [ Déploiement de fichiers de vue statiques ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=fr).

Utilisez ces options _uniquement_ si vous disposez de plusieurs paramètres régionaux :

- `standard` : déploie tous les fichiers de vue statiques pour tous les packages.
- `quick`—(_par défaut_) réduit le temps de déploiement.
- `compact` : permet de conserver de l&#39;espace disque sur le serveur. Dans Adobe Commerce version 2.2.4 et antérieure, ce paramètre remplace la valeur de `scd_threads` par une valeur de `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default**—Automatique
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Définit le nombre de threads pour le déploiement de contenu statique. La valeur par défaut est définie en fonction du nombre de threads CPU détectés et ne dépasse pas une valeur de 4. L’augmentation du nombre de threads accélère le déploiement de contenu statique ; la diminution du nombre de threads le ralentit. Vous pouvez définir la valeur du thread, par exemple :

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Pour réduire davantage le temps de déploiement, utilisez [Gestion de la configuration](../store/store-settings.md) avec la commande `scd-dump` pour déplacer le déploiement statique vers la phase de création.

## `SEARCH_CONFIGURATION`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Configurez le stockage de session Redis. Nécessite les options `save`, `redis`, `host`, `port` et `database` pour la variable de stockage de session. Par exemple :

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

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
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Définissez sur `true` pour ignorer le déploiement du contenu statique pendant la phase de déploiement.

Lors de la phase de déploiement, définissez `SKIP_SCD: true` afin que la création de contenu statique ne se produise pas pendant la phase de déploiement. Ce paramètre réduit le temps de déploiement et évite les temps d’arrêt du site qui peuvent se produire si la création de contenu statique échoue pendant le déploiement. Voir [Déploiement de contenu statique](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Default**—`true`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Lors du déploiement, remplacez les URL de base d’Adobe Commerce dans la base de données par les URL de projet spécifiées par la variable [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) . Cette configuration est utile pour le développement local, où les URL de base sont configurées pour votre environnement local. Lorsque vous effectuez un déploiement dans un environnement cloud, les URL sont mises à jour afin que vous puissiez accéder à votre storefront et à votre administrateur à l’aide des URL du projet.

Si vous devez mettre à jour des URL lors d’un déploiement dans des environnements d’évaluation et de production Pro ou Starter, utilisez la variable [`FORCE_UPDATE_URLS`](#force_update_urls) .

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

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
