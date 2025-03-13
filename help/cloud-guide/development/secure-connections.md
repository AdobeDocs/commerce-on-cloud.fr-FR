---
title: Connexions sécurisées
description: Découvrez comment appliquer des clés SSH à votre projet d’infrastructure Adobe Commerce on cloud et comment vous connecter à des environnements distants.
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: c25e5b74ae8105995107860246ecb9ba45910bb1
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 0%

---

# Connexions sécurisées à des environnements distants

Secure Shell (SSH) est un protocole courant utilisé pour se connecter en toute sécurité aux serveurs et systèmes distants. Vous pouvez utiliser SSH pour accéder à vos environnements distants afin de gérer l’application Adobe Commerce et d’accéder aux journaux d’environnement distant. Adobe ne prend en charge que les connexions FTP sécurisées (sFTP) à l’aide de votre clé publique SSH. Connexions FTP non prises en charge.

## Générer une paire de clés SSH

Créez une paire de clés SSH sur chaque ordinateur et espace de travail qui nécessite l’accès à votre code source de projet et à vos environnements. La clé SSH vous permet de vous connecter à GitHub pour gérer le code source et vous connecter aux serveurs cloud sans avoir à fournir constamment votre nom d’utilisateur et votre mot de passe. Voir [Connexion à GitHub avec SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) pour plus d’informations sur la création d’une paire de clés SSH.

- La _clé publique_ peut être fournie en toute sécurité pour accéder à un site, SSH et sFTP.
- La _clé privée_ reste privée sur la station de travail.

>[!CAUTION]
>
>**Ne partagez jamais votre clé privée.** Ne l’ajoutez pas à un ticket, ne le copiez pas dans une discussion ou ne le joignez pas à des e-mails.

## Ajouter une clé publique SSH à votre compte

Après avoir ajouté ou mis à jour votre clé publique SSH sur votre compte d’infrastructure cloud Adobe Commerce, [redéployez tous les environnements actifs](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy) sur votre compte pour installer la clé.

