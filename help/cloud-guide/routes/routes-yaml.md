---
title: Configuration des itinéraires
description: Découvrez comment définir les itinéraires des requêtes HTTPS entrantes pour Adobe Commerce sur les environnements d’infrastructure cloud.
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Configuration des itinéraires

Le fichier `routes.yaml` dans le répertoire `.magento/routes.yaml` définit les itinéraires pour votre Adobe Commerce dans les environnements d’intégration, d’évaluation et de production de l’infrastructure cloud. Les itinéraires déterminent la manière dont l’application traite les requêtes HTTP et HTTPS entrantes.

Le fichier `routes.yaml` par défaut spécifie les modèles d’itinéraire pour le traitement des requêtes HTTP en tant que HTTPS sur les projets ayant un seul domaine par défaut et sur les projets configurés pour plusieurs domaines :

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

Utilisez l’interface de ligne de commande `magento-cloud` pour afficher la liste des itinéraires configurés :

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Mises à jour de configuration des environnements Pro

{{pro-self-service-warning}}

## Modèles d’itinéraire

Le fichier `routes.yaml` est une liste d’itinéraires modélisés et de leurs configurations. Vous pouvez utiliser les espaces réservés suivants dans les modèles d’itinéraire :

- L’espace réservé `{default}` représente le nom de domaine qualifié configuré par défaut pour le projet.

  Par exemple, un projet avec le `example.com` de domaine par défaut et les modèles d’itinéraire suivants :

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  Ces modèles sont résolus sur les URL suivantes dans un environnement de production :

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- L’espace réservé `{all}` représente tous les noms de domaine configurés pour le projet.

  Par exemple, un projet avec des domaines `example.com` et `example1.com` avec les modèles d’itinéraire suivants :

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  Ces modèles sont résolus sur les itinéraires suivants pour tous les domaines du projet :

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  L’espace réservé `{all}` est utile pour les projets configurés pour plusieurs domaines. Dans une branche hors production, `{all}` est remplacé par l’ID de projet et l’ID d’environnement pour chaque domaine.

  Si aucun domaine n’est configuré pour un projet, ce qui est courant lors du développement, l’espace réservé `{all}` se comporte de la même manière que l’espace réservé `{default}`.

Adobe Commerce génère également des itinéraires pour chaque environnement d’intégration actif. Pour les environnements d’intégration, l’espace réservé `{default}` est remplacé par le nom de domaine suivant :

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

Par exemple, la branche `refactorcss` du projet `mswy7hzcuhcjw` hébergé dans le cluster `us` comporte le domaine suivant :

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Si votre projet cloud prend en charge plusieurs magasins, suivez les instructions de configuration d’itinéraire pour [ plusieurs sites web ou magasins ](../store/multiple-sites.md).

### Barre oblique de fin

Les définitions d’itinéraire contiennent une barre oblique de fin pour indiquer un dossier ou un répertoire ; toutefois, le même contenu peut être diffusé avec ou sans barre oblique de fin. Les URL suivantes résolvent les mêmes problèmes, mais peuvent être interprétées comme _deux URL différentes_ :

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>Il est recommandé d’utiliser une barre oblique pour les répertoires, mais quelle que soit la méthode choisie, il est important de **rester cohérent** pour éviter de générer deux emplacements.

## Protocoles d&#39;itinéraire

Tous les environnements prennent automatiquement en charge HTTP et HTTPS.

- Si la configuration ne spécifie que l’itinéraire HTTP, les itinéraires HTTPS sont créés automatiquement, ce qui permet de diffuser le site à partir des protocoles HTTP et HTTPS sans nécessiter de redirections.

  Par exemple, un projet avec le `example.com` de domaine par défaut et le modèle d’itinéraire suivant :

  ```text
  http://{default}/
  ```

  Ce modèle est résolu sur les URL suivantes :

  ```text
  http://example.com/
  
  https://example.com/
  ```

- Si la configuration spécifie uniquement l’itinéraire HTTPS, toutes les requêtes HTTP sont redirigées vers HTTPS.

  Par exemple, un projet avec le domaine par défaut `example.com` avec le modèle d’itinéraire suivant :

  ```text
  https://{default}/
  ```

  Ce modèle est résolu sur l’URL suivante :

  ```text
  https://example.com/
  ```

  Il traite également la redirection suivante :

  `http://example.com/` ==> `https://example.com/`

