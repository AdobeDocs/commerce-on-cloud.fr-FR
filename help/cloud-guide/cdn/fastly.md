---
title: Présentation des services Fastly
description: Découvrez comment les services Fastly inclus dans Adobe Commerce sur les infrastructures cloud vous aident à optimiser et sécuriser les opérations de diffusion de contenu pour vos sites Adobe Commerce.
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 0%

---

# Présentation des services Fastly

>[!WARNING]
>
>Pour assurer la conformité PCI des sites Adobe Commerce déployés sur la plateforme cloud, configurez Fastly sur votre branche principale Starter, vos environnements de production Pro et d’évaluation Pro. Si vous utilisez Adobe Commerce dans un déploiement découplé, nous vous recommandons vivement d’utiliser Fastly pour mettre en cache les réponses GraphQL. Voir [Mise en cache avec Fastly](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly) dans le Guide du développeur de GraphQL **.

Fastly fournit les services suivants pour optimiser et sécuriser les opérations de diffusion de contenu pour Adobe Commerce sur les projets d’infrastructure cloud. Ces services sont inclus avec Adobe Commerce sur l’infrastructure cloud sans coût supplémentaire.

- **réseau de diffusion de contenu (CDN)** service basé sur un vernis qui met en cache vos pages de site, ressources, feuilles CSS, etc. dans les centres de données principaux que vous avez configurés. Lorsque les clients accèdent à votre site et à vos magasins, les requêtes sont dirigées vers Fastly pour charger les pages mises en cache plus rapidement. Le service CDN fournit les fonctionnalités suivantes :