Vous pouvez ajouter des clés SSH à votre compte à l’aide de l’une des méthodes suivantes : Cloud CLI ou [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### Ajouter votre clé SSH à l’aide de l’interface de ligne de commande Cloud

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Connectez-vous à votre projet :

   ```bash
   magento-cloud login
   ```

1. Ajoutez la clé publique .

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Vous pouvez répertorier et supprimer des clés SSH à l’aide des commandes de l’interface de ligne de commande Cloud `ssh-key:list` et `ssh-key:delete`.

>[!TAB  Console ]

### Ajoutez votre clé SSH à l’aide du [!DNL Cloud Console] .

**Pour ajouter une clé SSH à un nouveau projet** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Cliquez sur **[!UICONTROL No SSH key]**. Cette icône se trouve à droite du champ de commande et est visible lorsque le projet ne contient pas de clé SSH.

1. Copiez et collez le contenu de votre clé SSH publique dans le champ **Clé publique**.

1. Suivez les autres invites.

**Pour ajouter une clé SSH à votre profil Cloud** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Dans le menu supérieur droit du compte, cliquez sur **Mon profil**.

1. Dans la vue _Clés SSH_, cliquez sur **Ajouter une clé publique**.

1. Dans le formulaire _Ajouter une clé SSH_, donnez à votre clé un **Titre** et collez la clé SSH publique dans le champ **Clé**.

1. Cliquez sur **Enregistrer**.

>[!ENDTABS]

## Connexion à un environnement distant

Vous pouvez vous connecter à un environnement distant à l’aide de l’interface de ligne de commande `magento-cloud` ou d’une commande SSH. Les commandes de l’interface de ligne de commande `magento-cloud` ne peuvent être utilisées que dans les environnements d’intégration Starter et Pro.

### Utilisation de l’interface de ligne de commande Cloud

**Pour vous connecter à un environnement d’intégration distant** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Répertoriez les environnements de ce projet.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### Utiliser une commande SSH

Le [!DNL Cloud Console] comprend une liste des commandes d’accès Web et SSH pour chaque environnement.

**Pour copier la commande SSH** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Sélectionnez un environnement.

1. Cliquez sur **[!UICONTROL SSH]**.

1. Dans l’onglet _SSH_, cliquez sur le bouton Copy (Copier) pour copier la commande SSH complète dans le presse-papiers.

1. Ouvrez un terminal et collez la commande SSH pour créer une connexion.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Pour les environnements d’évaluation et de production Pro, la commande SSH peut se présenter comme suit :
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

Adobe Commerce sur les infrastructures cloud prend en charge l’accès à vos environnements à l’aide de sFTP (FTP sécurisé) avec authentification SSH. Utilisez un client qui prend en charge l’authentification par clé SSH pour sFTP et utilisez votre clé SSH publique. Votre clé SSH publique doit être ajoutée à l’environnement cible. Pour les environnements de démarrage et les environnements d’intégration Pro, vous pouvez [l’ajouter via l’ [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

Les connexions sFTP en lecture seule ne sont _pas_ prises en charge ; l’accès sFTP est fourni avec une autorisation _écriture_ par défaut.

Lors de la configuration de sFTP, utilisez les informations de la commande de votre environnement d’accès SSH : `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **Nom d’utilisateur** : tout le contenu avant le `@` dans votre destination d’accès SSH.
- **Mot de passe** : vous n’avez pas besoin de mot de passe pour sFTP. L’accès sFTP utilise l’authentification par clé SSH.
- **Hôte** : tout le contenu après la `@` dans votre accès SSH.
- **Port** : 22, qui est le port SSH par défaut.
- **SSH** Clé privée : si nécessaire, indiquez l’emplacement de votre clé privée au client sFTP. Par défaut, les clés privées sont stockées dans le répertoire `~/.ssh`.

Selon le client, des options supplémentaires peuvent être nécessaires pour terminer l’authentification SSH pour sFTP. Consultez la documentation relative au client sélectionné.

Pour les **environnements de démarrage et les environnements d’intégration Pro**, vous pouvez également envisager d’ajouter [un `mount`](../application/properties.md#mounts) d’accès à un répertoire spécifique. Ajoutez le montage à votre fichier `.magento.app.yaml`. Pour obtenir une liste des répertoires accessibles en écriture, voir [Structure du projet](../project/file-structure.md). Ce point de montage ne fonctionne que dans ces environnements.

Pour les environnements d’évaluation et de production **Pro**, si vous ne disposez pas d’un accès SSH à l’environnement, vous devez [soumettre un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour demander un accès sFTP et un point de montage pour accéder au dossier spécifique, par exemple `pub/media`.

>[!NOTE]
>Pour l’évaluation et la production Pro, si la connexion sFTP est destinée à un utilisateur _générique_ qui n’a **pas** besoin d’être [ajouté au projet cloud](../project/user-access.md), vous devez [soumettre un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) avec sa clé **publique** jointe. **Ne fournissez jamais votre clé SSH privée.**

## Tunneling SSH

Vous pouvez utiliser le tunneling SSH pour vous connecter à un service à partir de votre environnement de développement local comme si le service était local. Avant de créer un tunnel, configurez votre [SSH](#add-an-ssh-public-key-to-your-account).

Utilisez une application de terminal pour vous connecter et exécuter des commandes.

```bash
magento-cloud login
```

Vérifiez si des tunnels sont ouverts à l’aide de .

```bash
magento-cloud tunnel:list
```

Pour construire un tunnel, vous devez connaître le [ nom de l’application ](../application/properties.md#name). Vous pouvez vérifier le nom de l’application à l’aide de l’interface de ligne de commande :

```bash
magento-cloud apps
```

### Configurer le tunnel SSH

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

Par exemple, pour ouvrir un tunnel vers la branche `sprint5` dans un projet avec une application nommée `mymagento`, saisissez .

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

Exemple de réponse :

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**Pour afficher des informations sur votre tunnel** :

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### Connexion aux services

Après avoir établi un tunnel SSH, vous pouvez vous connecter aux services comme s’ils s’exécutaient localement. Par exemple, pour vous connecter à la base de données, la commande est la suivante :

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
