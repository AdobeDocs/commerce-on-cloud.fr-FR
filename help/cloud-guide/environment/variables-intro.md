---
title: Variables d’environnement
description: Consultez une liste des variables d’environnement spécifiques à Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Build, Configuration, Deploy
exl-id: 38b2cdc2-1a98-48bd-90b2-13ef179da26f
TQID: https://experienceleague.adobe.com/qRdv72nxgkwRjRz0lXqs33rSmZKc3akq2W0pJK4CM7k
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 266
ht-degree: 0%

---

# Variables d’environnement

Adobe Commerce sur l’infrastructure cloud vous permet d’affecter des variables d’environnement pour remplacer les options de configuration. Le package `ece-tools` définit les valeurs du fichier `env.php` en fonction des valeurs des [variables cloud](variables-cloud.md), des variables définies dans le [!DNL Cloud Console] et du fichier de configuration `.magento.env.yaml`.

Les variables d’environnement du fichier `.magento.env.yaml` personnalisent l’environnement cloud en remplaçant votre configuration Commerce existante. Si une valeur par défaut est `Not Set`, le package `ece-tools` exécute l’action **NON** et utilise la valeur par défaut [!DNL Commerce] ou la valeur de la configuration `MAGENTO_CLOUD_RELATIONSHIPS`. Si la valeur par défaut est définie, le package `ece-tools` agit pour définir cette valeur par défaut.

Les types de variables d’environnement incluent :

- [ADMIN](variables-admin.md)—les variables remplacent les variables ADMIN du projet
- [MAGENTO_CLOUD](variables-cloud.md) : variables spécifiques à l’infrastructure cloud
- Variables utilisées dans le fichier `.magento.env.yaml` :
   - [Global](variables-global.md) : les variables affectent les étapes de création, de déploiement et de post-déploiement
   - [Build](variables-build.md) : les variables contrôlent les actions de build.
   - [Déployer](variables-deploy.md) : les variables contrôlent les actions de déploiement
   - [Post-déploiement](variables-post-deploy.md) : les variables contrôlent les actions après le déploiement.

Les variables sont _hiérarchiques_, ce qui signifie que si une variable n’est pas remplacée, elle est héritée de l’environnement parent.

Vous pouvez définir des [variables ADMIN](variables-admin.md) à partir du [!DNL Cloud Console] ou à l’aide de l’interface de ligne de commande Adobe Commerce. Vous pouvez gérer d’autres variables d’environnement à partir du fichier [`.magento.env.yaml`](configure-env-yaml.md) afin de gérer les actions de création et de déploiement dans tous vos environnements, y compris l’évaluation et la production professionnelles, sans avoir besoin d’un ticket d’assistance.

>[!TIP]
>
>Les fichiers YAML respectent la casse et n’autorisent pas les onglets. Veillez à utiliser une mise en retrait cohérente dans l’ensemble du fichier `.magento.env.yaml`, faute de quoi votre configuration risque de ne pas fonctionner comme prévu. Les exemples de cette documentation et de l’exemple de fichier utilisent la mise en retrait _à deux espaces_. Utilisez la commande [ece-tools validate](configure-env-yaml.md#validate-configuration-file) pour vérifier votre configuration.
