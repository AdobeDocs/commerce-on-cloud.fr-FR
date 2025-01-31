---
title: Projet de mise à niveau pour l'utilisation des outils de la CEE
description: Découvrez comment mettre à niveau votre projet Adobe Commerce sur l'infrastructure cloud pour utiliser le package ECE-Tools et tirer parti des derniers correctifs et fonctionnalités.
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Projet de mise à niveau pour utiliser le module ECE-Tools

L’Adobe a abandonné les packages `magento/magento-cloud-configuration` et `magento/ece-patches` au profit du package `ece-tools`, ce qui simplifie de nombreux processus cloud. Si vous utilisez un ancien projet d’infrastructure Adobe Commerce on cloud qui ne contient _pas_ le package `ece-tools`, vous devez effectuer une opération manuelle unique _mise à niveau_ sur votre projet.

>[!WARNING]
>
>Si votre projet contient le package `ece-tools`, vous pouvez ignorer la mise à niveau suivante. Pour vérifier, récupérez la version [!DNL Commerce] à l’aide de la commande `php vendor/bin/ece-tools -V` dans le répertoire racine de votre projet local.

Ce processus de mise à niveau du projet nécessite que vous mettiez à jour la contrainte de version `magento/magento-cloud-metapackage` dans le fichier `composer.json` du répertoire racine. Cette contrainte permet de mettre à jour Adobe Commerce sur les métapaquets d’infrastructure cloud (y compris la suppression des packages obsolètes) sans mettre à niveau votre version actuelle d’Adobe Commerce.

{{upgrade-tip}}

## Supprimer les packages obsolètes

Avant d’effectuer une mise à niveau pour utiliser le package `ece-tools`, recherchez les packages obsolètes suivants dans le fichier `composer.lock` :

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Mettre à jour le métapaquet

Chaque version d’Adobe Commerce nécessite une contrainte différente basée sur les éléments suivants :

```
>=current_version <next_version
```

- Par `current_version`, spécifiez la version d’Adobe Commerce à installer.
- Par `next_version`, spécifiez la version de correctif suivante après la valeur spécifiée dans `current_version`.

Si vous souhaitez installer Adobe Commerce `2.3.5-p2`, définissez `current_version` sur `2.3.5` et le `next_version` sur `2.3.6`. La contrainte `">=2.3.5 <2.3.6"` installe le dernier package disponible pour la version 2.3.5.

Vous pouvez toujours trouver la dernière contrainte de métapaquet dans le modèle de [`magento-cloud`](https://github.com/magento/magento-cloud/blob/master/composer.json).

L’exemple suivant place une contrainte pour le métapaquet Adobe Commerce sur l’infrastructure cloud sur toute version supérieure ou égale à la version actuelle 2.4.7 et inférieure à la version suivante 2.4.8 :

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Mettre à niveau le projet

Pour mettre à niveau votre projet afin d’utiliser le package `ece-tools`, vous devez mettre à jour le métapaquet et les propriétés des points d’extension `.magento.app.yaml`, puis effectuer une mise à jour du compositeur.

**Pour mettre à niveau le projet afin d&#39;utiliser ece-tools** :

1. Mettez à jour la contrainte de version `magento/magento-cloud-metapackage` dans le fichier `composer.json`.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Mettez à jour le métapaquet .

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Modifiez les commandes de hook dans le fichier `magento.app.yaml`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Recherchez et supprimez les [packages obsolètes](#remove-deprecated-packages). Les packages obsolètes peuvent empêcher une mise à niveau réussie.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Il peut être nécessaire de mettre à jour le package `ece-tools`.

   ```bash
   composer update magento/ece-tools
   ```

1. Ajoutez et validez les modifications de code. Dans cet exemple, les fichiers suivants ont été mis à jour :

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Envoyez vos modifications de code au serveur distant et fusionnez cette branche avec la branche `integration`.

   ```bash
   git push origin <branch-name>
   ```
