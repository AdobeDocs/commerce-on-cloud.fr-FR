---
title: Configuration du service OpenSearch
description: Découvrez comment activer le service OpenSearch pour Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Search, Services
exl-id: e704ab2a-2f6b-480b-9b36-1e97c406e873
source-git-commit: 5a190471f4ccc23eb1c311f3082af1948cf1c68d
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 0%

---

# Configuration du service OpenSearch

Le service [OpenSearch](https://www.opensearch.org) est un branchement open source d’Elasticsearch 7.10.2, qui fait suite aux modifications apportées aux licences d’Elasticsearch. Voir le [Projet OpenSource](https://github.com/opensearch-project) dans GitHub.

{{elasticsearch-support}}

OpenSearch vous permet de prendre des données de n’importe quelle source, n’importe quel format, et de les rechercher et de les visualiser en temps réel.

- Recherches rapides et avancées sur les produits du catalogue
- Les analyseurs OpenSearch prennent en charge plusieurs langues
- Prend en charge les mots vides et les synonymes
- L’indexation n’affecte pas les clients tant que l’opération de réindexation n’est pas terminée

{{service-instruction}}

>[!TIP]
>
>Pour les projets d’infrastructure cloud d’Adobe Commerce qui n’utilisent pas [Live Search](https://experienceleague.adobe.com/en/docs/commerce/live-search/overview), Adobe recommande de configurer [!DNL OpenSearch] pour fournir une option de secours pour les outils de recherche tiers. Cependant, [!DNL OpenSearch] et [!DNL Live Search] ne peuvent pas être activés tous les deux sur la même instance Commerce.

**Pour activer OpenSearch** :

1. Pour les environnements d’intégration, ajoutez le service `opensearch` au fichier `.magento/services.yaml` avec la version appropriée et l’espace disque alloué en Mo. Dans ce cas, la version 2 est appropriée. La version mineure n’est pas requise.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Pour les projets Pro, vous devez [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour modifier la version OpenSearch dans les environnements d’évaluation et de production.

1. Définissez ou vérifiez la propriété `relationships` dans le fichier `.magento.app.yaml`.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. Ajout, validation et modifications de code push.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Pour plus d’informations sur l’impact de ces modifications sur vos environnements, consultez [Configuration des services](services-yaml.md).

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

## Compatibilité du logiciel OpenSearch

Lorsque vous installez ou mettez à niveau votre projet d’infrastructure Adobe Commerce sur le cloud, vérifiez toujours la compatibilité entre la version du service OpenSearch et le client [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) pour Adobe Commerce.

- **Première configuration**-Vérifiez que la version OpenSearch spécifiée dans le fichier `services.yaml` est compatible avec le client PHP OpenSearch configuré pour Adobe Commerce.

- **Mise à niveau du projet**-Vérifiez que le client PHP OpenSearch dans la nouvelle version de l’application est compatible avec la version du service OpenSearch installée sur l’infrastructure cloud.

La prise en charge des versions de service et de la compatibilité est déterminée par les versions testées et déployées sur l’infrastructure cloud et diffère parfois des versions prises en charge par les déploiements sur site d’Adobe Commerce. Consultez [Configuration requise](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) dans le _Guide d’installation_ pour obtenir la liste des versions prises en charge.

**Pour vérifier la compatibilité du logiciel OpenSearch** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Afficher les détails OpenSearch pour l’environnement actif.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. Vous pouvez également utiliser SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Récupérez les détails de connexion au service OpenSearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Dans la réponse, recherchez l’adresse IP et le port du point d’entrée du service OpenSearch :

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. Récupérez le `version:number` de service OpenSearch installé à partir du point d’entrée du service.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## Redémarrez le service OpenSearch

Si vous devez redémarrer le service OpenSearch, vous devez contacter l’assistance Adobe Commerce.

## Configuration de recherche supplémentaire

- Par défaut, la configuration de la recherche pour les environnements cloud se régénère à chaque déploiement. Vous pouvez utiliser la variable de déploiement `SEARCH_CONFIGURATION` pour conserver les paramètres de recherche personnalisés entre les déploiements. Voir [Déploiement de variables](../environment/variables-deploy.md#search_configuration).

- Une fois que vous avez configuré le service OpenSearch pour votre projet, utilisez l’interface utilisateur d’administration pour tester la connexion OpenSearch et personnaliser les paramètres OpenSearch pour Adobe Commerce.

### Ajout de modules externes pour OpenSearch

Vous pouvez éventuellement ajouter des modules externes pour OpenSearch en ajoutant la section `configuration:plugins` au service OpenSearch dans le fichier `.magento/services.yaml`. Par exemple, le code suivant active les modules externes d’analyse ICU et d’analyse phonétique.

>[!NOTE]
>
>Cela s’applique uniquement aux environnements d’intégration et de démarrage. Pour installer les modules externes dans un cluster d’évaluation ou de production Pro, [envoyez une demande d’assistance](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case).


```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Voir le [Projet OpenSearch](https://github.com/opensearch-project) pour plus d’informations sur les modules externes.

### Suppression de modules externes pour OpenSearch

La suppression des entrées du module externe de la section `opensearch:` du fichier `.magento/services.yaml` ne désinstalle **pas** ni ne désactive le service. Pour désactiver complètement le service, vous devez réindexer vos données OpenSearch après avoir supprimé les modules externes de votre fichier `.magento/services.yaml`. Cette conception empêche la perte ou la corruption possible des données qui dépendent de ces modules externes.


**Pour supprimer les modules externes OpenSearch** :

>[!NOTE]
>
>Cette modification s’applique uniquement aux environnements d’intégration et de démarrage. Vous devrez [soumettre un ticket d’assistance](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) pour supprimer le plug-in dans un cluster d’évaluation ou de production Pro.

1. Supprimez les entrées du module externe OpenSearch de votre fichier `.magento/services.yaml`.
1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Validez les modifications `.magento/services.yaml` dans votre référentiel cloud.
1. Réindexez l’index de recherche catalogue (tous les environnements : clusters d’intégration, de démarrage, d’évaluation pro et de production).

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Nettoyez le cache.

   ```bash
   bin/magento cache:clean
   ```
