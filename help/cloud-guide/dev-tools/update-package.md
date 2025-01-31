---
title: Mise à jour du module ECE-Tools
description: Découvrez comment mettre à niveau le package ECE-Tools pour tirer parti des derniers correctifs et fonctionnalités appliqués à Adobe Commerce sur l'infrastructure cloud.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Mise à jour du module ECE-Tools

Une mise à jour du package `ece-tools` met également à jour les autres packages [Cloud Tools Suite for Commerce](../release-notes/cloud-tools-suite.md), qui sont des dépendances de `ece-tools`. Par conséquent, vous devez utiliser une version d’Adobe Commerce sur l’infrastructure cloud qui prend en charge le package `ece-tools`.

{{ece-tools-package}}

**Conditions préalables** :

- Avant de `ece-tools` mettre à jour, consultez les notes de mise à jour de la [suite d’outils cloud pour Commerce](../release-notes/cloud-tools-suite.md).
- Si vous effectuez une mise à jour de la version `ece-tools` 2002.0.22 ou antérieure à la version 2002.1.0, passez en revue la section [Modifications rétrocompatibles](../release-notes/backward-incompatible-changes.md) et apportez les modifications requises à votre projet d’infrastructure cloud Adobe Commerce.
- Examinez [Mises à niveau et correctifs](../development/commerce-version.md#upgrade-from-older-versions) pour déterminer les versions des outils ECE compatibles avec votre projet Adobe Commerce sur l&#39;infrastructure cloud.

{{upgrade-tip}}

**Pour mettre à jour le package `ece-tools`** :

1. Sur votre station de travail locale, effectuez une mise à jour à l’aide du compositeur.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Si vous ne pouvez pas effectuer la mise à jour au-delà de `ece-tools` version 2002.0.8, voir [Mettre à niveau le projet pour utiliser le package ECE-Tools](install-package.md).

1. Ajout, validation et modifications de code push.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Après la validation du test, fusionnez cette branche avec la branche d’intégration.
