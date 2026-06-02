---
title: Notes de mise à jour de la suite Cloud Tools
description: Découvrez les dernières améliorations apportées à la suite d’outils cloud pour Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
TQID: https://experienceleague.adobe.com/eQQvGGEwj4D6pOlhZqNA-SMdc6JxH-Wg-hBRZaR1C-M
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 220
ht-degree: 3%

---

# Notes de mise à jour de la suite d’outils Commerce Cloud

Ces informations de mise à jour présentent les dernières améliorations apportées à la suite d’outils cloud pour les packages Commerce conçus pour déployer et gérer les installations et mises à niveau d’Adobe Commerce sur la plateforme cloud.

| Notes de mise à jour | Version | Description | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [package ece-tools](ece-tools-package.md) | 2002.2.11 | Ensemble de scripts et d’outils conçu pour gérer et déployer des projets cloud. | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.11) |
| [Correctifs cloud pour Commerce](cloud-patches.md) | 1.1.14 | Un ensemble de correctifs qui améliorent l’intégration de toutes les versions d’Adobe Commerce avec les environnements cloud. Ce package comprend les correctifs Adobe Commerce et les correctifs disponibles qui sont appliqués lorsque vous utilisez `ece-tools` pour le déploiement | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.14) |
| [ Cloud Docker pour Commerce ](cloud-docker.md) | 1.4.8 | Fichiers de fonctionnalité et de configuration pour les images Docker afin de déployer Adobe Commerce dans un environnement cloud local | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.8) |
| [Composants cloud de Commerce](cloud-components.md) | 1.1.4 | Fonctionnalité principale Adobe Commerce étendue pour les sites déployés sur l’infrastructure cloud | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

Lorsque vous mettez à jour ECE-Tools 2002.1.0 ou une version ultérieure, vous mettez automatiquement à jour vers les dernières versions des autres packages, qui sont des dépendances pour le package `ece-tools`. Voir [Métapaquet cloud](../development/overview.md#cloud-metapackage) pour obtenir une liste des dépendances.
