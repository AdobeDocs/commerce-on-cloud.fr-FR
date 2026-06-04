---
title: Sécurité avancée Adobe Commerce
description: Découvrez comment Advanced Security ajoute la gestion des robots, la limitation de débit avancée et la protection DDoS de couche 7 à Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Configuration, Security
exl-id: 7aeb189f-be69-45d5-8163-4748424083c0
source-git-commit: 0b3ef117f85c990c2a01ecb655c930b8c4f61acb
workflow-type: tm+mt
source-wordcount: '2474'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

[!DNL Adobe Commerce Advanced Security] est un produit qui fonctionne avec [!DNL Adobe Commerce on Cloud Infrastructure] pour que votre boutique en ligne soit rapide, disponible et sécurisée. Cela permet de protéger le chiffre d’affaires, de réduire les temps d’arrêt et de maintenir la confiance des clients pendant les pics de trafic et les attaques automatisées.

[!DNL Adobe Commerce on Cloud Infrastructure] comprend une protection DDoS de couche 3 et 4 intégrée [](./fastly.md#ddos-protection) et un [pare-feu d’application web (WAF)](./fastly-waf-service.md). Dans le cadre du [modèle de responsabilité partagée](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility), la détection DDoS de couche 7, la protection des robots et le blocage proactif des adresses IP sont des responsabilités des commerçants, que [!DNL Adobe Commerce Advanced Security] est conçu pour traiter.

[!DNL Advanced Security] étend la protection du storefront grâce à des fonctionnalités de sécurité de périphérie optimisées par Fastly, qui offre une gestion des robots, une limitation de débit avancée et une protection DDoS de couche 7 dans le cadre d&#39;une plateforme de périphérie unifiée qui combine l&#39;évolutivité, les performances et la sécurité à la périphérie du réseau.

>[!NOTE]
>
>[!DNL Advanced Security] est disponible uniquement pour les projets [!DNL Adobe Commerce on Cloud Infrastructure] (PaaS).

## Fonctionnalités principales

[!DNL Adobe Commerce Advanced Security] comprend les protections supplémentaires suivantes :

- **[Gestion des robots](https://docs.fastly.com/products/bot-management)** : identifie et réduit les activités de robots indésirables sur vos applications web. Le service de gestion des robots fait la distinction entre les robots légitimes (robots d&#39;exploration de moteurs de recherche, robots de médias sociaux) et les robots malveillants, en fournissant une classification en temps réel à la périphérie du réseau avec des options pour bloquer, autoriser, contester ou limiter le trafic.

- **[Protection DDoS](https://docs.fastly.com/products/fastly-ddos-protection)** : fournit une protection DDoS de couche 7 (couche d&#39;application) au-delà de la protection existante des couches 3 et 4 incluse dans tous les projets [!DNL Adobe Commerce on Cloud Infrastructure]. Le service de protection contre les attaques DDoS absorbe les attaques volumétriques à grande échelle et assure la disponibilité continue des applications pendant les événements de déni de service distribué (DDoS), protégeant ainsi les recettes pendant les périodes de trafic élevé.

- **[Limitation de débit avancée](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)** : fournit des règles de limitation de débit configurables qui protègent des URL spécifiques, des points d&#39;entrée d&#39;API et des ressources d&#39;application contre les abus. Le service de limitation de débit avancée va au-delà de la [limitation de débit de base](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) disponible via le module Fast CDN pour cibler des modèles de trafic et des vecteurs d’attaque spécifiques, ce qui réduit la contrainte sur l’infrastructure et les coûts cloud.

>[!NOTE]
>
>Les configurations [!DNL Advanced Security] nécessitent actuellement l’envoi d’un ticket d’assistance. La configuration en libre-service via l’interface utilisateur d’administration est prévue pour une version ultérieure. Pour plus d’informations, voir [Demande [!DNL Advanced Security]](#request-advanced-security).

>[!IMPORTANT]
>
>**Limites actuelles**
>
>Jusqu’à la fin du 3e trimestre 2026, les clients ne peuvent pas modifier ni gérer directement les règles de gestion des robots.
>
>Pour ajouter, modifier ou ajuster une règle, contactez l’assistance Adobe Commerce au moyen d’un ticket d’assistance [](https://experienceleague.adobe.com/home?support-tab=home#support). L’équipe d’assistance mettra en œuvre les modifications demandées.
>
>À compter du 4e trimestre 2026, Fastly prévoit de publier une fonctionnalité complémentaire qui permettra aux clients de gérer les règles de gestion des robots dans le panneau d’administration Commerce.

## Règles et protections par défaut

Les règles et protections par défaut suivantes sont disponibles avec [!DNL Advanced Security].

### DDoS de couche 7

- Les seuils DDoS sont intégrés à la plateforme CDN Fastly et ne peuvent actuellement pas être personnalisés par client.
- Les journaux du trafic bloqué par les protections DDoS ne sont pas directement visibles par les clients.
- Sur demande, l’assistance Adobe Commerce peut fournir des détails relatifs au trafic DDoS bloqué.
- Des fonctionnalités natives de transfert de journal DDoS sont attendues dans une prochaine version.

### Gestion des robots

Les protections de base suivantes pour la gestion des robots sont disponibles via le tableau de bord Signal Sciences de Fastly.

| Type de règle | Statut | Visibilité |
|---|---|---|
| Bloquer le trafic marqué comme ROBOT suspect incorrect | Activé par défaut lors de l’intégration | Visible dans les journaux New Relic sous `sigsci_tags` |
| Bloquer le trafic en fonction d’une balise spécifique (balise &lt;sigsci>) | Configuré uniquement lorsque cela est nécessaire en collaboration avec le client | Visible dans les journaux New Relic sous `sigsci_tags` |
| Limitation du débit pour des API ou des modèles d’URL spécifiques | Configuré uniquement lorsque cela est nécessaire en collaboration avec le client | Le trafic bloqué est visible dans les journaux New Relic sous `Agent_response` |
| Défi dynamique pour des API ou des modèles d’URL spécifiques | Configuré uniquement lorsque cela est nécessaire en collaboration avec le client | Le trafic bloqué est visible dans les journaux New Relic sous `Agent_response` |
| Défi du navigateur | Configuré uniquement lorsque cela est nécessaire en collaboration avec le client | Le trafic bloqué est visible dans les journaux New Relic sous `Agent_response` |

## Observability — surveillance de la protection des robots et de l’activité du FANG

Les journaux CDN sont automatiquement transférés au compte New Relic du client. Pour plus d’informations, consultez la section [Gestion des journaux](../monitor/log-management.md).

Les journaux du réseau CDN incluent la télémétrie intégrée de Signal Sciences (protection des robots/WAF de nouvelle génération), ce qui permet aux clients de surveiller les événements de sécurité directement dans New Relic.

Les champs clés sont les suivants :

- **`Sigsci_Tags`** : indique les classifications et les balises appliquées par Signal Sciences.
- **`Agent_response`** : indique l&#39;action entreprise par l&#39;agent de protection des robots/NGWAF.

Exemples :

- Pour identifier le trafic bloqué par les règles de protection des robots ou NGWAF :

  `Agent_response:"406"`

  Un code de réponse 406 indique que la requête a été bloquée par les contrôles de sécurité.

- Pour identifier les requêtes marquées comme robots suspects :

  `Sigsci_Tags:"*SUSPECTED-BAD-BOT*"`

Vous pouvez utiliser ces champs pour créer des tableaux de bord, des alertes et des investigations dans New Relic afin de surveiller l’activité des robots, les demandes bloquées et d’autres événements liés à la sécurité.

## Les fonctionnalités VCL existantes restent inchangées

L’activation du module complémentaire [!DNL Advanced Security] ne modifie ni ne remplace les contrôles de sécurité Fastly basés sur VCL existants.

Les fonctionnalités de blocage de VCL existantes suivantes continuent à fonctionner sans aucune modification :

- Blocage par IP
- Blocage géographique
- Blocage basé sur l’agent utilisateur
- Blocage basé sur les signatures JA3
- Blocage basé sur les signatures JA4

Les clients peuvent continuer à utiliser les configurations VCL personnalisées existantes et les règles de sécurité parallèlement aux fonctionnalités de module complémentaire [!DNL Advanced Security].

Le module complémentaire [!DNL Advanced Security] fonctionne en plus du réseau CDN Fastly standard et des protections VCL existantes déjà disponibles dans [!DNL Adobe Commerce on Cloud Infrastructure].

## Couverture des menaces

[!DNL Advanced Security] protège les vitrines contre toute une gamme de menaces automatisées et de menaces au niveau des applications.

![Position de sécurité avancée dans la pile de sécurité Adobe Commerce](../../assets/advanced-security.svg)

### Abus commis par des robots

- **bourrage d’informations d’identification** : tente automatiquement de se connecter à l’aide d’informations d’identification volées suite à des violations de données.
- **Prise de contrôle de compte** : robots tentant d’accéder sans autorisation à des comptes clients.
- **Abus de création de compte** : création automatisée de faux comptes à des fins de fraude ou d’abus.
- **Test de carte** : robots qui testent les numéros de carte de crédit volés auprès de votre processeur de paiement.
- **Content scraping** : extraction automatisée des données, des prix ou du contenu des produits depuis votre storefront.
- **thésaurisation des stocks** - Des robots qui détiennent des produits dans des paniers pour empêcher les achats légitimes.

### Gestion des robots d’IA

- **Détection de robots d&#39;exploration d’IA** : identifie et gère les robots d&#39;exploration d’IA qui ratissent le contenu pour entraîner des modèles linguistiques volumineux sans consentement.
- **AI fetcher control** : contrôle les récupérateurs AI utilisés dans les résultats Recherche optimisée par l&#39;IA en temps réel.
- **Politiques de robots d’IA configurables** : fait la distinction entre les robots d’IA vérifiés et suspects avec des types de signaux configurables pour l’application des politiques.

### Attaques au niveau de la couche applicative

- **Attaques DDoS de couche 7** : attaques distribuées ciblant la couche applicative qui contourne les protections intégrées de couche 3 et 4. [!DNL Advanced Security] absorbe ces attaques volumétriques à la périphérie avant qu&#39;elles n&#39;atteignent vos serveurs d&#39;origine.
- **Abus d’URL et d’API** : les attaques ciblant des URL ou des points d’entrée d’API spécifiques s’étendent sur un grand nombre d’adresses IP, où le blocage d’adresses IP individuelles n’est pas efficace.
- **Attaques par violation du cache** : requêtes comportant des paramètres de requête manipulés conçus pour contourner la mise en cache CDN et surcharger le serveur d’origine.

### Fonctionnalités supplémentaires

- **Défis dynamiques** : attribue automatiquement le défi optimal au trafic suspect. Tire parti des jetons d’accès privé (PAT) pour valider facilement une partie des requêtes sans affecter l’expérience utilisateur.
- **Technologie de tromperie** : traite les tentatives de prise de contrôle de compte en renvoyant de fausses informations aux attaquant(e)s, en atténuant leur attaque tout en perturbant leur capacité à fonctionner à grande échelle.

## Choisir la bonne protection

Utilisez les conseils suivants pour déterminer si [!DNL Advanced Security] est la bonne solution pour vos besoins en matière de protection du storefront, ou si les protections existantes ou les solutions alternatives sont plus appropriées.

### Quand utiliser [!DNL Advanced Security]

Les scénarios suivants sont mieux gérés avec [!DNL Advanced Security] :

| Scénario | Comment [!DNL Advanced Security] aide |
|---|---|
| Votre site subit des attaques dirigées par des robots, telles que le bourrage d’informations d’identification, le grattage de contenu ou la thésaurisation d’inventaire | Bot Management identifie et réduit les menaces automatisées à la périphérie avant qu&#39;elles n&#39;atteignent votre application |
| Vous avez besoin d&#39;une protection DDoS de couche 7 au-delà de la couverture intégrée de couche 3 et 4 | La protection DDoS absorbe les attaques de couche applicative qui contournent les protections au niveau du réseau |
| Les URL spécifiques ou les points d’entrée d’API sont ciblés par un trafic distribué à volume élevé qui ne peut pas être bloqué par IP | La limitation de débit avancée fournit des contrôles granulaires pour des points d’entrée et des modèles de trafic spécifiques |
| Vous souhaitez gérer les robots d&#39;exploration d’IA et les récupérateurs accédant à votre contenu storefront | La gestion des robots comprend des politiques de détection et d’application des robots d’IA configurables |
| Vous avez besoin d’une solution de sécurité Edge prise en charge par Adobe intégrée à votre réseau CDN Fastly existant | [!DNL Advanced Security] s’exécute sur la même plateforme Fastly Edge qui sert déjà votre storefront |

### Quand utiliser les protections existantes ?

Les scénarios suivants sont mieux gérés avec les protections existantes :

| Scénario | Approche recommandée |
|---|---|
| Une seule adresse IP ou un petit ensemble d’adresses IP identifiables inonde votre site de requêtes | Bloquez les adresses IP à l’aide de l’API Commerce Admin ou Fastly . Utilisez la protection DDoS [couche 3/4](./fastly.md#ddos-protection) intégrée et les fragments de code VCL [IP](./fastly-vcl-blocking.md) existants. |
| Vous devez bloquer l’injection SQL, le cross-site scripting (XSS) ou d’autres menaces figurant dans le top dix d’OWASP | Le service [WAF inclus](./fastly-waf-service.md) bloque automatiquement ces menaces. |
| Vos schémas d&#39;attaque DDoS peuvent être contrôlés avec des règles de blocage VCL de base | Utilisez les [fragments de code VCL personnalisés](./fastly-vcl-custom-snippets.md) déjà disponibles avec Adobe Commerce. |

### Quand utiliser d’autres protections

Les scénarios suivants sont mieux gérés avec des protections alternatives qui peuvent compléter les [!DNL Advanced Security] :

| Scénario | Approche recommandée |
|---|---|
| Vous avez besoin d’une notation de la fraude au niveau des transactions ou d’une prévention des fraudes de paiement | Utiliser une plateforme dédiée à la prévention de la fraude. [!DNL Advanced Security] protège au niveau du réseau Edge et n&#39;évalue pas les transactions de paiement individuelles. |
| Vous avez besoin de la gestion des identités et des accès (IAM) | Implémentez une solution IAM dédiée. L’authentification des utilisateurs et la gestion des sessions restent des responsabilités du client. |
| Vous avez besoin de tests de sécurité des applications statiques ou dynamiques (SAST/DAST) | Utilisez des outils de test de sécurité des applications dédiés. L&#39;analyse de vulnérabilité au niveau du code n&#39;est pas fournie. |
| Vous avez besoin d’une sécurité API complète allant au-delà de la limitation de débit (comme la validation des schémas ou les fonctionnalités de passerelle API) | Envisagez une plateforme de sécurité d’API dédiée. |
| Vous avez besoin d’outils de conformité réglementaire tels que l’analyse PCI ou le reporting SOC | Utilisez des outils de gestion de la conformité dédiés. |

>[!TIP]
>
>Si vous utilisez actuellement un fournisseur tiers de protection contre les robots, la consolidation en [!DNL Advanced Security] peut réduire la complexité opérationnelle et éliminer les incohérences de couverture de sécurité entre les fournisseurs. Contactez votre équipe de compte Adobe pour évaluer les [!DNL Advanced Security] de votre projet.

## Positionnement de la pile de sécurité

[!DNL Advanced Security] s’intègre dans l’architecture de sécurité Adobe Commerce plus large en tant que couche supplémentaire de protection basée sur les périphériques. Il fonctionne en parallèle - et ne remplace pas - les protections DDoS de WAF et de couche 3/4 déjà incluses avec [!DNL Adobe Commerce on Cloud Infrastructure]. Les sections suivantes expliquent en quoi cette option est liée aux protections existantes et aux responsabilités qui incombent au client.

### Protections incluses

[!DNL Adobe Commerce on Cloud Infrastructure] comprend les fonctions de sécurité suivantes :

- **[Pare-feu d’application web (WAF)](./fastly-waf-service.md)** : protection gérée contre l’injection SQL, les scripts XSS et d’autres menaces liées aux projets Open Web Application Security Project (OWASP). Disponible uniquement sur les environnements de production.
- **[Protection DDoS de couches 3 et 4](./fastly.md#ddos-protection)** : protection intégrée contre les attaques de couche réseau telles que les inondations SYN, les inondations UDP, les attaques basées sur ICMP et les attaques de niveau TCP. Activé automatiquement avec le réseau CDN Fastly.
- **[Certificats SSL/TLS](./fastly-configuration.md#provision-ssltls-certificates)** : certificats de chiffrement validés par le domaine pour un trafic HTTPS sécurisé.
- **[Cloaking d&#39;origine](./fastly.md#origin-cloaking)** : assure tous les itinéraires de trafic via Fastly, en bloquant l&#39;accès direct aux serveurs d&#39;origine.
- **[Fragments de sécurité basés sur VCL](./fastly-vcl-custom-snippets.md)** : règles VCL (Custom Varnish Configuration Language) pour le blocage, le filtrage et le filtrage des demandes IP.

### [!DNL Advanced Security]

[!DNL Advanced Security] offre une protection accrue au-delà des protections intégrées incluses dans [!DNL Adobe Commerce on Cloud Infrastructure], mais avec un coût supplémentaire :

- **Gestion des robots** : détection et réduction des robots basés sur Edge avec la gestion des robots d’IA.
- **Protection DDoS de couche 7** : absorption et défense DDoS de couche applicative.
- **Limitation de débit avancée** : contrôles de débit granulaires pour les URL et les points d’entrée d’API.
- **Dynamic Challenges and Deception Technology** : attribution automatisée des défis et atténuation de la prise de contrôle des comptes.

### Responsabilité du client

- **Prévention de la fraude** — Notation des fraudes au niveau des transactions et détection des fraudes en matière de paiement.
- **Gestion des identités et des accès** : authentification, autorisation et gestion des sessions du client.
- **Test de sécurité des applications**—SAST/DAST et analyse des vulnérabilités.
- **Configurations de sécurité personnalisées**—règles basées sur VCL, places sur la liste autorisée IP et.
- **Outils de conformité** : analyses PCI, rapports de conformité SOC et outils d’audit réglementaire.
- **Renforcement au niveau de l’application** : authentification de l’API basée sur les jetons, normalisation des paramètres de requête et conception de stratégie de mise en cache.

Pour obtenir un aperçu complet des responsabilités d’Adobe et de la sécurité des clients, reportez-vous au [modèle de responsabilité partagée](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility).

## Schémas et protections d’attaque courants

Le tableau suivant met en correspondance les schémas d’attaque courants avec la couche de protection appropriée au sein de la pile de sécurité Adobe Commerce.

| Modèle d’attaque | Type | Protection |
|---|---|---|
| Une seule adresse IP ou un ensemble d’adresses IP identifiables envoyant un grand nombre de requêtes | DDoS + Robot | Bloquez les adresses IP à l’aide de l’API Commerce Admin ou Fastly . La protection DDoS de couche 3/4 intégrée filtre ce trafic à la périphérie du réseau. |
| Les attaques sur des URL ou des API spécifiques se propagent sur un grand nombre d’adresses IP | DDoS + Robot | **[!DNL Advanced Security]** : la limitation de débit avancée limite le volume de requêtes par URL. La gestion des robots identifie et bloque le trafic de robots distribué. |
| Attaques automatisées sur les points d’entrée de l’API REST sans authentification appropriée | Robot + DoS | Vérifiez que les points d’entrée de l’API utilisent l’authentification par jeton. Faire pivoter les informations d’identification si le jeton est compromis. **[!DNL Advanced Security]** : la limitation de débit avancée peut protéger les points d’entrée exposés. |
| Attaques de démantèlement de cache utilisant des paramètres de requête manipulés | Robot + DoS | Exclure les paramètres de requête non essentiels des clés de cache. Normalisez et limitez les paramètres de requête au niveau de l’application. **[!DNL Advanced Security]** : la gestion des robots détecte et bloque le trafic automatisé de contournement du cache. |
| Tentatives d’injection SQL ou de cross-site scripting (XSS) | WAF | Le service [WAF inclus](./fastly-waf-service.md) bloque automatiquement ces menaces à l&#39;aide de règles de sécurité gérées. |

### Comportement de blocage de WAF

Le comportement de WAF suivant s’applique à tous les projets [!DNL Adobe Commerce on Cloud Infrastructure], que le [!DNL Advanced Security] soit activé ou non. Le service WAF inclus utilise le comportement de blocage suivant pour les signaux d’attaque courants :

- Les requêtes **injection SQL** sont immédiatement bloquées, même pour une seule requête correspondante.
- Les requêtes identifiées avec les signaux de menace suivants provenant d&#39;une adresse IP malveillante connue sont immédiatement bloquées : Backdoor, Attack Tooling, CMDEXE, Log4J JNDI, Traversal et XSS.
- Les requêtes provenant d&#39;adresses IP non malveillantes qui présentent les signaux de menace ci-dessus sont bloquées lorsqu&#39;elles dépassent les seuils suivants :

| Intervalle | Seuil | Vérifier la fréquence |
|---|---|---|
| 1 minute | 50 requêtes | Toutes les 20 secondes |
| 10 minutes | 350 requêtes | Toutes les 3 minutes |
| 1 heure | 1 800 requêtes | Toutes les 20 minutes |

## [!DNL Advanced Security] de la demande

Pour demander une [!DNL Advanced Security] :

>[!NOTE]
>
>[!DNL Advanced Security] est disponible moyennant un supplément et nécessite un abonnement PaaS (Active [!DNL Adobe Commerce on Cloud Infrastructure]).

1. Contactez votre équipe de compte Adobe ou votre représentant commercial Adobe pour discuter des [!DNL Advanced Security] de votre projet.

1. Après l’achat de [!DNL Advanced Security], [envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) demandant l’activation du [!DNL Advanced Security]. Incluez votre ID de projet [!DNL Adobe Commerce on Cloud Infrastructure] et les environnements nécessitant une activation (par exemple, Production et Évaluation).

1. Adobe active [!DNL Advanced Security] sur votre service Fastly et configure les politiques de protection initiales. L’activation est généralement effectuée dans les jours ouvrables suivant l’envoi du ticket.

1. Vous recevez une confirmation indiquant que [!DNL Advanced Security] est actif, ainsi que des détails sur les protections activées pour vos environnements.

>[!NOTE]
>
>Les modifications de configuration apportées à [!DNL Advanced Security] nécessitent actuellement [envoi d’un ticket d’assistance](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). La configuration en libre-service via l’interface utilisateur d’administration est prévue pour une version ultérieure.

## Restrictions

[!DNL Advanced Security] offre une protection de storefront en couche de périphérie. Les fonctionnalités suivantes ne sont pas disponibles et sont mieux traitées avec des solutions complémentaires :

- **Cotation de la fraude au niveau des transactions** - [!DNL Advanced Security] n&#39;évalue pas les transactions de paiement individuelles pour le risque de fraude. Utilisez une plateforme dédiée à la prévention des fraudes pour la notation au niveau des transactions.
- **Gestion des identités et des accès (IAM)** : [!DNL Advanced Security] ne gère pas l’authentification des utilisateurs, l’autorisation ni la gestion des sessions. Ces responsabilités incombent toujours aux clients.
- **Tests de sécurité des applications statiques et dynamiques (SAST/DAST)**—[!DNL Advanced Security] n&#39;inclut pas d&#39;analyse de vulnérabilité au niveau du code ni de test de pénétration.
- **Sécurité des API** : bien que la limitation de débit avancée puisse protéger les points d’entrée d’API contre les abus, aucune fonctionnalité de sécurité d’API complète, telle que la validation des schémas et la gestion des passerelles d’API, n’est fournie.
- **Prévention complète de la fraude**—[!DNL Advanced Security] se concentre sur la protection du storefront à la périphérie et n&#39;est pas une plateforme complète de gestion de la fraude.
- **Outil de conformité**—[!DNL Advanced Security] ne fournit pas de fonctionnalités d&#39;analyse PCI, de reporting de conformité SOC ou d&#39;audit réglementaire.
