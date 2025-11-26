---
title: Présentation du développement
description: Préparez-vous au développement local avec un projet d’infrastructure cloud Adobe Commerce.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Présentation du développement

Les environnements distants d’Adobe Commerce sur les infrastructures cloud sont en **lecture seule**, y compris tous les environnements de démarrage et tous les environnements d’intégration, d’évaluation et de production Pro. Dans un environnement de développement local, vous pouvez écrire et tester le code avant de le transmettre à un environnement d’intégration pour des tests et un déploiement supplémentaires dans les environnements d’évaluation et de production.

Avant de préparer votre espace de travail local, vérifiez que vous disposez de vos [informations d’identification](../../get-started/prepare-workspace.md). Le développement local nécessite l’installation de PHP et du compositeur, sauf si vous choisissez d’utiliser [Cloud Docker pour Commerce](#docker-environment).

## Packages requis

Adobe Commerce sur les infrastructures cloud utilise le compositeur pour gérer les dépendances et les mises à niveau des projets. Pour le développement local, vous devez installer les versions PHP et Composer compatibles avec votre projet Cloud. Par exemple, si vous utilisez le modèle de cloud [!DNL Commerce] 2.4.8, vous pouvez voir que le fichier de configuration [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml) utilise **PHP 8.4** et **Composer 2.8.4**.

Le compositeur installe les bibliothèques et les dépendances requises pour votre projet dans le répertoire `vendor`. Les fichiers Composer requis suivants se trouvent dans le répertoire racine du projet :

- `composer.json` : utilisez le fichier `composer.json` pour gérer les installations et les mises à niveau des produits.
- `composer.lock` : le fichier `composer.lock` stocke un ensemble de dépendances de version exactes qui répondent aux contraintes de version de chaque exigence pour chaque package dans l&#39;arborescence des dépendances du projet.

**Commandes courantes :**

| Commande | Description |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Mises à jour vers les dernières versions des dépendances reflétées dans le fichier `composer.json`. Cette opération met à jour le fichier `composer.lock`. |
| `composer install` | Lit le fichier `composer.lock` pour télécharger les dépendances. Il est recommandé de conserver une copie à jour de `composer.lock` dans le référentiel de votre projet. |

{style="table-layout:auto"}

Une fois que vous avez ajouté, validé et envoyé le code mis à jour, le processus de déploiement exécute automatiquement la commande `composer install` pendant la [phase de création](../deploy/process.md#build-phase-build-phase).

### Métapaquet cloud

Adobe Commerce sur l’infrastructure cloud utilise un métapaquet qui nécessite des `magento/product-enterprise-edition`. Pour obtenir les dernières mises à jour de la dernière version de Commerce, utilisez la syntaxe de contrainte suivante :

```text
>=current_version <next_version
```

Par exemple, pour utiliser la dernière version d’Adobe Commerce, la version 2.4.9, définissez `2.4.8` comme la version « actuelle » et `2.4.9` comme la version « suivante » dans le fichier `composer.json` :

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

Les principaux packages de ce métapaquet sont les suivants :

- **fournisseur/magento/ece-tools** : le package `ece-tools` est compatible avec Adobe Commerce version 2.1.4 et ultérieure. Il fournit un ensemble riche de fonctionnalités que vous pouvez utiliser pour gérer votre projet d’infrastructure Adobe Commerce sur cloud. Il contient des scripts et des commandes d’infrastructure cloud d’Adobe Commerce conçues pour vous aider à gérer votre code et à créer et déployer automatiquement vos projets. Voir la présentation du package [`ece-tools`](../dev-tools/package-overview.md).
- **chez le fournisseur/magento/product-enterprise-edition** : ce métapaquet nécessite des composants d’application, notamment des modules, des structures, des thèmes, etc.
- **fournisseur/fastly2/magento2** : ce module gère le réseau CDN et les services Fastly pour les environnements d’évaluation et de production Pro et de production de démarrage. Voir [Services Fastly](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **fournisseur/magento/module-paypal-on-boarding**—Ce module permet de passer en caisse la passerelle de paiement PayPal en se connectant à votre compte marchand PayPal. Voir [Outil d’intégration PayPal](../store/paypal.md).
- **fournisseur/aem/rum** : ce module gère l’outil de collecte de données [Télémétrie opérationnelle](../monitor/operational-telemetry.md).

>[!TIP]
>
>Voir [Packages cloud pour Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) dans les _Notes de mise à jour de Commerce_ pour obtenir une liste des dépendances et des licences tierces.

## Environnement Docker

Vous pouvez utiliser l’outil Cloud Docker for Commerce pour émuler Adobe Commerce sur les environnements de production et de développement d’infrastructure cloud pour le développement local. Cloud Docker pour Commerce ne nécessite pas l’installation locale de PHP et du compositeur.

- [Développement local avec Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) sur le site Adobe Developer
- [Architecture Docker et commandes courantes](../dev-tools/cloud-docker.md)
- [Notes de mise à jour de Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Pour plus d’informations sur l’utilisation des services d’hébergement basés sur Git avec Adobe Commerce sur les infrastructures cloud, voir [&#x200B; Intégrations &#x200B;](../integrations/overview.md).
