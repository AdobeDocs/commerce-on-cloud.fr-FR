---
title: Adobe Commerce Traffic Insights
description: Découvrez l’outil de statistiques de trafic d’Adobe Commerce et comment il peut vous aider à comprendre le trafic sur votre projet d’infrastructure cloud Adobe Commerce.
feature: Cloud, Observability
role: Admin
source-git-commit: 119c9415abd22221e3ae785445d537f0609eba14
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# Insights de trafic

Adobe Commerce Traffic Insights est une application New Relic One qui permet de visualiser [!DNL Adobe Commerce on Cloud Infrastructure] trafic Fast CDN. Il lit les lignes du journal d’accès Fast CDN déjà envoyées dans New Relic en tant qu’événements `Log` et effectue le rendu d’un ensemble organisé de graphiques, dont la portée correspond à un compte New Relic que vous sélectionnez et la période de la plateforme. Vous visualisez ainsi le trafic Edge d’un magasin sans avoir à écrire manuellement NRQL, le langage de requête de New Relic.

## Qu’est-ce qui vous aide à enquêter ?

Traffic Insights est conçu pour vous aider à résoudre trois problèmes courants :

- **Dépassement de bande passante du réseau CDN** — Tendance du trafic au-dessus de la tolérance contractuelle. Attribuez le volume à des médias volumineux, des fichiers volumineux, des pages 404 non mises en cache ou un cache inefficace, jusqu’à un domaine, un type de contenu, une URL ou un projet spécifique.
- **Robot de recherche et charge de robot d&#39;exploration** — robot d&#39;exploration de recherche ou d’IA générant une part disproportionnée de requêtes, ce qui nuit à l’efficacité du cache et à la charge d’origine. Identifiez les robots nommés les plus actifs et ce qu’ils récupèrent exactement.
- **Scripts et scrapers malveillants** — Grattage, bourrage d&#39;informations d&#39;identification, test de cartes, création de faux comptes ou abus de couche 7. Affichez les signaux Fastly Next-Gen WAF et les adresses IP, les sous-réseaux et les pays derrière le trafic suspect.

Dans chaque cas, l’application identifie le *qui, quoi et où* du trafic. Agir sur ces informations par le biais des règles VCL Fastly, de l’optimisation des images, de l’optimisation du cache, de la limitation de débit ou du module complémentaire Adobe [Sécurité avancée](../../cdn/advanced-security.md) dans votre configuration Commerce et Fastly. Le [&#x200B; guide des enquêtes](investigation-playbook.md) couvre chacun de ces problèmes.

## Accès à l’application

- **Lien direct :** [Adobe Commerce Traffic Insights](https://one.newrelic.com/a9a0c3b8-3844-4ca1-8bad-c6742747be47).
- **À partir de l’écran d’accueil de New Relic One** (one.newrelic.com) — une fois le compte abonné à l’application, il apparaît comme sa propre mosaïque, **Informations de trafic Adobe Commerce** sur la page d’accueil.
- **Dans la barre de recherche supérieure (Recherche rapide)** — recherchez des `Adobe Commerce Traffic Insights` et sélectionnez-les dans les résultats.
- **Pour l’épingler pour un accès plus rapide** - utilisez le contrôle étoile ou épingler sur la mosaïque ou l’en-tête de page de l’application pour l’ajouter aux favoris ou au volet de navigation de gauche. L’emplacement exact de ce contrôle dépend de la version de l’interface utilisateur de New Relic utilisée pour le compte.

## Dans ce guide

- **[Présentation de l’application](understanding-the-app.md)** - Qu’est-ce que les informations de trafic, comment les piloter avec des filtres, comment les nombres sont mesurés et ce que les données peuvent ou ne peuvent pas vous dire.
- **[Guide d’investigation](investigation-playbook.md)** - Approches recommandées pour résoudre les trois problèmes que l’application est conçue pour résoudre : le dépassement de bande passante, la charge de robot d&#39;exploration et le trafic malveillant. Chacune de ces options fait référence au graphique qui le confirme et spécifie le chemin d’escalade natif d’Adobe [Sécurité avancée](../../cdn/advanced-security.md) pour les cas où la réduction manuelle est insuffisante.