Diffuser toutes les pages via TLS. Pour cette configuration, vous devez configurer des redirections pour toute requête non chiffrée vers l’équivalent TLS à l’aide de l’une des méthodes suivantes :

- Remplacez le protocole par HTTPS dans le fichier `routes.yaml`.

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- Pour les environnements d’évaluation et de production, activez l’option [ Forcer TLS sur Fastly ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html) à partir de l’interface utilisateur d’administration. Lorsque vous utilisez cette option, Fastly gère la redirection vers HTTPS, de sorte que vous n’ayez pas à mettre à jour la configuration `routes.yaml`.

## Options d’itinéraire

Configurez chaque itinéraire séparément à l’aide des propriétés suivantes :

| Propriété | Description |
| ---------------- | ----------- |
| `type: upstream` | Sert une application. En outre, il possède une propriété `upstream` qui spécifie le nom de l’application (tel que défini dans `.magento.app.yaml`) suivi du point d’entrée `:http`. |
| `type: redirect` | Redirige vers une autre route. Elle est suivie de la propriété `to` , qui est une redirection HTTP vers un autre itinéraire identifié par son modèle. |
| `cache:` | Contrôle [mise en cache pour l’itinéraire](caching.md). |
| `redirects:` | Contrôles [règles de redirection](redirects.md). |
| `ssi:` | Contrôle l’activation de [Inclusions côté serveur](server-side-includes.md). |

## Itinéraires simples

Dans les exemples suivants, la configuration de l’itinéraire achemine le domaine apex et le sous-domaine `www` vers l’application `mymagento`. Cet itinéraire ne redirige pas les requêtes HTTPS.

**Exemple 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

Dans cet exemple, le routage des requêtes suit les règles suivantes :

- Le serveur répond directement aux requêtes avec le modèle d’URL suivant :

  ```text
  http://example.com/path
  ```

- Le serveur émet une redirection _301_ pour les requêtes avec le modèle d’URL suivant :

  ```text
  http://www.example.com/mypath
  ```

  Ces requêtes redirigent vers le domaine apex, par exemple :

  ```text
  http://example.com/mypath
  ```

Dans l’exemple suivant, la configuration de l’itinéraire ne redirige pas les URL du domaine www vers le domaine apex. Au lieu de cela, les requêtes sont diffusées à partir des domaines www et apex.

**Exemple 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## Itinéraires génériques

Adobe Commerce sur l’infrastructure cloud prend en charge les itinéraires de caractères génériques, ce qui vous permet de mapper plusieurs sous-domaines à la même application. Cela fonctionne pour les itinéraires de redirection et en amont. Vous faites précéder l’itinéraire d’un astérisque (\*). Par exemple, les itinéraires suivants mènent à la même application :

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

Cela fonctionne comme un domaine fourre-tout dans un environnement en ligne.

### Acheminement vers un domaine non mappé

Vous pouvez acheminer vers un système qui n’est pas mappé à un domaine à l’aide d’un point (`.`) pour séparer le sous-domaine.

**Exemple:**

Un projet avec une branche `add-theme` est acheminé vers l’URL suivante :

```text
http://add-theme-projectID.us.magento.com/
```

Si vous définissez le modèle d&#39;itinéraire suivant :

```text
http://www.{default}/
```

L’itinéraire est résolu sur l’URL suivante :

```text
http://www.add-theme-projectID.us.magento.com/
```

Vous pouvez insérer n’importe quel sous-domaine avant la résolution du point et de l’itinéraire.

**Exemple:**

Définissez le modèle d’itinéraire suivant :

```text
http://*.{default}/
```

Ce modèle résout les deux URL suivantes :

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

Vous pouvez afficher le modèle d’itinéraire pour les domaines non mappés en établissant une connexion SSH à l’environnement et en utilisant l’interface de ligne de commande `magento-cloud` pour répertorier les itinéraires :

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## Redirections et mise en cache

Comme expliqué plus en détail dans la section [Redirections](redirects.md), vous pouvez gérer des règles de redirection complexes, telles que les _redirections partielles_, et spécifier des règles pour la [mise en cache](caching.md) basée sur les itinéraires :

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
