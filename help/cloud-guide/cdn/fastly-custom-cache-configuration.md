---
title: Personnalisation de la configuration du cache
description: Découvrez comment vérifier et personnaliser les paramètres de configuration du cache une fois la configuration du service Fastly terminée.
feature: Cloud, Configuration, Iaas, Cache
exl-id: f6901931-7b3f-40a8-9514-168c6243cc43
source-git-commit: a2f5e2f67c7739302a87eaa27df25a62fca1acb7
workflow-type: tm+mt
source-wordcount: '1995'
ht-degree: 0%

---

# Personnalisation de la configuration du cache

Une fois que vous avez configuré et testé le service Fastly dans vos environnements d’évaluation et de production, passez en revue et personnalisez les paramètres de configuration du cache. Par exemple, vous pouvez mettre à jour les paramètres pour forcer le protocole TLS à rediriger les requêtes HTTP vers Fastly, mettre à jour les paramètres de purge et activer l’authentification de base pour protéger votre site par mot de passe pendant le développement.

Les sections suivantes présentent une vue d’ensemble et des instructions pour configurer certains paramètres du cache.

>[!IMPORTANT]
>
>Les options d’administration disponibles pour configurer le cache Fastly dépendent de la version du module Fastly CDN pour Magento 2 installée. Adobe vous recommande d’[mettre à niveau le module Fastly](fastly-configuration.md#upgrade) le module Fastly dans vos environnements d’évaluation et de production vers la dernière version. Pour obtenir les informations les plus récentes, consultez les [notes de mise à jour du module Fastly CDN pour Magento2](https://github.com/fastly/fastly-magento2/blob/master/Release-Notes.md).

## Forcer TLS

Fastly fournit l’option _Forcer TLS_ pour rediriger les requêtes non chiffrées (HTTP) vers Fastly. Une fois votre environnement d’évaluation ou de production configuré avec un [certificat SSL/TLS valide](fastly-configuration.md#provision-ssltls-certificates), vous pouvez mettre à jour la configuration Fastly de votre magasin afin d’activer l’option Forcer le protocole TLS. Consultez le guide Fastly [Forcer TLS](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md) dans la documentation du _module Fastly CDN pour Magento 2_ .

>[!NOTE]
>
>L’activation de l’option Forcer TLS est une bonne pratique recommandée pour Adobe Commerce sur les magasins d’infrastructure cloud.

## Prolonger la temporisation rapide

La configuration du service Fastly spécifie un délai d’expiration par défaut de 180 secondes pour les requêtes HTTPS à l’administrateur. Tout traitement de requête qui dépasse le délai d’expiration renvoie une erreur 503. Par conséquent, vous pouvez recevoir des erreurs 503 en réponse à des requêtes qui nécessitent un traitement long ou lors d’une tentative d’exécution d’opérations en bloc.

Pour effectuer des actions en masse qui prennent plus de 3 minutes, modifiez la valeur_ _Délai d’expiration du chemin d’administration__ afin d’éviter les erreurs 503.

>[!NOTE]
>
>Si vous avez spécifié un point d’entrée de chemin d’administration personnalisé dans le champ **Chemin d’administration personnalisé** dans **Magasins** > **Configuration** > **Avancé** > **Admin** > **URL de base d’administration**, vous devez également définir la variable [ADMIN_URL](../environment/variables-admin.md#change-the-admin-url) dans cet environnement sur la même valeur. Si les paramètres sont différents, la temporisation ne fonctionne pas.
>
>Pour étendre les paramètres de délai d’expiration de Fastly à d’autres utilisateurs qu’Admin dans l’interface utilisateur de Fastly, voir [Augmentation des délais pour les tâches longues](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md).

**Pour prolonger le délai d’expiration de Fastly pour l’administrateur** :

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** et développez **Cache de page complet**.

1. Dans la section _Configuration Fastly_, développez **Configuration avancée**.

1. Définissez la valeur **Délai d’expiration du chemin d’administration** en secondes. Cette valeur ne peut pas dépasser 10 minutes (600 secondes).

>[!NOTE]
>
>Le paramètre de configuration **_Délai d’expiration du chemin d’administration_** ne contrôle pas les valeurs de délai d’expiration en dehors d’Adobe Commerce, telles que le délai d’expiration de Fastly WAF. Pour ajuster la valeur du délai d’expiration de Fastly WAF, vous devez ouvrir un ticket d’assistance Adobe pour la mettre à jour dans le service Fastly.

1. Cliquez sur **Enregistrer la configuration** en haut de la page.

1. Une fois la page rechargée, sélectionnez **Télécharger VCL vers Fastly** dans la section _Configuration Fastly_.

Fastly récupère le chemin d’accès Admin pour générer le fichier VCL à partir du fichier de configuration `app/etc/env.php`.

## Configurer les options de purge

Fastly fournit plusieurs types d’options de purge sur votre page de gestion du cache de Magento, notamment des options de purge de la catégorie de produits, des ressources de produits et du contenu. Lorsqu’il est activé, Fastly recherche les événements afin de purger automatiquement ces caches. Si vous désactivez une option de purge, vous pouvez purger manuellement les caches Fastly après avoir terminé les mises à jour via la page Gestion du cache .

Les options de purge sont les suivantes :

- **Purger la catégorie**-Purge le contenu d’une catégorie de produits (et non le contenu d’un produit) lorsque vous ajoutez et mettez à jour un seul produit. Vous pouvez conserver cette option désactivée et activer la purge des produits, qui purge les produits et les catégories de produits.
- **Purger le produit**-Purge tout le contenu des produits et des catégories de produits lors de l’enregistrement d’une seule modification sur un produit. L’activation de la purge de produit peut s’avérer utile pour informer immédiatement les clients lorsqu’un prix est modifié, qu’une option de produit est ajoutée et que l’inventaire des produits est en rupture de stock.
- **Purger la page CMS**-Purge le contenu des pages lors de la mise à jour et de l’ajout de pages au CMS Adobe Commerce. Par exemple, vous pouvez effectuer une purge lors de la mise à jour de vos Conditions générales ou de votre Politique de retour. Si vous apportez rarement ces modifications, vous pouvez désactiver la purge automatique.
- **Purge réversible**-Définit le contenu modifié sur périmé et purge selon le délai périmé. En plus des délais obsolètes, le contenu obsolète est diffusé aux clients tandis que Fastly met à jour le contenu en arrière-plan.

![Configurer les options de purge](../../assets/cdn/fastly-purge-options.png)

**Pour configurer les options de purge Fastly** :

1. Dans la section _Configuration Fastly_, développez **Configuration avancée** pour afficher les options de purge.

1. Pour chaque option de purge, sélectionnez **Oui** pour activer la purge automatique ou **Non** pour désactiver la purge automatique.

   Lorsque vous désactivez une option de purge, vous devez purger manuellement le cache de cette catégorie à partir de la page _Gestion du cache_.

1. Cliquez sur **Enregistrer la configuration** en haut de la page.

1. Une fois la page rechargée, sélectionnez **Télécharger VCL vers Fastly** dans la section _Configuration Fastly_.

Pour plus d’informations, voir [les options de configuration Fastly](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options).

## Configuration de la gestion GeoIP

Le module Fastly comprend la gestion GeoIP pour rediriger automatiquement les visiteurs ou fournir une liste de magasins correspondant au code de pays obtenu. Si vous utilisez déjà une extension pour la gestion de GeoIP, vous devrez peut-être vérifier les fonctionnalités avec les options Fastly .

**Pour configurer la gestion des adresses IP géographiques** :

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** et développez **Cache de page complet**.

1. Dans la section _Configuration Fastly_, développez **Configuration avancée**.

1. Faites défiler vers le bas et sélectionnez **Oui** pour **Activer GeoIP**. Des options de configuration supplémentaires s’affichent.

1. Pour une action GeoIP, choisissez si le visiteur est automatiquement redirigé avec **Rediriger** ou fournissez une liste de magasins à sélectionner avec **Dialogue**.

1. Pour **Mappage de pays**, sélectionnez **Ajouter** pour saisir un code de pays à deux lettres à mapper à un magasin Adobe Commerce spécifique à partir d’une liste.

   ![Ajouter des cartes géographiques GeoIP](/help/assets/cdn/fastly-geo-code.png)

1. Cliquez sur **Enregistrer la configuration** en haut de la page.

1. Après le rechargement de la page, sélectionnez **Charger VCL vers Fastly** dans la section _Configuration Fastly_.

>[!NOTE]
>
>L’implémentation actuelle du module Adobe Commerce Fastly GeoIP ne prend pas en charge les redirections entre plusieurs sites web.

Fastly propose également une série de [fonctions VCL liées à la géolocalisation](https://developer.fastly.com/reference/vcl/variables/geolocation/) pour un codage de géolocalisation personnalisé.

## Activer les modules Fastly Edge

Les modules Fastly Edge sont un framework flexible qui permet de définir des composants d’IU et le code VCL associé via un modèle. Ces modules permettent de personnaliser et d’étendre facilement la configuration de service Fastly par le biais de l’interface utilisateur au lieu d’utiliser des fragments de code VCL personnalisés.

Les modules Edge vous permettent d’activer des fonctionnalités spécifiques telles que les en-têtes CORS, les réécritures de plan de site cloud, ainsi que de configurer l’intégration entre votre boutique Adobe Commerce et d’autres CMS ou back-ends.

Pour accéder au menu Modules Edge afin d’afficher, de configurer et de gérer les modules disponibles, activez l’option _Activer les modules Fastly Edge_. Voir [Modules Fastly Edge](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md) dans la documentation du module Fastly CDN.

## Configuration des back-ends et du blindage d’origine

Les paramètres back-end fournissent des réglages précis pour les performances de Fastly avec le blindage et les délais d’expiration d’origine. Un _serveur principal_ est un emplacement spécifique (adresse IP ou domaine) avec un bouclier d’origine et des paramètres de délai d’expiration configurés pour vérifier et fournir du contenu mis en cache.

_Le blindage d’origine_ achemine toutes les demandes de votre boutique vers un point de présence spécifique (POP). Lorsqu’une requête est reçue, le POP recherche le contenu mis en cache et le fournit. S’il n’est pas mis en cache, il continue vers le Shield POP, puis vers le serveur d’origine qui met le contenu en cache. Les boucliers réduisent directement le trafic à l’origine.

Le code Fastly VCL par défaut spécifie les valeurs par défaut du blindage d’origine et des délais d’expiration pour votre Adobe Commerce sur les sites d’infrastructure cloud. Dans certains cas, vous devrez peut-être modifier les valeurs par défaut. Par exemple, si vous obtenez des erreurs de temps jusqu’au premier octet (TTFB), vous devrez peut-être ajuster la valeur _délai d’expiration du premier octet_.

>[!NOTE]
>
>Si votre site nécessite une diffusion fonctionnelle par le biais d’une intégration back-end telle que [Wordpress](fastly-vcl-wordpress.md), personnalisez votre configuration de service Fastly pour ajouter le back-end et gérer les redirections de votre boutique Adobe Commerce vers Wordpress. Pour plus d’informations, consultez [Modules Fastly Edge - Autre intégration CMS/serveur principal](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) dans la documentation du module Fastly .

**Pour passer en revue la configuration des paramètres du serveur principal** :

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** et développez **Cache de page complet**.

1. Développez la section **Configuration Fastly**.

1. Développez **Paramètres du serveur principal** et sélectionnez l’engrenage pour vérifier le serveur principal par défaut. Une boîte de dialogue modale s’ouvre pour afficher les paramètres actuels avec des options permettant de les modifier.

   ![Modifier le serveur principal](../../assets/cdn/fastly-backend.png)

1. Sélectionnez l’emplacement **Shield** (ou centre de données).

   La configuration Fastly par défaut de votre projet définit l’emplacement le plus proche de votre région de service cloud. Si vous devez le modifier, sélectionnez un emplacement proche de l’emplacement par défaut.

1. Modifiez les valeurs de temporisation (en microsecondes) pour la connexion au bouclier, le temps entre les octets et le temps du premier octet. Nous vous recommandons de conserver les paramètres de délai d’expiration par défaut.

1. Si vous le souhaitez, sélectionnez pour **Activer le serveur principal et le bouclier après modification ou enregistrement**.

1. Cliquez sur **Télécharger** pour enregistrer vos modifications et les télécharger sur les serveurs Fastly.

1. Dans Admin, sélectionnez **Enregistrer la configuration**.

Pour plus d’informations, consultez le [Guide des paramètres du serveur principal](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md) dans la documentation du module Fastly .

## Authentification de base

L’authentification de base est une fonctionnalité permettant de protéger chaque page et ressource de votre site avec un nom d’utilisateur et un mot de passe.

Adobe **déconseillé** active l’authentification de base sur votre environnement de production. Vous pouvez le configurer sur Évaluation pour protéger votre site pendant le processus de développement. Consultez le [&#x200B; Guide d’authentification de base &#x200B;](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md) dans la documentation du module Fastly CDN.

Si vous ajoutez un accès utilisateur et activez l’authentification de base lors de l’évaluation, vous pouvez toujours accéder à l’administrateur sans avoir besoin d’informations d’identification supplémentaires.

>[!NOTE]
>
>Ne **pas** vérifiez [!UICONTROL Enable HTTP access control] dans la console Cloud pour tout environnement où Fastly est activé (comme les environnements d’évaluation ou de production non actifs). Si le contrôle d’accès est configuré de cette manière, les utilisateurs qui y avaient précédemment accès peuvent toujours accéder au site si leurs informations d’identification restent mises en cache par Fastly, même après l’annulation de leur accès.

## Créer des fragments de code VCL personnalisés

Fastly prend en charge une version personnalisée du langage de configuration de vernis (VCL) pour personnaliser la configuration de service Fastly. Par exemple, vous pouvez autoriser, bloquer ou rediriger l’accès pour des utilisateurs ou des adresses IP spécifiques à l’aide de blocs de code VCL avec des dictionnaires Edge et de liste de contrôle d’accès (ACL).

Pour obtenir des instructions sur la création de fragments de code VCL personnalisés, de dictionnaires Edge et de listes de contrôle d’accès, voir [Fragments de code VCL Fastly personnalisés](fastly-vcl-custom-snippets.md).

>[!NOTE]
>
>Avant d’ajouter du code VCL personnalisé, des dictionnaires Edge et des listes de contrôle d’accès à votre configuration de module Fastly, vérifiez que le service de mise en cache Fastly fonctionne avec la configuration par défaut. Voir [&#x200B; Configuration rapide &#x200B;](fastly-configuration.md).

## Gestion des domaines

Pour les projets Starter et Pro, vous pouvez utiliser l’option [!UICONTROL Domains] pour ajouter et gérer la configuration de domaine Fastly pour votre boutique.

- Pour les projets de démarrage, accédez à l’URL du projet sous l’onglet [!UICONTROL Domains] dans la [!DNL Cloud Console] pour ajouter votre URL de projet.

- Pour les projets Pro, envoyez un [ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour ajouter le domaine à la configuration de votre projet cloud. L’équipe d’assistance met également à jour la configuration du compte Fastly Adobe Commerce pour ajouter le domaine.

**Pour gérer la configuration de domaine Fastly à partir de l’administrateur** :

{{admin-login-step}}

1. Sélectionnez **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** et développez **Cache de page complet**.

1. Dans la section Admin _Configuration Fastly_, sélectionnez **Domaines**.

1. Cliquez sur **Gérer les domaines** pour ouvrir la page Domaines .

1. Ajoutez les noms de niveau supérieur et de sous-domaine pour les magasins dans l’environnement cloud.

   Vous ne pouvez spécifier que des domaines qui ont déjà été ajoutés à votre configuration de l’infrastructure cloud.

   ![Ajouter une configuration de domaine Fastly pour Starter](../../assets/cdn/fastly-starter-activate-domain.png)

1. Cliquez sur **Activer** pour mettre à jour la configuration du domaine Fastly.

>[!NOTE]
>
>Si le même domaine a été configuré sur un compte Fastly différent, vous devez envoyer un ticket d’assistance Adobe Commerce pour demander la délégation de domaine avant de pouvoir ajouter le domaine à Adobe Commerce. Voir [Comptes Fastly multiples et domaines attribués](fastly.md#multiple-fastly-accounts-and-assigned-domains).

## Activer le mode de maintenance

Utilisez l’option _Mode de maintenance_ pour autoriser l’accès administratif à votre site à partir d’adresses IP spécifiées lors du renvoi d’une page d’erreur pour toutes les autres requêtes.

**Pour activer le mode Maintenance avec un accès administratif** :

1. Ouvrez la section _Configuration Fastly_ dans l’interface d’administration.

1. Dans la section _ACL Edge_, mettez à jour la liste de contrôle d’accès (ACL) `maint_allow` avec les adresses IP d’administration qui peuvent accéder à votre magasin lorsqu’il est en mode de maintenance.

   ![Mettre à jour la liste autorisée du mode de maintenance IP](../../assets/cdn/fastly-maint-allowlist.png)

1. Dans la section _Mode de maintenance_, sélectionnez **Activer le mode de maintenance**.

   Après avoir activé le mode de maintenance, tout le trafic est bloqué, à l’exception des requêtes provenant des adresses IP dans la liste de contrôle d’accès `maint_allowlist`. Vous pouvez mettre à jour le `maint_allowlist` pour modifier les adresses IP dans la liste de contrôle d’accès.

   Pour obtenir des instructions de configuration détaillées, consultez le guide [Mode de maintenance](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md) dans la documentation du module Fastly CDN pour Magento 2 .
