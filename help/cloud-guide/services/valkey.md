---
title: Configuration du service Valkey
description: Découvrez comment configurer et optimiser Valkey en tant que solution de cache principale pour Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
source-git-commit: 242582ea61d0d93725a7f43f2ca834db9e1a7c29
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Configuration du service Valkey

[Valkey](https://valkey.io) est une solution de cache d’arrière-plan facultative qui remplace le `Zend Framework Zend_Cache_Backend_File` utilisé par défaut par Adobe Commerce.

Voir [Configurer Valkey](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html){target="_blank"} dans le _Guide de configuration_.

{{service-instruction}}

**Pour activer Valkey** :

1. Ajoutez le nom et le type requis au fichier `.magento/services.yaml`.

   ```yaml
   cache:
       type: valkey:<version>
   ```

   Pour fournir votre propre configuration Valkey, ajoutez une clé `core_config` dans votre fichier `.magento/services.yaml` :

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. Configurez les relations dans le fichier `.magento.app.yaml`.

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Vérifier les relations de service](services-yaml.md#service-relationships).

{{service-change-tip}}

## Utilisation de l’interface de ligne de commande Valkey

En supposant que votre relation Valkey soit nommée `valkey`, vous pouvez y accéder à l’aide de l’outil `valkey-cli` .

1. Utilisez SSH pour vous connecter à l’environnement d’intégration avec Valkey installé et configuré.

1. Ouvrez un tunnel SSH vers un hôte.

   ```bash
   valkey-cli -h valkey.internal
   ```

## Obtenir la version de Valkey installée

Utilisez la commande suivante pour installer la version de Valkey dans un environnement d’intégration :

```bash
valkey-cli -h valkey.internal info | grep version
```

Réponse :

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valkey sur l’évaluation et la production Pro

Pour installer la version de Valkey dans un environnement d’évaluation ou de production, utilisez la commande `valkey-server` :

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

Utilisez la commande suivante pour installer la configuration Valkey dans un environnement d’évaluation ou de production Pro :

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Réponse :

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
