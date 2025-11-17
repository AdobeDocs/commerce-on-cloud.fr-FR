---
title: Configuration du service Elasticsearch
description: Découvrez comment activer le service Elasticsearch pour Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Search, Services
exl-id: 238b9ed5-ce73-428f-9459-35de8573d5d8
source-git-commit: ef22e7b305c20148f4ee4b2c0e64e2114bf229b5
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# Configuration du service Elasticsearch

[Elasticsearch](https://www.elastic.co) est un produit open-source qui vous permet de prendre des données de n’importe quelle source, n’importe quel format, et de les rechercher et de les visualiser en temps réel.

{{elasticsearch-support}}

Pour Adobe Commerce version 2.4.4 et ultérieure, voir [Configuration du service OpenSearch](opensearch.md).

- Elasticsearch effectue des recherches rapides et avancées sur les produits du catalogue
- Les analyseurs Elasticsearch prennent en charge plusieurs langues
- Prend en charge les mots vides et les synonymes
- L’indexation n’affecte pas les clients tant que l’opération de réindexation n’est pas terminée

>[!TIP]
>
>Adobe vous recommande de toujours configurer Elasticsearch pour votre projet d’infrastructure Adobe Commerce on cloud, même si vous envisagez de configurer un outil de recherche tiers pour votre application Adobe Commerce. La configuration d’Elasticsearch fournit une option de secours en cas d’échec de l’outil de recherche tiers.

{{service-instruction}}

**Pour activer Elasticsearch** :

1. Pour les projets de démarrage, ajoutez le service `elasticsearch` au fichier `.magento/services.yaml` avec la version d’Elasticsearch et l’espace disque alloué en Mo.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Pour les projets Pro, vous devez envoyer un ticket d’assistance Adobe Commerce pour modifier la version d’Elasticsearch dans les environnements d’évaluation et de production.

1. Définissez la propriété `relationships` dans le fichier `.magento.app.yaml`.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. Ajout, validation et modifications de code push.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   Pour plus d’informations sur l’impact de ces modifications sur vos environnements, voir [Services](services-yaml.md).

1. Une fois le processus de déploiement terminé, utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Réindexez l’index de recherche catalogue.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Nettoyez le cache.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Compatibilité logicielle Elasticsearch

Lorsque vous installez ou mettez à niveau votre projet d’infrastructure Adobe Commerce sur le cloud, vérifiez toujours la compatibilité entre la version du service Elasticsearch et le client [Elasticsearch PHP](https://github.com/elastic/elasticsearch-php) pour Adobe Commerce.

- **Première configuration**-Vérifiez que la version d’Elasticsearch spécifiée dans le fichier `services.yaml` est compatible avec le client Elasticsearch PHP configuré pour Adobe Commerce.

- **Mise à niveau du projet**-Vérifiez que le client PHP Elasticsearch dans la nouvelle version de l’application est compatible avec la version du service Elasticsearch installée sur l’infrastructure cloud.

La prise en charge des versions de service et de la compatibilité pour Adobe Commerce sur l’infrastructure cloud est déterminée par les versions déployées sur l’infrastructure cloud et diffère parfois des versions prises en charge par les déploiements sur site d’Adobe Commerce. Voir [Versions des services](services-yaml.md#service-versions).

**Pour vérifier la compatibilité logicielle d’Elasticsearch** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Afficher les détails Elasticsearch de l’environnement actif.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. Vous pouvez également utiliser SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Vérifiez la version du package du compositeur pour `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   Dans la réponse, vérifiez la version installée dans la propriété `versions` .

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   En outre, la version du client PHP Elasticsearch est disponible dans le fichier `composer.lock` du répertoire racine de l&#39;environnement.

1. À partir de la ligne de commande, récupérez les détails de connexion au service Elasticsearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Dans la réponse, recherchez l’adresse IP du point d’entrée du service Elasticsearch :

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. Récupérez le `version:number` de service Elasticsearch installé à partir du point d’entrée du service.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Vérifiez la compatibilité des versions entre le service Elasticsearch et le client PHP.

   Si les versions sont incompatibles, effectuez l’une des mises à jour suivantes dans la configuration de votre environnement :

   - Remplacez le client PHP Elasticsearch par une version compatible avec la version du service Elasticsearch.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - Remplacez la version du service Elasticsearch du fichier `services.yaml` par une version compatible avec le client Elasticsearch PHP.

     {{pro-update-service}}

## Redémarrez le service Elasticsearch

Si vous devez redémarrer le service [Elasticsearch](https://www.elastic.co), vous devez contacter l’assistance Adobe Commerce.

## Configuration de recherche supplémentaire

- Par défaut, la configuration de la recherche pour les environnements cloud se régénère à chaque déploiement. Vous pouvez utiliser la variable de déploiement `SEARCH_CONFIGURATION` pour conserver les paramètres de recherche personnalisés entre les déploiements. Voir [Déploiement de variables](../environment/variables-deploy.md#search_configuration).

- Une fois que vous avez configuré le service Elasticsearch pour votre projet, utilisez l’interface utilisateur d’administration pour tester la connexion Elasticsearch et personnaliser les paramètres Elasticsearch pour Adobe Commerce.

### Ajout de modules externes pour Elasticsearch

Vous pouvez éventuellement ajouter des modules externes pour Elasticsearch en ajoutant la section `configuration:plugins` au service Elasticsearch dans le fichier `.magento/services.yaml`. Par exemple, le code suivant active les modules externes d’analyse ICU et d’analyse phonétique.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Si vous utilisez le plug-in tiers Elastic Suite, vous devez [mettre à jour le package `ece-tools`](../dev-tools/update-package.md) vers la version 2002.0.19 ou ultérieure.
Lors de la configuration d’Elastic Suite, ajoutez les paramètres de configuration à la variable de déploiement `ELASTICSUITE_CONFIGURATION`. Cette configuration enregistre les paramètres dans tous les déploiements.

### Suppression de modules externes pour Elasticsearch

La suppression des entrées du plug-in d’`elasticsearch:` dans `.magento/services.yaml` ne les désinstalle pas ou ne les désactive pas comme vous pourriez vous y attendre. Vous devez réindexer vos données Elasticsearch. Ce comportement est destiné à empêcher la perte ou la corruption possible des données qui dépendent de ces modules externes.

**Pour supprimer les modules externes d’Elasticsearch** :

1. Supprimez les entrées du module externe Elasticsearch de votre fichier `.magento/services.yaml`.
1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Validez les modifications `.magento/services.yaml` apportées à votre référentiel cloud.
1. Réindexez l’index de recherche catalogue.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Nettoyez le cache.

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Pour plus d’informations sur l’utilisation ou la résolution des problèmes du plug-in Elastic Suite avec Adobe Commerce, consultez la [documentation Elastic Suite](https://github.com/Smile-SA/elasticsuite).

