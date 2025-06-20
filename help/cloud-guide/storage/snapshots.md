---
title: Gestion des sauvegardes
description: Découvrez comment créer et restaurer manuellement une sauvegarde pour votre projet d’infrastructure cloud d’Adobe Commerce.
feature: Cloud, Paas, Snapshots, Storage
exl-id: e73a57e7-e56c-42b4-aa7b-2960673a7b68
source-git-commit: 13cb5e3231c2173d5687aec3e4e64ecc154ee962
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 0%

---

# Gestion des sauvegardes

Vous pouvez effectuer une sauvegarde manuelle des environnements de démarrage actifs à tout moment à l’aide du bouton **[!UICONTROL Backup]** de la [!DNL Cloud Console] ou à l’aide de la commande `magento-cloud snapshot:create` .

Une sauvegarde ou _snapshot_ est une sauvegarde complète des données de l’environnement qui inclut toutes les données persistantes des services en cours d’exécution (base de données MySQL) et tous les fichiers stockés sur les volumes montés (var, pub/media, app/etc.). L’instantané n’inclut __ de code, car le code est déjà stocké dans le référentiel Git. Vous ne pouvez pas télécharger une copie d’un instantané.

>[!WARNING]
>
>Bien que les sauvegardes contiennent normalement le contenu des répertoires montés, y compris les répertoires Web publics comme `pub/media`, ne déplacez pas les fichiers de sortie de sauvegarde vers des répertoires Web publics comme `pub/media` ou `pub/static`.

La fonction de sauvegarde/instantané ne s’applique **pas** aux environnements d’évaluation et de production Pro, qui reçoivent par défaut des sauvegardes régulières à des fins de reprise après sinistre. Pour plus d&#39;informations, reportez-vous à la section [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery). Contrairement aux sauvegardes dynamiques automatiques dans les environnements d’évaluation et de production Pro, les sauvegardes ne sont **pas** automatiques. Il est de _votre_ responsabilité de créer manuellement une sauvegarde ou de configurer une tâche cron pour créer régulièrement une sauvegarde de vos environnements d’intégration Starter ou Pro.

## Création d’une sauvegarde manuelle

Vous pouvez créer une sauvegarde manuelle de n’importe quel environnement de démarrage actif et de l’environnement d’intégration Pro à partir de l’[!DNL Cloud Console] ou créer un instantané à partir de l’interface de ligne de commande Cloud. Vous devez disposer d’un [rôle d’administrateur](../project/user-access.md) pour l’environnement.

>[!NOTE]
>
>Vous pouvez créer une sauvegarde du code directement sur les clusters Pro Production et Staging en exécutant la commande suivante dans le terminal - en l’ajustant pour tous les dossiers/chemins que vous souhaitez inclure/exclure :
>
>```bash
>mkdir -p var/support
>/usr/bin/nice -n 15 /bin/tar -czhf var/support/code-$(date +"%Y%m%d%H%M%p").tar.gz app bin composer.* dev lib pub/*.php pub/errors setup vendor --exclude='pub/media'
>```

**Pour créer une sauvegarde de base de données de l&#39;environnement Pro** :

Pour créer un vidage de base de données de n&#39;importe quel environnement Pro, y compris l&#39;évaluation et la production, consultez l&#39;article [Créer un vidage de base de données](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud) de la base de connaissances.

**Pour créer une sauvegarde d’un environnement de démarrage à l’aide de l’[!DNL Cloud Console]** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .
1. Sélectionnez un environnement dans la barre de navigation du projet. L’environnement doit être actif.
1. Dans la vue _Sauvegardes_, cliquez sur **[!UICONTROL Backup]**. Cette option n&#39;est pas disponible pour un environnement Pro.

   ![ Sauvegarde ](../../assets/button-backup.png){width="150"}

**Pour créer une sauvegarde d’un environnement d’intégration à l’aide du[!DNL Cloud Console]** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .
1. Sélectionnez un environnement d’intégration/de développement dans la barre de navigation du projet. L’environnement doit être actif.
1. Sélectionnez l’option **[!UICONTROL Backup]** dans le menu supérieur droit. Cette option est disponible pour les environnements Starter et Pro.
1. Cliquez sur le bouton **[!UICONTROL Yes]** .

