---
title: Bonnes pratiques de déploiement
description: Découvrez les bonnes pratiques de déploiement d’Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Bonnes pratiques de déploiement

Les scripts de build et de déploiement s’activent lorsque vous fusionnez du code dans un environnement distant. Ces scripts utilisent l’environnement [fichiers de configuration](../environment/overview.md) et le code de l’application pour fournir à l’infrastructure cloud les données et services appropriés. En outre, ces scripts sont utilisés pour installer ou mettre à jour l’application Adobe Commerce, des services tiers et des extensions personnalisées dans l’environnement cloud.

Le processus de création et de déploiement est légèrement différent pour chaque plan :

- **Plans de démarrage** : pour l’environnement d’intégration, chaque branche active crée et déploie un environnement complet pour l’accès et les tests. Testez entièrement votre code après sa fusion dans la branche `staging`. Pour lancer votre site, envoyez des `staging` à `master` pour le déployer dans l’environnement de production. Vous disposez d’un accès complet à toutes les branches via les commandes [!DNL Cloud Console] et CLI.

- **Plans Pro** : pour l’environnement d’intégration, chaque branche active crée et déploie un environnement complet pour l’accès et les tests. Fusionnez votre code dans la branche `integration` avant de le fusionner dans les environnements d’évaluation et de production. Vous pouvez fusionner dans les environnements d’évaluation et de production à l’aide de l’[!DNL Cloud Console] ou des commandes SSH et `magento-cloud` de l’interface de ligne de commande.

## Suivre le processus

Vous pouvez effectuer le suivi des actions de génération et de déploiement en temps réel à l’aide du terminal ou des messages de statut de [!DNL Cloud Console] (`in-progress`, `pending`, `success` ou `failed`) affichés pendant le processus de déploiement. Vous pouvez afficher les détails dans les fichiers journaux. Voir [ Afficher les journaux ](../test/log-locations.md).

Si vous utilisez des référentiels GitHub externes, le journal des opérations ne s’affiche pas dans la session GitHub. Cependant, vous pouvez toujours suivre l’activité dans l’interface pour le référentiel externe et le [!DNL Cloud Console]. Pour plus d&#39;informations, consultez la section [ Intégrations ](../integrations/overview.md).

