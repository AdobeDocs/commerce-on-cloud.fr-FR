---
source-git-commit: 7c3f2965f89503cc94a31aabca3d317a9330a4f6
workflow-type: tm+mt
source-wordcount: '565'
ht-degree: 0%

---
# Points d’extension de pré-validation pour l’optimisation des images

Ce répertoire contient des hooks de prévalidation qui optimisent automatiquement les images avant qu’elles ne soient validées dans le référentiel.

## Ce que font les crochets

- **Détection automatique** fichiers image intermédiaires (PNG, JPEG, GIF, SVG)
- **Exécutez des`image_optim`** pour compresser et optimiser les images pixellisées (PNG, JPEG, GIF).
- **Réorganiser automatiquement les images optimisées**
- **Assurez-vous que toutes les images pixellisées validées** sont correctement optimisées
- **Vérifiez les SVG intermédiaires** par rapport à une limite de taille et abandonnez la validation si un SVG la dépasse.

## Avantages

- Taille réduite du référentiel
- Chargement plus rapide des pages pour la documentation
- Qualité d’image cohérente pour tous les contributeurs et contributrices
- Aucune optimisation manuelle requise

## Conditions préalables

- Ruby 3.0 ou version ultérieure
- Bundler
- Git

## Configuration

### Configuration automatique (recommandée)

```bash
.githooks/setup-hooks.sh
```

### Configuration manuelle

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### Terminer la configuration du projet

1. Clonez le référentiel :

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. Activez les hooks de prévalidation :

   ```bash
   .githooks/setup-hooks.sh
   ```

3. Installez les dépendances Jekyll :

   ```bash
   cd _jekyll
   bundle install
   ```

## Test des points d’extension

1. Ajouter un fichier image au référentiel
2. Préparez-le : `git add <image-file>`
3. Essayer de valider : `git commit -m 'test'`
4. Le hook doit automatiquement optimiser l’image

### Sortie attendue

```bash
Found 1 staged image(s). Running optimization...
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## Instructions relatives aux images

- **PNG** : à utiliser pour les captures d’écran et les éléments d’interface utilisateur (sera optimisé automatiquement)
- **** : utiliser pour les photos (sera optimisé automatiquement)
- **** : à utiliser pour les animations (sera optimisé automatiquement)
- **** : à utiliser pour les icônes et les graphiques simples (non optimisés, mais vérifiés par rapport à une limite de taille ; la validation échoue si la limite est dépassée)

Les hooks de prévalidation optimisent automatiquement les images PNG, JPEG et GIF lors de la validation et comparent les SVG intermédiaires à une taille maximale (140 Ko).

Si un SVG intermédiaire dépasse la limite, la validation est abandonnée. Convertissez-le plutôt en PNG :

```bash
cd _jekyll
bundle exec rake images:svg_to_png path=path/to/image.svg
```

## Optimisation manuelle

Pour l’optimisation manuelle des images :

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## Configuration

Les points d’extension utilisent le fichier de configuration `_jekyll/.image_optim.yml` pour personnaliser les paramètres d’optimisation :

- **PNG** : utilise `advpng`, `optipng` et `pngquant`
- **** : utilise `jhead`, `jpegoptim` et `jpegtran`
- **** : Utilise `gifsicle`
- **SVG** : non optimisé (exclu de `image_optim` pour conserver les graphiques et animations vectoriels), mais vérifié par rapport à une taille limite de 140 Ko

## Dépannage

### Crochet non actif

- Vérifier la configuration du crochet : `git config core.hooksPath`
- Assurez-vous que le fichier de hook est exécutable : `chmod +x .githooks/pre-commit`
- Vérifiez que vous vous trouvez dans le référentiel correct avec le répertoire `_jekyll`

### Échecs d’optimisation

- Vérifiez `bundle install` a été exécuté dans le répertoire `_jekyll`
- Vérifiez que le `adobe-comdox-exl-rake-tasks` gem est installé (fournit des `image_optim`)
- Vérifier le fichier de configuration `.image_optim.yml`

### SVG dépasse la limite de taille.

- La validation est abandonnée si un SVG intermédiaire dépasse 140 Ko
- Convertir le SVG en PNG : `cd _jekyll && bundle exec rake images:svg_to_png path=path/to/image.svg`
- Ensuite, préparez le fichier PNG à la place du SVG et validez à nouveau

### Problèmes de performances

- Ajuster le nombre de threads dans `_jekyll/.image_optim.yml`
- Définir `DEBUG=1` variable d’environnement pour obtenir des informations détaillées sur les erreurs

## Fonctionnement

1. **Déclencheur de pré-validation** : lorsque vous exécutez `git commit`, le hook s’exécute automatiquement
2. **Détection d’images** : recherche les extensions d’image dans les fichiers intermédiaires
3. **Optimisation** : s’exécute `image_optim` sur chaque fichier PNG, JPEG ou GIF intermédiaire
4. **Réévaluation** : ajoute automatiquement les images optimisées à la zone d’évaluation
5. **Vérification de la taille de SVG** : vérifie la taille maximale de 140 Ko de chaque SVG intermédiaire
6. **La validation se poursuit** : si l’optimisation réussit et qu’aucun SVG ne dépasse la limite de taille, la validation se poursuit normalement, sinon la validation est abandonnée

## Formats d’image pris en charge

- **PNG** (`.png`) - Compression sans perte et avec perte
- **** (`.jpg`, `.jpeg`) - Compression avec perte avec nettoyage des métadonnées
- **** (`.gif`) - Animation et optimisation statique
- **** (`.svg`) - Non optimisé (validation en l’état pour préserver la qualité), mais vérifié par rapport à une taille limite de 140 Ko ; la validation est abandonnée si la limite est dépassée.

## Bonnes pratiques

1. **Tester le hook** : essayez d’abord de valider une petite image pour vous assurer qu’elle fonctionne
2. **Vérifier les modifications** : vérifiez la comparaison Git pour voir les résultats de l’optimisation.
3. **Surveillance des performances** : l’optimisation des images volumineuses peut prendre du temps
4. **Contrôle de version** : les hooks sont stockés dans ce répertoire `.githooks/`

## Support technique

Pour les problèmes liés aux hooks de prévalidation :

1. Vérifier la sortie du hook pour les messages d’erreur
2. Vérifier que la configuration de votre `image_optim` fonctionne
3. Testez d’abord les tâches de râtelage manuel.
4. Examiner les journaux et la configuration des points d’extension
5. Vérifiez la configuration du hook : `git config core.hooksPath`
