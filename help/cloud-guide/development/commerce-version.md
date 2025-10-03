---
title: Mettre à niveau la version de Commerce
description: Découvrez comment mettre à niveau la version d’Adobe Commerce dans l’environnement de l’infrastructure cloud.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 7f9aac358effdf200b59678098e6a1635612301b
workflow-type: tm+mt
source-wordcount: '898'
ht-degree: 0%

---

# Mettre à niveau la version de Commerce

Vous pouvez mettre à niveau la base de code Adobe Commerce vers une version plus récente. Avant de mettre à niveau l’environnement, consultez la section [ Configuration requise ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) dans le guide _Installation_ pour connaître la configuration requise pour la dernière version du logiciel.

Selon le type d’environnement (Développement, Évaluation ou Production), vos tâches de mise à niveau peuvent inclure les éléments suivants :

- Mettez à jour le fichier `.magento/services.yaml` avec les nouvelles versions de MariaDB (MySQL), OpenSearch, RabbitMQ et Redis pour garantir la compatibilité avec les nouvelles versions d’Adobe Commerce.
- Mettez à jour le fichier `.magento.app.yaml` avec de nouveaux paramètres pour les hooks et les variables d’environnement.
- Mettez à niveau les extensions tierces vers la dernière version prise en charge.

{{upgrade-tip}}

{{pro-update-service}}

## Fichiers de configuration

