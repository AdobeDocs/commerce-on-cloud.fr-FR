---
title: Configuration du service Redis
description: Découvrez comment configurer et optimiser Redis en tant que solution de cache principale pour Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# Configuration du service Redis

[Redis](https://redis.io) est une solution de cache back-end facultative qui remplace le `Zend Framework Zend_Cache_Backend_File` utilisé par défaut par Adobe Commerce.

>[!IMPORTANT]
>
>Le cache Redis n’est pas pris en charge pour Adobe Commerce 2.4.9 ou les versions de correctif ultérieures à 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 et 2.4.8-p4. Utilisez [Valkey](valkey.md) pour la configuration du cache lorsque Redis n’est pas pris en charge. Consultez [Configuration requise](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) pour connaître les services de cache pris en charge par version.

{{service-instruction}}

## Activer Redis

Pour activer Redis, mettez à jour les fichiers suivants :

- `.magento/services.yaml`
- `.magento.app.yaml`

### Configuration du service

Dans `.magento/services.yaml`, ajoutez la définition du service Redis. Remplacez `<version>` par une version Redis prise en charge par votre version d’Adobe Commerce et votre modèle cloud actuel.

```yaml
cache:
  type: redis:<version>
```

Par exemple, pour une version de Commerce et un modèle de cloud qui prennent en charge Redis 7.2 :

```yaml
cache:
  type: redis:7.2
```

La version d’exemple n’est pas universelle. Les versions de service par défaut et prises en charge dépendent de la version d’Adobe Commerce, du niveau de correctif et du modèle cloud actuel. Vérifiez la combinaison prise en charge dans [Configuration requise](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) et le modèle de projet actuel.

### Configurer la relation de service

Dans `.magento.app.yaml`, configurez la relation entre l’application et le service Redis :

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

La clé de relation, `redis`, est le nom utilisé par l’application pour accéder au service. La valeur, `cache:redis`, se compose de l’ID de service (`cache`) et du type de service (`redis`) définis dans `.magento/services.yaml`.

### Validez et déployez les modifications

Ajoutez, validez et transmettez les modifications de configuration :

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

Une fois le déploiement terminé, vérifiez que la relation de service Redis est disponible.

{{service-change-tip}}

## Vérifier la relation de service

Après avoir déployé la configuration, exécutez la commande suivante à partir d’un conteneur d’applications pour afficher l’objet `MAGENTO_CLOUD_RELATIONSHIPS` décodé :

Utilisez SSH pour vous connecter à l’environnement cloud distant, puis exécutez :

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

La commande affiche toutes les relations de service configurées. Recherchez la relation `redis` pour identifier les détails de la connexion Redis.

L’exemple abrégé suivant illustre la relation `redis`. Ce n&#39;est pas un schéma universel.

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

La sortie varie en fonction de la configuration de l’environnement et du service. Ne codez pas en dur les noms d’hôtes, les ports, les adresses IP, les noms de cluster, les versions de service, les noms d’utilisateur ou les mots de passe à partir de cet exemple. Utilisez les valeurs renvoyées par `MAGENTO_CLOUD_RELATIONSHIPS` dans l’environnement cible.

Si `jq` est disponible, utilisez la commande suivante pour afficher uniquement la relation Redis :

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

Pour plus d’informations sur les relations de service, voir [Configuration des services](services-yaml.md).

## Personnaliser la configuration Redis

Pour les recommandations relatives au cache, à la session, à L2 et à la connexion de réplica, consultez [Bonnes pratiques pour la configuration de service Valkey et Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) dans le _Guide des bonnes pratiques du playbook d’implémentation_.

## Utilisation de l’interface de ligne de commande Redis

En supposant que votre relation Redis soit nommée `redis`, utilisez l’hôte et le port renvoyés par `MAGENTO_CLOUD_RELATIONSHIPS` pour vous connecter à Redis.

Connectez-vous à l’environnement avec Redis installé et configuré, puis exécutez la commande suivante :

```terminal
redis-cli -h <host> -p <port>
```

**Exemple**

```terminal
redis-cli -h redis.internal -p 6379
```

## Obtenir la version Redis installée

>[!BEGINTABS]

>[!TAB Environnement d’intégration]

Sur un environnement d’intégration, utilisez l’hôte et le port renvoyés par la relation `redis` pour exécuter :

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**Exemple de réponse**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

Les détails de version et de build varient selon l’environnement. Ne traitez pas un exemple de version affiché comme une version requise ou de service universel.

>[!TAB Évaluation et production Pro]

Sur les environnements d’évaluation et de production Pro, exécutez :

```terminal
redis-server -v
```

**Exemple de réponse**

```text
Redis server v=<installed-version> ...
```

Les détails de version et de build varient selon l’environnement. Ne traitez pas un exemple de version affiché comme une version requise ou de service universel.

>[!ENDTABS]

## Résolution des problèmes liés à Redis

Consultez les articles d’assistance Adobe Commerce suivants pour obtenir de l’aide sur la résolution des problèmes Redis :

- [Alertes gérées sur Adobe Commerce : alerte d’avertissement de mémoire Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Alertes gérées sur Adobe Commerce : alerte Redis avec mémoire critique](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### Les erreurs de nettoyage du cache référencent Redis sur un cache configuré par Valkey

Un échec de nettoyage du cache avant déploiement peut afficher le `[107]` de code d’erreur (`clean-redis-cache`) et un message d’`Connection to Redis`, même lorsque le service `cache` est configuré comme Valkey. `ece-tools` utilise ce code d’erreur et ce message hérités orientés Redis pour l’étape de nettoyage du cache, quel que soit le service qui soutient la relation `cache` ; le libellé n’indique donc pas que Redis est installé.

Si l’erreur sous-jacente est un échec du DNS, par exemple `Name or service not known` pour l’hôte de relation, l’étape de déploiement s’est exécutée avant que la relation de service ne soit disponible, ou le nom de la relation dans `.magento.app.yaml` ne correspond pas à l’ID de service dans `.magento/services.yaml`. Voir [Vérifier la relation de service](#verify-the-service-relationship).
