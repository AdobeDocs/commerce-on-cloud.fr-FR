---
title: Inclusions côté serveur
description: Découvrez comment utiliser les inclusions côté serveur avec Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# Inclusions côté serveur

Les [ inclusions côté serveur ](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) sont des directives dans les pages HTML qui sont évaluées sur le serveur pendant le rendu des pages. SSI vous permet d’ajouter du contenu généré dynamiquement à une page HTML existante sans diffuser la page entière.

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

SSI vous permet d’inclure dans votre réponse HTML des directives qui font en sorte que le serveur remplisse des parties d’HTML, en respectant toute [ configuration de mise en cache](caching.md) existante.

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
