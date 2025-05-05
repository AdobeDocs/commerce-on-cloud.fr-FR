---
title: Mise en cache
description: Découvrez comment activer la mise en cache pour votre Adobe Commerce sur les environnements d’infrastructure cloud.
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
source-git-commit: a1ed2818cbaf5adf8b673df0ee9b9218e6f700a2
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Mise en cache

Vous pouvez activer la mise en cache dans votre environnement de projet d’infrastructure cloud. Si vous désactivez la mise en cache, Adobe Commerce diffuse directement les fichiers.

{{route-placeholder}}

## Configurer la mise en cache

Activez la mise en cache pour votre application en configurant les règles de mise en cache dans le fichier `.magento/routes.yaml` comme suit :

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## Mise en cache basée sur les itinéraires

Activez la mise en cache affinée en configurant des règles de mise en cache pour plusieurs itinéraires séparément, comme le montre l’exemple suivant :

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

L’exemple précédent met en cache les itinéraires suivants :

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

Et les itinéraires suivants ne sont **pas** mis en cache :

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Les expressions régulières dans les itinéraires ne sont **pas prises** charge.

## Durée de mise en cache

La durée du cache est déterminée par la valeur de l’en-tête de réponse `Cache-Control`. Si aucun en-tête de `Cache-Control` n’est présent dans la réponse, la clé `default_ttl` est utilisée.

## Clé de cache

Pour décider comment mettre en cache une réponse, Adobe Commerce crée une clé de cache qui dépend de plusieurs facteurs et stocke la réponse associée à cette clé. Lorsqu’une requête est fournie avec la même clé de cache, la réponse est réutilisée. Son objectif est similaire à celui de l’en-tête HTTP [`Vary`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

Les paramètres `headers` et clés de `cookies` permettent de modifier cette clé de cache.

La valeur par défaut de ces clés est la suivante :

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Attributs de cache

### `enabled`

Lorsqu’il est défini sur `true`, activez le cache pour cet itinéraire. Lorsqu’il est défini sur `false`, désactivez le cache pour cet itinéraire.

### `headers`

Définit les valeurs dont la clé de cache doit dépendre.

Par exemple, si la clé `headers` est la suivante :

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

Ensuite, Adobe Commerce met en cache une réponse différente pour chaque valeur de l’en-tête HTTP `Accept`.

### `cookies`

La clé `cookies` définit les valeurs dont la clé de cache doit dépendre.

Par exemple :

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

La clé de cache dépend de la valeur du cookie `value` dans la requête.

Il existe un cas particulier si la clé `cookies` a la valeur `["*"]`. Cette valeur signifie que toute requête avec un cookie contourne le cache. Il s’agit de la valeur par défaut.

>[!NOTE]
>
>Vous ne pouvez pas utiliser de caractères génériques dans le nom du cookie. Utilisez un nom de cookie précis ou faites correspondre tous les cookies avec un astérisque (`*`). Par exemple, `SESS*` ou `~SESS` sont actuellement des valeurs **non** valides.

Les cookies présentent les restrictions suivantes :

- Le système contient un maximum défini de **50 cookies**. Dans le cas contraire, l’application renvoie une exception `Unable to send the cookie. Maximum number of cookies would be exceeded`. Pour augmenter le nombre de cookies à 200, appliquez le correctif [MDVA-12304](https://experienceleague.adobe.com/docs/commerce-operations/tools/quality-patches-tool/release-notes.html?lang=fr) à l’aide de l’outil [Correctifs de qualité](https://experienceleague.adobe.com/fr/docs/commerce-learn/tutorials/tools/quality-patch-tool).
- La taille maximale des cookies est de **4 096 octets**. Dans le cas contraire, l’application renvoie une exception `Unable to send the cookie. Size of '%name' is %size bytes`.

### `default_ttl`

Si la réponse ne comporte pas d’en-tête `Cache-Control`, la clé `default_ttl` est utilisée pour définir la durée du cache, en secondes. La valeur par défaut est `0`, ce qui signifie que rien n’est mis en cache.
