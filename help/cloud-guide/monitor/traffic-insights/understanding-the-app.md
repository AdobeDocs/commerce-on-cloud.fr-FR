---
title: Présentation de l’application
description: Découvrez le fonctionnement des statistiques de trafic Adobe Commerce, comment les piloter avec des filtres, comment leurs données sont mesurées, ainsi que leurs limites et performances de données.
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Présentation de l’application

L’application [!DNL Adobe Commerce Traffic Insights] visualise les journaux d’accès au réseau de diffusion de contenu (CDN) Fastly bruts dans une image du trafic Edge d’un magasin. Les graphiques sont regroupés dans les onglets suivants :

- **Bande passante** : répartition de la bande passante du trafic entre les domaines, les types de contenu et de ressources, ainsi que les projets cloud au fil du temps.
- **Performances de mise en cache complète des pages** — Efficacité de la mise en cache dynamique d’HTML storefront pour les pages de détails du produit (PDP), les pages de listes de produits (PLP) et les pages du système de gestion de contenu (CMS) à la périphérie.
- **Analyse des activités et des demandes des robots** — Le trafic est ventilé par agents de robots connus, géolocalisation, adresses IP/sous-réseaux, URL et signaux Fastly Next-Gen Web Application Firewall (WAF).

Un quatrième onglet in-app **Documentation** comporte des notes conceptuelles et le [playbook d’investigation](investigation-playbook.md).

## À qui s&#39;adresse ce guide ?

- **Opérateurs de site et Ingénierie de fiabilité du site (SRE)** enquête sur la surcharge de bande passante du réseau CDN, les pics de trafic ou la charge d’origine.
- **Développeurs** réglage de la couverture et du taux d’accès de Full Page Cache (FPC) ou implémentation de règles VCL (Fastly Varnish Configuration Language).
- **Administrateurs et ingénieurs en sécurité** identifier et atténuer les robots indésirables, les scrapers et le trafic automatisé malveillant.

Une bonne connaissance de [!DNL Adobe Commerce on Cloud Infrastructure], des concepts Fast CDN et de la navigation de base de New Relic est supposée.

## Fonctionnement

Sélectionnez un compte et une période dans les commandes de la plateforme en haut de la page. Un **ID de projet** facultatif permet d’affiner davantage les graphiques à des projets cloud spécifiques. Dans une configuration de compte principal ou de partenariat, la possibilité d’afficher un compte dans la liste déroulante ne signifie pas que vous pouvez l’interroger. Si un graphique signale une erreur d’autorisation, basculez vers un compte auquel vous avez accès en langage de requête New Relic (NRQL).

Vous continuez à appliquer des filtres pour transformer une vue d’ensemble large en une enquête ciblée. Cliquez sur une valeur dans une colonne de facettes, telle qu’un robot, une adresse IP, un sous-réseau, un pays ou un type de contenu pour ajouter un [filtre global](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use). Les filtres actifs s’affichent en haut de la grille et s’appliquent simultanément à chaque widget dans chaque onglet. Pour élargir la portée, supprimez un filtre.

**Présentation** - Supposons que la *Bande passante totale* ait une tendance supérieure à l’allocation contractuelle et que vous souhaitiez savoir qui la dirige :

1. Ouvrez l’onglet **Analyse des activités et des demandes des robots** et lisez **Structure de bande passante** pour voir quelle partie du trafic est automatisée ou organique.
1. Si les robots semblent avoir plus de trafic, ouvrez **Robots connus par bande passante** et cliquez sur le robot le plus lourd, par exemple un scraper. Cela ajoute un nouveau filtre, ce qui signifie que désormais chaque widget est défini sur ce robot.
1. Lisez **Known Bots Impact Details** pour connaître le taux de requêtes, le mélange de statuts et le taux d’accès FPC.
1. Pour savoir d’où provient le robot, cochez la case **Bande passante par pays**. Pour savoir ce que le robot récupère, consultez **URL par bande passante**.
1. Si le trafic se concentre sur un seul réseau, cliquez sur **Stats par sous-réseaux IP** pour confirmer la rotation d’un acteur entre les adresses dans un seul bloc.
1. Vous disposez désormais des informations nécessaires pour rédiger une atténuation ciblée, ainsi que pour savoir qui, quoi et où. Passez au [Guide d’investigation](investigation-playbook.md) pour savoir comment procéder.

La même méthode de filtrage fonctionne à partir de n’importe quelle facette de départ : un pays suspect, une adresse IP unique, un type de contenu ou un segment de chemin d’URL.

## Méthode de mesure des données

Comprendre quelques choix de mesure facilite la confiance et l’interprétation des chiffres.

- **Bande passante (BW)** est le nombre total d’octets servis par le réseau CDN pour les requêtes correspondantes, en comptant **à la fois les en-têtes de réponse et le corps**. C&#39;est la mesure du coût global qui est prise en compte dans l&#39;allocation au contrat.
- **Demandes (Req.)** correspond au nombre de requêtes distinctes, cependant, avec Fastly [blindage](https://www.fastly.com/documentation/guides/concepts/shielding/) activé, une seule requête est consignée **deux fois**, une fois sur chacune des opérations suivantes :
  - Écran interne [Point de présence (POP)](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)
  - EDGE POP
    Cela se produit à moins que la réponse ne vienne directement du cache POP local ou que le bouclier agisse lui-même comme POP pour la position de l’expéditeur. Pour éviter de compter deux fois ces cas `HIT,MISS` et `MISS,MISS`, les requêtes de l’application s’agrègent avec des [`uniqueCount`](https://docs.newrelic.com/docs/nrql/nrql-syntax-clauses-functions/#func-uniqueCount) sur le champ `request_id` . Cela renvoie une approximation **approximation** proche avec une marge d&#39;erreur attendue de **~5%**, et non un comptage exact.
- Les **segments réseau CDN** sont compressés différemment. La réponse diffusée au client est compressée, mais le trafic de protection vers POP n’est [pas compressé](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge) afin de préserver la prise en charge des [inclusions côté Edge (ESI)](https://www.fastly.com/documentation/reference/vcl/statements/esi/). Un faible taux d’accès au cache gonfle donc davantage le segment interne que le segment orienté client, car le contenu non mis en cache doit être extrait sur le bouclier à plusieurs reprises à une taille complète et non compressée. Cette compression est la raison pour laquelle le widget **Bande passante du segment du réseau CDN** et le taux d’accès FPC sont deux vues du même coût sous-jacent.

## Limites et performances des données

- **rétention de 30 jours** - Les journaux du réseau CDN Fastly sont conservés dans New Relic pendant **30 jours** selon la formule d’abonnement. Toute fenêtre que vous sélectionnez doit être comprise dans les 30 derniers jours. Pour une bande passante **totale** à plus long terme, utilisez l’intégration directe Fastly dans le panneau de [!DNL Adobe Commerce admin], **Tableau de bord > Fastly > Bande passante > Total**, mais considérez qu’elle renvoie par ID de service ; les données doivent donc être collectées par environnement et agrégées pour être comparées à l’indemnité contractuelle.
- **limite de requête de 60 secondes** - Le NRQL de chaque graphique dispose d’une limite d’exécution de [&#x200B; 60 secondes](https://docs.newrelic.com/docs/nrql/using-nrql/rate-limits-nrql-queries/#query-duration). Pour les comptes à trafic très élevé, un widget peut expirer lors de l’analyse d’un trop grand nombre d’enregistrements de journal. Si cela se produit, réduisez la période et rechargez les graphiques. Vous pouvez le développer à nouveau pour des onglets plus clairs.
