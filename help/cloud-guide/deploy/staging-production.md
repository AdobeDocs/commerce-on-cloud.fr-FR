---
title: Déploiement dans les environnements d’évaluation et de production
description: Découvrez comment déployer votre code d’infrastructure cloud Adobe Commerce sur les environnements d’évaluation et de production pour des tests supplémentaires.
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 1cfeb472-c6ec-44ff-9b32-516ffa1b30d2
source-git-commit: fe634412c6de8325faa36c07e9769cde0eb76c48
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%

---

# Déploiement dans les environnements d’évaluation et de production

Le processus de déploiement et de mise en production commence par le développement, se poursuit par l’évaluation et se termine par la mise en production. Adobe fournit une solution d’environnement de bout en bout pour assurer des configurations cohérentes. Chaque environnement prend en charge l’accès URL direct au storefront et l’accès Admin et SSH pour les commandes d’interface de ligne de commande.

Lorsque vous êtes prêt à déployer votre magasin, vous devez terminer le déploiement et les tests sur l’environnement d’évaluation avant le déploiement en production. Cette section fournit des instructions et des informations détaillées sur le processus de création et de déploiement, la migration des données et du contenu, ainsi que les tests.

>[!TIP]
>
>Adobe recommande de créer une [&#x200B; sauvegarde &#x200B;](../storage/snapshots.md) l’environnement avant les déploiements.

En outre, vous pouvez activer le [suivi des déploiements avec New Relic](../monitor/track-deployments.md) pour surveiller les événements de déploiement et vous aider à analyser les performances entre les déploiements.

## Flux de déploiement de démarrage

Adobe recommande de créer une branche `staging` à partir de la branche `master` pour prendre en charge le développement et le déploiement de votre plan de démarrage. Deux de vos quatre environnements actifs sont alors prêts : `master` pour la production et `staging` pour l’évaluation.

Pour plus d’informations sur le processus, voir [Démarrer le développement et le déploiement d’un workflow](../architecture/starter-develop-deploy-workflow.md).

## Flux de déploiement Pro

Pro est fourni avec un vaste environnement d’intégration comprenant deux branches actives, une branche de `master` globale, des branches d’évaluation et de production. Lorsque vous créez votre projet, le code est prêt à être embranché, développé et envoyé pour la création et le déploiement de votre site. Bien que l’environnement d’intégration puisse comporter de nombreuses branches, l’évaluation et la production ne disposent que d’une seule branche pour chaque environnement.

Pour plus d’informations sur le processus, voir [Développement et déploiement de workflow Pro](../architecture/pro-develop-deploy-workflow.md).

## Déploiement du code vers l’évaluation

L’environnement d’évaluation fournit un environnement de quasi-production qui comprend une base de données, un serveur web et tous les services, y compris Fastly et New Relic. Vous pouvez effectuer une notification push, une fusion et un déploiement complets via les commandes [[!DNL Cloud Console]](../project/overview.md) ou [Cloud CLI](../dev-tools/cloud-cli-overview.md) via une application de terminal.

### Déployer le code avec le [!DNL Cloud Console]

Le [!DNL Cloud Console] fournit des fonctionnalités pour créer, gérer et déployer du code dans les environnements d’intégration, d’évaluation et de production pour les plans Starter et Pro.

**Pour les projets Pro, déployez la branche d’intégration vers l’évaluation** :

1. [Connectez-vous](https://accounts.magento.cloud) à votre projet.
1. Sélectionnez la branche `integration` .
1. Sélectionnez l’option **Fusionner** pour effectuer un déploiement dans l’environnement intermédiaire.

   ![Fusionner](../../assets/button-merge.png){width="150"}

1. Effectuez tous les [tests](../test/staging-and-production.md) dans l’environnement d’évaluation.
1. Lorsque l’évaluation est prête, sélectionnez l’option **Fusionner** pour effectuer le déploiement en production.

**Pour commencer, déployez la branche de développement vers l’évaluation** :

1. [Connectez-vous](https://accounts.magento.cloud) à votre projet.
1. Sélectionnez la branche de code préparée.
1. Sélectionnez l’option **Fusionner** pour effectuer un déploiement dans l’environnement intermédiaire.

   ![Fusionner](../../assets/button-merge.png){width="150"}

1. Effectuez tous les [tests](../test/staging-and-production.md) dans l’environnement d’évaluation.
1. Lorsque l’évaluation est prête, sélectionnez l’option **Fusionner** pour déployer en production (`master`).

### Déployer le code avec la ligne de commande

L’interface de ligne de commande Cloud fournit des commandes pour déployer le code. Vous avez besoin d’un accès SSH et Git à votre projet.

#### Étape 1 : déployer et tester l’environnement d’intégration

1. Après vous être connecté au projet, consultez l’environnement d’intégration :

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Synchronisez votre environnement d’intégration local avec l’environnement distant :

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Créez un instantané de l’environnement en tant que sauvegarde :

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Mettez à jour le code dans votre branche locale si nécessaire.

1. Ajouter, valider et transmettre les modifications à l’environnement.

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. Effectuez les tests du site.

#### Étape 2 : fusion des modifications apportées à l’évaluation et au déploiement

1. Consultez l’environnement d’évaluation :

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Synchronisez votre environnement d’évaluation local avec l’environnement distant :

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Créez un instantané de l’environnement en tant que sauvegarde :

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Fusionnez l’environnement d’intégration avec l’environnement d’évaluation pour déployer :

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. Effectuez les tests du site.

#### Étape 3 : déploiement en production

1. Extrayez, synchronisez et créez un instantané de votre environnement de production local.

1. Fusionnez l’environnement d’évaluation avec l’environnement de production à déployer :

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. Effectuez les tests du site.

## Migration de fichiers statiques

Les [fichiers statiques](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) sont stockés dans `mounts`. Il existe deux méthodes pour migrer des fichiers d’un emplacement de montage source, tel que votre environnement local, vers un emplacement de montage de destination. Les deux méthodes utilisent l’utilitaire `rsync`, mais Adobe recommande d’utiliser l’interface de ligne de commande `magento-cloud` pour déplacer les fichiers entre l’environnement local et l’environnement distant. Adobe recommande également d’utiliser la méthode `rsync` lors du déplacement de fichiers d’une source distante vers un autre emplacement distant.

### Migration de fichiers à l’aide de l’interface de ligne de commande

Vous pouvez utiliser les commandes d’interface de ligne de commande `mount:upload` et `mount:download` pour migrer des fichiers entre les environnements local et distant. Les deux commandes utilisent l’utilitaire `rsync`, mais les commandes de l’interface de ligne de commande fournissent des options et des invites adaptées à l’environnement Adobe Commerce sur le cloud. Par exemple, si vous utilisez la commande simple sans options, l’interface de ligne de commande vous invite à sélectionner le ou les montages à charger ou à télécharger.

```bash
magento-cloud mount:download
```

Exemple de réponse :

```
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**Pour charger des fichiers d’un dossier de `pub/media/` local vers le dossier de `pub/media/` distant pour l’environnement actuel** :

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

Exemple de réponse :

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

Utilisez l’option `--help` pour les commandes `mount:upload` et `mount:download` pour afficher plus d’options. Par exemple, il existe une option `--delete` pour supprimer les fichiers superflus lors de la migration.

### Migrer des fichiers à l’aide de rsync

Vous pouvez également utiliser l’utilitaire `rsync` pour migrer les fichiers.

```bash
rsync -azvP <source> <destination>
```

Cette commande utilise les options suivantes :

- `a`-archive
- `z`-compresser les fichiers pendant la migration
- `v`-verbose
- `P` partielle

Voir l’aide de [rsync](https://linux.die.net/man/1/rsync).

>[!NOTE]
>
>Pour transférer directement des médias d’environnements distants à distants, vous devez activer le transfert de l’agent SSH. Voir [Guide GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Pour migrer directement des fichiers statiques d’environnements distants à distants (approche rapide)** :

1. Utilisez SSH pour vous connecter à l’environnement source. N’utilisez pas l’interface de ligne de commande `magento-cloud`. L’utilisation de l’option `-A` est importante, car elle permet le transfert de la connexion de l’agent d’authentification.

   >[!TIP]
   >
   >Pour trouver le lien **Accès SSH** dans votre [!DNL Cloud Console], sélectionnez l’environnement et cliquez sur **Accéder au site**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Utilisez la commande `rsync` pour copier le répertoire `pub/media` de votre environnement source vers un autre environnement distant.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Connectez-vous à l’autre environnement distant pour vérifier que les fichiers ont bien été migrés.

## Migrer la base de données

>[!WARNING]
>
>Les opérations d&#39;import et d&#39;export de base de données peuvent prendre beaucoup de temps, ce qui peut affecter les performances et la disponibilité du site. Planifiez les opérations d’import et d’export pendant les heures creuses afin d’éviter des performances lentes ou des pannes sur les sites de production.

>[!BEGINSHADEBOX]

**Prérequis :** un vidage de base de données (voir Étape 3) doit inclure des déclencheurs de base de données. Pour les vider, vérifiez que vous disposez de l’autorisation [TRIGGER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger).

>[!IMPORTANT]
>
>La base de données de l’environnement d’intégration est strictement destinée aux tests de développement et peut inclure des données que vous ne souhaitez pas migrer vers les environnements d’évaluation et de production.

Pour les déploiements d’intégration continue, Adobe **déconseille** de migrer les données de l’intégration vers les environnements d’évaluation et de production. Vous pouvez transmettre des données de test ou remplacer des données importantes. Toutes les configurations essentielles sont transmises à l’aide du [fichier de configuration](../store/store-settings.md) et de la commande `setup:upgrade` lors de la génération et du déploiement.

>[!ENDSHADEBOX]

Adobe **recommande** de migrer les données de la production vers l’évaluation pour tester entièrement votre site et le stocker dans un environnement de quasi-production avec tous les services et paramètres.

>[!NOTE]
>
>Pour transférer directement des médias d’environnements distants à distants, vous devez activer le transfert de l’agent ssh. Voir [Guide GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Sauvegarde de la base de données

Il est recommandé de créer une sauvegarde de la base de données. La procédure suivante utilise les conseils de la section [Sauvegarder la base de données](../storage/database-dump.md).

**Pour vider la base de données** :

1. [Utilisez SSH pour vous connecter à l’environnement distant](../development/secure-connections.md#use-an-ssh-command) qui contient la base de données à copier.

1. Répertoriez les relations entre les environnements et notez les informations de connexion à la base de données.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Pour Pro Staging et Production, le nom de la base de données est dans la variable `MAGENTO_CLOUD_RELATIONSHIPS` (généralement identique au nom de l&#39;application et au nom d&#39;utilisateur).

1. Créez une sauvegarde de la base de données. Pour choisir un répertoire cible pour l’image mémoire de la base de données, utilisez l’option `--dump-directory`.

   Pour les environnements de démarrage et les environnements d’intégration Pro, utilisez `main` comme nom de base de données :

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Options de vidage :
   - `--dump-directory=<dir>` : sélectionnez un répertoire cible pour l&#39;image mémoire de la base de données
   - `--remove-definers` : supprimer les instructions DEFINER de l&#39;image mémoire de la base de données

1. Bien que la méthode ECE-Tools soit préférée, une autre méthode consiste à créer un fichier de vidage de base de données en utilisant MySQL natif au format GZIP.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Si vous avez configuré l’authentification à deux facteurs sur l’environnement cible, il est préférable d’exclure les tables 2FA associées pour éviter de la reconfigurer après la migration de la base de données :

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Saisissez `logout` pour mettre fin à la connexion SSH.

### Déposer et recréer la base de données

Lors de l’import de données, vous devez supprimer et créer une base de données.

**Pour supprimer et recréer la base de données** :

1. Établissez un [tunnel SSH](../development/secure-connections.md#ssh-tunneling) vers l’environnement distant.

1. Connectez-vous au service de base de données.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. À l’invite de `MariaDB [main]>`, déposez la base de données.

   Pour l’intégration de Starter et Pro :

   ```shell
   drop database main;
   ```

   Pour les environnements de production et d’évaluation :

   ```shell
   drop database <database_name>;
   ```

1. Recréez la base de données.

   Pour l’intégration de Starter et Pro :

   ```shell
   create database main;
   ```

1. Importez la base de données.

   Importer pour production :

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Importer pour l’évaluation :

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Ces commandes décompressent le fichier de vidage de la base de données, suppriment les instructions `DEFINER` et importent la base de données à l&#39;aide des informations d&#39;identification spécifiées.
