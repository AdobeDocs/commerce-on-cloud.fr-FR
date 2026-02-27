---
title: Liste de contrôle de Launch
description: Consultez les éléments de la liste de contrôle pour le lancement du site.
exl-id: efc97d4a-a9f3-49fa-b977-061282765e90
source-git-commit: ca2d94364787695398b2b8af559733fe52ec2949
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 0%

---

# Liste de contrôle de Launch

Avant de procéder au déploiement dans l’environnement de production, téléchargez la liste de contrôle [Launch](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf) et utilisez-la avec ces instructions pour confirmer que vous avez terminé toutes les configurations et tous les tests requis. Consultez une présentation du processus de déploiement complet pour Starter et Pro sur [Déployer votre boutique](../deploy/staging-production.md).

## Test complet en production

Voir [Tester le déploiement](../test/staging-and-production.md) pour tester tous les aspects de vos sites, magasins et environnements. Ces tests comprennent la vérification de Fastly, les tests d’acceptation utilisateur (UAT) et les tests de performances.

## TLS et Fastly

Adobe fournit un certificat Let’s Encrypt SSL/TLS pour chaque environnement. Ce certificat est requis pour que Fastly diffuse du trafic sécurisé via HTTPS.

Pour utiliser ce certificat, vous devez mettre à jour votre configuration DNS afin qu’Adobe puisse terminer la validation du domaine et appliquer le certificat à votre environnement. Chaque environnement dispose d’un certificat unique qui couvre les domaines d’Adobe Commerce sur les sites d’infrastructure cloud déployés dans cet environnement. Nous vous recommandons d’effectuer et de mettre à jour la configuration pendant le processus de [configuration rapide](../cdn/fastly-configuration.md).

## Mise à jour de la configuration DNS avec les paramètres de production

Lorsque vous êtes prêt à lancer votre site, vous devez mettre à jour la configuration DNS pour acheminer le trafic depuis votre environnement de production via le service Fastly.

**Conditions préalables :**

