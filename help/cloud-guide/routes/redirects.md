---
title: Redirections
description: Découvrez comment gérer les règles de redirection pour votre projet d’infrastructure cloud Adobe Commerce.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Redirections

La gestion des règles de redirection est une exigence courante pour les applications web, en particulier dans les cas où vous ne souhaitez pas perdre les liens entrants qui ont été modifiés ou supprimés au fil du temps.

Vous trouverez ci-dessous comment gérer les règles de redirection sur vos projets d’infrastructure cloud Adobe Commerce à l’aide du fichier de configuration `routes.yaml`. Si les méthodes de redirection décrites dans cette rubrique ne fonctionnent pas pour vous, vous pouvez utiliser la mise en cache des en-têtes pour faire la même chose.

{{route-placeholder}}

## Mises à jour des environnements Pro

{{pro-self-service-warning}}

>[!WARNING]
>
>Pour Adobe Commerce sur les projets d’infrastructure cloud, la configuration de nombreuses redirections et réécritures non regex dans le fichier `routes.yaml` peut entraîner des problèmes de performances. Si votre fichier `routes.yaml` fait 32 Ko ou plus, déchargez vos redirections non regex et vos réécritures vers Fastly. Voir [Déchargement des redirections non regex vers Fastly au lieu de Nginx (itinéraires)](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) dans le _Centre d’aide d’Adobe Commerce_.

## Redirections de l’ensemble de l’itinéraire

En utilisant les redirections d’itinéraire complet, vous pouvez définir des itinéraires simples à l’aide du fichier `routes.yaml`. Par exemple, vous pouvez rediriger d’un domaine apex vers un sous-domaine `www` comme suit :

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Redirections d’itinéraire partiel

Dans le fichier `.magento/routes.yaml`, vous pouvez ajouter des règles de redirection partielles à des itinéraires existants en fonction de la correspondance des modèles :

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

Les redirections partielles fonctionnent avec tout type de route, y compris les routes desservies directement par l&#39;application.

Deux clés sont disponibles sous `redirects` :

- **expires**—Facultatif, spécifie la durée de mise en cache de la redirection dans le navigateur. Les exemples de valeurs valides comprennent `3600s`, `1d`, `2w`, `3m`.

- **chemins** : une ou plusieurs paires clé-valeur qui spécifient la configuration des règles de redirection d’itinéraire partiel.

  Pour chaque règle de redirection, la clé est une expression permettant de filtrer les chemins de requête pour la redirection. La valeur est un objet qui spécifie la destination cible de la redirection et les options de traitement de la redirection.

  L’objet de valeur possède les propriétés suivantes :

  | Propriété | Description |
  | ---------- | ----------- |
  | `to` | Obligatoire, chemin d’accès absolu partiel, URL avec protocole et hôte ou modèle spécifiant la destination cible de la règle de redirection. |
  | `regexp` | Facultatif, la valeur par défaut est `false`. Indique si la clé de chemin doit être interprétée comme une expression régulière PCRE. |
  | `prefix` | Indique si la redirection s’applique à la fois au chemin et à tous ses enfants, ou uniquement au chemin lui-même. La valeur par défaut est `true`. Cette valeur n’est pas prise en charge si `regexp` est `true`. |
  | `append_suffix` | Détermine si le suffixe est conservé avec la redirection. La valeur par défaut est `true`. Cette valeur n’est pas prise en charge si la clé `regexp` est `true` ou* si la clé `prefix` est `false`. |
  | `code` | Indique le code d’état HTTP. Les codes d’état valides sont [`301` (Déplacé définitivement)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8) et [`308`](https://www.rfc-editor.org/rfc/rfc7238). La valeur par défaut est `302`. |
  | `expires` | Facultatif, indique la durée de mise en cache de la redirection dans le navigateur. La valeur par défaut est la valeur `expires` définie directement sous la clé `redirects`, mais à ce niveau, vous pouvez affiner l’expiration du cache pour les redirections partielles individuelles. |

## Exemples de redirections d’itinéraire partiel

Les exemples suivants montrent comment spécifier des redirections d’itinéraire partiel dans le fichier `routes.yaml` à l’aide de diverses options de configuration `paths`.

### Correspondance de modèles d’expressions régulières

Utilisez le format suivant pour configurer les requêtes de redirection en fonction d’une expression régulière.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Cette configuration filtre les chemins d’accès aux requêtes par rapport à une expression régulière et redirige les requêtes correspondantes vers `https://example.com`. Par exemple, une requête vers `https://example.com/regexp/a/b/c/match` redirige vers `https://example.com/a/b/c`.

### Correspondance de modèles de préfixe

Utilisez le format suivant pour configurer des requêtes de redirection pour les chemins commençant par un modèle de préfixe spécifié.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

Cette configuration fonctionne comme suit :

- Redirige les requêtes qui correspondent au modèle `/from` vers le chemin `http://{default}/to`.

- Redirige vers `https://{default}/to/another/path` les requêtes qui correspondent au modèle `/from/another/path`.

- Si vous définissez la propriété `prefix` sur `false`, les requêtes qui correspondent au modèle de `/from` déclenchent une redirection, mais les requêtes qui correspondent au modèle de `/from/another/path` ne le font pas.

### Correspondance du modèle de suffixe

Utilisez le format suivant pour configurer les requêtes de redirection qui ajoutent le suffixe de chemin de la requête à la destination cible :

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Cette configuration fonctionne comme suit :

- Redirige les requêtes qui correspondent au modèle `/from/path/suffix` vers le chemin `https://{default}/to`.

- Si vous définissez la propriété `append_suffix` sur `true`, les requêtes qui correspondent `/from/path/suffix` sont redirigées vers le chemin d’accès `https://{default}/to/path/suffix`.

### Configuration du cache spécifique au chemin d’accès

Utilisez le format suivant pour personnaliser le temps de mise en cache d’une redirection à partir d’un chemin spécifique :

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

Cette configuration fonctionne comme suit :

- Les redirections à partir du premier chemin (`/from`) sont mises en cache pendant un jour.

- Les redirections à partir du deuxième chemin (`/here`) sont mises en cache pendant deux semaines.
