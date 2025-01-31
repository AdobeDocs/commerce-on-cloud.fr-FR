---
title: Modifications non rétrocompatibles
description: Découvrez la rétrocompatibilité lors de la mise à niveau de projets cloud existants.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Modifications non rétrocompatibles

Les modifications non rétrocompatibles peuvent nécessiter l’ajustement de la configuration et des processus cloud pour les projets cloud existants lorsque vous effectuez une mise à niveau vers la dernière version du package `ece-tools` ou d’autres packages Cloud Tools Suite pour Commerce.

## Modifications apportées `ece-tools` package

Certaines fonctionnalités précédemment incluses dans le package `ece-tools` sont désormais fournies dans des packages distincts. Ces packages sont des dépendances de compositeur pour `ece-tools`, qui sont installées et mises à jour automatiquement lorsque vous installez ou mettez à jour ece-tools.

La nouvelle architecture ne doit pas affecter les processus d’installation ou de mise à jour. Cependant, il se peut que vous deviez modifier certains processus et syntaxes de commande lors de l’utilisation de votre projet d’infrastructure cloud Adobe Commerce. Pour plus d’informations, consultez les informations suivantes sur les modifications rétrocompatibles et les notes de mise à jour de la [suite d’outils cloud](cloud-tools-suite.md).

### Modifications des exigences de version de service

Nous avons modifié l&#39;exigence de version PHP minimale de 7.0.x à 7.1.x pour les projets Cloud qui utilisent `ece-tools` v2002.1.0 et versions ultérieures. Si la configuration de votre environnement spécifie PHP 7.0, mettez à jour la [configuration php](../application/php-settings.md) dans le fichier `.magento.app.yaml`.

>[!WARNING]
>
>En raison du changement d’exigence de version de PHP, `ece-tools` 2002.1.0 prend uniquement en charge Adobe Commerce sur les projets d’infrastructure cloud exécutant Adobe Commerce 2.1.15 ou une version ultérieure. Si votre projet utilise une version antérieure, vous devez effectuer la [mise à niveau](../development/commerce-version.md) avant d’effectuer la mise à niveau vers la version `ece-tools` 2002.1.0.

### Modifications de la configuration de l’environnement

Le tableau suivant fournit des informations sur les variables d’environnement et d’autres fichiers de configuration d’environnement qui ont été supprimés ou rendus obsolètes dans `ece-tools` v2002.1.0.

| Poste | Remplacement |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` variable | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` variable | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` variable | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` variable | Aucune. Désormais, la version crée toujours un lien symbolique vers le répertoire de contenu statique `pub/static`. |
| fichier `build_options.ini` | Utilisez le fichier [`.magento.env.yaml`](../application/configure-app-yaml.md) pour configurer les variables d’environnement afin de gérer les actions de création et de déploiement dans tous vos environnements.<p>Si vous créez un environnement Cloud qui inclut le fichier `build_options.ini`, la création échoue. |

### Modifications des commandes de l’interface de ligne de commande

Le tableau suivant résume les changements de commande de l&#39;interface de ligne de commande dans ECE-Tools v2002.1.0 qui pourraient nécessiter la mise à jour de commandes ou de scripts.

| Commande | Remplacement |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

Dans les versions antérieures de ECE-Tools, vous pouviez utiliser les commandes `m2-ece-build` et `m2-ece-deploy` pour configurer les crochets de déploiement dans le fichier `.magento.app.yaml`. Lorsque vous effectuez une mise à jour vers la version v2002.1.0, vérifiez la configuration `hooks` dans le fichier `.magento.app.yaml` pour les commandes obsolètes et remplacez-les si nécessaire.

## Modifications des correctifs cloud

- **Supprimer les correctifs téléchargés**-Le package `magento/magento-cloud-patches` regroupe tous les correctifs disponibles à partir de la page [téléchargements de logiciels](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) et les applique automatiquement lorsque vous effectuez un déploiement sur le cloud. Pour éviter les conflits de correctifs après la mise à niveau vers ECE-Tools 2002.1.0 ou une version ultérieure, supprimez tous les correctifs fournis par l&#39;Adobe que vous avez téléchargés et ajoutés manuellement à votre projet.