Avant de mettre à niveau l’application, vous devez mettre à jour les fichiers de configuration de votre projet afin de tenir compte des modifications apportées aux paramètres de configuration par défaut d’Adobe Commerce sur l’infrastructure cloud ou l’application. Les dernières valeurs par défaut se trouvent dans le référentiel GitHub [magento-cloud](https://github.com/magento/magento-cloud).

### composer.json

Avant la mise à niveau, vérifiez toujours que les dépendances du fichier `composer.json` sont compatibles avec la version Adobe Commerce.

Pour mettre à jour le fichier `composer.json` pour Adobe Commerce version 2.4.4 et ultérieure **

1. Ajoutez les `allow-plugins` suivantes à la section `config` :

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Ajoutez le module externe suivant à la section `require` :

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Ajoutez le composant suivant à la section `extra:component_paths` :

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Enregistrez le fichier. Ne validez pas et n’envoyez pas encore de modifications à votre branche.

1. Poursuivez le processus de mise à niveau.

## Sauvegarde de l’environnement

Nous vous recommandons de créer une sauvegarde de l’instance avant une mise à niveau. Suivez les étapes ci-après pour sauvegarder vos environnements d’intégration, d’évaluation et de production.

**Pour sauvegarder la base de données et le code de votre environnement d’intégration** :

1. Créez une sauvegarde locale de la base distante.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >La commande `magento-cloud db:dump` exécute la commande [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) avec l&#39;indicateur `--single-transaction`, qui vous permet de sauvegarder votre base de données sans verrouiller les tables.

1. Sauvegardez le code et le média.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Vous pouvez éventuellement omettre `[--media]` si vous disposez d’un grand nombre de fichiers statiques qui se trouvent déjà dans le contrôle de code source.

**Pour sauvegarder la base de données de votre environnement d’évaluation ou de production avant le déploiement** :

1. Utilisez SSH pour vous connecter à l’environnement distant.

1. Créez une [image mémoire de la base de données](../storage/database-dump.md). Pour choisir un répertoire cible pour l’image mémoire de la base de données, utilisez l’option `--dump-directory`.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   L’opération de vidage crée un fichier d’archive `dump-<timestamp>.sql.gz` dans votre répertoire de projet distant. Voir [Sauvegarde de la base de données](../storage/database-dump.md).

## Mise à niveau de l’application

Consultez les informations [versions de service](../services/services-yaml.md#service-versions) pour connaître les dernières exigences en matière de version logicielle avant de mettre à niveau votre application.

**Pour mettre à niveau la version de l’application** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Définissez la [contrainte de version](overview.md#cloud-metapackage) pour la version de mise à niveau cible. Cette étape n’est nécessaire que si la version cible se trouve en dehors de la contrainte existante.

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Vous devez utiliser la syntaxe de contrainte de version pour mettre à jour le package `ece-tools`. La contrainte de version se trouve dans le fichier `composer.json` correspondant à la version du modèle d&#39;application [modèle d&#39;application](https://github.com/magento/magento-cloud/blob/master/composer.json) que vous utilisez pour la mise à niveau.

1. Mettez à jour votre fichier `composer.json` avec la version de mise à niveau de Commerce principale.

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. Si vous utilisez le B2B, mettez à jour votre fichier `composer.json` avec la [version prise en charge](https://experienceleague.adobe.com/en/docs/commerce-operations/release/product-availability#adobe-authored-extensions) pour Commerce.

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. Mettez à jour les dépendances de projet.

   ```bash
   composer update
   ```

1. Examinez les correctifs actuellement appliqués :

   - Si des correctifs sont installés dans le répertoire `m2-hotfixes`, [envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) et contactez l’assistance Adobe Commerce pour vérifier quels correctifs peuvent toujours être appliqués à la nouvelle version. Supprimez le ou les correctifs non applicables du répertoire `m2-hotfixes`.

   - Si des [correctifs de qualité] sont appliqués dans le fichier `.magento.env.yaml`, vérifiez s’ils peuvent toujours être appliqués à la nouvelle version. Supprimez le ou les correctifs non applicables de la section `QUALITY_PATCHES` du fichier `.magento.env.yaml`.

   **Méthode 1** : [vérifiez les versions applicables dans les notes de mise à jour des correctifs de qualité](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Méthode 2** : [affichage des correctifs et de l’état disponibles](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Méthode 3** : [Rechercher des correctifs](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. Ajout, validation et modifications de code push.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` est nécessaire pour ajouter tous les fichiers modifiés au contrôle de code source en raison de la manière dont le compositeur marshale les packages de base. `composer install` et `composer update` marshalent les fichiers du package de base (`magento/magento2-base` et `magento/magento2-ee-base`) dans la racine du package.

   Les fichiers que le compositeur marshale appartiennent à la nouvelle version d’Adobe Commerce, afin de remplacer la version obsolète de ces mêmes fichiers. Actuellement, le marshalling est désactivé dans Adobe Commerce, vous devez donc ajouter les fichiers marshalés au contrôle de code source.

1. Attendez la fin du déploiement.

1. Vérifiez la mise à niveau dans votre environnement d’intégration, d’évaluation ou de production à l’aide de SSH pour vous connecter et vérifier la version.

   ```bash
   php bin/magento --version
   ```

### Mettre à niveau les extensions

Passez en revue vos pages d’extension et de module tiers sur Marketplace ou d’autres sites d’entreprise et vérifiez la prise en charge d’Adobe Commerce et d’Adobe Commerce sur l’infrastructure cloud. Si vous devez mettre à niveau des extensions et modules tiers, Adobe recommande de travailler dans une nouvelle branche d’intégration avec vos extensions désactivées.

**Pour vérifier et mettre à niveau vos extensions** :

1. Créez une branche sur votre station de travail locale.

1. Désactivez vos extensions selon vos besoins.

1. Lorsqu’elles sont disponibles, téléchargez les mises à niveau d’extension.

1. Installez la mise à niveau comme indiqué dans la documentation tierce.

1. Activez et testez l’extension.

1. Ajoutez, validez et envoyez les modifications de code à la télécommande.

1. Envoyez et testez dans votre environnement d’intégration.

1. Push vers l’environnement d’évaluation pour effectuer des tests dans un environnement de pré-production.

Adobe recommande vivement de mettre à niveau votre environnement de production _avant_ y compris les extensions mises à niveau dans le processus de lancement de votre site.

>[!NOTE]
>
>Lorsque vous mettez à niveau votre version de l’application, le processus de mise à niveau se met automatiquement à jour vers la dernière version du [module Fastly CDN](../cdn/fastly.md#fastly-cdn-module-for-magento-2).

## Résolution des problèmes de mise à niveau

Si la mise à niveau a échoué, vous recevez un message d’erreur dans le navigateur indiquant que vous ne pouvez pas accéder à votre storefront ou au panneau d’administration :

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**Pour résoudre l’erreur** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Ouvrez le fichier `./app/var/report/<error number>`.

1. [Examinez les journaux](../test/log-locations.md) et déterminez la source du problème.

1. Ajout, validation et modifications de code push.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
