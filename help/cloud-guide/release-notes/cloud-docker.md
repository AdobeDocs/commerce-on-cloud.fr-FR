---
title: Package Cloud Docker
description: Consultez la liste des dernières améliorations apportées au package Cloud Docker.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: c1f5e663716d3c9390e8ffb8bb2da6c7c67996b0
workflow-type: tm+mt
source-wordcount: '3790'
ht-degree: 0%

---

# Package Cloud Docker

Le package [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) fournit des fonctionnalités et des images Docker pour déployer Adobe Commerce dans un environnement cloud local. Ces notes de mise à jour décrivent les dernières améliorations apportées à ce package, qui est un composant de [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

Le package de `magento/magento-cloud-docker` utilise la séquence de version suivante : `<major>.<minor>.<patch>`

Les notes de mise à jour incluent les éléments suivants :

- ![nouvelle icône](../../assets/new.svg) Nouvelles fonctionnalités
- ![icône de correctif](../../assets/fix.svg) Correctifs et améliorations

<!--Add release notes below-->

## v1.4.5 {#latest}

Date de publication : 8 octobre 2025

- ![nouvelle icône](../../assets/new.svg) **ActiveMQ** : ajout de la prise en charge d’ActiveMQ dans le docker cloud avec des tests fonctionnels.<!-- MCLOUD-13771 -->

## v1.4.4

Date de publication : 7 août 2025

- ![icône de correction](../../assets/fix.svg) **PHP 8.4**—Ajout de tests PHP 8.4.<!-- MCLOUD-13311 -->
- ![icône de correctif](../../assets/fix.svg) **extension FTP**-Correctif ajouté pour l’extension FTP.<!-- MCLOUD-13843 -->
- ![nouvelle icône](../../assets/new.svg) **Image Opensearch3**—Ajout de la prise en charge d’Opensearch3.<!-- MCLOUD-13766 -->
- ![nouvelle icône](../../assets/new.svg) **Tests Opensearch3**—Ajout de tests PHP 8.4 pour Opensearch3.<!-- MCLOUD-13768 -->
- ![nouvelle icône](../../assets/new.svg) **Valkey**—Ajout de la prise en charge de Valkey.<!-- MCLOUD-13558 -->

## v1.4.3

Date de publication : 3 juin 2025

- ![icône de correction](../../assets/fix.svg) **compatibilité améliorée avec la version 2.4.8**-bibliothèques tierces mises à jour pour une meilleure compatibilité avec la version 2.4.8<!-- MCLOUD-13707	 - -->

## v1.4.2

Date de publication : 7 avril 2025

- ![nouvelle icône](../../assets/new.svg) **PHP 8.4**—Ajout `php-cli` images 8.4 et `php-fpm` 8.4.


## v1.4.1

Date de publication : 6 février 2025

- ![nouvelle icône](../../assets/new.svg) **PHP 8.4**—Ajout de la prise en charge de PHP 8.4.


## v1.4.0

Date de publication : 7 octobre 2024

- ![icône de correction](../../assets/fix.svg) **code refactorisé** - Suppression de la prise en charge des anciennes versions de PHP (7.4, 7.3, 7.2) et des bibliothèques et images associées.

## v1.3.7

Date de publication : 8 avril 2024

- ![nouvelle icône](../../assets/new.svg) **PHP** — Ajout de la prise en charge des images PHP 8.3 et PHP 8.3.
- ![nouvelle icône](../../assets/new.svg) **Nginx** — Ajout de l&#39;image nginx v. 1.24.
- ![nouvelle icône](../../assets/new.svg) **Opensearch** - Ajout de l’image OpenSearch v. 2.12, 1.3.
- ![nouvelle icône](../../assets/new.svg) **Compositeur** - Mise à jour de la version du compositeur vers la version 2.2.23.

## v1.3.6

Date de publication : 31 juillet 2023

- ![nouvelle icône](../../assets/new.svg) **Ajout d’une nouvelle version de service**—OpenSearch 2.5.
- ![nouvelle icône](../../assets/new.svg) **Activer le cache du compositeur**—Vous pouvez désormais étendre la configuration Docker pour activer le cache effacé du compositeur lors du démarrage du conteneur Docker. Voir [Extension de la configuration Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) dans le guide _Cloud Docker pour Commerce_ .

## v1.3.5

Date de publication : 10 mars 2023

- ![new icon](../../assets/new.svg) **ionCube**—Ajout de l&#39;extension ionCube pour l&#39;image PHP 8.1.
- ![nouvelle icône](../../assets/new.svg) **Ajout de nouvelles versions de service**—OpenSearch 2.3 et 2.4, PHP 8.2, Varnish 7.1.1.
- ![nouvelle icône](../../assets/new.svg) **Prise en charge améliorée de PHP 8.2** - Correction de problèmes de compatibilité avec certaines versions de PHP 8.2.x pour la prise en charge de Commerce 2.4.6.
- ![icône de correction](../../assets/fix.svg) **problème du compositeur** : correction de problèmes qui se produisaient après la mise à jour de la version du compositeur dans les conteneurs Docker.

## v1.3.4

Date de publication : 27 octobre 2022

- ![nouvelle icône](../../assets/new.svg) **Ajout de nouvelles images de vernis**—Ajout d’images pour les versions 6.5, 7.0 et 7.1.<!-- MCLOUD-7879 --> de Varnish

## v1.3.3

Date de publication : 13 septembre 2022

- ![nouvelle icône](../../assets/new.svg) **Prise en charge d’Apple M1 (ARM64)**—Ajout de modifications aux images Docker pour activer la prise en charge de l’architecture Apple M1 (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![icône de correction](../../assets/fix.svg) **Mailhog** : correction d’un problème en raison duquel le service Mailhog ne captait pas les e-mails en mode développeur.<!-- MCLOUD-8643 -->
- ![icône de correction](../../assets/fix.svg) **init-docker.sh** : correction du programme de validation des versions de service dans le script `init-docker.sh`.<!-- MCLOUD-8765 -->

## v1.3.2

Date de publication : 31 mars 2022

- ![nouvelle icône](../../assets/new.svg) **Image Elasticsearch 7.10 ajoutée**<!-- MCLOUD-8548 -->

## v1.3.1

Date de publication : 10 mars 2022

- ![nouvelle icône](../../assets/new.svg) **Support PHP 8.1**—Ajout de la prise en charge de PHP 8.1.
- ![nouvelle icône](../../assets/new.svg) **OpenSearch**—Ajout d’images des versions 1.1 et 1.2 d’OpenSearch.
- ![nouvelle icône](../../assets/new.svg) **Compositeur 2.1**—Définissez le compositeur 2.1.x par défaut dans les images PHP 8.x.
- ![nouvelle icône](../../assets/new.svg) **améliorations des images PHP**—

   - Ajout d’images PHP 8.1
   - Mise à niveau de xDebug version 3.1.2
   - Mise à niveau de xmlrpc 1.0.0RC3

- ![icône de correction](../../assets/fix.svg) **Améliorations d’Elasticsearch et d’OpenSearch**—Améliorations d’Elasticsearch et d’OpenSearch Dockerfiles ; suppression de l’image Elasticsearch 5.2.
- ![fix icon](../../assets/fix.svg) **Sodium extension**—Activation de l&#39;extension `sodium` par défaut dans toutes les images PHP.
- ![icône de correction](../../assets/fix.svg) **volume de cache du compositeur** : chemin d’accès fixe pour que le volume de cache du compositeur ait les packages du compositeur mis en cache.
- ![icône de correction](../../assets/fix.svg) **Limitation de la mémoire dans NGINX** : limitation de la mémoire dans l’image NGINX.

## v1.3.0

Date de publication : 25 octobre 2021

- ![icône de correction](../../assets/fix.svg) **workflow Améliorer le mode Développeur**—Auparavant, vous deviez spécifier le mode dans les étapes de création et de déploiement. Désormais, l’option `--mode` de l’étape `build` détermine le mode de l’étape `deploy` ultérieure. Il n’est plus nécessaire de définir le mode après le déploiement. Voir [mode Développeur](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/).<!-- ACMP-1086 -->
- ![icône de correction](../../assets/fix.svg) **améliorations apportées au système de fichiers en lecture seule**—<!-- ACMP-1106 -->
   - Correction du problème de démarrage d&#39;un conteneur PHP pour la configuration du mail.
   - Peut utiliser des variables d’environnement dans des fichiers INI.
   - Assurez-vous que les points d’entrée PHP n’ont pas besoin d’une autorisation d’écriture.
- ![icône de correction](../../assets/fix.svg) **Mettre à jour le nœud**—Mettez à jour la version du nœud groupé ; lors de l’installation du nœud dans les images PHP-CLI, il utilise désormais la version LTS actuelle.<!-- ACMP-1539 -->
- ![icône de correction](../../assets/fix.svg) **Mettre à jour Symfony**—Mise à jour des dépendances de configuration Symfony pour les rendre compatibles avec Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## v1.2.4

Date de publication : 29 juillet 2021

- ![nouvelle icône](../../assets/new.svg) **Nouveau conteneur `Zookeeper`**—Ajout d&#39;un [conteneur Zookeeper](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container) pour gérer la configuration du fournisseur de verrou pour les projets qui ne sont pas déployés sur Adobe Commerce sur l&#39;infrastructure cloud.<!--MCLOUD-8000-->

- ![nouvelle icône](../../assets/new.svg) **Ajout de la prise en charge du compositeur 2.0.**—Ajout de la version 2.0 du compositeur au fichier de configuration du compositeur pour prendre en charge les mises à niveau à partir du compositeur 1.0, qui approche de la fin de vie.<!--MCLOUD-8003-->

## v1.2.3

Date de publication : 14 juin 2021

- ![nouvelle icône](../../assets/new.svg) **Ajout de PHP 8.0**—Mise à jour de PHP vers la version 8.0, vous permettant de profiter de toutes les nouvelles fonctionnalités et optimisations que PHP 8.0 inclut.<!--MCLOUD-7941-->
- ![nouvelle icône](../../assets/new.svg) **Mise à jour vers Varnish 6.6 et Elasticsearch 7.11.2**—Les liens ci-après fournissent des informations sur [Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) et Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![nouvelle icône](../../assets/new.svg) **Ajout de l&#39;extension `ioncube` pour l&#39;image PHP 7.4**—L&#39;extension `ioncube` a été rajoutée à l&#39;image PHP 7.4 après avoir été initialement exclue de la mise à niveau de PHP 7.3 vers PHP 7.4. *[Soumis par mattskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![nouvelle icône](../../assets/new.svg) **Ajout d’une option de synchronisation des fichiers :`manual-native`**—L’option de synchronisation des fichiers `manual-native` permet de contrôler manuellement la synchronisation, ce qui offre les meilleures performances pour les environnements macOS et Windows. Découvrez comment utiliser l’option `manual-native` en [mode Développeur](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) et [Synchronisation des données dans un environnement de développement Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options).<!--MCLOUD-7977-->
- ![nouvelle icône](../../assets/new.svg) **Suppression des suppressions de volume des commandes `up` et `down`**—L&#39;option `--volume` a été supprimée des commandes `bin/magento-docker up` et `bin/magento-docker down`, remplacée par la nouvelle commande `bin/magento-docker init` avec un avertissement de perte de données. Cette modification permet d’éviter la perte de données accidentelle. *[Présenté par joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![icône de correction](../../assets/fix.svg) **valeur `CN` mise à jour pour le certificat généré**—Suppression de la valeur `CN` codée en dur du fichier Docker. Cette valeur a créé une erreur de certificat (`NET::ERR_CERT_INVALID`) qui a provoqué l&#39;ignorance de l&#39;option `--host` de la commande `ece-docker build:compose`.<!--MCLOUD-7934-->

## v1.2.2

Date de publication : 20 avril 2021

- ![nouvelle icône](../../assets/new.svg) **Mise à jour des `host.docker.internal` pour qu’elles soient indépendantes de la plateforme**—Vous pouvez désormais créer les mêmes scripts Docker Compose pour Ubuntu, Windows et macOS. L’utilisation de Xdebug sur Ubuntu ne nécessite plus de variable d’environnement distincte. [Correctif soumis par Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![new icon](../../assets/new.svg) **Updated init-docker.sh** : ajout de l’objet `mounts` à la variable d’environnement `MAGENTO_CLOUD_APPLICATION`. [Correctif soumis par Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![nouvelle icône](../../assets/new.svg) **Mise à jour de init-docker.sh**—Mise à jour du script `init-docker.sh` avec les versions PHP 7.4 et Cloud Docker 1.2.1. [Correctif soumis par Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![nouvelle icône](../../assets/new.svg) **Sodium activé par défaut**—Activation de l&#39;extension PHP `sodium` par défaut dans les images PHP Docker.<!--MCLOUD-7548-->
- ![nouvelle icône](../../assets/new.svg) **`custom-registry`option**—Ajout d&#39;une option `--custom-registry` à `php ./vendor/bin/ece-docker build:compose` commande pour utiliser votre propre registre d&#39;images.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![nouvelle icône](../../assets/new.svg) **Suppression des anciennes versions d’Elasticsearch**—Suppression des versions 1.7 et 2.4 d’Elasticsearch des images Elasticsearch.<!--MCLOUD-7504-->
- ![nouvelle icône](../../assets/new.svg) **Génération automatique de certificats NGINX**—Suppression des certificats existants de l&#39;image NGINX. Les certificats NGINX sont désormais générés automatiquement à chaque nouveau déploiement pour une sécurité accrue.<!--MCLOUD-7396-->
- ![icône de correction](../../assets/fix.svg) **`opcache.validate_timestamps`** activé—Activation du paramètre PHP `opcache.validate_timestamps` par défaut en mode développeur. L’activation de ce paramètre a résolu le problème où les modifications apportées au système de fichiers n’étaient pas reconnues dans Docker.<!--MCLOUD-7466-->
- ![icône de correction](../../assets/fix.svg) **`build:custom:compose`** de correction—Correction de la commande `build:custom:compose` pour générer une erreur lorsque les fichiers ne peuvent pas être remplacés pendant le processus de création. Le renvoi d’une erreur empêche les situations dans lesquelles `docker-compose up` pourriez utiliser les mauvais fichiers.<!--MCLOUD-7457-->
- ![icône de correction](../../assets/fix.svg) **option de `--sync_engine="native"` fixe**—Correction du problème en raison duquel, en mode de production (`--mode="production"`), l’option de `--sync_engine="native"` ne créait aucune entrée pour les dossiers locaux dans le fichier `docker.composer.yml`.<!--MCLOUD-7254-->
- ![Icône de correction](../../assets/fix.svg) **Correction des erreurs de validation de version de service**—Ajout de versions de service pour [!DNL RabbitMQ], Elasticsearch et d’autres services à la propriété `type` dans la variable `MAGENTO_CLOUD_RELATIONSHIP`. L’ajout de ces versions à la variable `relationships` a corrigé les erreurs de validation qui se produisaient lors de la phase de déploiement.<!--MCLOUD-7572-->

## v1.2.1

Date de publication : 21 décembre 2020

- ![nouvelle icône](../../assets/new.svg) **Options de commande NGINX**—Ajout d&#39;options de commande de génération pour modifier le nombre de `worker_processes` NGINX et de `worker_connections` NGINX pour TLS et les services Web. Le paramètre `worker_process` conserve la possibilité de définir la valeur sur `auto`. Exemples : <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![nouvelle icône](../../assets/new.svg) **option de commande TLS**—Ajout de l&#39;option de commande build pour créer une configuration sans le service TLS. Exemple : <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![nouvelle icône](../../assets/new.svg) **consommation de mémoire NGINX** : réduction de la mémoire consommée par le processus NGINX pour TLS et les services Web<!--MCLOUD-7259-->.

- ![nouvelle icône](../../assets/new.svg) **Blackfire** : désactivation de l’extension PHP Blackfire par défaut dans l’image Cloud Docker.

- ![icône de correction](../../assets/fix.svg) **Conteneur PHP-FPM**—Correction du contrôle de l&#39;intégrité du conteneur PHP-FPM en modifiant le `WEB_PORT` de `80` à `8080`.<!--MCLOUD-7232-->

- ![Icône de correction](../../assets/fix.svg) **Nom de volume non valide**—Correction d’une erreur qui entraînait un nom de volume non valide en mode développeur.<!--MCLOUD-7442-->

- ![icône de correction](../../assets/fix.svg) **port amont NGINX**—Mise à jour de l&#39;image Docker NGINX 1.19 pour utiliser le port 8080 afin d&#39;éviter une boucle infinie. [Correctif soumis par Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

Date de publication : 9 novembre 2020

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des conteneurs—**

   - ![nouvelle icône](../../assets/new.svg) **Conteneur PHP-FPM**—Ajout de la prise en charge de l&#39;extension Gnupg PHP. [Correctif soumis par G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![icône de correction](../../assets/fix.svg) **Conteneur de base de données** : correction du contrôle de l’intégrité du conteneur de base de données en ajoutant le mot de passe de base de données requis à la commande de contrôle de l’intégrité<!--MCLOUD-7122-->.

   - ![nouvelle icône](../../assets/new.svg) **Conteneur Elasticsearch**

      - Ajout de la prise en charge d’Elasticsearch 7.9 pour la compatibilité avec les prochaines versions d’Adobe Commerce.<!--MCLOUD-7190-->

      - **Configuration du plug-in Elasticsearch**—Ajout de la prise en charge de l’utilisation des informations de configuration du plug-in Elasticsearch du fichier `services.yaml` pour générer le fichier `docker-compose.yaml` pour un environnement Cloud Docker pour Commerce. Voir [Plug-ins Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Prise en charge des modules externes Elasticsearch**—Ajout de la prise en charge des modules externes Elasticsearch suivants : `analysis-icu`, `analysis-phonetic`, `analysis-stempel` et `analysis-nori`. Les plug-ins `analysis-icu` et `analysis-phonetic` sont installés par défaut. Vous pouvez ajouter ou supprimer les modules externes `analysis-stempel` et `analysis-nori` selon vos besoins.<!--MCLOUD-2789-->

   - ![nouvelle icône](../../assets/new.svg) **conteneur CLI**

      - **Exécuter des commandes dans les conteneurs PHP Docker**—Vous pouvez désormais utiliser l&#39;interface de ligne de commande de Cloud Docker pour exécuter des commandes dans les conteneurs PHP dans votre environnement Docker sans avoir à installer PHP sur l&#39;hôte. Par exemple, la commande suivante crée la configuration : `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Voir [Cloud Docker CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli). [Correctif soumis par G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - Ajout du client OpenSSH aux conteneurs de l’interface de ligne de commande PHP. Désormais, vous pouvez utiliser le transfert ssh-agent pour le compositeur si le fichier `composer.json` contient des référentiels Git privés qui nécessitent un client ssh pour utiliser les commandes du compositeur.<!--MCLOUD-6008-->

   - ![Icône de correction](../../assets/fix.svg) **Conteneur TLS** : désormais, le [Conteneur TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) est basé sur l’image Docker `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` au lieu de l’image CentOS. Cette modification corrige les problèmes qui provoquaient des erreurs lors de l’envoi de requêtes HTTPS entre les conteneurs dans l’environnement Cloud Docker.<!--MCLOUD-6469-->

   - ![nouvelle icône](../../assets/new.svg) **Conteneur de test** : ajout d’un conteneur de test pour les tests d’application et ajout de l’option `--with-test` à la commande Docker `build:compose` pour créer le conteneur uniquement lors des tests dans l’environnement Docker. Voir [test d’application](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/).<!--MCLOUD-6394-->

   - ![nouvelle icône](../../assets/new.svg) **Conteneur FPM-XDEBUG**

      - ![nouvelle icône](../../assets/new.svg) **Configurer Xdebug sous Linux** : ajout de l’option `--set-docker-host` à la commande `ece-docker build:compose` pour configurer la valeur `host.docker.internal` dans le conteneur Xdebug . Cette option est requise pour utiliser Xdebug sur les systèmes Linux. Voir [Configuration de Xdebug pour Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-6430-->

      - ![icône de correction](../../assets/fix.svg) correction de la configuration de la variable Xdebug pour le point d’entrée Docker afin de résoudre les erreurs `uninitialized "with_xdebug" variable` dans les journaux. [Correctif soumis par Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![nouvelle icône](../../assets/new.svg) **modifications apportées à la configuration Docker**

   - **Configuration MailHog** : vous pouvez maintenant utiliser les options de commande `ece-docker build:compose` suivantes pour désactiver MailHog et spécifier les ports : `--no-mailhog`, `--mailhog-http-port` et `--mailhog-smtp-port`. Voir [Configurer l’e-mail](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Pour Cloud Docker pour Commerce 1.2.0 et versions ultérieures, Adobe fournit désormais des images Docker pour chaque version de correctif, et le générateur de configuration Docker crée la configuration Docker avec une version de correctif spécifiée au lieu d’utiliser la dernière. Auparavant, le générateur de configuration Docker créait la configuration à l’aide de la dernière version de correctif, ce qui pouvait interrompre Cloud Docker pour les environnements Commerce créés à l’aide d’une version antérieure.<!--MCLOUD-7093-->

   - **Spécifier des images et des versions personnalisées dans la configuration personnalisée de Cloud Docker**—Mise à jour de la commande `build:custom:compose` avec des options pour spécifier des images et des versions personnalisées lors de la génération d’un fichier de configuration de composition Docker personnalisée (`docker-compose.yaml`). Voir [Créer une configuration Docker Compose personnalisée](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/). <!--MCLOUD-7089-->

   - Mise à jour de la configuration de l’hôte Docker afin d’exposer le port 443 pour permettre l’accès à Adobe Commerce (`https://magento2.docker`) à partir de tous les conteneurs de l’interface de ligne de commande. Vous pouvez modifier le port par défaut en ajoutant l’option `--tls-port` lorsque vous générez le fichier de configuration Docker.<!--MCLOUD-6806-->

- ![icône de correction](../../assets/fix.svg) correction d’un problème en raison duquel la version de Cloud Docker pour Commerce échouait si le fichier `app/etc/env.php` existait.<!--MCLOUD-6732-->

- ![icône de correction](../../assets/fix.svg) Mise à jour de la configuration de build pour remplacer les volumes nommés par des volumes standard afin d’éviter des problèmes lors du déploiement de Cloud Docker pour Commerce sous Linux ou du sous-système Windows pour Linux (WSL2).<!--MCLOUD-6732-->

- ![icône de correctif](../../assets/fix.svg) Mise à jour de Cloud Docker pour les tests fonctionnels Commerce afin de prendre en charge Composer 2.0.<!--MCLOUD-7183-->

## v1.1.2

Date de publication : 9 septembre 2020

- ![nouvelle icône](../../assets/new.svg) Ajout de la prise en charge d’Elasticsearch 7.7<!--MCLOUD-6219-->

## v1.1.1

Date de publication : 5 août 2020

- ![icône de correction](../../assets/fix.svg) **configuration d’e-mail mise à jour**—Mise à jour de la configuration par défaut de Cloud Docker pour Commerce afin de prendre en charge le service MailHog au lieu d’utiliser SendMail. Voir [Configurer l’e-mail](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-5624-->

- ![icône de correction](../../assets/fix.svg) Restauration de la bibliothèque PS à la configuration de l’environnement Cloud Docker pour corriger les erreurs `ps:  command not found`. <!--MCLOUD-6621-->

- ![icône de correction](../../assets/fix.svg) Mise à jour de la configuration par défaut de Cloud Docker pour Commerce afin de supprimer le montage automatique du point d’entrée de la base de données et des volumes MariaDB pour corriger `Cannot create container for service db` erreurs qui peuvent se produire lors du démarrage de votre environnement Cloud Docker.

  Vous pouvez maintenant configurer l’environnement Cloud Docker pour monter les répertoires de la base de données en ajoutant les options suivantes à la commande `ece-docker build:compose` : `--with-entry-point` et `with-mariadb-conf`. Voir [Options de configuration du service](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![nouvelle icône](../../assets/new.svg) **mises à jour des commandes de l’interface de ligne de commande**

| Action | Commande |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Ajoutez un point d’entrée au conteneur de base de données pour restaurer la base de données à partir de la sauvegarde | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Ajouter un volume de configuration MariaDB | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

Date de publication : 25 juin 2020

- ![nouvelle icône](../../assets/new.svg) **Ajout de la prise en charge de la solution de performances de base de données partagée**—Vous pouvez désormais configurer et déployer un magasin à l’aide de la solution de performances de base de données partagée dans l’environnement Cloud Docker.<!--MCLOUD-3740-->

- ![nouvelle icône](../../assets/new.svg) **Prise en charge du déploiement d’Adobe Commerce et de Magento Open Source**—Vous pouvez désormais utiliser Cloud Docker pour Commerce afin de déployer un environnement de développement local pour les projets qui ne sont pas hébergés sur Adobe Commerce sur une infrastructure cloud.<!--MCLOUD-5667-->

- ![nouvelle icône](../../assets/new.svg) prise en charge de **Blackfire.io**—Ajout de la prise en charge de l’utilisation de l’extension [Blackfire.io](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/) pour les tests de performance automatisés. [Correctif soumis par Adarsh Manickam de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des conteneurs**

   - **Vernis** : le vernis est désormais le cache par défaut lorsque vous déployez Adobe Commerce dans un environnement Cloud Docker à l’aide d’une version prise en charge du modèle d’application cloud. Voir [Conteneur de vernis](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container).<!--MCLOUD-2634-->

   - Ajout de l’option `--no-varnish` permettant d’ignorer l’installation du service Varnish lorsque vous générez le fichier de configuration Cloud Docker.<!--MCLOUD-2634-->

   - ![nouvelle icône](../../assets/new.svg) **Base de données**

      - Ajout de la prise en charge de la base de données MySQL. Vous pouvez désormais configurer l’environnement Cloud Docker avec MariaDB ou MySQL. Voir [Options de configuration du service](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - Ajout de la possibilité de définir les paramètres d’incrémentation et de décalage pour la réplication de la base de données lorsque vous générez le fichier de composition Docker. Voir [Conteneurs de services](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![nouvelle icône](../../assets/new.svg) **PHP-FPM**

      - Ajout de la prise en charge de PHP 7.4. [Correctif soumis par Mohanela Murugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - Possibilité de copier un fichier `php.ini` dans le répertoire racine du projet dans l&#39;environnement Cloud Docker et d&#39;appliquer des paramètres PHP personnalisés aux conteneurs PHP-FPM et CLI. Voir [Personnaliser les paramètres PHP](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings). [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - Ajout d’un contrôle d’intégrité du conteneur. [Correctif soumis par Visanth Sampath de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![icône de correction](../../assets/fix.svg) **Node.js** : mise à jour de la version par défaut de Node.js de la version 8 à la version 10 pour améliorer la sécurité. Node.js version 8 est obsolète et n’est plus mis à jour avec des correctifs de bugs ou de sécurité. [Correctif soumis par Mohan Elamurugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![nouvelle icône](../../assets/new.svg) **Elasticsearch**

      - Ajout de la prise en charge d’Elasticsearch 6.8, 7.2, 7.5 et 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Ajout de la possibilité de personnaliser la configuration du conteneur [Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) lorsque vous générez le fichier de configuration de composition Docker.<!--MCLOUD-3059-->

      - Ajout de l’option `--no-es` aux options de configuration du service pour générer le fichier de configuration Docker Compose. Utilisez cette option pour ignorer l’installation du conteneur Elasticsearch et utiliser plutôt la recherche MySQL. Cette option est prise en charge uniquement pour Adobe Commerce versions 2.3.5 et antérieures.<!--MCLOUD-3766-->

   - ![nouvelle icône](../../assets/new.svg) **Conteneur FPM-XDEBUG**—Ajout d’une option de configuration du service pour installer et configurer Xdebug pour le débogage de PHP dans votre environnement Cloud Docker. Voir [Configurer Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-4098-->

- ![nouvelle icône](../../assets/new.svg) **modifications apportées à la configuration Docker**

   - Ajout de contrôles d’intégrité pour les conteneurs de services PHP-FPM, Redis, Elasticsearch et MySQL Docker.<!--MCLOUD-3335 and MCLOUD-5856-->

   - Modification du mode de synchronisation de fichiers par défaut en `native` en mode Développeur.<!--MCLOUD-3890 -->

   - Ajout d’informations de version à l’image du conteneur de services Docker générique lors de la génération du fichier `docker-compose.yml`.<!--MCLOUD-3878-->

   - Amélioration de la capacité à gérer les réponses volumineuses du conteneur PHP-FPM en amont en augmentant la valeur `fastcgi_buffers` pour le serveur Nginx.<!--MCLOUD-5980-->

   - Amélioration des performances de la synchronisation des fichiers mutagènes en ajoutant une deuxième session de synchronisation pour synchroniser les fichiers du répertoire `vendor`. Cette modification empêche le blocage du mutagène pendant le processus de synchronisation des fichiers. [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![nouvelle icône](../../assets/new.svg) **mises à jour des commandes de l’interface de ligne de commande**

| Action | Commande |
| -------- | --------------- |
| Effacer le cache Redis | `bin/magento-docker flush-redis` |
| Effacer le cache de vernis | `bin/magento-docker flush-varnish` |
| Ignorer l&#39;installation par défaut du vernis | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Personnaliser les options d’Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Supprimer la configuration Elasticsearch](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Configuration du conteneur de base de données avec MySQL version 5.6 ou 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Spécifier une URL de base personnalisée | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Ajouter un conteneur pour la configuration Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![icône de correction](../../assets/fix.svg) Correction de la configuration de la synchronisation des fichiers mutagènes pour empêcher mutagène de créer des sessions obsolètes. [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![icône de correction](../../assets/fix.svg) Correction d’un problème de configuration qui provoquait des erreurs de syntaxe dans le journal de composition Docker lors du démarrage du conteneur PHP-FPM. [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![icône de correction](../../assets/fix.svg) correction des erreurs de conflit de volume qui se produisaient parfois lors de l’utilisation de plusieurs environnements Docker. [Correctif soumis par G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![icône de correction](../../assets/fix.svg) correction d’un problème en raison duquel la commande `ece-docker build:compose` échouait si la configuration incluait Blackfire.io. [Correctif soumis par G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![icône de correction](../../assets/fix.svg) Mise à jour de la configuration de l’image de l’interface de ligne de commande PHP pour éviter les erreurs de mémoire insuffisante qui se produisaient lors de l’installation de plusieurs packages à l’aide de Cloud Docker pour Commerce. [Correctif soumis par Mohan Elamurugan de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![icône de correction](../../assets/fix.svg) Ajout de la prise en charge de plusieurs utilisateurs MySQL dans l’environnement Cloud Docker. Dans les versions antérieures, l’opération `build:compose` échouait si le fichier `magento.app.yaml` spécifiait plusieurs utilisateurs de la base de données. [Correctif soumis par G Arvind de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![icône de correctif](../../assets/fix.svg) Suppression des `rsyslog` des conteneurs PHP Cloud Docker for Commerce pour résoudre les problèmes de compatibilité qui provoquaient des notifications d’avertissement lors du déploiement. Cloud Docker n’utilise pas l’utilitaire rsyslog.<!--MCLOUD-6173-->

## v1.0.0

Date de publication : 5 février 2020

- ![nouvelle icône](../../assets/new.svg) **Création d’un package distinct pour diffuser`Cloud Docker for Commerce`**—Déplacement du code source pour diffuser Cloud Docker pour Commerce du référentiel `ece-tools` vers le [nouveau référentiel `magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) pour maintenir la qualité du code et fournir des versions indépendantes. Le nouveau package est une dépendance pour ECE-Tools v2002.1.0 et versions ultérieures.

  Lorsque vous mettez à jour ece-tools, vous mettez également à jour le package `magento/magento-cloud-docker` vers la version 1.0.0. Si vous avez utilisé Cloud Docker pour Commerce avec une version `ece-tools` antérieure (2002.0.x), passez en revue les [incompatibilités rétroactives](backward-incompatible-changes.md) et mettez à jour votre projet sous la forme de scripts, de commandes et de processus, si nécessaire.

- ![nouvelle icône](../../assets/new.svg) **Ajout du contrôle de version aux images Docker**—Vous devez maintenant mettre à jour le package `magento/magento-cloud-docker` pour obtenir les images mises à jour.<!--MAGECLOUD-4737-->

- ![nouvelle icône](../../assets/new.svg) **Mises à jour des conteneurs**—

   - ![nouvelle icône](../../assets/new.svg) **Conteneur PHP-FPM**—

      - ![nouvelle icône](../../assets/new.svg) **Ajout de la prise en charge de Node.js**—Mise à jour de l’image PHP-FPM pour prendre en charge le nœud , npm, et les fonctionnalités grunt-cli dans le conteneur PHP.<!--MAGECLOUD-3953-->

      - ![nouvelle icône](../../assets/new.svg) **Ajout de la prise en charge d’[ionCube](https://www.ioncube.com/)**—Mise à jour de la configuration Docker par défaut afin de prendre en charge ionCube dans l’environnement de développement Docker local.<!--MAGECLOUD-4354-->

   - ![nouvelle icône](../../assets/new.svg) **Conteneur web**—

      - ![nouvelle icône](../../assets/new.svg) **Personnaliser la configuration NGINX**—Ajout de la possibilité de monter un fichier `nginx.conf` personnalisé dans l’environnement Cloud Docker pour Commerce. Voir [Conteneur web](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container).<!--MAGECLOUD-4204-->

      - ![nouvelle icône](../../assets/new.svg) **Certificats NGINX générés automatiquement**—Le fichier de configuration Docker inclut désormais la configuration permettant de générer automatiquement des certificats NGINX pour le conteneur Web.<!--MAGECLOUD-4258-->

   - ![nouvelle icône](../../assets/new.svg) **Nouveau conteneur Selenium**—Ajout d’un [conteneur Selenium](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container) pour prendre en charge les tests d’application Adobe Commerce à l’aide de la structure de test fonctionnel de Magento (MFTF).<!--MAGECLOUD-4040-->

   - ![nouvelle icône](../../assets/new.svg) prise en charge de la version **[!DNL RabbitMQ]**—Mise à jour de la configuration du conteneur [!DNL RabbitMQ] pour prendre en charge [!DNL RabbitMQ] version 3.8.<!--MAGECLOUD-4674-->

   - ![icône de correction](../../assets/fix.svg) **Conteneur de base de données persistante** : le volume de base de données `magento-db: /var/lib/mysql` persiste désormais après l’arrêt et la suppression de la configuration Docker et est restauré lorsque vous redémarrez la configuration Docker. Vous devez maintenant supprimer manuellement le volume de la base de données. Voir [Conteneurs de base de données].<!--MAGECLOUD-3978-->

   - ![nouvelle icône](../../assets/new.svg) **conteneur TLS**—

      - ![nouvelle icône](../../assets/new.svg) **Mise à jour de l’image de base du conteneur pour utiliser l’image officielle**—L’image [Conteneur Cloud TLS](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) est désormais basée sur l’image officielle `debian:jessie` Docker.—<!--MAGECLOUD-4163-->

      - ![nouvelle icône](../../assets/new.svg) **Ajout de la prise en charge du [Proxy de terminaison du TLS en livres]**—Le fichier de configuration [Pound](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) ajoute les variables ENV suivantes pour personnaliser la configuration Docker pour le conteneur TLS :

         - **`TimeOut`** : permet de définir la valeur du délai d&#39;expiration du délai de premier octet (TTFB). La valeur par défaut est de 300 secondes.

         - **`RewriteLocation`** : détermine si le proxy de la livre réécrit l&#39;emplacement sur l&#39;URL de la requête par défaut. La valeur par défaut est `0` pour empêcher la réécriture d’interrompre les redirections vers des sites web externes tels qu’un site SSO externe. [Correctif soumis par Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![nouvelle icône](../../assets/new.svg) Augmentation de la valeur du délai d’expiration dans la configuration du conteneur TLS de 15 à 300 secondes. [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![nouvelle icône](../../assets/new.svg) **Conteneur de vernis**—

      - ![nouvelle icône](../../assets/new.svg) **Mise à jour de l’image de base du conteneur pour utiliser l’image officielle**—Le [conteneur de vernis cloud](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) est désormais basé sur l’image officielle `centos` Docker.<!--MAGECLOUD-4163-->

      - ![nouvelle icône](../../assets/new.svg) **Amélioration de la configuration du délai d’expiration par défaut**-Ajout de la `.first_byte_timeout` et de la configuration `.between_bytes_timeout` au conteneur de vernis. Les deux valeurs de délai d’expiration par défaut sont `300s` (5 minutes). [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![icône de correction](../../assets/fix.svg) **Ignorer le vernis pendant les sessions Xdebug**—Mise à jour de la configuration du conteneur de vernis pour renvoyer le `pass` sur les requêtes reçues lorsque Xdebug est activé. Dans les versions précédentes, vous ne pouviez pas utiliser Xdebug si l’environnement Docker incluait Varnish. [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![nouvelle icône](../../assets/new.svg) **modifications apportées à la configuration Docker**—

   - ![nouvelle icône](../../assets/new.svg) **Gérer les montages et les volumes pour votre projet**—Ajout de la possibilité de gérer les montages et les volumes lors du lancement d’un environnement Docker pour le développement local. Voir [ Partage de données de projet ].<!--MAGECLOUD-3248-->

   - ![nouvelle icône](../../assets/new.svg) **Prise en charge du mode pont réseau**—Ajout de la prise en charge du mode pont réseau pour activer les connexions entre les conteneurs Docker sur le réseau local.<!--MAGECLOUD-4165-->

   - ![nouvelle icône](../../assets/new.svg) **Conteneur Cron désactivé par défaut** : pour améliorer les performances, le conteneur Cron n’est plus configuré par défaut lorsque vous créez l’environnement Docker. Vous pouvez utiliser l’option `--with-cron` sur la commande de création Docker pour ajouter un conteneur Cron à votre environnement. Voir [Gestion des tâches cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/).<!--MAGECLOUD-5181-->

   - ![nouvelle icône](../../assets/new.svg) **Arrêtez de synchroniser les fichiers de sauvegarde volumineux**—Ajout des fichiers d’archive et de vidage de base de données (ZIP, SQL, GZ et BZ2) à la liste d’exclusion dans les fichiers `dist/docker-sync.yml` et `dist/mutagen.sh`. La synchronisation de fichiers volumineux (> 1 Go) peut entraîner une période d’inactivité et les fichiers de sauvegarde ne nécessitent normalement pas de synchronisation, car vous pouvez les régénérer.<!--MAGECLOUD-3979-->

- ![nouvelle icône](../../assets/new.svg) **modifications de commande**—

   - ![icône de correction](../../assets/fix.svg) renommez le fichier `./bin/docker` en `./bin/magento-docker` pour résoudre un problème qui entraînait l’interruption de certains environnements Docker, car le fichier `./bin/docker` remplace les fichiers binaires Docker existants. Il s’agit d’une [modification rétrocompatible](backward-incompatible-changes.md) qui nécessite des mises à jour de vos scripts et commandes<!-- MAGECLOUD-4038 -->.

   - ![nouvelle icône](../../assets/new.svg) **Ajout d’une option de configuration du service pour exposer le port de base de données à l’hôte**—Utilisez l’option `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` pour exposer le port de base de données à l’hôte lors de la création du fichier `docker-compose.yml` : `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![nouvelle icône](../../assets/new.svg) **Nouvelle commande de post-déploiement** : auparavant, les hooks de post-déploiement définis dans le fichier `.magento.app.yaml` s’exécutaient automatiquement après le déploiement d’Adobe Commerce dans un conteneur Cloud Docker à l’aide de la commande `cloud-deploy`. Vous devez à présent émettre une commande `cloud-post-deploy` distincte pour exécuter les hooks de post-déploiement après le déploiement. Consultez les instructions de lancement mises à jour pour le mode [développeur](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) et [production](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/).<!--MAGECLOUD-3996-->

   - ![nouvelle icône](../../assets/new.svg) Ajout de l’option `--rm` pour `./bin/magento-docker` les commandes des conteneurs de création et de déploiement. Le conteneur est supprimé une fois la tâche terminée.<!--MAGECLOUD-4205-->

   - ![nouvelle icône](../../assets/new.svg) **Mises à jour de `build:compose` commande**—

      - ![nouvelle icône](../../assets/new.svg) Ajout de l’option `--sync-engine="native"` à la commande `docker-build` pour désactiver la synchronisation des fichiers lorsque vous générez le fichier de configuration Docker Compose en mode développeur. Utilisez cette option lors du développement sur des systèmes Linux, qui ne nécessitent pas de synchronisation de fichiers pour le développement local de Docker. Voir [ Synchronisation des données dans l’environnement Docker ](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![nouvelle icône](../../assets/new.svg) Modification du paramètre de synchronisation de fichiers par défaut de `docker-sync` à `native`. [Correctif soumis par Mathew Beane de Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![nouvelle icône](../../assets/new.svg) **Améliorations de la validation**—

   - ![nouvelle icône](../../assets/new.svg) Ajout d’une validation au processus de déploiement pour les environnements de développement Docker locaux afin de vérifier que la configuration de l’environnement Cloud inclut la clé de chiffrement requise pour déchiffrer la base de données. Désormais, un message d’erreur s’affiche dans le journal si la configuration de l’environnement ne spécifie aucune valeur pour la clé de chiffrement.<!--MAGECLOUD-4423-->

   - ![nouvelle icône](../../assets/new.svg) Ajout d’un contrôle de l’intégrité du conteneur au service Elasticsearch pour s’assurer que le service est prêt avant de poursuivre le traitement de la génération et du déploiement. Si le contrôle de l’intégrité renvoie une erreur, le conteneur redémarre automatiquement.<!--MAGECLOUD-4456-->
