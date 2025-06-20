---
title: Gestion de l’espace disque
description: Découvrez comment gérer l’espace disque à l’aide de l’interface de ligne de commande.
feature: Cloud, Storage
exl-id: 1d13dc4e-56eb-4153-a8b1-48d2263ebc4c
source-git-commit: b8cabaad4b7805858563cecbe5ffc2fdb9aeac58
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Gestion de l’espace disque

La capacité de stockage totale de votre projet cloud figure dans votre contrat d’infrastructure cloud Adobe Commerce et sur la page [Compte](https://accounts.magento.cloud/user). Chaque carte de projet de votre compte indique le nombre d’_environnements_, la capacité de _stockage_ en Go et le nombre d’_utilisateurs_. Vous pouvez également utiliser la commande cloud suivante :

```bash
magento-cloud subscription:info | grep storage
```

Exemple de réponse :

```
| storage              | 51200
```

Lorsqu’un environnement de production ou d’évaluation Pro atteint ou dépasse 95 % de la capacité de stockage, l’outil de surveillance de l’infrastructure cloud déclenche une alerte d’assistance vous informant d’une augmentation automatique de la capacité de stockage.

Exemple de notification :

>[!BEGINSHADEBOX]

_« Notre surveillance a détecté que le stockage des fichiers sur votre cluster (project-id-environment) est presque complet. L’utilisation du disque atteint actuellement des niveaux critiques avec moins de 1 Go restant. Le volume de stockage partagé est actuellement en cours de mise à niveau de 60 à 70 Go pour que vos services restent opérationnels. Jetez un coup d’œil à l’utilisation des fichiers de production et d’évaluation pour voir si vous pouvez libérer de l’espace. »_

>[!ENDSHADEBOX]

>[!TIP]
>
>Adobe vous recommande de surveiller régulièrement votre capacité de stockage et de la maintenir bien en dessous des 90 % afin d’éviter ces augmentations automatiques. Une fois alloué, l’augmentation du stockage pour l’évaluation et la production Pro est permanente et ne peut pas être rétablie.

## Vérifier l’environnement d’intégration

Vous pouvez vérifier l’utilisation de l’espace disque pour votre environnement d’intégration à l’aide de l’interface de ligne de commande `magento-cloud`.

**Pour vérifier l’utilisation approximative de l’espace disque** :

```bash
magento-cloud db:size
```

Exemple de réponse :

```
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

Tous les montages partagent un disque. Vous pouvez vérifier l’utilisation de l’espace disque pour les montages à l’aide de l’interface de ligne de commande `magento-cloud`.

**Pour vérifier l’utilisation approximative de l’espace disque pour les montages** :

```bash
magento-cloud mount:size
```

Exemple de réponse :

```
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## Vérifier les clusters dédiés

Pour les environnements d’évaluation et de production Pro, vous pouvez vérifier l’utilisation de l’espace disque dans chaque environnement à l’aide de la commande `disk free`, qui indique la quantité d’espace disque utilisée par le système de fichiers. Vous devez utiliser SSH pour vous connecter à un environnement distant.

```bash
df -h
```

L’option `-h` affiche le rapport dans un format lisible par l’utilisateur (Ko, Mo ou Go).

Dans l’exemple de réponse suivant, le montage `/data/exports` indique l’espace disque pour le média et `/data/mysql/` montage indique l’espace disque pour la base de données :

```
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

Vous pouvez limiter la réponse en spécifiant un répertoire. Par exemple :

```bash
df -h var/
```

Exemple de réponse :

```
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## Allouer de l’espace disque

Deux [fichiers de configuration](../environment/overview.md) contrôlent l’allocation de l’espace disque dans les environnements cloud : le fichier `.magento.app.yaml` et le fichier `.magento/services.yaml`. Chaque fichier contient la propriété `disk` , qui définit la taille du disque en Mo pour la configuration correspondante. Vous pouvez uniquement modifier l’allocation de l’espace disque dans les environnements d’intégration et de démarrage Pro.

>[!IMPORTANT]
>
>Pour les environnements de production et d’évaluation Pro, vous devez [soumettre un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour modifier l’allocation de l’espace disque. Une augmentation de la taille des environnements de production et d&#39;évaluation Pro ne peut se produire qu&#39;à certains intervalles. Par conséquent, en fonction de l&#39;utilisation actuelle de votre espace disque, le support peut recommander d&#39;augmenter l&#39;allocation d&#39;espace disque d&#39;un minimum de 10 Go. Une fois alloué, l’augmentation du stockage pour l’évaluation et la production Pro ne peut pas être rétablie. Le stockage ne peut pas être réaffecté ou redistribué entre les ressources. Pour ajouter plus d’espace de stockage de fichiers, réduisez l’espace disque alloué à MySQL.

### Espace disque de l’application

Le fichier `.magento.app.yaml` contrôle l’[espace disque persistant](../application/properties.md#disk) disponible pour l’application.

**Pour augmenter l’espace disque de votre application** :

1. Dans votre environnement de développement local, ouvrez le fichier de configuration `.magento.app.yaml` .

1. Définissez une nouvelle valeur pour la propriété `disk` (en Mo).

   ```yaml
   disk: <value-mb>
   ```

1. Enregistrez les modifications dans le fichier .

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   Les modifications prennent effet après avoir envoyé le fichier YAML mis à jour dans l’environnement distant.

### Espace disque du service

Le fichier `.magento/services.yaml` contrôle l’espace disque disponible pour chaque service, tel que MySQL et Redis.

**Pour augmenter l’espace disque d’un service** :

1. Dans votre environnement de développement local, ouvrez le fichier de configuration `.magento/services.yaml` .

1. Ajoutez ou recherchez un service dans le fichier . Voir [en savoir plus sur la configuration des services](../services/services-yaml.md).

1. Définissez une nouvelle valeur pour la propriété de disque (en Mo).

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. Enregistrez les modifications dans le fichier .

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   Les modifications prennent effet après avoir envoyé le fichier YAML mis à jour dans l’environnement distant.

## Surveiller l’espace disque

Sur les environnements de production Pro, vous pouvez surveiller l’espace disque et d’autres indicateurs de performances à l’aide de la politique d’alerte Alertes gérées pour Adobe Commerce pour New Relic. Pour plus d’informations, consultez [Surveillance des performances avec des alertes gérées](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). Pour plus d’informations, voir [Bonnes pratiques pour résoudre les problèmes de performances des bases de données](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html?lang=fr).

## Pas d’espace restant

Le cache de build peut augmenter au fil du temps. Si vous recevez un avertissement indiquant `No space left on device`, essayez d’effacer le cache de version et de redéployer :

```bash
magento-cloud project:clear-build-cache
```
