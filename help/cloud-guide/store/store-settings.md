---
title: Gestion de la configuration de la boutique
description: Découvrez comment gérer et synchroniser les paramètres de configuration des magasins dans tous les environnements Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Gestion de la configuration de la boutique

Les configurations par défaut de votre magasin sont stockées dans un `config.xml` pour le module approprié. Lorsque vous modifiez les paramètres dans Commerce Admin ou la commande CLI `bin/magento config:set`, les modifications sont répercutées dans la base de données principale, en particulier dans le tableau `core_config_data`. Ces paramètres remplacent les configurations par défaut stockées dans le fichier `config.xml`.

Les paramètres de la boutique, qui font référence aux configurations de la section Admin **Magasins** > **Paramètres** > **Configuration**, sont stockés dans les fichiers de configuration du déploiement en fonction du type de configuration :

- `app/etc/config.php` : paramètres de configuration pour les magasins, les sites web, les modules ou les extensions, l’optimisation des fichiers statiques et les valeurs système liées au déploiement de contenu statique. Voir la référence [config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html?lang=fr) dans le _Guide de configuration_.
- `app/etc/env.php` : valeurs pour les remplacements spécifiques au système et les paramètres sensibles qui ne doivent _PAS_ être stockés dans le contrôle de code source. Voir la [référence env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=fr) dans le _Guide de configuration_.

>[!NOTE]
>
>Comme Adobe Commerce sur les infrastructures cloud ne prend en charge que les modes de production et de maintenance, la section **Avancé** > **Développeur** n’est pas accessible dans l’administration. Vous devez disposer des [privilèges d’administration d’environnement](../project/user-access.md) pour effectuer les tâches de gestion de la configuration. Vous pouvez configurer des paramètres supplémentaires à l’aide de [variables d’environnement](../environment/configure-env-yaml.md).

