---
title: Activer l’authentification multifacteur pour l’accès SSH
description: Découvrez comment gérer les exigences d’authentification pour l’accès SSH à Adobe Commerce sur les environnements d’infrastructure cloud.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Activer l’authentification multifacteur pour l’accès SSH

Pour une sécurité accrue, Adobe Commerce sur les infrastructures cloud fournit une application d’authentification multifacteur (MFA) pour gérer les exigences d’authentification pour l’accès SSH aux environnements cloud.

Lorsque l’authentification multifacteur est activée sur un projet, tous les comptes utilisateur avec un accès SSH nécessitent un code d’authentification à deux facteurs (TFA) ou un jeton API et un certificat SSH pour accéder à l’environnement.

>[!NOTE]
>
>Par défaut, MFA n’est pas activé sur les projets cloud . Le propriétaire du compte pour le projet d’infrastructure cloud d’Adobe Commerce doit [envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour l’activer. Lorsque l’authentification multifacteur est activée, tous les utilisateurs doivent disposer de l’authentification à deux facteurs (TFA) activée sur leur compte d’infrastructure Adobe Commerce sur le cloud pour pouvoir accéder aux environnements de projet via SSH.

## Certificats pour l’accès SSH

MFA permet aux utilisateurs d’exchange un jeton d’accès OAUTH à un certificat SSH de courte durée généré par l’API Adobe Cloud Certifier. Si l’utilisateur dispose du rôle d’administrateur ou de contributeur, d’une clé SSH valide et d’un code TFA ou d’un jeton API valide, Adobe Commerce sur l’infrastructure cloud utilise ces informations d’identification pour générer le certificat SSH temporaire. L’expiration du certificat est définie sur une heure, mais elle s’actualise automatiquement pendant la session en cours.

Après s’être connecté à un projet avec MFA, les utilisateurs doivent utiliser l’interface de ligne de commande `magento-cloud` pour générer le certificat SSH :

```bash
magento-cloud ssh-cert:load
```

La commande `ssh-cert:load` génère le certificat SSH et l’installe dans l’agent SSH de l’utilisateur local.

### Générer automatiquement un certificat lors de la connexion

Vous pouvez configurer votre environnement local pour générer automatiquement le certificat SSH lorsque vous vous authentifiez auprès de l’interface de ligne de commande `magento-cloud`.

**Pour ajouter la génération automatique de certificat SSH à votre configuration d’interface de ligne de commande `magento-cloud`** :

1. Sur votre station de travail locale, créez un fichier nommé `config.yaml` dans le dossier `.magento-cloud` de votre répertoire personnel s’il n’existe pas.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Ajoutez la configuration suivante au fichier `config.yaml`.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. Utilisez l’interface de ligne de commande `magento-cloud` pour vous authentifier à nouveau :

   >Déconnectez-vous :

   ```bash
   magento-cloud logout
   ```

   >Connexion :

   ```bash
   magento-cloud login
   ```

   >Suivez la réponse :

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## Connexion à un environnement à l’aide de SSH avec TFA

Lorsque MFA est activé sur un projet, TFA doit être activé sur votre compte avant de pouvoir vous connecter à un environnement distant à l’aide d’un SSH. Voir [&#x200B; Activer TFA &#x200B;](user-access.md#enable-tfa-for-cloud-accounts).

>[!BEGINSHADEBOX]

**Conditions préalables :**

Pour les projets activés avec l’application MFA, l’accès SSH nécessite les autorisations et paramètres de compte suivants :

- [Accès des administrateurs ou des contributeurs à l’environnement](user-access.md)
- [Clé d’accès SSH configurée sur le compte](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA activé sur le compte](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**Pour vous connecter à l’aide de SSH avec les informations d’identification de compte utilisateur TFA** :

1. Connectez-vous à [votre compte](https://console.adobecommerce.com).

1. Sur votre station de travail locale, utilisez l’interface de ligne de commande `magento-cloud` pour générer le certificat SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Exemple de réponse :

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Utilisez un SSH pour vous connecter à l’environnement distant.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## Gestion du code source à l’aide de SSH avec TFA

Lors de la gestion du code source pour Adobe Commerce sur des projets d’infrastructure cloud, vous utilisez SSH pour vous authentifier auprès du référentiel Git pour le projet. Si l’application MFA est activée pour votre projet, vous devez générer un certificat SSH avant de pouvoir effectuer des opérations de ligne de commande à l’aide du référentiel Git.

**Pour vous connecter à l’aide de SSH avec les informations d’identification de compte utilisateur TFA** :

1. Connectez-vous à [votre compte](https://console.adobecommerce.com) et authentifiez-vous à l’aide de TFA.

   >[!NOTE]
   >
   >Si le TFA n’est pas activé sur votre compte, vous devez l’activer. Voir [Activer TFA sur les comptes cloud](user-access.md#enable-tfa-for-cloud-accounts).

1. Sur votre station de travail locale, utilisez l’interface de ligne de commande `magento-cloud` pour générer le certificat SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Exemple de réponse :

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Clonez le référentiel Git pour l’environnement de votre projet :

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > Exemple de réponse :

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## Connexion à un environnement à l’aide de SSH avec un jeton API

Lorsque MFA est activé sur un projet, les processus automatisés qui nécessitent un accès SSH à un environnement cloud nécessitent un jeton API. Vous pouvez générer le jeton à partir d’un compte d’infrastructure cloud Adobe Commerce avec un accès administrateur ou contributeur au projet.

L’authentification avec un jeton API nécessite toujours la génération d’un certificat SSH. Les processus automatisés doivent également automatiser la génération d’un certificat SSH.

>[!BEGINSHADEBOX]

**Conditions préalables :**

- [Accès des administrateurs ou des contributeurs à l’environnement Adobe Commerce sur les infrastructures cloud](user-access.md)
- [Jeton API valide disponible sur le compte](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**Pour vous connecter à l’aide de SSH avec des informations d’identification de jeton API** :

1. Connectez-vous au projet Cloud à l’aide de l’authentification par clé API.

   ```bash
   magento-cloud auth:api-token
   ```

1. À l’invite, saisissez la valeur d’un jeton API valide.

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### Exemple : script SSH automatisé

Il existe deux options pour stocker le jeton API.

>[!NOTE]
>
>Si un jeton API est stocké, l’interface de ligne de commande `magento-cloud` s’authentifie automatiquement et il n’est pas nécessaire d’exécuter la commande `magento-cloud login`.

**Option 1** : créer une variable d’environnement pour stocker le jeton API

Écrire le jeton sur votre bash_profile

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**Option 2** : ajouter le jeton au fichier `config.yaml`

1. Sur votre station de travail locale, créez un fichier nommé `config.yaml` dans le dossier `.magento-cloud` de votre répertoire personnel s’il n’existe pas.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Ajoutez la configuration suivante au fichier `config.yaml`.

   ```yaml
   api:
      token: <your api token>
   ```

>Exemple de script Bash

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## Dépannage

Utilisez les informations suivantes pour résoudre les échecs des demandes de connexion SSH en raison d’erreurs d’authentification telles que `access requires MFA` ou `permission denied`.

### Votre demande ne fournit pas de certificat valide

Si votre requête ne fournit pas de certificat valide, un message similaire à celui-ci s’affiche :

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

Essayez les procédures de dépannage suivantes pour résoudre le problème de connexion :

- Vérifier la configuration de l’AFE du compte
- Réauthentifiez-vous, puis rechargez le certificat

**Pour vérifier la configuration et l’authentification TFA** :

1. Connectez-vous à [votre compte](https://console.adobecommerce.com).

1. Dans le menu supérieur droit du compte, cliquez sur **[!UICONTROL My Profile]**.

1. Sur la page _Mon profil_, cliquez sur l’onglet **[!UICONTROL Security]**.

   Si TFA est activé, la section Sécurité fournit des options pour gérer la configuration TFA.

1. Si TFA n’est pas configuré, cliquez sur **[!UICONTROL Set up application]** et suivez les instructions pour l’activer. Voir [&#x200B; Activer TFA &#x200B;](user-access.md#enable-tfa-for-cloud-accounts).

1. Si TFA est configuré, essayez de vous authentifier à nouveau.

**Pour authentifier et recharger le certificat SSH** :

1. Utilisez l’interface de ligne de commande `magento-cloud` pour vous authentifier à nouveau :

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. Rechargez le certificat SSH :

   ```bash
   magento-cloud ssh-cert:load
   ```

### Autorisation refusée

Si la clé SSH est manquante ou non valide, la requête de connexion SSH renvoie une erreur `Permission denied (publickey)`.

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

Pour résoudre le problème, ajoutez la clé SSH à votre session en cours ou mettez à jour le fichier de configuration SSH pour charger automatiquement vos clés SSH. Voir [Ajouter une clé SSH publique](../development/secure-connections.md#add-an-ssh-public-key-to-your-account).

### Impossible d’accéder aux projets sans MFA

Si vous vous authentifiez dans un projet pour lequel l’authentification multifacteur (MFA) est activée, vous pouvez recevoir l’erreur suivante lors de la connexion à d’autres projets qui ne nécessitent pas d’authentification multifacteur :

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

Exemple de réponse :

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

Pendant la génération du certificat SSH, l’interface de ligne de commande `magento-cloud` ajoute une clé SSH supplémentaire à votre environnement local. Cette clé est utilisée par défaut si votre configuration SSH locale n’inclut pas la clé SSH pour l’accès au projet.

**Pour ajouter votre clé SSH à la configuration locale** :

1. Créez le fichier `config` s’il n’existe pas.

   ```bash
   touch ~/.ssh/config
   ```

1. Ajoutez une configuration `IdentityFile`.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>Vous pouvez spécifier plusieurs clés SSH en ajoutant plusieurs entrées `IdentityFile` à votre configuration.
