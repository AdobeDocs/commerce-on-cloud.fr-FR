---
title: Bonnes pratiques pour mettre à niveau votre projet
description: Consultez une liste des bonnes pratiques pour mettre à niveau vos fichiers de projet.
feature: Cloud, Best Practices, Upgrade
exl-id: 64f92739-9170-4cbf-90ef-aab6593a37ca
TQID: https://experienceleague.adobe.com/Nnr9fNMT210WTnaLTWyRM-YCWRXrZuOv0m-EZYpzKVw
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 696
ht-degree: 0%

---

# Bonnes pratiques pour mettre à niveau votre projet

Suivez les bonnes pratiques en matière de versions et de déploiement, et utilisez le workflow [&#x200B; Mises à niveau et correctifs &#x200B;](../development/commerce-version.md) pour mettre à niveau votre application. Suivez les instructions suivantes pour planifier votre travail de mise à niveau et après la mise à niveau :

- **Sauvegardez votre projet**-Avant de mettre à niveau Adobe Commerce et toute extension tierce ou personnalisée, sauvegardez la base de données dans les environnements d’intégration, d’évaluation et de production. Pour plus d&#39;informations, consultez la section [&#x200B; Sauvegarder la base de données &#x200B;](../development/commerce-version.md#project-backup).

- **Rechercher les problèmes de compatibilité**-

   - Vérifiez que tous les thèmes personnalisés sont compatibles avec la nouvelle version d’Adobe Commerce

   - Après la mise à niveau des extensions tierces et personnalisées, utilisez la commande `magento-cloud local:build` pour valider les dépendances du compositeur avant le déploiement, puis exécutez l’outil de compatibilité de [mise à niveau](#use-the-upgrade-compatibility-tool) pour identifier les incompatibilités de niveau de code entre vos versions actuelle et cible. Utilisez ensuite l’[outil de compatibilité de mise à niveau](https://fluffyjaws.adobe.com/#use-the-upgrade-compatibility-tool) pour identifier et classer par priorité les incompatibilités au niveau du code avant leur déploiement dans les environnements d’intégration, d’évaluation ou de production.

   - Consultez les notes de mise à jour et la documentation de l’extension Adobe Commerce pour vous assurer que vous avez mis en œuvre toutes les solutions ou modifications de configuration requises pour résoudre les problèmes fonctionnels et les bogues connus liés à la mise à niveau de la version d’Adobe Commerce et de ses extensions.

   - Assurez-vous que les versions de service installées sont compatibles avec la nouvelle version d’Adobe Commerce et mettez à niveau les services si nécessaire. Voir [Services](../services/services-yaml.md).

   - Testez votre base de données pour résoudre les problèmes introduits par les mises à jour de la version d’Adobe Commerce et des extensions.

   - Apportez les mises à jour requises aux paramètres spécifiques à l’environnement avant de procéder au déploiement sur l’environnement distant.

   - Assurez-vous que la version du service de recherche est compatible avec la version du client PHP. Voir [Configuration d’Elasticsearch](../services/elasticsearch.md) ou [Configuration d’OpenSearch](../services/opensearch.md).

- **Vérifier la connectivité de la base de données et le stockage disponible dans les environnements distants**-

   - Utilisez SSH pour vous connecter au serveur distant et vérifier la connexion à la base de données MySQL. Voir [Connexion à la base de données](../services/mysql.md#connect-to-the-database).

   - Vérifier le stockage disponible dans l’environnement distant : utilisez la commande `disk free` pour afficher et gérer l’espace disque disponible dans vos environnements cloud. Voir [&#x200B; Gérer l’espace disque &#x200B;](../storage/manage-disk-space.md).

      - Vérifiez la taille de la base de données mise à niveau et que le fichier `services.yaml` dispose de suffisamment d&#39;espace disque alloué.

      - Libérez de l’espace disque : videz le cache et nettoyez les répertoires `/log` et `/tmp` avant de procéder au déploiement.

- **Planifiez et effectuez une mise à niveau réussie sur les environnements locaux et d’intégration, avant le déploiement vers l’évaluation**-Après la mise à niveau, testez votre déploiement et résolvez les problèmes.

- **Fusionnez le code dans l’environnement d’évaluation, puis dans l’environnement de production** testez et résolvez tous les problèmes dans l’environnement d’évaluation avant d’envoyer les modifications dans l’environnement de production.

- **Terminer les tâches après la mise à niveau**-

   - Utilisez SSH pour vous connecter au serveur distant et vérifier les éléments suivants :

      - Vérifiez le statut de l’indexeur et réindexez-le si nécessaire. Voir [Gestion des indexeurs](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=fr) dans le _Guide de configuration_.

      - Vérifiez les journaux `cron` et la table `cron_schedule` dans la base de données Adobe Commerce pour connaître l’état cron, puis réexécutez les tâches cron si nécessaire.
Voir [Journalisation](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=fr#logging) dans le _Guide de configuration_.

   - Effectuez le test d’acceptation utilisateur UAT après la mise à niveau sur les environnements d’évaluation et de production et résolvez tous les problèmes liés aux mises à niveau des extensions tierces et personnalisées.

## Utilisation de l’outil de compatibilité de mise à niveau

Exécutez l’outil de compatibilité de mise à niveau (UCT) dans le cadre de votre analyse préalable à la mise à niveau pour comprendre la portée et l’impact d’une mise à niveau.

- L’UCT compare votre instance actuelle à une version cible d’Adobe Commerce, renvoyant une liste de problèmes critiques, d’erreurs et d’avertissements qui doivent être corrigés avant la mise à niveau.
- Utilisez `--coming-version (-c)` pour effectuer une comparaison avec la version cible prévue et `--ignore-current-version-compatibility-issues` pour vous concentrer uniquement sur les nouveaux problèmes introduits par la mise à niveau.
- Traitez le rapport HTML UCT comme une entrée de votre liste de contrôle de mise à niveau, ainsi que des contrôles de compatibilité d’extension, de versions de service et de base de données.

Pour plus d’informations sur la configuration et l’utilisation, voir :

- [Présentation de l’outil de compatibilité de mise à niveau](https://experienceleague.adobe.com/fr/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/overview)
- [Exécution de l’outil de compatibilité de mise à niveau](https://experienceleague.adobe.com/fr/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/run)

Pour les commerçants Cloud qui utilisent l’outil d’analyse à l’échelle du site, vous pouvez également déclencher l’UCT à partir du tableau de bord et télécharger le rapport HTML directement à partir du widget. Voir Intégration de l’outil [Analyse à l’échelle du site](https://experienceleague.adobe.com/fr/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/integrate-analysis-tool).