La gestion de la configuration permet de déployer des paramètres de magasin cohérents dans vos environnements avec un temps d’arrêt minimal à l’aide du déploiement du pipeline. Le projet d’infrastructure d’Adobe Commerce sur le cloud comprend le serveur de génération, les scripts de génération et de déploiement, ainsi que les environnements de déploiement conçus en tenant compte de la [&#x200B; stratégie de déploiement du pipeline &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html?lang=fr).

## Schéma de remplacement de configuration

Toutes les configurations système sont définies au cours des phases de création et de déploiement selon le schéma de remplacement suivant :

1. S’il existe une variable d’environnement , utilisez la configuration personnalisée et ignorez la configuration par défaut.
1. Si une variable d’environnement n’existe pas, utilisez la configuration à partir d’une paire nom-valeur `MAGENTO_CLOUD_RELATIONSHIPS` dans le fichier [`.magento.app.yaml`](../application/configure-app-yaml.md). Ignorez la configuration par défaut.
1. Si une variable d’environnement n’existe pas et `MAGENTO_CLOUD_RELATIONSHIPS` ne contient pas de paire nom-valeur, supprimez toute configuration personnalisée et utilisez les valeurs de la configuration par défaut.

En résumé, les variables d’environnement remplacent toutes les autres valeurs.

>[!TIP]
>
>Voir [Gestion de la configuration](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html?lang=fr) dans le _Guide de configuration_ pour en savoir plus sur le schéma de remplacement pour le déploiement du pipeline.

Si le même paramètre est configuré à plusieurs endroits, l’application s’appuie sur la hiérarchie de configuration suivante pour déterminer la valeur à appliquer à l’environnement :

| Priorité | Configuration<br>Method | Description |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>variables d’environnement | Valeurs ajoutées à partir de l’onglet _Variables_ de la configuration de l’environnement dans le [!DNL Cloud Console]. Spécifiez ici des valeurs pour les configurations sensibles ou spécifiques à un environnement. Les paramètres spécifiés ici ne peuvent pas être modifiés à partir de l’administrateur. Voir [Variables de configuration d’environnement](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Valeurs ajoutées dans la section `variables` du fichier `.magento.app.yaml`. Spécifiez les valeurs ici pour garantir une configuration cohérente dans tous les environnements. **Ne spécifiez pas de valeurs sensibles dans le fichier `.magento.app.yaml`.** Voir [Paramètres de l’application](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | Les valeurs de configuration spécifiques à un environnement stockées ici sont ajoutées à l’aide de la commande `app:config:dump`. Définissez les valeurs sensibles et spécifiques au système à l’aide de variables d’environnement ou de l’interface de ligne de commande. Voir [&#x200B; Données sensibles &#x200B;](#sensitive-data). Le fichier `env.php` n **est pas inclus** contrôle de code source. |
| 4 | `app/etc/config.php` | Les valeurs stockées ici sont ajoutées à l’aide de la commande `app:config:dump`. Les valeurs de configuration partagées sont ajoutées aux `config.php`. Définissez la configuration partagée depuis l’interface de ligne de commande de l’administrateur ou en utilisant l’interface de ligne de commande. Le fichier `config.php` est inclus dans le contrôle de code source. |
| 5 | Base de données | Les valeurs stockées ici sont ajoutées en définissant des configurations dans l’Admin. Les configurations définies à l’aide de l’une des méthodes précédentes sont verrouillées (grisées) et ne peuvent pas être modifiées à partir de l’Administration. |
| 6 | `config.xml` | De nombreuses configurations comportent des valeurs par défaut définies dans le fichier `config.xml` d’un module. Si Adobe Commerce ne trouve aucune valeur définie par l’une des méthodes précédentes, il revient à la valeur par défaut, si elle est définie. |

{style="table-layout:auto"}

## Image mémoire de la configuration

Vous pouvez utiliser la commande `ece-tools` suivante pour générer un fichier `config.php` contenant toutes les configurations de magasin actuelles :

```bash
./vendor/bin/ece-tools config:dump
```

Les données « vidées » dans le fichier `app/etc/config.php` deviennent _verrouillées_, ce qui signifie que le champ correspondant dans l’administrateur Commerce devient **lecture seule**. Le fichier `config.php` comprend uniquement les paramètres que vous configurez. Les valeurs par défaut ne sont pas verrouillées. Le verrouillage uniquement des valeurs que vous mettez à jour garantit également que toutes les extensions utilisées dans les environnements d’évaluation et de production ne sont pas rompues en raison de configurations en lecture seule, en particulier Fastly.

>[!WARNING]
>
>La commande `ece-tools config:dump` ne récupère pas les configurations détaillées des modules, tels que B2B. Si vous avez besoin d’une image mémoire de configuration complète, utilisez la commande `app:config:dump`, mais cette commande verrouille les valeurs de configuration en lecture seule.

### Données sensibles

Toutes les configurations sensibles sont exportées dans le fichier `app/etc/env.php` lorsque vous utilisez la commande `bin/magento app:config:dump`. Vous pouvez définir des valeurs sensibles à l’aide de la commande CLI : `bin/magento config:sensitive:set`. Voir [Paramètres sensibles et spécifiques à un environnement](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) dans le guide _Extensions PHP Commerce_ pour savoir comment désigner les paramètres de configuration comme étant sensibles ou spécifiques à un système.

Consultez une liste des [paramètres sensibles ou spécifiques au système](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html?lang=fr) dans le _guide de configuration_.

### Performances SCD

Selon la taille de votre boutique, vous pouvez avoir un grand nombre de fichiers de contenu statique à déployer. Normalement, le contenu statique se déploie pendant la phase de déploiement lorsque l’application est en mode de maintenance. La configuration la plus optimale consiste à générer du contenu statique pendant la phase de création. Voir [&#x200B; Choix d’une stratégie de déploiement](../deploy/static-content.md).

Si vous avez activé la gestion de la configuration après avoir vidé les configurations, vous devez déplacer les variables SCD_* de l’étape de déploiement à l’étape de création afin d’activer correctement la génération de contenu statique pendant la phase de création. Voir [&#x200B; Variables d’environnement &#x200B;](../environment/configure-env-yaml.md#environment-variables).

**Avant la gestion de la configuration** :

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

**Après avoir activé la gestion de la configuration** :

Déplacez les variables SCD_* vers l’étape de création :

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

>[!NOTE]
>
>Avant de déployer des fichiers statiques, les phases de création et de déploiement compressent le contenu statique à l’aide de GZIP. La compression de fichiers statiques réduit la charge du serveur et améliore les performances du site. Voir [Options de création](../environment/variables-build.md) pour en savoir plus sur la personnalisation ou la désactivation de la compression de fichier.

## Procédure de gestion des paramètres

Voici un aperçu général de ce processus :

![Présentation de la gestion de la configuration de démarrage](../../assets/starter/configuration-management-flow.png)

**Pour configurer votre magasin et générer un fichier de configuration** :

1. Effectuez toutes les configurations pour vos magasins dans l’Admin pour l’un des environnements :

   - Démarreur : branche de développement active
   - Pro : branche active de l’environnement d’intégration

   Ces configurations n’incluent pas les produits réels, sauf si vous prévoyez de vider la base de données de cet environnement vers des environnements d’évaluation et de production. En règle générale, les bases de données de développement n’incluent pas l’ensemble des données de votre magasin.

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Créez une image mémoire locale de la base de données distante.

   ```bash
   magento-cloud db:dump
   ```

1. Ajouter, valider et intégrer des modifications de code pour mettre à jour un environnement distant.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

Une fois le déploiement terminé, connectez-vous à l’administration de l’environnement mis à jour pour vérifier les paramètres. Continuez à fusionner les configurations supplémentaires dans les environnements d’évaluation et de production, si nécessaire.

### Mettre à jour les configurations

Lorsque vous modifiez votre environnement par l’intermédiaire de l’Administration et exécutez à nouveau la commande, de nouvelles configurations sont ajoutées au code dans le fichier `config.php`.

>[!WARNING]
>
>Bien que vous puissiez modifier manuellement le fichier `config.php` dans les environnements d’évaluation et de production, il n’est **pas** recommandé. Le fichier permet de conserver la cohérence de toutes les configurations dans tous les environnements. Ne supprimez jamais le fichier `config.php` pour le recréer. La suppression du fichier peut supprimer des configurations et des paramètres spécifiques requis pour les processus de génération et de déploiement.

### Restaurer des fichiers de configuration

Des copies des fichiers `app/etc/env.php` et `app/etc/config.php` d’origine ont été créées pendant le processus de déploiement et stockées dans le même dossier. Vous trouverez ci-dessous les fichiers BAK (fichiers de sauvegarde) et PHP (fichiers d&#39;origine) dans le même dossier `app/etc` :

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

Les anciennes configurations utilisaient le fichier `app/etc/config.local.php`. Voir [&#x200B; Migration des anciennes configurations &#x200B;](#migrate-older-configurations).

**Pour restaurer des fichiers de configuration** :

1. Sur votre station de travail locale, utilisez SSH pour vous connecter au projet et à l’environnement distants.

   ```bash
   magento-cloud ssh
   ```

1. Vérifiez l’emplacement et la disponibilité des fichiers de sauvegarde.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Exemple de réponse :

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Restaurez les fichiers de sauvegarde.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migration des configurations plus anciennes

Si vous effectuez une mise à niveau vers Adobe Commerce sur une infrastructure cloud 2.2 ou ultérieure, vous pouvez migrer les paramètres du fichier `config.local.php` vers votre nouveau fichier `config.php`. Si les paramètres de configuration de votre administrateur correspondent au contenu du fichier, suivez les instructions pour générer et ajouter le fichier `config.php`.

S’ils sont différents, vous pouvez ajouter du contenu du fichier `config.local.php` à votre nouveau fichier `config.php` :

1. Suivez les instructions pour générer le fichier `config.php`.

1. Ouvrez le fichier `config.php` et supprimez la dernière ligne.

1. Ouvrez le fichier `config.local.php` et copiez le contenu.

1. Collez le contenu dans le fichier `config.php`, enregistrez-le et terminez de l’ajouter à Git.

1. Effectuez un déploiement dans vos environnements.

Vous ne terminez cette migration qu’une seule fois. Après la migration, utilisez le fichier `config.php`.

### Modifier les paramètres régionaux

Vous pouvez modifier les paramètres régionaux de votre boutique sans suivre un processus complexe d’importation et d’exportation de configurations, _si_ vous avez activé [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand). Vous pouvez mettre à jour les paramètres régionaux à l’aide de l’Administration.

Vous pouvez ajouter d’autres paramètres régionaux à l’environnement d’évaluation ou de production en activant `SCD_ON_DEMAND` dans une branche d’intégration, générer un fichier `config.php` mis à jour avec les nouvelles informations de paramètres régionaux et copier le fichier de configuration dans l’environnement cible.

>[!WARNING]
>
>Ce processus **remplace** la configuration du magasin ; ne procédez comme suit que si les environnements contiennent les mêmes magasins.

1. Dans l’environnement d’intégration, activez la variable `SCD_ON_DEMAND` à l’aide du fichier [`.magento.env.yaml`](../environment/configure-env-yaml.md).

1. Ajoutez les paramètres régionaux nécessaires à l’aide de votre administrateur.

1. Utilisez SSH pour vous connecter à l’environnement distant et générer le fichier `/app/etc/config.php` contenant tous les paramètres régionaux.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Copiez le nouveau fichier de configuration de l’environnement d’intégration distant dans votre répertoire d’environnement local.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Ajouter, valider et intégrer des modifications de code pour mettre à jour un environnement distant.
