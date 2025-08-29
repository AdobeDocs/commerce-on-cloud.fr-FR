---
source-git-commit: 5236f2718e5091dfc2fc140e15d4ac87a073d1d0
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---
# Points d’extension de pré-validation pour l’optimisation des images

Ce répertoire contient des hooks de prévalidation qui optimisent automatiquement les images avant qu’elles ne soient validées dans le référentiel.

## Ce que font les crochets

- **Détection automatique** fichiers image intermédiaires (PNG, JPG, JPEG, GIF, SVG)
- **Exécuter des`image_optim`** pour compresser et optimiser les images
- **Réorganiser automatiquement les images optimisées**
- **Vérifiez que toutes les images validées** sont correctement optimisées.

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
- **SVG** : à utiliser pour les icônes et les graphiques simples (optimisation désactivée par défaut)
- **JPEG** : utiliser pour les photos (sera optimisé automatiquement)
- **GIF** : à utiliser pour les animations (sera optimisé automatiquement)

Les hooks de pré-validation optimisent automatiquement toutes les images lors de la validation.

## Optimisation manuelle

Pour l’optimisation manuelle des images :

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## Configuration

Les points d’extension utilisent le fichier de configuration `_jekyll/.image_optim` pour personnaliser les paramètres d’optimisation :

- **PNG** : utilise `advpng`, `optipng` et `pngquant`
- **JPEG** : utilise `jhead`, `jpegoptim` et `jpegtran`
- **GIF** : Utilise `gifsicle`
- **SVG** : l&#39;optimisation du SVG est désactivée par défaut (peut rompre les graphiques et animations vectoriels complexes)

## Dépannage

### Crochet non actif

- Vérifier la configuration du crochet : `git config core.hooksPath`
- Assurez-vous que le fichier de hook est exécutable : `chmod +x .githooks/pre-commit`
- Vérifiez que vous vous trouvez dans le référentiel correct avec le répertoire `_jekyll`

### Échecs d’optimisation

- Vérifiez `bundle install` a été exécuté dans le répertoire `_jekyll`
- Vérifiez que `image_optim` et `image_optim_pack` gemmes sont installés
- Vérifier le fichier de configuration `.image_optim`

### Problèmes de performances

- Ajuster le nombre de threads dans `_jekyll/.image_optim`
- Définir `DEBUG=1` variable d’environnement pour obtenir des informations détaillées sur les erreurs

## Fonctionnement

1. **Déclencheur de pré-validation** : lorsque vous exécutez `git commit`, le hook s’exécute automatiquement
2. **Détection d’images** : recherche les extensions d’image dans les fichiers intermédiaires
3. **Optimisation** : s’exécute `image_optim` sur chaque image intermédiaire
4. **Réévaluation** : ajoute automatiquement les images optimisées à la zone d’évaluation
5. **La validation se poursuit** : si l’optimisation réussit, la validation se poursuit normalement

## Formats d’image pris en charge

- **PNG** (`.png`) - Compression sans perte et avec perte
- **JPEG** (`.jpg`, `.jpeg`) - Compression avec perte avec nettoyage des métadonnées
- **GIF** (`.gif`) - Animation et optimisation statique
- **SVG** (`.svg`) - Optimisation vectorielle (désactivée par défaut)

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
