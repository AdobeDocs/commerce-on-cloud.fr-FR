---
title: Workflow de pro-projet
description: Découvrez comment utiliser les workflows de développement et de déploiement Pro.
feature: Cloud, Iaas, Paas
exl-id: efe41991-8940-4d5c-a720-80369274bee3
source-git-commit: 7758ca69fc8232a8e1798c536410dc028c87fee6
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 0%

---

# Workflow de pro-projet

Le projet Pro comprend un référentiel Git unique avec une branche de `master` globale et trois environnements principaux :

1. Environnement **de production** de lancement et de maintenance du site actif
1. Environnement **intermédiaire** pour les tests avec tous les services
1. **Intégration** environnement de développement et de test

![Liste des environnements Pro](../../assets/pro-environments.png)

Ces environnements sont `read-only`, acceptant les modifications de code déployé des branches transmises à partir de votre espace de travail local uniquement.

L’illustration suivante présente le workflow de développement et de déploiement Pro, qui utilise une approche simple par branchement Git. Vous [développez](#development-workflow) codez à l’aide d’une branche active basée sur l’environnement `integration`, en _poussant_ et _extrayant_ le code change vers et depuis votre branche active distante. Vous déployez du code vérifié en _fusionnant_ la branche distante vers la branche de base, ce qui active un processus automatisé [création et déploiement](#deployment-workflow) pour cet environnement.

![Vue d&#39;ensemble du workflow de développement de l&#39;architecture Pro](../../assets/pro-dev-workflow.png)

Comme l’environnement est en lecture seule, vous ne pouvez pas apporter de modifications de code directement dans l’environnement Cloud. Si vous essayez d’exécuter `composer install` pour installer des modules, une erreur s’affiche, par exemple :

```bash
file_put_contents(...): Failed to open stream: Read-only file system  
The disk hosting /app/<cluster_ID> is full
```

>[!NOTE]
>
>Il n&#39;est pas possible de modifier les autorisations sur les dossiers en lecture seule dans aucun des environnements Pro. Cette restriction protège l’intégrité et la sécurité de l’application. Les autorisations de dossier sur ces systèmes de fichiers en lecture seule ne peuvent pas être modifiées. Même le support technique ne peut pas les modifier. Toutes les modifications doivent être apportées à partir d’une branche dans votre environnement de développement local et transmises à l’environnement d’application. Pour plus d&#39;informations, consultez [Architecture Pro](pro-architecture.md) pour un aperçu des environnements Pro, et voir [[!DNL Cloud Console]](../project/overview.md#cloud-console) pour un aperçu de la liste Environnements Pro dans la vue Projet.

## Workflow de développement

L’environnement d’intégration fournit une branche de `integration` de base unique contenant le code de votre infrastructure Adobe Commerce on cloud. Vous pouvez créer une branche d’environnement active supplémentaire. Cela permet d’avoir jusqu’à deux branches actives déployées sur les conteneurs Platform as a Service (PaaS). Le nombre d’environnements inactifs n’est pas limité. Cependant, plus il y a d’environnements inactifs, plus le chargement de la console cloud prendra du temps.

{{enhanced-integration-envs}}

Les environnements de projet prennent en charge un processus d’intégration flexible et continu. Commencez par cloner la branche `integration` sur le dossier de votre projet local. Créez une ou plusieurs branches, développez de nouvelles fonctionnalités, configurez des modifications, ajoutez des extensions et déployez des mises à jour :

- **Récupérer** modifications à partir de `integration`

- **Branche** à partir du `integration`

- **Développement** code sur une station de travail locale, y compris les mises à jour [!DNL Composer]

- **Push** modifications du code vers Remote et validate

- **Fusionner** pour `integration` et tester

Avec une branche de code développée et les fichiers de configuration correspondants, vos modifications de code sont prêtes à être fusionnées dans la branche `integration` pour des tests plus complets. L’environnement `integration` est également idéal pour :

- **Intégration de services tiers**—Tous les services ne sont pas disponibles dans l&#39;environnement PaaS.

- **Génération des fichiers de gestion de la configuration**—Certains paramètres de configuration sont _Lecture seule_ dans un environnement déployé.

- **Configuration de votre boutique** : vous devez configurer entièrement tous les paramètres de la boutique à l’aide de l’environnement d’intégration. L’URL d’administration du magasin **Store** se trouve dans la vue d’environnement _integration_ de la _[!DNL Cloud Console]_.

## Workflow de déploiement

Chaque fois que vous envoyez du code de votre station de travail locale vers l’environnement distant ou que vous fusionnez du code vers une branche d’environnement, les scripts de build et de déploiement génèrent un nouveau code et fournissent les services configurés à l’environnement distant.

Créer des actions de script :

- Le site de l’environnement cible continue de s’exécuter lors d’une création

- Vérifiez et exécutez Adobe Commerce sur les correctifs et correctifs d’infrastructure cloud.

- Compilation du code avec une version et déploiement du journal

- Vérifiez la gestion de la configuration. Le déploiement de contenu statique se produit pendant cette phase

- Créez ou utilisez un rappel de code inchangé pour accélérer le processus

- Configuration de tous les services et applications principaux

Déployer les actions de script :

- Placez le site dans l’environnement cible en mode _Maintenance_

- Déployer du contenu statique s’il n’est pas terminé lors de la création

- Installation ou mise à jour d’Adobe Commerce sur une infrastructure cloud

- Configurer le routage du trafic

Après le processus de création et de déploiement, votre boutique revient en ligne avec vos dernières modifications et configurations de code. Voir [Processus de déploiement](../deploy/process.md).

### Fusionner vers l’intégration

Combinez toutes les modifications de code vérifiées en fusionnant votre branche de développement active dans la branche de `integration` de base. Vous pouvez tester toutes vos modifications sur la branche `integration` avant de promouvoir les modifications dans l’environnement d’évaluation.

### Fusion vers l’évaluation

L’évaluation est un environnement de préproduction qui fournit tous les services et paramètres aussi proche que possible de l’environnement de production. Envoyez toujours vos modifications de code de l’environnement `integration` vers l’environnement `staging` afin de pouvoir effectuer des tests approfondis avec tous les services. La première fois que vous utilisez l’environnement d’évaluation, vous devez configurer des services, tels que [Fast CDN](../cdn/fastly.md) et [New Relic](../monitor/new-relic-service.md). Configurez les passerelles de paiement, l’expédition, les notifications et d’autres services essentiels avec des informations d’identification de test ou de sandbox.

Il est préférable de tester minutieusement chaque service, de vérifier vos outils de test de performance et d’effectuer des tests UAT en tant qu’administrateur et client, jusqu’à ce que vous sentiez que votre boutique est prête pour l’environnement de production. Voir [ Déployer votre boutique ](../deploy/staging-production.md).

{{second-staging}}

### Fusionner en environnement de production

Après des tests approfondis dans l’environnement d’évaluation, fusionnez-les avec l’environnement de production et testez-les minutieusement à l’aide des informations d’identification actives. Au moment du lancement de votre site de production, les clients doivent pouvoir effectuer des achats et les administrateurs doivent pouvoir gérer le magasin en ligne. Consultez les rubriques suivantes pour une présentation détaillée et claire du déploiement et de la mise en ligne de votre boutique :

- [Déploiement de votre boutique](../deploy/staging-production.md)
- [Lancement du site](../launch/overview.md)

### Fusionner vers le Principal global

Envoyez toujours une copie du code de production au `master` global au cas où un besoin urgent se ferait sentir de déboguer l’environnement de production sans interrompre les services.

Ne créez **pas** de branche à partir de la `master` globale . Utilisez la branche `integration` pour créer des branches actives pour le développement et les correctifs.
