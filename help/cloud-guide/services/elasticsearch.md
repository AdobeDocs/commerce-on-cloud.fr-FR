---
title: Configurer le service Elasticsearch
description: Découvrez comment activer le service Elasticsearch pour Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# Configurer le service Elasticsearch

[Elasticsearch &#x200B;](https://www.elastic.co) est un produit open-source qui vous permet de prendre des données de n’importe quelle source, n’importe quel format, et de les rechercher et de les visualiser en temps réel.

{{elasticsearch-support}}

Pour Adobe Commerce version 2.4.4 et ultérieure, voir [Configuration du service OpenSearch](opensearch.md).

- Elasticsearch effectue des recherches rapides et avancées sur les produits du catalogue de produits
- Les analyseurs Elasticsearch prennent en charge plusieurs langues
- Prend en charge les mots vides et les synonymes
- L’indexation n’affecte pas les clients tant que l’opération de réindexation n’est pas terminée

>[!TIP]
>
>Adobe vous recommande de toujours configurer Elasticsearch pour votre projet d’infrastructure Adobe Commerce on cloud, même si vous envisagez de configurer un outil de recherche tiers pour votre application Adobe Commerce. La configuration d’Elasticsearch fournit une option de secours en cas d’échec de l’outil de recherche tiers.

{{service-instruction}}

**Pour activer l’Elasticsearch** :

1. Pour les projets de démarrage, ajoutez le service `elasticsearch` au fichier `.magento/services.yaml` avec la version Elasticsearch et l’espace disque alloué en Mo.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Pour les projets Pro, vous devez envoyer un ticket d’assistance Adobe Commerce pour modifier la version de l’Elasticsearch dans les environnements d’évaluation et de production.

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

- **Première configuration**-Vérifiez que la version Elasticsearch spécifiée dans le fichier `services.yaml` est compatible avec le client PHP Elasticsearch configuré pour Adobe Commerce.

- **Mise à niveau du projet**-Vérifiez que le client PHP Elasticsearch dans la nouvelle version de l&#39;application est compatible avec la version du service Elasticsearch installée sur l&#39;infrastructure cloud.

La prise en charge des versions de service et de la compatibilité pour Adobe Commerce sur l’infrastructure cloud est déterminée par les versions déployées sur l’infrastructure cloud et diffère parfois des versions prises en charge par les déploiements sur site d’Adobe Commerce. Voir [Versions des services](services-yaml.md#service-versions).

**Pour vérifier la compatibilité logicielle Elasticsearch** :

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

   En outre, vous pouvez retrouver la version Elasticsearch du client PHP dans le fichier `composer.lock` dans le répertoire racine de l&#39;environnement.

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

   - Remplacez la version du service Elasticsearch dans le fichier `services.yaml` par une version compatible avec le client PHP Elasticsearch.

     {{pro-update-service}}

## Redémarrez le service Elasticsearch

Si vous devez redémarrer le service [Elasticsearch &#x200B;](https://www.elastic.co), vous devez contacter l&#39;assistance Adobe Commerce.

## Configuration de recherche supplémentaire

- Par défaut, la configuration de la recherche pour les environnements cloud se régénère à chaque déploiement. Vous pouvez utiliser la variable de déploiement `SEARCH_CONFIGURATION` pour conserver les paramètres de recherche personnalisés entre les déploiements. Voir [Déploiement de variables](../environment/variables-deploy.md#search_configuration).

- Une fois que vous avez configuré le service Elasticsearch pour votre projet, utilisez l’interface utilisateur d’administration pour tester la connexion Elasticsearch et personnaliser les paramètres Elasticsearch pour Adobe Commerce.

### Ajout de modules externes pour l’Elasticsearch

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

### Supprimer les modules externes pour l’Elasticsearch

La suppression des entrées du plug-in d’`elasticsearch:` dans `.magento/services.yaml` ne les désinstalle pas ou ne les désactive pas comme vous pourriez vous y attendre. Vous devez réindexer les données de votre Elasticsearch. Ce comportement est destiné à empêcher la perte ou la corruption possible des données qui dépendent de ces modules externes.

**Pour supprimer des modules externes Elasticsearch** :

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

## Dépannage

Consultez les articles d’assistance Adobe Commerce suivants pour obtenir de l’aide sur la résolution des problèmes Elasticsearch :

- [L&#39;Elasticsearch 5 est configuré, mais la page de recherche ne se charge pas avec l&#39;erreur « Fielddata is disabled... » &#x200B;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html?lang=fr)
- [Elasticsearch dans l’utilitaire de résolution de problèmes Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Statut de l&#39;index Elasticsearch `yellow` ou `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html?lang=fr)
