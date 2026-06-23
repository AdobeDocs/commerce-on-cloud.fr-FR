---
title: Pare-feu d’application web (WAF)
description: Découvrez comment le service Fastly WAF détecte, consigne et bloque le trafic de requêtes malveillantes avant qu’il ne puisse endommager le réseau ou les sites Adobe Commerce.
feature: Cloud, Configuration, Security
exl-id: f00e35f2-9800-4e24-a4d0-d36fde59a003
TQID: https://experienceleague.adobe.com/GhpLOxZbJMYhBTmj8W4a90wfmFYq-8h2bvrKQWj6ZWk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: b5f00040-57a0-4a6d-a39e-383b1936c2c9id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: bd989d82-1e15-4534-88db-f1f51dd77ffaid: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7
subfeature_v2: id: f2261633-201d-46c5-8a66-999e70527a83
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 987
ht-degree: 0%

---

# Pare-feu d’application web (WAF)

Optimisé par Fastly, le service de pare-feu d’application web (WAF) pour Adobe Commerce sur l’infrastructure cloud détecte, consigne et bloque le trafic de requêtes malveillantes avant qu’il ne puisse endommager vos sites ou votre réseau. Le service WAF est disponible uniquement dans les environnements de production.

Le service WAF offre les avantages suivants :

