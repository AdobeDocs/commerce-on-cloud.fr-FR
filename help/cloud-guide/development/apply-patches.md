---
title: Application de correctifs
description: Découvrez comment appliquer des correctifs dans le projet d’infrastructure cloud d’Adobe Commerce.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Application de correctifs

Les correctifs [Cloud pour Commerce](https://github.com/magento/magento-cloud-patches) et l’outil de correctifs de la qualité [Quality Patches Tool](https://github.com/magento/quality-patches) fournissent des correctifs à votre application Adobe Commerce installée.

- Le package Correctifs cloud pour Commerce fournit les correctifs requis avec des correctifs critiques
- Les correctifs de qualité fournissent des correctifs de qualité facultatifs et à faible impact sous la forme de [correctifs individuels](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html?lang=fr#individual-patch) qui ne contiennent pas de modifications non rétrocompatibles

Consultez [Correctifs disponibles](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=fr) dans le _Guide des outils d’exploitation Commerce_ pour obtenir une liste complète des correctifs publiés.

Les deux packages améliorent l’intégration de toutes les versions d’Adobe Commerce avec les environnements cloud et prennent en charge la diffusion rapide de correctifs critiques, facultatifs et personnalisés. Vous pouvez utiliser ces packages pour appliquer, rétablir et afficher des informations générales sur tous les correctifs individuels disponibles pour Commerce.

>[!TIP]
>
>Vous pouvez utiliser l’[outil de correctifs de qualité](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=fr) et les correctifs cloud pour Commerce en tant que packages autonomes pour les projets Magento Open Source et Adobe Commerce. Nous vous recommandons d’utiliser l’outil de correctifs de qualité pour les projets non cloud.

Lorsque vous déployez des modifications dans l’environnement distant, le package `ece-tools` utilise `magento/magento-cloud-patches` et `magento/quality-patches` pour rechercher les correctifs en attente et les applique automatiquement dans l’ordre suivant :

1. Appliquez tous les correctifs Commerce requis inclus dans le package Correctifs cloud pour Commerce .
1. Application des correctifs Commerce facultatifs sélectionnés inclus dans l’outil de correctifs de la qualité.
1. Appliquez les correctifs personnalisés dans le répertoire `/m2-hotfixes`, par ordre alphabétique et par nom de correctif.

>[!NOTE]
>
>Lorsque vous mettez à jour le package `ece-tools` ou le package Correctifs cloud pour Commerce, les derniers correctifs requis sont appliqués la prochaine fois que vous déployez votre projet. Vous pouvez également les déployer immédiatement à l’aide de la commande de l’interface de ligne de commande `ece-patches apply` et redéployer votre environnement Cloud. Vous ne pouvez pas ignorer [ correctifs requis](https://github.com/magento/magento-cloud-patches/tree/develop/patches) pendant le processus de déploiement.

## Conditions préalables

{{upgrade-tip}}

L’outil de correctifs de qualité est une dépendance pour les correctifs cloud pour Commerce et le package `ece-tools`. Pour appliquer les derniers patchs, vous devez avoir installé [la dernière version de ECE-Tools](../dev-tools/update-package.md). La version minimale requise des outils ECE est la version 2002.1.2.

## Afficher les correctifs et l’état disponibles

Pour afficher la liste des patchs disponibles :

```bash
php ./vendor/bin/ece-patches status
```

Exemple de réponse :

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

Le tableau d&#39;état contient les types d&#39;informations suivants :

- **Type** :
   - `Optional` : tous les correctifs de l’outil de correctifs de la qualité et du package de correctifs cloud sont facultatifs pour les installations d’Adobe Commerce et de Magento Open Source. Pour Adobe Commerce sur les infrastructures cloud, tous les correctifs sont facultatifs.
   - `Required` : tous les correctifs du package Correctifs cloud pour Commerce sont requis pour les clients cloud.
   - `Deprecated` : le correctif individuel est marqué comme obsolète et nous vous recommandons de le rétablir si vous l’avez appliqué. Une fois que vous avez rétabli un correctif obsolète, il ne s’affiche plus dans le tableau d’état.
   - `Custom` : tous les correctifs du répertoire « m2-hotfix ».

- **Statut** :
   - `Applied` : le correctif a été appliqué.
   - `Not applied` : le correctif n&#39;a pas été appliqué.
   - `N/A` : impossible de définir l&#39;état du correctif en raison de conflits.

- **Détails** :
   - `Affected components` : liste des modules concernés.
   - `Required patches` : liste des patchs requis (dépendances).
   - `Recommended replacement` : correctif qui est le remplacement recommandé d&#39;un correctif obsolète.

## Application d’un correctif dans un environnement local

Vous pouvez appliquer les correctifs manuellement dans un environnement local et les tester avant de les déployer.

**Pour appliquer des correctifs individuels dans un environnement de développement local** :

1. Ajoutez la variable &#39;QUALITY_PATCH&#39; au fichier `.magento.env.yaml` et répertoriez les correctifs requis sous.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. Appliquez les correctifs à partir de la racine du projet.

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   La commande `ece-patches apply` applique les correctifs dans l’ordre suivant :
   - Correctifs requis
   - Correctifs individuels facultatifs
   - Correctifs personnalisés du répertoire `/m2-hotfixes`

1. Effacez le cache.

   ```bash
   php ./bin/magento cache:clean
   ```

1. Testez les correctifs et apportez les modifications nécessaires aux correctifs personnalisés.

## Application d’un correctif dans un environnement distant

>[!WARNING]
>
>Nous vous recommandons vivement de tester tous les correctifs dans une intégration ou des environnements d’évaluation avant le déploiement dans l’environnement de production.

**Pour appliquer des correctifs dans un environnement distant** :

1. Ajoutez la variable `QUALITY_PATCHES` au fichier `.magento.env.yaml` et répertoriez les correctifs requis sous .

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >Après la mise à niveau vers une nouvelle version d’Adobe Commerce, vous devez réappliquer les correctifs si ceux-ci ne sont pas inclus dans la nouvelle version.

1. Ajouter, valider et transmettre le fichier `.magento.env.yaml` mis à jour.

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## Application d’un correctif personnalisé

Lorsque vous déployez, ECE-Tools applique tous les patchs Adobes et tous les patchs personnalisés que vous ajoutez au répertoire `/m2-hotfixes` dans la racine du projet.

>[!NOTE]
>
>Tous les noms de fichier de correctif doivent se terminer par l’extension `.patch`.

**Pour appliquer et tester un correctif personnalisé dans un environnement cloud** :

1. Dans la racine du projet, créez un répertoire appelé `m2-hotfixes` s’il n’existe pas

   ```bash
   mkdir m2-hotfixes
   ```

1. Copiez le fichier de correctif dans le répertoire `/m2-hotfixes`.

1. Ajout, validation et modifications de code push.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Veillez à tester tous les correctifs dans un environnement de pré-production. Pour Adobe Commerce sur les infrastructures cloud, vous pouvez créer des branches à l’aide de la commande d’interface de ligne de commande `magento-cloud environment:branch <branch-name>`.

## Rétablir un correctif personnalisé

Pour rétablir ou désinstaller un correctif personnalisé précédemment appliqué :

1. Supprimez le fichier de correctif du répertoire `/m2-hotfixes`.

1. Ajout, validation et modifications de code push.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Veillez à effectuer le test dans un environnement de pré-production. Pour Adobe Commerce sur les infrastructures cloud, vous pouvez créer des branches à l’aide de la commande d’interface de ligne de commande `magento-cloud environment:branch <branch-name>`.

## Application de correctifs à un projet non cloud

Utilisez l’outil [Correctifs de qualité](https://github.com/magento/quality-patches) pour les projets Magento Open Source et Adobe Commerce.

## Rétablissement d’un correctif dans un environnement local

Vous pouvez rétablir tous les correctifs précédemment appliqués dans un environnement de développement local à l’aide de l’interface de ligne de commande `ece-patches`.

Pour rétablir tous les correctifs appliqués :

```bash
php ./vendor/bin/ece-patches revert
```

Cette commande rétablit tous les correctifs dans l’ordre suivant :

- Annule tous les correctifs personnalisés appliqués à partir du répertoire /m2-hotfixes.
- Rétablit tous les correctifs individuels facultatifs appliqués.
- Annule tous les correctifs obligatoires appliqués.

## Journalisation

L’outil de correctifs de qualité consigne toutes les opérations dans le fichier `<Project_root>/var/log/patch.log`.