- **Mise à jour de la commande apply patches**-Nous avons déplacé la commande pour appliquer des correctifs du répertoire `vendor/bin/ece-tools` au répertoire `vendor/bin/ece-patches`. Si vous utilisez cette commande pour appliquer manuellement des correctifs, utilisez le nouveau chemin d&#39;accès.

  > Application manuelle de correctifs

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Modifications de Cloud Docker

- **La version PHP minimale requise est désormais PHP 7.1**-Si votre hôte Cloud Docker pour Commerce exécute une version antérieure, effectuez une mise à niveau vers PHP v7.1 ou une version ultérieure.

- **Modifications de la commande Cloud Docker for Commerce**-

   - **Mise à jour des commandes de Cloud Docker pour Commerce pour les opérations de build de Docker**-Nous avons déplacé les commandes de Cloud Docker pour Commerce du répertoire `vendor/bin/ece-tools` vers le répertoire `vendor/bin/ece-docker`. Mettez à jour vos scripts et commandes pour utiliser le nouveau chemin d’accès.

     Après la mise à niveau vers `ece-tools` 2002.1.0, utilisez la commande suivante pour afficher les commandes `ece-docker` disponibles.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Mise à jour des commandes Cloud docker-compose**-Nous avons renommé le chemin d’accès au fichier de commandes de `./bin/docker` à `./bin/magento-docker`. Mettez à jour vos scripts et commandes pour utiliser le nouveau chemin d’accès.

   - **Le conteneur Cron n’est plus inclus dans la configuration Docker par défaut**-Maintenant, vous devez ajouter l’option `--with-cron` à la commande `ece-docker build:compose` pour inclure le conteneur Cron dans la configuration de l’environnement Docker. Voir [Gestion des tâches cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/) dans le guide _Cloud Docker for Commerce_ .

     Les scripts qui généraient auparavant des conteneurs avec des tâches cron ne disposent plus du conteneur cron.

   - **Utilisation de conteneurs temporaires**-Dans les versions précédentes, les conteneurs créés par `bin/magento-docker` opérations de commande n’étaient pas supprimés. Vous pouviez donc les utiliser pour d’autres opérations. Désormais, les commandes `magento-docker` suppriment tous les conteneurs qu’elles créent une fois la commande terminée.

     Si vous souhaitez conserver un conteneur créé par une opération docker-compose, utilisez la commande `docker-compose run` au lieu de la commande `bin/magento-docker`.

   - **Exécution des hooks de post-déploiement**-La commande `cloud-deploy` n’exécute plus les hooks de post-déploiement. Utilisez la nouvelle commande `cloud-post-deploy` pour exécuter les hooks de post-déploiement après le déploiement. Mettez à jour vos scripts pour ajouter la commande permettant d’exécuter les hooks de post-déploiement.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     Si vous utilisez directement des commandes `docker-compose`, vous pouvez également exécuter la commande `docker-compose run deploy cloud-post-deploy` après la commande de déploiement.

- **Actualisation de la base de données**-Le conteneur Base de données est désormais stocké dans le volume Docker persistant `magento-db`. Lorsque vous actualisez l’environnement Docker, la base de données n’est plus automatiquement supprimée. Si nécessaire, utilisez l’une des commandes suivantes pour la supprimer manuellement.

   - Supprimez le conteneur `magento-db` :

     ```bash
     docker volume rm magento-db
     ```

   - Supprimez tous les volumes associés lors de l&#39;arrêt des conteneurs Docker :

     ```bash
     docker-compose down -v
     ```

- **Remplacer les paramètres de synchronisation de fichier pour les fichiers d’archive et de sauvegarde**-Les fichiers d’archive et de sauvegarde avec les extensions suivantes ne sont plus synchronisés lors de l’utilisation de docker-sync ou de mutagen : SQL, GZ, ZIP et BZ2. Vous pouvez remplacer la synchronisation de fichiers par défaut pour ces types de fichiers en renommant le fichier pour qu’il se termine par une extension différente. Par exemple : `synchronize-me.zip-backup`
