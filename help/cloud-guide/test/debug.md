---
title: Configurez  [!DNL Xdebug]
description: Découvrez comment configurer l’extension Xdebug pour déboguer votre Adobe Commerce sur le développement de projet d’infrastructure cloud.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1920'
ht-degree: 0%

---

# Configurer Xdebug

[!DNL Xdebug] est une extension pour déboguer votre PHP. Bien que vous puissiez utiliser un IDE de votre choix, la procédure suivante explique comment configurer [!DNL Xdebug] et [!DNL PhpStorm] pour le débogage dans votre environnement local.

>[!NOTE]
>
>Vous pouvez configurer [!DNL Xdebug] pour qu’il s’exécute dans l’environnement Cloud Docker à des fins de débogage local sans modifier la configuration de votre projet Adobe Commerce sur l’infrastructure cloud. Voir [Configuration de Xdebug pour Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).

Pour activer [!DNL Xdebug], vous devez configurer un fichier dans votre référentiel Git, votre IDE et configurer le transfert de port. Vous pouvez configurer certains paramètres dans le fichier `magento.app.yaml`. Après modification, poussez les modifications Git sur tous les environnements de démarrage et les environnements d’intégration Pro pour activer [!DNL Xdebug]. [!DNL Xdebug] est déjà disponible dans les environnements d’évaluation et de production Pro.

