---
title: Intégration de GitHub
description: Découvrez comment intégrer votre projet d’infrastructure cloud Adobe Commerce avec GitHub.
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Intégration de GitHub

L’intégration GitHub vous permet de gérer vos environnements Adobe Commerce sur les infrastructures cloud directement à partir de votre référentiel GitHub. L’intégration gère le contenu déjà présent dans GitHub et se synchronise avec votre référentiel de code Adobe Commerce sur l’infrastructure cloud. En substance, le référentiel de code devient un miroir du référentiel GitHub.

{{private-repository}}

Cette intégration vous permet d’effectuer les opérations suivantes :

- Création d’un environnement lors de la création d’une branche
- Redéploiement de l’environnement lors de la fusion d’une demande d’extraction
- Suppression de l’environnement lors de la suppression de la branche

Vous devez obtenir un jeton GitHub et un webhook pour continuer le processus.

## Conditions préalables

- Accès des administrateurs au projet d’infrastructure cloud d’Adobe Commerce
- Référentiel GitHub
- Jeton d’accès personnel GitHub

## Générer un jeton GitHub

Créez un jeton d’accès personnel classique dans les paramètres du développeur GitHub. Vous devez être membre d’un groupe disposant d’un accès en écriture au référentiel GitHub, de sorte que vous puissiez _pousser_ vers le référentiel. Incluez les portées suivantes lors de la création de votre jeton :

- `admin:repo_hook` : création de hooks web
- `repo` : intégration à votre référentiel
- `read:org`—Intégration avec le référentiel de votre organisation

