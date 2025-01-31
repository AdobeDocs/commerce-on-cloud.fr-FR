---
title: Thème personnalisé
description: Découvrez comment installer un thème personnalisé avec Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Themes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Thème personnalisé

Vous pouvez installer un ou plusieurs thèmes à utiliser pour un ou tous vos magasins et sites dans votre projet. Les thèmes incluent plusieurs fichiers statiques, notamment des images, des polices, des fichiers CSS, JavaScript, PHP, etc. afin de concevoir entièrement vos magasins. Vous pouvez ajouter le thème en extrayant son code dans le système de fichiers ou à l’aide du compositeur.

## Installer un thème manuellement

Pour installer un thème manuellement, vous devez disposer du code du thème dans une archive compressée ou dans une structure de répertoires similaire à ce qui suit :

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**Pour installer un thème manuellement** :

1. Copiez le code du thème sous `<Project root dir>/app/design/frontend` pour un thème de storefront ou `<Project root dir>/app/design/adminhtml` pour un thème Admin. Vérifiez que le répertoire de niveau supérieur est `<VendorName>` ; sinon, le thème ne s’installe pas correctement.

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. Confirmez que le thème a été copié au bon endroit.

   * Thème de Storefront : `ls <project-root>/app/design/frontend`
   * Thème administrateur : `ls <project-root>/app/design/adminhtml`

   Voici un exemple :

   ExempleThème Adobe Commerce

1. Ajoutez et validez des fichiers.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. Envoyez les fichiers à votre branche.

   ```bash
   git push origin <branch name>
   ```

1. Attendez la fin du déploiement.
1. Connectez-vous à l’administrateur.
1. Cliquez sur **Contenu** > Conception > **Thèmes**.

   Le thème s’affiche dans le volet de droite.

## Installer un thème à l’aide du compositeur

L’installation d’un thème à l’aide du compositeur est identique à l’installation de toute autre extension utilisant le compositeur. Voir [Installation, gestion et mise à niveau des modules](extensions.md) pour plus d’informations.

Pour installer un thème à l’aide du compositeur :

1. Achetez le thème auprès de Commerce Marketplace.
1. Obtenez le nom du compositeur du thème.
1. Dans votre répertoire racine Adobe Commerce, saisissez la commande :

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   Par exemple,

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. Attendez que les dépendances soient mises à jour.
1. Saisissez les commandes suivantes :

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. Connectez-vous à l’administrateur.
1. Cliquez sur **Contenu** > Conception > **Thèmes**.

   Le thème s’affiche dans le volet de droite.

## Plusieurs thèmes

Lorsque vous utilisez plusieurs thèmes, par exemple différents thèmes par paramètre régional, passez en revue la variable d’environnement `SCD_MATRIX` pour personnaliser le déploiement du thème. Voir les étapes [création](../environment/variables-build.md#scd_matrix) ou [déploiement](../environment/variables-deploy.md#scd_matrix) dans la [configuration de l’environnement](../environment/configure-env-yaml.md).
