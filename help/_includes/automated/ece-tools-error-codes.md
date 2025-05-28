---
source-git-commit: 7f2934af84c947046fed3a32c3b6e2937aed418a
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# Codes d&#39;erreur ECE-Tools

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## Erreurs critiques

Les erreurs critiques indiquent un problème de configuration du projet Commerce sur l’infrastructure cloud qui provoque l’échec du déploiement, par exemple une configuration incorrecte, non prise en charge ou manquante pour les paramètres requis. Avant de pouvoir effectuer un déploiement, vous devez mettre à jour la configuration pour résoudre ces erreurs.

### Étape de création

| Code d’erreur | Étape de création | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 2 |  | Impossible d’écrire dans le fichier `./app/etc/env.php` | Le script de déploiement ne peut pas apporter les modifications requises au fichier `/app/etc/env.php`. Vérifiez les autorisations de votre système de fichiers. |
| 3 |  | La configuration n’est pas définie dans le fichier `schema.yaml` | La configuration n’est pas définie dans le fichier `./vendor/magento/ece-tools/config/schema.yaml`. Vérifiez que le nom de la variable de configuration est correct et défini. |
| 4 |  | Échec de l’analyse du fichier `.magento.env.yaml` | Le format de fichier `./.magento.env.yaml` n&#39;est pas valide. Utilisez un analyseur YAML pour vérifier la syntaxe et corriger les erreurs. |
| 5 |  | Impossible de lire le fichier `.magento.env.yaml` | Impossible de lire le fichier `./.magento.env.yaml`. Vérifiez les autorisations de fichier. |
| 6 |  | Impossible de lire le fichier `.schema.yaml` | Impossible de lire le fichier `./vendor/magento/ece-tools/config/magento.env.yaml`. Vérifiez les autorisations de fichier et redéployez (`magento-cloud environment:redeploy`). |
| 7 | refresh-modules | Impossible d’écrire dans le fichier `./app/etc/config.php` | Le script de déploiement ne peut pas apporter les modifications requises au fichier `/app/etc/config.php`. Vérifiez les autorisations de votre système de fichiers. |
| 8 | validate-config | Impossible de lire le fichier `composer.json` | Impossible de lire le fichier `./composer.json`. Vérifiez les autorisations de fichier. |
| 9 | validate-config | Le fichier `composer.json` ne contient pas la section de chargement automatique requise | La section de `autoload` obligatoire est manquante dans le fichier `composer.json`. Comparez la section de chargement automatique au fichier `composer.json` dans le modèle cloud et ajoutez la configuration manquante. |
| 10 | validate-config | Le fichier `.magento.env.yaml` contient une option qui n’est pas déclarée dans le schéma, ou une option configurée avec une valeur ou une étape non valide | Le fichier `./.magento.env.yaml` contient une configuration non valide. Consultez le journal des erreurs pour obtenir des informations détaillées. |
| 11 | refresh-modules | Échec de la commande : `/bin/magento module:enable --all` | Essayez d’exécuter `composer update` localement. Ensuite, validez et envoyez le fichier `composer.lock` mis à jour. Vérifiez également le `cloud.log` pour plus d’informations. Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 12 | apply-patches | Échec de l’application du correctif |  |
| 13 | set-report-dir-nested-level | Impossible d&#39;écrire dans le fichier `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | Échec de la copie des fichiers de données d’exemple |  |
| 15 | compile-di | Échec de la commande : `/bin/magento setup:di:compile` | Pour plus d’informations, consultez la `cloud.log` . Ajoutez `VERBOSE_COMMANDS: '-vvv'` dans `.magento.env.yaml` pour une sortie de commande plus détaillée. |
| 16 | dump-autoload | Échec de la commande : `composer dump-autoload` | La commande `composer dump-autoload` a échoué. Pour plus d’informations, consultez la `cloud.log` . |
| 17 | ramasseuse-presse | Échec de la commande d’exécution de `Baler` pour le regroupement JavaScript | Vérifiez la variable d’environnement `SCD_USE_BALER` pour vous assurer que le module Baler est configuré et activé pour le regroupement JS. Si vous n’avez pas besoin du module Baler, définissez `SCD_USE_BALER: false`. |
| 18 | compress-static-content | Utilitaire requis introuvable (temporisation, bash) |  |
| 19 | deploy-static-content | Échec de la `/bin/magento setup:static-content:deploy` de commande | Pour plus d’informations, consultez la `cloud.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 20 | compress-static-content | Échec de la compression du contenu statique | Pour plus d’informations, consultez la `cloud.log` . |
| 21 | backup-data : static-content | Échec de la copie du contenu statique dans le répertoire `init` | Pour plus d’informations, consultez la `cloud.log` . |
| 22 | backup-data : writable-dirs | Impossible de copier certains répertoires accessibles en écriture dans le répertoire `init` | Impossible de copier les répertoires accessibles en écriture dans le dossier `./init`. Vérifiez les autorisations de votre système de fichiers. |
| 23 |  | Impossible de créer un objet enregistreur |  |
| 24 | backup-data : static-content | Impossible de nettoyer le répertoire `./init/pub/static/` | Échec du nettoyage du dossier `./init/pub/static`. Vérifiez les autorisations de votre système de fichiers. |
| 25 |  | Impossible de trouver le package du compositeur | Si vous avez installé l’application Adobe Commerce directement à partir du référentiel GitHub, vérifiez que la variable d’environnement `DEPLOYED_MAGENTO_VERSION_FROM_GIT` est configurée. |
| 26 | validate-config | Supprimez la configuration du module Magento Braintree qui n’est plus prise en charge dans Adobe Commerce et Magento Open Source 2.4 et les versions ultérieures. | La prise en charge du module Braintree n’est plus incluse dans Magento 2.4.0 et versions ultérieures. Supprimez la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la section des variables du fichier `.magento.app.yaml`. Pour la prise en charge des paiements Braintree, utilisez plutôt une extension officielle du Commerce Marketplace . |

### Étape de déploiement

| Code d’erreur | Étape de déploiement | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 101 | pré-déploiement : cache | Configuration de cache incorrecte (port ou hôte manquant) | Il manque des paramètres obligatoires `server` ou `port` à la configuration du cache. Pour plus d’informations, consultez la `cloud.log` . |
| 102 |  | Impossible d’écrire dans le fichier `./app/etc/env.php` | Le script de déploiement ne peut pas apporter les modifications requises au fichier `/app/etc/env.php`. Vérifiez les autorisations de votre système de fichiers. |
| 103 |  | La configuration n’est pas définie dans le fichier `schema.yaml` | La configuration n’est pas définie dans le fichier `./vendor/magento/ece-tools/config/schema.yaml`. Vérifiez que le nom de la variable de configuration est correct et qu&#39;il est défini. |
| 104 |  | Échec de l’analyse du fichier `.magento.env.yaml` | La configuration n’est pas définie dans le fichier `./vendor/magento/ece-tools/config/schema.yaml`. Vérifiez que le nom de la variable de configuration est correct et qu&#39;il est défini. |
| 105 |  | Impossible de lire le fichier `.magento.env.yaml` | Impossible de lire le fichier `./.magento.env.yaml`. Vérifiez les autorisations de fichier. |
| 106 |  | Impossible de lire le fichier `.schema.yaml` |  |
| 107 | pré-déploiement : clean-red-cache | Échec du nettoyage du cache Redis | Échec du nettoyage du cache Redis. Vérifiez que la configuration du cache Redis est correcte et que le service Redis est disponible. Voir [Configuration du service Redis](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html?lang=fr). |
| 140 | pré-déploiement : clean-valkey-cache | Échec du nettoyage du cache Valkey | Échec du nettoyage du cache Valkey. Vérifiez que la configuration du cache Valkey est correcte et que le service Valkey est disponible. Voir [Configurer le service Valkey](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/valkey.html?lang=fr). |
| 108 | pré-déploiement : set-production-mode | Échec de la `/bin/magento maintenance:enable` de commande | Pour plus d’informations, consultez la `cloud.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 109 | validate-config | Configuration de base de données incorrecte | Vérifiez que la variable d’environnement `DATABASE_CONFIGURATION` est correctement configurée. |
| 110 | validate-config | Configuration de session incorrecte | Vérifiez que la variable d’environnement `SESSION_CONFIGURATION` est correctement configurée. La configuration doit contenir au moins le paramètre `save` . |
| 111 | validate-config | Configuration de recherche incorrecte | Vérifiez que la variable d’environnement `SEARCH_CONFIGURATION` est correctement configurée. La configuration doit contenir au moins le paramètre `engine` . |
| 112 | validate-config | Configuration de ressource incorrecte | Vérifiez que la variable d’environnement `RESOURCE_CONFIGURATION` est correctement configurée. La configuration doit contenir au moins `connection` paramètre. |
| 113 | validate-config:elasticsuite-integrity | ElasticSuite est installé, mais le service Elasticsearch n’est pas disponible | Vérifiez que la variable d’environnement `SEARCH_CONFIGURATION` est configurée correctement et que le service Elasticsearch est disponible. |
| 114 | validate-config:elasticsuite-integrity | ElasticSuite est installé, mais un autre moteur de recherche est utilisé | ElasticSuite est installé, mais un autre moteur de recherche est configuré. Mettez à jour la variable d’environnement `SEARCH_CONFIGURATION` pour activer Elasticsearch et vérifiez la configuration du service Elasticsearch dans le fichier `services.yaml`. |
| 115 |  | Échec de l&#39;exécution de la requête de base de données |  |
| 116 | install-update : installation | Échec de la `/bin/magento setup:install` de commande | Pour plus d’informations, consultez les `cloud.log` et `install_upgrade.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 117 | install-update : config-import | Échec de la `app:config:import` de commande | Pour plus d’informations, consultez la `cloud.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 118 |  | Utilitaire requis introuvable (temporisation, bash) |  |
| 119 | install-update : deploy-static-content | Échec de la `/bin/magento setup:static-content:deploy` de commande | Pour plus d’informations, consultez la `cloud.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 120 | compress-static-content | Échec de la compression du contenu statique | Pour plus d’informations, consultez la `cloud.log` . |
| 121 | deploy-static-content:generate | Impossible de mettre à jour la version déployée | Impossible de mettre à jour le fichier `./pub/static/deployed_version.txt`. Vérifiez les autorisations de votre système de fichiers. |
| 122 | clean-static-content | Échec du nettoyage des fichiers de contenu statiques |  |
| 123 | install-update : split-db | Échec de la `/bin/magento setup:db-schema:split` de commande | Pour plus d’informations, consultez la `cloud.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 124 | clean-view-preprocessed | Échec du nettoyage du dossier `var/view_preprocessed` | Impossible de nettoyer le dossier `./var/view_preprocessed`. Vérifiez les autorisations de votre système de fichiers. |
| 125 | install-update : reset-password | Échec de la mise à jour du fichier `/var/credentials_email.txt` | Échec de la mise à jour du fichier `/var/credentials_email.txt`. Vérifiez les autorisations de votre système de fichiers. |
| 126 | install-update : mise à jour | Échec de la `/bin/magento setup:upgrade` de commande | Pour plus d’informations, consultez les `cloud.log` et `install_upgrade.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 127 | clean-cache | Échec de la `/bin/magento cache:flush` de commande | Pour plus d’informations, consultez la `cloud.log` . Pour une sortie de commande plus détaillée, ajoutez l’option `VERBOSE_COMMANDS: '-vvv'` au fichier `.magento.env.yaml`. |
| 128 | disable-maintenance-mode | Échec de la `/bin/magento maintenance:disable` de commande | Pour plus d’informations, consultez la `cloud.log` . Ajoutez `VERBOSE_COMMANDS: '-vvv'` dans `.magento.env.yaml` pour une sortie de commande plus détaillée. |
| 129 | install-update : reset-password | Impossible de lire le modèle de mot de passe de réinitialisation |  |
| 130 | install-update : cache_type | Échec de la commande : `php ./bin/magento cache:enable` | La commande `php ./bin/magento cache:enable` s’exécute uniquement lors de l’installation d’Adobe Commerce, mais `./app/etc/env.php` fichier était absent ou vide au début du déploiement. Pour plus d’informations, consultez la `cloud.log` . Ajoutez `VERBOSE_COMMANDS: '-vvv'` dans `.magento.env.yaml` pour une sortie de commande plus détaillée. |
| 131 | install-update | La valeur de clé `crypt/key` n’existe pas dans le fichier `./app/etc/env.php` ou la variable d’environnement cloud `CRYPT_KEY` | Cette erreur se produit si le fichier `./app/etc/env.php` n’est pas présent au début du déploiement d’Adobe Commerce ou si la valeur `crypt/key` est indéfinie. Si vous avez migré la base de données à partir d’un autre environnement, récupérez la valeur de la clé de chiffrement de cet environnement. Ajoutez ensuite la valeur à la variable d’environnement cloud [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html?lang=fr#crypt_key) dans votre environnement actuel. Voir [Clé de chiffrement Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html?lang=fr#gather-credentials). Si vous avez accidentellement supprimé le fichier `./app/etc/env.php`, utilisez la commande suivante pour le restaurer à partir des fichiers de sauvegarde créés à partir d’un déploiement précédent : `./vendor/bin/ece-tools backup:restore` la commande CLI. » |
| 132 |  | Impossible de se connecter au service Elasticsearch | Recherchez des informations d’identification Elasticsearch valides et vérifiez que le service est en cours d’exécution |
| 137 |  | Connexion au service OpenSearch impossible | Recherchez des informations d’identification OpenSearch valides et vérifiez que le service fonctionne |
| 133 | validate-config | Supprimez la configuration du module Magento Braintree qui n’est plus prise en charge dans Adobe Commerce ou Magento Open Source 2.4 et les versions ultérieures. | La prise en charge du module Braintree n’est plus incluse dans Adobe Commerce ou Magento Open Source 2.4.0 et versions ultérieures. Supprimez la variable CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL de la section des variables du fichier `.magento.app.yaml`. Pour la prise en charge de Braintree, utilisez plutôt une extension Braintree Payments officielle depuis Commerce Marketplace. |
| 134 | validate-config | Adobe Commerce et Magento Open Source 2.4.0 nécessitent l’installation du service Elasticsearch | Installer le service Elasticsearch |
| 138 | validate-config | Adobe Commerce et Magento Open Source 2.4.4 nécessitent que le service OpenSearch ou Elasticsearch soit installé | Installer le service OpenSearch |
| 135 | validate-config | Le moteur de recherche doit être défini sur Elasticsearch pour Adobe Commerce et Magento Open Source >= 2.4.0 | Vérifiez la variable SEARCH_CONFIGURATION pour l’option `engine` . S’il est configuré, supprimez l’option ou définissez la valeur sur « elasticsearch ». |
| 136 | validate-config | La base de données fractionnée a été supprimée à partir d&#39;Adobe Commerce et de Magento Open Source 2.5.0. | Si vous utilisez une base de données fractionnée, vous devez revenir à une base de données unique, la migrer vers une base de données unique ou utiliser une autre approche. |
| 139 | validate-config | Moteur de recherche incorrect | Cette version d’Adobe Commerce ou de Magento Open Source ne prend pas en charge OpenSearch. Utilisez les versions 2.3.7-p3, 2.4.3-p2 ou ultérieures. |

### Étape de post-déploiement

| Code d’erreur | Étape de post-déploiement | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 201 | is-deploy-failed | Échec du déploiement de l’étape |  |
| 202 |  | Le fichier `./app/etc/env.php` n&#39;est pas accessible en écriture | Le script de déploiement ne peut pas apporter les modifications requises au fichier `/app/etc/env.php`. Vérifiez les autorisations de votre système de fichiers. |
| 203 |  | La configuration n’est pas définie dans le fichier `schema.yaml` | La configuration n’est pas définie dans le fichier `./vendor/magento/ece-tools/config/schema.yaml`. Vérifiez que le nom de la variable de configuration est correct et qu&#39;il est défini. |
| 204 |  | Échec de l’analyse du fichier `.magento.env.yaml` | Le format de fichier `./.magento.env.yaml` n&#39;est pas valide. Utilisez un analyseur YAML pour vérifier la syntaxe et corriger les erreurs. |
| 205 |  | Impossible de lire le fichier `.magento.env.yaml` | Vérifiez les autorisations de fichier. |
| 206 |  | Impossible de lire le fichier `.schema.yaml` |  |
| 207 | préchauffage | Échec du préchargement de certaines pages de préchauffage |  |
| 208 | time-to-first-byte | Échec du test du temps sur le premier octet (TTFB) |  |
| 227 | clean-cache | Échec de la `/bin/magento cache:flush` de commande | Pour plus d’informations, consultez la `cloud.log` . Ajoutez `VERBOSE_COMMANDS: '-vvv'` dans `.magento.env.yaml` pour une sortie de commande plus détaillée. |

### Général

| Code d’erreur | Étape générale | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 243 |  | La configuration n’est pas définie dans le fichier `schema.yaml` | Vérifiez que le nom de la variable de configuration est correct et qu&#39;il est défini. |
| 244 |  | Échec de l’analyse du fichier `.magento.env.yaml` | Le format de fichier `./.magento.env.yaml` n&#39;est pas valide. Utilisez un analyseur YAML pour vérifier la syntaxe et corriger les erreurs. |
| 245 |  | Impossible de lire le fichier `.magento.env.yaml` | Impossible de lire le fichier `./.magento.env.yaml`. Vérifiez les autorisations de fichier. |
| 246 |  | Impossible de lire le fichier `.schema.yaml` |  |
| 247 |  | Impossible de générer un module pour les événements | Pour plus d’informations, consultez la `cloud.log` . |
| 248 |  | Impossible d’activer un module pour les événements | Pour plus d’informations, consultez la `cloud.log` . |
| 249 |  | Échec de la génération du module AdobeCommerceWebhookPlugins | Pour plus d’informations, consultez la `cloud.log` . |
| 250 |  | Échec de l’activation du module AdobeCommerceWebhookPlugins | Pour plus d’informations, consultez la `cloud.log` . |

## Erreurs d’avertissement

Les erreurs d’avertissement indiquent un problème de configuration du projet Commerce sur l’infrastructure cloud, tel que des paramètres de configuration incorrects, obsolètes, non pris en charge ou manquants pour les fonctionnalités facultatives pouvant affecter le fonctionnement du site. Bien qu’un avertissement ne provoque pas d’échec de déploiement, vous devez consulter les messages d’avertissement et mettre à jour la configuration pour les résoudre.

### Étape de création

| Code d’erreur | Étape de création | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 1001 | validate-config | Le fichier app/etc/config.php n&#39;existe pas |  |
| 1002 | validate-config | Le .Le fichier /build_options.ini n’est plus pris en charge |  |
| 1003 | validate-config | La section Modules est absente du fichier de configuration partagé |  |
| 1004 | validate-config | La configuration n’est pas compatible avec cette version de Magento |  |
| 1005 | validate-config | Options SCD ignorées |  |
| 1006 | validate-config | L’état configuré n’est pas idéal |  |
| 1007 | ramasseuse-presse | Impossible d’utiliser le regroupement JS de la presse |  |

### Étape de déploiement

| Code d’erreur | Étape de déploiement | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 2001 | pre-deploy:cache | Le cache est configuré pour un service Redis qui n’est pas disponible. La configuration est ignorée. |  |
| 2032 | pre-deploy:cache | Le cache est configuré pour un service Valkey qui n&#39;est pas disponible. La configuration est ignorée. |  |
| 2002 | validate-config | L’état configuré n’est pas idéal |  |
| 2003 | validate-config | La valeur du niveau d’imbrication du répertoire pour les rapports d’erreur n’a pas été configurée |  |
| 2004 | validate-config | Configuration non valide dans le .Fichier /pub/errors/local.xml. |  |
| 2005 | validate-config | Les données d’administration sont utilisées pour créer un utilisateur administrateur lors de l’installation initiale uniquement. Toute modification apportée aux données d’administration est ignorée pendant le processus de mise à niveau. | Après l’installation initiale, vous pouvez supprimer les données d’administration de la configuration. |
| 2006 | validate-config | L’utilisateur administrateur n’a pas été créé, car l’adresse électronique de l’administrateur n’était pas définie | Après l’installation, vous pouvez créer manuellement un utilisateur administrateur : utilisez ssh pour vous connecter à votre environnement. Exécutez ensuite la commande `bin/magento admin:user:create` . |
| 2007 | validate-config | Mettre à jour la version php vers la version recommandée |  |
| 2008 | validate-config | La prise en charge de Solr est obsolète dans Adobe Commerce et Magento Open Source 2.1. |  |
| 2009 | validate-config | Solr n’est plus pris en charge par Adobe Commerce et Magento Open Source 2.2 ou version ultérieure. |  |
| 2010 | validate-config | Le service Elasticsearch est installé au niveau de la couche d’infrastructure, mais il n’est pas utilisé comme moteur de recherche. | Envisagez de supprimer le service Elasticsearch de la couche d’infrastructure pour optimiser l’utilisation des ressources. |
| 2011 | validate-config | La version du service Elasticsearch sur la couche d’infrastructure n’est pas compatible avec la version actuelle du module elasticsearch/elasticsearch, utilisée par votre application Adobe Commerce. |  |
| 2012 | validate-config | La configuration actuelle n’est pas compatible avec cette version d’Adobe Commerce |  |
| 2013 | validate-config | Options SCD ignorées car le processus de déploiement ne s’est pas exécuté lors de la phase de création |  |
| 2014 | validate-config | La configuration contient des variables ou des valeurs obsolètes |  |
| 2015 | validate-config | Configuration de l’environnement non valide |  |
| 2016 | validate-config | La configuration de type JSON ne peut pas être décodée |  |
| 2017 | validate-config | La configuration actuelle n’est pas compatible avec cette version d’Adobe Commerce |  |
| 2018 | validate-config | Certains services ont atteint leur fin de vie |  |
| 2019 | validate-config | L’option de configuration de la recherche MySQL est obsolète | Utilisez plutôt Elasticsearch. |
| 2029 | validate-config | La base de données fractionnée est obsolète dans Adobe Commerce et Magento Open Source 2.4.2 et sera supprimée dans la version 2.5. | Si vous utilisez une base de données fractionnée, vous devez commencer à planifier le retour à une base de données unique ou la migration vers cette base de données, ou utiliser une autre approche. |
| 2020 | install-update | Installation d&#39;Adobe Commerce terminée, mais le fichier de configuration `app/etc/env.php` était manquant ou vide. | Les données requises sont restaurées à partir des configurations d’environnement et du fichier .magento.env.yaml. |
| 2021 | install-update:db-connection | Pour les bases de données fractionnées, utiliser des connexions personnalisées |  |
| 2022 | install-update:db-connection | La configuration de la base de données que vous avez modifiée est incompatible avec la connexion esclave. |  |
| 2023 | install-update:split-db | L&#39;activation d&#39;une base de données fractionnée est ignorée. |  |
| 2024 | install-update:split-db | La variable SPLIT_DB ne contient pas la configuration pour les types de connexion partagés. |  |
| 2025 | install-update:split-db | Connexion esclave non définie. |  |
| 2026 | pre-deploy:restore-writable-dirs | Impossible de restaurer certaines données générées pendant la phase de création dans les répertoires montés | Pour plus d’informations, consultez la `cloud.log` . |
| 2027 | validate-config:image-mode-variable | Valeur de mode pour la variable d’environnement MAGE_MODE non prise en charge | Supprimez la variable d’environnement MAGE_MODE ou définissez sa valeur sur « production ». Adobe Commerce sur les infrastructures cloud prend uniquement en charge le mode « production ». |
| 2028 | stockage à distance | Impossible d&#39;activer le stockage distant. | Vérifiez les informations d&#39;identification de l&#39;enregistrement distant. |
| 2030 | validate-config | Les services Elasticsearch et OpenSearch sont tous deux installés au niveau de la couche d’infrastructure. Adobe Commerce et Magento Open Source 2.4.4 et versions ultérieures utilisent OpenSearch par défaut | Envisagez de supprimer le service Elasticsearch ou OpenSearch de la couche d’infrastructure pour optimiser l’utilisation des ressources. |

### Étape de post-déploiement

| Code d’erreur | Étape de post-déploiement | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 3001 | validate-config | La journalisation du débogage est activée dans Adobe Commerce. | Pour économiser de l’espace disque, n’activez pas la journalisation du débogage pour vos environnements de production. |
| 3002 | préchauffage | Impossible de récupérer les URL du magasin |  |
| 3003 | préchauffage | Impossible de récupérer l’URL du magasin |  |
| 3004 | sauvegarde | Impossible de créer des fichiers de sauvegarde |  |

### Général

| Code d’erreur | Étape générale | Description de l’erreur (titre) | Action suggérée |
| - | - | - | - |
| 4001 |  | Impossible d&#39;obtenir le nombre de processeurs système : |  |
