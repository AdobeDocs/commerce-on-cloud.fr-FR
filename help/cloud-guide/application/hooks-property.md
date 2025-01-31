---
title: Propriété Hooks
description: Consultez des exemples sur la façon de configurer la propriété hooks dans le fichier  [!DNL Commerce]  configuration de l’application .
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Propriété Hooks

Utilisez la section `hooks` pour exécuter des commandes shell au cours des phases de création, de déploiement et de post-déploiement :

- **`build`** : exécutez les commandes _avant_ de conditionner votre application. Les services tels que la base de données ou Redis ne sont pas disponibles, car l’application n’a pas encore été déployée. Ajoutez des commandes personnalisées _avant_ la commande de `php ./vendor/bin/ece-tools` par défaut afin que le contenu personnalisé se poursuive jusqu’à la phase de déploiement.

- **`deploy`** : exécutez des commandes _après_ le regroupement et le déploiement de votre application. Vous pouvez accéder à d’autres services à ce stade. Comme la commande `php ./vendor/bin/ece-tools` par défaut copie le répertoire `app/etc` à l’emplacement correct, vous devez ajouter des commandes personnalisées _après_ la commande de déploiement pour empêcher les commandes personnalisées d’échouer.

- **`post_deploy`** : exécutez les commandes _après_ le déploiement de votre application et _après_ le conteneur commence à accepter les connexions. Le hook `post_deploy` efface le cache et précharge (réchauffe) le cache. Vous pouvez personnaliser la liste des pages à l’aide de la variable `WARM_UP_PAGES` dans l’étape [Post-déploiement](../environment/variables-post-deploy.md). Bien que cela ne soit pas obligatoire, cela fonctionne en tandem avec la variable d’environnement `SCD_ON_DEMAND`.

L’exemple suivant illustre la configuration par défaut dans le fichier `.magento.app.yaml`. Ajoutez des commandes d’interface de ligne de commande sous les sections `build`, `deploy` ou `post_deploy` _avant_ la commande `ece-tools` :

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

En outre, vous pouvez personnaliser davantage la phase de création à l’aide des commandes `generate` et `transfer` pour effectuer des actions supplémentaires lors de la création de code ou du déplacement de fichiers.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` : fait échouer les points d&#39;extension sur la première commande ayant échoué, au lieu de la dernière commande ayant échoué.
- `build:generate` : applique des correctifs, valide la configuration, génère des identifiants et génère du contenu statique si SCD est activé pour la phase de création.
- `build:transfer` : transfère le code généré et le contenu statique vers la destination finale.

Les commandes s’exécutent à partir du répertoire de l’application (`/app`). Vous pouvez utiliser la commande `cd` pour modifier le répertoire. Les points d’extension échouent si la commande finale qu’ils contiennent échoue. Pour provoquer leur échec lors de la première commande ayant échoué, ajoutez `set -e` au début du hook.

**Pour compiler des fichiers Sass à l’aide de grunt** :

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Compilez les fichiers Sass à l’aide de `grunt` avant le déploiement du contenu statique, qui se produit pendant la génération. Placez la commande `grunt` avant la commande `build`.

{{scenarios}}