- [Configuration et test rapides dans votre environnement de développement](../cdn/fastly-configuration.md#)

- La configuration de l’environnement de production a été mise à jour avec tous les domaines requis.

  En règle générale, vous travaillez avec votre conseiller technique client pour ajouter tous les domaines et sous-domaines de niveau supérieur requis pour vos magasins. Pour ajouter ou modifier les domaines de votre environnement de production, [Envoyez un ticket d’assistance Adobe Commerce](https://support.magento.com/hc/en-us/articles/360019088251). Attendez la confirmation que la configuration de votre projet a été mise à jour.

  Dans les projets de démarrage, vous devez ajouter les domaines à votre projet. Voir [&#x200B; Gestion des domaines &#x200B;](../cdn/fastly-custom-cache-configuration.md#manage-domains).

- Certificat SSL/TLS configuré pour vos environnements de production.

  Si vous avez ajouté les enregistrements de défi ACME pour vos domaines de production au cours du processus de configuration Fastly, Adobe charge automatiquement le certificat SSL/TLS dans votre environnement de production lorsque vous mettez à jour la configuration DNS pour acheminer le trafic vers le service Fastly. Si vous n’avez pas préconfiguré le certificat ou si vous avez mis à jour vos domaines, Adobe doit effectuer la validation du domaine et configurer le certificat, ce qui peut prendre jusqu’à 12 heures.

### Pour mettre à jour la configuration DNS pour le lancement du site :

1. Mettez à jour la configuration DNS suivante pour votre site de production :

   - Définissez toutes les redirections nécessaires, en particulier si vous migrez à partir d’un site existant

   - Définissez l’enregistrement de ressource racine de la zone pour le nom d’hôte

   - Réduisez la valeur de la durée de vie (TTL) pour actualiser les informations DNS afin de diriger les clients vers le magasin de production approprié.

     Nous vous recommandons de réduire considérablement la valeur de TTL lors du changement d’enregistrement DNS. Cette valeur indique au DNS combien de temps mettre en cache l’enregistrement DNS. Lorsqu’il est raccourci, il actualise le DNS plus rapidement. Par exemple, vous pouvez modifier la valeur de durée de vie de trois jours à 10 minutes lorsque vous mettez à jour votre site. Notez que le raccourcissement de la valeur de TTL ajoute de la charge à l’infrastructure DNS. Restaurez la valeur précédente, plus élevée, après le lancement du site.


1. Ajoutez des enregistrements CNAME pour pointer les sous-domaines de votre environnement de production vers le `prod.magentocloud.map.fastly.net` de service Fastly, par exemple :

   | Domaine ou sous-domaine | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. Si nécessaire, ajoutez des enregistrements A et AAAA pour mapper le domaine apex (`<domain-name>.com`) aux adresses IP Fastly suivantes :

   | Domaine apex | NOM | AAANAME |
   | --------------- | ----------------- | -------- |
   | `<domain-name>.com` | `151.101.1.124` | 2a04:4e42:200::380 |
   | `<domain-name>.com` | `151.101.65.124` | 2a04:4e42:400::380 |
   | `<domain-name>.com` | `151.101.129.124` | 2a04:4e42:600::380 |
   | `<domain-name>.com` | `151.101.193.124` | 2a04:4e42::380 |

>[!IMPORTANT]
>
>Les instructions DNS de [RFC1034](https://www.rfc-editor.org/rfc/rfc1912) (**section 2.4**) indiquent que :
>_Un enregistrement CNAME n’est pas autorisé à coexister avec d’autres données. En d’autres termes, si suzy.podunk.xx est un alias de sue.podunk.xx, vous ne pouvez pas non plus avoir d’enregistrement MX pour suzy.podunk.edu, ou un enregistrement A, ou même un enregistrement TXT._
>
>Pour cette raison, les enregistrements DNS doivent être de type `CNAME` pour les sous-domaines et de type `A` pour les domaines apex (domaines racines). L’abandon de cette règle peut entraîner des perturbations de votre service de messagerie ou de la propagation DNS, car vous perdez la possibilité d’ajouter d’autres enregistrements, tels que MX ou NS. Certains fournisseurs DNS peuvent contourner ce problème en utilisant des personnalisations internes, mais le respect de la norme assure la stabilité et la flexibilité (comme le changement de fournisseur DNS).

1. Mettez à jour l’URL de base.

   - Utilisez SSH pour vous connecter à l’environnement de production.

     ```bash
     magento-cloud ssh -e production
     ```

   - Utilisez l’interface de ligne de commande pour modifier l’URL de base de votre boutique.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **REMARQUE** : vous pouvez également mettre à jour l’URL de base à partir de l’Administration. Voir [URL des magasins](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html) dans le _Guide des magasins et de l’expérience d’achat Adobe Commerce_.

1. Patientez quelques minutes pour que le site se mette à jour.

1. Testez votre site.

## Vérification de la configuration de production

Effectuez une dernière passe pour valider la configuration de production pour un ou plusieurs magasins. Vous pouvez mettre à jour la configuration dans l’environnement de production. Si les paramètres sont en lecture seule, vous devrez peut-être ouvrir une connexion SSH et utiliser les commandes de l’interface de ligne de commande pour modifier la configuration ou apporter des modifications à la configuration de votre environnement local. Une fois les mises à jour terminées, vous pouvez déployer les modifications dans les environnements d’évaluation et de production.

Les modifications et vérifications recommandées sont les suivantes :

- [Test des e-mails sortants terminé](../project/outgoing-emails.md)

- [Configuration sécurisée des informations d’identification des administrateurs et de l’URL de l’administrateur de base](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [Optimiser toutes les images pour le web](../cdn/fastly-image-optimization.md)

- [Vérifier les paramètres de minimisation pour HTML, JavaScript et CSS](../deploy/static-content.md)

## Vérifier la mise en cache rapide

- Testez et vérifiez que la mise en cache Fastly fonctionne correctement sur le site de production. Pour obtenir des tests et des vérifications détaillés, voir [Tests rapides](../test/staging-and-production.md#check-fastly-caching).

- [Assurez-vous que la dernière version du module Fastly CDN pour Commerce est installée dans votre environnement de production](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Assurez-vous que la version la plus récente du code VCL Fastly a été chargée](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## Tests de performance

Nous vous recommandons d’examiner les options [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) dans le cadre de votre processus de préparation préalable au lancement.

Vous pouvez également effectuer des tests à l’aide des options tierces suivantes :

- [Siege](https://www.joedog.org/siege-home/) : logiciel de test et de mise en forme du trafic pour pousser votre boutique à la limite. Accédez à votre site avec un nombre configurable de clients simulés. Siege prend en charge l’authentification de base, les cookies, les protocoles HTTP, HTTPS et FTP.

- [Jmeter](https://jmeter.apache.org/) : Excellent test de charge pour aider à évaluer les performances pour le trafic en pic, comme pour les ventes flash. Créez des tests personnalisés à exécuter sur votre site.

- [New Relic](https://support.newrelic.com/s/) (fourni) : permet de localiser les processus et les zones du site, ce qui ralentit les performances avec le temps passé par action suivi, comme la transmission de données, de requêtes, de Redis, etc.

- [WebPageTest](https://www.webpagetest.org/) et [Pingdom](https://www.pingdom.com/) : analyse en temps réel du temps de chargement des pages de votre site avec différents emplacements d’origine. Pingdom peut coûter un supplément. WebPageTest est un outil gratuit.

## Configuration de la sécurité

- [Configuration de l’analyse de sécurité](overview.md#set-up-the-security-scan-tool)

- [Configuration sécurisée pour l’utilisateur administrateur](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [Configuration sécurisée de l’URL d’administration](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [Supprimez tous les utilisateurs qui ne participent plus au projet d’infrastructure cloud d’Adobe Commerce](../project/user-access.md)

- [Configurer l’authentification à deux facteurs](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## Suivi des performances

Vous pouvez utiliser les services New Relic pour la surveillance des performances dans les environnements Pro et Starter. Sur les comptes Pro plan, nous fournissons la stratégie d’alerte Managed alerts pour Adobe Commerce afin de surveiller les performances des applications et des infrastructures à l’aide des agents New Relic APM et Infrastructure. Pour plus d’informations sur l’utilisation de ces services, voir [Surveillance des performances avec des alertes gérées](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

### Étape suivante

[Étapes de lancement](steps.md)
