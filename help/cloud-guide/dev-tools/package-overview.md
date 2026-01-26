---
title: Package [!DNL ECE-Tools]
description: Découvrez le package  [!DNL ECE-Tools]  et comment il permet de gérer et de déployer Adobe Commerce.
exl-id: 15d762ef-bca7-480b-b719-caf131dc9180
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# Ensemble d&#39;outils de la CEE

Le package [!DNL ECE-Tools] est un ensemble de scripts et d’outils conçus pour gérer et déployer l’application [!DNL Commerce]. Le package `ece-tools` simplifie de nombreux processus, tels que la gestion des tâches cron, la vérification de la configuration du projet et l’application de correctifs et de correctifs logiciels Adobe. Vous pouvez afficher le référentiel de code [open-source [!DNL ECE-Tools] sur GitHub](https://github.com/magento/ece-tools) et y contribuer.

{{ece-tools-package}}

Le package `ece-tools` est compatible avec Adobe Commerce (à partir de la version 2.1.4) et contient des scripts et des commandes Adobe Commerce sur l’infrastructure cloud conçus pour vous aider à gérer votre code et à créer et déployer automatiquement vos projets.

Voici la liste des commandes `ece-tools` disponibles :

```bash
php ./vendor/bin/ece-tools list
```

## Créer et déployer

Le package `ece-tools` contient des commandes permettant d’effectuer des opérations pour les étapes de création, de déploiement et de post-déploiement du lancement de votre application Adobe Commerce sur l’infrastructure cloud. Par exemple, la commande `php ./vendor/bin/ece-tools build` lance le processus de création de l’application.

Par défaut, ces commandes `ece-tools` se trouvent dans la propriété [hooks](../application/hooks-property.md) du fichier de configuration `.magento.app.yaml`.

## Générateur de configuration Docker

Le package `ece-tools` comprend une dépendance pour le package [magento/magento-cloud-docker](https://github.com/magento/magento-cloud-docker), qui fournit des fichiers de fonctionnalité et de configuration pour que les images Docker lancent un environnement de développement Docker pour Adobe Commerce sur l’infrastructure cloud. Vous pouvez également exécuter Cloud Docker pour Commerce en tant que package autonome. Voir [Développement Docker](../dev-tools/cloud-docker.md).

## Services, itinéraires et variables

Vous pouvez utiliser le package `ece-tools` pour afficher des informations détaillées sur les [variables cloud](../environment/variables-cloud.md) codées en Base64 et utilisées dans n’importe quel environnement cloud. La commande suivante affiche tous les services, itinéraires et variables.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Pour afficher un ensemble spécifique d&#39;informations, utilisez le format suivant :

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` : affiche les données de relation de la variable d&#39;environnement `MAGENTO_CLOUD_RELATIONSHIPS`, définie dans le fichier `services.yaml`.
- `routes` : affiche les itinéraires configurés pour le projet à l&#39;aide de la variable d&#39;environnement `MAGENTO_CLOUD_ROUTES`.
- `variables` : affiche les variables configurées pour le projet à l&#39;aide de la variable d&#39;environnement `MAGENTO_CLOUD_VARIABLES`.

Exemple de sortie pour l’option `services` :

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Vérification de la configuration de l’environnement

Un ensemble de commandes de vérification est disponible pour vous aider à évaluer la configuration de votre projet. Voir [Assistants intelligents](../deploy/smart-wizards.md) dans la section _Optimiser le déploiement_ pour une description détaillée de chaque commande d’assistant. La commande `wizard:ideal-state` s’exécute automatiquement pendant la phase de création. Pour vérifier l’état idéal de votre projet :

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Vous devez exécuter la commande `wizard:ideal-state` dans l’environnement cloud distant. La commande renvoie toujours l’erreur `The configured state is not ideal` lors de l’exécution dans l’environnement de développement local.

Exemple de sortie :

```
Ideal state is configured
```

Voir [ Notes de mise à jour pour ece-tools](../release-notes/cloud-tools-suite.md).

## Correctifs Adobe et correctifs personnalisés

Le package `ece-tools` comprend une dépendance pour le package [magento/magento-cloud-patches](https://github.com/magento/magento-cloud-patches), qui fournit des correctifs Adobe et des correctifs logiciels qui améliorent l’intégration de toutes les versions d’Adobe Commerce aux environnements cloud et prennent en charge la diffusion rapide de correctifs critiques. Le « fournit également des correctifs personnalisés que vous ajoutez à votre projet d’infrastructure cloud Adobe Commerce on cloud. Voir [ Application de correctifs](../development/apply-patches.md).

