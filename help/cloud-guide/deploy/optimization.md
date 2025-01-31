---
title: Optimisation du déploiement dans le cloud
description: Découvrez comment optimiser le processus de déploiement d’Adobe Commerce sur les projets d’infrastructure cloud, notamment en réduisant les temps d’arrêt, le déploiement de contenu statique, le déploiement basé sur des scénarios et les assistants intelligents.
feature: Cloud, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Optimiser le déploiement

Les performances du site peuvent diminuer pendant le processus de déploiement. La durée pendant laquelle un site est en mode de maintenance lors du déploiement sur un site de production dépend de nombreux facteurs, tels que la configuration de l’environnement et la quantité de contenu qu’un site contient. La première bonne pratique pour optimiser votre déploiement dans le cloud consiste à [mettre à niveau pour utiliser les `ece-tools`](../dev-tools/install-package.md) afin de bénéficier des fonctionnalités du package, telles que les commandes permettant de créer une sauvegarde de la base de données et de vérifier la configuration de l’environnement.

Les rubriques suivantes peuvent vous aider à mieux comprendre comment optimiser le processus de déploiement :

- [Processus de déploiement cloud](process.md)
Le processus de déploiement dans le cloud comprend trois phases. Vous pouvez tirer parti des forces et des faiblesses de chaque phase.

- [Aucun déploiement sans interruption](reduce-downtime.md)
Découvrez ce qui se passe pendant le déploiement et comment réduire les temps d’arrêt de vos expériences de magasin lors d’une mise à jour de l’environnement de production.

- [Déploiement de contenu statique](static-content.md)
Le meilleur moyen d’optimiser votre déploiement dans le cloud consiste à contrôler comment et quand générer du contenu statique.

- [Assistants intelligents](smart-wizards.md)
Le package `ece-tools` fournit les commandes de l’assistant intelligent pour évaluer rapidement la configuration de votre projet.

- [Suivi des déploiements avec New Relic](../monitor/track-deployments.md)
Utilisez le service New Relic pour surveiller les événements de déploiement et analyser l’impact du déploiement sur les performances globales.
