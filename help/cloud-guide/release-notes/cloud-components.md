---
title: Composants cloud pour Commerce
description: Consultez la liste des dernières améliorations apportées au package Composants cloud .
recommendations: noDisplay, catalog
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: dcf71ffbdafae46e6a02735c090c33a8fe248bc6
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Composants cloud pour Commerce

Le package [Composants cloud](https://github.com/magento/magento-cloud-components) fournit des fonctionnalités de base Adobe Commerce étendues pour les sites déployés sur une infrastructure cloud. Ce module dépend du module ECE-Tools. Ces notes de mise à jour décrivent les dernières améliorations apportées à ce package, qui est un composant de [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

Le package de `magento/magento-cloud-components` utilise la séquence de version suivante : `<major>.<minor>.<patch>`

Les notes de mise à jour incluent les éléments suivants :

- ![nouvelle icône](../../assets/new.svg) Nouvelles fonctionnalités
- ![icône de correctif](../../assets/fix.svg) Correctifs et améliorations

<!--Add release notes below-->

## v1.1.2 {#latest}

Date de publication : 3 juin 2025

- ![icône de correction](../../assets/fix.svg) **compatibilité améliorée avec la version 2.4.8**-bibliothèques tierces mises à jour pour une meilleure compatibilité avec la version 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.1

Date de publication : 6 février 2025

- ![nouvelle icône](../../assets/new.svg) **PHP 8.4**—Ajout de la prise en charge de PHP 8.4.<!-- MCLOUD-13148	 - -->
- ![Icône de correction](../../assets/fix.svg) **Correction pour le préchauffage du cache**—Correction d’un problème lié aux URL de catégorie lors du préchauffage du cache.<!-- MCLOUD-12454 - -->


## v1.1.0

Date de publication : 7 octobre 2024

- ![icône de correction](../../assets/fix.svg) **code refactorisé**—Suppression de la prise en charge des anciennes versions PHP 7.4, 7.3, 7.2 et des bibliothèques associées.<!-- MCLOUD-9278 - -->
- ![icône de correction](../../assets/fix.svg) **version de monolog mise à niveau**—Ajout de la prise en charge de monolog 3.6.<!-- MCLOUD-12855 - -->

## v1.0.14

Date de publication : 8 avril 2024

- ![nouvelle icône](../../assets/new.svg) **PHP**—Ajout de la prise en charge de PHP 8.3.

## v1.0.13

Date de publication : 10 mars 2023

- ![nouvelle icône](../../assets/new.svg) **Prise en charge améliorée de PHP 8.2** - Correction de problèmes de compatibilité avec certaines versions de PHP 8.2.x pour la prise en charge de Commerce 2.4.6.

## v1.0.12

Date de publication : 13 septembre 2022

- ![Icône de correction](../../assets/fix.svg) **Erreurs lors du préchauffage**—Correction d’un problème qui tentait d’effectuer un [préchauffage](../environment/variables-post-deploy.md#warm_up_pages) lorsque la visibilité de la page était définie sur [**Non visible individuellement**](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure) dans l’administration, ce qui entraînait des erreurs `ERROR: Warming up failed: <link to page>` dans le journal de déploiement.<!-- MCLOUD-9134 -->

## v1.0.11

Date de publication : 4 août 2022

- ![Icône de correction](../../assets/fix.svg) **Ajout de la prise en charge de la compatibilité avec Symfony 5.4**—Correctifs pour la compatibilité avec Symfony 5.4.<!-- AC-3550 -->

## v1.0.10

Date de publication : 10 mars 2022

- ![nouvelle icône](../../assets/new.svg) **Prise en charge de PHP 8.1**—Ajout de la prise en charge de PHP 8.1 et abandon de la prise en charge de PHP 7.1.

## v1.0.9

Date de publication : 25 octobre 2021

- ![icône de correction](../../assets/fix.svg) **Mettre à jour le monologue**—Mise à jour de la version minimale requise pour que le package `monolog` soit `^2.3`.<!-- ACMP-1263 -->

## v1.0.8

Date de publication : 29 juillet 2021

- ![Icône de correction](../../assets/fix.svg) **Suppression des barres obliques de fin des URL générées automatiquement**—Suppression des barres obliques de fin des URL de page de catégorie générées lors du préchauffage du cache.<!--MCLOUD-7192-->

## v1.0.7

Date de publication : 9 septembre 2020

- ![nouvelle icône](../../assets/new.svg) **Améliorations de la journalisation**—Réduisez la taille du fichier `cache.log` pour améliorer les performances.<!--MCLOUD-6859-->

- ![Icône de correction](../../assets/fix.svg) Correction d’une erreur de type dans les valeurs de configuration du cache qui entraînait l’échec de la commande de l’interface de ligne de commande `php bin/magento cache:evict`.

## v1.0.6

Date de publication : 5 août 2020

- ![nouvelle icône](../../assets/new.svg) **Améliorer les performances Redis**—Ajout de la commande `./bin/magento cache:evict` pour supprimer les clés Redis expirées, ce qui réduit l&#39;utilisation de la mémoire Redis pour améliorer les performances.<!--MCLOUD-6023-->

- ![icône de correction](../../assets/fix.svg) Suppression de la prise en charge de *Journaux New Relic en contexte* pour résoudre un problème de performances.<!--MCLOUD-6422-->

## v1.0.5

Date de publication : 25 juin 2020

- ![icône de correction](../../assets/fix.svg) correction d’un problème introduit dans magento/magento-cloud-components version 1.0.4 qui entraînait l’échec de l’opération de vidage du cache pendant la phase de déploiement, interrompant le processus de déploiement.

## v1.0.4

Date de publication : 25 juin 2020

- ![nouvelle icône](../../assets/new.svg) **Journaux New Relic implémentés en contexte** : les journaux d’application générés par Adobe Commerce s’affichent désormais dans les traces dans New Relic afin d’améliorer les fonctionnalités de dépannage.<!--MCLOUD-6029-->

- ![nouvelle icône](../../assets/new.svg) **Amélioration de la journalisation**—Ajout de la journalisation pour suivre l’invalidation du cache et les événements de réindexation complète.<!--MCLOUD-6157-->

## v1.0.3

Date de publication : 27 février 2020

- ![icône de correction](../../assets/fix.svg) Correction d’un problème de compatibilité afin de prendre en charge les versions 2002.0.x de `ece-tools` qui utilisent des versions PHP plus anciennes.

## v1.0.2

Date de publication : 6 février 2020

- ![nouvelle icône](../../assets/new.svg) Extension de la fonctionnalité de la variable d’environnement `WARM_UP_PAGES` afin de prendre en charge le préchargement du cache pour des pages de produits spécifiques. Consultez la rubrique [variables post-déploiement](../environment/variables-post-deploy.md#warm_up_pages) pour obtenir une description détaillée des fonctionnalités.<!--MAGECLOUD-4444-->

- ![icône de correction](../../assets/fix.svg) correction d’un problème en raison duquel une URL de magasin non valide entraînait l’échec du hook de post-déploiement lors de l’utilisation de la fonctionnalité `WARM_UP_PAGES` pour remplir le cache. Ce problème se produisait uniquement lorsque les réécritures d’URL étaient désactivées.<!-- MAGECLOUD-4094 -->

## v1.0.1

Date de publication : 23 juillet 2019

- ![Icône de correction](../../assets/fix.svg) Correction d’un problème affectant la fonctionnalité [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) qui utilise une URL de magasin par défaut. Désormais, si la commande `config:show:default-url` ne peut pas récupérer une URL de base, l’URL de la variable MAGENTO_CLOUD_ROUTES est utilisée.<!-- MAGECLOUD-3866 -->

## v1.0.0

Date de publication : 12 juin 2019

Il s’agit de la première version du package [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components), qui est une nouvelle dépendance pour `ece-tools` package version 2002.0.20 et ultérieure.

- ![nouvelle icône](../../assets/new.svg) Ajout de la possibilité d’utiliser des modèles d’expression régulière pour configurer la variable d’environnement **WARM_UP_PAGES** afin de mettre en cache des pages uniques, plusieurs domaines et plusieurs pages. Voir [Variables post-déploiement](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
