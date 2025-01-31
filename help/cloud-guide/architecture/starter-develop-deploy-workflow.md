---
title: Workflow du projet de démarrage
description: Découvrez comment utiliser les workflows de développement et de déploiement de Starter.
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '2103'
ht-degree: 0%

---

# Workflow du projet de démarrage

L’infrastructure d’Adobe Commerce sur le cloud comprend un référentiel Git unique avec une branche `master` pour l’environnement de production, qui peut être ramifiée afin de créer un environnement d’évaluation et plusieurs environnements d’intégration pour le travail de test et de développement. Vous pouvez avoir jusqu’à quatre environnements actifs, y compris un environnement `master` pour votre serveur de production. Consultez [Architecture de démarrage](starter-architecture.md) pour une présentation.

Pour vos environnements, suivez le workflow [!UICONTROL Development > Staging > Production] pour développer et déployer votre site.

- **Environnement de production (site actif)**—Fournit un environnement de production complet avec tous les services créés et déployés à partir du code sur la branche `master`.
- **Environnement d&#39;évaluation** : fournit un environnement d&#39;évaluation complet qui correspond à l&#39;environnement de production avec tous les services créés et déployés à partir d&#39;une branche `staging` que vous créez en clonant à partir d&#39;`master`.
- **Environnements d&#39;intégration** : fournit jusqu&#39;à deux environnements de développement actifs que vous créez à partir de la branche `staging`. L’environnement `integration` ne prend pas en charge les services tiers tels que Fastly et New Relic.

Pour vos branches, vous pouvez suivre n’importe quelle méthodologie de développement. Par exemple, vous pouvez suivre une méthodologie Agile telle que Scrum pour créer des branches pour chaque sprint.

À partir de chaque sprint, vous pouvez créer des branches pour chaque histoire d’utilisateur. Toutes les histoires deviennent testables. Vous pouvez fusionner en continu dans la branche de sprint et valider cette branche en continu. Lorsque le sprint se termine, vous pouvez fusionner la branche du sprint pour `master` à déployer toutes les modifications du sprint en production sans avoir à gérer un goulot d’étranglement de test.

## Workflow de développement

Le développement et le déploiement des plans de démarrage commencent avec votre projet initial. Vous créez votre projet avec le « site vierge », qui est un référentiel de code de modèle d’infrastructure cloud d’Adobe Commerce avec un magasin entièrement préparé. Vous créez ainsi une branche `master` avec une copie du code de votre environnement de production.

Le workflow de développement comprend les éléments suivants :

