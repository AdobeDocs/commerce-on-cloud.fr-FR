---
title: Optimisation du déploiement dans le cloud
description: Découvrez comment optimiser le processus de déploiement d’Adobe Commerce sur les projets d’infrastructure cloud, notamment en réduisant les temps d’arrêt, le déploiement de contenu statique, le déploiement basé sur des scénarios et les assistants intelligents.
feature: Cloud, Deploy, SCD
exl-id: 4315e2f4-06af-4a5c-9db9-e7b2f63660df
TQID: https://experienceleague.adobe.com/bd9n9CFrpyn1UZG6SX8qkoZGBOFd2N7z9Hoa1hQ8rew
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 230
ht-degree: 0%

---

# Optimiser le déploiement

Les performances du site peuvent diminuer pendant le processus de déploiement. La durée pendant laquelle un site est en mode de maintenance lors du déploiement sur un site de production dépend de nombreux facteurs, tels que la configuration de l’environnement et la quantité de contenu qu’un site contient. La première bonne pratique pour optimiser votre déploiement dans le cloud consiste à [mettre à niveau pour utiliser les `ece-tools`](../dev-tools/install-package.md) afin de bénéficier des fonctionnalités du package, telles que les commandes permettant de créer une sauvegarde de la base de données et de vérifier la configuration de l’environnement.

Les rubriques suivantes peuvent vous aider à mieux comprendre comment optimiser le processus de déploiement :

- [Processus de déploiement dans le cloud](process.md)
Le processus de déploiement dans le cloud comprend trois phases. Vous pouvez tirer parti des forces et des faiblesses de chaque phase.

- Déploiement sans interruption de service [&#128279;](reduce-downtime.md)
Découvrez ce qui se passe pendant le déploiement et comment réduire les temps d’arrêt de vos expériences de magasin lors d’une mise à jour de l’environnement de production.

- [Déploiement de contenu statique](static-content.md)
Le meilleur moyen d’optimiser votre déploiement dans le cloud consiste à contrôler comment et quand générer du contenu statique.

- [Assistants intelligents](smart-wizards.md)
Le package `ece-tools` fournit les commandes de l’assistant intelligent pour évaluer rapidement la configuration de votre projet.

- [Suivi des déploiements avec New Relic](../monitor/track-deployments.md)
Utilisez le service New Relic pour surveiller les événements de déploiement et analyser l’impact du déploiement sur les performances globales.

