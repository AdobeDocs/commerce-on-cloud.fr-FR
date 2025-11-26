---
title: Tests d’évaluation et de production
description: Découvrez comment effectuer des tests dans les environnements d’évaluation et de production.
exl-id: 39625c97-5eb0-4039-ac5f-ddaeb43156de
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Tests d’évaluation et de production

Une fois la migration de votre code, de vos fichiers et de vos données vers les environnements d’évaluation ou de production terminée, utilisez les URL d’environnement pour tester vos sites et magasins. Vous trouverez ci-dessous des informations sur la vérification des journaux, le test des configurations Fastly, les tests d’acceptation utilisateur (UAT), etc.

{{second-staging}}

## Fichiers journaux

Si vous rencontrez des erreurs lors du déploiement ou d’autres problèmes lors du test, vérifiez les fichiers journaux. Les fichiers journaux se trouvent dans le répertoire `var/log`.

Le journal de déploiement est en `/var/log/platform/<prodject-ID>/deploy.log`. La valeur de `<project-ID>` dépend de l’identifiant du projet et du statut de l’environnement : Évaluation ou Production. Par exemple, avec un ID de projet de `yw1unoukjcawe`, l’utilisateur intermédiaire est `yw1unoukjcawe_stg` et l’utilisateur de production est `yw1unoukjcawe`.

