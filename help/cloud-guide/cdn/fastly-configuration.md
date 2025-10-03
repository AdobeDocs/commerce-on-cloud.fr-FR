---
title: Configuration des services Fastly
description: Découvrez comment configurer les services Fastly pour votre projet Adobe Commerce.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: f9ce1e8b-4e9f-488e-8a4d-f866567c41d8
source-git-commit: cfb9aa37ddb4220aa9ce0b2e876c99bcdd40ae5a
workflow-type: tm+mt
source-wordcount: '2121'
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

Pendant la mise en service du projet, Adobe ajoute votre projet au [compte de service Fastly](fastly.md#fastly-service-account-and-credentials) pour Adobe Commerce sur l’infrastructure cloud et crée les informations d’identification de compte Fastly pour les environnements d’évaluation et de production Starter `master` et Pro. Chaque environnement possède des informations d’identification uniques.

Vous avez besoin des informations d’identification Fastly pour configurer les services de réseau CDN Fastly à partir de l’administrateur Adobe Commerce et pour envoyer les requêtes d’API Fastly.

## Accès au tableau de bord d’administration Fastly

Avec Adobe Commerce sur les infrastructures cloud, vous ne pouvez pas accéder directement au tableau de bord d’administration Fastly .

Utilisez l’Administration d’Adobe Commerce pour examiner et mettre à jour la configuration Fastly pour vos environnements. Si vous ne pouvez pas résoudre un problème à l’aide des fonctionnalités Fastly dans l’administration, envoyez un [ticket d’assistance pour Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html).

## Obtenir des informations d’identification Fastly

L’ID de service Fastly et le jeton API pour vos environnements d’évaluation et de production sont stockés dans votre environnement de projet cloud. Vous avez besoin des informations d’identification pour les deux environnements.

**Obtention des informations d’identification pour les projets Cloud Pro** :

Sur les projets Cloud Pro, vérifiez les informations d’identification à partir du répertoire partagé monté sur IaaS.

1. Utilisez SSH pour vous connecter à votre serveur.

2. Ouvrez le fichier `/mnt/shared/fastly_tokens.txt` pour obtenir les informations d’identification.

   Les environnements d’évaluation et de production disposent d’informations d’identification uniques. Vous devez obtenir les informations d’identification pour chaque environnement.

**Obtention des informations d’identification pour les projets Cloud Starter** :

Dans les projets Cloud Starter, obtenez les informations d’identification à partir de la console Cloud ou à l’aide de l’interface de ligne de commande Cloud :

- Dans la [!DNL Cloud Console] , vérifiez les variables d’environnement suivantes dans la [Configuration de l’environnement](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

- À partir de la ligne de commande de votre espace de travail local, utilisez l’interface de ligne de commande `magento-cloud` pour [répertorier et réviser](../environment/variables-cloud.md#viewing-environment-variables) les variables d’environnement Fastly.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

### Dépannage

- Si vous ne parvenez pas à trouver les informations d’identification Fastly pour les environnements d’évaluation ou de production, contactez votre conseiller technique client Adobe (CTA).

- [&#x200B; Erreur lors de la validation des informations d’identification Fastly &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials#solution).

## Sécuriser vos informations d’identification

Ne partagez pas votre jeton API dans des tickets d’assistance, des forums publics ou tout autre emplacement public. En outre, ne validez jamais les jetons API dans des référentiels de code. Les référentiels ne doivent contenir que des fichiers non modifiables sans informations sensibles.

L’assistance Adobe Commerce a déjà accès aux clés nécessaires. Vous n’avez donc pas besoin de fournir votre jeton API lorsque vous demandez de l’aide.

Si votre jeton API n’est jamais partagé publiquement ou joint à un ticket d’assistance, il est considéré comme compromis. Dans ce cas, Adobe doit générer un nouveau jeton pour vous.

## Activer la mise en cache Fastly

Vous avez besoin des composants suivants pour activer et configurer les services Fastly :

- La dernière version du module [Fast CDN for Magento 2](fastly.md#fastly-cdn-module-for-magento-2) est installée dans les environnements d’évaluation et de production. Voir [&#x200B; Mise à niveau rapide &#x200B;](#upgrade-the-fastly-module).

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
   >Ne sélectionnez pas le lien pour créer le jeton API Fastly. Utilisez plutôt les informations d’identification Fastly [&#x200B; (ID de service et jeton API) fournies par Adobe](#get-fastly-credentials).

1. Cliquez sur **Tester les informations d’identification**.

1. Si le test réussit, cliquez sur **Enregistrer la configuration**, puis effacez le cache.

   Si le test échoue, vérifiez que l’identifiant de service et les valeurs du jeton API corrects correspondent aux informations d’identification de l’environnement actuel.

   Si le test échoue à nouveau, envoyez un ticket d’assistance Adobe Commerce ou contactez votre représentant de compte Adobe. Pour les projets Pro, incluez les URL de vos sites de production et d’évaluation. Pour les projets de démarrage, incluez les URL de votre site `Master` et intermédiaire.

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

   ![Charger un VCL Magento vers Fastly](../../assets/cdn/fastly-upload-vcl-admin.png)

1. Une fois le chargement terminé, actualisez le cache en fonction de la notification en haut de la page.

## Approvisionnement des certificats SSL/TLS

Adobe fournit un certificat SSL/TLS validé par un domaine pour chiffrer le trafic HTTPS sécurisé provenant de Fastly. Adobe fournit un certificat pour chaque environnement Pro de production, d’évaluation et de démarrage de production afin de sécuriser tous les domaines de cet environnement. Pour plus d’informations sur le certificat fourni, voir [Certificats Adobe SSL (TLS) pour Adobe Commerce sur l’infrastructure cloud](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq).

>[!NOTE]
>
>Vous pouvez fournir votre propre certificat TLS ou SSL au lieu d’utiliser le certificat Let’s Encrypt fourni par Adobe. Toutefois, ce processus nécessite un travail supplémentaire de configuration et de maintenance. Pour choisir cette option, envoyez un ticket d’assistance Adobe Commerce ou utilisez Adobe pour ajouter des certificats hébergés personnalisés à votre Adobe Commerce sur les environnements d’infrastructure cloud.

Pour activer les certificats SSL/TLS pour les environnements Adobe Commerce, l’automatisation d’Adobe effectue les étapes suivantes :

- Valide la propriété du domaine
- Fournit un certificat Let’s Encrypt SSL/TLS qui couvre les domaines de premier niveau et sous-domaines spécifiés pour vos magasins
- Télécharge le certificat dans l’environnement Cloud lorsque le site est actif

Cette automatisation nécessite que vous mettiez à jour la configuration DNS de votre site pour fournir des informations de validation de domaine. Utilisez **une** l’une des méthodes suivantes :

- **Validation DNS**-Pour les sites en ligne, mettez à jour votre configuration DNS avec des enregistrements CNAME qui pointent vers le service Fastly
- **Enregistrements CNAME de défi ACME**-Mettez à jour votre configuration DNS avec les enregistrements CNAME de défi ACME fournis par Adobe pour chaque domaine de votre environnement

>[!TIP]
>
>Si vous avez un domaine de production qui n’est pas actif, utilisez les enregistrements CNAME de défi ACME pour la validation du domaine. L’ajout précoce des enregistrements à votre configuration DNS permet à Adobe de configurer le certificat SSL/TLS avec les domaines appropriés avant le lancement du site. Avant le lancement en production, vous devez remplacer ces enregistrements d’espace réservé par les enregistrements CNAME fournis par Adobe.

Une fois la validation du domaine terminée, Adobe fournit le certificat Let’s Encrypt TLS/SSL et le charge dans des environnements d’évaluation ou de production actifs. Ce processus peut prendre jusqu’à 12 heures. Adobe vous recommande de terminer les mises à jour de la configuration DNS plusieurs jours à l’avance afin d’éviter tout retard dans le développement et le lancement du site.

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
- Dans les projets de démarrage, ajoutez les domaines à votre configuration de service Fastly . Voir [&#x200B; Gestion des domaines &#x200B;](fastly-custom-cache-configuration.md#manage-domains).
- Pour plus d’informations sur la mise à jour de la configuration DNS, contactez votre [bureau d’enregistrement DNS](https://lookup.icann.org/) pour connaître la méthode appropriée pour votre service de domaine.

**Pour mettre à jour votre configuration DNS pour le développement** :

1. Pointez les URL de pré-production sur le service Fastly en ajoutant des enregistrements CNAME : `prod.magentocloud.map.fastly.net`, par exemple :

   | Domaine ou sous-domaine | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   Lorsque les enregistrements CNAME sont actifs, Adobe approvisionne les certificats et charge les certificats SSL/TLS.

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
   >Les enregistrements de défi ACME dans cet exemple sont des espaces réservés qui ne sont pas destinés à approvisionner vos sites d&#39;évaluation et de production Adobe Commerce. Obtenez les informations d’enregistrement de défi ACME appropriées pour votre projet en contactant Adobe.

   Après avoir ajouté les enregistrements CNAME, Adobe valide les domaines et approvisionne le certificat SSL/TLS pour l’environnement. Lorsque vous mettez à jour la configuration DNS pour acheminer le trafic de ces domaines vers le service Fastly , Adobe charge le certificat dans l’environnement.

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
   >Au lieu d’utiliser l’interface de ligne de commande Cloud, vous pouvez mettre à jour l’URL de base à partir de l’[&#x200B; Admin &#x200B;](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls)

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

1. Dans la réponse, vérifiez les [en-têtes](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) pour vous assurer que Fastly fonctionne. Par exemple, vous devriez voir les en-têtes uniques suivants dans la réponse :

   ```http
   < Fastly-Magento-VCL-Uploaded: 1.2.228
   < X-Cache: HIT, MISS
   ```

Si les en-têtes n’ont pas les valeurs correctes, consultez [Résoudre les erreurs trouvées dans les en-têtes de réponse](fastly-troubleshooting.md#curl) pour obtenir de l’aide sur la résolution des problèmes.

## Mise à niveau du module Fastly

Fastly met à jour le module Fastly CDN pour Magento 2 afin de résoudre les problèmes, d’augmenter les performances et de fournir de nouvelles fonctionnalités.
Adobe vous recommande de mettre à jour le module Fastly dans vos environnements d’évaluation et de production vers la [dernière version](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

Pour obtenir les dernières informations sur les versions et les mises à jour des modules, consultez les [notes de mise à jour du réseau CDN Fastly pour le module Magento2](https://github.com/fastly/fastly-magento2/blob/master/Release-Notes.md) sur GitHub.

Après avoir mis à jour le module, vous devez charger le code VCL pour appliquer les modifications à la configuration de service Fastly.

>[!WARNING]
>
> Si vous avez personnalisé le code VCL Fastly par défaut avec une version personnalisée, la mise à niveau du module Fastly remplace vos modifications. Si vous avez ajouté des fragments de code VCL personnalisés avec des noms uniques, ces modifications sont conservées pendant le processus de mise à niveau. Il est recommandé de mettre à niveau l’environnement d’évaluation et de valider les modifications avant d’appliquer les modifications à l’environnement de production.

**Pour vérifier la version du module Fastly CDN pour Magento 2** :

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
> Si vous rencontrez des problèmes avec les services Fastly dans vos environnements Adobe Commerce, consultez l’[utilitaire de dépannage Fastly Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter).
