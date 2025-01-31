---
title: Structure du projet
description: Découvrez la structure de fichiers et les modèles de projet pour Adobe Commerce sur l’infrastructure cloud.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Structure du projet

Un projet d’infrastructure cloud Adobe Commerce comprend des fichiers essentiels pour les informations d’identification et la configuration de l’application. Ces fichiers sont disponibles dans comme modèle selon la version d’Adobe Commerce. Consultez les modèles cloud basés sur la version d’Adobe Commerce dans le référentiel GitHub [`magento/magento-cloud`](https://github.com/magento/magento-cloud).

Le tableau suivant décrit les fichiers inclus dans un projet cloud :

| Fichier | Description |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Fichier de configuration qui redirige le `www` vers le domaine apex et `php` application vers le HTTP. Voir [Configurer les itinéraires](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | Un fichier de configuration qui définit une instance MySQL (MariaDB), Redis et OpenSearch ou Elasticsearch. Voir [ Configuration des services ](../services/services-yaml.md). |
| `/app` | Le dossier `code` est utilisé pour les modules personnalisés. Le dossier `design` est utilisé pour les [thèmes personnalisés](../store/custom-theme.md). Le dossier `etc` contient les fichiers de configuration de l’application. |
| `/m2-hotfixes` | Utilisé pour les correctifs personnalisés. |
| `/update` | Dossier de service utilisé par le module de support. |
| `.gitignore` | Spécifiez les fichiers et répertoires à ignorer. Voir [`.gitignore` référence](#ignoring-files). |
| `.magento.app.yaml` | Un fichier de configuration qui définit les propriétés pour créer votre application. Voir [Configurer l’application](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Fichier de configuration pour les phases de build, de déploiement et de post-déploiement. Le package `ece-tools` comprend un exemple de ce fichier. Voir [Configuration des environnements](../environment/configure-env-yaml.md). |
| `composer.json` | Récupère Adobe Commerce et les scripts de configuration pour préparer votre application. Voir [Packages requis](../development/overview.md#required-packages). |
| `composer.lock` | Stocke les dépendances de version pour chaque package. Voir [Packages requis](../development/overview.md#required-packages). |
| `magento-vars.php` | Utilisé pour définir [plusieurs magasins](../store/multiple-sites.md) et sites à l’aide de variables. |

{style="table-layout:auto"}

>[!NOTE]
>
>Lorsque vous poussez vos modifications locales vers le serveur distant, le script de déploiement utilise les valeurs définies par les fichiers de configuration dans le répertoire `.magento`, puis le script supprime le répertoire et son contenu. Votre environnement de développement local n’est pas affecté.

## Répertoire racine de l&#39;application

L’emplacement du répertoire racine de l’application dépend de l’environnement.

- **Intégration Starter et Pro** : `/app`
- **Démarrer la production** : `/<project-ID>`
- **Évaluation Pro** : `/<project-ID>_stg`
- **Pro Production** : `/<project-ID>`

### Répertoires modifiables

Les environnements distants d’intégration, d’évaluation et de production sont en lecture seule. Pour des raisons de sécurité&#x200B;*les répertoires suivants sont les* seuls modifiables :

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>Dans les environnements de production et d’évaluation, chaque nœud du cluster à trois nœuds possède un répertoire `/tmp` qui n’est pas partagé avec les autres nœuds.

## Ignorer les fichiers

Il existe un fichier de `.gitignore` de base avec le référentiel de projet d’infrastructure cloud d’Adobe Commerce. Voir le dernier fichier [.gitignore dans le référentiel magento-cloud](https://github.com/magento/magento-cloud/blob/master/.gitignore). Pour ajouter un fichier qui se trouve dans la liste de `.gitignore`, vous pouvez utiliser l’option `-f` (forcer) lors de l’évaluation d’une validation :

```bash
git add <path/filename> -f
```

## Modifier le modèle de base

Vous pouvez suivre les étapes ci-après pour modifier la structure d’un projet existant afin de refléter le dernier modèle de base d’Adobe Commerce sur l’infrastructure cloud.

1. Clonez le projet sur une station de travail locale.

1. Mettez à jour le fichier `composer.json` avec les valeurs suivantes pour la section `extra`.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Ajoutez le fichier `.gitignore` conçu pour le modèle de base. Par exemple, si vous avez besoin du fichier `.gitignore` pour le modèle de la version 2.2.6, utilisez le fichier [.gitignore pour la version 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) comme référence.

1. Effacez le cache Git.

   ```bash
   git rm -r --cached .
   ```

1. Ajoutez et validez les modifications.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
