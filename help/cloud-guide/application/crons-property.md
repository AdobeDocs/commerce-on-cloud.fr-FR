---
title: Propriété Cron
description: Consultez des exemples sur la façon de configurer la propriété « crons » dans le fichier  [!DNL Commerce]  configuration de l’application.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Propriété Cron

Adobe Commerce utilise la propriété `crons` pour planifier des activités répétitives. Il est idéal pour planifier l’exécution d’une tâche spécifique à certains moments de la journée. En raison de la nature des environnements en lecture seule, une seule tâche cron peut s’exécuter à la fois sur l’instance web pour Adobe Commerce dans les projets d’infrastructure cloud. Il est recommandé de ventiler les tâches de longue durée en tâches plus petites et en file d’attente. Vous pouvez également créer une [&#x200B; instance de travail &#x200B;](workers-property.md).

Adobe vous recommande d’exécuter `crons` en tant que [propriétaire du système de fichiers](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=fr). N’exécutez __ `crons` en tant que `root` ou utilisateur du serveur web.

Cette configuration est différente des déploiements sur site d’Adobe Commerce, qui comportent plusieurs tâches cron par défaut. Voir [Configuration des tâches cron](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=fr) dans le _Guide de configuration_.

## Configurer les tâches cron

La propriété `crons` décrit les processus qui sont déclenchés selon un planning. Chaque tâche requiert un nom et les options suivantes :

- `spec` : expression cron utilisée pour la planification.
- `cmd` : commande à exécuter sur `start` et `stop`.
- `shutdown_timeout`—(_Facultatif_) Si une tâche cron est annulée, il s&#39;agit du nombre de secondes après lequel un signal SIGKILL est envoyé pour arrêter la tâche ou le processus. La valeur par défaut est de 10 secondes.
- `timeout`—(_Facultatif_) Durée maximale pendant laquelle une tâche cron peut s’exécuter avant la temporisation. La valeur par défaut est la valeur maximale autorisée de 86400 secondes (24 heures).

Par défaut, chaque projet cloud Commerce présente la configuration de `crons` par défaut suivante dans le fichier `.magento.app.yaml` :

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Si votre projet nécessite des tâches cron personnalisées, vous pouvez les ajouter à la configuration `crons` par défaut. Voir [&#x200B; Création d’une tâche cron &#x200B;](#build-a-cron-job).

### `crontab`

Adobe Commerce a ajouté une option de configuration auto-crons uniquement aux projets Pro pour prendre en charge la configuration de `crons` en libre-service dans les environnements d’évaluation et de production. Si cette option est activée, vous pouvez utiliser `crontab` pour passer en revue la configuration cron. Cette option n’est _pas_ disponible avec les projets de démarrage.

Bien que vous puissiez utiliser `crontab` pour passer en revue la configuration sur les projets Pro, Adobe Commerce n’utilise pas `crontab` pour exécuter des tâches cron pour les sites déployés sur l’infrastructure cloud.

**Pour vérifier la configuration cron sur les environnements Pro** :

1. Utilisez [SSH](../development/secure-connections.md#use-an-ssh-command) pour vous connecter à l’environnement distant.

1. Répertoriez les processus cron planifiés.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Si la commande `crontab -l` renvoie une erreur `Command not found` (dans les environnements d’évaluation et de production Pro uniquement), vous devez [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour activer l’option de configuration en libre-service auto-crons sur votre projet.

L’exemple suivant illustre la sortie `crontab` pour un environnement qui ne dispose que de la configuration `crons` par défaut :

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Création d’une tâche cron

Une tâche cron inclut la spécification de planification et de minutage, ainsi que la commande à exécuter à l’heure planifiée. Pour les environnements Starter et Pro `integration`, l’intervalle minimum est d’une fois toutes les cinq minutes. Pour les environnements d’évaluation et de production Pro, l’intervalle minimum est d’une fois par minute. Sur Adobe Commerce sur les infrastructures cloud, vous ajoutez des tâches cron personnalisées au fichier `.magento.app.yaml` dans la section `crons` . Le format général est `spec` pour la planification et `cmd` pour spécifier la commande ou le script personnalisé à exécuter.

### Spécification

Adobe Commerce utilise une expression à cinq valeurs pour une spécification de `crons` (spec) : `* * * * *`

1. Minute (0 à 59) Pour tous les environnements Starter et Pro, la fréquence minimale prise en charge pour les tâches cron est de cinq minutes. Vous devrez peut-être configurer les paramètres dans votre administrateur.
2. Heure (0 à 23)
3. Jour du mois (1 à 31)
4. Mois (1 à 12)
5. Jour de la semaine (0 à 6) (du dimanche au samedi ; 7 correspond également au dimanche sur certains systèmes)

Voici quelques exemples :

- `00 */3 * * *` s’exécute toutes les trois heures à la première minute (12 h, 3 h et 6 h)
- `20 */8 * * *` s’exécute toutes les 8 heures à la minute 20 (12 h 20, 8 h 20 et 16 h 20)
- `00 00 * * *` fonctionne une fois par jour à minuit
- `00 * * * 1` fonctionne une fois par semaine le lundi à minuit.

>[!NOTE]
>
>L’heure de `crons` spécifiée dans le fichier `.magento.app.yaml` est basée sur le fuseau horaire du serveur et non sur celui spécifié dans les valeurs de configuration du magasin dans la base de données.

Lorsque vous déterminez la planification, tenez compte du temps nécessaire pour terminer la tâche. Par exemple, si vous exécutez une tâche toutes les trois heures et que son exécution dure 40 minutes, vous pouvez envisager de modifier la durée planifiée.

### Commande

Le `cmd` spécifie la commande ou le script personnalisé à exécuter. Le format du script de commande peut inclure les éléments suivants :

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Par exemple :

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

Dans cet exemple, `<path-to-php-binary>` est `/usr/bin/php`. Le répertoire d’installation, qui comprend l’ID de projet, est `/app/abc123edf890/bin/magento` et l’action de script est `export:start catalog_category_product`.

### Ajouter des tâches cron personnalisées à votre projet

Sur la plateforme d’Adobe Commerce sur l’infrastructure cloud, vous pouvez ajouter des personnalisations à la section `crons` du fichier [`.magento.app.yaml`](../application/configure-app-yaml.md).

>[!NOTE]
>
>Pour les environnements Starter et Pro `integration`, l’intervalle minimum est d’une fois toutes les cinq minutes. Pour les environnements d’évaluation et de production Pro, l’intervalle minimum est d’une fois par minute. Vous ne pouvez pas configurer d’intervalles plus fréquents que les intervalles minimaux par défaut.

Dans les projets Adobe Commerce Pro, la fonction [auto-crons](#set-up-cron-jobs) doit être activée sur votre projet avant que vous puissiez ajouter des tâches cron personnalisées aux environnements d’évaluation et de production à l’aide du fichier `.magento.app.yaml`. Si cette fonctionnalité n’est pas activée, [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour activer les crons automatiques.

**Pour ajouter des tâches cron personnalisées** :

1. Dans votre environnement de développement local, modifiez le fichier `.magento.app.yaml` dans le répertoire `/app` d’Adobe Commerce.

1. Dans la section `crons` , ajoutez votre personnalisation à l’aide du format suivant :

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   Dans l’exemple suivant, la tâche `productcatalog` exporte le catalogue de produits toutes les huit heures, 20 minutes après l’heure.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Ajout, validation et modifications de code push.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Mettre à jour les traitements cron

Pour ajouter, supprimer ou mettre à jour une tâche personnalisée, modifiez la configuration dans la section `crons` du fichier `.magento.app.yaml`. Ensuite, testez les mises à jour dans l’environnement de `integration` distant avant d’envoyer les modifications aux environnements d’évaluation et de production.

## Désactiver les tâches cron

Vous pouvez désactiver manuellement les tâches cron avant d’effectuer des tâches de maintenance telles que la réindexation ou le nettoyage du cache pour éviter des problèmes de performances. Vous pouvez utiliser la `cron:disable` de commande de l’interface de ligne de commande `ece-tools` pour désactiver toutes les tâches cron et arrêter tous les processus cron actifs.

**Pour désactiver les tâches cron** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Désactivez les tâches cron et arrêtez les processus cron actifs.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. Une fois que vous avez terminé les tâches de maintenance requises, assurez-vous de réactiver les tâches cron.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Résolution des problèmes liés aux tâches cron

Adobe a mis à jour le package Adobe Commerce sur l’infrastructure cloud pour optimiser le traitement cron sur la plateforme Adobe Commerce sur l’infrastructure cloud et résoudre les problèmes liés à cron. Si vous rencontrez des problèmes lors du traitement de cron, assurez-vous que votre projet utilise la version la plus récente du package `ece-tools`. Voir [Mise à jour des outils CEE](../dev-tools/update-package.md).

Vous pouvez consulter les informations de traitement cron dans les fichiers journaux au niveau de l’application pour chaque environnement. Voir [Journaux d’application](../test/log-locations.md#application-logs).

Consultez les articles d’assistance Adobe Commerce suivants pour obtenir de l’aide sur la résolution des problèmes liés à cron :

- [Les tâches cron verrouillent les tâches d’autres groupes](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html?lang=fr)

- [Réinitialiser manuellement les tâches cron bloquées sur le cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html?lang=fr)
