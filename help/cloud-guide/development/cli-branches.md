---
title: Gestion des branches avec l’interface de ligne de commande
description: Découvrez comment gérer les branches d’environnement pour Adobe Commerce sur les infrastructures cloud à l’aide de l’interface de ligne de commande cloud.
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Gestion des branches avec l’interface de ligne de commande

Pour installer l’interface de ligne de commande `magento-cloud`, reportez-vous à la [référence de l’interface de ligne de commande Cloud](../dev-tools/cloud-cli-overview.md). Après avoir installé l’interface de ligne de commande `magento-cloud` et configuré les clés SSH pour l’accès à distance à votre infrastructure cloud, vous pouvez utiliser `magento-cloud` commandes d’interface de ligne de commande pour gérer les environnements de vos projets. Pour plus d’informations sur l’architecture de l’environnement, voir [Architecture de démarrage](../architecture/starter-architecture.md) ou [Architecture Pro](../architecture/pro-architecture.md).

Pour gérer les branches et les environnements avec le [!DNL Cloud Console], voir [Gérer les branches avec le [!DNL Cloud Console]](../project/console-branches.md).

## Utilisation des commandes de l’interface de ligne de commande

Les commandes de l’interface de ligne de commande `magento-cloud` sont similaires aux commandes Git. Vous pouvez les utiliser pour vous connecter à votre projet et gérer vos environnements. Bien que vous puissiez exécuter les commandes à partir de n’importe quel répertoire, il est recommandé de les exécuter à partir d’un répertoire de projet. Lors de l’exécution à partir d’un répertoire de projet, vous pouvez omettre le paramètre `-p <project-ID>` . Voir la [référence de l’interface de ligne de commande Cloud](../dev-tools/cloud-cli-overview.md).

## Cloner le projet

Les instructions suivantes utilisent une combinaison de commandes d’interface de ligne de commande `magento-cloud` et de commandes Git pour cloner votre projet sur votre station de travail locale. Pour afficher la liste complète des commandes de l’interface de ligne de commande `magento-cloud`, utilisez la commande `magento-cloud list`.

>[!IMPORTANT]
>
>Certaines commandes Git ne peuvent pas effectuer une action dans votre projet d’infrastructure cloud Adobe Commerce. Par exemple, vous pouvez créer une branche à l’aide d’une commande Git, mais vous ne pouvez pas créer ni activer un nouvel environnement. Vous devez créer un environnement à l’aide de la commande `magento-cloud environment:branch <branch-name>` pour que l’environnement devienne _actif_. Vous pouvez également utiliser l’[!DNL Cloud Console] pour créer des environnements actifs. Voir [Référence de l’interface de ligne de commande Cloud](../dev-tools/cloud-cli-overview.md#git-commands).

**Pour cloner un projet `master` un environnement** :

1. Connectez-vous à votre station de travail locale avec un compte [propriétaire du système de fichiers](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html).

1. Accédez au serveur web ou à l’hôte virtuel _répertoire docroot_.

1. Connectez-vous à l’aide de l’interface de ligne de commande `magento-cloud`.

   ```bash
   magento-cloud login
   ```

1. Répertoriez vos projets.

   ```bash
   magento-cloud project:list
   ```

1. Clonez un projet.

   ```bash
   magento-cloud project:get <project-ID>
   ```

   Lorsque vous y êtes invité, indiquez un nom de répertoire.

1. Accédez au répertoire `magento2`.

1. Liste des environnements disponibles pour le projet.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >La commande `magento-cloud environment:list` affiche les hiérarchies d’environnement, contrairement à la commande `git branch` .

1. Récupérez les branches distantes.

   ```bash
   git fetch origin
   ```

1. Extrayez le code mis à jour.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Consultez [Intégrations](../integrations/overview.md) pour plus d’informations sur l’utilisation des services d’hébergement basés sur Git avec Adobe Commerce sur les infrastructures cloud.

## Créer une branche pour le développement

Après avoir cloné votre projet et mis à jour la configuration du compte administrateur Adobe Commerce, vous pouvez créer une branche pour le développement. Comme indiqué précédemment, vous devez créer un environnement à l’aide de la commande `magento-cloud environment:branch <branch-name>` ou de la [!DNL Cloud Console] pour que l’environnement devienne _actif_.

- Pour [Starter](../architecture/starter-develop-deploy-workflow.md#clone-and-branch), pensez à créer une branche pour `staging`, puis à créer une branche de développement basée sur la branche `staging`.
- Pour [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow), créez des branches de développement basées sur la branche `Integration`.

**Pour créer une branche de développement** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Créez un environnement basé sur la branche recommandée pour le workflow de votre projet.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. Mettez à jour les dépendances.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_facultatif_] Créez une [sauvegarde](../storage/snapshots.md) de l’environnement.

### Fusion d’une branche

Une fois le développement terminé, fusionnez cette branche avec le parent :

1. Modifications du code de validation et de notification push :

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Fusionner avec l’environnement parent :

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### Suppression d’un environnement

Ne supprimez un environnement que si vous êtes certain de n’en plus avoir besoin. Vous ne pouvez pas récupérer un environnement après l’avoir supprimé.

>[!WARNING]
>
>Vous ne pouvez pas supprimer la branche `master` d’un projet.

Pour effectuer cette tâche, vous devez être un administrateur de projet, un administrateur d’environnement ou un propriétaire de compte. Voir [Gérer l’accès des utilisateurs aux projets cloud](../project/user-access.md).

Lorsque vous supprimez un environnement, celui-ci est défini sur _inactif_. Le code est toujours disponible dans la branche Git, mais ne contient plus les services ni la base de données. Pour supprimer complètement l’environnement, vous devez également supprimer la branche Git distante correspondante.

**Pour supprimer un environnement** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Récupérez les mises à jour à partir du serveur distant.

   ```bash
   git fetch
   ```

1. Supprimez la branche d’environnement.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   Vous pouvez éventuellement supprimer plusieurs environnements à la fois en ajoutant plusieurs identifiants d’environnement à la commande de suppression.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. Répondez aux invites pour supprimer l&#39;environnement local et l&#39;environnement distant correspondant.

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   La suppression de l’environnement le place dans un état _inactif_.

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   La suppression de la branche Git distante supprime l’environnement du projet.

1. Attendez que l’environnement soit supprimé.

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>Pour activer un environnement inactif, utilisez la commande `magento-cloud environment:activate`.

## Interaction avec des environnements distants

Après avoir [configuré les clés SSH](../development/secure-connections.md), vous pouvez [vous connecter de votre espace de travail local à un environnement distant](../development/secure-connections.md#connect-to-a-remote-environment) interagir avec les services de votre projet et modifier les paramètres.
