---
title: Variables post-déploiement
description: Consultez la liste des variables d’environnement qui contrôlent les actions dans la phase post-déploiement d’Adobe Commerce sur l’infrastructure cloud .
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variables post-déploiement

Les variables _post-déploiement_ suivantes contrôlent les actions dans la phase de post-déploiement et peuvent hériter et remplacer les valeurs de [variables globales](variables-global.md). Insérez ces variables dans l’étape `post-deploy` du fichier `.magento.env.yaml` :

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Pour plus d’informations sur la personnalisation du processus de création et de déploiement :

- [Configuration du déploiement](configure-env-yaml.md)
- [Processus de déploiement](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Par défaut**— `[]` (un tableau vide)
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Configurez le test _Temps jusqu’au premier octet_ (TTFB) pour les pages spécifiées afin de tester les performances de votre site. Spécifiez une référence de chemin d’accès absolue, ou URL avec protocole et hôte, pour chaque page qui nécessite le test.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Une fois que vous avez spécifié les pages à tester et validé vos modifications, le test _Délai jusqu’au premier octet_ s’exécute lors de la phase de post-déploiement et publie les résultats de chaque chemin dans le journal cloud :

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Pour les chemins redirigés, le journal signale le chemin de la cible de redirection au lieu de celui configuré dans la variable d’environnement . Si vous indiquez un chemin d’accès non valide, le journal affiche un message d’avertissement.

## `WARM_UP_CONCURRENCY`

- **Par défaut**—_Non défini_
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Spécifiez la limite de requêtes simultanées à envoyer pendant les opérations de préchauffage du cache pour réduire la charge du serveur. Cette valeur limite le nombre de connexions parallèles et est utile pour les configurations d’environnement dans lesquelles la variable post-déploiement `WARM_UP_PAGES` spécifie plusieurs pages pour le préchargement du cache.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Par défaut**— `index.php`
- **Version**—Adobe Commerce 2.1.4 et versions ultérieures

Personnalisez la liste des pages utilisées pour précharger le cache dans l’étape de `post_deploy`. Vous devez configurer le hook de post-déploiement. Voir la section [hooks](../application/hooks-property.md) du fichier `.magento.app.yaml`.

- **pages uniques** : spécifiez une page unique à ajouter au cache. Vous n’avez pas à indiquer l’URL de base par défaut. L’exemple suivant met en cache la page `BASE_URL/index.php` :

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **plusieurs domaines**—Répertoriez plusieurs URL. L’exemple suivant met en cache des pages de deux domaines :

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **plusieurs pages** : utilisez le format suivant pour mettre en cache plusieurs pages selon un modèle d&#39;expression régulière spécifique :

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type` : variantes possibles `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku` : utilisez un modèle de `regexp` ou un `url` de correspondance exacte pour filtrer les URL, ou utilisez un astérisque (\*) pour toutes les pages. Utiliser le sku du produit pour le type d’entité `product`
   - `store_id|store_code` : utilisez l’ID ou le code du magasin, ou un astérisque (\*) pour tous les magasins. Vous pouvez transmettre plusieurs ID ou codes de magasin séparés par des `|`

  L’exemple suivant met en cache pour les types d’entités `category` et `cms-page` en fonction de ces critères :
   - toutes les pages de catégories pour le magasin avec l’ID `1`
   - toutes les pages de catégories pour les magasins avec le code `store1` et `store2`
   - page de catégorie `cars` pour le magasin avec le code `store_en`
   - `contact` de page cms pour tous les magasins
   - `contact` de page cms pour les magasins avec ID `1` et `2`
   - toute page de catégorie contenant des `car_` et se terminant par `html` pour le magasin avec l’ID 2.
   - toute page de catégorie contenant des `tires_` à stocker avec le code `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  L’exemple de mise en cache suivant pour le type d’entité `product` en fonction de ces critères :
   - tous les produits pour tous les magasins (limité par programmation à 100 par magasin pour éviter des problèmes de performances)
   - tous les produits pour store `store1`
   - produits avec `sku1` pour tous les magasins
   - produits avec `sku1` pour les magasins avec le code `store1` et `store2`
   - produits avec `sku1`, `sku2` et `sku3` pour les magasins avec le code `store1` et `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  L’exemple de mise en cache suivant pour le type d’entité `store-page` en fonction de ces critères :
   - `/contact-us` de page pour tous les magasins
   - `/contact-us` de page pour le magasin avec l’ID `1`
   - `/contact-us` de page pour les magasins avec le code `code1` et `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
