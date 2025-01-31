---
title: Présentation des fichiers de configuration
description: Découvrez comment configurer l’environnement de l’infrastructure cloud pour prendre en charge le déploiement et la gestion de votre boutique Adobe Commerce personnalisée.
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Présentation des fichiers de configuration

Les environnements dans Adobe Commerce sur les infrastructures cloud comprennent des conteneurs avec des applications, des services et une base de données afin de fournir un système complet pour la base de code et les fichiers de votre application Adobe Commerce.

Vous pouvez configurer les paramètres d’application, les itinéraires, les actions de génération et de déploiement et les notifications pour prendre en charge les environnements de projet à l’aide des fichiers de configuration suivants :

| Configuration | Nom de fichier | Description |
| ------------- | -------- | ----------- |
| [ Application ](../application/configure-app-yaml.md) | `.magento.app.yaml` | Définit comment créer et déployer Adobe Commerce, y compris les services, les hooks et les tâches cron. |
| [ Environnement ](configure-env-yaml.md) | `.magento.env.yaml` | Centralise la gestion des actions de génération et de déploiement dans tous vos environnements, y compris l’évaluation et la production professionnelles, à l’aide de variables d’environnement. |
| [Itinéraires](../routes/routes-yaml.md) | `.magento/routes.yaml` | Configurez la mise en cache, les redirections et les inclusions côté serveur. |
| [Service ](../services/services-yaml.md) | `.magento/services.yaml` | Définit les services utilisés par Adobe Commerce par nom et par version. Par exemple, ce fichier peut inclure des versions de MariaDB, des extensions PHP, Redis, RabbitMQ et Elasticsearch ou OpenSearch. Vous devez ouvrir un ticket d’assistance pour pousser ces modifications vers les environnements d’évaluation et de production ProPlan. |
| [Paramètres PHP](../application/php-settings.md#configure-php) | `php.ini` | Fichier facultatif qui peut être ajouté au projet. Les paramètres contenus dans ce fichier sont ajoutés à ceux gérés par l’infrastructure cloud. |

{style="table-layout:auto"}

## Mises à jour de configuration des environnements Pro

Pour les environnements d’évaluation et de production d’Adobe Commerce sur les infrastructures cloud Pro, vous pouvez mettre à jour de nombreuses options de configuration dans votre environnement de développement local et valider les modifications afin de les appliquer à ces environnements. Cependant, vous devez [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour mettre à jour les options de configuration suivantes :

- Installez ou mettez à jour les services dans le fichier `.magento/services.yaml`.
- Modifiez la configuration des propriétés `mounts` et `disk` dans le fichier `.magento.app.yaml`.

{{pro-self-service-warning}}
