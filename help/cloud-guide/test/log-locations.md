---
title: Affichage et gestion des journaux
description: Identifiez les types de fichiers journaux disponibles dans l’infrastructure cloud et où les trouver.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: 7615347cd5b528406c2a0e72be3450350655eeb9
workflow-type: tm+mt
source-wordcount: '1083'
ht-degree: 0%

---

# Affichage et gestion des journaux

Les journaux d’Adobe Commerce sur les projets d’infrastructure cloud sont utiles pour résoudre les problèmes liés à [création et déploiement de hooks](../application/hooks-property.md), aux services cloud et à l’application Adobe Commerce.

Vous pouvez afficher les journaux du système de fichiers, du [!DNL Cloud Console] et de l’interface de ligne de commande `magento-cloud`.

- **Système de fichiers** : le répertoire système `/var/log` contient les journaux de tous les environnements. Le répertoire `var/log/` contient des journaux spécifiques à l’application, propres à un environnement particulier. Ces répertoires ne sont pas partagés entre les nœuds d’un cluster. Dans les environnements de production et d’évaluation Pro, vous devez vérifier les journaux sur chaque nœud.

- **[!DNL Cloud Console]** : vous pouvez voir les informations du journal de génération, de déploiement et de post-déploiement dans la liste des _messages_ de l&#39;environnement.

- **Interface de ligne de commande Cloud** : vous pouvez afficher les journaux d’environnement local à l’aide de la commande `magento-cloud log` ou les journaux d’environnement distant à l’aide de la commande `magento-cloud ssh`.

## Emplacements du journal

Les journaux système sont stockés aux emplacements suivants :

- Intégration : `/var/log/<log-name>.log`
- Staging pro : `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Production professionnelle : `/var/log/platform/<project-ID>/<log-name>.log`

La valeur de `<project-ID>` dépend du projet et du statut de l’environnement : Évaluation ou Production. Par exemple, avec un ID de projet de `yw1unoukjcawe`, l’utilisateur de l’environnement d’évaluation est `yw1unoukjcawe_stg` et l’utilisateur de l’environnement de production est `yw1unoukjcawe`.

Dans cet exemple, le journal de déploiement est le suivant : `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Affichage des journaux d’environnement distant