**Pour créer un instantané à l’aide de l’interface de ligne de commande `magento-cloud`** :

1. Sur votre station de travail locale, accédez au répertoire du projet.
1. Extrayez la branche d’environnement pour obtenir un instantané.
1. Créez l’instantané.

   ```bash
   magento-cloud snapshot:create --live
   ```

   Vous pouvez également utiliser la commande `magento-cloud backup` short. L’option `--live` laisse l’environnement en cours d’exécution pour éviter les temps d’arrêt. Pour obtenir la liste complète des options, saisissez `magento-cloud snapshot:create --help`.

   Exemple de réponse :

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Vérifiez les instantanés les plus récents.

   ```bash
   magento-cloud snapshot:list
   ```

   La liste renvoie des informations sur le statut de l’instantané :

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Restaurer une sauvegarde manuelle

Vous devez disposer d’un [accès administrateur](../project/user-access.md) à l’environnement. Vous avez jusqu’à **sept jours** pour _restaurer_ une sauvegarde manuelle. La restauration d’une sauvegarde ne modifie pas le code de la branche Git actuelle. La restauration d&#39;une sauvegarde de cette manière ne s&#39;applique pas aux environnements d&#39;évaluation et de production Pro ; consultez [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery).

Les délais de restauration varient en fonction de la taille de votre base de données :

- Une base de données volumineuse (200 Go et plus) peut prendre 5 heures
- la base de données moyenne (150 Go) peut prendre 2 1/2 heures
- Une petite base de données (60 Go) peut prendre 1 heure

>[!TIP]
>
>Restauration sans sauvegarde :
>
>- Pour restaurer le code précédent ou supprimer les extensions ajoutées dans un environnement, consultez [Restaurer le code](#roll-back-code).
>- _Pour restaurer un environnement instable sans sauvegarde_ reportez-vous à la section [Restaurer un environnement](../development/restore-environment.md).

**Pour restaurer une sauvegarde à l’aide de l’[!DNL Cloud Console]** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .
1. Sélectionnez un environnement dans la barre de navigation du projet.
1. Dans la vue _Sauvegardes_, choisissez une sauvegarde dans la liste _Stocké_. La fonction de sauvegarde ne s&#39;applique **pas** aux environnements Pro.
1. Dans le menu ![Plus](../../assets/icon-more.png){width="32"} (_plus_), cliquez sur **Restaurer**.
1. Vérifiez la restauration à partir des informations de sauvegarde et cliquez sur **Oui, restaurer**.

**Pour restaurer un instantané à l’aide de l’interface de ligne de commande Cloud** :

1. Sur votre station de travail locale, accédez au répertoire du projet.
1. Consultez la branche d’environnement à restaurer.
1. Répertorier tous les instantanés disponibles.

   ```bash
   magento-cloud snapshot:list
   ```

   La liste renvoie des informations sur les instantanés disponibles :

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Restaurez un instantané à l’aide de l’ID d’instantané de la liste.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Restauration d’un instantané de reprise après sinistre

Pour restaurer l&#39;instantané de reprise après sinistre dans les environnements d&#39;évaluation et de production Pro, [Importez l&#39;image mémoire de la base de données directement depuis le serveur](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## Restaurer le code

Les sauvegardes et les instantanés ne comprennent _pas_ une copie de votre code. Votre code est déjà stocké dans le référentiel Git. Vous pouvez donc utiliser des commandes basées sur Git pour restaurer (ou rétablir) le code. Par exemple, utilisez `git log --oneline` pour faire défiler les validations précédentes, puis utilisez [`git revert`](https://git-scm.com/docs/git-revert) pour restaurer le code d’une validation spécifique.

Vous pouvez également choisir de stocker le code dans une branche _inactive_. Utilisez les commandes Git pour créer une branche au lieu d’utiliser des commandes `magento-cloud`. Voir à propos des [commandes Git](../dev-tools/cloud-cli-overview.md#git-commands) dans la rubrique relative à l’interface de ligne de commande Cloud.

## Informations connexes

- [Sauvegarde de la base de données](database-dump.md)
- [Sauvegarde et reprise après sinistre](../architecture/pro-architecture.md#backup-and-disaster-recovery) pour les clusters Pro de production et d&#39;évaluation