Lors de l’accès aux journaux dans les environnements de production ou d’évaluation, utilisez SSH pour vous connecter à chacun des trois nœuds afin de localiser les journaux. Vous pouvez également utiliser la gestion des journaux de [New Relic](../monitor/log-management.md) pour afficher et interroger les données de journaux agrégées de tous les nœuds. Voir [ Afficher les journaux ](log-locations.md#application-logs).

## Vérifier la base de code

Vérifiez que votre base de code est correctement déployée dans les environnements d’évaluation et de production. Les environnements doivent avoir des bases de code identiques.

## Vérifier les paramètres de configuration

Vérifiez les paramètres de configuration via le panneau d’administration, y compris l’URL de base, l’URL d’administration de base, les paramètres multisite, etc. Si vous devez apporter des modifications supplémentaires, effectuez les modifications dans votre branche Git locale et envoyez-les à la branche `master` dans les environnements d’intégration, d’évaluation et de production.

## Vérifier la mise en cache rapide

[La configuration de Fastly](../cdn/fastly-configuration.md) nécessite une attention particulière aux détails : l’utilisation de l’identifiant de service Fastly et des informations d’identification du jeton API Fastly corrects, le chargement du code VCL Fastly, la mise à jour de la configuration DNS et l’application des certificats SSL/TLS à vos environnements. Après avoir effectué ces tâches de configuration, vous pouvez vérifier la mise en cache rapide dans les environnements d’évaluation et de production.

**Pour vérifier la configuration du service Fastly** :

1. Connectez-vous à l’administration pour l’évaluation et la production à l’aide de l’URL avec `/admin` ou de l’[URL d’administration mise à jour](../environment/variables-admin.md#admin-url).

1. Accédez à **Magasins** > **Paramètres** > **Configuration** > **Avancé** > **Système**. Faites défiler l’écran et cliquez sur **Cache de page complet**.

1. Assurez-vous que la valeur **Application de mise en cache** est définie sur _Fast CDN_ .

1. Testez les informations d’identification Fastly .

   - Cliquez sur **Configuration rapide**.

   - Vérifiez les valeurs de l’ID de service Fastly et des informations d’identification du jeton API Fastly . Voir [Obtention des informations d’identification Fastly](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials).

   - Cliquez sur **Tester les informations d’identification**.

   >[!WARNING]
   >
   >Assurez-vous d’avoir saisi l’identifiant de service Fastly et le jeton API corrects dans vos environnements d’évaluation et de production. Les informations d’identification Fastly sont créées et mappées par environnement de service. Si vous saisissez les informations d’identification d’évaluation dans votre environnement de production, vous ne pouvez pas charger vos fragments de code VCL. La mise en cache ne fonctionne pas correctement et votre configuration de mise en cache pointe vers le mauvais serveur et magasins.

**Pour vérifier le comportement de mise en cache rapide** :

1. Recherchez des en-têtes à l’aide de l’utilitaire de ligne de commande `dig` pour obtenir des informations sur la configuration du site.

   Vous pouvez utiliser n’importe quelle URL avec la commande `dig`. Les exemples suivants utilisent des URL Pro :

   - Évaluation : `dig https://mcstaging.<your-domain>.com`
   - Production : `dig https://mcprod.<your-domain>.com`

   Pour des tests de `dig` supplémentaires, voir Tests de Fastly [ avant de modifier le DNS](https://docs.fastly.com/en/guides/working-with-domains).

1. Utilisez `cURL` pour vérifier les informations d’en-tête de réponse.

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   Voir [Vérifier les en-têtes de réponse](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) pour plus d’informations sur la vérification des en-têtes.

1. Une fois que vous êtes en ligne, utilisez `cURL` pour vérifier votre site actif.

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## Test UAT complet

Effectuez le test d’acceptation utilisateur (UAT) lors de l’évaluation et de la production. Les tests suivants constituent une liste rapide des tâches et zones possibles à tester en tant que commerçant et client. Votre liste peut être plus longue et inclure des tests supplémentaires pour les modules personnalisés, les extensions et les intégrations tierces. Lors du test, utilisez des ordinateurs de bureau, des ordinateurs portables et des appareils mobiles.

Si vous rencontrez des problèmes, enregistrez les étapes de reproduction, les messages d’erreur, les captures d’écran étranges et les liens. Utilisez ces informations pour étudier et résoudre les problèmes liés au code et aux configurations de l’environnement d’intégration ou aux paramètres d’environnement.

<table>
<tr>
<td style="width:150px">Gestion des utilisateurs et utilisatrices</td>
<td>
<ul>
<li>Créer et modifier des comptes clients, vérifier les e-mails</li>
<li>Créer des rôles d'administrateur pour les commerçants</li>
<li>Créer des comptes marchands avec des rôles spécifiques</li>
<li>Tester l'accès au compte marchand par rôle</li>
</ul>
</td>
</tr>
<tr>
<td>Catalogues et produits</td>
<td>
<ul>
<li>Création d’un catalogue avec des produits associés</li>
<li>Créer des produits pour votre storefront, y compris tous les types de produits : simples, configurables, groupés</li>
<li>Ajout d’images, d’échantillons, de vidéos et d’autres options multimédias au produit</li>
<li>Configurer le prix, les remises et les règles de tarification </li>
<li>Configurer des fonctionnalités avancées, notamment les plages de prix, les produits présentés et les dates de disponibilité</li>
<li>Modifier le stock et vérifier que les valeurs correctes s'affichent et changent par augmentation et achat terminé</li>
</ul>
</td>
</tr>
<tr>
<td>Paniers et passage en caisse</td>
<td>
<ul>
<li>Rechercher des produits et sélectionner des options de filtrage</li>
<li>Ajouter des produits au panier à partir des résultats de recherche, des pages de catégories et des pages de produits</li>
<li>Tester tous les types de produits</li>
<li>Afficher le panier et modifier le contenu en supprimant ou en modifiant des montants </li>
<li>Passez en caisse pour vérifier les montants de commande par rapport au panier et aux informations sur le produit.</li>
<li>Vérifier que la taxe est correctement calculée pour le panier</li>
<li>Effectuez un achat avec différentes options : ajoutez un coupon, sélectionnez l’expédition, saisissez les informations d’expédition et de facturation, ainsi que les informations de paiement</li>
<li>Vérifier les passerelles et options de paiement lors du passage en caisse</li>
<li>Recherchez les notifications à l’écran, les commandes répertoriées dans le compte client et les notifications par e-mail</li>
<li>Test du passage en caisse des invités et des clients</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>Créer une commande pour un client</li>
<li>Rechercher et afficher les commandes</li>
<li>Modifier une commande en ajoutant et en supprimant des produits, en modifiant les montants, en modifiant les informations d’expédition et de facturation</li>
<li>Gérer un remboursement</li>
<li>Annuler une commande</li>
<li>Appliquer des codes coupon et des remises</li>
</ul>
</td>
</tr>
<tr>
<td>Contenu du site</td>
<td>
<ul>
<li>Vérifier que tous les thèmes et ressources se chargent correctement</li>
<li>Vérifier que les affichages CSS sont corrects, y compris les tailles de médias réactifs</li>
<li>Consultez les conditions générales, la politique de remboursement et d’autres informations sur la politique</li>
<li>Consultez les coordonnées et les liens de votre entreprise pour en savoir plus</li>
<li>Rechercher des produits et du contenu, vérifier le filtrage des résultats</li>
<li>Vérifiez le bloc de pied de page et les blocs de navigation supérieurs.</li>
<li>Tester les pages 404 et Maintenance</li>
</ul>
</td>
</tr>
<tr>
<td>Extensions</td>
<td>
<ul>
<li>Vérifiez tous les paramètres d'extension, en particulier pour les modules de taxation, d'expédition et de paiement (par exemple : commande envoyée à l'entrepôt et système de gestion financière)</li>
<li>Tester toutes les interactions de module personnalisées et d’extension installées</li>
<li>Vérifiez les données pour toutes les interactions qui doivent se terminer (paiements, commandes, notifications par e-mail).</li>
<li>Vérifier les configurations par environnement pour vos extensions</li>
<li>Vérification des dépendances entre les modules et le fonctionnement des extensions</li>
<li>Vérifier toutes les actions en tant que commerçant et client</li>
</ul>
</td>
</tr>
<tr>
<td>Intégrations tierces</td>
<td>
<ul>
<li>Vérifiez que les données sont correctement enregistrées dans Adobe Commerce et exportées, transmises ou accessibles par le service tiers (exemple : les commandes s’affichent dans le système de gestion des commandes tiers)</li>
<li>Vérifier les configurations et interactions par intégration</li>
<li>Effectuer des tests aller-retour depuis Adobe Commerce et votre service tiers</li>
<li>Vérifier que l’authentification est terminée</li>
<li>Vérifiez les problèmes consignés pour mettre à jour les intégrations de code ou les messages d’erreur dans les panneaux de contrôle</li>
</ul>
</td>
</tr>
<tr>
<td>Test du serveur principal</td>
<td>
<ul>
<li>Tester et effacer le cache </li>
<li>Réindexation et vérification des résultats</li>
<li>Vérifiez les tâches cron et les erreurs cron_schedule.</li>
<li>Vérifier et vérifier les problèmes éventuels de script shell</li>
<li>Vérifiez les problèmes consignés : journaux d’application, journaux PHP, journaux MySQL, journaux d’e-mail.</li>
</ul>
</td>
</tr>
</table>

## Tests de charge et de contrainte

Avant le lancement, il est préférable d’effectuer des tests approfondis de trafic et de performances sur vos environnements d’évaluation et de production. Envisagez des tests de performance pour vos processus front-end et back-end.

Avant de commencer les tests, saisissez un ticket auprès de l’assistance pour informer les environnements que vous testez, les outils que vous utilisez et la période. Mettez à jour le ticket avec les résultats et les informations pour suivre les performances. Une fois le test terminé, ajoutez les résultats mis à jour et notez que le test du ticket est terminé avec un horodatage et une date.

Examinez les options [Boîte à outils de performance](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) dans le cadre de votre processus de préparation avant le lancement.

Pour de meilleurs résultats, utilisez les outils suivants :

- [Test de performance de l’application](../environment/variables-post-deploy.md#ttfb_tested_pages) : testez les performances de l’application en configurant la variable d’environnement `TTFB_TESTED_PAGES` pour tester le temps de réponse du site.
- [Siege](https://www.joedog.org/siege-home/) : logiciel de test et de mise en forme du trafic pour pousser votre magasin à la limite. Accédez à votre site avec un nombre configurable de clients simulés. Siege prend en charge l’authentification de base, les cookies, les protocoles HTTP, HTTPS et FTP.
- [Jmeter](https://jmeter.apache.org)—Excellent test de charge pour aider à évaluer les performances pour le trafic en pic, comme pour les ventes flash. Créez des tests personnalisés à exécuter sur votre site.
- [New Relic](../monitor/new-relic-service.md) (fourni) : permet de localiser les processus et les zones du site, ce qui ralentit les performances grâce au suivi du temps passé par action, tel que la transmission de données, de requêtes, de Redis, etc.
- [WebPageTest](https://www.webpagetest.org) et [Pingdom](https://www.pingdom.com) : analyse en temps réel du temps de chargement des pages de votre site avec différents emplacements d’origine. Pingdom peut exiger des frais. WebPageTest est un outil gratuit.

## Tests fonctionnels

Vous pouvez utiliser la structure de tests fonctionnels (MFTF) Magento pour effectuer des tests fonctionnels pour Adobe Commerce à partir de l’environnement Cloud Docker. Voir [Test d’application](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing) dans le guide _Cloud Docker for Commerce_.

## Configuration de l’outil d’analyse de sécurité

Il existe un outil gratuit d&#39;analyse de sécurité pour vos sites. Pour ajouter vos sites et exécuter l’outil, voir [Outil d’analyse de sécurité](../launch/overview.md#set-up-the-security-scan-tool).