La plupart des journaux incluent des événements qui se produisent dans l’environnement distant. Pour Pro, il existe plusieurs nœuds et chaque nœud possède des journaux uniques. Utilisez ce qui suit pour afficher la liste de tous les hôtes :

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Exemple de réponse :

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**Pour afficher la liste des journaux d’environnement distant** :

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Exemple pour Pro :

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**Pour afficher un journal distant** :

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Exemple pour Pro :

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Pour les environnements d’évaluation et de production Pro, la rotation, la compression et la suppression automatiques des journaux sont activées pour les fichiers journaux ayant un nom de fichier fixe. Chaque type de fichier journal possède un modèle rotatif et une durée de vie.
>Vous trouverez des détails complets sur la rotation des journaux de l’environnement et la durée de vie des journaux compressés dans : `/etc/logrotate.conf` et `/etc/logrotate.d/<various>`.
>Pour les environnements d’évaluation et de production Pro, vous devez [soumettre un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour demander des modifications dans la configuration de la rotation du journal.

>[!TIP]
>
>La rotation de journal ne peut pas être configurée dans les environnements d&#39;intégration Pro.
>Pour l’intégration Pro, vous devez mettre en œuvre une solution/un script personnalisé et [configurer votre cron](../application/crons-property.md) pour exécuter le script si nécessaire.

>[!NOTE]
>
>Les environnements de projet de démarrage n’ont pas de rotation de journal.

## Créer et déployer des journaux

Après avoir envoyé les modifications à votre environnement, vous pouvez vérifier la journalisation de chaque hook dans le fichier `var/log/cloud.log`. Le journal contient les messages de démarrage et d’arrêt pour chaque hook. Dans l&#39;exemple suivant, les messages sont « `Starting post-deploy.` » et « `Post-deploy is complete.` »

Vérifiez les horodatages sur les entrées du journal, puis recherchez les journaux d’un déploiement spécifique. Voici un exemple condensé de sortie de journal que vous pouvez utiliser à des fins de dépannage :

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Lorsque vous configurez votre environnement cloud, vous pouvez configurer des [notifications Slack et par e-mail basées sur les journaux](../environment/set-up-notifications.md) pour des actions de génération et de déploiement.

Les journaux suivants ont un emplacement commun pour tous les projets cloud :

- **Journal de déploiement** : `var/log/cloud.log`
- **Dernier journal des erreurs de déploiement** : `var/log/cloud.error.log`
- **Journal de débogage** : `var/log/debug.log`
- **Journal des exceptions** : `var/log/exception.log`
- **Journal système** : `var/log/system.log`
- **Journal d’assistance** : `var/log/support_report.log`
- **Rapports** : `var/report/`

Bien que le fichier `cloud.log` contienne des commentaires provenant de chaque étape du processus de déploiement, les journaux créés par le hook de déploiement sont propres à chaque environnement. Le journal de déploiement spécifique à un environnement se trouve dans les répertoires suivants :

- **Intégration Starter et Pro** : `/var/log/deploy.log`
- **Évaluation Pro** : `/var/log/platform/<project-ID>_stg/deploy.log`
- **Pro Production** : `/var/log/platform/<project-ID>/deploy.log`

### Journal de déploiement

Le journal de chaque déploiement se concatène dans le fichier `deploy.log` spécifique. L’exemple suivant imprime le journal de déploiement de l’environnement actuel dans le terminal :

```bash
magento-cloud log -e <environment-ID> deploy
```

Exemple de réponse :

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Journal des erreurs

Les messages d’erreur et d’avertissement générés lors du processus de déploiement sont écrits dans les fichiers `var/log/cloud.log` et `var/log/cloud.error.log`. Le fichier journal des erreurs cloud contient uniquement des erreurs et des avertissements du dernier déploiement. Un fichier vide indique un déploiement réussi sans erreur.

Vous pouvez afficher le fichier journal à l&#39;aide du SSH [Cloud CLI](#view-remote-environment-logs), ou vous pouvez utiliser ECE-Tools pour afficher les erreurs avec des suggestions :

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Exemple de réponse :

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

La plupart des messages d’erreur contiennent une description et une action suggérée. Utilisez la référence [Message d&#39;erreur pour ECE-Tools](../dev-tools/error-reference.md) pour rechercher le code d&#39;erreur afin d&#39;obtenir d&#39;autres indications. Pour plus d’informations, utilisez l’utilitaire de dépannage du déploiement d’Adobe Commerce [&#128279;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html).

## Logs de l&#39;application

Comme pour les journaux de déploiement, les journaux d’application sont uniques pour chaque environnement :

| Fichier journal | Intégration de Starter et Pro | Description |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Journal de déploiement** | `/var/log/deploy.log` | Activité à partir du [hook de déploiement](../application/hooks-property.md). |
| **Log de post-déploiement** | `/var/log/post_deploy.log` | Activité à partir du [hook de post-déploiement](../application/hooks-property.md). |
| **Log cron** | `/var/log/cron.log` | Sortie des tâches cron. |
| **Journal des accès Nginx** | `/var/log/access.log` | Au démarrage de Nginx, erreurs HTTP pour les répertoires manquants et les types de fichiers exclus. |
| **Journal des erreurs Nginx** | `/var/log/error.log` | Messages de démarrage utiles pour le débogage des erreurs de configuration associées à Nginx. |
| **Log d’accès PHP** | `/var/log/php.access.log` | Requêtes au service PHP. |
| **Log FPM PHP** | `/var/log/app.log` | |

Pour les environnements d’évaluation et de production Pro, les journaux Déployer, Post-déploiement et Cron ne sont disponibles que sur le premier nœud du cluster :

| Fichier journal | Évaluation de pro | Pro Production |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Journal de déploiement** | Premier nœud uniquement :<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | Premier nœud uniquement :<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Log de post-déploiement** | Premier nœud uniquement :<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | Premier nœud uniquement :<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Log cron** | Premier nœud uniquement :<br>`/var/log/platform/<project-ID>_stg*/cron.log` | Premier nœud uniquement :<br>`/var/log/platform/<project-ID>/cron.log` |
| **Journal des accès Nginx** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Journal des erreurs Nginx** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **Log d’accès PHP** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **Log FPM PHP** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### Fichiers journaux archivés

Les journaux d’application sont compressés et archivés une fois par jour et conservés pendant **30 jours**. Les journaux compressés sont nommés à l’aide d’un identifiant unique qui correspond au `Number of Days Ago + 1`. Par exemple, sur les environnements de production Pro, un journal d’accès PHP datant de 21 jours est stocké et nommé comme suit :

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

Les fichiers journaux archivés sont toujours stockés dans le répertoire où se trouvait le fichier d’origine avant la compression.

>[!NOTE]
>
>Les fichiers journaux **Déploiement** et **Post-déploiement** ne sont pas pivotés ni archivés. L’historique de déploiement complet est écrit dans ces fichiers journaux.

## Logs de service

Comme chaque service s’exécute dans un conteneur distinct, les journaux de service ne sont pas disponibles dans l’environnement d’intégration. Adobe Commerce sur les infrastructures cloud permet d’accéder au conteneur du serveur web dans l’environnement d’intégration uniquement. Les emplacements de journaux de service suivants sont destinés aux environnements de production et d&#39;évaluation Pro :

- **Journal Redis** : `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Journal Elasticsearch** : `/var/log/elasticsearch/elasticsearch.log`
- **Journal de nettoyage de la mémoire Java** : `/var/log/elasticsearch/gc.log`
- **Journal des e-mails** : `/var/log/mail.log`
- **Journal des erreurs MySQL** : `/var/log/mysql/mysql-error.log`
- **Log lent de MySQL** : `/var/log/mysql/mysql-slow.log`
- **Journal RabbitMQ** : `/var/log/rabbitmq/rabbit@host1.log`

Les journaux de service sont archivés et enregistrés pendant différentes périodes, selon le type de journal. Par exemple, les journaux MySQL ont la durée de vie la plus courte : ils sont supprimés après sept jours.

>[!TIP]
>
>Les emplacements des fichiers journaux dans l’architecture mise à l’échelle dépendent du type de nœud. Voir la section [Emplacements du journal dans la rubrique Architecture mise à l’échelle](../architecture/scaled-architecture.md#log-locations).

## Données de journal pour la production et l’évaluation Pro

Dans les environnements de production et d’évaluation Pro, utilisez la gestion des journaux [New Relic](../monitor/log-management.md) intégrée à votre projet pour gérer les données de journaux agrégées à partir de tous les journaux associés à votre projet d’infrastructure Adobe Commerce sur le cloud.

L’application Journaux New Relic fournit un tableau de bord de gestion des journaux centralisé pour dépanner et surveiller Adobe Commerce sur les infrastructures cloud et les environnements d’évaluation et de production. Le tableau de bord permet également d’accéder aux données de journal pour les services Fast CDN, Image Optimization et Web Application Firewall (WAF). Voir [Services New Relic](../monitor/new-relic-service.md).
