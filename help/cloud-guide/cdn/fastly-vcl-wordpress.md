---
title: Réacheminer les requêtes vers un serveur principal CMS
description: Découvrez comment réacheminer les requêtes entrantes d’un magasin Adobe Commerce vers un site WordPress distinct à l’aide du module Fastly Edge.
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%

---

# Réacheminer les requêtes vers un serveur principal CMS

Réacheminez les requêtes entrantes d’un magasin Adobe Commerce vers un site WordPress distinct à l’aide du module Fastly Edge _autre intégration CMS/serveur principal_ avec un dictionnaire Edge. Vous pouvez suivre un processus similaire pour réacheminer les requêtes vers d’autres serveurs principaux CMS.

Utilisez les modules Fastly Edge pour créer et charger le code VCL personnalisé à partir de l’administrateur au lieu d’écrire le code VCL manuellement et de le charger à l’aide de l’API Fastly.

>[!NOTE]
>
>Nous vous recommandons d’ajouter des configurations VCL personnalisées à un environnement d’évaluation où vous pouvez les tester avant de mettre à jour la configuration de service Fastly dans l’environnement de production.

**Conditions préalables**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Pour réacheminer les requêtes d’Adobe Commerce vers WordPress** :

1. Activez les modules Fastly Edge dans l’environnement d’évaluation ou de production.

   - Connectez-vous à l’administrateur.

   - Accédez à **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** > **Cache de page complet** > **Fastly Configuration** > **Advanced Configuration**.

   - Définissez la valeur de **Modules Fastly Edge** sur **Oui**.

   - Enregistrez la configuration.

1. Identifiez les chemins d’URL à réacheminer vers le serveur principal WordPress.

1. Effectuez les tâches suivantes pour configurer le service Fastly et créer le code VCL personnalisé pour réacheminer les requêtes vers le serveur principal WordPress.

   - Créez un dictionnaire Edge qui spécifie les chemins d’accès à réacheminer du magasin Adobe Commerce vers le serveur principal.

   - Ajoutez le serveur principal WordPress à la configuration du service Fastly et joignez la condition de requête pour les réécritures d’URL.

   - Configurez le module _Autre intégration CMS/serveur principal_ Edge pour gérer les réécritures d’URL d’Adobe Commerce vers le serveur principal WordPress.

     Pour obtenir des instructions détaillées, consultez [Modules Fastly Edge - Autre intégration CMS/serveur principal](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) dans la documentation _Module Fastly CDN pour Magento 2_.

1. Après la mise à jour de la configuration du service Fastly , testez votre boutique Adobe Commerce pour vous assurer que les requêtes d’URL spécifiées pour WordPress sont correctement redirigées.
