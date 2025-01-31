---
title: Configuration de l’environnement
description: Découvrez comment configurer des actions de génération et de déploiement sur tous les environnements d’infrastructure cloud de Commerce, y compris l’évaluation et la production professionnelles, à l’aide de variables d’environnement.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Configuration des variables d’environnement pour le déploiement

Le fichier `.magento.env.yaml` utilise des variables d’environnement pour centraliser la gestion des actions de création et de déploiement dans tous vos environnements, y compris l’évaluation et la production de pro. Pour configurer des actions uniques dans chaque environnement, vous devez modifier ce fichier dans chaque environnement.

>[!TIP]
>
>Les fichiers YAML respectent la casse et n’autorisent pas les onglets. Veillez à utiliser une mise en retrait cohérente dans l’ensemble du fichier `.magento.env.yaml`, faute de quoi votre configuration risque de ne pas fonctionner comme prévu. Les exemples de la documentation et de l’exemple de fichier utilisent la mise en retrait _à deux espaces_. Utilisez la commande [ece-tools validate](#validate-configuration-file) pour vérifier votre configuration.

## Structure de fichier

Le fichier `.magento.env.yaml` contient deux sections : `stage` et `log`. La section `stage` contrôle les actions qui se produisent au cours des phases du processus de déploiement [Cloud](../deploy/process.md).

- `stage` : utilisez la section étape pour définir certaines actions pour les étapes suivantes du déploiement :
   - `global` : contrôle les actions au cours des phases de création, de déploiement et de post-déploiement. Vous pouvez remplacer ces paramètres dans les sections Créer, déployer et post-déployer.
   - `build` : contrôle uniquement les actions de la phase de création. Si vous ne spécifiez pas de paramètres dans cette section, la phase de création utilise les paramètres de la section globale.
   - `deploy` : contrôle les actions lors de la phase de déploiement uniquement. Si vous ne spécifiez pas de paramètres dans cette section, la phase de déploiement utilise les paramètres de la section globale.
   - `post-deploy` : contrôle les actions _après_ le déploiement de votre application et _après_ le conteneur commence à accepter les connexions.
- `log` : utilisez la section journal pour configurer les [notifications](set-up-notifications.md), y compris les types de notification et le niveau de détail.
   - `slack` : permet de configurer un message à envoyer à un robot de Slack.
   - `email` : configurez un e-mail à envoyer à un ou plusieurs destinataires d&#39;e-mail.
   - [gestionnaires de journaux](log-handlers.md) : configurez les messages des applications matérielles et logicielles envoyés à un serveur de journalisation distant.

### Variables d’environnement

Le package `ece-tools` définit les valeurs du fichier `env.php` en fonction des valeurs des [variables cloud](variables-cloud.md), des variables définies dans le [!DNL Cloud Console] et du fichier de configuration `.magento.env.yaml`. Les variables d’environnement du fichier `.magento.env.yaml` personnalisent l’environnement cloud en remplaçant votre configuration Commerce existante. Si une valeur par défaut est `Not Set`, le package de `ece-tools` exécute l’action **NON** et utilise la valeur par défaut [!DNL Commerce] ou la valeur de la configuration MAGENTO_CLOUD_RELATIONSHIPS. Si la valeur par défaut est définie, le package `ece-tools` agit pour définir cette valeur par défaut.

Les rubriques suivantes contiennent des définitions détaillées, indiquant par exemple si une valeur par défaut est définie ou non, de toutes les variables que vous pouvez utiliser dans le fichier `.magento.env.yaml` :

- [Global](variables-global.md) : les variables contrôlent les actions lors de chaque phase : création, déploiement et post-déploiement
- [Build](variables-build.md) : les variables contrôlent les actions de build.
- [Déployer](variables-deploy.md) : les variables contrôlent les actions de déploiement
- [Post-déploiement](variables-post-deploy.md) : les variables contrôlent les actions après le déploiement.

### Créer un fichier de configuration à partir de l’interface de ligne de commande

Vous pouvez générer un fichier de configuration `.magento.env.yaml` pour un environnement Cloud à l’aide des commandes `ece-tools` suivantes.

>Création d’un fichier de configuration

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Mettre à jour les valeurs dans le fichier de configuration

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Les deux commandes nécessitent un seul argument : un tableau au format JSON qui spécifie une valeur pour au moins une variable de build, de déploiement ou de post-déploiement. Par exemple, la commande suivante définit les valeurs des variables `SCD_THREADS` et `CLEAN_STATIC_FILES` :

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

et crée un fichier `.magento.env.yaml` avec les paramètres suivants :

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

Vous pouvez utiliser la commande `cloud:config:update` pour mettre à jour le nouveau fichier. Par exemple, la commande suivante modifie la valeur `SCD_THREADS` et ajoute la configuration `SCD_COMPRESSION_TIMEOUT` :

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

Le fichier mis à jour contient la configuration suivante :

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Validation du fichier de configuration

Utilisez la commande `ece-tools` suivante pour valider le fichier de configuration `.magento.env.yaml` avant d’envoyer les modifications vers l’environnement cloud distant.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

L’exemple de réponse suivant fournit une liste d’éléments à corriger :

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## Constantes PHP

Vous pouvez utiliser des constantes PHP dans les définitions de fichiers `.magento.env.yaml` au lieu de valeurs en dur. L&#39;exemple suivant définit le `driver_options` à partir d&#39;une constante PHP :

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>L’analyse constante ne fonctionne pas lors de l’utilisation d’une version de package `symfony/yaml` antérieure à la version 3.2.

## Gestion des erreurs

Lorsqu’un échec se produit en raison d’une valeur inattendue dans le fichier de configuration `.magento.env.yaml`, vous recevez un message d’erreur. Par exemple, le message d’erreur suivant présente une liste de modifications suggérées pour chaque élément avec une valeur inattendue, fournissant parfois des options valides :

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Apportez les corrections nécessaires, validez et poussez les modifications. Si vous ne recevez pas de message d’erreur, les modifications apportées à votre fichier de configuration réussissent la validation.

## Optimisation de la gestion des configurations

Si vous avez activé la gestion de la configuration après avoir vidé les configurations, vous devez déplacer les variables SCD_* du déploiement vers l’étape de création. Voir [Stratégies de déploiement de contenu statique](../deploy/static-content.md).

>Avant la gestion de la configuration :

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>Après avoir activé la gestion de la configuration, déplacez les variables SCD_* vers l’étape de création :

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