- [Clonage et branche](#clone-and-branch) à partir du `master` pour créer des branches de `staging` et de développement
- [Développement de code](#develop-code) et installation locale d’extensions dans une branche de développement, y compris les mises à jour [!DNL Composer]
- [Configurer](#configure-store) vos paramètres de magasin et d’extension
- [Générer la configuration](#generate-configuration-management-files) les fichiers de gestion
- [Code push](#push-code-and-test) et configuration à créer et déployer dans les environnements `staging` et `production`

![Développement et déploiement d’un workflow](../../assets/starter/workflow.png)

Vous disposez également de quelques étapes facultatives pour développer et tester votre code et les données de votre boutique :

- [Installer des données d’exemple](#optional-install-sample-data) dans votre magasin
- [Extraction des données du magasin de production](#optional-pull-production-data) vers les environnements

Ce processus suppose que vous avez configuré votre [espace de travail de développeur local](../development/overview.md).

### Clonage et branche

Pour un nouveau projet de plan de démarrage, une branche `master` a été clonée à partir du référentiel Git d’Adobe Commerce sur l’infrastructure cloud. Pour commencer à créer des branches et à utiliser du code, clonez la branche `master` à votre environnement local.

Le format de la commande du clone Git est le suivant :

```bash
git fetch origin
```

```bash
git pull origin <environment-ID>
```

La première fois que vous commencez à travailler dans des branches pour votre projet de démarrage, créez une branche `staging`. Cela crée une branche de code correspondant à la branche `master` qui se déploie vers un environnement d’évaluation pour tester la configuration et les modifications de code avant le déploiement dans l’environnement de production.

Créez ensuite des branches à partir de `staging` pour développer du code, ajouter des extensions et configurer des intégrations tierces. Chaque fois que vous développez du code personnalisé, ajoutez des extensions, intégrez un service tiers, travaillez dans une branche de développement créée à partir de la branche `staging`. Quatre environnements d’intégration actifs sont disponibles. Lorsque vous poussez une branche active, l’un de ces environnements d’intégration déploie automatiquement votre code pour le tester.

Le format de la commande de la branche Git est le suivant :

```bash
git checkout <branch-name>
```

Le format de la commande Cloud CLI `branch` est le suivant :

```bash
magento-cloud environment:branch <environment-name> <parent-environment-ID>
```

![Branche du Principal ](../../assets/starter/branching.png)

### Développer du code

À l’aide de la branche de base d’Adobe Commerce sur le code d’infrastructure cloud, vous pouvez commencer à installer des extensions, à développer du code personnalisé, à ajouter des thèmes, etc.

Utilisez une stratégie d’embranchement avec votre travail de développement. L’utilisation d’une seule branche pour effectuer tout votre travail en même temps peut rendre les tests difficiles. Par exemple, vous pouvez suivre les méthodologies d’intégration continue et de sprint pour fonctionner :

- Ajoutez quelques extensions et configurez-les avec votre première branche
- Envoyez ce code, testez et fusionnez-le à l’évaluation, puis à la production
- Configurez entièrement vos services dans `services.yaml` et ajoutez un thème
- Envoyez ce code, testez et fusionnez-le à l’évaluation, puis à la production
- Intégration à un service tiers
- Envoyez ce code, testez et fusionnez-le à l’évaluation, puis à la production

Jusqu’à ce que votre boutique soit entièrement créée, configurée et prête à être lancée. Mais continuez à lire, il existe de nombreuses options pour la configuration de votre magasin et de votre code.

>[!NOTE]
>
>Aucune configuration n&#39;a encore été effectuée sur votre station de travail locale.

![Code push depuis le local](../../assets/starter/push-code.png)

### Configurer le magasin

Lorsque vous êtes prêt à configurer votre boutique, envoyez l’ensemble de votre code vers l’environnement `integration`. Configurez les paramètres de votre magasin à partir de l’Administration pour l’environnement d’intégration, et non dans votre environnement local. Pour trouver l’URL, cliquez sur **Accéder au site** dans le [!DNL Cloud Console]

Pour obtenir de meilleures informations sur les configurations, consultez la documentation relative à Adobe Commerce et aux extensions installées. Voici quelques liens et idées pour commencer :

- [Bonnes pratiques relatives à la configuration du magasin](../store/best-practices.md) pour connaître les bonnes pratiques spécifiques en matière de cloud
- [Configuration de base](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/store-details) pour l’accès administrateur de boutique, le nom, les langues, les devises, le branding, les sites, les vues de boutique, etc
- [Thème](https://experienceleague.adobe.com/en/docs/commerce-admin/content-design/content-menu#design-features) pour l’aspect du site et des magasins, y compris les CSS et les mises en page
- [Configuration du système](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/guide-overview) pour les rôles, les outils, les notifications et votre clé de chiffrement pour la base de données
- Paramètres d’extension utilisant leur documentation

Outre les paramètres de la boutique, vous pouvez configurer plusieurs sites et boutiques, services configurés, etc. Voir [ Configuration de votre boutique ](../store/overview.md).

### Générer des fichiers de gestion de la configuration

Si vous connaissez Adobe Commerce, vous vous demandez peut-être comment obtenir vos paramètres de configuration de votre base de données en développement vers les environnements d’évaluation et de production. Auparavant, vous deviez copier tous vos paramètres de configuration sur papier ou dans un fichier, puis appliquer manuellement les paramètres à d’autres environnements. Vous avez peut-être vidé votre base de données et envoyé ces données vers un autre environnement.

Adobe Commerce sur l’infrastructure cloud fournit un ensemble de deux commandes [Gestion de la configuration](../store/store-settings.md) qui exportent les paramètres de configuration de votre environnement dans un fichier. Ces commandes ne sont disponibles que pour **Adobe Commerce sur Cloud Infrastructure 2.2 et versions ultérieures**.

- `php .vendor/bin/ece-tools config:dump` : exporte uniquement les paramètres de configuration que vous avez saisis ou modifiés à partir des valeurs par défaut dans un fichier de configuration. _Recommandé_.
- `php bin/magento app:config:dump` : exporte chaque paramètre de configuration, y compris les paramètres modifiés et par défaut, dans un fichier de configuration.

Le fichier généré est `app/etc/config.php`.

Générez le fichier dans l’environnement d’intégration où vous avez configuré Adobe Commerce. Suivez les étapes du processus de génération du fichier, d’ajout à votre branche et de déploiement.

**Remarques importantes** sur la gestion de la configuration :

- Tout paramètre de configuration inclus dans le fichier généré à partir de la commande `app:config:dump` est verrouillé contre la modification ou en lecture seule dans l’environnement déployé. C’est l’une des raisons pour lesquelles Adobe recommande d’utiliser la commande `.vendor/bin/ece-tools config:dump`.

  Par exemple, vous installez un module pour Fastly dans votre environnement de développement. Vous ne pouvez configurer ce module que dans l’environnement d’évaluation et de production. Grâce à la commande `.vendor/bin/ece-tools config:dump`, ces champs par défaut restent modifiables lorsque vous déployez vos modifications de développement dans l’environnement d’évaluation et de production.

- Le fichier généré peut être long selon la taille de votre déploiement . La commande `.vendor/bin/ece-tools config:dump` génère un fichier plus petit que le fichier généré par la commande `app:config:dump`.

Si vous utilisez Adobe Commerce version 2.2 ou ultérieure, les commandes de gestion de la configuration fournissent une fonctionnalité supplémentaire pour protéger les données sensibles, telles que les informations d’identification de sandbox pour un module PayPal. Pendant le processus d’exportation, toutes les valeurs contenant des données sensibles sont exportées dans un fichier de configuration distinct, `env.php` dans le répertoire `app/etc/`. Ce fichier reste dans votre environnement local et n’est pas copié lorsque vous poussez votre code vers une autre branche. Vous pouvez également créer des variables d’environnement avec des commandes d’interface de ligne de commande dans toutes les versions d’Adobe Commerce sur le cloud.

![Génération des variables d’environnement](../../assets/starter/env-variables.png)

Voir [Gestion de la configuration](../store/store-settings.md).

### Code push et test

À ce stade, vous devez disposer d’une branche de code développée avec un fichier de configuration (`config.local.php` ou `config.php`) prêt à être testé.

Chaque fois que vous envoyez du code depuis votre environnement local, une série de scripts de build et de déploiement s’exécutent. Ces scripts génèrent un nouveau code et le déploient dans l’environnement distant. Par exemple, si vous poussez une branche de développement de votre environnement local vers la branche distante, un environnement correspondant met à jour les services, le code et le contenu statique.

Vous pouvez accéder directement à cet environnement avec une URL de magasin, une URL d’administration et un SSH. Ces environnements comprennent un serveur web, une base de données et des services configurés. Une fois prêt, vous pouvez commencer le déploiement et le test dans l’environnement d’évaluation.

Pour plus d’informations, voir [Workflow de déploiement](#deployment-workflow).

### Facultatif : installez des données d’exemple.

Si vous avez besoin de données d’exemple lors du développement de votre boutique, vous pouvez installer des données d’exemple. Ces données simulent un magasin actif, y compris les clients, les produits et d’autres données. Ces exemples de données fonctionnent mieux avec une installation de modèle d’infrastructure cloud « site vierge » d’Adobe Commerce lors de la création de votre projet. Il est recommandé de supprimer les exemples de données avant la mise en ligne. Voir [Installation de données d’exemple facultatives](../test/sample-data.md).

![Installer des exemples de données facultatifs](../../assets/starter/sample-data.png)

### Facultatif : extraction des données de production

Ajoutez tous vos produits, catalogues, contenu de site, etc. directement à l’environnement de `production`. En ajoutant ces données à l’environnement de production, vous pouvez fournir des prix, des coupons, des stocks, des annonces de ventes mis à jour, des informations sur les offres futures, etc. pour vos clients. Ces données n’incluent pas les configurations d’extension que vous configurez dans votre branche de développement local.

Lorsque vous développez des fonctionnalités, ajoutez des extensions et concevez des thèmes, il est utile d’avoir des données réelles pour travailler. À tout moment, vous pouvez [créer une image mémoire de base de données](../storage/database-dump.md) à partir de l’environnement de production et la transmettre à vos environnements d’évaluation et d’intégration, si nécessaire.

Pour exporter des données de production en tant que données de test à utiliser dans les environnements d’évaluation et d’intégration :

- [Exécutez les utilitaires d’assistance](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) commandes d’interface de ligne de commande (recommandé) lors de l’exportation d’une sauvegarde protégée des données client et de stockage à l’aide de votre clé de chiffrement Adobe Commerce

- Outil [Collecte de données](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/support#data-collector) pour générer et exporter des données

Pour migrer ces données, voir [Migrer et déployer des fichiers et des données statiques](../deploy/staging-production.md#migrate-static-files).

![Extraire et nettoyer les données de production](../../assets/starter/data-code-process.png)

>[!NOTE]
>
>Avant de transférer les données vers un autre environnement, vous devez envisager de nettoyer vos données. Vous disposez de plusieurs options, notamment l’[utilisation d’utilitaires d’assistance](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) ou le développement d’un script pour nettoyer les données client.

>[!WARNING]
>
>Ne poussez pas une base de données d’un environnement d’intégration ou d’évaluation vers un environnement de production. Si vous le faites, les données de l’environnement d’intégration ou d’évaluation remplacent vos données de production actives, y compris les ventes, les commandes, les nouveaux clients et les clients mis à jour, etc.

## Workflow de déploiement

Comme indiqué dans les informations d’architecture, Adobe Commerce sur les infrastructures cloud est piloté par Git. Le déploiement d’Adobe Commerce sur une infrastructure cloud fait partie de vos processus de notification push Git pour les branches.

Lorsque vous poussez le code embranché de votre environnement local vers la branche distante, une série de scripts de build et de déploiement commence.

Scripts de build :

- Le site de l’environnement cible continue de s’exécuter lors d’une création

- Vérifiez et exécutez Adobe Commerce sur les correctifs et correctifs d’infrastructure cloud.

- Compilez votre code avec une version et déployez le journal

- Recherchez Configuration Management si le déploiement de contenu statique se produit pendant cette phase

- Création ou utilisation d’un titre de rappel de code inchangé pour accélérer le processus

- Configuration de tous les services et applications principaux

Déployer les scripts :

- Place votre site sur l&#39;environnement-cible en mode Maintenance

- Déploie du contenu statique s’il n’est pas terminé lors de la création

- Installe ou met à jour Adobe Commerce sur une infrastructure cloud

- Configurer le routage du trafic

Une fois l’opération terminée, votre boutique revient en ligne, en ligne, avec l’ensemble de votre code et de vos configurations mis à jour.

Voir [Processus de déploiement](../deploy/process.md).

### Intégrer à l’évaluation et au test

Envoyez toujours votre code par itérations vers l’environnement `staging` pour un test complet. La première fois que vous utilisez cet environnement, vous devez configurer quelques services, notamment [Fastly](/help/cloud-guide/cdn/fastly.md) et [New Relic](../monitor/new-relic-service.md). Configurez également les passerelles de paiement, l’expédition, les notifications et d’autres services essentiels avec des informations d’identification de test ou de sandbox.

L’évaluation est un environnement de préproduction qui fournit tous les services et paramètres aussi proche que possible de la production. Testez minutieusement chaque service, vérifiez vos outils de test de performance, effectuez des tests UAT en tant qu’administrateur et client, jusqu’à ce que vous sentiez que votre boutique est prête pour la production.

Voir [ Déployer votre boutique ](../deploy/staging-production.md).

### Intégrer à la production

Lorsque vous effectuez une transmission de type push vers la branche `master`, vous effectuez une transmission de type push vers l’environnement `production`. Effectuez les activités de configuration et de test dans l’environnement de production comme vous l’avez fait dans l’environnement d’évaluation avec une différence importante. Dans l’environnement de production, utilisez les informations d’identification actives pour la configuration et les tests. Au moment où vous lancez votre site, les clients peuvent effectuer des achats et les administrateurs peuvent gérer la boutique en ligne.

Voir [ Déployer votre boutique ](../deploy/staging-production.md).

### Lancement du site

Il existe une présentation claire de la mise en ligne de votre site. Une fois ces étapes terminées, votre boutique peut proposer immédiatement des produits à vendre dans votre thème personnalisé.

Voir [Site launch](../launch/overview.md).

## Intégration continue

En suivant vos méthodologies d’embranchement et de développement, vous pouvez facilement développer de nouvelles fonctionnalités, configurer des modifications et ajouter des extensions pour développer et déployer des mises à jour en continu.

Tous les environnements d’infrastructure cloud prennent en charge l’intégration continue pour des mises à jour constantes. Ce workflow prend en charge les versions plusieurs fois par jour ou selon un planning défini en fonction des besoins de votre entreprise.

- Créer des branches de développement avec les fonctionnalités et modifications futures

- Tester le code dans votre environnement de `integration`

- Déploiement et test dans `staging` environnement

- Déploiement dans l’environnement `production`
