---
title: Configuration du service Redis
description: Découvrez comment configurer et optimiser Redis en tant que solution de cache principale pour Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Configuration du service Redis

[Redis](https://redis.io) est une solution de cache back-end facultative qui remplace Zend_Cache_Backend_File de la structure Zend, utilisée par défaut par Adobe Commerce.

Voir [Configuration de Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html) dans le _Guide de configuration_.

{{service-instruction}}

**Pour activer Redis** :

1. Ajoutez le nom et le type requis au fichier `.magento/services.yaml`.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   Pour fournir votre propre configuration Redis, ajoutez une clé `core_config` dans votre fichier `.magento/services.yaml` :

   ```yaml
   cache:
       type: redis:<version>
   ```

1. Configurez les relations dans le fichier `.magento.app.yaml`.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [Vérifier les relations de service](services-yaml.md#service-relationships).

{{service-change-tip}}

## Utilisation de l’interface de ligne de commande Redis

En supposant que votre relation Redis soit nommée `redis`, vous pouvez y accéder à l’aide de l’outil `redis-cli` .

1. Utilisez SSH pour vous connecter à l’environnement d’intégration avec Redis installé et configuré.

1. Ouvrez un tunnel SSH vers un hôte.

   ```bash
   redis-cli -h redis.internal
   ```

## Obtenir la version Redis installée

Utilisez la commande suivante pour installer la version Redis dans un environnement d’intégration :

```bash
redis-cli -h redis.internal info | grep version
```

Exemple de réponse :

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis on Pro évaluation et production

Pour installer la version de Redis dans un environnement d’évaluation ou de production, utilisez la commande `redis-server` :

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

Utilisez la commande suivante pour installer la configuration Redis dans un environnement d’évaluation ou de production Pro :

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Exemple de réponse :

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Résolution des problèmes liés à Redis

Consultez les articles d’assistance Adobe Commerce suivants pour obtenir de l’aide sur la résolution des problèmes Redis :

- [Délai de résolution des problèmes Connexion ou passage en caisse de l’administrateur](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [Implémentation étendue du cache Redis dans Adobe Commerce 2.3.5 et ultérieure](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [Alertes gérées sur Adobe Commerce : alerte d’avertissement de mémoire Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Alertes gérées sur Adobe Commerce : alerte critique de mémoire Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
