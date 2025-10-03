---
title: Résolution rapide des problèmes
description: Découvrez comment dépanner et gérer le module et les services Fastly CDN pour Adobe Commerce.
feature: Cloud, Configuration, Cache, Services
exl-id: 69954ef9-9ece-411e-934e-814a56542290
source-git-commit: f496a4a96936558e6808b3ce74eac32dfdb9db19
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Résolution rapide des problèmes

Utilisez les informations suivantes pour résoudre les problèmes et gérer le module de réseau CDN Fastly pour Magento 2 dans vos environnements de projet d’infrastructure cloud Adobe Commerce. Par exemple, vous pouvez examiner les valeurs d’en-tête de réponse et le comportement de mise en cache pour résoudre les problèmes de service et de performances Fastly.

Dans les environnements de production et d’évaluation Pro, vous pouvez utiliser les [journaux New Relic](../monitor/log-management.md) pour afficher et analyser les données des journaux Fastly CDN et WAF afin de résoudre les erreurs et les problèmes de performances.

>[!NOTE]
>
>Pour plus d’informations sur la configuration de Fastly, voir [Configuration de Fastly](fastly.md).

## Localisation de l’ID de service Fastly

Vous avez besoin de l’identifiant du service Fastly pour configurer Fastly à partir de l’administration ou pour envoyer des requêtes d’API Fastly pour une configuration et un dépannage Fastly avancés.

