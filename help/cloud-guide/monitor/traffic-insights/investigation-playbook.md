---
title: Guide d’investigation
description: Découvrez comment analyser la surcharge de bande passante du réseau CDN, le chargement des robots et des robots d'exploration de recherche, ainsi que le trafic malveillant à l’aide des informations de trafic Adobe Commerce, et quand remonter.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# Manuel d’investigation

L’application [!DNL Adobe Commerce Traffic Insights] est conçue pour vous aider à examiner les problèmes suivants :

- Dépassement de bande passante
- charge du robot d&#39;exploration
- Trafic malveillant

Vous pouvez également demander [Sécurité avancée : gestion native des robots, DDoS de couche 7 et limitation de débit](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting), chemin d’escalade natif Adobe lorsque la réduction manuelle est insuffisante. Chaque étape fait référence au widget qui affiche le symptôme, de sorte que vous puissiez passer d’une mesure à une action concrète.

>[!WARNING]
>
>Les suggestions de cette page ne sont que des indications. Validez toujours toute règle de blocage par rapport à votre propre trafic avant de la déployer.

## Dépassement de bande passante du réseau CDN

Avant de prendre en compte les dépassements de bande passante, comprenez comment la bande passante est facturée. Le trafic pour **tous** les services Fastly regroupés avec le compte [!DNL Adobe Commerce on Cloud Infrastructure], y compris chaque environnement de production **et** d’évaluation, est comptabilisé dans l’utilisation commune par rapport à l’allocation annuelle prévue dans votre contrat. Commencez par **Bande passante > Bande passante totale**, puis attribuez le volume avec **Bande passante par type de contenu** et **Bande passante par détails de domaine**.

### Contenu multimédia

Certains magasins fournissent légitimement une grande partie de la bande passante en tant que médias en raison de leur catalogue. Si l’option **Bande passante par type de contenu** indique une quantité importante de bande passante multimédia, tenez compte des restrictions suivantes :

- Testez la [conversion avec perte rapide](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion) pour diffuser des images plus petites et de qualité inférieure.
- Examinez [Fast Deep Image Optimization](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization) pour générer des images redimensionnées du côté du réseau de diffusion de contenu (CDN).

### Fichiers volumineux

Certains sites contiennent des fichiers volumineux ou des réponses spécifiques importantes, par exemple, les intégrations ou les exportations de la planification des ressources de l’entreprise (ERP). Utilisez **URL par bande passante** pour consulter les colonnes **BW** et **Taille moyenne** afin de trouver ces fichiers volumineux. Vous pouvez utiliser **Path Segment lvl 1 By Bandwidth** pour une vue de niveau supérieur.

### Lourd 404

Une page Adobe Commerce **404 introuvable** est généralement une page lourde avec un thème (~1,5 Mo) et **impossible à mettre en cache**. Par conséquent, des pages 404 répétées peuvent générer un trafic anormal. Même une ressource manquante triviale comme `favicon.ico` peut se transformer en une page `404` lourde au lieu d’un petit fichier. Utilisez les colonnes **404** et **404 BW** dans **Bande passante par détails de domaine**, **URL par bande passante**, **Principales adresses IP par bande passante** et **Stats par sous-réseaux IP** pour rechercher des clients, des adresses IP et des URL générant systématiquement un volume 404. Ensuite, réduisez ou limitez cet accès. Par exemple, renvoyez plutôt un `403` léger.

### Faible taux d’accès FPC

