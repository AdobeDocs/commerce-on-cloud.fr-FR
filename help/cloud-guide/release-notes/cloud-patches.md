---
title: Correctifs cloud pour Commerce
description: Consultez la liste des dernières améliorations apportées au package Cloud Patches.
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: 07f42fc886d394b602533927b153a3f9370d192c
workflow-type: tm+mt
source-wordcount: '2536'
ht-degree: 0%

---

# Correctifs cloud pour Commerce

Le package [Correctifs cloud](https://github.com/magento/magento-cloud-patches) fournit un ensemble de correctifs requis qui améliorent l’intégration de toutes les versions d’Adobe Commerce avec les environnements cloud et prend en charge la diffusion rapide de correctifs critiques.

Le package Cloud Patches for Commerce dépend du package ECE-Tools et est installé et mis à jour lorsque vous installez ou mettez à jour le package ECE-Tools. Vous pouvez également utiliser et gérer les correctifs cloud pour Commerce sous la forme d’un package autonome permettant d’appliquer des correctifs à un projet Adobe Commerce qui ne se trouve pas sur la plateforme cloud. Ces notes de mise à jour décrivent les dernières améliorations apportées à ce package.

>[!TIP]
>
>Pour vous assurer que votre projet dispose de tous les correctifs requis, mettez à jour vers la [dernière version des outils](../dev-tools/update-package.md).

>[!NOTE]
>
>Voir [Application de correctifs](../development/apply-patches.md) pour obtenir des instructions sur l’application de correctifs à vos projets.

Le package de `magento/magento-cloud-patches` utilise la séquence de version suivante : `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.12 {#latest}

Date de publication : 13 novembre 2025

- ![Icône de correction](../../assets/fix.svg) **Package Symfony**-Ajout de la prise en charge des derniers packages Symfony YAML.<!-- MCLOUD-14020 -->
- ![Icône de correction](../../assets/fix.svg) **Correctif**-Correctif pour passage en caisse rompu (AC-15867) et newrelic (ACSD-68617).<!-- MCLOUD-14191 -->
- ![icône de correction](../../assets/fix.svg) **vue améliorée des catégories** - MCLOUD-13752 : amélioration de la vue des catégories<!-- MCLOUD-13752 | MCLOUD-14139  - -->

## v1.1.11

Date de publication : 9 septembre 2025

- ![icône de correctif](../../assets/fix.svg) **WebAPI**-Correctif pour CVE-2025-54236.<!-- MCLOUD-14016 -->

## v1.1.10

Date de publication : 7 août 2025

- ![nouvelle icône](../../assets/new.svg) **PHP 8.4**—Ajout de tests fonctionnels.<!-- MCLOUD-13312 -->

## v1.1.9

Date de publication : 9 juin 2025

- ![icône de correction](../../assets/fix.svg) **vue de catégorie améliorée**-vue de catégorie améliorée CVE-2025-47109<!-- MCLOUD-13752	 - -->
- ![Icône de correction](../../assets/fix.svg) **Cache d’administration amélioré**-Améliore-l’efficacité-du-cache-d’administration CVE-2025-47110<!-- MCLOUD-13753	 - -->

## v1.1.8

Date de publication : 3 juin 2025

- ![icône de correction](../../assets/fix.svg) **compatibilité améliorée avec la version 2.4.8**-bibliothèques tierces mises à jour pour une meilleure compatibilité avec la version 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.7

Date de publication : 5 mai 2025

- ![nouvelle icône](../../assets/new.svg) **Correctif mis à jour pour Commerce 2.4.4 à 2.4.8**—Il s’agit d’un correctif mis à jour pour [CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch), publié dans la version 1.1.7<!-- MCLOUD-13619 -->

## v1.1.6

Date de publication : 24 avril 2025

- ![nouvelle icône](../../assets/new.svg) **Correctif mis à jour pour Commerce 2.4.4 à 2.4.7**—Il s’agit d’un correctif mis à jour pour [CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08), publié dans la version 1.1.4<!-- MCLOUD-13240 -->

## v1.1.5

Date de publication : 15 avril 2025

- ![nouvelle icône](../../assets/new.svg) **Correctif ajouté pour B2B 1.5.2**—Correctif pour le problème ACP2E-3833 avec le module B2B 1.5.2 et MariaDB 10.6<!-- MCLOUD-13605	-->

## v1.1.4

Date de publication : 13 février 2025

- ![nouvelle icône](../../assets/new.svg) **Correctif ajouté pour Commerce 2.4.4 à 2.4.7**—Cette mise à jour corrige les correctifs [CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08).<!-- MCLOUD-13240	 - -->

## v1.1.3

Date de publication : 6 février 2025

- ![nouvelle icône](../../assets/new.svg) **PHP 8.4**—Ajout de la prise en charge de PHP 8.4.<!-- MCLOUD-13149	 - -->

## v1.1.2

Date de publication : 5 novembre 2024

- ![icône de correction](../../assets/fix.svg) **correctif ajouté pour Commerce 2.4.4 à 2.4.7**—Cette mise à jour corrige une vulnérabilité [CVE-2024-45115](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73) critique pour Adobe Commerce lors de l’utilisation du module B2B.<!-- MCLOUD-12980 - -->

## v1.1.1

Date de publication : 5 novembre 2024

- ![icône de correction](../../assets/fix.svg) **correctif ajouté pour Commerce 2.4.4 à 2.4.7**—Cette mise à jour corrige une vulnérabilité critique [CVE-2024-34102](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting.<!-- MCLOUD-12980 - -->

## v1.1.0

Date de publication : 7 octobre 2024

- ![icône de correction](../../assets/fix.svg) **code refactorisé**—Suppression de la prise en charge des anciennes versions de PHP (7.4, 7.3, 7.2) et des bibliothèques associées.<!-- MCLOUD-9278 - -->
- ![icône de correction](../../assets/fix.svg) **version de monolog mise à niveau**—Ajout de la prise en charge de monolog 3.6.<!-- MCLOUD-12855 - -->
- ![icône de correction](../../assets/fix.svg) **correctif pour le serveur d’applications** : résout un problème connu avec le serveur d’applications GraphQL. Plus précisément, la `CatalogGraphQl\\Model\\Config\\AttributeReader` de la version 2.4.7 contenait un bug qui pouvait entraîner la récupération de réponses par les requêtes GraphQL en fonction d’une configuration d’attributs obsolète.<!-- ACPT-1876 -->

## v1.0.27

Date de publication : 21 mai 2024

- **Prise en charge de PHP 8.3**—Ce patch résout les erreurs de compatibilité entre php 8.3 et la version du package du compositeur.

## v1.0.26

Date de publication : 8 avril 2024

- ![nouvelle icône](../../assets/new.svg) **PHP** — Ajout de la prise en charge de PHP 8.3.

## v1.0.25

Date de publication : 16 janvier 2024

- **Améliorations du cache**-ce correctif améliore l’efficacité du cache de disposition et réduit considérablement l’utilisation de la mémoire, pour les versions 2.4.4 et ultérieures d’Adobe Commerce.<!-- MCLOUD-11514 -->
- **Améliorations des tâches CRON**-Ce correctif corrige le problème où les tâches manquées attendent inutilement que les verrous de tâche CRON se produisent pour les versions 2.4.4 et ultérieures d’Adobe Commerce.<!-- MCLOUD-11329 -->

## v1.0.24

Date de publication : 15 septembre 2023

- **Amélioration des performances**-ce correctif corrige un problème affectant les performances en réduisant le nombre de fois où les mêmes configurations de déploiement se chargent pour Adobe Commerce 2.4.6 à 2.4.6-p1<!-- MCLOUD-10604 -->

## v1.0.23

Date de publication : 31 juillet 2023

- **Suppression du correctif MCLOUD-10604**-Ce correctif a été déplacé vers QPT.<!-- MCLOUD-10736 -->

## v1.0.22

Date de publication : 19 juin 2023

- **Assistant/sortie de l’interface de ligne de commande QPT amélioré**—Ajout d’un avertissement à l’assistant/sortie de l’interface de ligne de commande QPT qui vous rappelle de vérifier les détails et les exigences du correctif s’il existe des dépendances.<!-- ACP2E-1963 -->
- **Correctifs ajoutés pour Commerce 2.4.6:**
   - Correction de la validation du `regexp cache tag`.<!-- MCLOUD-10226 -->
   - Amélioration des performances en réduisant le nombre de fois où les mêmes configurations de déploiement se chargent.<!-- MCLOUD-10604 -->
- **Correctifs ajoutés pour Commerce 2.3.7 à 2.4.6**—Correction d’un problème qui entraînait un incrément par une valeur aléatoire au lieu d’un incrément par 1 pour les tables `catalog_product_entity_*`.<!-- MCLOUD-10032 -->
- **Correctifs ajoutés pour Commerce 2.4.0 à 2.4.6**—Correction d’une erreur indiquant que `The file can't be deleted. Warning!unlink: No such file or directory`, qui se produisait lors du vidage du cache JS/CSS d’Admin.<!-- MCLOUD-10279 -->

## v1.0.21

Date de publication : 10 mars 2023

- **Amélioration de la prise en charge de PHP 8.2** - Correction de problèmes de compatibilité avec certaines versions de PHP 8.2.x pour la prise en charge de Commerce 2.4.6.

## v1.0.20

Date de publication : 27 octobre 2022

- **Correctif d’améliorations du cache L2 ajouté** : ce correctif corrige un problème de vidage du cache L2 local pour les versions 2.4.0 et 2.4.1.<!-- MCLOUD-7845 --> de Commerce

## v1.0.19

Date de publication : 13 septembre 2022

- **Prise en charge améliorée de PHP 8.1** - Correction de problèmes de compatibilité avec certaines versions de PHP 8.1.x.

## v1.0.18

Date de publication : 11 août 2022

Correctif critique pour Adobe Commerce 2.4.5 :

- **Problème lié aux commandes à l’aide des paiements Braintree**—Ce correctif résout un problème critique empêchant les administrateurs de passer de nouvelles commandes ou de passer de nouvelles commandes.<!-- MCLOUD-9137 -->

Voir [L’administrateur ne peut pas créer de commande/réorganiser lorsque le paiement Braintree est activé](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## v1.0.17

Date de publication : 24 mai 2022

Correction de contraintes liées aux correctifs de sécurité dans le fichier `patches.json`.

## v1.0.16

Date de publication : 31 mars 2022

Correctif critique pour Adobe Commerce 2.3.3-p1 et les versions ultérieures :

Mise à jour des correctifs pour résoudre une vulnérabilité **critique** entraînant l’exécution de code à distance non authentifié.<!-- MCLOUD-8479 -->

Voir [Bulletin de sécurité Adobe APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.15

Date de publication : 10 mars 2022

- **Support PHP 8.1**—Ajout de la prise en charge de PHP 8.1 et abandon de la prise en charge de PHP 7.0 et 7.1.
- **Correctif ajouté pour Adobe Commerce 2.3.3** : affichage de la devise fixe sur la page du produit.

## v1.0.14

Date de publication : 13 février 2022

Correctif critique pour Adobe Commerce 2.3.3-p1 et les versions ultérieures :

Ajout d’un correctif pour résoudre une vulnérabilité **critique** qui entraîne l’exécution de code distant non authentifié.<!-- MCLOUD-8461 -->

Voir [Bulletin de sécurité Adobe APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.13

Date de publication : 25 octobre 2021

- **Mettre à jour le monologue** : mise à jour de la version minimale requise pour que le package `monolog` soit `^2.3`.<!-- ACMP-1263 -->
- **Méthode PHP incompatible** : correction d&#39;une méthode PHP incompatible avec les versions 2.4.3 et 2.3.7-p1.<!-- AC-384 --> d&#39;Adobe Commerce
- **Erreur PHP**—Correction d&#39;une erreur `PHP error 'Undefined variable: errorMessage' ...` qui se produisait lors de l&#39;application d&#39;un patch.<!-- ACP2E-138 -->

## v1.0.12

Date de publication : 12 août 2021

Correctif critique pour Adobe Commerce 2.4.3 et 2.3.7-p1 :

- **Problème de limitation de débit de l’API** : ce correctif corrige une limite de débit par défaut qui empêchait les API Web de traiter les requêtes comportant plus de 20 éléments dans un tableau. Ce correctif augmente la valeur par défaut de la limite de débit. Voir les notes de mise à jour d’Adobe Commerce [2.4.3](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

Date de publication : 29 juillet 2021

- **Correction d’un problème lié à l’application du correctif de navigation en couche B2B** : pour les clients qui ont appliqué le correctif de navigation en couche B2B, ce correctif résout une erreur `Undefined offset` qui s’affiche sur la page Rechercher après avoir basculé sur la vue Boutique<!--MCLOUD-5287-->.

- **Paypal Checkout patch** : corrige un problème d’Adobe Commerce 2.3.7 avec PayPal Express en raison duquel le prix de la commande précédemment passée s’affiche.<!--MC-42674-->

- **Prise en charge des catégories de correctifs**—Ajout de la prise en charge du traitement des catégories de correctifs et des sources d&#39;origine affectées aux correctifs de qualité. Les catégories permettent aux clients d&#39;utiliser des filtres et un tri pour trouver des correctifs plus rapidement lors de l&#39;utilisation de l&#39;outil [Quality Patches Tool](https://github.com/magento/quality-patches) et de l&#39;outil d&#39;analyse à l&#39;échelle du site (SWAT). <!--MC-38577-->

## v1.0.10

Date de publication : 10 mai 2021

- **Compatibilité avec Adobe Commerce 2.3.7** : résolution du conflit des dépendances du compositeur pour l’installation sur Adobe Commerce 2.3.7.<!--MC-42131-->
- **Correction d’un problème lié à l’application répétée d’un correctif groupé** : l’application répétée d’un correctif groupé (qui inclut d’autres correctifs obsolètes) pouvait annuler les packages obsolètes inclus. Tous les correctifs ne sont désormais appliqués qu’une seule fois. Toute tentative d’application du même package affiche à nouveau un message indiquant que le correctif a déjà été appliqué.<!--MC-41912-->
- **Correctif de navigation par couches B2B** : correction d’un autre problème qui empêchait la navigation par couches d’afficher toutes les options de produit lorsque l’utilisateur activait le catalogue partagé B2B<!--MCLOUD-7742-->.

## v1.0.9

Date de publication : 1er février 2021

- **Correctif de navigation par couches B2B** : correction d’un problème qui empêchait la navigation par couches d’afficher toutes les options de produit lorsque le catalogue partagé B2B était activé.<!--MCLOUD-6923-->
- **Compatibilité avec PHP 7.4**—Correction d&#39;un problème de compatibilité de correctifs cloud avec PHP 7.4.<!--MCLOUD-7367-->
- **Les correctifs obsolètes deviennent visibles** : correction d’un problème de correctifs cloud en raison duquel les correctifs obsolètes sont visibles dans la table des correctifs après l’application d’un correctif de remplacement contenant l’intégralité du contenu du correctif obsolète. Cela peut se produire si vous appliquez un correctif qui combine plusieurs autres correctifs.<!--MC-40626-->
- **Échecs silencieux lors de l’application de correctifs**—Correction d’un problème de correctifs cloud en raison duquel la commande `git apply` échouait silencieusement à appliquer des correctifs dans certains environnements.<!--MC-40529-->

## v1.0.8

Date de publication : 14 octobre 2020

- **Mises à jour de compatibilité pour les correctifs magento/magento-cloud** : mise à jour des contraintes de version `symfony` et `semver` dans le fichier `composer.json` pour assurer la compatibilité avec Adobe Commerce 2.4.1 et les versions ultérieures.<!--MCLOUD-7111-->

## v1.0.7

Date de publication : 14 octobre 2020

- **Correctifs Redis pour Adobe Commerce 2.3.0 à 2.3.5, 2.4.0**—Mise à jour des correctifs Redis pour prendre en charge l’ajout de produits à une catégorie lors de l’implémentation d’un cache de niveau 2. <!--MCLOUD-6659-->

- **Correctif Braintree VBE**—Correctif d&#39;un problème qui générait une erreur lorsqu&#39;un administrateur tentait d&#39;afficher un rapport de règlement Braintree. <!--MCLOUD-6684-->

- Désormais, la commande `ece-patches apply` utilise la commande Unix `patch` pour appliquer des correctifs si Git n’est pas disponible sur le système hôte. <!--MCLOUD-7069-->

## v1.0.6

Date de publication :

- **Correctifs Redis pour Adobe Commerce 2.3.0 - 2.3.4** : optimisez la communication et améliorez les performances.
   - Réduction de la taille des transferts réseau entre Redis et Adobe Commerce
   - Correction des conditions de concurrence sur les opérations de rechargement et d’écriture
   - Adaptateur de cache de base de réécriture pour gérer les erreurs lors de l&#39;enregistrement
   - Diminuez la consommation de Redis CPU<!--MCLOUD-6139-->

- **Correctifs Redis pour Adobe Commerce 2.3.0 - 2.3.5**—Amélioration des performances et correction des erreurs
   - Correction de l’implémentation du verrou de cache pour éviter des verrous infinis
   - Améliorer le mécanisme de verrouillage actuel
   - Implémenter des verrous signés pour empêcher le déverrouillage des requêtes parallèles
   - Corrigez l&#39;erreur suivante qui se produit lors de l&#39;opération d&#39;écriture Redis : `OOM command not allowed when used memory > maxmemory`
   - Correction du traitement du cache propre par `cat_p` balise qui s’exécute lors des mises à jour du produit<!--MCLOUD-6110-->

- Correction d’un problème qui provoquait une erreur lors de l’application du correctif `amzn/amazon-pay-module` requis à Adobe Commerce sur les projets d’infrastructure cloud avec Adobe Commerce v2.2.6 ou 2.3.5, qui n’incluent pas ce module. Désormais, le processus d’application de correctifs ignore le correctif `amzn/amazon-pay-module` si le module n’est pas installé.<!--MCLOUD-6588-->

## v1.0.5

Date de publication : 26 juin 2020

- **Améliorations des performances de Redis** : ajoute des fonctionnalités d’optimisation Redis aux versions 2.3.3 et 2.3.4 d’Adobe Commerce. Ces correctifs étaient inclus dans la version 2.3.5 d’Adobe Commerce.<!--MCLOUD-5771-->

- **New Relic log enricher** : ajoute l&#39;interface de processeur Monolog requise pour prendre en charge les améliorations des fonctionnalités de journalisation New Relic introduites dans les composants cloud de Commerce version 1.0.4. Ce correctif est nécessaire pour déployer Adobe Commerce 2.1.x. Si le correctif n’est pas appliqué, la création échoue pendant le processus de `di:compile`.<!--MCLOUD-6029-->

## v1.0.4

Date de publication : 12 mai 2020

- **Passage en caisse d’Amazon Pay** : correction d’un problème lié au widget de paiement Amazon Pay qui empêchait les clients de modifier le mode de paiement à l’étape _Révision et paiements_ pendant le processus de passage en caisse.<!--MCLOUD-5930-->

- **Affichage des produits sur la page Catégorie**—Correction d&#39;un problème qui empêchait l&#39;affichage des produits sur la page Catégorie dans la vue _Afficher toutes les pages_.<!--MCLOUD-5684-->

- **Chargement d’images Page Builder** : correction d’un problème d’interface de Page Builder qui provoquait parfois l’erreur suivante lors du chargement d’images dans la galerie d’images : `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Supprimer les avertissements de génération de plan de site inutiles**—Ajoute une nouvelle tentative lorsque des erreurs se produisent lors de la génération du plan de site et ignore la notification par e-mail du client dans les cas où les erreurs peuvent être récupérées automatiquement.<!--MCLOUD-3025-->

- **Amélioration des performances du site**—Correction d&#39;un problème de performances de la fonction `Magento\Framework\App\DeploymentConfig\Reader::load`, qui subissait périodiquement de longs temps de chargement affectant les performances du site. <!--MCLOUD-5650-->

- Mise à jour de l&#39;affectation de correctifs pour les correctifs de méthode de paiement afin de cibler les modules de paiement au lieu du package de base Magento (magento/magento2-base) de sorte que les correctifs de paiement soient appliqués uniquement si les modules de paiement existent.<!--MCLOUD-5666-->

- Correctifs mis à jour pour des raisons de compatibilité avec Magento Open Source.<!--MCLOUD-5701-->

## v1.0.3

Date de publication : 28 avril 2020

- Ajout d’un correctif pour le correctif « FPC est désactivé lors des déploiements » afin de prendre en charge Adobe Commerce 2.3.5.

## v1.0.2

Date de publication : 27 février 2020

Cette version comprend les correctifs et correctifs critiques suivants :

- **Mises à jour de compatibilité pour les correctifs magento/magento-cloud**

   - Mise à jour des contraintes de version `symfony` et `semver` dans le fichier `composer.json` pour des raisons de compatibilité avec Adobe Commerce 2.4 et versions ultérieures.<!--MAGECLOUD-5127-->

   - Mise à jour des contraintes dans `composer.json` pour la compatibilité avec les versions `ece-tools` 2002.0.22 et ultérieures 2002.0.x.

- **PayPal Express Checkout** - Publié le 12 février 2020, ce correctif résout un problème qui affecte les commandes passées avec PayPal Express Checkout lorsque l’adresse d’expédition de la commande indique une région de pays qui a été saisie manuellement dans le champ de texte plutôt que sélectionnée dans le menu déroulant de la page Expédition. Voir la description complète du correctif sur la page de téléchargement des correctifs.

- **Correctif du déploiement de l’application** : ajout d’un correctif pour résoudre un problème qui désactivait le cache de pages complet pendant le processus de déploiement. Ce correctif s’applique à Adobe Commerce version 2.3.2 et ultérieures.

- **Paramètre d’étendue pour l’API asynchrone/en bloc** : mise à jour de ce correctif pour corriger une erreur de syntaxe dans le fichier `composer.json`. Ce correctif s’applique à Magento Open Source 2.3.1 et 2.3.2. Voir la description complète du correctif sur la page de téléchargement des correctifs.

## v1.0.1

Date de publication : 6 février 2020

Nous avons inclus tous les correctifs Magento Open Source 2.x de la page téléchargements de logiciels dans la version 1.0.1 de magento/magento-cloud-patches. Si vous avez déjà copié des correctifs dans votre projet, supprimez-les pour éviter les conflits.

Cette version comprend les correctifs et correctifs critiques suivants :

- **Réparer les blocages cron et améliorer le verrouillage cron**—

   - Correction d’un problème en raison duquel certaines tâches cron ne s’exécutaient pas en raison d’une valeur de statut incorrecte dans le tableau `cron_schedule`. Désormais, nous utilisons le framework de verrouillage d’Adobe Commerce pour vérifier et mettre à jour l’état de la tâche cron au lieu d’utiliser la table `cron_schedule`. Les tâches cron qui se sont terminées avec un statut d’erreur sont retentées lors de la prochaine exécution cron au lieu d’attendre 24 heures.

   - Ajoute une opération _retry_ pour éviter tout blocage lors des mises à jour des données dans la table `cron_schedule`.

- **Mise à jour du `magento/magento-cloud-patches` pour inclure tous les correctifs disponibles pour Magento Open Source 2.x**—Mise à jour du package magento/magento-cloud-patches pour inclure tous les correctifs Magento Open Source 2.x disponibles sur la page des téléchargements de logiciels. Si vous avez déjà copié des correctifs Magento Open Source dans votre projet d’infrastructure cloud Adobe Commerce, supprimez-les pour éviter les conflits.<!--MAGECLOUD-4606-->

- **Correctif de pagination de catalogue Elasticsearch** —A remplacé le correctif de pagination de catalogue Elasticsearch fourni dans magento/magento-cloud-patches v1.0 par un correctif plus efficace.<!--MAGECLOUD-4847-->

- **Correctifs Page Builder** : dans Cloud Patches pour Commerce 1.0.0, nous avons regroupé les correctifs Page Builder pour répondre à une vulnérabilité connue de type exécution de code à distance (RCE) Page Builder, avec le correctif initial basé sur Adobe Commerce 2.3.3. Nous avons mis à jour ces correctifs avec une implémentation plus stable basée sur Adobe Commerce 2.3.4., qui inclut plusieurs optimisations pour résoudre le problème.<!--MAGECLOUD-4884-->

  Si vous disposez du package magento/magento-cloud-patches 1.0.0, vous êtes toujours protégé contre les problèmes de vulnérabilité RCE de Page Builder. Si vous effectuez une mise à jour vers la version 1.0.1 ou ultérieure, vous bénéficiez d’une meilleure implémentation du même correctif.

## v1.0.0

Date de publication : 14 novembre 2019

Il s’agit de la première version du package [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), qui est une nouvelle dépendance pour le package `ece-tools` version 2002.0.22 ou ultérieure.

Cette version comprend les correctifs et correctifs critiques suivants :

- **Correctifs de sécurité Page Builder pour les versions 2.3.1.x et 2.3.2.x**—Correction d’un problème dans l’aperçu Page Builder qui permet aux utilisateurs non authentifiés d’accéder à certaines méthodes de création de modèles qui peuvent être utilisées pour déclencher l’exécution de code arbitraire sur le réseau (RCE), entraînant des fuites d’informations globales. Ce problème peut se produire lors de l’utilisation de versions non prises en charge de Page Builder avec les versions 2.3.1 et 2.3.2.<!--MAGECLOUD-4649--> d’Adobe Commerce

- **Correctifs MSI** : correction des problèmes qui entraînaient des erreurs d’indexation et des problèmes de performances lors de l’utilisation des paramètres d’inventaire par défaut pour la gestion des stocks<!--MAGECLOUD-4428-->.

- **Compatibilité descendante des nouvelles interfaces de messagerie**-corrige un problème d’incompatibilité descendante causé par l’interface PHP `Magento\Framework\Mail\EmailMessageInterface` introduite dans Adobe Commerce v2.3.3. Dans la portée de ce correctif, le nouveau `EmailMessageInterface` hérite de l’ancien `MessageInterface` et les modules principaux d’Adobe Commerce deviennent dépendants de `MessageInterface`.<!--MAGECLOUD-4422-->

- **La pagination du catalogue ne fonctionne pas sur Elasticsearch 6.x**—Correction d’un problème critique lié à la pagination des résultats de recherche qui affecte les clients utilisant Elasticsearch 6.x comme moteur de recherche de catalogue.<!--MAGECLOUD-4448-->
