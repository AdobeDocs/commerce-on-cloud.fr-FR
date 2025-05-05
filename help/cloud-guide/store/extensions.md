---
title: Gestion des extensions
description: Découvrez comment installer et gérer des extensions dans Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Gestion des extensions

Vous pouvez étendre les fonctionnalités de votre application Adobe Commerce en ajoutant une extension à partir du Commerce Marketplace [&#128279;](https://marketplace.magento.com). Par exemple, vous pouvez ajouter un thème pour modifier l’aspect de votre storefront ou vous pouvez ajouter un package de langue pour localiser votre storefront et votre administrateur.

>[!NOTE]
>
>Pour éviter tout problème d’installation, tous les achats sur Marketplace doivent être effectués à l’aide du compte (MAGEID) propriétaire du projet cloud.

## Nom du compositeur d’une extension

Bien que cette section explique comment obtenir le nom et la version du compositeur d’une extension à partir du Commerce Marketplace, vous pouvez retrouver le nom et la version du module _any_ dans le fichier Compositeur du module. Ouvrez le fichier `composer.json` dans un éditeur de texte et notez les valeurs `"name"` et `"version"`.

**Pour obtenir le nom du compositeur d’un module à partir du Commerce Marketplace** :

1. Connectez-vous au Commerce Marketplace [&#128279;](https://marketplace.magento.com) avec le nom d&#39;utilisateur et le mot de passe que vous avez utilisés pour acheter le composant.

1. Dans le coin supérieur droit, cliquez sur votre nom d’utilisateur et sélectionnez **Mon profil**.

   ![Accéder à votre compte Marketplace](../../assets/marketplace/my-profile.png)

1. Sur la page _Mon compte_, cliquez sur **Mes achats**.

   ![Historique des achats du marché](../../assets/marketplace/my-purchases.png)

1. Sur la page _Mes achats_, sélectionnez le module que vous avez acheté, puis cliquez sur **Détails techniques**.

1. Cliquez sur **Copier** pour copier le [!UICONTROL Component name] dans le presse-papiers.

1. Ouvrez un éditeur de texte, collez le nom du composant et ajoutez un caractère deux-points (`:`).

1. Dans **Détails techniques**, cliquez sur **Copier** pour copier le [!UICONTROL Component version] dans le presse-papiers.

1. Dans l’éditeur de texte, ajoutez le numéro de version au nom du composant après les deux-points. Par exemple :

   ```text
   extension-name/magento2:1.0.1
   ```

## Installation d’une extension

Adobe recommande de travailler dans une branche de développement lors de l’ajout d’une extension à votre implémentation. Lors de l’installation d’une extension, le nom de l’extension (`<VendorName>_<ComponentName>`) est automatiquement inséré dans le fichier [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html). Il n’est pas nécessaire de modifier directement le fichier.

**Pour installer une extension** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Créez ou extrayez une branche de développement. Voir [embranchement](../development/cli-branches.md).

1. À l’aide du nom et de la version du compositeur, ajoutez l’extension à la section `require` du fichier `composer.json`.

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. Mettez à jour les dépendances du projet.

   ```bash
   composer update
   ```

1. Ajout, validation et modifications de code push.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >Lors de l’installation d’une extension, vous devez inclure le fichier `composer.lock` lorsque vous poussez des modifications de code vers l’environnement distant. La commande `composer install` lit le fichier `composer.lock` pour activer les dépendances définies dans l’environnement distant.

1. Une fois la création et le déploiement terminés, utilisez un SSH pour vous connecter à l’environnement distant et vérifier l’extension installée.

   ```bash
   bin/magento module:status <extension-name>
   ```

   Un nom d’extension utilise le format suivant : `<VendorName>_<ComponentName>`.

   Exemple de réponse :

   ```
   Module is enabled
   ```

   Si vous rencontrez des erreurs de déploiement, consultez [échec du déploiement de l’extension](../deploy/recover-failed-deployment.md).

## Gestion des extensions

Lorsque vous ajoutez une extension à l’aide du compositeur, le processus de déploiement active automatiquement l’extension. Si l’extension est déjà installée, vous pouvez l’activer ou la désactiver à l’aide de l’interface de ligne de commande. Lors de la gestion des extensions, utilisez le format suivant : `<VendorName>_<ComponentName>`

N’activez ou ne désactivez jamais une extension lorsque vous êtes connecté aux environnements distants.

**Pour activer ou désactiver une extension** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Activer ou désactiver un module. La commande `module` met à jour le fichier `config.php` avec l’état demandé du module .

   >Activez un module.

   ```bash
   bin/magento module:enable <module-name>
   ```

   >Désactivez un module.

   ```bash
   bin/magento module:disable <module-name>
   ```

1. Si vous avez activé un module, utilisez `ece-tools` pour actualiser la configuration.

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. Vérification de l&#39;état d&#39;un module.

   ```bash
   bin/magento module:status <module-name>
   ```

1. Ajout, validation et modifications de code push.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## Mettre à niveau une extension

Avant de continuer, vous avez besoin du nom et de la version du compositeur pour l’extension. Vérifiez également que l’extension est compatible avec votre projet et la version d’Adobe Commerce. En particulier, [vérifiez la version PHP requise](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) avant de commencer.

**Pour mettre à jour une extension** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Créez ou extrayez une branche de développement. Voir [embranchement](../development/cli-branches.md).

1. Ouvrez le fichier `composer.json` dans un éditeur de texte.

1. Localisez votre extension et mettez à jour la version.

1. Enregistrez vos modifications et quittez l’éditeur de texte.

1. Mettez à jour les dépendances du projet.

   ```bash
   composer update
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

Si vous rencontrez des erreurs, reportez-vous à la section [Récupération après une défaillance de composant](../deploy/recover-failed-deployment.md). Pour en savoir plus sur l’utilisation des extensions avec Adobe Commerce, voir [Extensions](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html) dans le _Guide d’administration_.
