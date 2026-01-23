---
title: Ajout de la carte du site et des robots de moteurs de recherche
description: Découvrez comment ajouter des robots de carte de site et de moteur de recherche à Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1d52481fb6874f3a9ba14b0ff4fe39dc7d564938
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# Ajout de la carte du site et des robots de moteurs de recherche

Toute tentative de génération et d’écriture du fichier `sitemap.xml` dans le répertoire racine génère l’erreur suivante :

```
Please make sure that "/" is writable by the web-server.
```

Avec Adobe Commerce sur l’infrastructure cloud, vous ne pouvez écrire que dans des répertoires spécifiques, tels que `var`, `pub/media`, `pub/static` ou `app/etc`. Lorsque vous générez le fichier `sitemap.xml` à l’aide du panneau d’administration, vous devez spécifier le chemin d’accès `/media/`.

Il n’est pas nécessaire de générer un fichier `robots.txt`, car il génère le contenu `robots.txt` à la demande et le stocke dans la base de données. Vous pouvez afficher le contenu dans votre navigateur avec le lien `<domain.your.project>/robots.txt` ou `<domain.your.project>/robots` .

Cela nécessite ECE-Tools version 2002.0.12 et ultérieure avec un fichier `.magento.app.yaml` mis à jour. Consultez un exemple de ces règles dans le [référentiel magento-cloud](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Pour générer un fichier `sitemap.xml` dans les versions 2.2 et ultérieures** :

1. Accédez à l’administrateur.
1. Dans le menu _Marketing_, cliquez sur **Plan du site** dans la section _SEO et recherche_.
1. Dans la vue _Plan du site_, cliquez sur **Ajouter un plan de site**.
1. Dans la vue _Nouvelle carte du site_, saisissez les valeurs suivantes :

   - **Filename**:`sitemap.xml`
   - **Path**:`/media/`

1. Cliquez sur **Enregistrer et générer**. Le nouveau plan du site est alors disponible dans la grille _Plan du site_.
1. Cliquez sur le chemin d’accès dans la colonne _Lien vers Google_.

**Pour ajouter du contenu au fichier `robots.txt`** :

1. Accédez à l’administrateur.
1. Dans le menu _Contenu_, cliquez sur **Configuration** dans la section _Conception_.
1. En mode _Configuration de la conception_, cliquez sur **Modifier** pour le site web dans la colonne _Action_.
1. Dans la vue _Site web principal_, cliquez sur **Robots de moteurs de recherche**.
1. Mettez à jour le champ **Modifier l’instruction personnalisée du fichier robots.txt**.
1. Cliquez sur **Enregistrer la configuration**.
1. Vérifiez le fichier `<domain.your.project>/robots.txt` ou `<domain.your.project>/robots` URL dans votre navigateur.

>[!NOTE]
>
>Si le fichier `<domain.your.project>/robots.txt` génère un `404 error`, [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour supprimer la redirection de `/robots.txt` vers `/media/robots.txt`.

## Réécrire à l’aide du fragment de code VCL Fastly

Si vous disposez de domaines différents et que vous avez besoin de plans de site distincts, vous pouvez créer un VCL pour acheminer vers le plan de site approprié. Générez le fichier `sitemap.xml` dans le panneau d’administration comme décrit ci-dessus, puis créez un fragment de code VCL Fastly personnalisé pour gérer la redirection. Voir [Extraits personnalisés Fastly VCL](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Vous pouvez charger des fragments de code VCL personnalisés depuis l’interface utilisateur d’administration ou à l’aide de l’API Fastly. Voir [Exemples de fragments de code VCL personnalisés et tutoriels](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Utiliser un fragment de code VCL Fastly pour la redirection

Créez un fragment de code VCL personnalisé pour réécrire le chemin d’accès pour `sitemap.xml` à `/media/sitemap.xml` à l’aide des paires clé-valeur `type` et `content`.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

L’exemple suivant montre comment réécrire le chemin d’accès de `robots.txt` et `sitemap.xml` vers `/media/robots.txt` et `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Pour utiliser un fragment de code VCL Fastly pour une redirection de domaine spécifique** :

Créez un fichier `pub/media/domain_robots.txt`, où le domaine est `domain.com`, et utilisez le fragment de code VCL suivant :

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

Le fragment de code VCL achemine `http://domain.com/robots.txt` et présente le fichier `pub/media/domain_robots.txt`.

Pour configurer une redirection pour `robots.txt` et `sitemap.xml` dans un seul fragment de code, créez des fichiers `pub/media/domain_robots.txt` et `pub/media/domain_sitemap.xml`, où le domaine est `domain.com`, et utilisez le fragment de code VCL suivant :

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

Dans la configuration d’administration `sitemap`, vous devez spécifier l’emplacement du fichier à l’aide de `pub/media/` plutôt que de `/`.

### Configuration de l’indexation par moteur de recherche

Pour activer `robots.txt` personnalisations dans l’environnement de production, activez l’indexation par les moteurs de recherche pour l’option `<environment-name>` dans les paramètres du projet sur la console cloud :

- Console cloud héritée : l’URL suit le modèle `https://<region-id>.magento.cloud/projects/<project_id>`

  Définissez le paramètre [!UICONTROL Indexing by search engines] (console héritée) [!UICONTROL Hide from search engines] (console Adobe) sur **Activé**.

  ![Utilisez l’[!DNL Cloud Console] pour gérer les environnements](../../assets/robots-indexing-by-search-engine.png)

- Console Adobe Cloud : l’URL suit le modèle ``https://console.adobecommerce.com/<username>/<project_id>``

  Décochez l’[!UICONTROL Hide from search engines] Paramètre .

- Vous pouvez également utiliser l’interface de ligne de commande magento-cloud pour mettre à jour ce paramètre :

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- L’indexation par les moteurs de recherche ne peut être activée qu’en production, mais pas dans les environnements inférieurs.
>
>- Si vous utilisez PWA Studio Placer sur la liste autorisée et que vous ne pouvez pas accéder à votre fichier `robots.txt` configuré, ajoutez `robots.txt` à [Nom de front](https://github.com/magento/magento2-upward-connector#front-name-allowlist) sous **Magasins** > Configuration > **Général** > **Web** > Configuration UPWARD PWA.

