---
title: Variables globales
description: Consultez la liste des variables d’environnement qui contrôlent les actions dans le processus de déploiement d’Adobe Commerce sur les infrastructures cloud .
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Variables globales

Les variables globales contrôlent les actions à chaque phase du processus de déploiement [!DNL Commerce] : création, déploiement et post-déploiement. Les variables globales ayant un impact sur chaque phase, vous devez les définir à l’étape `global` du fichier `.magento.env.yaml` :

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

Pour plus d’informations sur la personnalisation du processus de création et de déploiement :

- [Configuration du déploiement](configure-env-yaml.md)
- [Processus de déploiement](../deploy/process.md)

## `ENABLE_EVENTING`

- **Default**-_Not set_
- **Version**—Adobe Commerce 2.4.5 et versions ultérieures

Lorsque la valeur est définie sur `true`, permet à cron d’exécuter les consommateurs de file d’attente de messages. Adobe I/O Events pour Adobe Commerce utilise les files d’attente de messages pour accélérer la diffusion des événements critiques.

Adobe vous recommande également d’ajouter la variable [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) à l’étape `deploy` du fichier `.magento.env.yaml` avec `cron_run` définie sur `true`.

L’exemple suivant illustre une variable `ENABLE_EVENTING` entièrement configurée.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **Default**-_Not set_
- **Version**—Adobe Commerce 2.4.4 et versions ultérieures

Lorsqu’il est défini sur `true`, active les Webhooks Commerce. Le webhook s’exécute sur un point d’entrée externe, tel qu’une action d’exécution App Builder ou un système de gestion des stocks tiers. Le [_guide des Webhooks_](https://developer.adobe.com/commerce/extensibility/webhooks) décrit cette fonctionnalité en détail.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Il remplace le niveau de journalisation minimal pour tous les flux de sortie sans modifier le code, ce qui s’avère utile lors de la résolution des problèmes de déploiement. Par exemple, si votre déploiement échoue, vous pouvez utiliser cette variable pour augmenter la granularité de la journalisation au niveau mondial. Voir [Niveaux de journal](log-handlers.md#log-levels). La valeur `min_level` dans Gestionnaires de journalisation remplace ce paramètre.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>Le paramètre de la variable `MIN_LOGGING_LEVEL` ne modifie pas la configuration du niveau de journal pour le gestionnaire de fichiers, qui est défini sur `debug` par défaut.

## `SCD_ON_DEMAND`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Activez la génération de contenu statique lorsqu’un utilisateur ou une utilisatrice (SCD) le demande. Le contenu statique à la demande est idéal pour le workflow de développement et de test, car il réduit le temps de déploiement.

Le préchargement du cache à l’aide du hook [`post_deploy` réduit &#x200B;](../application/hooks-property.md) temps d’arrêt du site. Le réchauffement du cache n’est disponible que pour les projets Pro contenant des environnements d’évaluation et de production dans les projets [!DNL Cloud Console] et de démarrage. Ajoutez la variable d’environnement `SCD_ON_DEMAND` à l’étape `global` dans le fichier `.magento.env.yaml` :

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

La variable `SCD_ON_DEMAND` ignore le SCD lors des deux phases (création et déploiement), efface les dossiers `pub/static` et `var/view_preprocessed` et écrit les éléments suivants dans le fichier `app/etc/env.php` :

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.2.0 et versions ultérieures

Permet d’augmenter le temps d’exécution maximal attendu pour le déploiement de contenu statique.

Par défaut, Adobe Commerce définit l’exécution maximale prévue sur 900 secondes, mais dans certains scénarios, vous aurez peut-être besoin de plus de temps pour terminer le déploiement du contenu statique pour un projet cloud.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.4.2 et versions ultérieures

Définissez cette variable sur `true` afin d’empêcher la génération de contenu statique pour les thèmes parents pendant les phases de création et de déploiement. Lorsque cette option est définie sur `true`, moins de contenu statique est généré, ce qui améliore votre temps de création et de déploiement global.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.3.0 et versions ultérieures

[Baler](https://github.com/magento/baler) est un module qui analyse le code JavaScript généré et crée un lot JavaScript optimisé. Le déploiement du lot optimisé sur votre site peut réduire le nombre de requêtes réseau lors du chargement de votre site et améliorer les temps de chargement des pages.

Définissez sur `true` pour exécuter la presse après avoir effectué un déploiement de contenu statique.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Installez et configurez le module Baler avant d’utiliser cette fonctionnalité. Comme Baler est en version Alpha, activez cette option uniquement dans les environnements d’évaluation.

## `SKIP_HTML_MINIFICATION`

- **Par défaut** :
   - `true` : pour les versions `ece-tools` 2002.0.13 et ultérieures
   - `false` : pour les versions antérieures d’`ece-tools`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Active ou désactive la copie de fichiers de vue statiques dans le répertoire `<magento_root>/init/` à la fin de l&#39;étape de création. Si le paramètre est défini sur `true`, les fichiers ne sont pas copiés et une minimisation d’HTML est disponible sur demande. Définissez cette valeur sur `true` afin de réduire les temps d’arrêt lors du déploiement dans les environnements d’évaluation et de production.

- **`false`** : copie le répertoire `view_preprocessed` dans le répertoire `<magento_root>/init/` à la fin de la phase de création et restaure le répertoire dans le répertoire `<magento_root>/var` au début de la phase de déploiement.
- **`true`** : permet la minimisation de l&#39;HTML à la demande ; ne copie _pas_ le `<magento_root>var/view_preprocessed` dans le répertoire `<magento_root>/init/` à l&#39;issue de la phase de création.

Ajoutez la variable d’environnement `SKIP_HTML_MINIFICATION` à l’étape `global` dans le fichier `.magento.env.yaml` :

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Utilisez la variable `X_FRAME_CONFIGURATION` pour modifier la configuration de l’en-tête [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html?lang=fr) de votre site Adobe Commerce. Cette configuration contrôle la manière dont le navigateur effectue le rendu d’une page dans une `<frame>`, un `<iframe>` ou un `<object>`. Utilisez l’une des options suivantes :

- `DENY` : la page ne peut pas être affichée dans un cadre.
- `SAMEORIGIN` : (paramètre Adobe Commerce par défaut). La page ne peut être affichée que dans un cadre de la même origine que la page elle-même.

>[!WARNING]
>
>L’option `ALLOW-FROM <uri>` est obsolète, car les navigateurs pris en charge par Adobe Commerce ne la prennent plus en charge. Voir [Compatibilité navigateur](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility).

Ajoutez la variable d’environnement `X_FRAME_CONFIGURATION` à l’étape `global` dans le fichier `.magento.env.yaml` :

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
