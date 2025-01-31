---
title: Créer des variables
description: Consultez la liste des variables d’environnement qui contrôlent les actions dans la phase de création d’Adobe Commerce sur l’infrastructure cloud .
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# Créer des variables

Les variables _build_ suivantes contrôlent les actions lors de la phase de build et peuvent hériter et remplacer les valeurs des [variables globales](variables-global.md). Insérez ces variables dans l’étape `build` du fichier `.magento.env.yaml` :

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

Pour plus d’informations sur la personnalisation du processus de création et de déploiement :

- [Configuration du déploiement](configure-env-yaml.md)
- [Processus de déploiement](../deploy/process.md)

Les variables suivantes ont été supprimées dans la version v2.2 :

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **Default**—`1`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Définissez le niveau d’imbrication des répertoires pour l’enregistrement des fichiers de rapport d’erreur afin d’éviter de remplir le répertoire de rapports avec des dizaines de milliers de fichiers, ce qui peut rendre difficile la gestion et la révision des données. Ce paramètre est défini par défaut sur `1`. En règle générale, il n’est pas nécessaire de modifier la valeur par défaut, sauf si vous rencontrez des problèmes lors de la gestion des fichiers de rapport d’erreur dans le répertoire `<magento_root>/var/report/`.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Spécifiez une liste de correctifs de qualité Adobe Commerce à appliquer pendant le déploiement.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

L’exemple suivant spécifie trois correctifs à appliquer pendant le déploiement.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

Voir [ Application de correctifs](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **Default**—`6`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Indique le niveau de compression [gzip](https://www.gnu.org/software/gzip) (`0` à `9`) à utiliser lors de la compression du contenu statique ; `0` désactive la compression.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **Default**—`600`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Lorsque le temps nécessaire à la compression des ressources statiques dépasse le délai d’expiration de la compression, le processus de déploiement est interrompu. Définissez la durée d’exécution maximale, en secondes, de la commande de compression de contenu statique.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **Default**—`false`
- **Version**—Adobe Commerce 2.4.2 et versions ultérieures

Définissez-le sur `true` pour éviter de générer du contenu statique pour les thèmes parents pendant la phase de création.

Définissez `SCD_NO_PARENT: false` pendant la phase de création de sorte que la génération de contenu statique pour les thèmes parents n’ait pas d’incidence sur le déploiement du site et ne provoque pas d’interruption inutile du site. Voir [Déploiement de contenu statique](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Vous pouvez configurer plusieurs paramètres régionaux par thème. Cette personnalisation permet d’accélérer le processus de création en réduisant le nombre de fichiers de thème inutiles. Par exemple, vous pouvez créer le thème _magento/backend_ en anglais et un thème personnalisé dans d’autres langues.

L’exemple suivant crée le thème `Magento/backend` avec trois paramètres régionaux :

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

L’exemple suivant crée trois thèmes avec trois paramètres régionaux :

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Vous pouvez également choisir de ne _pas_ déployer un thème :

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Permet d’augmenter le temps d’exécution maximal attendu pour le déploiement de contenu statique.

Par défaut, Adobe Commerce sur l’infrastructure cloud définit l’exécution maximale prévue sur 900 secondes, mais dans certains scénarios, vous aurez peut-être besoin de plus de temps pour terminer le déploiement du contenu statique pour un projet cloud.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **Default**—`quick`
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Personnalisez la [stratégie de déploiement](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) pour le contenu statique. Voir [ Déploiement de fichiers de vue statiques ](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Utilisez ces options _uniquement_ si vous disposez de plusieurs paramètres régionaux :

- `standard` : déploie tous les fichiers de vue statiques pour tous les packages.
- `quick`—(_par défaut_) réduit le temps de déploiement.
- `compact` : permet de conserver de l&#39;espace disque sur le serveur. Dans Adobe Commerce version 2.2.4 et antérieure, ce paramètre remplace la valeur de `scd_threads` par une valeur de `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Default**—Automatique
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Définit le nombre de threads pour le déploiement de contenu statique. La valeur par défaut est définie en fonction du nombre de threads CPU détectés et ne dépasse pas une valeur de 4. L’augmentation du nombre de threads accélère le déploiement de contenu statique ; la diminution du nombre de threads le ralentit. Vous pouvez définir la valeur du thread, par exemple :

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Pour réduire davantage le temps de déploiement, utilisez [Gestion de la configuration](../store/store-settings.md) avec la commande `scd-dump` pour déplacer le déploiement statique vers la phase de création.

## `SCD_USE_BALER`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.3.0 et versions ultérieures

[Baler](https://github.com/magento/baler) analyse le code JavaScript généré et crée un lot JavaScript optimisé. Le déploiement du lot optimisé sur votre site peut réduire le nombre de requêtes réseau lors du chargement de votre site et améliorer les temps de chargement des pages.

Définissez sur `true` pour exécuter la presse après avoir effectué un déploiement de contenu statique.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Comme Baler est en version Alpha, il n’est pas recommandé de l’utiliser dans les environnements de production.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Par défaut**— _Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Définissez sur `true` pour ignorer la commande `composer dump-autoload` lors de l’installation de Cloud Docker. Cette variable ne concerne que les conteneurs Cloud Docker dotés de systèmes de fichiers accessibles en écriture. Dans ce cas, ignorer la commande empêche les erreurs d’autres commandes qui tentent d’accéder au code du répertoire de `generated` supprimé.

Lorsqu’Adobe Commerce s’exécute `composer dump-autoload`, il crée des fichiers de chargement automatique avec des liens vers des classes générées dans le dossier `generated`, ce qui ne pose pas de problème dans les environnements de production dotés de systèmes de fichiers en lecture seule. Cependant, pour les installations Cloud Docker avec des systèmes de fichiers accessibles en écriture (créés uniquement pour les tests et le développement à l’aide de `./vendor/bin/ece-docker build:compose --with-test`), vous pouvez exécuter la commande `bin/magento -n setup:upgrade` sans l’option `--keep-generated` , qui supprime le répertoire `generated`. Si le répertoire est supprimé, la commande `composer dump-autoload` échoue, car le chargement automatique contient des liens vers des fichiers du répertoire supprimé.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Par défaut**— _Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Définissez sur `true` pour ignorer le déploiement du contenu statique pendant la phase de création.

Si vous déployez déjà du contenu statique pendant la phase de création avec [Gestion de la configuration](../store/store-settings.md), vous pouvez ignorer le déploiement du contenu statique pour un test de création rapide.

Lors de la phase de création, définissez `SKIP_SCD: false` afin que la création de contenu statique se produise au cours de la phase de création, où le processus n’a aucune incidence sur le déploiement du site et ne provoque pas d’interruption inutile du site. Voir [Déploiement de contenu statique](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Activez ou désactivez le niveau de détail de débogage [Symfony](https://symfony.com/doc/current/console/verbosity.html) pour `bin/magento` commandes d’interface de ligne de commande exécutées lors de la phase de déploiement.

>[!NOTE]
>
>`bin/magento` Pour utiliser VERBOSE_COMMANDS afin de contrôler le détail dans la sortie de commande pour les commandes CLI réussies et échouées, vous devez définir [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Choisissez le niveau de détail fourni dans les logs :

- `-v`= sortie normale
- `-vv`= sortie plus détaillée
- `-vvv` = sortie détaillée idéale pour le débogage

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
