---
title: Pile technologique
description: Découvrez la pile technologique qui constitue Commerce sur l’infrastructure cloud.
feature: Cloud, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Pile technologique

Considérez Adobe Commerce sur les infrastructures cloud comme cinq couches fonctionnelles, comme illustré ci-dessous :

![Pile cloud](../../assets/CloudStack.svg)

1. [**Infrastructure cloud**](pro-architecture.md) : choisissez Amazon Web Services (AWS) ou Microsoft Azure comme base d’infrastructure as a Service (IaaS) pour vos projets Pro d’infrastructure cloud Adobe Commerce.

   L’Adobe analyse régulièrement l’utilisation de votre processeur virtuel et alloue automatiquement les ressources afin d’optimiser l’utilisation à long terme et de réduire le risque de dépasser votre quota annuel maximal de jours alloués au processeur virtuel. Si vous prévoyez une augmentation du trafic sur le site pour des périodes spécifiques, vous devez continuer à ouvrir un ticket d’assistance pour [demander une mise à niveau temporaire](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=fr).

1. [**Platform as a Service**](cloud-architecture.md) : chaque projet d’infrastructure cloud d’Adobe Commerce fournit un environnement d’intégration PaaS (Platform as a Service) pour le développement, le test et l’intégration de services.
1. [**Adobe Commerce**](../project/overview.md) : Adobe Commerce sur l’infrastructure cloud fournit une infrastructure préconfigurée qui inclut PHP, MySQL (MariaDB), Redis, [!DNL RabbitMQ] et les technologies de moteurs de recherche prises en charge.
1. [**Outils de performance**](../monitor/new-relic-service.md) : les outils de performance de New Relic vous permettent de déboguer, de surveiller et de gérer vos applications et votre infrastructure en collectant, en analysant et en affichant les données de votre Adobe Commerce sur les projets d’infrastructure cloud.
1. [**réseau de diffusion de contenu (CDN), pare-feu d’application web ([!DNL WAF]) et optimisation des images (IO)**](../cdn/fastly.md) :

   * [Fast CDN](../cdn/fastly.md#ddos-protection) : fournit des services CDN sécurisés avec une protection intégrée contre les attaques DDoS (Distributed Denial of Service) telles que les attaques [!DNL Ping of Death], les attaques [!DNL Smurf] et d&#39;autres attaques par inondation basées sur le protocole ICMP (Internet Control Message Protocol).
   * [Pare-feu d’application web (WAF)](../cdn/fastly-waf-service.md) les services WAF assurent la conformité PCI des storefronts Adobe Commerce dans les environnements de production et les politiques WAF qui protègent vos applications web Adobe Commerce des attaques par injection, des entrées malveillantes, du cross-site scripting, de l’exfiltration de données, des violations de protocole HTTP et d’autres [[!DNL OWASP] dix principales menaces de sécurité](https://owasp.org/www-project-top-ten/).
   * [Optimisation des images (IO)](../cdn/fastly-image-optimization.md) : permet de manipuler et d’optimiser les images en temps réel afin d’accélérer la diffusion des images et de simplifier la maintenance des jeux de sources d’images pour les applications web réactives. Fastly IO décharge le traitement et le redimensionnement des images, libérant ainsi les serveurs pour traiter les commandes et les conversions de manière efficace.

Les applications monolithiques sont gourmandes en ressources et difficiles à mettre à l’échelle et à diffuser rapidement. Grâce à l’infrastructure cloud, les clients Commerce bénéficient d’un accès inégalé aux microservices SaaS, riches, intelligents et performants. Voir [Logiciels et services pris en charge](cloud-architecture.md#supported-software-and-services).

Utilisez le guide de prise en main de [Commerce](../../get-started/overview.md) pour configurer votre nouveau programme cloud et commencer à gérer votre application [!DNL Commerce] dans un environnement conçu pour le cloud.
