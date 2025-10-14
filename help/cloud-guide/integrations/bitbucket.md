---
title: Intégration de l’intervalle de bits
description: Découvrez comment intégrer votre projet d’infrastructure cloud Adobe Commerce on cloud avec Bitbucket.
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Intégration de l’intervalle de bits

Vous pouvez configurer votre référentiel Bitbucket pour créer et déployer automatiquement un environnement lorsque vous poussez des modifications de code. Cette intégration synchronise votre référentiel Bitbucket avec votre compte d’infrastructure cloud Adobe Commerce.

{{private-repository}}

## Conditions préalables

- Accès des administrateurs au projet d’infrastructure cloud d’Adobe Commerce
- [`magento-cloud` de l’interface de ligne &#x200B;](../dev-tools/cloud-cli-overview.md) commande dans votre environnement local
- Un compte Bitbucket
- Accès de l’administrateur au référentiel Bitbucket
- Clé d’accès SSH pour le référentiel Bitbucket

## Préparation de votre référentiel

Clonez votre projet Adobe Commerce sur l’infrastructure cloud à partir d’un environnement existant et migrez les branches du projet vers un nouveau référentiel Bitbucket vide, en conservant les mêmes noms de branche. Il est **essentiel** de conserver une arborescence Git identique, afin de ne pas perdre d’environnements ou de branches existants dans votre projet d’infrastructure cloud Adobe Commerce.

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

1. Ajoutez votre référentiel Bitbucket en tant que référentiel distant.

   ```bash
   git remote add origin git@bitbucket.org:<user-name>/<repo-name>.git
   ```

   Le nom par défaut de la connexion distante peut être `origin` ou `magento`. S’`origin` existe déjà, vous pouvez choisir un autre nom ou renommer ou supprimer la référence existante. Voir la [documentation Git-remote](https://git-scm.com/docs/git-remote).

1. Vérifiez que vous avez correctement ajouté la télécommande Bitbucket.

   ```bash
   git remote -v
   ```

   Réponse attendue :

   ```
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. Envoyez les fichiers du projet vers votre nouveau référentiel Bitbucket. N’oubliez pas de conserver les mêmes noms de branche.

   ```bash
   git push -u origin master
   ```

   Si vous commencez avec un nouveau référentiel Bitbucket, vous devrez peut-être utiliser l’option `-f`, car le référentiel distant ne correspond pas à votre copie locale.

1. Vérifiez que votre référentiel Bitbucket contient tous vos fichiers de projet.

## Créer un client OAuth

L’intégration de Bitbucket nécessite un client [OAuth](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/). Vous avez besoin du `key` et du `secret` OAuth de ce client pour terminer la section suivante.

**Pour créer un client OAuth dans Bitbucket** :

1. Connectez-vous à votre compte [&#x200B; Bitbucket &#x200B;](https://id.atlassian.com/login).

1. Cliquez sur **Paramètres** > **Gestion des accès** > **OAuth**.

1. Cliquez sur **Ajouter un client** et configurez-le comme suit :

   ![Configuration du client OAuth de compartiment de bits](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >Aucune **URL de rappel** valide n’est requise, mais vous devez saisir une valeur dans ce champ pour terminer l’intégration.

1. Cliquez sur **Enregistrer**.

1. Cliquez sur le client **Nom** pour afficher vos `key` et `secret` OAuth.

1. Copiez votre `key` OAuth et `secret` pour configurer l’intégration.

## Configuration de l’intégration

1. Depuis le terminal, accédez à votre projet d’infrastructure cloud Adobe Commerce.

1. Créez un fichier temporaire appelé `bitbucket.json` et ajoutez les éléments suivants, en remplaçant les variables entre crochets par vos valeurs :

   ```json
   {
     "type": "bitbucket",
     "repository": "<bitbucket-user-name/bitbucket-repo-name>",
     "app_credentials": {
       "key": "<oauth-consumer-key>",
       "secret": "<oauth-consumer-secret>"
     },
     "prune_branches": true,
     "fetch_branches": true,
     "build_pull_requests": true,
     "resync_pull_requests": true
   }
   ```

   >[!TIP]
   >
   >Veillez à utiliser le nom de votre référentiel Bitbucket et non l’URL. L’intégration échoue si vous utilisez une URL.

1. Ajoutez l’intégration à votre projet à l’aide de l’outil d’interface de ligne de commande `magento-cloud`.

   >[!WARNING]
   >
   >La commande suivante remplace le code _all_ de votre projet d’infrastructure cloud Adobe Commerce par le code de votre référentiel Bitbucket. Cela inclut toutes les branches, y compris la branche `production`. Cette action se produit instantanément et ne peut pas être annulée. En règle générale, il est important de cloner toutes les branches de votre projet d’infrastructure cloud Adobe Commerce on cloud et de les pousser vers votre référentiel Bitbucket **avant** d’ajouter l’intégration Bitbucket.

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   Cela renvoie une réponse HTTP longue avec des en-têtes. Une intégration réussie renvoie un code d’état 200 ou 201. Un statut supérieur ou égal à 400 indique qu’une erreur s’est produite.

1. Supprimez le fichier `bitbucket.json` temporaire.

1. Vérifiez l’intégration du projet.

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   Notez l’**URL Hook** pour configurer un webhook dans BitBucket.

### Ajouter un webhook dans BitBucket

Pour communiquer des événements (une notification push, par exemple) avec votre serveur Git Cloud, est-il nécessaire de disposer d’un webhook pour votre référentiel BitBucket ? La méthode de configuration d’une intégration Bitbucket présentée sur cette page, lorsqu’elle est correctement suivie, crée automatiquement un webhook. Il est important de vérifier le webhook pour éviter de créer plusieurs intégrations.

1. Connectez-vous à votre compte [&#x200B; Bitbucket &#x200B;](https://id.atlassian.com/login).

1. Cliquez sur **Référentiels** et sélectionnez votre projet.

1. Cliquez sur **Paramètres du référentiel** > **Workflow** > **Webhooks**.

1. Vérifiez le webhook avant de continuer.

   Si le hook est actif, ignorez les étapes restantes et [Tester l’intégration](#test-the-integration). Le hook doit avoir un nom similaire à **»Adobe Commerce on cloud infrastructure &lt;project_id> »** et un format d’URL de hook similaire à : `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook`

1. Cliquez sur **Ajouter un webhook**.

1. Dans la vue _Ajouter un nouveau webhook_, modifiez les champs suivants :

   - **Titre** : Intégration d’Adobe Commerce
   - **URL** : utilisez l’URL Hook de votre liste d’intégration `magento-cloud`
   - **Triggers** : la valeur par défaut est une _de base Repository push_

1. Cliquez sur **Enregistrer**.

### Tester l’intégration

Après avoir configuré l’intégration Bitbucket, vous pouvez vérifier que l’intégration est opérationnelle à l’aide de l’interface de ligne de commande `magento-cloud` :

```bash
magento-cloud integration:validate
```

Vous pouvez également la tester en envoyant une modification simple à votre référentiel Bitbucket.

1. Créez un fichier test.

   ```bash
   touch test.md
   ```

1. Validez et envoyez la modification à votre référentiel Bitbucket.

   ```bash
   git add . && git commit -m "Testing Bitbucket integration" && git push
   ```

1. Connectez-vous au [[!DNL Cloud Console]](../project/overview.md) et vérifiez que le message de validation s’affiche, ainsi que le déploiement de votre projet.

   ![Test de l’intégration Bitbucket](../../assets/bitbucket-integration.png)

## Création d’une branche Cloud

L’intégration Bitbucket ne peut pas activer de nouveaux environnements dans votre projet d’infrastructure cloud Adobe Commerce on cloud. Si vous créez un environnement avec Bitbucket, vous devez l’activer manuellement. Pour éviter cette étape supplémentaire, il est recommandé de créer des environnements à l’aide de l’outil d’interface de ligne de commande `magento-cloud` ou de l’[!DNL Cloud Console] .

**Pour activer une branche créée avec Bitbucket** :

1. Utilisez l’interface de ligne de commande `magento-cloud` pour pousser la branche.

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. Vérifiez que l’environnement est actif.

   ```bash
   magento-cloud environment:list
   ```

   ```
   Your environments are:
   +---------------------+----------------+--------+
   | ID                  | Name           | Status |
   +---------------------+----------------+--------+
   | master              | Master         | Active |
   |  integration        | integration    | Active |
   |    from-bitbucket * | from-bitbucket | Active |
   +---------------------+----------------+--------+
   * - Indicates the current environment
   ```

Après avoir créé un environnement, vous pouvez pousser la branche correspondante vers votre référentiel Bitbucket distant à l’aide de commandes Git standard. Les modifications ultérieures apportées à votre branche dans Bitbucket créent et déploient automatiquement l’environnement.

## Suppression de l’intégration

Vous pouvez supprimer en toute sécurité l’intégration Bitbucket de votre projet sans affecter votre code.

**Pour supprimer l’intégration de Bitbucket** :

1. À partir du terminal, connectez-vous à votre projet d’infrastructure cloud Adobe Commerce.

1. Répertorier vos intégrations. Vous avez besoin de l’identifiant d’intégration Bitbucket pour terminer l’étape suivante.

   ```bash
   magento-cloud integration:list
   ```

1. Supprimez l’intégration.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Vous pouvez également supprimer l’intégration Bitbucket en vous connectant à votre compte Bitbucket et en révoquant l’octroi OAuth sur la page _Paramètres_ du compte.

## Intégration du serveur Bitbucket

Pour utiliser l’intégration du serveur Bitbucket, vous avez besoin des éléments suivants :

- [Jeton d’accès Bitbucket](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html) : générez un jeton qui accorde l’accès à Project `read` et à Repository `admin`
- [&#x200B; URL du serveur Bitbucket &#x200B;](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html) : ajoutez l’URL de base de votre instance Bitbucket

Bien que vous puissiez utiliser l’interface de ligne de commande Cloud pour suivre les étapes d’intégration du serveur Bitbucket, la commande complète ressemble à ce qui suit :

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

Utilisez la commande d’aide pour connaître les autres exigences et options d’utilisation : `magento-cloud integration:add --help`