Une fois configurée, vous pouvez déboguer les commandes de l’interface de ligne de commande, les requêtes web et le code. N’oubliez pas que tous les environnements d’infrastructure cloud sont en lecture seule. Clonez le code dans votre environnement de développement local pour effectuer le débogage. Pour les environnements d’évaluation et de production Pro, consultez [instructions supplémentaires](#debug-for-pro-staging-and-production) pour plus d’[!DNL Xdebug].

## Conditions requises

Pour exécuter et utiliser [!DNL Xdebug], vous avez besoin de l’URL SSH pour l’environnement . Vous pouvez localiser les informations via le [[!DNL Cloud Console]](../project/overview.md) ou votre [!DNL Cloud Onboarding UI].

## Configurer Xdebug

Pour configurer [!DNL Xdebug], procédez comme suit :

- [Utiliser une branche pour transmettre les mises à jour de fichiers](#get-started-with-a-branch)
- [Activation  [!DNL Xdebug]  environnements](#enable-xdebug-in-your-environment)
- [Configuration du serveur PHPStorm](#configure-phpstorm-server)
- [Configurer le transfert de port](#set-up-port-forwarding)

### Prise en main d’une branche

Pour ajouter des [!DNL Xdebug], Adobe recommande de travailler dans [une branche de développement](../dev-tools/cloud-cli-overview.md#create-an-environment-branch).

### Activation de Xdebug dans votre environnement

Vous pouvez activer [!DNL Xdebug] directement dans tous les environnements de démarrage et les environnements d’intégration Pro. Cette étape de configuration n’est pas requise pour les environnements de production et d’évaluation Pro. Voir [Débogage pour l’évaluation et la production pro](#debug-for-pro-staging-and-production).

>[!VIDEO](https://video.tv.adobe.com/v/3437407?learn=on)

Pour activer le [!DNL Xdebug] pour votre projet, ajoutez des `xdebug` à la section `runtime:extensions` du fichier `.magento.app.yaml`.

**Pour activer Xdebug** :

1. Dans votre terminal local, ouvrez le fichier `.magento.app.yaml` dans un éditeur de texte.

1. Dans la section `runtime`, sous `extensions`, ajoutez `xdebug`. Par exemple :

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. Enregistrez vos modifications dans le fichier `.magento.app.yaml` et quittez l’éditeur de texte.

1. Ajoutez, validez et envoyez les modifications pour redéployer l’environnement.

   ```bash
   git add .magento.app.yaml
   ```

   ```bash
   git commit -m "add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

Lorsqu’il est déployé dans des environnements de démarrage et des environnements d’intégration Pro, [!DNL Xdebug] est désormais disponible. Continuez à configurer votre IDE. Pour PhpStorm, voir [Configuration de PhpStorm](#configure-phpstorm).

### Configuration du serveur PhpStorm

>[!VIDEO](https://video.tv.adobe.com/v/3437409?learn=on)

L’IDE [PhpStorm](https://www.jetbrains.com/phpstorm/) doit être configuré pour fonctionner correctement avec [!DNL Xdebug].

**Pour configurer PhpStorm afin qu’il fonctionne avec Xdebug** :

1. Dans votre projet PhpStorm, ouvrez le panneau **Paramètres**.

   - _macOS_—Sélectionnez **PhpStorm** > **Settings**.
   - _Windows/Linux_ : Sélectionnez **Fichier** > **Paramètres**.

1. Dans le panneau _Paramètres_, développez la section **PHP** et cliquez sur **Serveurs**.

1. Cliquez sur **+** pour ajouter une configuration de serveur. Le nom du projet est en gris en haut.

1. [Facultatif] Configurez les paramètres suivants pour la nouvelle configuration du serveur. Voir [Aucun serveur de débogage configuré](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured) dans la documentation de _PHPStorm_.

   - **Nom** : saisissez le même nom que le nom d&#39;hôte. Cette valeur doit correspondre à la valeur de la variable `PHP_IDE_CONFIG` dans les commandes [Debug CLI](#debug-cli-commands) pour utiliser l’interface de ligne de commande à des fins de débogage.
   - **Hôte** : saisissez le nom d&#39;hôte.
   - **Port**—Saisissez `443`.
   - **Debugger** : sélectionnez `Xdebug`.

1. Sélectionnez **Utiliser les mappages de chemin**. Dans le volet _Fichier/Répertoire_, la racine du projet du `serverName` s’affiche.

1. Dans la colonne **Chemin d’accès absolu sur le serveur**, cliquez sur l’icône **Modifier** et ajoutez un paramètre en fonction de l’environnement.

   - Pour tous les environnements de démarrage et les environnements d’intégration Pro, le chemin d’accès distant est `/app`.
   - Pour les environnements d’évaluation et de production Pro :

      - Production : `/app/<project_code>/`
      - Évaluation : `/app/<project_code>_stg/`

1. Remplacez le port [!DNL Xdebug] par `9000,9003` ou limitez-le à `9000` dans le panneau **PHP** > **Debug** > **Xdebug** > **Debug Port**.

1. Cliquez sur **Appliquer**.

### Créer la configuration PHPStorm Run/Debug

Cela permet à l’application de disposer des paramètres de débogage corrects pour gérer la requête de l’application Adobe Commerce.

>[!VIDEO](https://video.tv.adobe.com/v/3437426?learn=on)

1. Ouvrez l&#39;application PHPStorm et cliquez sur **[!UICONTROL Add Configuration]** dans le coin supérieur droit de l&#39;écran.

1. Cliquez sur **[!UICONTROL Add new run configuration]**.

1. Sélectionnez l’option **[!UICONTROL PHP Remote Debug]** .

   - Saisissez un nom unique, mais reconnaissable.
   - Cochez la case [!UICONTROL Filter debug connection by IDE key]** .
   - Sélectionnez le serveur que vous avez créé dans la [section précédente](#configure-phpstorm-server). Si vous ne l’avez pas encore créé, vous pouvez le faire maintenant, mais reportez-vous à cette partie du guide de configuration.
   - Dans le champ de texte **[!UICONTROL IDE key(session id)]**, saisissez `PHPSTORM` en majuscules. Nous l’utiliserons dans d’autres parties de la configuration. Il est donc important de conserver la même chose. Si vous choisissez une autre chaîne, vous devez vous rappeler de l’utiliser ailleurs dans le processus d’installation et de configuration.

1. Cliquez sur **[!UICONTROL Apply]** > **[!UICONTROL OK]**.

### Configurer le transfert de port

>[!VIDEO](https://video.tv.adobe.com/v/3437410?learn=on)

Mappez la connexion `XDEBUG` du serveur à votre système local. Pour effectuer tout type de débogage, vous devez transférer le port 9000 de votre serveur Adobe Commerce on cloud infrastructure vers votre ordinateur local. Consultez l’une des sections suivantes :

- [Transfert de port sous Mac ou UNIX](#port-forwarding-on-mac-or-unix)
- [Transfert de port sous Windows](#port-forwarding-on-windows)

#### Transfert de port sous Mac ou UNIX®

**Pour configurer le transfert de port sur un environnement Mac ou UNIX®, procédez comme suit**

1. Ouvrez un terminal.

1. Utilisez SSH pour établir la connexion.

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   Utilisez l’option `-v` (verbose) afin que chaque fois qu’un socket est connecté au port en cours de transfert, il s’affiche dans le terminal.

   Si une erreur « impossible de se connecter » ou « impossible d&#39;écouter le port sur la télécommande » s&#39;affiche, une autre session SSH active peut persister sur le serveur qui occupe le port 9000. Si cette connexion n’est pas utilisée, vous pouvez l’arrêter.

**Pour résoudre les problèmes de connexion** :

1. Utilisez SSH pour vous connecter à l’environnement d’intégration, d’évaluation ou de production distant.

1. Consultez la liste des sessions SSH : `who`

1. Afficher les sessions SSH existantes par utilisateur. Veillez à ne pas affecter un utilisateur autre que vous-même !

   - intégration : les noms d’utilisateur sont similaires à `dd2q5ct7mhgus`
   - Évaluation : les noms d’utilisateur sont similaires à `dd2q5ct7mhgus_stg`
   - Production : les noms d’utilisateur sont similaires à `dd2q5ct7mhgus`

1. Pour une session utilisateur plus ancienne que la vôtre, recherchez la valeur du pseudo-terminal (PTS), telle que `pts/0`.

1. Interrompt l’ID de processus (PID) correspondant à la valeur PTS.

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   Exemple de réponse :

   ```
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   Pour mettre fin à la connexion, saisissez une commande kill avec l&#39;ID de processus (PID).

   ```bash
   kill 3664
   ```

#### Transfert de port sous Windows

Pour configurer le transfert de port (tunneling SSH) sous Windows, vous devez configurer votre application de terminal Windows. Cet exemple montre comment créer un tunnel SSH à l’aide de [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Vous pouvez utiliser d&#39;autres applications telles que Cygwin. Pour plus d’informations sur les autres applications, consultez la documentation fournisseur fournie avec ces applications.

**Pour configurer un tunnel SSH sous Windows à l’aide de Putty** :

1. Si vous ne l’avez pas déjà fait, téléchargez [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

1. Démarrez Putty.

1. Dans le volet Catégorie, cliquez sur **Session**.

1. Saisissez les informations suivantes :

   - Champ **Nom d’hôte (ou adresse IP)** : saisissez l’[URL SSH](../development/secure-connections.md#connect-to-a-remote-environment) de votre serveur Cloud
   - Champ **Port** : saisissez `22`

   ![Configurer Putty](../../assets/xdebug/putty-session.png)

1. Dans le volet _Catégorie_, cliquez sur **Connexion** > **SSH** > **Tunnels**.

1. Saisissez les informations suivantes :

   - **Port Source** champ : saisissez `9000`
   - Champ **Destination** : saisissez `127.0.0.1:9000`
   - Cliquez sur **Distant**

1. Cliquez sur **Ajouter**.

   ![Créer un tunnel SSH dans Putty](../../assets/xdebug/putty-tunnels.png)

1. Dans le volet _Catégorie_, cliquez sur **Session**.

1. Dans le champ **Sessions enregistrées**, saisissez un nom pour ce tunnel SSH.

1. Cliquez sur **Enregistrer**.

   ![Enregistrez votre tunnel SSH](../../assets/xdebug/putty-session-save.png)

1. Pour tester le tunnel SSH, cliquez sur **Charger**, puis sur **Ouvrir**.

   Si une erreur « impossible de se connecter » s’affiche, vérifiez les points suivants :

   - Tous les paramètres Putty sont corrects
   - Vous exécutez Putty sur la machine sur laquelle se trouvent vos clés SSH Adobe Commerce privées sur l’infrastructure cloud

## Accès SSH aux environnements Xdebug

Pour lancer le débogage, effectuer la configuration, etc., vous avez besoin des commandes SSH pour accéder aux environnements. Vous pouvez obtenir ces informations via le [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) et la feuille de calcul de votre projet.

Pour les environnements de démarrage et les environnements d’intégration Pro, vous pouvez utiliser la commande d’interface de ligne de commande `magento-cloud` suivante pour SSH dans ces environnements :

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

Pour utiliser [!DNL Xdebug], SSH dans l’environnement comme suit :

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

Par exemple,

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## Débogage pour l’évaluation et la production professionnelles

>[!NOTE]
>
>Sur les environnements d’évaluation et de production Pro, [!DNL Xdebug] est toujours disponible, car ces environnements disposent d’une configuration spéciale pour les [!DNL Xdebug]. Toutes les requêtes web normales sont acheminées vers un processus PHP dédié qui n&#39;a pas de [!DNL Xdebug]. Par conséquent, ces requêtes sont traitées normalement et ne subissent pas de dégradation des performances lors du chargement de la [!DNL Xdebug]. Lorsqu’une requête web est envoyée avec la clé [!DNL Xdebug], elle est acheminée vers un processus PHP distinct qui a [!DNL Xdebug] chargé.

Pour utiliser [!DNL Xdebug] spécifiquement sur l&#39;environnement d&#39;évaluation et de production Pro Plan, vous devez créer un tunnel SSH distinct et une session Web à laquelle vous avez uniquement accès. Cette utilisation diffère de l’accès standard, en ce qu’elle vous fournit uniquement un accès et non à tous les utilisateurs et utilisatrices.

Vous avez besoin des éléments suivants :

- Commandes SSH pour accéder aux environnements. Vous pouvez obtenir ces informations par le biais du [[!DNL Cloud Console]](../project/overview.md) ou de votre [!DNL Cloud Onboarding UI].
- La valeur `xdebug_key` définie lors de la configuration des environnements d’évaluation et Pro.

  Vous pouvez trouver le `xdebug_key` en utilisant SSH pour vous connecter au nœud principal et en exécutant :

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**Pour configurer un tunnel SSH vers un environnement d’évaluation ou de production** :

1. Ouvrez un terminal.

1. Nettoyez toutes les sessions SSH pour chaque nœud web du cluster.

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. Configurez le tunnel SSH pour Xdebug pour chaque nœud web du cluster.

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

>[!NOTE]
>
>Pour obtenir la valeur correcte de `USERNAME@CLUSTER.ent.magento.cloud` :
>- Méthode 1 : interface de ligne de commande magento-cloud : `magento-cloud ssh --all`
>- Méthode 2 : console Commerce : https://CONSOLE-URL/ENVIRONMENT, cliquez sur la liste déroulante `SSH v` .

**Pour commencer le débogage à l’aide de l’URL d’environnement** :

Il s’agit d’une démonstration des configurations utilisées, ainsi que du paramètre de GET permettant de démarrer une session de débogage à distance.

>[!VIDEO](https://video.tv.adobe.com/v/3437417?learn=on)

1. Activer le débogage à distance ; visitez le site dans le navigateur et ajoutez ce qui suit à l’URL où `KEY` est la valeur de `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   Cette étape définit le cookie qui envoie les requêtes du navigateur pour déclencher [!DNL Xdebug].

1. Terminez le débogage avec [!DNL Xdebug].

1. Lorsque vous êtes prêt à mettre fin à la session, utilisez la commande suivante pour supprimer le cookie et terminer le débogage via le navigateur dans lequel `KEY` est la valeur de `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >Les `XDEBUG_SESSION_START` transmises par `POST` requêtes ne sont pas prises en charge.

## Commandes de débogage de l’interface de ligne de commande

Cette section décrit les commandes de l’interface de ligne de commande de débogage.

Pour déboguer les commandes de l’interface de ligne de commande :

1. SSH dans le serveur que vous souhaitez déboguer à l’aide des commandes de l’interface de ligne de commande.

1. Créez les variables d’environnement suivantes :

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   Ces variables sont supprimées à la fin de la session SSH.

1. Commencer le débogage

   Dans les environnements de démarrage et les environnements d’intégration Pro, exécutez la commande de l’interface de ligne de commande pour déboguer.
Vous pouvez ajouter des options d’exécution, par exemple :

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   Sur les environnements d&#39;évaluation et de production Pro, vous devez spécifier le chemin d&#39;accès au fichier de configuration PHP [!DNL Xdebug] lors du débogage des commandes de l&#39;interface de ligne de commande, par exemple :

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## Déboguer les requêtes web

Les étapes suivantes vous aident à déboguer les requêtes web.

1. Dans le menu _Extension_, cliquez sur **Déboguer** pour activer.

1. Cliquez avec le bouton droit de la souris, sélectionnez le menu d&#39;options, puis définissez la touche IDE sur **PHPSTORM**.

1. Installez le client [!DNL Xdebug] sur le navigateur. Configurez et activez-le.

### Exemple : configuration de Chrome

Cette section explique comment utiliser [!DNL Xdebug] dans Chrome à l’aide de l’extension [!DNL Xdebug] Helper. Pour plus d’informations sur les outils de [!DNL Xdebug] pour d’autres navigateurs, consultez la documentation du navigateur.

**Pour utiliser Xdebug Helper avec Chrome** :

1. Créez un [tunnel SSH](#ssh-access-to-xdebug-environments) vers le serveur cloud.

1. Installez l’extension [Xdebug Helper](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc) à partir du magasin Chrome.

1. Activez l’extension dans Chrome, comme illustré ci-dessous.

   ![Activer l’extension Xdebug dans Chrome](../../assets/xdebug/enable-chrome-ext.png)

1. Dans Chrome, cliquez avec le bouton droit sur l’icône d’assistance verte dans la barre d’outils de Chrome.

1. Dans le menu pop-up, cliquez sur **Options**.

1. Dans la liste _IDE Key_, cliquez sur **PhpStorm**.

1. Cliquez sur **Enregistrer**.

   ![Options de l’assistant Xdebug](../../assets/xdebug/helper-options.png)

1. Ouvrez votre projet PhpStorm.

1. Dans la barre de navigation supérieure, cliquez sur l’icône **Commencer à écouter**.

   Si la barre de navigation n’est pas affichée, cliquez sur **Affichage** > **Barre de navigation**.

1. Dans le volet de navigation PhpStorm, double-cliquez sur le fichier PHP à tester.

## Déboguer le code local

En raison des environnements en lecture seule, vous devez extraire du code vers le poste de travail local à partir d’un environnement ou d’une branche Git spécifique pour effectuer le débogage.

La méthode que vous choisissez dépend de vous. Les options disponibles sont les suivantes :

- Extraire le code à partir de Git et exécuter `composer install`

  Cette méthode fonctionne, sauf si `composer.json` fait référence à des packages dans des référentiels privés auxquels vous n’avez pas accès. Cette méthode permet d’obtenir la base de code Adobe Commerce complète.

- Copiez les répertoires `vendor`, `app`, `pub`, `lib` et `setup`

  Grâce à cette méthode, vous disposez de tout le code que vous pouvez tester. Selon le nombre de ressources statiques dont vous disposez, un long transfert peut être nécessaire avec un grand volume de fichiers.

- Copiez uniquement le répertoire `vendor`

  Comme la plupart du code se trouve dans le répertoire `vendor`, cette méthode est susceptible d’entraîner de bons tests, bien que ne testent pas l’ensemble de la base de code.

**Pour compresser des fichiers et les copier sur votre ordinateur local** :

1. Utilisez SSH pour vous connecter à l’environnement distant.

1. Compressez les fichiers.

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   Par exemple, pour compresser uniquement le répertoire `vendor` :

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. Sur votre environnement local, utilisez PhpStorm pour compresser les fichiers.

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
