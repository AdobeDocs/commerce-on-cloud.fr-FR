---
title: Configuration des services Fastly
description: Découvrez comment configurer les services Fastly pour votre projet Adobe Commerce.
feature: Cloud, Configuration, Iaas, Cache, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1960'
ht-degree: 0%

---

# Configuration des services Fastly

Fastly est requis pour Adobe Commerce dans les environnements d’évaluation et de production d’infrastructure cloud.

Fastly fonctionne avec Varnish pour fournir des fonctionnalités de mise en cache rapide et un réseau de diffusion de contenu (CDN) pour les ressources statiques. Fastly fournit également un pare-feu d’application web (WAF) pour sécuriser votre site et votre infrastructure cloud. Pour protéger votre site et votre infrastructure cloud du trafic et des attaques malveillants, acheminez tout le trafic entrant sur le site via Fastly.

>[!NOTE]
>
>Fastly n’est pas disponible dans les environnements d’intégration.

Suivez les étapes suivantes pour activer, configurer et tester Fastly dès le début du processus de développement de votre site afin de permettre un accès sécurisé à votre site.

- Obtention des informations d’identification Fastly pour les environnements d’évaluation et de production
- Activer la mise en cache Fast CDN
- Chargement rapide de fragments de code VCL
- Mettez à jour la configuration DNS pour acheminer le trafic vers le service Fastly
- Tester la mise en cache Fastly

>[!NOTE]
>
>Après avoir activé et vérifié la configuration Fastly initiale, vous pouvez personnaliser la configuration. Par exemple, vous pouvez activer des options supplémentaires telles que l’optimisation des images, les modules Edge et le code VCL personnalisé. Voir [Personnalisation de la configuration du cache](fastly-custom-cache-configuration.md).

## Obtenir des informations d’identification Fastly

