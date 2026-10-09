---
title: Présentation des fichiers de configuration
description: Découvrez comment configurer l’environnement de l’infrastructure cloud pour prendre en charge le déploiement et la gestion de votre boutique Adobe Commerce personnalisée.
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: 305380b0-1920-4037-a1db-80e72c6af333
TQID: 'https://experienceleague.adobe.com/mFjzrTN6R7LC3e9ADnzzulcWAwun4k-g3aCjc9Bo3gQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: 00451af3-7b97-5414-9992-3a6c269e413f
    internal-label: Paas
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: df5e974b-6742-4873-a687-a6bedaafdaa2
    internal-label: IaaS
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%
---
# Présentation des fichiers de configuration

Les environnements dans Adobe Commerce sur les infrastructures cloud comprennent des conteneurs avec des applications, des services et une base de données afin de fournir un système complet pour la base de code et les fichiers de votre application Adobe Commerce.

Vous pouvez configurer les paramètres d’application, les itinéraires, les actions de génération et de déploiement et les notifications pour prendre en charge les environnements de projet à l’aide des fichiers de configuration suivants :

| Configuration | Nom de fichier | Description |
| ------------- | -------- | ----------- |
| [&#x200B; Application &#x200B;](../application/configure-app-yaml.md) | `.magento.app.yaml` | Définit comment créer et déployer Adobe Commerce, y compris les services, les hooks et les tâches cron. |
| [&#x200B; Environnement &#x200B;](configure-env-yaml.md) | `.magento.env.yaml` | Centralise la gestion des actions de génération et de déploiement dans tous vos environnements, y compris l’évaluation et la production professionnelles, à l’aide de variables d’environnement. |
| [Itinéraires](../routes/routes-yaml.md) | `.magento/routes.yaml` | Configurez la mise en cache, les redirections et les inclusions côté serveur. |
| [Service &#x200B;](../services/services-yaml.md) | `.magento/services.yaml` | Définit les services utilisés par Adobe Commerce par nom et par version. Par exemple, ce fichier peut inclure des versions de MariaDB, des extensions PHP, Redis ou Valkey, RabbitMQ et Elasticsearch ou OpenSearch. Vous devez ouvrir un ticket d’assistance pour pousser ces modifications vers les environnements d’évaluation et de production ProPlan. |
| [Paramètres PHP](../application/php-settings.md#configure-php) | `php.ini` | Fichier facultatif qui peut être ajouté au projet. Les paramètres contenus dans ce fichier sont ajoutés à ceux gérés par l’infrastructure cloud. |

{style="table-layout:auto"}

## Mises à jour de configuration des environnements Pro

Pour les environnements d’évaluation et de production d’Adobe Commerce sur les infrastructures cloud Pro, vous pouvez mettre à jour de nombreuses options de configuration dans votre environnement de développement local et valider les modifications afin de les appliquer à ces environnements. Cependant, vous devez [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/fr/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) pour mettre à jour les options de configuration suivantes :

- Installez ou mettez à jour les services dans le fichier `.magento/services.yaml`.
- Modifiez la configuration des propriétés `mounts` et `disk` dans le fichier `.magento.app.yaml`.

{{pro-self-service-warning}}
