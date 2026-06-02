---
title: Pile technologique
description: Découvrez la pile technologique qui constitue Commerce sur l’infrastructure cloud.
feature: Cloud, Iaas, Paas
exl-id: 3fac1ab7-6440-4bf9-8169-9fadf51d70dd
TQID: https://experienceleague.adobe.com/2-uZdx1Oi-3LQUK-L7rC4kZWYcYobEhnVcUDNwOHpFs
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: b5f00040-57a0-4a6d-a39e-383b1936c2c9id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: c1256247-af4b-46d8-9dca-0c654ecfa157id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: df5e974b-6742-4873-a687-a6bedaafdaa2id: f2261633-201d-46c5-8a66-999e70527a83
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: efe0f95c16d9aab9af6a9c263034b08dce4d7c93
workflow-type: tm+mt
source-wordcount: 405
ht-degree: 0%

---

# Pile technologique

Considérez Adobe Commerce sur les infrastructures cloud comme cinq couches fonctionnelles, comme illustré ci-dessous :

![Pile cloud](../../assets/CloudStack.png)

1. [**Infrastructure cloud**](pro-architecture.md) : choisissez Amazon Web Services (AWS) ou Microsoft Azure comme base d’infrastructure en tant que service (IaaS) pour vos projets Pro d’infrastructure cloud Adobe Commerce.

   Adobe analyse régulièrement l’utilisation de votre processeur virtuel (vCPU) et alloue automatiquement les ressources pour optimiser l’utilisation à long terme et atténuer le risque de dépasser votre quota annuel maximal de jours alloués aux processeurs virtuels. Si vous prévoyez une augmentation du trafic sur le site pour des périodes spécifiques, vous devez continuer à ouvrir un ticket d’assistance pour [demander une mise à niveau temporaire](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html).

1. [**Platform as a Service**](cloud-architecture.md) : chaque projet d’infrastructure cloud d’Adobe Commerce fournit un environnement d’intégration PaaS (Platform as a Service) pour le développement, le test et l’intégration de services.
1. [**Adobe Commerce**](../project/overview.md) : Adobe Commerce sur l’infrastructure cloud fournit une infrastructure préconfigurée qui inclut PHP, MySQL (MariaDB), Redis, les services de file d’attente des messages ([!DNL RabbitMQ] ou [!DNL ActiveMQ]) et les technologies de moteurs de recherche prises en charge.
1. [**Outils de performance**](../monitor/new-relic-service.md) : les outils de performance de New Relic vous permettent de déboguer, de surveiller et de gérer vos applications et votre infrastructure en collectant, en analysant et en affichant les données de votre Adobe Commerce sur les projets d’infrastructure cloud.
1. [**réseau de diffusion de contenu (CDN), pare-feu d’application web ([!DNL WAF]) et optimisation des images (IO)**](../cdn/fastly.md) :

   * [Fast CDN](../cdn/fastly.md#ddos-protection) : fournit des services CDN sécurisés avec une protection intégrée contre les attaques DDoS (Distributed Denial of Service) telles que les attaques [!DNL Ping of Death], les attaques [!DNL Smurf] et d&#39;autres attaques par inondation basées sur le protocole ICMP (Internet Control Message Protocol).
   * [Pare-feu d’application web (WAF)](../cdn/fastly-waf-service.md) les services WAF assurent la conformité PCI des storefronts Adobe Commerce dans les environnements de production et les politiques WAF qui protègent vos applications web Adobe Commerce des attaques par injection, des entrées malveillantes, du cross-site scripting, de l’exfiltration de données, des violations de protocole HTTP et d’autres [[!DNL OWASP] dix principales menaces de sécurité](https://owasp.org/www-project-top-ten/).
   * [Optimisation des images (IO)](../cdn/fastly-image-optimization.md) : permet de manipuler et d’optimiser les images en temps réel afin d’accélérer la diffusion des images et de simplifier la maintenance des jeux de sources d’images pour les applications web réactives. Fastly IO décharge le traitement et le redimensionnement des images, libérant ainsi les serveurs pour traiter les commandes et les conversions de manière efficace.

Les applications monolithiques sont gourmandes en ressources et difficiles à mettre à l’échelle et à diffuser rapidement. Grâce à l’infrastructure cloud, les clients Commerce bénéficient d’un accès inégalé aux microservices SaaS, riches, intelligents et performants. Voir [Logiciels et services pris en charge](cloud-architecture.md#supported-software-and-services).

Utilisez le guide de prise en main de [](../../get-started/overview.md) pour configurer votre nouveau programme cloud et commencer à gérer votre application [!DNL Commerce] dans un environnement conçu pour le cloud.