Pendant la mise en service du projet, Adobe ajoute votre projet au [compte de service Fastly](fastly.md#fastly-service-account-and-credentials) pour Adobe Commerce sur l’infrastructure cloud et crée les informations d’identification de compte Fastly pour les environnements d’évaluation et de production Starter `master` et Pro. Chaque environnement possède des informations d’identification uniques.

Vous avez besoin des informations d’identification Fastly pour configurer les services de réseau CDN Fastly à partir de l’administrateur et pour envoyer les requêtes d’API Fastly.

>[!NOTE]
>
>Avec Adobe Commerce sur les infrastructures cloud, vous ne pouvez pas accéder directement à l’administration Fastly. Utilisez l’Administration pour passer en revue et mettre à jour la configuration Fastly pour vos environnements. Si vous ne pouvez pas résoudre un problème à l’aide des fonctionnalités Fastly dans l’administration, envoyez un [ticket d’assistance pour Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

Utilisez les méthodes suivantes pour rechercher et enregistrer l’ID de service Fastly et le jeton API pour votre environnement :

**Pour afficher vos informations d’identification Fastly** :

La méthode d’affichage des informations d’identification est différente pour les projets Pro et Starter.

- Répertoire partagé monté sur IaaS : sur les projets Pro, utilisez SSH pour vous connecter à votre serveur et obtenir les informations d’identification Fastly à partir du fichier `/mnt/shared/fastly_tokens.txt`. Les environnements d’évaluation et de production disposent d’informations d’identification uniques. Vous devez obtenir les informations d’identification pour chaque environnement.

- Espace de travail local : à partir de la ligne de commande, utilisez l’interface de ligne de commande `magento-cloud` pour [répertorier et réviser](../environment/variables-cloud.md#viewing-environment-variables) les variables d’environnement Fastly.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console]—Vérifiez les variables d&#39;environnement suivantes dans la [configuration de l&#39;environnement](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>Si vous ne parvenez pas à trouver les informations d’identification Fastly pour les environnements d’évaluation ou de production, contactez votre conseiller technique client Adobe (CTA).

## Activer la mise en cache Fastly

Vous avez besoin des composants suivants pour activer et configurer les services Fastly :

- Dernière version du module [Fast CDN for Magento 2](fastly.md#fastly-cdn-module-for-magento-2) installé dans les environnements d’évaluation et de production. Voir [ Mise à niveau rapide ](#upgrade-the-fastly-module).

- [Informations d’identification Fastly](#get-fastly-credentials) pour Adobe Commerce sur les environnements d’évaluation et de production d’infrastructure cloud

**Pour activer la mise en cache Fast CDN dans les environnements d’évaluation et de production** :

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** et développez **Cache de page complet**.

   ![Développer pour sélectionner Fastly](../../assets/cdn/fastly-menu.png)

1. Dans la section _Application de mise en cache_, supprimez la sélection de **Utiliser la valeur du système**, puis sélectionnez **Fast CDN** dans la liste déroulante.

   ![Choisir rapidement](../../assets/cdn/fastly-enable-admin.png)

1. Développez **Configuration rapide** et [choisissez les options de mise en cache](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module).

1. Après avoir configuré les options de mise en cache, cliquez sur **Enregistrer la configuration** en haut de la page.

1. Effacez le cache en fonction de la notification.

1. Continuez à configurer Fastly en revenant à **Magasins** > **Paramètres** > **Configuration** > **Avancé** > **Système** > **Configuration Fastly**.

### Tester les informations d’identification Fastly

1. Dans Admin, accédez à **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** > **Configuration Fastly**.

1. Si nécessaire, ajoutez les valeurs **ID de service Fastly** et **Jeton API** pour votre environnement de projet.

   ![Administrateur des informations d’identification Fastly](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >Ne sélectionnez pas le lien pour créer le jeton API Fastly. Utilisez plutôt les informations d’identification Fastly [ (ID de service et jeton API) fournies par Adobe ](#get-fastly-credentials) fournies par Adobe.

1. Cliquez sur **Tester les informations d’identification**.

1. Si le test réussit, cliquez sur **Enregistrer la configuration**, puis effacez le cache.

   Si le test échoue, vérifiez que l’identifiant de service et les valeurs du jeton API corrects correspondent aux informations d’identification de l’environnement actuel.

   Si le test échoue à nouveau, envoyez un ticket d’assistance Adobe Commerce ou contactez votre représentant de compte d’Adobe. Pour les projets Pro, incluez les URL de vos sites de production et d’évaluation. Pour les projets de démarrage, incluez les URL de votre site `Master` et intermédiaire.

>[!NOTE]
>
>Pour obtenir des instructions sur la modification des informations d’identification du jeton API Fastly pour un environnement d’évaluation ou de production, voir [Modifier les informations d’identification Fastly](fastly.md#change-fastly-api-token).

### Charger VCL vers Fastly

Après avoir activé le module Fastly, téléchargez le code [VCL](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets) par défaut sur les serveurs Fastly. Ce code fournit une série de fragments de code VCL qui spécifient les paramètres de configuration pour activer la mise en cache et d’autres services Fast CDN pour votre Adobe Commerce sur l’infrastructure cloud.

>[!NOTE]
>
>Les services de mise en cache Fastly ne fonctionnent pas tant que vous n’avez pas terminé le chargement initial du code VCL Fastly sur les sites d’évaluation et de production Adobe Commerce.

**Pour charger le VCL Fastly** :

1. Dans la section _Configuration Fastly_, cliquez sur **Télécharger VCL vers Fastly** comme le montre la figure suivante.

   ![Charger un VCL de Magento sur Fastly](../../assets/cdn/fastly-upload-vcl-admin.png)

1. Une fois le chargement terminé, actualisez le cache en fonction de la notification en haut de la page.

## Approvisionnement des certificats SSL/TLS

Adobe fournit un certificat SSL/TLS validé par un domaine pour chiffrer le trafic HTTPS sécurisé provenant de Fastly. Adobe fournit un certificat pour chaque environnement de production Pro, d&#39;évaluation et de démarrage pour sécuriser tous les domaines dans cet environnement. Pour plus d’informations sur le certificat fourni, voir [Certificats SSL Adobe (TLS) pour Adobe Commerce sur l’infrastructure cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html).

>[!NOTE]
>
>Vous pouvez fournir votre propre certificat TLS ou SSL au lieu d’utiliser le certificat Let’s Encrypt fourni par Adobe. Toutefois, ce processus nécessite un travail supplémentaire pour sa configuration et sa maintenance. Pour choisir cette option, envoyez un ticket d’assistance Adobe Commerce ou utilisez Adobe pour ajouter des certificats hébergés personnalisés à votre Adobe Commerce sur les environnements d’infrastructure cloud.

Pour activer les certificats SSL/TLS pour les environnements Adobe Commerce, l’automatisation d’Adobe effectue les étapes suivantes :

- Valide la propriété du domaine
- Fournit un certificat Let’s Encrypt SSL/TLS qui couvre les domaines de premier niveau et sous-domaines spécifiés pour vos magasins
- Télécharge le certificat dans l’environnement Cloud lorsque le site est actif

Cette automatisation nécessite que vous mettiez à jour la configuration DNS de votre site pour fournir des informations de validation de domaine. Utilisez **une** l’une des méthodes suivantes :

- **Validation DNS**-Pour les sites en ligne, mettez à jour votre configuration DNS avec des enregistrements CNAME qui pointent vers le service Fastly
- **Enregistrements CNAME de défi ACME**-Mettez à jour votre configuration DNS avec des enregistrements CNAME de défi ACME fournis par Adobe pour chaque domaine de votre environnement

>[!TIP]
>
>Si vous avez un domaine de production qui n’est pas actif, utilisez les enregistrements CNAME de défi ACME pour la validation du domaine. L’ajout précoce des enregistrements à votre configuration DNS permet à l’Adobe de configurer le certificat SSL/TLS avec les domaines corrects avant le lancement du site. Avant le lancement en production, vous devez remplacer ces enregistrements d’espace réservé par les enregistrements CNAME fournis par Adobe.

Une fois la validation du domaine terminée, Adobe fournit le certificat Let’s Encrypt TLS/SSL et le charge dans des environnements d’évaluation ou de production actifs. Ce processus peut prendre jusqu’à 12 heures. Nous vous recommandons de terminer les mises à jour de la configuration DNS plusieurs jours à l&#39;avance afin d&#39;éviter les retards dans le développement et le lancement du site.

## Mise à jour de la configuration DNS avec les paramètres de développement

Pendant le processus de configuration Fastly initial, vous pouvez utiliser les URL suivantes pour configurer et tester la mise en cache Fastly dans les environnements d’évaluation et de production :

- Pour l’évaluation et la production pro :

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- Pour la production de démarrage uniquement :

   - `mcprod.<your-domain>.com`

Ces URL de pré-production par défaut sont disponibles une fois que votre projet est configuré. La valeur de `"your-domain"` est le nom de domaine que vous avez spécifié lors du processus d’intégration.

>[!NOTE]
>
>Vous ne pouvez pas spécifier de domaine personnalisé pour un environnement hors production dans les projets de démarrage.

Pour acheminer le trafic depuis les URL de votre boutique vers le service Fastly, mettez à jour votre configuration DNS. Lorsque vous mettez à jour la configuration, Adobe fournit automatiquement les certificats SSL/TLS requis et les charge dans vos environnements cloud. Cette mise en service peut prendre jusqu’à 12 heures.

>[!NOTE]
>
>Lorsque vous êtes prêt à lancer votre site de production, vous devez à nouveau mettre à jour la configuration DNS pour pointer vos domaines de production vers le service Fastly et effectuer des tâches de configuration supplémentaires. Voir [Liste de contrôle Launch](../launch/checklist.md).

**Conditions préalables :**

- Activez le module Fastly .
- Chargez le code VCL Fastly par défaut.
- Fournissez une liste de niveau supérieur et de sous-domaines pour chaque environnement à Adobe, ou envoyez un ticket d’assistance Adobe Commerce.
- Attendez la confirmation que les domaines spécifiés ont été ajoutés à vos environnements cloud.
- Dans les projets de démarrage, ajoutez les domaines à votre configuration de service Fastly . Voir [ Gestion des domaines ](fastly-custom-cache-configuration.md#manage-domains).
- Pour plus d’informations sur la mise à jour de la configuration DNS, contactez votre [bureau d’enregistrement DNS](https://lookup.icann.org/) pour connaître la méthode appropriée pour votre service de domaine.

**Pour mettre à jour votre configuration DNS pour le développement** :

1. Pointez les URL de pré-production sur le service Fastly en ajoutant des enregistrements CNAME : `prod.magentocloud.map.fastly.net`, par exemple :

   | Domaine ou sous-domaine | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   Lorsque les enregistrements CNAME sont actifs, l’Adobe fournit des certificats et charge les certificats SSL/TLS.

   >[!NOTE]
   >
   >Si vous prévoyez d’utiliser des domaines apex (`your-domain.com`) pour votre site de production, vous devez configurer les enregistrements d’adresses DNS (enregistrements A) pour pointer vers les adresses IP du serveur Fastly. Voir [Mise à jour de la configuration DNS avec les paramètres de production](../launch/checklist.md#to-update-dns-configuration-for-site-launch).


1. Ajoutez des enregistrements CNAME de défi ACME pour la validation de domaine et le pré-approvisionnement des certificats SSL/TLS de production, par exemple :

   | Domaine ou sous-domaine | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >Les enregistrements de défi ACME dans cet exemple sont des espaces réservés qui ne sont pas destinés à approvisionner vos sites d&#39;évaluation et de production Adobe Commerce. Obtenez les informations correctes sur l’enregistrement de défi ACME pour votre projet en contactant l’Adobe.

   Après avoir ajouté les enregistrements CNAME, l’Adobe valide les domaines et fournit le certificat SSL/TLS pour l’environnement. Lorsque vous mettez à jour la configuration DNS pour acheminer le trafic de ces domaines vers le service Fastly , Adobe charge le certificat dans l’environnement .

1. Mettez à jour l’URL de base d’Adobe Commerce.

   - Utilisez SSH pour vous connecter à l’environnement de production.

     ```bash
     magento-cloud ssh
     ```

   - Utilisez l’interface de ligne de commande Cloud pour modifier l’URL de base de votre boutique.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >Au lieu d’utiliser l’interface de ligne de commande Cloud, vous pouvez mettre à jour l’URL de base à partir de l’[ Admin ](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)

1. Redémarrez le navigateur web.

1. Testez votre site web.

## Tester la mise en cache Fastly

Une fois les modifications apportées à la configuration DNS, utilisez l’outil de ligne de commande [cURL](https://curl.se/) pour vérifier que le cache Fastly fonctionne.

**Pour vérifier les en-têtes de réponse** :

1. Dans un terminal, utilisez la commande `curl` suivante pour tester l’URL de votre site en direct :

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   Si vous n’avez pas défini d’itinéraire statique ou effectué la configuration DNS pour les domaines de votre site actif, utilisez l’indicateur `--resolve` qui contourne la résolution de nom DNS.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. Dans la réponse, vérifiez les [en-têtes](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) pour vous assurer que Fastly fonctionne. Vous devriez voir les en-têtes uniques suivants dans la réponse :

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Si les en-têtes n’ont pas les valeurs correctes, consultez [Résoudre les erreurs trouvées dans les en-têtes de réponse](fastly-troubleshooting.md#curl) pour obtenir de l’aide sur la résolution des problèmes.

## Mise à niveau du module Fastly

Fastly met à jour le réseau CDN Fastly pour le module Magento 2 afin de résoudre les problèmes, d’augmenter les performances et de fournir de nouvelles fonctionnalités.
Nous vous recommandons de mettre à jour le module Fastly dans vos environnements d’évaluation et de production vers la [dernière version](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

Après avoir mis à jour le module, vous devez charger le code VCL pour appliquer les modifications à la configuration de service Fastly.

>[!WARNING]
>
> Si vous avez personnalisé le code VCL Fastly par défaut avec une version personnalisée, la mise à niveau du module Fastly remplace vos modifications. Si vous avez ajouté des fragments de code VCL personnalisés avec des noms uniques, ces modifications sont conservées pendant le processus de mise à niveau. Il est recommandé de mettre à niveau l’environnement d’évaluation et de valider les modifications avant d’appliquer les modifications à l’environnement de production.

**Pour vérifier la version du module Fastly CDN pour le Magento 2** :

1. Accédez au répertoire racine de votre environnement cloud.

1. Utilisez le compositeur pour vérifier la version installée.

   ```bash
   composer show *fastly*
   ```

1. Si la [dernière version](https://github.com/fastly/fastly-magento2/releases) n’est pas installée, suivez les étapes de mise à niveau du module Fastly .

**Pour mettre à niveau le module Fastly** :

1. Dans votre environnement d’intégration local, utilisez les informations de module suivantes pour [mettre à niveau le module Fastly](../store/extensions.md#upgrade-an-extension).

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. Envoyez vos mises à jour à l’environnement d’évaluation.

1. Connectez-vous à l’administration de votre environnement d’évaluation pour [télécharger le code VCL](#upload-vcl-to-fastly).

1. [Vérifiez les services Fastly](fastly-troubleshooting.md#verify-or-debug-fastly-services) sur le site d’évaluation Adobe Commerce.

Après avoir vérifié les services Fastly sur le site d’évaluation, répétez le processus de mise à niveau dans l’environnement de production.

>[!TIP]
>
> Si vous rencontrez des problèmes avec les services Fastly dans vos environnements Adobe Commerce, consultez l’[utilitaire de dépannage Fastly Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html).