Voir [GitHub : Créer](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## Préparation de votre référentiel

Clonez votre projet Adobe Commerce sur l’infrastructure cloud à partir d’un environnement existant et migrez les branches du projet vers un nouveau référentiel GitHub vide, en conservant les mêmes noms de branche. Il est **essentiel** de conserver une arborescence Git identique, afin de ne pas perdre d’environnements ou de branches existants dans votre projet d’infrastructure cloud Adobe Commerce.

1. À partir du terminal, connectez-vous à votre projet d’infrastructure cloud Adobe Commerce.

   ```bash
   magento-cloud login
   ```

1. Répertoriez vos projets et copiez l’ID du projet.

   ```bash
   magento-cloud project:list
   ```

1. Clonez le projet sur votre environnement local.

   ```bash
   magento-cloud project:get <project-ID>
   ```

1. Ajoutez votre référentiel GitHub en tant que référentiel distant.

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   Le nom par défaut de la connexion distante peut être `origin` ou `magento`. S’`origin` existe déjà, vous pouvez choisir un autre nom ou renommer ou supprimer la référence existante. Voir la [documentation Git-remote](https://git-scm.com/docs/git-remote).

1. Vérifiez que vous avez correctement ajouté la télécommande GitHub.

   ```bash
   git remote -v
   ```

   Réponse attendue :

   ```
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. Envoyez les fichiers du projet vers votre nouveau référentiel GitHub. N’oubliez pas de conserver les mêmes noms de branche.

   ```bash
   git push -u origin master
   ```

   Si vous commencez avec un nouveau référentiel GitHub, vous devrez peut-être utiliser l’option `-f` , car le référentiel distant ne correspond pas à votre copie locale.

1. Vérifiez que votre référentiel GitHub contient tous vos fichiers de projet.

## Activation de l’intégration GitHub

Avant de commencer, le code et les environnements de votre projet doivent se trouver dans le référentiel GitHub. Après avoir activé l’intégration, le référentiel GitHub devient la source de code. Si vous envoyez des modifications de code au référentiel `magento` d’origine, elles sont remplacées par l’intégration lorsque vous envoyez des modifications de code à votre référentiel GitHub.

Ce qui suit active l’intégration GitHub et fournit une URL de payload à utiliser lors de la création d’un webhook.

>[!WARNING]
>
>La commande suivante remplace le code _all_ de votre projet d’infrastructure cloud Adobe Commerce par le code de votre référentiel GitHub, qui inclut toutes les branches, y compris la branche `production`. Cette action se produit instantanément et ne peut pas être annulée. En règle générale, il est important de cloner toutes les branches de votre projet d’infrastructure cloud Adobe Commerce on cloud et de les pousser vers votre référentiel GitHub **avant** d’ajouter l’intégration GitHub.

Vous pouvez choisir de parcourir les invites de l’interface de ligne de commande à l’aide de `magento-cloud integration:add` ou de créer la commande d’intégration avec les options suivantes :

| Option | Obligatoire ? | Description |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | Oui | URL de base de l’installation du serveur, qui peut être `https://github.com/` ou personnalisée. Omettez cette option si votre référentiel est hébergé avec un Github public ou s’il n’est pas hébergé sur des serveurs privés. Omettez cette option si l’URL de votre référentiel est similaire à `https://github.com/{account}/{repository-name}`. Cela peut entraîner des erreurs telles que `Unable to connect to GitHub: repository not found`. |
| `--token` | Oui | Jeton d’accès personnel généré pour GitHub |
| `--repository` | Oui | Nom du référentiel : `owner-or-organisation/repository` |
| `--build-pull-requests` | Facultatif | Indique à Adobe Commerce sur l’infrastructure cloud de procéder au déploiement après la fusion d’une demande d’extraction (`true` par défaut) |
| `--fetch-branches` | Facultatif | Entraîne Adobe Commerce sur l’infrastructure cloud à suivre les branches et à déployer après la mise à jour d’une branche (`true` par défaut) |
| `--prune-branches` | Facultatif | Supprimer les branches qui n’existent pas sur la télécommande (`true` par défaut) |

Il existe de nombreuses autres options, que vous pouvez afficher à l’aide de l’option d’aide :

```bash
magento-cloud integration:add --help
```

**Pour activer l’intégration GitHub** :

1. Activez l’intégration.

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **Exemple 1** : activez l’intégration GitHub pour un référentiel personnel et privé :

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **Exemple 2** : activez l’intégration GitHub pour un référentiel d’organisation :

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. Saisissez les informations requises lorsque vous y êtes invité.

1. Copiez l’**URL de payload** affichée par la sortie renvoyée.

   ```
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## Ajouter le webhook dans GitHub

Pour communiquer des événements (une notification push, par exemple) avec votre serveur Git Cloud, vous devez créer un webhook pour votre référentiel GitHub :

1. Dans votre référentiel GitHub, cliquez sur l’onglet **Paramètres**.

1. Dans la barre de navigation de gauche, cliquez sur **Webhooks**.

1. Dans le volet _Webhooks_, cliquez sur **Ajouter un webhook**.

1. Dans le formulaire _Webhooks/Add webhook_, modifiez les champs suivants :

   - **URL de payload** : saisissez l’URL renvoyée lorsque vous avez activé l’intégration GitHub.
   - **Type de contenu** : sélectionnez **application/json** dans la liste.
   - **Secret** : saisissez un secret de vérification.
   - **Quels événements souhaitez-vous déclencher ce webhook ?** : Sélectionnez **Envoyer-moi tout**.
   - Cochez la case **Actif**.

1. Cliquez sur **Ajouter un webhook**.

## Tester l’intégration

Après avoir configuré l’intégration GitHub, vous pouvez vérifier qu’elle est opérationnelle à l’aide de l’interface de ligne de commande `magento-cloud` :

```bash
magento-cloud integration:validate
```

Vous pouvez également la tester en envoyant une modification simple à votre référentiel GitHub.

1. Créez un fichier test.

   ```bash
   touch test.md
   ```

1. Validez et envoyez la modification à votre référentiel GitHub.

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. Connectez-vous au [[!DNL Cloud Console]](../project/overview.md) et vérifiez que le message de validation s’affiche, ainsi que le déploiement de votre projet.

## Suppression de l’intégration

Vous pouvez supprimer en toute sécurité l’intégration GitHub de votre projet sans affecter votre code.

**Pour supprimer l’intégration GitHub** :

1. À partir du terminal, connectez-vous à votre projet d’infrastructure cloud Adobe Commerce.

1. Répertorier vos intégrations. Vous avez besoin de l’ID d’intégration GitHub pour effectuer l’étape suivante.

   ```bash
   magento-cloud integration:list
   ```

1. Supprimez l’intégration.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Vous pouvez également supprimer l’intégration GitHub en vous connectant à votre compte GitHub et en supprimant le hook web dans l’onglet _Webhooks_ du référentiel _Settings_.
