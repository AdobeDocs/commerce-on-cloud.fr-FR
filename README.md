---
source-git-commit: 8cbda8ca194c5e5865073c9eb08e061cfecb5ace
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 1%

---
# Adobe Commerce sur les infrastructures cloud

Ce site contient la documentation la plus récente pour les développeurs de Commerce sur les infrastructures cloud.

- [Guide de Commerce sur les infrastructures cloud](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/overview)
- [Prise en main de Commerce](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/start/overview) sur les infrastructures cloud

## Code de conduite d’Adobe Open Source

Ce projet a adopté le [code de conduite d’Adobe Open Source](code-of-conduct.md). Pour plus d’informations, consultez l’article [Contribution](contributing.md).

## À propos de vos contributions au contenu d’Adobe

Consultez le Guide du contributeur aux documents Adobe [&#128279;](https://experienceleague.adobe.com/fr/docs/contributor/contributor-guide/introduction).

La façon dont vous contribuez dépend de qui vous êtes et du type de modifications que vous souhaitez apporter :

### Modifications mineures

Si vous contribuez à des mises à jour mineures, consultez l’article et cliquez sur la zone de commentaires qui s’affiche au bas de l’article, cliquez sur **Options de commentaires détaillées**, puis cliquez sur **Suggérer une modification** pour accéder au fichier source Markdown sur GitHub. Utilisez l’interface utilisateur GitHub pour effectuer vos mises à jour.

Les modifications ou précisions mineures que vous apportez aux documents et aux exemples de code dans ce référentiel sont soumises aux conditions d’utilisation d’Adobe.

### Modifications majeures ou nouveaux articles des membres de la communauté

Si vous faites partie de la communauté Adobe et que vous souhaitez rédiger un nouvel article ou apporter des modifications majeures, utilisez l’onglet Problèmes du référentiel Git pour soumettre un problème et commencer une conversation avec l’équipe de documentation. Une fois que vous avez accepté un plan, vous devez travailler avec un employé pour vous aider à introduire ce nouveau contenu en utilisant à la fois les référentiels publics et privés.

### Modifications majeures apportées par les employés d’Adobe

Si vous êtes rédacteur technique, responsable de programme ou développeur au sein de l’équipe produit d’une solution Adobe Experience Cloud et qu’il vous incombe de contribuer ou de rédiger des articles techniques, vous devez utiliser le référentiel privé à l’adresse `https://git.corp.adobe.com/AdobeDocs`.

## Outils et configuration

Les contributeurs de la communauté peuvent utiliser l’interface utilisateur de GitHub pour apporter des modifications mineures, ou dupliquer le référentiel pour apporter des contributions majeures.

Pour plus d’informations, consultez le Guide du contributeur aux documents Adobe [&#128279;](https://experienceleague.adobe.com/fr/docs/contributor/contributor-guide/introduction).

## Comment utiliser Markdown pour formater votre rubrique

Tous les articles de ce référentiel utilisent GitHub Flavored Markdown. Si vous ne connaissez pas Markdown, voir :

- [Principes de base de Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [Aide-mémoire imprimable Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## Modèles

Pour certaines rubriques, nous utilisons des fichiers de données et des modèles pour générer du contenu publié. Les cas d’utilisation de cette approche sont les suivants :

- Publication de grands ensembles de contenu généré par programmation
- Fournir une source unique de vérité aux clients sur plusieurs systèmes qui nécessitent des formats de fichiers lisibles par machine, tels que YAML, pour l’intégration (par exemple, outils d’interface de ligne de commande cloud, configurations de service)

Voici quelques exemples de contenu modélisé, sans s’y limiter :

- [Référence de l’interface de ligne de commande Cloud](help/templated/cloud-cli-ref.md)
- [Packages cloud](help/templated/cloud-packages.md)
- [Référence des outils CEE](help/templated/ece-tools.md)
- [Extensions PHP pour le cloud](help/templated/php-extensions-cloud.md)

### Générer le contenu modélisé

En règle générale, la plupart des auteurs et des autrices n’ont besoin d’ajouter qu’une version aux tableaux Disponibilité du produit et Configuration requise . La maintenance de tous les autres contenus modélisés est automatisée ou gérée par un membre de l’équipe dédié. Ces instructions sont destinées à la plupart des rédacteurs.

>**REMARQUE :**
>
>- La génération de contenu modélisé nécessite de travailler sur la ligne de commande dans un terminal.
>- Ruby doit être installé pour exécuter le script de rendu. Voir [_jekyll/.ruby-version] (_jekyll/.ruby-version) pour la version requise.

Pour obtenir une description de la structure de fichiers pour le contenu modélisé, reportez-vous aux sections suivantes :

- `_jekyll` : contient des rubriques modélisées et les ressources requises.
- `_jekyll/_data` : contient les formats de fichiers lisibles par machine utilisés pour le rendu des modèles
- `_jekyll/templated` : contient les fichiers de modèle basés sur HTML utilisant le langage de modèle Liquid
- `help/_includes/templated` : contient la sortie générée pour le contenu modélisé au format de fichier `.md` afin qu&#39;il puisse être publié dans les rubriques Experience League ; le script de rendu écrit automatiquement la sortie générée dans ce répertoire pour vous

Pour mettre à jour le contenu modélisé :

1. Dans l’éditeur de texte, ouvrez un fichier de données dans le répertoire `_jekyll/_data`. Par exemple :

   - [Référence de l’interface de ligne de commande Cloud](help/templated/cloud-cli-ref.md) : `_jekyll/_data/cloud-cli-ref.yaml`
   - [Packages cloud](help/templated/cloud-packages.md) : `_jekyll/_data/cloud-packages.yaml`
   - [Référence des outils CEE](help/templated/ece-tools.md) : `_jekyll/_data/ece-tools.yaml`

2. Utilisez la structure YAML existante pour créer des entrées.

3. Accédez au répertoire `_jekyll`.

   ```bash
   cd _jekyll
   ```

4. Générez le contenu modélisé et écrivez la sortie dans le répertoire `help/_includes/templated`.

   ```bash
   bundle exec rake render
   ```

   >**REMARQUE :** vous devez exécuter le script à partir du répertoire `_jekyll`. Si c’est la première fois que vous exécutez le script, vous devez d’abord installer les dépendances Ruby avec la commande `bundle install`. Les tâches et dépendances principales de Rake (Jekyll, Rake, optimisation des images) sont fournies par le `adobe-comdox-exl-rake-tasks` gem pour une meilleure maintenabilité dans les référentiels de documentation Adobe Commerce. Les tâches personnalisées spécifiques à ce référentiel sont implémentées dans le `Rakefile`.

5. Revenez au répertoire `root`.

   ```bash
   cd ..
   ```

6. Vérifiez que les fichiers `help/_includes/templated` attendus ont été modifiés.

   ```bash
   git status
   ```

   Vous devriez voir une sortie similaire à ce qui suit :

   ```bash
   modified:   _data/cloud-cli-ref.yaml
   modified:   help/_includes/templated/cloud-cli-ref.md
   ```

7. Envoyez vos modifications.

   ```bash
   git add .
   git commit -m "descriptive message of the intended commit"
   git push
   ```

Consultez la documentation Jekyll pour plus d’informations sur [Fichiers de données](https://jekyllrb.com/docs/datafiles), [Filtres liquides](https://jekyllrb.com/docs/liquid/filters/) et d’autres fonctionnalités.

## Tâches de classement disponibles

Ce référentiel utilise les tâches rake fournies par `adobe-comdox-exl-rake-tasks` gem. Pour afficher toutes les tâches disponibles, exécutez :

```bash
cd _jekyll
bundle exec rake --tasks
```

## Points d’extension de pré-validation pour l’optimisation des images

Ce référentiel comprend des hooks de prévalidation automatisés qui optimisent les images avant validation. **Tous les contributeurs doivent activer ces raccordements** afin d’assurer une optimisation cohérente des images et une taille de référentiel réduite.

### Configuration rapide

Après avoir cloné le référentiel, exécutez :

```bash
.githooks/setup-hooks.sh
```

### Ce que font les crochets

- Détecter automatiquement les fichiers image intermédiaires (PNG, JPG, JPEG, GIF, SVG)
- Exécutez `image_optim` pour compresser et optimiser les images.
- Réévaluation automatique des images optimisées
- Vérifiez que toutes les images validées sont correctement optimisées.

### Avantages

- Taille réduite du référentiel
- Chargement plus rapide des pages pour la documentation
- Qualité d’image cohérente pour tous les contributeurs et contributrices
- Aucune optimisation manuelle requise

Pour obtenir des instructions détaillées sur la configuration, le dépannage et la configuration, voir [`.githooks/README.md`](.githooks/README.md).
