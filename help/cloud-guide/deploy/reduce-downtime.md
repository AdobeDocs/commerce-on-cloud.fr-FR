---
title: Déploiement sans temps d’arrêt
description: Découvrez comment réduire le temps d’arrêt global lors du déploiement d’Adobe Commerce sur des projets d’infrastructure cloud.
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
source-git-commit: b831bc5bce0f76ec8972b3578c500508dd4d7d41
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Déploiement sans temps d’arrêt

Adobe Commerce sur l’infrastructure cloud exécute l’application en mode [_maintenance_ pendant la phase de déploiement](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode) qui met votre site hors ligne jusqu’à ce que le déploiement soit terminé. La durée pendant laquelle votre site de production est en mode de maintenance dépend de la taille du site, du nombre de modifications appliquées pendant le déploiement et de la configuration pour le déploiement de contenu statique. Il est possible de configurer votre projet afin qu’il se déploie avec un effet de temps d’arrêt **nul**.

Pendant le processus de déploiement, toutes les connexions sont mises en file d’attente pendant 5 minutes au maximum, ce qui permet de préserver les sessions actives et les actions en attente, telles que l’ajout au panier ou le passage en caisse. Après le déploiement, la file d’attente est libérée et les connexions continuent sans interruption. Pour tirer parti de cette _suspension de la connexion_ et réduire le temps d’arrêt du déploiement à _zéro_, vous devez configurer votre projet afin d’utiliser la stratégie de déploiement la plus efficace.

>[!NOTE]
>
>Pour vérifier si votre projet cloud est configuré de manière optimale afin de réduire le temps d’arrêt du déploiement, utilisez l’assistant [Smart Wizard](smart-wizards.md). L’assistant dynamique vérifie votre configuration actuelle et vous guide tout au long des ajustements de configuration recommandés afin d’activer les bonnes pratiques pour les déploiements sans interruption de service.

Procédez comme suit pour réduire le temps nécessaire à votre boutique pour déployer une mise à jour en production :

1. [Effectuez la mise à niveau vers le package `ece-tools`](../dev-tools/install-package.md) ou [mettez à jour la `ece-tools` version](../dev-tools/update-package.md)
Votre projet d’infrastructure cloud Adobe Commerce doit disposer du dernier package `ece-tools` afin que vous disposiez des outils nécessaires pour configurer un déploiement optimal. Si vous disposez des dernières `ece-tools`, passez à l’étape suivante.

   >[!NOTE]
   >
   >Bien qu’il soit recommandé d’utiliser le dernier package `ece-tools`, la méthode de déploiement sans interruption fonctionne avec `ece-tools` [version 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) et les versions ultérieures.

1. [Configurer le déploiement de contenu statique](static-content.md)
Si le déploiement de contenu statique échoue lors de la phase de déploiement, votre site est bloqué en mode de maintenance. En cas d’échec lors de la phase de création, le processus évite les temps d’arrêt, car il ne commence jamais la phase de déploiement. La [génération de contenu statique pendant la phase de création avec HTML miniaturisé](static-content.md#setting-the-scd-on-build), également appelée statut idéal, est la configuration optimale pour les déploiements sans interruption de service et _empêche_ l’interruption de service en cas de panne.

1. [Configuration du hook de post-déploiement](../application/hooks-property.md)
Vous devez configurer le hook de post-déploiement pour nettoyer et préchauffer le cache. Par défaut, le nettoyage du cache se produit pendant la phase de déploiement lorsque le site est hors service. Le passage du nettoyage du cache à la phase post-déploiement signifie que votre cache reste actif jusqu’à ce que la phase de déploiement soit terminée. Vous pouvez ensuite nettoyer le cache en toute sécurité.

   Personnalisez la liste des pages utilisées pour précharger le cache avec la variable d’environnement [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages).

1. [Réduire les fichiers de thème](../environment/variables-deploy.md#scdmatrix)
Vous pouvez réduire le nombre de fichiers de thème inutiles en configurant la variable d’environnement SCD\_MATRIX.

1. [Accélérer le déploiement de contenu statique](../environment/variables-deploy.md#scdthreads)
Vous pouvez accélérer le processus de déploiement en mettant à jour la variable d’environnement SCD\_THREADS afin d’augmenter le nombre de threads pour le déploiement du contenu statique.

>[!NOTE]
>
>Vous pouvez valider la configuration de votre projet pour un déploiement optimal en [exécutant l’assistant d’état idéal](smart-wizards.md#verifying-an-ideal-configuration).
