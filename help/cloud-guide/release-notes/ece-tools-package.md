---
title: Notes de mise à jour des outils de la CEE
description: Voir la liste des dernières améliorations apportées au progiciel ECE-Tools.
recommendations: noDisplay, catalog
last-substantial-update: 2024-05-27T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 70664897a10a59668fad74565c04b4ad72474736
workflow-type: tm+mt
source-wordcount: '3166'
ht-degree: 0%

---

# Notes de mise à jour des outils de la CEE

Le package [ece-tools](https://github.com/magento/ece-tools) est un ensemble de scripts et d’outils conçus pour gérer et déployer des projets Cloud. Ces notes de mise à jour décrivent les dernières améliorations apportées à ce package, qui fait partie de la [suite d’outils cloud pour Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Voir [Upgrade ECE-Tools](../dev-tools/update-package.md) pour plus d&#39;informations sur la mise à jour vers la dernière version du package `ece-tools`.

Le package de `ece-tools` utilise l’ordre de contrôle de version suivant : `200<major>.<minor>.<patch>`

Les notes de mise à jour incluent les éléments suivants :

- ![nouvelle icône](../../assets/new.svg) Nouvelles fonctionnalités
- ![icône de correctif](../../assets/fix.svg) Correctifs et améliorations

<!--Add release notes below-->

## v2002.2.5 {#latest}

Date de publication : 27 mai 2025

- ![icône de correction](../../assets/new.svg) **compatibilité Valkey étendue** compatibilité Valkey étendue dans Adobe Commerce.<!-- MCLOUD-13595	 - -->
- ![Icône de correction](../../assets/fix.svg) **Validateur RabbitMQ mis à jour**-Validateur mis à jour pour RabbitMQ.<!-- MCLOUD-13589	 - -->
- ![icône de correction](../../assets/fix.svg) **Mise à jour de la validation MariaDB**-Mise à jour de la validation ece-tools pour MariaDB 10.11.<!-- MCLOUD-13593	 - -->
- ![icône de correction](../../assets/fix.svg) **compatibilité étendue Opensearch2**-Opensearch2 compatible avec les dernières versions 2.4.4.<!-- MCLOUD-13710	 - -->

## v2002.2.4

Date de publication : 24 avril 2025

- ![icône de correction](../../assets/fix.svg) **Opensearch2 pour 2.4.4/2.4.5**—Correction d’un problème lié à la prise en charge des `opensearch2` dans les versions d’Adobe Commerce 2.4.4/2.4.5.<!-- MCLOUD-13607 -->

## v2002.2.3

Date de publication : 9 avril 2025

- ![Icône de correction](../../assets/fix.svg) **Correction du problème Valkey** Correction du problème lié à la configuration personnalisée Valkey.<!-- MCLOUD-13569	 - -->
- ![Icône de correction](../../assets/fix.svg) **Correction du programme de validation**-Correction du programme de validation pour RabbitMQ 4.0.<!-- MCLOUD-13560	 - -->

## v2002.2.2

Date de publication : 7 avril 2025

## v2002.2.2

Date de publication : 7 avril 2025

- ![nouvelle icône](../../assets/new.svg) **Valkey**—Ajout de la prise en charge d&#39;un nouveau service (Valkey), qui remplace Redis.&lt;!— MCLOUD-13455 —>
- ![icône de correction](../../assets/fix.svg) **Opensearch2 pour 2.4.4/2.4.5**—Ajout de la prise en charge de `opensearch2` dans les versions d’Adobe Commerce 2.4.4/2.4.5. &lt;!— MCLOUD-13493 —>

## v2002.2.1

Date de publication : 6 février 2024

- ![nouvelle icône](../../assets/new.svg) **PHP 8.4**—Ajout de la prise en charge de PHP 8.4.<!-- MCLOUD-13145     - -->
- ![icône de correction](../../assets/fix.svg) **Validateur pour Opensearch**-Correction du validateur qui générait un message trompeur sur la mauvaise version du service.&lt;!— MCLOUD-13184 —>


## v2002.2.0

Date de publication : 7 octobre 2024

- ![nouvelle icône](../../assets/new.svg) **MariaDB 11.4**-Ajout de la prise en charge de MariaDB 11.4.
- ![icône de correction](../../assets/fix.svg) **code refactorisé**-Suppression de la prise en charge des anciennes versions PHP 7.4, 7.3, 7.2 et des bibliothèques associées.<!-- MCLOUD-9278 -->
- ![icône de correction](../../assets/fix.svg) **mise à niveau de Monolog version**-prise en charge ajoutée de monolog 3.6.<!-- MCLOUD-12855 -->
- ![icône de correction](../../assets/fix.svg) **Validateur pour RabbitMQ, MariaDB et PHP**-Correction du validateur qui générait un message trompeur sur la mauvaise version du service.

## v2002.1.19

Date de publication : 21 mai 2024

- ![nouvelle icône](../../assets/new.svg) **Lua** : option ajoutée useLua pour CACHE_CONFIGURATION.
- ![icône de correctif](../../assets/fix.svg) **Validateur**—Validateurs mis à jour pour les nouvelles versions de Redis et RabbitMQ.

## v2002.1.18

Date de publication : 8 avril 2024

- ![nouvelle icône](../../assets/new.svg) **PHP** — Ajout de la prise en charge de PHP 8.3.
- ![icône de correction](../../assets/fix.svg) **programme de validation** - Programme de validation de fin de vie mis à jour.

## v2002.1.17

Date de publication : 16 janvier 2024

- ![icône de correction](../../assets/fix.svg) **Validator pour Elasticsearch et OpenSearch**—Correction du validateur qui générait un message trompeur pour installer un service de recherche lorsque LiveSearch était activé.<!-- MCLOUD-10167 -->
- ![icône de correction](../../assets/fix.svg) **avertissement de déploiement**—Correction d’un problème qui entraînait des avertissements de déploiement concernant les dossiers non vides.<!-- MCLOUD-8958 -->

## v2002.1.16

Date de publication : 16 octobre 2023

- ![nouvelle icône](../../assets/new.svg) **Variable d’environnement globale ENABLE_WEBHOOKS** : ajout de la variable globale [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) à utiliser avec les Webhooks Commerce pour se connecter à un point d’entrée externe, tel qu’une action d’exécution App Builder ou un système de gestion d’inventaire tiers.

## v2002.1.15

Date de publication : 31 juillet 2023

- ![icône de correction](../../assets/fix.svg) **Codes d’erreur**—Mise à jour du schéma de code d’erreur et du générateur de document de code d’erreur.
- ![Icône de correction](../../assets/fix.svg) **Validateur pour le modèle Redis personnalisé**-Mise à jour du validateur pour les modèles principaux Redis personnalisés. [Voir l’exemple de configuration du cache](../environment/variables-deploy.md#cache_configuration).
- ![icône de correction](../../assets/fix.svg) **Validateur pour RabbitMQ**-prise en charge ajoutée pour RabbitMQ 3.11
- ![Icône de correction](../../assets/fix.svg) **Correction du lien incorrect**-Correction du lien incorrect vers la documentation d’intégration dans le modèle d’e-mail de bienvenue.

## v2002.1.14

Date de publication : 10 mars 2023

- ![nouvelle icône](../../assets/new.svg) **PHP**—Ajout de la prise en charge de PHP 8.2.
- ![nouvelle icône](../../assets/new.svg) **Validateurs pour les services**—Mise à jour des validateurs pour Commerce 2.4.6 services requis : MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x et RabbitMQ 3.9.
- ![icône de correction](../../assets/fix.svg) **ece-tools db-dump** : correction d’un problème en raison duquel l’opération `db-dump` s’arrêtait prématurément.

## v2002.1.13

Date de publication : 27 octobre 2022

- ![nouvelle icône](../../assets/new.svg) **Ajout de la prise en charge de Adobe I/O Events pour Adobe Commerce**. Les développeurs d’extensions peuvent désormais utiliser le framework [Adobe I/O Events](https://developer.adobe.com/events/docs/) pour envoyer des informations d’événement Commerce depuis des instances Cloud à leurs applications écrites pour [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/). Adobe I/O Events pour Adobe Commerce est en version préliminaire pour les partenaires.<!-- CEXT-932 -->
- ![nouvelle icône](../../assets/new.svg) **Validateur pour la configuration du cache OP**—Ajout d&#39;un validateur pour vérifier la configuration du cache OPpour les chemins exclus.<!-- MCLOUD-9485 -->
- ![icône de correction](../../assets/fix.svg) **correction d’un problème lié à la configuration du cache de GraphQL**—ECE-Tools conserve désormais la valeur de `id_salt` de GraphQL dans `cache` configuration dans le fichier `app/etc/env.php`.<!-- MCLOUD-9486 -->

## v2002.1.12

Date de publication : 13 septembre 2022

- ![nouvelle icône](../../assets/new.svg) **Activer`synchronous_replication`**—ECE-Tools définit `synchronous_replication=>true` dans le fichier `app/etc/env.php` lorsque `MYSQL_USE_SLAVE_CONNECTION` est activé. Cette configuration n’affecte que Commerce 2.4.6+. Voir la description de la variable `MYSQL_USE_SLAVE_CONNECTION` dans la section [Déployer les variables](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![nouvelle icône](../../assets/new.svg) **OpenSearch** : ajout d’une fonctionnalité permettant de configurer et de définir le moteur de `opensearch` pour la prochaine version 2.4.6 d’Adobe Commerce. Voir [Configuration du service OpenSearch](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Date de publication : 4 août 2022

- ![icône de correction](../../assets/fix.svg) **ElasticSuite Validator et OpenSearch** - Correction du problème de validation de vérification d’intégrité ElasticSuite lors de l’installation d’OpenSearch.<!-- MCLOUD-8767 -->
- ![Icône de correction](../../assets/fix.svg) **Types de retour pour les commandes de déploiement**—Types de retour fixes pour les commandes de déploiement.<!-- AC-3208 -->
- ![icône de correction](../../assets/fix.svg) **[!DNL RabbitMQ]problème lié à la nouvelle installation de Commerce 2.4.5**—Correction d’[!DNL RabbitMQ] problème de blocage sur la nouvelle installation de Commerce 2.4.5<!-- MCLOUD-9059 -->

## v2002.1.10

Date de publication : 31 mars 2022

- ![icône de correction](../../assets/fix.svg) **Elasticsearch 7.10**—Mise à jour des validateurs pour la prise en charge de la version 7.10 d’Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Date de publication : 10 mars 2022

- ![nouvelle icône](../../assets/new.svg) **OpenSearch**—Ajout de la prise en charge d’OpenSearch pour Adobe Commerce versions 2.4.4, 2.4.3-p2 et 2.3.7-p3.<!-- MCLOUD-8296 -->
- ![nouvelle icône](../../assets/new.svg) **PHP**—Ajout de la prise en charge de PHP 8.1.
- ![icône de correction](../../assets/fix.svg) **symfony/process**—Ajout de la compatibilité avec symfony/process ^5.3.<!-- MCLOUD-8283 -->

- ![nouvelle icône](../../assets/new.svg) **Consommateur de plusieurs processus**—Ajout d&#39;une option `multiple_processes` afin que vous puissiez spécifier le nombre de processus à générer pour chaque client. Voir la description de la variable `CRON_CONSUMERS_RUNNER` dans la section [Déployer les variables](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![nouvelle icône](../../assets/new.svg) **Schéma OpenSearch et chemin d’accès hôte complet**—Ajout de la possibilité de configurer un schéma Elasticsearch et un chemin d’accès hôte complet.
- ![icône de correction](../../assets/fix.svg) **AWS S3** : modification de la méthode d’activation d’AWS S3.
- ![icône de correction](../../assets/fix.svg) **Corriger le lecteur driver_options**—Ajout de la lecture de la configuration driver_options pour la connexion à la base de données à partir du fichier `env.php` en `ece-tools` pour les validateurs.<!-- MCLOUD-8420 -->

## v2002.1.8

Date de publication : 25 octobre 2021

- ![nouvelle icône](../../assets/new.svg) **Autre emplacement de vidage**—Ajout de l&#39;option `--dump-directory` afin que vous puissiez choisir un répertoire cible pour un vidage de base de données. `/app/var/dump-main` est désormais le répertoire cible par défaut d’une image mémoire de base de données. Voir [Gestion des sauvegardes : vider votre base de données](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![icône de correction](../../assets/fix.svg) **Mettre à jour le monologue**—Mise à jour de la version minimale requise pour que le package `monolog` soit `^2.3`.<!-- ACMP-1263 -->
- ![icône de correction](../../assets/fix.svg) **Mettre à jour Symfony** : mise à jour des dépendances Symfony pour les rendre compatibles avec Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![Icône de correction](../../assets/fix.svg) **Chargement automatique de la fonctionnalité/résolution** : correction d’un problème lors du déploiement dans un environnement d’intégration et de l’affichage de l’erreur `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`. <!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Date de publication : 29 juillet 2021

**Mises à jour de la configuration**—

- ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge du compositeur 2.0.<!--MCLOUD-8003-->

- ![icône de correction](../../assets/fix.svg) **Mise à jour des exigences du compositeur pour`symphony/console`**—Mise à jour des exigences de version du `composer.json` ECE-Tools pour le package `symphony/console` afin de résoudre un problème qui a provoqué l&#39;échec des commandes `di:compile` avec l&#39;erreur suivante : `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![icône de correction](../../assets/fix.svg) Mise à jour des contrôles logiciels de fin de vie (`eol.yaml`) afin d’inclure Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Date de publication : 20 avril 2021

- ![nouvelle icône](../../assets/new.svg) **Informations d’identification d’authentification Redis**—Ajout de la possibilité de lire les informations d’identification d’autorisation Redis à partir de la propriété `relationships` pendant la phase de déploiement.<!--MCLOUD-7694-->

- ![nouvelle icône](../../assets/new.svg) **informations d’identification d’autorisation Elasticsearch**—Ajout de la possibilité de lire les informations d’identification d’autorisation Elasticsearch à partir de la propriété `relationships` pendant la phase de déploiement.<!--MCLOUD-7695-->

- ![nouvelle icône](../../assets/new.svg) **Service de stockage de session dédié**—Ajout de `redis-session` en tant que seconde option pour le stockage de session. Vous pouvez utiliser le service `redis-session` pour stocker les informations de session et utiliser le service `redis` pour le cache afin d’offrir de meilleures performances.<!--MCLOUD-7698-->

- ![nouvelle icône](../../assets/new.svg) **Messages SPLIT_DB obsolètes**—Ajout de messages d’avertissement et critiques du programme de validation pour l’option de `SPLIT_DB` obsolète d’Adobe Commerce 2.4.2 et sa suppression dans Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![icône de correction](../../assets/fix.svg) **version d’Elasticsearch à partir des relations**—Correction du validateur de service pour récupérer la version correcte d’Elasticsearch à partir des propriétés `relationships` dans Cloud Docker et les environnements d’intégration.<!--MCLOUD-7572-->

- ![icône de correction](../../assets/fix.svg) **validation flexible du port Redis** : Redis peut désormais valider le port dans une connexion de cache personnalisée à partir de l’URL `server`. Par exemple, vous pouvez ajouter votre numéro de port à l’URL de votre serveur comme suit : `server: 'tcp://rfs-store-simple-page-cache:26379'`. Cela permet d’éviter les erreurs de validation lorsque l’option `port` est manquante ou incorrecte.<!--MCLOUD-7722-->

- ![icône de correction](../../assets/fix.svg) **Mise à niveau vers Adobe Commerce 2.4.2**—Correction du problème qui obligeait les utilisateurs à exécuter manuellement `bin/magento setup:upgrade` pour que leurs sites soient opérationnels après la mise à niveau vers Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Date de publication : 1er février 2021

- ![nouvelle icône](../../assets/new.svg) **Stockage à distance** : ajout de la variable d’environnement `REMOTE_STORAGE` pour activer les projets cloud pour le stockage à distance de fichiers multimédias à l’aide d’un service de stockage, tel qu’AWS S3. Cette option de configuration fait partie du package ECE-Tools, mais n&#39;est pas prise en charge sur Adobe Commerce sur les infrastructures cloud.<!--MCLOUD-7153-->

- ![nouvelle icône](../../assets/new.svg) **Nouvelle commande de `cloud:config:validate`**—Ajout d’une `php vendor/bin/ece-tools cloud:config:validate` de commande pour valider la configuration `.magento.env.yaml` avant de pousser les modifications vers l’environnement cloud distant.<!--MCLOUD-7120-->

- ![nouvelle icône](../../assets/new.svg) **Vidage de l’opcache**—Ajout de la prise en charge de l’option `opcache.enable_cli` PHP pour vider l’opcache avant d’exécuter le hook de déploiement. Cette configuration réinitialise la configuration du cache pour s’assurer que les paramètres de configuration actuels sont appliqués à chaque déploiement.<!--MCLOUD-7015-->

- ![nouvelle icône](../../assets/new.svg) **Validation de la base de données Aurora**—Mise à jour de la validation du service de base de données afin qu&#39;elle soit compatible avec la base de données Aurora.<!--MCLOUD-7269-->

- ![nouvelle icône](../../assets/new.svg) **Nouvelle variable d’environnement SCD_NO_PARENT** : ajout de la variable d’environnement `SCD_NO_PARENT` (pour Adobe Commerce >=2.4.2) pour gérer la génération de contenu statique pour les thèmes parents<!--MCLOUD-7284-->.

- ![icône de correction](../../assets/fix.svg) **limites et commandes de mémoire**—Correction d&#39;un problème où les commandes de `php vendor/bin/ece-tools` ne fonctionnaient pas si la taille du fichier `cloud.log` dépassait la limite de mémoire PHP. Au lieu de lire l’intégralité du fichier `cloud.log` en mémoire, nous ne lisons désormais qu’un plus petit sous-ensemble de données à partir du fichier journal.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![icône de correction](../../assets/fix.svg) **Connexions à la base de données personnalisée**—Correction d&#39;un problème de configuration `.magento.env.yaml` en raison duquel les connexions à la base de données personnalisée définies pour `DATABASE_CONFIGURATION` n&#39;étaient pas utilisées. Les paramètres de connexion n’ont pas été ajoutés à `app/etc/env.php`.<!--MCLOUD-7426-->

- ![icône de correction](../../assets/fix.svg) **Journaux d’erreurs vides**—Correction d’un problème en raison duquel les déploiements échouaient si le `cloud.error.log` était vide.<!--MCLOUD-7296-->

- ![Icône de correction](../../assets/fix.svg) **Validation de MariaDB 10.3**—Correction de la validation de MariaDB 10.3 pour Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![Icône de correction](../../assets/fix.svg) **Cache:flush logging**—Amélioration des entrées de journal pour indiquer le début et la fin de l’étape de `cache:flush`.<!--MCLOUD-7503-->

## v2002.1.4

Date de publication : 19 novembre 2020

- ![icône de correction](../../assets/fix.svg) correction d’un problème qui entraînait un échec du déploiement lorsque le moteur de recherche spécifié dans la variable d’environnement `SEARCH_CONFIGURATION` est une valeur autre que `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Date de publication : 9 novembre 2020

**Mises à jour de l&#39;infrastructure**—

- ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge des outils ECE pour le répertoire `pub/static` en lecture seule lorsque le contenu statique est prêt à être déployé à l’étape de création.<!--MC-37699-->

- ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge d’Elasticsearch 7.9 et de Redis 6 pour la compatibilité avec les prochaines versions d’Adobe Commerce.<!--MCLOUD-7191-->

- ![icône de correction](../../assets/fix.svg) Mise à jour du `composer.json` ECE-Tools pour ajouter une dépendance requise pour l&#39;outil de correctifs de qualité. Ceci corrige une dépendance circulaire qui existait entre les paquets ECE-Tools et magento-cloud-patches.<!--MCLOUD-6910-->

**Validation et amélioration des logs**—

- ![nouvelle icône](../../assets/new.svg) Ajout d’une validation du moteur de recherche pour s’assurer que `elasticsearch` est défini pour Adobe Commerce sur les infrastructures cloud 2.4 et versions ultérieures. Si la validation échoue, le déploiement est arrêté avec un message d’erreur critique qui suggère des correctifs pour le problème. Voir [Erreurs critiques, étape de déploiement](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![nouvelle icône](../../assets/new.svg) Ajout d’une validation Elasticsearch pour vérifier la compatibilité entre la version du service Elasticsearch et la version Adobe Commerce.<!--MCLOUD-7193-->

- ![nouvelle icône](../../assets/new.svg) Mise à jour du message d’erreur de compatibilité d’Elasticsearch pour afficher les versions d’Elasticsearch compatibles avec le module Adobe Commerce Elasticsearch. Le message d’erreur indique désormais les versions Elasticsearch spécifiques à installer dans votre infrastructure cloud afin qu’elle soit compatible avec le module Elasticsearch utilisé par votre version d’Adobe Commerce. Voir [Erreurs d’avertissement, étape de déploiement](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![nouvelle icône](../../assets/new.svg) Ajout d’erreurs d’avertissement `2026` et `2027` pour les paramètres de variable d’environnement `MAGE_MODE` non valides. La seule valeur valide est `production`. Avant ce correctif, `MAGE_MODE` pouvait être défini sur `developer` sans erreur de déploiement, pour entraîner ultérieurement des erreurs lors de tentatives d’écriture dans des fichiers en lecture seule. Voir [Erreurs d’avertissement](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![Icône de correction](../../assets/fix.svg) Correction de la validation des services Redis, RabbitMQ et MySQL pour s’assurer que ces versions sont compatibles avec la version Adobe Commerce. Des versions valides de ces services sont désormais écrites dans le `cloud.log`.<!--MCLOUD-7098-->

- ![Icône de correction](../../assets/fix.svg) Mise à jour du `cloud.log` afin d’inclure la limite de requêtes simultanées pour l’envoi de requêtes pendant le préchauffage du cache. Cette valeur est configurée dans la variable post-déploiement [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency). <!--MCLOUD-5563-->

**Mises à jour des commandes de l’interface de ligne de commande**—

- ![nouvelle icône](../../assets/new.svg) Ajout de commandes d’interface de ligne de commande (`cloud:config:create` et `cloud:config:update`) pour créer et mettre à jour le fichier `.magento.env.yaml` avec une configuration pouvant inclure une ou plusieurs variables de version, de déploiement et de post-déploiement. Voir [Créer un fichier de configuration à partir de l’interface de ligne de commande](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Mises à jour des variables d’environnement**—

- ![nouvelle icône](../../assets/new.svg) Ajout de la variable de build [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload). La définition de la variable sur `true` empêche l’application d’exécuter la commande `composer dump-autoload` lors de l’installation de Cloud Docker pour Commerce. La variable ne concerne que Cloud Docker pour les conteneurs Commerce dotés de systèmes de fichiers accessibles en écriture (créés pour les tests et le développement à l’aide de `./vendor/bin/ece-docker build:compose --with-test`). Avec de telles installations, l’omission de la commande `composer dump-autoload` empêche les erreurs lors de l’exécution d’autres commandes qui tentent d’accéder à des fichiers d’un répertoire `generated` supprimé.<!--MCLOUD-6939-->

## v2002.1.2

Date de publication : 5 août 2020

**Validation et amélioration des logs**—

- ![nouvelle icône](../../assets/new.svg) Ajout du fichier `schema.error.yaml` qui comprend toutes les notifications d’erreur et d’avertissement qui peuvent se produire pendant les processus de création, de déploiement et de post-déploiement, ainsi que des suggestions pour résoudre les erreurs. Les informations contenues dans ce fichier sont également disponibles dans le _Guide cloud pour Commerce_. Voir [Référence du message d’erreur pour ece-tools](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![nouvelle icône](../../assets/new.svg) Modification des entrées du journal d’erreurs cloud (`/var/log/cloud.error.log`) au format JSON afin de faciliter l’analyse du journal par programmation.<!--MCLOUD-5879-->

- ![nouvelle icône](../../assets/new.svg) Ajout de vérifications d’erreur supplémentaires pour le traitement de création, de déploiement et de post-déploiement, et amélioration des vérifications existantes :

   - Code d’erreur 2026 : échec de la restauration de certaines données générées pendant la phase de création dans les répertoires montés

   - Code d’erreur 3004 : impossible de créer des fichiers de sauvegarde

   - Code d&#39;erreur 102 : ajout de vérifications supplémentaires pour les problèmes qui se produisent lorsque le fichier `env.php` n&#39;est pas accessible en écriture <!--MCLOUD-6221-->

- ![nouvelle icône](../../assets/new.svg) Ajout de la variable d’environnement **QUALITY_PATCHES** pour spécifier un ou plusieurs correctifs de qualité à appliquer pendant le processus de déploiement. Voir [Créer des variables](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Date de publication : 25 juin 2020

- ![nouvelle icône](../../assets/new.svg) **Mises à jour de l’infrastructure**—

   - ![nouvelle icône](../../assets/new.svg) **Améliorations de la journalisation** : amélioration de la fonctionnalité de suivi des journaux en attribuant des codes de sortie aux erreurs de déploiement critiques et en exposant les codes de sortie dans les notifications de messages d’erreur et les événements de journal. Voir [Référence du message d’erreur pour ece-tools](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![nouvelle icône](../../assets/new.svg) Amélioration du processus pour les vidages de base de données (`vendor/bin/ece-tools db-dump`) et mise à jour des messages de journal afin de clarifier que l’opération de vidage de base de données fait passer l’application en mode de maintenance, arrête les processus de file d’attente des clients et désactive les tâches cron avant le début du vidage.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![Icône de correction](../../assets/fix.svg) Correction d’un problème afin de s’assurer que l’URL du projet est correctement mise à jour lors du déploiement dans les environnements d’évaluation et de production. Désormais, `ece-tools` utilise l’URL de l’itinéraire avec l’attribut `primary:true` défini dans la configuration de l’itinéraire du projet. Voir [Déploiement de variables](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![icône de correctif](../../assets/fix.svg) Mise à jour du workflow de scénario de build `generate.xml` pour l’application de correctifs. Des correctifs doivent être appliqués plus tôt pour mettre à jour Adobe Commerce afin de résoudre tous les problèmes qui peuvent entraîner l’échec des étapes `di:compile` et `module:refresh`.<!--MCLOUD-5941-->

   - ![icône de correction](../../assets/fix.svg) Correction d’un problème dans le processus d’installation qui renvoyait incorrectement l’erreur `Crypt key missing`. La valeur `crypt/key` est générée automatiquement lors de l&#39;installation.<!--MCLOUD-6120-->

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des services**—

   - ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge de PHP 7.4 et MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des variables d’environnement**—

   - ![nouvelle icône](../../assets/new.svg) Ajout de la variable **SCD_USE_BALER** pour activer le module Baler pour le regroupement JavaScript pendant le processus de création d’Adobe Commerce sur l’infrastructure cloud. Voir la description de la variable dans la section [Variables de build](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![nouvelle icône](../../assets/new.svg) Ajout de la variable d’environnement **REDIS_BACKEND** pour configurer le modèle principal Redis pour le cache Redis pour Adobe Commerce version 2.3.5 ou ultérieure. Voir la description de la variable dans la [déploiement des variables](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![nouvelle icône](../../assets/new.svg) **mises à jour des commandes CLI**—

   - ![nouvelle icône](../../assets/new.svg) Mise à jour des commandes d’interface de ligne de commande suivantes avec une option pour une journalisation plus détaillée :

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     Le niveau de journalisation pour chaque appel est déterminé par la configuration de la variable [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) dans le fichier `.magento.env.yaml`.<!--MCLOUD-3503-->

- ![nouvelle icône](../../assets/new.svg) **Améliorations de la validation**—

   - ![nouvelle icône](../../assets/new.svg) **Contrôles de compatibilité Elasticsearch 7.x**—Mise à jour de la validation Elasticsearch pour les contrôles de compatibilité logicielle Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![nouvelle icône](../../assets/new.svg) **Mise à jour de la version du service et contrôles de validation de fin de vie**—Mise à jour de la validation pour vérifier les versions de service installées par rapport aux exigences d’Adobe Commerce 2.4.<!--MCLOUD-6144-->.

   - ![Icône de correction](../../assets/fix.svg) Correction d’un problème de validation en raison duquel le message d’avertissement post-déploiement suivant s’affichait uniquement si la configuration du hook `post-deploy` était absente du fichier `.magento.app.yaml` :

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![nouvelle icône](../../assets/new.svg) **Ajout de la validation pour les dépendances de Zend Framework**—Ajout de la validation des dépendances du compositeur pour Zend Framework qui a migré vers le projet Laminas. Si les dépendances requises sont manquantes, le message d’erreur suivant s’affiche pendant le processus de création.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Voir [Vérifier les dépendances de Zend Framework](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![nouvelle icône](../../assets/new.svg) **Ajout d’une validation pour `env.php` fichier et les données**—Ajout de vérifications pour le fichier et les données `env.php` pendant le processus d’installation et de mise à niveau.<!--MCLOUD-5991-->

      - Si le fichier `env.php` est absent de l’installation et que la valeur `crypt/key` n’est pas spécifiée dans le fichier `.magento.app.yaml`, le déploiement échoue avec la notification suivante :

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Si l’installation n’inclut pas le fichier `env.php` ou si la configuration ne contient qu’un seul type de cache, la commande `cron:enable` s’exécute pendant le processus de mise à niveau pour restaurer le fichier avec tous les `cache_types`. La notification suivante est ajoutée au journal :

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Date de publication : 6 février 2020

- ![nouvelle icône](../../assets/new.svg) **Mises à jour de l’infrastructure**—

   - ![nouvelle icône](../../assets/new.svg) **Ajout d’un package distinct pour Cloud Docker for Commerce**—Découplage le package Docker du package `ece-tools` pour maintenir la qualité du code et fournir des versions indépendantes. Les mises à jour et les correctifs liés à `ece-tools` sont gérés à partir du référentiel GitHub [magento-cloud-docker](https://github.com/magento/magento-cloud-docker).<!--MAGECLOUD-2927-->

   - ![nouvelle icône](../../assets/new.svg) **Fonctionnalités de correction mises à jour**—Déplacement de la fonctionnalité de correction du package ECE-Tools vers un package [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) distinct. Pendant le déploiement, `ece-tools` utilise le nouveau package pour appliquer des correctifs. Voir [Notes de mise à jour des correctifs cloud](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![nouvelle icône](../../assets/new.svg) **Dépendances du compositeur mises à jour**—Mise à jour du fichier `composer.json` pour Adobe Commerce sur l&#39;infrastructure cloud avec une dépendance pour le package `magento/magento-cloud-docker`. Désormais, `ece-tools` inclut des dépendances pour tous les packages du [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Ces packages sont installés et mis à jour automatiquement lorsque vous installez ou mettez à jour `ece-tools`.

- ![nouvelle icône](../../assets/new.svg) **prise en charge des déploiements basés sur des scénarios**—<!--MAGECLOUD-4101-->

   - ![nouvelle icône](../../assets/new.svg) Vous pouvez désormais personnaliser les processus de création, de déploiement et de post-déploiement à l’aide de fichiers de configuration XML pour remplacer ou personnaliser la configuration par défaut.

   - ![nouvelle icône](../../assets/new.svg) **Modification de la configuration `hooks` dans`.magento.app.yaml`**—Nous avons mis à jour le format de configuration `hooks` pour prendre en charge les déploiements basés sur des scénarios. Le format hérité de la version 2002.0.x de ECE-Tools antérieure est toujours pris en charge. Cependant, vous devez effectuer une mise à jour vers le nouveau format pour utiliser la fonctionnalité de déploiement basé sur un scénario. Voir [Déploiements basés sur des scénarios](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Avant de mettre à jour vers la version 2002.1.0 de ECE-Tools, passez en revue le [rétrospectif   modifications incompatibles ](backward-incompatible-changes.md) pour en savoir plus sur les modifications qui pourraient nécessiter que vous   mettez à jour Adobe Commerce sur la configuration ou les processus de projet d’infrastructure cloud.

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des services**—

   - ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge de PHP 7.3.<!--MAGECLOUD-4022-->

   - ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge de RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![nouvelle icône](../../assets/new.svg) Ajout d’une validation pour vérifier les versions de service installées par rapport à la date de fin de vie de chaque service. Désormais, les clients reçoivent une notification si une version de service est dans les trois mois suivant la date de fin de vie, ainsi qu’un avertissement si la date de fin de vie est dans le passé.<!--MAGECLOUD-4076-->

   - ![icône de correction](../../assets/fix.svg) correction d’un problème de configuration d’Elasticsearch afin de s’assurer que les paramètres Elasticsearch appropriés sont configurés dans tous les environnements.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consultez la section [Versions des services](../services/services-yaml.md#service-versions) pour obtenir la liste des services utilisés dans Adobe Commerce sur les infrastructures cloud et leur compatibilité de version avec le modèle cloud.

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des variables d’environnement**—

   - ![nouvelle icône](../../assets/new.svg) Extension de la fonctionnalité de la variable d’environnement `WARM_UP_PAGES` afin de prendre en charge le préchargement du cache pour des pages de produits spécifiques. Consultez la définition développée dans la rubrique [variables post-déploiement](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-4444-->

   - ![nouvelle icône](../../assets/new.svg) Ajout de la variable d’environnement `ERROR_REPORT_DIR_NESTING_LEVEL` pour simplifier la gestion des données du rapport d’erreur dans le répertoire `<magento_root>/var/report/`. Voir la description de la variable dans la rubrique [créer des variables](../environment/variables-build.md#error_report_dir_nesting_level).

   - ![icône de correction](../../assets/fix.svg) Supprimez les variables d’environnement `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`, `DO_DEPLOY_STATIC_CONTENT` et `STATIC_CONTENT_SYMLINK`. Voir [Modifications rétrocompatibles](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![icône de correction](../../assets/fix.svg) correction d’un problème dans le processus de configuration d’Elastic Suite afin que la configuration par défaut soit remplacée comme prévu lorsque vous configurez la variable de déploiement `ELASTICSUITE_CONFIGURATION` sans l’option `_merge`.<!--MAGECLOUD-4388-->

- ![nouvelle icône](../../assets/new.svg) **mises à jour des commandes CLI**—

   - ![nouvelle icône](../../assets/new.svg) **Nouvelle commande cron**—Vous pouvez désormais gérer manuellement le traitement cron dans votre environnement Adobe Commerce sur l&#39;infrastructure cloud à l&#39;aide des commandes `cron:disable` et `cron:enable`. Utilisez la commande disable pour arrêter tous les processus cron actifs et désactiver toutes les tâches cron. Utilisez la commande d’activation pour réactiver les tâches cron lorsque vous êtes prêt. Voir [ Désactiver les tâches cron ](../application/crons-property.md#disable-cron-jobs).

   - ![nouvelle icône](../../assets/new.svg) **Amélioration des rapports d&#39;erreur**—Ajout d&#39;une meilleure journalisation pour les défaillances de commande CLI qui se produisent pendant le traitement ECE-Tools.<!--MAGECLOUD-4849-->

   - ![nouvelle icône](../../assets/new.svg) **Supprimer les commandes de build obsolètes**— Suppression des commandes de build suivantes : `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump`, et renommage des commandes de `ece-tools docker` en `ece-docker`. Voir [Modifications rétrocompatibles](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![nouvelle icône](../../assets/new.svg) Suppression du fichier `build_options.ini` obsolète et ajout de la validation pour faire échouer la version si le fichier existe. Utilisez le fichier [.magento.env.yaml](../environment/configure-env-yaml.md) pour configurer les options de génération.

- ![icône de correction](../../assets/fix.svg) correction d’un problème en raison duquel le processus de création échouait lorsque le fichier `config.php` était vide.<!--MAGECLOUD-4127-->

## 2002.0.23

Date de publication : 27 février 2020

- ![icône de correction](../../assets/fix.svg) correction d’un problème de compatibilité avec les versions 2002.0.x de `ece-tools` qui empêchait la génération de contenu statique à la demande de se terminer correctement en mode de production.

## Anciennes versions

Voir l’[archive des notes de mise à jour](cloud-release-archive.md) pour les versions 2002.0.22 et antérieures.
