---
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---
# Fichiers de bibliothèque Rake

Ce répertoire contient les définitions de tâches principales organisées par fonctionnalité. Rake charge automatiquement tous les fichiers `.rake` de ce répertoire.

## Organisation des fichiers

### `includes.rake`

Contient toutes les tâches rake liées à l’inclusion sous l’espace de noms `:includes` :

- `includes:maintain_relationships` - Découvrir et maintenir les relations d’inclusion
- `includes:maintain_timestamps` - Ajouter/mettre à jour des horodatages en fonction des modifications apportées au fichier d’inclusion
- `includes:maintain_all` - Exécuter les deux opérations en séquence

## Fonctionnement

Rake détecte et charge automatiquement tous les fichiers `.rake` dans le répertoire `rakelib/`. Cela nous permet de :

1. **Organiser les tâches par fonctionnalité** - Regrouper les tâches associées
2. **Gardez le fichier principal propre** - Concentrez-vous sur les tâches du projet principal
3. **Ajouter facilement de nouveaux groupes de tâches** - Créez simplement de nouveaux fichiers `.rake`
4. **Maintenir la séparation des préoccupations** - Chaque fichier gère un domaine spécifique

## Ajout de nouveaux groupes de tâches

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