Si Fastly est activé dans votre environnement de projet, vous pouvez obtenir l’ID de service auprès de l’administrateur. Voir [Obtention des informations d’identification Fastly](fastly-configuration.md#get-fastly-credentials).

Les développeurs et les utilisateurs avancés de VCL peuvent utiliser un VCL personnalisé pour récupérer l’ID de service à l’aide de la variable Fastly `req.service_id`. Par exemple, vous pouvez ajouter le `req.service_id` à la directive de journalisation personnalisée dans votre VCL pour capturer la valeur de l’ID de service :

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

Vous pouvez utiliser le même VCL pour les environnements de production et d’évaluation. Voir [`vcl_log`](https://www.fastly.com/documentation/reference/vcl/subroutines/log/) dans la _documentation Fastly_.

## Problèmes de performances, de purge et de mise en cache du site

Utilisez la liste suivante pour identifier et résoudre les problèmes liés à la configuration de service Fastly pour votre environnement Adobe Commerce sur une infrastructure cloud.

- **Le menu Boutique ne s’affiche pas ou ne fonctionne pas**—Il se peut que vous utilisiez un lien ou un lien temporaire directement vers le serveur d’origine au lieu d’utiliser l’URL du site actif, ou que vous utilisiez `-H "host:URL"` dans une commande [cURL](#check-live-site-through-fastly). Si vous ignorez Fastly au serveur d’origine, le menu principal ne fonctionne pas et des en-têtes incorrects s’affichent pour permettre la mise en cache côté navigateur.

- **La navigation supérieure ne fonctionne pas**—La navigation supérieure repose sur le traitement ESI (Edge Side Includes) qui est activé lorsque vous téléchargez les fragments de code VCL Fastly Magento par défaut. Si la navigation ne fonctionne pas, [chargez le VCL Fastly](fastly-configuration.md#upload-vcl-to-fastly) et vérifiez à nouveau le site.

- **La géolocalisation/GeoIP ne fonctionne pas**— Les fragments de code VCL Fastly Magento par défaut ajoutent le code de pays à l’URL. Si le code pays ne fonctionne pas, [téléchargez le VCL Fastly](fastly-configuration.md#upload-vcl-to-fastly) et vérifiez à nouveau le site.

- **Les pages ne sont pas mises en cache** : par défaut, Fastly ne met pas en cache les pages avec l’en-tête `Set-Cookies`. Adobe Commerce définit les cookies même sur les pages pouvant être mises en cache (TTL > 0). Le VCL Fastly de Magento par défaut supprime ces cookies sur les pages pouvant être mises en cache. Si les pages ne sont pas mises en cache, [chargez le VCL Fastly](fastly-configuration.md#upload-vcl-to-fastly) et vérifiez à nouveau le site.

  Ce problème peut également se produire si un bloc de page d’un modèle est marqué comme ne pouvant pas être mis en cache. Dans ce cas, le problème est probablement dû à un module tiers ou à une extension qui bloque ou supprime les en-têtes Adobe Commerce. Pour résoudre le problème, voir [X-Cache contient uniquement MISS, aucun ACCÈS](#x-cache-contains-only-miss-no-hit).

- **Les requêtes de purge échouent** : renvoie rapidement l’erreur suivante lorsque vous soumettez une requête de purge :

  ```text
  The purge request was not processed successfully.
  ```

  Ce problème peut être dû à l’un des problèmes suivants :

   - Informations d’identification Fastly non valides dans la configuration du service Fastly pour l’environnement de projet d’infrastructure cloud Adobe Commerce
   - Code non valide dans un fragment de code VCL personnalisé

  Pour résoudre ce problème, consultez la section [Erreur lors de la purge du cache Fastly sur Cloud](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) dans le Centre d’aide Adobe Commerce.

## Erreurs 503 de Fastly

Si Fastly renvoie des erreurs de délai d’expiration 503, vérifiez les journaux d’erreurs et la page d’erreur 503 pour identifier la cause première.

>[!NOTE]
>
>Si le délai d’expiration survient lors de l’exécution d’opérations en bloc, vous pouvez [prolonger le délai d’expiration Fastly pour l’administrateur](fastly-custom-cache-configuration.md#extend-fastly-timeout).

Si vous recevez une erreur 503, vérifiez le journal des erreurs de l’environnement de production ou d’évaluation et le journal d’accès php pour résoudre le problème.

**Pour vérifier les journaux d’erreurs** :

- [Journal des erreurs](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  Ce journal inclut toutes les erreurs de l&#39;application ou du moteur PHP, par exemple les erreurs `memory_limit` ou `max_execution_time exceeded`. Si vous ne trouvez pas d&#39;erreurs Fastly, vérifiez le journal d&#39;accès PHP.

- Log d&#39;accès PHP

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  Recherchez dans le journal les réponses HTTP 200 de l’URL qui a renvoyé l’erreur 503. Si vous trouvez la réponse 200, cela signifie qu’Adobe Commerce a renvoyé la page sans erreur. Cela indique que le problème a pu se produire après l’intervalle qui dépasse la valeur `first_byte_timeout` définie dans la configuration de service Fastly.

Lorsqu’une erreur 503 se produit, Fastly renvoie la raison sur la page d’erreur et de maintenance. Il se peut que vous ne puissiez pas en voir la raison si vous avez ajouté du code pour une [page de réponse personnalisée](fastly-custom-response.md). Pour afficher le code de motif sur la page d’erreur par défaut, vous pouvez supprimer le code HTML de la page d’erreur personnalisée.

**Pour vérifier la page d’erreur Fastly 503** :

{{admin-login-step}}

1. Cliquez sur **Magasins** > **Paramètres** > **Configuration** > **Avancé** > **Système**.

1. Dans le volet de droite, développez **Cache de page complet**.

1. Dans la section **Configuration Fastly**, développez **Pages synthétiques personnalisées** comme le montre la figure suivante.

   ![Page d’erreur 503 personnalisée](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. Cliquez sur **Définir HTML**.

1. Supprimez le code personnalisé. Vous pouvez l’enregistrer dans un programme texte pour l’ajouter ultérieurement.

1. Cliquez sur **Télécharger** pour envoyer vos mises à jour à Fastly.

1. Cliquez sur **Enregistrer la configuration** en haut de la page.

1. Rouvrez l’URL à l’origine de l’erreur 503. Fastly renvoie une page d’erreur avec la raison comme illustré dans l’exemple suivant.

   ![Erreur Fastly](../../assets/cdn/fastly-503-example.png)

## Apex et sous-domaines déjà associés à un compte Fastly

Si le domaine apex et les sous-domaines de votre projet d’infrastructure cloud Adobe Commerce sont déjà associés à un compte Fastly existant avec un ID de service attribué, vous ne pouvez pas lancer tant que vous n’avez pas mis à jour votre configuration Fastly :

- Mettez à jour la configuration apex et de sous-domaine sur le compte Fastly existant. Voir [Comptes Fastly multiples et domaines attribués](fastly.md#multiple-fastly-accounts-and-assigned-domains).

- [Activez et configurez Fastly](fastly-configuration.md#enable-fastly-caching) et terminez la [configuration DNS](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Vérification ou débogage des services Fastly

Vous pouvez résoudre les problèmes de performances ou de mise en cache d’un site Adobe Commerce sur une infrastructure cloud en testant les URL du site et en examinant les valeurs d’en-tête renvoyées dans la réponse.

### Vérifier le site en direct via Fastly

Utilisez l’API Fastly pour vérifier les en-têtes de réponse `Fastly-Magento-VCL-Uploaded` et `X-Cache` renvoyés par votre site en ligne.

Les requêtes d’API Fastly sont transmises par l’intermédiaire de l’extension Fastly pour obtenir une réponse de vos serveurs d’origine. Si la réponse renvoie des en-têtes incorrects, testez directement les serveurs [origin](#bypass-fastly-cache-to-check-adobe-commerce-sites).

**Pour vérifier les en-têtes de réponse** :

1. Dans un terminal, utilisez la commande `curl` suivante pour tester l’URL de votre site en direct :

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   Si vous n’avez pas défini d’itinéraire statique ou effectué la configuration DNS pour les domaines de votre site actif, utilisez l’indicateur `--resolve` qui contourne la résolution de nom DNS.

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >Pour utiliser cette commande avec l’option `--resolve`, TLS doit être activé avec Fastly via un certificat SSL/TLS et l’adresse IP du nœud de cache doit être trouvée.

1. Dans la réponse, vérifiez les [en-têtes](#check-cache-hit-and-miss-response-headers) pour vous assurer que Fastly fonctionne. Vous devriez voir les en-têtes uniques suivants dans la réponse :

   ```http
   < Fastly-Magento-VCL-Uploaded: 1.2.222
   < X-Cache: HIT, MISS
   ```

Si les en-têtes n’ont pas les valeurs correctes, consultez les informations suivantes :

- [Vérifier le chargement VCL](#fastly-vcl-has-not-been-uploaded)

- [X-Cache contient uniquement MISS, pas d’ACCÈS.](#x-cache-contains-only-miss-no-hit)

### Contournement du cache Fastly pour vérifier les sites Adobe Commerce

Si le service Fastly renvoie des en-têtes incorrects, vous pouvez créer un fragment de code VCL qui vous permet d’envoyer des requêtes qui contournent le cache Fastly. Voir [ Contournement du cache Fastly ](fastly-vcl-bypass-to-origin.md).

Après avoir ajouté le fragment de code VCL, utilisez les commandes cURL pour envoyer des requêtes au serveur d’origine à partir de l’adresse IP spécifiée. Vérifiez ensuite les réponses pour détecter les erreurs.

### Vérifier les en-têtes de réponse d’accès au cache et d’absence

Vérifiez que la réponse renvoyée contient les informations suivantes :

- Inclut l’en-tête `X-Magento-Tags`

- La valeur de l’en-tête `Fastly-Module-Enabled` est `Yes` ou le numéro de version du module Fastly pour CDN Magento 2 installé dans l’environnement de projet

- [Contrôle de cache : âge maximal](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) est supérieur à 0.

- Le paramètre [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) est `cache`

L’extrait suivant de la sortie de la commande cURL affiche les valeurs correctes pour les en-têtes `Pragma`, `X-Magento-Tags` et `Fastly-Module-Enabled` :

```
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>Pour plus d’informations sur les accès et les échecs, voir [Présentation des en-têtes d’accès et de manque dans le cache avec des services blindés](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) dans la documentation Fastly .

### Résoudre les erreurs trouvées dans les en-têtes de réponse

Cette section fournit des suggestions pour résoudre les erreurs renvoyées lors de la vérification des en-têtes de réponse à l’aide de l’API Fastly.

#### Le module Fastly n’est pas activé

Si le module Fastly n’est pas activé (`Fastly-Module-Enabled: no`) ou si l’en-tête est manquant, [utilisez SSH pour vous connecter](../development/secure-connections.md#connect-to-a-remote-environment) au projet. Exécutez ensuite la commande suivante pour vérifier le statut du module.

```bash
php bin/magento module:status Fastly_Cdn
```

En fonction du statut renvoyé, utilisez les instructions suivantes pour mettre à jour la configuration Fastly .

- `Module does not exist` : si le module n’existe pas [installer et configurer](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md) le module Fastly CDN pour Magento 2 dans une branche d’intégration. Une fois l’installation terminée, activez et configurez le module . Voir [ Configuration rapide ](fastly-configuration.md).

- `Module is disabled` : si le module Fastly est désactivé, mettez à jour la configuration de l’environnement sur une branche `integration` de votre environnement local pour l’activer. Ensuite, poussez les modifications vers les environnements d’évaluation et de production. Voir [Gestion des extensions](../store/extensions.md#install-an-extension).

  Si vous utilisez [Gestion de la configuration](../store/store-settings.md#configure-store), vérifiez l’état du module de réseau CDN Fastly dans le fichier de configuration `app/etc/config.php` avant d’appliquer les modifications à l’environnement de production ou d’évaluation.

  Si le module n’est pas activé (`Fastly_CDN => 0`) dans le fichier `config.php`, supprimez le fichier et exécutez la commande suivante pour `config.php` mettre à jour avec les derniers paramètres de configuration.

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Fastly VCL n’a pas été chargé

Si le Fastly VCL n’a pas été chargé (`Fastly-Magento-VCL-Uploaded` : `false`), utilisez l’option *Télécharger VCL* dans l’Administration pour le charger. Voir [Chargement rapide de fragments de code VCL](fastly-configuration.md#upload-vcl-to-fastly).

#### X-Cache contient uniquement MISS, pas d’ACCÈS.

Si l’en-tête `X-Cache` contient des `HIT` (`HIT, HIT` ou `HIT, MISS`), cela indique que Fastly renvoie correctement le contenu mis en cache.

Si l’en-tête `X-Cache` est `MISS, MISS` et ne contient pas de `HIT`, exécutez à nouveau la commande `curl` pour vous assurer que la page n’a pas été récemment purgée du cache.

Si vous obtenez le même résultat, utilisez les commandes [`curl` et vérifiez ](#check-live-site-through-fastly)en-têtes de réponse [](#check-cache-hit-and-miss-response-headers) :

- `Pragma` est `cache`
- `X-Magento-Tags` existe
- `Cache-Control: max-age` est supérieur à 0

Si le problème persiste, une autre extension réinitialise probablement ces en-têtes. Répétez la procédure suivante dans l’environnement d’évaluation en désactivant toutes les extensions et en réactivant chacune d’elles pour déterminer l’extension qui réinitialise les en-têtes. Après avoir identifié l’extension à l’origine du problème, vous devez la désactiver dans l’environnement de production.

**Pour identifier une extension qui réinitialise les en-têtes de réponse :**

{{admin-login-step}}

1. Accédez à **Magasins** > **Paramètres** > **Configuration** > **Avancé** > **Avancé**.

1. Dans la section *Désactiver la sortie des modules* du volet de droite, recherchez toutes vos extensions et désactivez-les.

1. Cliquez sur **Enregistrer la configuration**.

1. Cliquez sur **Système** > **Outils** > **Gestion du cache**.

1. Cliquez sur **Vider le cache Magento**.

1. Effectuez les étapes suivantes pour chaque extension, ce qui peut entraîner des problèmes avec les en-têtes Fastly :

   - Activez une extension à la fois, enregistrez la configuration et videz le cache d’Adobe Commerce.

   - Exécutez les commandes [`curl` pour vérifier ](#check-live-site-through-fastly) en-têtes de réponse [](#check-cache-hit-and-miss-response-headers).

   Répétez ce processus pour chaque extension. Si les en-têtes de réponse Fastly ne s’affichent plus, vous avez identifié l’extension à l’origine des problèmes liés à Fastly.

Après avoir identifié l’extension qui réinitialise les en-têtes Fastly, contactez le développeur d’extensions pour obtenir une assistance supplémentaire. Nous ne pouvons pas fournir de correctifs ou de mises à jour pour que les extensions tierces fonctionnent avec la mise en cache Fastly.

## Restaurer rapidement la configuration

Si des mises à jour de fragments de code VCL personnalisés ou d&#39;autres modifications de configuration Fastly entraînent une panne ou le renvoi d&#39;erreurs par un site d&#39;infrastructure cloud d&#39;Adobe Commerce, utilisez la commande Fastly API [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) pour restaurer une version antérieure de VCL. Vous ne pouvez pas restaurer la version VCL à partir de l&#39;administrateur.

**Pour restaurer la version VCL** :

1. Pour obtenir la liste des versions VCL disponibles pour un service, exécutez la commande suivante

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. Exécutez la commande suivante pour modifier la version VCL active en une version spécifiée.

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Pour plus d’informations sur l’utilisation de l’API Fastly pour examiner et gérer le VCL, voir [Gérer le VCL à l’aide de l’API](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
