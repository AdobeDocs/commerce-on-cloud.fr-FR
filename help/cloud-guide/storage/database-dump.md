---
title: Sauvegarde de la base de données
description: Découvrez comment utiliser les outils ECE pour créer une sauvegarde de la base de données pour un projet d'infrastructure Adobe Commerce on cloud.
feature: Cloud, Iaas, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Sauvegarde de la base de données

Vous pouvez créer une copie de votre base de données à l’aide de la commande `ece-tools db-dump` sans capturer toutes les données d’environnement des services et des montages. Par défaut, cette commande crée des sauvegardes dans le répertoire `/app/var/dump-main` pour toutes les connexions à la base de données spécifiées dans la configuration de l&#39;environnement. L’opération de vidage de la base de données fait passer l’application en mode de maintenance, arrête les processus de file d’attente des clients et désactive les tâches cron avant que le vidage ne commence.

Tenez compte des instructions suivantes pour l’image mémoire de la base de données :

- Pour les environnements de production, Adobe recommande d’effectuer les opérations de vidage de la base de données pendant les heures creuses afin de minimiser les interruptions de service qui se produisent lorsque le site est en mode de maintenance.
- Si une erreur se produit lors de l’opération de vidage, la commande supprime le fichier de vidage pour préserver l’espace disque. Consultez les journaux pour plus de détails (`var/log/cloud.log`).
- Pour les environnements de production Pro, cette commande n’effectue le vidage qu’à partir de _un_ des trois nœuds à haute disponibilité, de sorte que les données de production écrites sur un autre nœud au cours du vidage peuvent ne pas être copiées. La commande génère un fichier `var/dbdump.lock` pour empêcher l’exécution de la commande sur plusieurs nœuds.
- Pour une sauvegarde de tous les services d’environnement, Adobe recommande de créer une [ sauvegarde ](snapshots.md).

Vous pouvez choisir de sauvegarder plusieurs bases de données en ajoutant les noms de base de données à la commande . L&#39;exemple suivant sauvegarde deux bases de données : `main` et `sales` :

```bash
php vendor/bin/ece-tools db-dump main sales
```

Utilisez la commande `php vendor/bin/ece-tools db-dump --help` pour accéder à plus d’options :

- `--dump-directory=<dir>` : sélectionnez un répertoire cible pour l&#39;image mémoire de la base de données
- `--remove-definers` : supprimer les instructions DEFINER de l&#39;image mémoire de la base de données

**Pour créer une image mémoire de base de données dans l’environnement d’évaluation ou de production** :

1. [Utilisez SSH pour vous connecter ou créez un tunnel pour vous connecter à l’environnement distant](../development/secure-connections.md) qui contient la base de données à copier.

1. Répertoriez les relations entre les environnements et notez les informations de connexion à la base de données.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. Créez une sauvegarde de la base de données. Pour choisir un répertoire cible pour l’image mémoire de la base de données, utilisez l’option `--dump-directory`.

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   Exemple de réponse :

   ```
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /tmp/qxmtlseakof6y/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. La commande `db-dump` crée un fichier d’archive `dump-<timestamp>.sql.gz` dans le répertoire de projet distant.

>[!TIP]
>
>Si vous souhaitez transférer ces données vers un environnement spécifique, consultez [Migration des données et des fichiers statiques](../deploy/staging-production.md#migrate-static-files).
