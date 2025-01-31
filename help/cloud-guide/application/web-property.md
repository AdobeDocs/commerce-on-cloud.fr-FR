---
title: Propriété web
description: Consultez des exemples sur la façon de configurer la propriété web dans le fichier  [!DNL Commerce]  configuration de l’application .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Propriété web

La propriété `web` définit la manière dont votre application est exposée au web (en HTTP), détermine la manière dont l’application web diffuse du contenu et contrôle la manière dont le conteneur d’applications répond aux requêtes entrantes en définissant des règles à chaque emplacement _bloc_. Un bloc représente un chemin absolu menant par une barre oblique (`/`).

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

Vous pouvez affiner votre configuration de `locations` à l’aide des valeurs de clé suivantes pour chaque bloc de `locations` :

| Attribut | Description |
| :--- | :--- |
| `allow` | Servez des fichiers qui ne correspondent pas aux « règles ». Valeur par défaut = `true` |
| `expires` | Définissez le nombre de secondes pour mettre le contenu en cache dans le navigateur. Cette clé active les en-têtes `cache-control` et `expires` pour le contenu statique. Si cette valeur n’est pas définie, la directive `expires` et les en-têtes qui en résultent ne sont pas inclus lors de la diffusion de fichiers de contenu statique. Une valeur 1 (`-1`) négative n’entraîne aucune mise en cache et est la valeur par défaut. Vous pouvez exprimer la valeur temporelle dans les unités suivantes : `ms` (millisecondes), `s` (secondes), `m` (minutes), `h` (heures), `d` (jours), `w` (semaines), `M` (mois, 30j) ou `y` (années, 365j) |
| `headers` | Définissez des en-têtes personnalisés, tels que `X-Frame-Options`, pour le contenu statique diffusé à partir de cet emplacement. |
| `index` | Répertoriez les fichiers statiques destinés à votre application, tels que le fichier `index.html`. Cette clé exige une collection. Cela ne fonctionne que si l’accès au ou aux fichiers est « autorisé » par la clé `allow` ou `rules` pour cet emplacement. |
| `rules` | Spécifier des remplacements pour un emplacement. Utilisez une expression régulière pour correspondre à une requête. Si une requête entrante correspond à la règle, la gestion régulière de la requête est remplacée par les clés utilisées dans la règle. |
| `passthru` | Définissez l’URL utilisée au cas où un fichier statique ou un fichier PHP serait introuvable. En règle générale, cette URL est le contrôleur frontal de vos applications, telles que `/index.php` ou `/app.php`. |
| `root` | Définissez le chemin d’accès par rapport à la racine de l’application exposée sur le web. Le répertoire public (emplacement « / ») d’un projet cloud est défini sur « pub » par défaut. |
| `scripts` | Autorise le chargement de scripts à cet emplacement. Définissez la valeur sur `true` pour autoriser les scripts. |

La configuration par défaut permet ce qui suit :

- À partir du chemin d’accès racine (`/`), seuls le web et les médias sont accessibles
- À partir des chemins d’accès `~/pub/static` et `~/pub/media`, n’importe quel fichier est accessible

L’exemple suivant illustre la configuration par défaut dans le fichier `.magento.app.yaml` pour un ensemble d’emplacements accessibles sur le web associés à une entrée dans la propriété [`mounts` ](properties.md#mounts)

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>Cet exemple illustre la configuration web par défaut d’un projet cloud configuré pour prendre en charge un seul domaine. Pour un projet qui nécessite la prise en charge de plusieurs sites web ou magasins, la configuration `web` doit être configurée pour prendre en charge les domaines partagés. Voir [Configuration des emplacements pour les domaines partagés](../store/multiple-sites.md#configure-locations-for-shared-domains).
