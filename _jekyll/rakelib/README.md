---
source-git-commit: f75dbbc9726ff21a648b1bb3d8f06cb45a5d6b08
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---
# Fichiers de bibliothèque Rake

Ce répertoire contient les définitions de tâches principales organisées par fonctionnalité. Rake charge automatiquement tous les fichiers `.rake` de ce répertoire.

## Organisation des fichiers

### `adobe-docs-tasks.rake`

Contient des tâches requises courantes, des fonctionnalités partagées et des tâches sans espace de noms pour le référentiel de documentation d’Adobe Commerce sur Experience League. Effectuez les tâches suivantes :

- `whatsnew` - Générer des données pour le résumé des actualités (par défaut : depuis la dernière mise à jour)
- `render` - Générer des fichiers modélisés et gérer les inclusions

### `includes.rake`

Contient des tâches de gestion des inclusions organisées dans l&#39;espace de noms `:includes` :

- `includes:maintain_relationships` - Découvrir et gérer les relations d’inclusion dans les fichiers Markdown
- `includes:maintain_timestamps` - Ajouter/mettre à jour des horodatages en fonction des modifications apportées au fichier d’inclusion
- `includes:maintain_all` - Exécuter les deux opérations en séquence
- `includes:unused` - Rechercher les fichiers d’inclusion inutilisés

### `images.rake`

Contient des tâches de gestion des images organisées dans l&#39;espace de noms `:images` :

- `images:optimize` - Optimisation des images dans les fichiers non validés modifiés
- `images:unused` - Rechercher des images inutilisées dans le projet

## Fonctionnement

Rake détecte et charge automatiquement tous les fichiers `.rake` dans le répertoire `rakelib/`. Cela nous permet de :

1. **Organiser les tâches par fonctionnalité** - Regrouper les tâches associées
2. **Gardez le fichier principal propre** - Concentrez-vous sur les tâches du projet principal
3. **Ajouter facilement de nouveaux groupes de tâches** - Créez simplement de nouveaux fichiers `.rake`
4. **Maintenir la séparation des préoccupations** - Chaque fichier gère un domaine spécifique

## Ajouter de nouveaux groupes de tâches

Pour ajouter un nouveau groupe de tâches associées :

1. Créer un nouveau fichier `.rake` dans ce répertoire
2. Utilisez un espace de noms explicite (par exemple, `namespace :images` pour les tâches liées aux images).
3. Définir vos tâches dans l’espace de noms
4. Rake les chargera automatiquement

## Exemple de structure

```ruby
namespace :your_namespace do
  desc 'Description of what this task does'
  task :task_name do
    # Task implementation
  end
end
```

## Utilisation

Les tâches sont disponibles immédiatement après leur création :

```bash
rake your_namespace:task_name
```

## Avantages

- **Modularité** : chaque fichier se concentre sur un domaine spécifique
- **Maintenabilité** : plus facile de trouver et de modifier des groupes de tâches spécifiques
- **Évolutivité** : peut ajouter de nombreux groupes de tâches sans encombrer le Rakefile principal.
- **Développement d’équipe** : différents développeurs peuvent travailler sur différents groupes de tâches