- **Conformité PCI** : l’activation de WAF garantit que les vitrines Adobe Commerce dans les environnements de production répondent aux exigences de sécurité PCI DSS 6.6.
- **Politique WAF par défaut** : la politique WAF par défaut, configurée et conservée par Fastly, fournit un ensemble de règles de sécurité conçues pour protéger vos applications web Adobe Commerce contre un large éventail d’attaques, notamment les attaques par injection, les entrées malveillantes, le cross-site scripting, l’exfiltration de données, les violations de protocole HTTP et autres [menaces du top dix d’OWASP](https://owasp.org/www-project-top-ten/) en matière de sécurité.
- **Intégration et activation de WAF**—Adobe déploie et active la politique WAF par défaut dans votre environnement de production dans les 2 à 3 semaines suivant la fin de la mise en service.
- **Soutien à l&#39;exploitation et à la maintenance**—
   - Adobe et Fastly configurent et gèrent vos journaux, règles et alertes pour le service WAF.
   - Adobe trie les tickets du service clientèle liés aux problèmes de service WAF qui bloquent le trafic légitime en tant que problèmes de priorité 1.
   - Les mises à niveau automatisées vers la version de service WAF assurent une couverture immédiate des exploits nouveaux ou en évolution. Voir [Maintenance et mises à niveau de ](#waf-maintenance-and-updates).

>[!TIP]
>
>Pour plus d’informations sur le maintien de la conformité PCI pour votre Adobe Commerce sur les magasins d’infrastructure cloud, voir [Conformité PCI](https://business.adobe.com/products/magento/pci-compliance.html).

## Activation du WAF

Adobe active le service WAF sur de nouveaux comptes dans les 2 à 3 semaines suivant la fin de l’approvisionnement. Le WAF est implémenté via le service Fastly CDN. Vous n&#39;avez pas à installer ou à entretenir de matériel ou de logiciel.

>[!NOTE]
>
>Avant de pouvoir utiliser le service WAF, vous devez acheminer tout le trafic externe vers votre projet d’infrastructure cloud d’Adobe Commerce via le service Fastly . Voir [ Configuration rapide ](fastly-configuration.md).

## Fonctionnement

Le service WAF s’intègre à Fastly et utilise la logique de cache dans le service de réseau CDN Fastly pour filtrer le trafic aux nœuds globaux Fastly. Nous activons le service WAF dans votre environnement de production avec une politique WAF par défaut basée sur les règles [ModSecurity Rules de Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) et les dix principales menaces de sécurité d’OWASP.

Le service WAF inspecte le trafic HTTP et HTTPS (requêtes GET et POST) par rapport à l’ensemble de règles WAF et bloque le trafic malveillant ou non conforme à des règles spécifiques. Le service inspecte uniquement le trafic lié à l’origine qui tente d’actualiser le cache. Par conséquent, nous arrêtons la plupart du trafic d’attaque au cache Fastly, protégeant ainsi votre trafic d’origine des attaques malveillantes. En traitant uniquement le trafic d’origine, le service WAF préserve les performances du cache, introduisant uniquement une latence estimée entre 1,5 milliseconde et 20 millisecondes pour chaque requête non mise en cache.

## Résolution des problèmes liés aux requêtes bloquées

Lorsque le service WAF est activé, il inspecte tout le trafic web et administrateur par rapport aux règles de WAF et bloque toute requête web qui déclenche une règle. Lorsqu’une requête est bloquée, le demandeur voit une page d’erreur de `403 Forbidden` par défaut qui inclut un identifiant de référence pour l’événement de blocage.

![Page d&#39;erreur ](../../assets/cdn/fastly-waf-403-error.png)

Vous pouvez personnaliser cette page de réponse d’erreur à partir de l’Administration. Voir [Personnaliser la page de réponse de WAF](fastly-custom-response.md#customize-the-waf-error-page).

Si votre page d’administration Adobe Commerce ou votre storefront renvoie une page d’erreur `403 Forbidden` en réponse à une requête d’URL légitime, envoyez un [ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case). Copiez l’ID de référence de la page de réponse d’erreur et collez-le dans la description du ticket.

Pour identifier la réponse WAF pour une requête spécifique à l’aide de New Relic, reportez-vous aux sections suivantes :

- `Agent_response` : indique le code de réponse WAF (`200` signifie bien et `406` signifie bloqué).
- `sigsci` des balises—Associe la demande à une balise Signal Sciences particulière en fonction de la nature de la demande

## Maintenance et mises à jour de WAF

Fastly met à jour et déploie des correctifs pour les nouveaux CVE/règles modélisées en fonction des mises à jour de règles provenant de tiers commerciaux, de recherches Fastly et de sources ouvertes. Met rapidement à jour les règles publiées dans une politique selon les besoins ou lorsque des modifications des règles sont disponibles à partir de leurs sources respectives. Fastly peut également ajouter des règles correspondant aux classes de règles publiées dans l’instance WAF de n’importe quel service une fois que le service WAF est activé. Ces mises à jour assurent une couverture immédiate des exploits nouveaux ou en évolution.

Adobe et Fastly gèrent le processus de mise à jour pour s’assurer que les règles WAF nouvelles ou modifiées fonctionnent efficacement dans votre environnement de production avant le déploiement des mises à jour en mode bloquant.

## Problèmes

Si vous constatez que le WAF bloque les requêtes légitimes, il s’agit souvent de faux positifs qui devront être contournés ou pour lesquels une solution doit être mise en œuvre au niveau du service WAF. Envoyez un ticket d’assistance et incluez l’URL affectée, les étapes exactes pour reproduire l’erreur et la référence de l’erreur sous forme de texte (par opposition à une capture d’écran) pour éviter les erreurs de transcription.

## Restrictions

Le service WAF standard optimisé par Fastly ne prend pas en charge les fonctionnalités suivantes :

- Protection contre les programmes malveillants ou les robots : envisagez d&#39;utiliser [listes de contrôle d&#39;accès](./fastly-vcl-allowlist.md) ou un service tiers.
- Limitation de débit : consultez la section [Limitation de débit](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) de la documentation Fastly ou consultez la section [Limitation de débit](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) de la sécurité _API Web Commerce_.
- Configuration d’un point d’entrée de journalisation pour le client : consultez la section [Service PrivateLink](../development/privatelink-service.md) comme alternative.

Le service WAF vous permet de bloquer ou d’autoriser le trafic en fonction des adresses IP. Vous pouvez ajouter des listes de contrôle d’accès (ACL) et des fragments de code VCL personnalisés à votre service Fastly pour spécifier les adresses IP et la logique VCL pour bloquer ou autoriser le trafic. Voir [Extraits personnalisés Fastly VCL](fastly-vcl-custom-snippets.md).

Le service WAF ne prend pas en charge le filtrage des requêtes TCP, UDP ou ICMP. Cependant, cette fonctionnalité est fournie par la protection DDoS intégrée incluse avec le service Fast CDN. Voir [ Protection DDoS ](fastly.md#ddos-protection).