>[!NOTE]
>
>Dans les environnements d’intégration, vous ne pouvez pas afficher les journaux de déploiement à partir du [!DNL Cloud Console]. Cette fonctionnalité est disponible uniquement pour les environnements de production et d’évaluation. Cependant, vous pouvez afficher les journaux de chaque phase du déploiement dans n’importe quel environnement à l’aide des journaux [créer et déployer](../test/log-locations.md#build-and-deploy-logs). Pour obtenir des informations de dépannage, voir la [Référence d’erreur de déploiement](../dev-tools/error-reference.md).

Vous pouvez activer le [suivi des déploiements avec New Relic](../monitor/track-deployments.md) pour surveiller les événements de déploiement et analyser les performances entre les déploiements.

## Bonnes pratiques relatives aux builds et au déploiement

Examinez ces bonnes pratiques et considérations concernant votre processus de déploiement :

- **Assurez-vous d’exécuter la version la plus récente du package `ece-tools`**

  Voir [ Notes de mise à jour pour les outils CEE](../release-notes/ece-tools-package.md).

- **Suivez le processus de création et de déploiement**

  Assurez-vous que vous disposez du code correct dans chaque environnement afin d’éviter de remplacer des configurations lors de la fusion de code entre des environnements. Par exemple, pour appliquer des modifications de configuration à tous les environnements, modifiez et testez les modifications dans l’environnement local avant de procéder au déploiement dans l’environnement d’intégration distant. Ensuite, déployez et testez les modifications dans l’environnement d’évaluation avant de procéder au déploiement en production. Lorsque vous fusionnez d’un environnement à un autre, le déploiement remplace tout le code de l’environnement, à l’exception de la configuration et des paramètres spécifiques à l’environnement.

- **Utiliser les mêmes variables dans les environnements**

  Les valeurs de ces variables peuvent différer d’un environnement à l’autre. Cependant, vous avez généralement besoin des mêmes variables dans chaque environnement. Voir [Gestion de la configuration pour les paramètres de magasin](../store/store-settings.md).

- **Conserver les valeurs de configuration et les données sensibles dans des variables spécifiques à un environnement**

  Ces valeurs incluent les variables spécifiées à l’aide de l’interface de ligne de commande Cloud, du [!DNL Cloud Console] ou ajoutées au fichier `env.php`. Voir [Niveaux de variable](../environment/variable-levels.md).

- **Vérifiez que tout le code est disponible dans la branche d’environnement**

  Le référencement de code d’autres branches, telles qu’une branche privée, peut entraîner des problèmes au cours du processus de création et de déploiement. Par exemple, si vous référencez un thème à partir d’une branche privée, il n’est pas accessible et ne peut pas être créé avec le code de l’application.

- **Ajouter des extensions, des intégrations et du code dans des branches itérées**

  Apportez et testez les modifications localement, appuyez sur `integration`, puis sur `staging` et `production`. Testez et résolvez les problèmes dans chaque environnement avant de fusionner les mises à jour dans l’environnement suivant. Certaines extensions et intégrations doivent être activées et configurées dans un ordre spécifique en raison des dépendances. L’ajout et le test de groupes peuvent faciliter considérablement votre processus de création et de déploiement et vous aider à déterminer où les problèmes se produisent.

- **Vérifier les versions et les relations des services et la possibilité de se connecter**

  Vérifiez les services disponibles pour votre application et assurez-vous que vous utilisez la version la plus récente et compatible. Voir [Relations de service](../services/services-yaml.md#service-relationships) et [Configuration requise](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=fr) dans le _Guide d’installation_ pour obtenir les versions recommandées.

- **Testez localement et dans l’environnement d’intégration avant le déploiement dans les environnements d’évaluation et de production**

  Identifiez et corrigez les problèmes dans vos environnements locaux et d’intégration afin d’éviter les interruptions de service prolongées lors du déploiement dans les environnements d’évaluation et de production.

  >[!TIP]
  >
  >Vous pouvez utiliser des commandes [assistant intelligent](../deploy/smart-wizards.md) pour vérifier que la configuration de votre projet cloud suit les bonnes pratiques de création et de configuration de déploiement, y compris la stratégie de déploiement de contenu statique (SCD).

- **Après avoir effectué les tests dans les environnements locaux et d’intégration, déployez et testez-les dans l’environnement d’évaluation**

  Voir [ Test d’évaluation et de production ](../test/staging-and-production.md).

- **Vérifiez la configuration de l’environnement de production**

  Avant le déploiement en production, effectuez les tâches suivantes :

   - Assurez-vous que vous pouvez vous connecter aux trois nœuds de l’environnement de production à l’aide de [SSH](../development/secure-connections.md).

   - Vérifiez que les indexeurs sont définis sur _Mise à jour selon le calendrier_. Voir [Modes d’indexation](https://developer.adobe.com/commerce/php/development/components/indexing/) dans le _Guide du développeur de l’extension_.

   - Préparez l’environnement en mettant à jour les variables spécifiques à l’environnement dans le code de production, en vérifiant la disponibilité et la compatibilité du service et en apportant toute autre modification de configuration requise.

- **Surveiller le processus de déploiement**

  Passez en revue les messages de statut du déploiement et atténuez les problèmes si nécessaire. Consultez les [journaux](../test/log-locations.md#) de Cloud pour obtenir des messages de journal détaillés.

## Cinq phases de création et de déploiement de l’intégration

Les phases suivantes se produisent dans votre environnement de développement local et dans l’environnement d’intégration. Pour les plans Pro, le code n’est pas déployé dans les environnements d’évaluation ou de production au cours de ces phases initiales.

### Phase 1 : validation du code et de la configuration

Lorsque vous configurez initialement un projet, [le modèle d’infrastructure cloud](https://github.com/magento/magento-cloud) fournit une base pour les fichiers de code. Ce référentiel de code est cloné dans votre projet en tant que branche `master`.

- **Pour commencer** : `master` branche est votre environnement de production.
- **Pour pro** : `master` commence comme branche d&#39;origine pour l&#39;environnement d&#39;intégration.

Créez une branche à partir de `master` pour votre code personnalisé, vos extensions et modules, ainsi que les intégrations tierces. Il existe un environnement d’intégration à distance pour tester votre code dans le cloud.

Lorsque vous poussez votre code de votre espace de travail local vers le référentiel distant, une série de vérifications et de validations de code se termine avant le démarrage des scripts de build et de déploiement. Le serveur Git intégré valide les éléments que vous transmettez et apporte des modifications. Par exemple, si vous ajoutez un service OpenSearch, le serveur Git intégré vérifie que la topologie de votre cluster est modifiée en conséquence.

Si vous rencontrez une erreur de syntaxe dans un fichier de configuration, le serveur Git rejette la notification push. Voir [Bloc de protection](../development/protective-block.md).

Cette phase s’exécute également `composer install` pour récupérer les dépendances.

### Phase 2 : création

>[!NOTE]
>
>Pendant la phase de création, le site n’est pas en mode de maintenance et si des erreurs ou des problèmes se produisent, il n’est pas mis hors service. Il ne crée que le code qui a été modifié depuis la version précédente.

Cette phase crée la base de code et exécute les crochets dans la section `build` de `.magento.app.yaml`. Le hook de build par défaut est la commande `php ./vendor/bin/ece-tools` et effectue les opérations suivantes :

- Application de correctifs dans `vendor/magento/ece-patches` et de correctifs facultatifs spécifiques au projet dans `m2-hotfixes`
- Régénère le code et la configuration [injection de dépendance](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/glossary) (c’est-à-dire le répertoire `generated/`, qui comprend `generated/code` et `generated/metapackage`) à l’aide de `bin/magento setup:di:compile`.
- Vérifie si le fichier [`app/etc/config.php`](../store/store-settings.md) existe dans la base de code. Adobe Commerce génère automatiquement ce fichier s’il ne le détecte pas lors de la phase de création et inclut une liste de modules et d’extensions. S’il existe, la phase de création se poursuit normalement, comprime les fichiers statiques à l’aide de GZIP et se déploie, ce qui réduit le temps d’arrêt dans la phase de déploiement. Pour en savoir plus sur la personnalisation ou la désactivation de la compression de fichiers[&#128279;](../environment/variables-build.md) voir Options de création.

>[!WARNING]
>
>À ce stade, le cluster n’a pas été créé. N’essayez donc pas de vous connecter à une base de données ou de supposer qu’il existe un processus de démon actif.

Une fois l’application créée, elle est montée sur un **système de fichiers en lecture seule**. Vous pouvez configurer des points de montage spécifiques qui vont être lus/écrits. Vous ne pouvez pas FTP sur le serveur et ajouter des modules. Au lieu de cela, vous devez ajouter du code à votre référentiel local et exécuter `git push`, ce qui crée et déploie l’environnement. Pour la structure du projet, voir [Structure de répertoires de projet local](../project/file-structure.md).

### Phase 3 : Préparation du slug

Le résultat de la phase de création est un système de fichiers en lecture seule appelé _slug_. Cette phase crée une archive et place le slug dans un stockage permanent. La prochaine fois que vous intégrerez du code, si un service n’a pas été modifié, il utilisera le slug de l’archive.

- Rend l’intégration continue plus rapide en réutilisant le code inchangé
- Si le code change, met à jour le slug pour que la prochaine build réutilise
- Permet la restauration instantanée d’un déploiement, si nécessaire
- Inclut des fichiers statiques si le fichier `app/etc/config.php` existe dans le codebase

Le slug comprend tous les fichiers et dossiers **à l’exclusion des éléments suivants** configurés dans `magento.app.yaml` :

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Phase 4 : déploiement des slugs et du cluster

Vos applications et tous les services [principaux](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/glossary) sont configurés comme suit :

- Monte chaque service dans un conteneur, tel qu’un serveur web, OpenSearch, [!DNL RabbitMQ]
- Monte le système de fichiers en lecture-écriture (monté sur une grille de stockage distribuée à haute disponibilité)
- Configure le réseau afin que les services puissent se « voir » (et uniquement les uns les autres)

>[!NOTE]
>
>Apportez vos modifications dans une branche Git une fois la création et le déploiement terminés, puis effectuez une nouvelle notification push. Tous les systèmes de fichiers d’environnement sont en _lecture seule_. Un système en lecture seule garantit des déploiements déterministes et améliore considérablement la sécurité de votre site, car aucun processus ne peut écrire dans le système de fichiers. Il permet également de s’assurer que votre code est identique dans les environnements d’intégration, d’évaluation et de production.

### Phase 5 : crochets de déploiement

>[!NOTE]
>
>Cette phase met l’application [!DNL Commerce] en mode de maintenance jusqu’à ce que le déploiement soit terminé.

La dernière étape exécute un script de déploiement que vous pouvez utiliser pour rendre anonymes les données dans les environnements de développement, effacer les caches et interroger les outils d’intégration continue externes. Lorsque ce script s’exécute, vous avez accès à tous les services de votre environnement, tels que Redis.

Si le fichier `app/etc/config.php` n’existe pas dans le codebase, les fichiers statiques sont compressés à l’aide de `gzip` et déployés au cours de cette phase, ce qui allonge la durée de la phase de déploiement et de la maintenance du site.

>[!NOTE]
>
>Pour en savoir plus sur la personnalisation ou la désactivation de la compression de fichiers, voir [déployer des variables](../environment/variables-deploy.md).

Il existe deux hooks de déploiement. Le hook `pre-deploy.php` effectue le nettoyage et la récupération nécessaires des ressources et du code générés dans le hook de build. Le hook `php ./vendor/bin/ece-tools deploy` exécute une série de commandes et de scripts :

- Si Adobe Commerce n’est **pas installé**, il s’installe avec `bin/magento setup:install` et met à jour la configuration du déploiement, les `app/etc/env.php` et la base de données pour l’environnement spécifié, tel que Redis et les URL de site web. **Important :** lorsque vous avez terminé le [premier déploiement](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html?lang=fr) lors de la configuration, Adobe Commerce a été installé et déployé dans tous les environnements.

- Si Adobe Commerce **est installé**, effectuez les mises à niveau nécessaires. Le script de déploiement exécute `bin/magento setup:upgrade` pour mettre à jour le schéma et les données de la base de données (ce qui est nécessaire après les mises à jour de l’extension ou du code principal). Il met également à jour la configuration du déploiement, les `app/etc/env.php` et la base de données pour votre environnement. Enfin, le script de déploiement efface le cache d’Adobe Commerce.

- Le script génère éventuellement du contenu web statique à l’aide de la `magento setup:static-content:deploy` de commande .

- Utilise des portées (indicateur `-s` dans les scripts de version) avec un paramètre par défaut de `quick` pour la stratégie de déploiement de contenu statique. Vous pouvez personnaliser la stratégie à l’aide de la variable d’environnement [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). Pour plus d’informations sur ces options et fonctionnalités, consultez [Stratégies de déploiement de fichiers statiques](../deploy/static-content.md) et l’indicateur de `-s` pour [Déployer des fichiers d’affichage statiques](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html?lang=fr).

>[!NOTE]
>
>Le script de déploiement utilise les valeurs définies par les fichiers de configuration dans le répertoire `.magento`, puis le script supprime le répertoire et son contenu. Votre environnement de développement local n’est pas affecté.

### Après le déploiement : configurer le routage

Pendant l’exécution du déploiement , le processus interrompt le trafic entrant au point d’entrée pendant 60 secondes et reconfigure le routage de sorte que le trafic web arrive à votre cluster nouvellement créé.

Un déploiement réussi supprime le mode de maintenance pour permettre un accès normal et crée des fichiers de sauvegarde (BAK) pour les fichiers de configuration `app/etc/env.php` et `app/etc/config.php`.

Activez la génération de contenu statique à l’aide de la variable `SCD_ON_DEMAND` et configurez le hook [`post_deploy`](../application/hooks-property.md) de sorte qu’il vide le cache et précharge (réchauffe) le cache _une fois que_ le conteneur commence à accepter les connexions et _pendant_ le trafic entrant normal.

Pour consulter les journaux de génération et de déploiement, voir [Afficher les journaux](../test/log-locations.md#view-and-manage-logs).
