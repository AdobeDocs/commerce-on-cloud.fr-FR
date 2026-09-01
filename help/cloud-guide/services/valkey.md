---
title: Configuration du service Valkey
description: Découvrez comment configurer et optimiser Valkey en tant que solution de cache principale pour Adobe Commerce sur les infrastructures cloud, notamment en remplaçant Redis et en personnalisant les paramètres du cache principal.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# Configuration du service Valkey

[Valkey](https://valkey.io) est une solution de cache back-end facultative pour Adobe Commerce sur les infrastructures cloud. Valkey est requis lorsque vous remplacez la configuration de cache par défaut sur Adobe Commerce 2.4.9 et versions ultérieures, ou sur les versions de correctif ultérieures à 2.4.5-p16, 2.4.6-p14, 2.4.7-p9 et 2.4.8-p4.

{{service-instruction}}

## Configurer Valkey

Pour remplacer Redis par Valkey, mettez à jour les fichiers suivants :

- `.magento/services.yaml`
- `.magento.app.yaml`

### Configuration du service

Dans `.magento/services.yaml`, remplacez la définition de service Redis par une définition de service Valkey. Remplacez `<version>` par une version Valkey prise en charge par votre version d’Adobe Commerce et votre modèle cloud actuel.

```yaml
cache:
  type: valkey:<version>
```

**Exemple**

```yaml
cache:
  type: valkey:8.0
```

La version d’exemple n’est pas universelle. Les versions de service par défaut et prises en charge dépendent de votre version d’Adobe Commerce et du modèle de cloud actuel. Utiliser la version spécifiée par le modèle de projet actuel. Voir [Configuration des services](services-yaml.md#service-versions) pour plus d’informations.

>[!WARNING]
>
>Si vous modifiez l’ID de service, le service existant est supprimé et un nouveau service est créé. Les données existantes dans le service supprimé sont définitivement supprimées. Sauvegardez l’environnement avant de renommer un service.

Ne supposez pas que les données du cache et de la session persistent lorsque vous modifiez la valeur de `type` de `redis:<version>` en `valkey:<version>`, même si vous conservez le même ID de service. Traiter la migration comme créant un nouveau cache : la conservation des données existantes du cache et de la session n’est pas garantie, et les utilisateurs sont déconnectés une fois la migration terminée.

### Configurer la relation de service

Dans `.magento.app.yaml`, configurez la relation entre l’application et le service Valkey :

```yaml
relationships:
  valkey: "cache:valkey"
```

La clé de relation, `valkey`, est le nom utilisé par l’application pour accéder au service. La valeur, `cache:valkey`, fait référence à l’ID de service et au type de service définis dans `.magento/services.yaml`.

>[!TIP]
>
>Adobe Commerce communique avec Valkey par le biais de la bibliothèque cliente `credis`, qui fonctionne par défaut sur des sockets PHP simples. Pour améliorer les performances, activez l’extension PHP `redis` dans `.magento.app.yaml`. `credis` utilise automatiquement l’extension compilée lorsqu’elle est disponible.
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### Validez et déployez les modifications

Ajoutez, validez et transmettez les modifications de configuration :

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

Une fois le déploiement terminé, vérifiez que la relation de service Valkey est disponible.

{{service-change-tip}}

{{valkey-newrelic}}

## Personnalisation de la configuration Valkey

Pour les recommandations relatives au cache, à la session, à L2 et à la connexion de réplica, consultez [Bonnes pratiques pour la configuration de service Valkey et Redis](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration) dans le _Guide des bonnes pratiques du playbook d’implémentation_.

## Vérifier la relation de service

Pour afficher l’objet `MAGENTO_CLOUD_RELATIONSHIPS` décodé, exécutez la commande suivante à partir d’un conteneur d’applications après le déploiement de la configuration :

Utilisez SSH pour vous connecter à l’environnement cloud distant, puis exécutez :

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

La commande affiche toutes les relations de service configurées. Pour identifier les détails de la connexion Valkey, recherchez la relation Valkey.

**Exemple de sortie**

L’exemple abrégé suivant illustre la relation `valkey`. Ce n&#39;est pas un schéma universel.

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
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

La sortie varie en fonction de la configuration de l’environnement et du service. Ne codez pas en dur les noms d’hôtes, les ports, les adresses IP, les noms de cluster, les versions de service, les noms d’utilisateur ou les mots de passe à partir de cet exemple. Utilisez les valeurs renvoyées par `MAGENTO_CLOUD_RELATIONSHIPS` dans l’environnement cible.

Si `jq` est disponible, afficher uniquement la relation Valkey :

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

Pour plus d’informations sur les relations de service, voir [Configuration des services](services-yaml.md).

## Utilisation de l’interface de ligne de commande Valkey

En supposant que votre relation Valkey soit nommée `valkey`, utilisez l’hôte et le port renvoyés par `MAGENTO_CLOUD_RELATIONSHIPS` pour vous connecter à Valkey :

```terminal
valkey-cli -h <host> -p <port>
```

**Exemple**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## Obtenir la version de Valkey installée

>[!BEGINTABS]

>[!TAB Environnement d’intégration]

Sur un environnement d’intégration, utilisez l’hôte et le port renvoyés par la relation `valkey` pour exécuter :

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**Exemple de réponse**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

Les détails de version et de build varient selon l’environnement. Ne traitez pas un exemple de version affiché comme une version requise ou de service universel.

>[!TAB Évaluation et production Pro]

Sur les environnements d’évaluation et de production Pro, exécutez :

```terminal
valkey-server -v
```

**Exemple de réponse**

```text
Valkey server v=<installed-version> ...
```

Les détails de version et de build varient selon l’environnement. Ne traitez pas un exemple de version affiché comme une version requise ou de service universel.

>[!ENDTABS]

## Résolution des problèmes liés à Valkey

### Les erreurs de nettoyage du cache référencent Redis sur un cache configuré par Valkey

Un échec de nettoyage du cache avant déploiement peut afficher le `[107]` de code d’erreur (`clean-redis-cache`) et un message d’`Connection to Redis`, même lorsque le service `cache` est configuré comme Valkey. `ece-tools` utilise ce code d’erreur et ce message pour l’étape de nettoyage du cache, que le service de cache de sauvegarde soit Redis ou Valkey.

Si l’erreur sous-jacente est un échec du DNS, par exemple `Name or service not known` pour l’hôte de relation, l’étape de déploiement s’est exécutée avant que la relation de service ne soit disponible, ou le nom de la relation dans `.magento.app.yaml` ne correspond pas à l’ID de service dans `.magento/services.yaml`. Voir [Vérifier la relation de service](#verify-the-service-relationship).
