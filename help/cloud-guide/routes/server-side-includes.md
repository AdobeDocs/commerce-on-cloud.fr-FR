---
title: Inclusions côté serveur
description: Découvrez comment utiliser les inclusions côté serveur avec Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Inclusions côté serveur

Les [inclusions côté serveur](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) sont des directives dans des pages d’HTML qui sont évaluées sur le serveur pendant le rendu des pages. SSI vous permet d’ajouter du contenu généré dynamiquement à une page d’HTML existante sans diffuser la page entière.

Vous pouvez activer ou désactiver SSI par route dans votre `.magento/routes.yaml` ; par exemple :

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI vous permet d’inclure dans votre HTML des directives de réponse qui font en sorte que le serveur remplisse des parties de l’HTML, en respectant toute [configuration de mise en cache](caching.md) existante.

L&#39;exemple suivant montre comment insérer un contrôle de date dynamique en haut d&#39;une page et un autre contrôle de date en bas qui est mis à jour toutes les 600 secondes :

Ajoutez les éléments suivants à n’importe quelle page, par exemple `/index.php` :

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

Ajoutez le code suivant à `time.php` :

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

Accédez à la page sur laquelle vous avez ajouté le contrôle. Actualisez la page plusieurs fois et notez que l’heure en haut de la page change, mais que l’heure en bas ne change que toutes les 600 secondes.
