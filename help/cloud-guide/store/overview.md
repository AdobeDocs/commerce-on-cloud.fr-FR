---
title: Présentation des options de magasin et de la gestion des configurations
description: Personnalisez votre boutique Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Configuration, Services
exl-id: e653172f-7370-4761-b2ce-3a420b33b948
TQID: https://experienceleague.adobe.com/iseYcfjh61-4ArUf9rKBGKFVuOz1dbS1xVq-FYiCxsw
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 193
ht-degree: 0%

---

# Présentation des options de magasin et de la gestion des configurations

Il existe de nombreuses façons de personnaliser votre boutique, comme l’ajout d’un thème personnalisé, l’installation d’une extension ou l’application d’une configuration spécifique dans les environnements d’infrastructure cloud. Vous pouvez configurer des paramètres pour des services spécifiques directement dans les environnements d’évaluation et de production. Vous pouvez configurer plusieurs sites web et magasins. La configuration de la boutique permet de configurer ces options sur votre station de travail locale et de déployer des paramètres spécifiques dans les environnements.

Pour accéder à votre storefront, utilisez la commande `magento-cloud url` et répondez aux invites. Vous pouvez également trouver l’URL dans la [!DNL Cloud Console] sous **Site d’accès**.

## Configurer les options du magasin

Les options de la boutique sont les suivantes :

* [Module business-to-business (B2B)](b2b-module.md)
* [Thèmes personnalisés](custom-theme.md)
* [Extensions](extensions.md)
* [Plusieurs sites](multiple-sites.md)
* [Services de paiement](paypal.md)

## Configuration des services et des intégrations

Il existe des [fichiers de configuration](../environment/overview.md) spécifiques qui gèrent certains comportements de déploiement dans les environnements distants. Vous pouvez consulter ces rubriques séparément :

* [Déploiement de l&#39;application](../application/configure-app-yaml.md)
* [Actions de création et de déploiement d’environnement](../environment/configure-env-yaml.md)
* [Itinéraires des requêtes entrantes](../routes/routes-yaml.md)
* [Services pris en charge](../services/services-yaml.md)

## Gestion de la configuration

Une fois les options de magasin, les services et les intégrations configurés, utilisez la gestion des configurations pour déployer ces configurations dans tous les environnements de manière cohérente et avec un temps d’arrêt minimal. Voir [Gestion de la configuration](store-settings.md).
