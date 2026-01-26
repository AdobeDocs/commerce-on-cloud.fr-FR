---
title: Configurer plusieurs sites web ou magasins
description: Découvrez comment configurer plusieurs sites web ou magasins pour Adobe Commerce sur une infrastructure cloud.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Configurer plusieurs sites web ou magasins

Vous pouvez configurer Adobe Commerce pour qu’il dispose de plusieurs sites web ou magasins, tels qu’un magasin en anglais, un magasin en français et un magasin en allemand. Voir [Présentation des sites web, des boutiques et des affichages de boutique](best-practices.md#store-views).

>[!WARNING]
>
>Les données du catalogue se développent à mesure que vous augmentez le nombre de sites web et de boutiques. Selon l’architecture de votre projet, les magasins supplémentaires peuvent entraîner un processus d’indexation plus long et des temps de réponse plus lents pour les pages de catalogue non mises en cache. Adobe vous recommande de surveiller étroitement les performances du site.

Le processus de configuration de plusieurs magasins dépend de votre choix d’utiliser des domaines uniques ou partagés.

Plusieurs magasins avec des domaines uniques :

```
https://first.store.com/
https://second.store.com/
```

Plusieurs magasins avec le même domaine :

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Pour ajouter une vue de magasin à l’URL de base du site, il n’est pas nécessaire de créer plusieurs répertoires. Voir [Ajouter le code de magasin à l’URL de base](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) dans le _Guide de configuration_.

## Ajouter des domaines

Les domaines personnalisés peuvent être ajoutés à Pro Staging et à tout environnement de production ; ils ne peuvent pas être ajoutés aux environnements d’intégration.

Le processus d’ajout d’un domaine dépend du type de compte Cloud :

- Pour l’évaluation et la production pro

  Ajoutez le nouveau domaine à Fastly, consultez [Gestion des domaines](../cdn/fastly-custom-cache-configuration.md#manage-domains) ou ouvrez un ticket d’assistance pour demander de l’aide. En outre, vous devez [envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour demander l’ajout de nouveaux domaines à un cluster.

- Pour la production de démarrage uniquement

  Ajoutez le nouveau domaine à Fastly, consultez [Gestion des domaines](../cdn/fastly-custom-cache-configuration.md#manage-domains) ou [Envoi d’un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour demander de l’aide. En outre, vous devez ajouter le nouveau domaine à l’onglet **Domaines** dans le [!DNL Cloud Console] : `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Configuration de l’installation locale

Pour configurer votre installation locale afin d’utiliser plusieurs magasins, voir [Sites web ou magasins multiples](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html) dans le _Guide de configuration_.

Une fois l’installation locale créée et testée pour utiliser plusieurs magasins, vous devez préparer votre environnement d’intégration :

1. **Configurer des itinéraires ou des emplacements** : indiquez comment les URL entrantes sont gérées par Adobe Commerce

   - [Itinéraires pour des domaines distincts](#configure-routes-for-separate-domains)
   - [Emplacements pour les domaines partagés](#configure-locations-for-shared-domains)

1. **Configurer des sites web, des boutiques et des affichages de boutique** à configurer à l’aide de l’interface utilisateur d’administration d’Adobe Commerce
1. **Modifier les variables** : spécifiez les valeurs des variables `MAGE_RUN_TYPE` et `MAGE_RUN_CODE` dans le fichier `magento-vars.php`
1. **Déployer et tester les environnements**—déployer et tester la branche `integration`

>[!TIP]
>
>Vous pouvez utiliser un environnement local pour configurer plusieurs sites web ou magasins. Consultez les instructions de Cloud Docker pour [Configurer plusieurs sites web ou magasins](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites).

### Mises à jour de configuration des environnements Pro

{{pro-self-service-warning}}

### Configurer des itinéraires pour des domaines distincts

Les itinéraires définissent comment traiter les URL entrantes. Plusieurs magasins avec des domaines uniques requièrent que vous définissiez chaque domaine dans le fichier `routes.yaml`. La façon dont vous configurez les itinéraires dépend de la façon dont vous souhaitez que votre site fonctionne.

**Pour configurer des itinéraires dans un environnement d’intégration** :

1. Sur votre station de travail locale, ouvrez le fichier `.magento/routes.yaml` dans un éditeur de texte.

1. Définissez le domaine et les sous-domaines. La valeur en amont `mymagento` est identique à la valeur de la propriété name dans le fichier `.magento.app.yaml`.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Enregistrez vos modifications dans le fichier `routes.yaml`.

1. Passez à [Configurer des sites web, des boutiques et des affichages de boutique](#set-up-websites-stores-and-store-views).

### Configuration des emplacements pour les domaines partagés

Lorsque la configuration des itinéraires définit la manière dont les URL sont traitées, la propriété `web` du fichier `.magento.app.yaml` définit la manière dont votre application est exposée au Web. Les _web permettent_ requêtes entrantes avec plus de granularité. Par exemple, si votre domaine est `store.com`, vous pouvez utiliser `/first` (site par défaut) et `/second` pour les requêtes vers deux magasins différents qui partagent un domaine.

**Pour configurer un nouvel emplacement web** :

1. Créez un alias pour la racine (`/`). Dans cet exemple, l’alias est `&app` à la ligne 3.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Créez un pass-through pour le site web (`/website`) et référencez la racine à l’aide de l’alias de l’étape précédente.

   L’alias `website` permet d’accéder aux valeurs à partir de l’emplacement racine. Dans cet exemple, le site web `passthru` se trouve à la ligne 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**Pour configurer un emplacement avec un autre répertoire** :

1. Créez un alias pour les emplacements racine (`/`) et statique (`/static`).

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Créez un sous-répertoire pour le site web dans le répertoire `pub` : `pub/<website>`

1. Copiez le fichier `pub/index.php` dans le répertoire `pub/<website>` et mettez à jour le chemin d’accès `bootstrap` (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Créez un pass-through pour le fichier `index.php`.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Validez et envoyez les fichiers modifiés.

   - `pub/<website>/index.php` (si ce fichier est en `.gitignore`, la notification push peut nécessiter l’option forcer .)
   - `.magento.app.yaml`

### Configurer des sites web, des boutiques et des affichages de boutique

Dans l’_interface utilisateur d’administration_, configurez vos **sites web**, **magasins** et **vues de magasin** Adobe Commerce. Voir [Configuration de plusieurs sites web, boutiques et vues de boutique dans la section Admin](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) du _Guide de configuration_.

Il est important d’utiliser le même nom et le même code pour vos sites web, magasins et vues de magasin de votre administrateur lorsque vous configurez votre installation locale. Vous avez besoin de ces valeurs lorsque vous mettez à jour le fichier `magento-vars.php`.

### Modification des variables

Au lieu de configurer un hôte virtuel NGINX, transmettez les variables `MAGE_RUN_CODE` et `MAGE_RUN_TYPE` à l’aide du fichier `magento-vars.php` dans le répertoire racine du projet.

**Pour transmettre des variables à l’aide du fichier `magento-vars.php`** :

1. Ouvrez le fichier `magento-vars.php` dans un éditeur de texte.

   Le [fichier `magento-vars.php` par défaut](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) doit se présenter comme suit :

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Déplacez le bloc de `if` commenté afin qu’il soit _après_ le bloc de `function` et qu’il ne soit plus commenté.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Remplacez les valeurs suivantes dans le bloc `if (isHttpHost("example.com"))` :
   - `example.com` : avec l’URL de base de votre _site web_
   - `default` : avec le CODE unique pour votre _vue de site web_ ou _de magasin_
   - `store` : avec l&#39;une des valeurs suivantes :
      - `website` : charge le _site web_ dans le storefront
      - `store` : charge une vue _store_ dans le storefront

   Pour plusieurs sites utilisant des domaines uniques :

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   Pour plusieurs sites avec le même domaine, vous devez vérifier le _hôte_ et le _URI_ :

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Enregistrez vos modifications dans le fichier `magento-vars.php`.

### Déploiement et test sur le serveur d’intégration

Envoyez vos modifications à votre environnement d’intégration d’Adobe Commerce sur l’infrastructure cloud et testez votre site.

1. Ajouter, valider et transférer les modifications de code vers la branche distante.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Attendez la fin du déploiement.

1. Après le déploiement, ouvrez l’URL de votre Boutique dans un navigateur web.

   Avec un domaine unique, utilisez le format suivant : `http://<magento-run-code>.<site-URL>`

   Par exemple, `http://french.master-name-projectID.us.magentosite.cloud/`

   Avec un domaine partagé, utilisez le format suivant : `http://<site-URL>/<magento-run-code>`

   Par exemple, `http://master-name-projectID.us.magentosite.cloud/french/`

1. Testez minutieusement votre site et fusionnez le code dans la branche `integration` pour un déploiement ultérieur.

## Déploiement dans les environnements d’évaluation et de production

Suivez le processus de déploiement [déploiement dans les environnements d’évaluation et de production](../deploy/staging-production.md). Pour les environnements Starter et Pro, vous utilisez le [!DNL Cloud Console] pour transmettre le code entre les environnements.

Adobe recommande d’effectuer des tests complets dans l’environnement d’évaluation avant d’effectuer le transfert vers l’environnement de production. Apportez des modifications de code dans l’environnement d’intégration et recommencez le processus de déploiement dans les environnements.

