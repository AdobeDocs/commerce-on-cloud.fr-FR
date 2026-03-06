---
title: Notes de mise à jour de la suite Cloud Tools
description: Découvrez les dernières améliorations apportées à la suite d’outils cloud pour Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: 2375b3a2a368f4db5157d3995cc48fd5f4418972
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 3%

---

# Notes de mise à jour de la suite d’outils Commerce Cloud

Ces informations de mise à jour présentent les dernières améliorations apportées à la suite d’outils cloud pour les packages Commerce conçus pour déployer et gérer les installations et mises à niveau d’Adobe Commerce sur la plateforme cloud.

| Notes de mise à jour | Version | Description | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [package ece-tools](ece-tools-package.md) | 2002.2.10 | Ensemble de scripts et d’outils conçu pour gérer et déployer des projets cloud. | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.10) |
| [Correctifs cloud pour Commerce](cloud-patches.md) | 1.1.13 | Un ensemble de correctifs qui améliorent l’intégration de toutes les versions d’Adobe Commerce avec les environnements cloud. Ce package comprend les correctifs Adobe Commerce et les correctifs disponibles qui sont appliqués lorsque vous utilisez `ece-tools` pour le déploiement | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.13) |
| [&#x200B; Cloud Docker pour Commerce &#x200B;](cloud-docker.md) | 1.4.7 | Fichiers de fonctionnalité et de configuration pour les images Docker afin de déployer Adobe Commerce dans un environnement cloud local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.7) |
| [Composants cloud de Commerce](cloud-components.md) | 1.1.4 | Fonctionnalité principale Adobe Commerce étendue pour les sites déployés sur l’infrastructure cloud | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

Lorsque vous mettez à jour ECE-Tools 2002.1.0 ou une version ultérieure, vous mettez automatiquement à jour vers les dernières versions des autres packages, qui sont des dépendances pour le package `ece-tools`. Voir [Métapaquet cloud](../development/overview.md#cloud-metapackage) pour obtenir une liste des dépendances.
