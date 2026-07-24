---
source-git-commit: 3d3df68ab5e1781974348190b30372c0b8169d1d
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---
# Points d’extension de pré-validation pour l’optimisation des images

Ce répertoire contient des hooks de prévalidation qui optimisent automatiquement les images avant qu’elles ne soient validées dans le référentiel.

## Ce que font les crochets

- **Détection automatique** fichiers image intermédiaires (`.png`, `.jpeg`, `.jpg`, `.gif`, `.svg`)
- **Exécutez des`image_optim`** pour compresser et optimiser les images pixellisées (`.png`, `.jpeg`, `.jpg`, `.gif`)
- **Réorganiser automatiquement les images optimisées**
- **Assurez-vous que toutes les images pixellisées validées** sont correctement optimisées
- **Vérifiez les SVG intermédiaires par rapport à une limite de taille et abandonnez la validation si un SVG surdimensionné est référencé à partir de n’importe quel fichier dans `help/` (sinon, avertissez simplement).**

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

Checking images ...
path/to/your/image.png    100.00%
Pre-commit image checks complete!
```

### Tests unitaires

La logique de détection des liens SVG du hook (qui décide si un SVG surdimensionné est référencé à partir de `help/`) est couverte par des tests unitaires qui n’ont besoin que des `minitest` groupées de Ruby, sans gems ni configuration `_jekyll` :

```bash
ruby .githooks/test/svg_link_checker_test.rb
```

## Instructions relatives aux images

- **PNG** : à utiliser pour les captures d’écran et les éléments d’interface utilisateur (sera optimisé automatiquement)
- **** : utiliser pour les photos (sera optimisé automatiquement)
- **** : à utiliser pour les animations (sera optimisé automatiquement)
- **SVG** : à utiliser pour les icônes et les graphiques simples (non optimisés, mais vérifiés par rapport à une limite de taille ; la validation échoue uniquement si le SVG surdimensionné est lié par `help/`)

Les hooks de pré-validation optimisent automatiquement les images `.png`, `.jpeg`/`.jpg` et `.gif` lors de la validation, et vérifient les SVG intermédiaires par rapport à une limite de taille (140 Ko).

Si un SVG intermédiaire dépasse la limite et est référencé à partir d’un fichier dans `help/`, la validation est abandonnée. Si le SVG surdimensionné n’est référencé nulle part dans `help/`, le hook imprime uniquement un avertissement et la validation se poursuit. Convertissez plutôt les SVG surdimensionnés en PNG :

```bash
cd _jekyll
bundle exec rake images:svg_to_png path=../help/assets/image.svg
```

Le chemin étant relatif à `_jekyll`, les images sous `help/` sont référencées comme `../help/...`.

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
- Vérifiez que le `adobe-comdox-exl-rake-tasks` gem est installé (fournit les tâches `images:optimize`, `images:check_size` et `images:svg_to_png` que le hook exécute)
- Vérifier le fichier de configuration `.image_optim.yml`

### SVG dépasse la limite de taille.

- La validation est abandonnée si un SVG intermédiaire dépasse 140 Ko et est référencé à partir d’un fichier dans `help/` (sinon le hook n’affiche qu’un avertissement et la validation se poursuit)
- Convertissez le SVG en PNG : `cd _jekyll && bundle exec rake images:svg_to_png path=../help/assets/image.svg` (le chemin d’accès est relatif à `_jekyll`, de sorte que les images sous `help/` sont référencées comme `../help/...`)
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
6. **La validation se poursuit** : si l’optimisation réussit et qu’aucun SVG surdimensionné n’est référencé depuis `help/`, la validation se poursuit normalement, sinon elle est abandonnée (un SVG surdimensionné non référencé depuis `help/` déclenche uniquement un avertissement)

## Formats d’image pris en charge

- **PNG** (`.png`) - Compression sans perte et avec perte
- **** (`.jpg`, `.jpeg`) - Compression avec perte avec nettoyage des métadonnées
- **** (`.gif`) - Animation et optimisation statique
- **** (`.svg`) - Non optimisé (validation en l’état pour préserver la qualité), mais vérifié par rapport à une taille limite de 140 Ko ; la validation est abandonnée si la limite est dépassée et le SVG est référencé à partir de `help/` (sinon le hook affiche uniquement un avertissement).

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
