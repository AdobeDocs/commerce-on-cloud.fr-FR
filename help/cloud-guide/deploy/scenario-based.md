---
title: Déploiement basé sur un scénario
description: Découvrez comment personnaliser Adobe Commerce sur les déploiements d’infrastructure cloud à l’aide de fichiers de configuration personnalisés.
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Déploiement basé sur un scénario

Avec `ece-tools` version 2002.1.0 et ultérieure, vous pouvez utiliser la fonctionnalité de déploiement basé sur un scénario pour personnaliser le comportement de déploiement par défaut.
Cette fonctionnalité utilise **scénarios** et **étapes** dans la configuration :

- **Configuration du scénario**-Chaque hook de déploiement est un *scenario*, qui est un fichier de configuration XML qui décrit la séquence et les paramètres de configuration pour terminer les tâches de déploiement. Vous configurez les scénarios dans la section `hooks` du fichier `.magento.app.yaml`.

- **Configuration des étapes**-Chaque scénario utilise une séquence d’*étapes* qui décrivent par programmation les opérations requises pour terminer les tâches de déploiement. Vous configurez les étapes dans un fichier de configuration de scénario XML.

Adobe Commerce sur l’infrastructure cloud fournit un ensemble de [scénarios par défaut](https://github.com/magento/ece-tools/tree/2002.1/scenario) et [étapes par défaut](https://github.com/magento/ece-tools/tree/2002.1/src/Step) dans le package `ece-tools`. Vous pouvez personnaliser le comportement de déploiement en créant des fichiers de configuration XML personnalisés pour remplacer ou personnaliser la configuration par défaut. Vous pouvez également utiliser des scénarios et des étapes pour exécuter du code à partir de modules personnalisés.

## Ajout de scénarios à l’aide de hooks de build et de déploiement

Ajoutez les scénarios de création et de déploiement d’Adobe Commerce à la section `hooks` du fichier `.magento.app.yaml`. Chaque hook spécifie les scénarios à exécuter pendant chaque phase. L’exemple suivant illustre la configuration de scénario par défaut.

> crochets `magento.app.yaml`

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>Avec la version 2002.1.x de `ece-tools`, il existe un nouveau format [configuration des hooks](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html?lang=fr). Le format hérité des versions 2002.0.x de `ece-tools` est toujours pris en charge. Cependant, vous devez effectuer une mise à jour vers le nouveau format pour utiliser la fonctionnalité de déploiement basé sur un scénario.

## Examiner les étapes du scénario

Dans la configuration de hook, chaque scénario est un fichier XML contenant les étapes d’exécution des tâches de build, de déploiement ou de post-déploiement. Par exemple, le fichier `scenario/transfer` comprend trois étapes : `compress-static-content`, `clear-init-directory` et `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Étendre un scénario par défaut

L’exemple suivant étend le scénario de déploiement par défaut en ajoutant des fichiers de configuration de déploiement supplémentaires à la configuration de hook.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Lors du déploiement, les scénarios personnalisés fusionnent avec le scénario par défaut en fonction des règles suivantes :

- Les scénarios sont classés par priorité en fonction de leur séquence dans la définition de hook, le dernier scénario répertorié ayant la priorité la plus élevée.

  Dans l’exemple, les scénarios ont la priorité suivante :

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (scénario par défaut ou scénario de référence)

- Les étapes du scénario avec la priorité la plus élevée remplacent les étapes portant le même nom dans les autres scénarios. De nouvelles étapes sont ajoutées à la configuration. Les mêmes règles s’appliquent à plus de deux scénarios, chaque scénario étant prioritaire de droite à gauche, par exemple (C → B → A).

### Supprimer les étapes par défaut

Pour supprimer des étapes des scénarios par défaut, utilisez le paramètre `skip` .

Par exemple, pour ignorer les étapes `enable-maintenance-mode` et `set-production-mode` dans le scénario de déploiement par défaut, créez un fichier de configuration qui comprend la configuration suivante.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Pour utiliser le fichier de configuration personnalisé, mettez à jour le fichier `.magento.app.yaml` par défaut.

> `.magento.app.yaml` avec le scénario de déploiement personnalisé

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Remplacer les étapes par défaut

Les scénarios personnalisés peuvent remplacer les étapes par défaut pour fournir une implémentation personnalisée. Pour ce faire, utilisez le nom d’étape par défaut comme nom de l’étape personnalisée.

Par exemple, dans le scénario [déploiement par défaut] l’étape `enable-maintenance-mode` exécute le script PHP [EnableMaintenanceMode] par défaut.

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Pour remplacer cette étape, créez un fichier de configuration de scénario personnalisé pour exécuter un script différent lors de l’exécution de l’étape `enable-maintenance-mode`.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Modifier la priorité de l’étape

Les scénarios personnalisés peuvent modifier la priorité des étapes par défaut. L’étape suivante modifie la priorité de l’étape de `enable-maintenance-mode` de `300` en `10` afin que l’étape s’exécute plus tôt dans le scénario de déploiement.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

Dans cet exemple, l’étape `enable-maintenance-mode` est déplacée au début du scénario, car elle a une priorité inférieure à toutes les autres étapes du scénario de déploiement par défaut.

### Exemple : extension du scénario de déploiement

L’exemple suivant personnalise le [scénario de déploiement par défaut] avec les modifications suivantes :

- Remplace l’étape `remove-deploy-failed-flag` par une étape personnalisée
- Ignore la sous-étape `clean-redis-cache` dans l’étape de prédéploiement
- Ignore l’étape `unlock-cron-jobs`
- Ignore l’étape `validate-config` pour désactiver les validateurs critiques
- Ajoute une nouvelle étape de pré-déploiement

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Pour utiliser ce script dans votre projet, ajoutez la configuration suivante au fichier `.magento.app.yaml` pour votre projet d’infrastructure cloud Adobe Commerce :

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>Vous pouvez consulter les [scénarios par défaut](https://github.com/magento/ece-tools/tree/2002.1/scenario) et [configuration de l’étape par défaut](https://github.com/magento/ece-tools/tree/2002.1/src/Step) dans le référentiel GitHub `ece-tools` afin de déterminer les scénarios et les étapes à personnaliser pour vos tâches de création, de déploiement et de post-déploiement de projet.

## Ajout d’un module personnalisé pour étendre les `ece-tools`

Le package `ece-tools` fournit des interfaces API par défaut qui respectent les normes de version sémantique. Toutes les interfaces API sont marquées avec l’annotation **@api**. Vous pouvez remplacer l’implémentation par défaut de l’API par la vôtre en créant un module personnalisé et en modifiant le code par défaut selon vos besoins.

Pour utiliser le module personnalisé avec Adobe Commerce sur une infrastructure cloud, vous devez enregistrer votre module dans la liste des extensions du package `ece-tools`. Le processus d’enregistrement est similaire à celui que vous utilisez pour enregistrer des modules dans Adobe Commerce.

**Pour enregistrer un module avec le package `ece-tools`, procédez comme suit**

1. Créez ou étendez le fichier `registration.php` à la racine de votre module.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Mettez à jour la section `autoload` pour que le fichier de configuration de votre module inclue le fichier `registration.php` pour charger automatiquement les fichiers de module dans `composer.json`.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Ajoutez le fichier `config/services.xml` dans votre module. Cette configuration est fusionnée sur `config/services.xml` à partir `ece-tools` package .

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Pour en savoir plus sur l’injection de dépendance, voir [Injection de dépendance de Symfony](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[scénario de déploiement par défaut]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[Script PHP EnableMaintenanceMode]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