[!DNL Adobe] recommande d’activer Fastly [blindage](https://www.fastly.com/documentation/guides/concepts/shielding/) afin qu’un agrégateur de cache CDN principal serve l’origine, ce qui permet de réduire le nombre de requêtes qui l’atteignent à partir des points de présence locaux ([POP](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)) les plus proches du client. Voir [vérification de votre configuration](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding).

Le trafic POP-to-client et shield-to-POP est comptabilisé séparément. Bien que la réponse client soit compressée, le trafic shield-to-POP [n’est pas compressé](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) afin de préserver la prise en charge des inclusions côté Edge ([ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/)). Cela signifie qu’un faible taux d’accès au cache de page complète (FPC) entraîne une bande passante beaucoup plus élevée sur les pages dynamiques. Confirmez le symptôme avec **Taux d&#39;accès FPC**, **Statistiques FPC par domaine** et **Bande passante de segment réseau CDN**.

Un faible taux d’accès est souvent dû à un grand volume de robots d&#39;exploration de moteurs de recherche (voir [robots et robots d&#39;exploration de recherche](#search-bots-and-crawlers)). Une autre solution consiste à [servir un cache obsolète aux robots d&#39;exploration &#x200B;](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/) lorsqu’il est disponible. Si des invalidations du cache étendues et fréquentes en sont la cause, utilisez **Invalidation du cache par balises** et **Âge FPC par URL principales** pour rechercher les balises/URL résiliées.

## Robots de recherche et robots d&#39;exploration

Pour évaluer l’impact du robot d&#39;exploration, commencez par **Robots connus par la bande passante** et **Détails sur l’impact des robots connus** pour identifier les robots les plus actifs, puis [filtrez](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use) par un robot spécifique afin d’étudier uniquement ses requêtes.

### Trop de requêtes

La cause la plus courante d’un robot de recherche qui envoie trop de requêtes se produit lors de l’analyse des pages qui contiennent des `<meta name="robots" content="index,follow">`. Les robots peuvent suivre les liens de navigation supérieure et en couches dans une boucle quasi infinie. Envisagez les options suivantes pour résoudre ce problème :

>[!WARNING]
>
> Consultez un expert en optimisation du moteur de recherche (SEO) avant de restreindre l’activité du robot d&#39;exploration. Le recyclage peut avoir un impact négatif sur votre SEO.

- Ajoutez des `nofollow` aux liens de navigation supérieure et en couches, par exemple `<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`.
- Remplacez la balise meta de la page par `index,nofollow`, soit en tant que paramètre de configuration de conception [&#x200B; courant](https://experienceleague.adobe.com/fr/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt) soit en fonction du type de page avec des extensions personnalisées. Veillez à ce que les `sitemap.xml` soient toujours exactes, de sorte que les robots disposent toujours d’une liste à jour des pages à indexer.
- Mettez à jour les `robots.txt` pour bloquer les chemins et les ressources auxquels les robots ne doivent pas accéder.
- Notez que la directive `crawl-delay` ne fait pas partie du protocole d&#39;exclusion des robots officiel, mais elle fonctionne pour certains robots, tels que Bingbot, Slurp, SEMrushBot, et quelques autres. Googlebot ignore cette directive.
- Ajoutez des règles de limite de débit. Le module Fastly offre une [protection contre les robots d&#39;exploration abusifs](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection) native. Pour un contrôle plus précis, un [fragment de code VCL (Varnish Configuration Language)](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets) personnalisé peut renvoyer des `429` (Too Many Requests) ou des `405` (Method Not Allowed) pour une expression régulière agent-utilisateur avec une limite de taux individuelle. Consultez la documentation du robot d&#39;exploration pour connaître la méthode et le code de réponse préférés. Voir le guide Fastly sur le [guidage VCL à limitation de débit](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/).
- L’IA et les robots d&#39;exploration de grands modèles linguistiques (LLM) constituent un cas particulier de plus en plus fréquent. Ils ne s’identifient pas toujours de manière cohérente, de sorte que les règles user-agent VCL peuvent prendre du retard. Le module complémentaire Adobe [Sécurité avancée](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/advanced-security) dispose de [gestion native des robots](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting) qui permet de distinguer les robots d&#39;exploration d’IA vérifiés des suspects et les récupérateurs en périphérie, ce que VCL à lui seul ne peut pas faire.

### Blocage des robots d&#39;exploration indésirables

Si certains moteurs de recherche génèrent un trafic important et ne sont pas importants pour l’entreprise, ils peuvent être entièrement bloqués :

- Certains robots suivent `robots.txt` modifications 1 à 2 jours plus tard, après avoir relu et mis à jour leurs règles d’analyse.
- Si un robot d&#39;exploration ignore `robots.txt`, bloquez-le avec un fragment de code VCL personnalisé ([exemple](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent)). Certains robots d&#39;exploration documentent explicitement cela comme la méthode préférée ou unique de contrôle de fréquence.

## Scripts et scrapers malveillants

Utilisez l’application Informations sur le trafic pour identifier les directions courantes des attaques, en filtrant par zones cible selon les besoins. Si les requêtes avec indicateur rouge proviennent principalement de certaines adresses IP, de certains sous-réseaux ou de certaines géolocalisations (**Principales adresses IP par nombre de requêtes**, **Statistiques par sous-réseaux IP**, **Statistiques par pays**), envisagez de les bloquer avec le VCL Fastly personnalisé.

Chaque projet d’infrastructure cloud dispose déjà d’une base de protection automatique, quelle que soit la configuration utilisée. Le pare-feu d’application web (WAF) inclus bloque immédiatement l’injection SQL et les signaux IP malveillants connus (backdoor, outil d’attaque, CMDEXE, Log4J-JNDI, traversal, XSS), et limite la fréquence des autres adresses IP non malveillantes une fois qu’elles traversent 50 requêtes/minute, 350 requêtes/10 minutes ou 1 800 requêtes/heure. C’est cette ligne de base qui est indiquée par **Réponse Requests By WAF** et les colonnes de signaux WAF dans les tableaux de cette application. Un pic dans ces colonnes ne signifie pas nécessairement que vous n’êtes pas protégé.

- Recherchez le bourrage d’informations d’identification, la prise de contrôle de compte, la création de faux comptes, les tests de carte, le grattage de contenu et l’accumulation d’inventaire/de panier. Ces modèles d’abus pilotés par les robots sont affichés dans l’onglet **Analyse des activités et des demandes de robots**. Un trafic à volume élevé et faible diversité atteignant les points d’entrée de connexion, de compte, de passage en caisse ou de catalogue est la signature à rechercher dans **Nombre d’adresses IP les plus utilisées par les requêtes** et **Détails de l’impact des robots connus**.
- Protégez les points d’entrée de l’API de passage en caisse et de passage en caisse des attaques de robots avec [Google reCAPTCHA](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/security/captcha/security-google-recaptcha).
- Utilisez la limite de débit native du module Fastly [protection de chemin](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection).
- Cochez [Signaux WAF de nouvelle génération](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/) dans le champ `Sigsci_Tags` séparés par des virgules et associez les correspondances de signal pertinentes dans une règle de blocage ciblée. La valeur d’une requête suspecte peut ressembler à `BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT`. Le WAF attribue un libellé à une adresse IP dont la `SITE-FLAGGED-IP` est comprise jusqu’à un certain seuil avant de commencer le blocage automatique. Les widgets **Signaux d’attaque et d’anomalie** **Signaux de robot WAF** et **Requêtes par réponse WAF**, ainsi que les colonnes WAF dans les tables IP, de sous-réseau et de pays, font apparaître ces éléments.
- Consultez l’article d’Adobe sur [le blocage du trafic malveillant pour Adobe Commerce au niveau Fastly](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level) pour connaître les approches courantes.
- Pour les scénarios complexes dans lesquels le blocage manuel n’est pas une option viable, comme les campagnes de robots prolongées, les attaques réparties sur de nombreuses adresses IP/API ou les déni de service distribué (DDoS) de couche 7, envisagez d’abord d’Adobe le module complémentaire [Sécurité avancée](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/advanced-security) (voir [gestion native des robots](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)). Il fonctionne sur le même Fastly edge desservant votre storefront. Si vous avez besoin de fonctionnalités en dehors de son champ d’application, un service de réduction des robots géré par un tiers avec l’intégration native de Fastly, tel que [Datadome](https://docs.datadome.co/docs/module-fastly) ou [HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/) (anciennement PerimeterX), est la solution de remplacement suggérée. Toutes ces options entraînent des coûts supplémentaires.

## Sécurité avancée : gestion native des robots, DDoS de couche 7 et limitation de débit

Les sections précédentes expliquent ce qui peut être fait avec les données et le manuel Fastly VCL de l’application Traffic Insights. Pour les scénarios où cela n’est pas suffisant, comme les campagnes de robots continues ou en évolution, les attaques DDoS de couche 7 (couche d’application), ou les abus dispersés sur de nombreuses adresses IP et points d’entrée d’API, Adobe offre une [sécurité avancée](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/advanced-security).

Advanced Security est un module complémentaire payant pour [!DNL Adobe Commerce on Cloud Infrastructure] qui ajoute la gestion des robots de périphérie (y compris la détection de robot d&#39;exploration et d&#39;extraction par l&#39;IA), la protection DDoS de couche 7 et la limitation de débit avancée sur la même plateforme Fastly qui dessert déjà le storefront. Consultez la section [Sécurité avancée](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/advanced-security) pour connaître toutes les fonctionnalités, les limites actuelles et comment les demander.

Une fois acheté et activé, utilisez l’application Informations de trafic pour vérifier que la sécurité avancée fonctionne. Ses décisions sont signalées par le biais des mêmes champs de `Sigsci_Tags` et de `Agent_response` derrière **Signaux d’attaque et d’anomalie de WAF**, **Signaux de robots WAF** et **Requêtes par la réponse WAF**. Comparez ces widgets avant et après leur activation pour confirmer qu’ils agissent activement sur votre trafic.