- **Gestion du cache** : mettez en cache les pages, les ressources, le code CSS et autres éléments de votre site dans les centres de données back-end que vous avez configurés afin de réduire la charge et les coûts de bande passante

   - Utilisez [des fragments de code VCL personnalisés Fastly](fastly-vcl-custom-snippets.md) (compatible avec Varnish 2.1) pour modifier la manière dont la mise en cache répond aux requêtes

   - Configuration [prise en charge du service GeoIP](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [Forcer les requêtes non chiffrées à accéder au protocole TLS](fastly-custom-cache-configuration.md#force-tls)

   - [Personnaliser la temporisation rapide](fastly-custom-cache-configuration.md#extend-fastly-timeout) paramètres pour éviter les réponses 503 aux demandes d’opération en bloc

   - Créer [pages de réponse d’erreur personnalisées](fastly-custom-response.md)

- **Sécurité** : après avoir activé les services Fastly pour les sites Adobe Commerce, des fonctions de sécurité supplémentaires sont disponibles pour protéger vos sites et votre réseau :

   - [Pare-feu d’application web](fastly-waf-service.md) (WAF) : service de pare-feu d’application web géré qui fournit une protection compatible PCI pour bloquer le trafic malveillant avant qu’il ne puisse endommager votre Adobe Commerce de production sur les sites et le réseau d’infrastructure cloud. Le service WAF est disponible uniquement dans les environnements de production Pro et Starter.

   - [Protection DDoS (Distributed Denial of Service)](#ddos-protection) : protection DDoS intégrée contre les attaques courantes telles que les attaques Ping of Death, les attaques Smurf et d&#39;autres attaques par inondation ICMP.

   - [Certificats SSL/TLS](fastly-configuration.md#provision-ssltls-certificates) : le service Fastly nécessite un certificat SSL/TLS pour traiter le trafic sécurisé via HTTPS.

     Adobe Commerce fournit un certificat Let’s Encrypt SSL/TLS validé par le domaine pour chaque environnement d’évaluation et de production. Adobe Commerce effectue la validation du domaine et la mise en service des certificats pendant le processus de configuration rapide.

- **Cloaking d&#39;origine**—Empêche le trafic de contourner Fastly WAF et masque les adresses IP de vos serveurs d&#39;origine pour les protéger des attaques DDoS et d&#39;accès direct.

  Le cloaking de l’origine est activé par défaut sur les projets de pro production d’infrastructure cloud d’Adobe Commerce. Pour activer le cloaking de l’origine sur les projets de démarrage d’exploitation d’Adobe Commerce sur l’infrastructure cloud, envoyez un [ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Si vous avez du trafic qui ne nécessite pas de mise en cache, vous pouvez personnaliser la configuration du service Fastly pour permettre aux requêtes de [contourner le cache Fastly](fastly-vcl-bypass-to-origin.md).

- **[Optimisation des images](fastly-image-optimization.md)** : décharge le traitement et le redimensionnement des images sur le service Fastly afin que les serveurs puissent traiter les commandes et les conversions plus efficacement.

- **[Journaux Fastly CDN et WAF](../monitor/new-relic-service.md#new-relic-log-management)** : pour les projets Pro d’Adobe Commerce sur les infrastructures cloud, vous pouvez utiliser le service Journaux New Relic pour examiner et analyser les données des journaux Fastly CDN et WAF.

## Module Fast CDN pour Magento 2

Les services Fastly pour Adobe Commerce sur les infrastructures cloud utilisent le module [Fastly CDN pour Magento 2] installé dans les environnements suivants : évaluation et production Pro, démarrage de la production (branche `master`).

Lors de l’approvisionnement initial ou de la mise à niveau de votre projet Adobe Commerce, Adobe installe la dernière version du module Fast CDN dans vos environnements d’évaluation et de production. Lorsque Fastly publie des mises à jour de module, vous recevez des notifications dans l’Admin pour vos environnements. Adobe vous recommande de mettre à jour vos environnements pour utiliser la dernière version. Voir [ Mise à niveau rapide ](fastly-configuration.md#upgrade-the-fastly-module).

## Compte de service et informations d’identification Fastly

Adobe Commerce sur les projets d’infrastructure cloud ne dispose pas d’un compte Fastly dédié. Le service Fastly est géré dans un compte centralisé enregistré dans Adobe, et le tableau de bord de gestion n’est accessible qu’à l’équipe d’assistance cloud.

Au lieu de cela, chaque environnement d’évaluation et de production dispose d’informations d’identification Fastly uniques (jeton API et ID de service) pour configurer et gérer les services Fastly à partir de l’administrateur Commerce. L’API Fastly est disponible pour effectuer une gestion avancée du service Fastly, qui nécessitera les informations d’identification pour envoyer ces requêtes.

Pendant la mise en service du projet, Adobe ajoute votre projet au compte de service Fastly pour Adobe Commerce sur l’infrastructure cloud et ajoute les informations d’identification Fastly à la configuration pour les environnements d’évaluation et de production. Voir [Obtention des informations d’identification Fastly](fastly-configuration.md#get-fastly-credentials).

### Modifier le jeton API Fastly

Envoyez un ticket d’assistance Adobe Commerce pour modifier les informations d’identification du jeton API Fastly. Lorsque vous recevez le nouveau jeton, mettez à jour votre environnement d’évaluation ou de production pour utiliser le nouveau jeton.

**Pour modifier les informations d’identification du jeton API Fastly** :

1. [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) demandant de nouvelles informations d’identification d’API Fastly.

   Incluez votre Adobe Commerce sur l’ID de projet d’infrastructure cloud et les environnements qui nécessitent de nouvelles informations d’identification.

1. Une fois que vous avez reçu le nouveau jeton API, mettez à jour la valeur du jeton API dans la [configuration des informations d’identification Fastly](fastly-configuration.md#test-the-fastly-credentials) dans Admin ou à partir des [[!DNL Cloud Console] variables d’environnement](../project/overview.md#configure-environment).

1. [Tester les nouvelles informations d’identification](fastly-configuration.md#test-the-fastly-credentials).

1. Après avoir mis à jour les informations d’identification, envoyez un ticket d’assistance Adobe Commerce pour supprimer l’ancien jeton API.

### Comptes Fastly multiples et domaines attribués

Fastly ne vous permet d’affecter qu’un seul domaine apex et ses sous-domaines associés à un seul service et compte Fastly. Si vous disposez déjà d’un compte Fastly qui lie les mêmes apex et sous-domaines utilisés pour votre site Adobe Commerce, vous disposez des options suivantes :

- Supprimez les apex et les sous-domaines du compte existant avant de demander les informations d’identification de service Fastly pour vos environnements de projet d’infrastructure cloud Adobe Commerce. Voir [Utilisation des domaines] dans la documentation Fastly.

  Utilisez cette option pour lier le domaine apex et tous les sous-domaines au compte de service Fastly pour Adobe Commerce sur l’infrastructure cloud.

- Envoyez un ticket d’assistance Adobe Commerce pour demander la délégation de domaine afin que les apex et les sous-domaines puissent être liés à différents comptes.

  Utilisez cette option si vous disposez d’un domaine apex qui comporte plusieurs sous-domaines pour les sites Adobe Commerce et non Adobe Commerce et que vous souhaitez lier ces sous-domaines à différents comptes Fastly.

#### Délégation de domaine de demande

*Scénario 1:*

Le domaine apex (`testweb.com` et `www.testweb.com`) est lié à un compte Fastly existant. Vous disposez d’un projet d’infrastructure cloud Adobe Commerce configuré avec les sous-domaines suivants : `mcstaging.testweb.com` et `mcprod.testweb.com`. Vous ne souhaitez pas déplacer le domaine apex vers le compte de service Fastly pour Adobe Commerce sur l’infrastructure cloud.

Envoyez un [ticket d’assistance Fastly] demandant que les sous-domaines soient délégués du compte Fastly existant au compte Fastly pour Adobe Commerce sur l’infrastructure cloud. Incluez votre ID de projet Adobe Commerce dans le ticket.

Une fois la délégation terminée, les sous-domaines de votre projet peuvent être ajoutés au compte de service Fastly pour Adobe Commerce sur l’infrastructure cloud. Voir [Obtention des informations d’identification Fastly](fastly-configuration.md#get-fastly-credentials).

*Scénario 2:*

Le domaine apex (`testweb.com` et `www.testweb.com`) est lié au compte de service Fastly Adobe Commerce sur les infrastructures cloud. Vous souhaitez gérer les services Fastly pour les sous-domaines `service.testweb.com` et `product-updates.testweb.com` à partir d’un autre compte Fastly.

Envoyez un ticket d’assistance Adobe Commerce demandant que les sous-domaines soient délégués du compte de service Fastly d’Adobe Commerce sur l’infrastructure cloud au compte Fastly . Incluez l’ID de service du compte Fastly dans le ticket.

## Protection DDoS

La protection DDOS est intégrée au service Fast CDN. Une fois que vous avez activé les services Fastly pour vos sites Adobe Commerce, Fastly filtre tout le trafic web et administrateur afin de détecter et bloquer les attaques potentielles.

- Pour les attaques ciblant la couche 3 ou 4, le service Fastly filtre le trafic en fonction du port et du protocole, en examinant uniquement les requêtes HTTP ou HTTPS. ICMP, UDP et d’autres attaques lancées par le réseau sont ignorés à la périphérie de notre réseau. Cela inclut les attaques par réflexion et amplification, qui utilisent des services UDP tels que SSDP ou NTP. En fournissant ce niveau de protection, nous bloquons efficacement plusieurs attaques courantes telles que le Ping of Death, les attaques de Schtroumpfs et d&#39;autres inondations basées sur ICMP.

  Fast gère les attaques de niveau TCP au niveau de la couche de cache. Cette stratégie fournit l&#39;échelle et le contexte nécessaires par client pour faire face à une attaque de CRUE SYN et à ses nombreuses variantes, y compris la pile TCP, les attaques de ressources et les attaques TLS dans les systèmes Fastly.

- Fastly fournit également une protection contre les attaques de couche 7. Si votre boutique rencontre des problèmes de performances et que vous suspectez une attaque DDoS de couche 7, envoyez un ticket d’assistance Adobe Commerce. Adobe peut créer et appliquer des règles personnalisées au service Fastly afin de rechercher et de filtrer les requêtes malveillantes en fonction de l’en-tête, de la payload ou d’une combinaison d’attributs qui identifient le trafic d’attaque. Voir [Recherche d’attaques DDoS] et [Comment bloquer le trafic malveillant] dans le *Centre d’aide d’Adobe Commerce*.

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Recherche des attaques DDoS]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Module Fast CDN pour Magento 2]: https://github.com/fastly/fastly-magento2

[Ticket d’assistance Fastly]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[Comment bloquer le trafic malveillant]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[Utilisation des domaines]: https://docs.fastly.com/en/guides/working-with-domains
