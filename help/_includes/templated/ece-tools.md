---
source-git-commit: 9fbcca6f545276e7afedcdb6e6061a87dd8f2dd9
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 3%

---
# Outils de la CEE

**Version** : 2002.2.2

Cette référence contient 34 commandes disponibles via l’outil de ligne de commande `ece-tools`.
La liste initiale est générée automatiquement à l’aide de la commande `ece-tools list` dans Adobe Commerce sur l’infrastructure cloud.

## Général

Cette référence est générée à partir de la base de code de l’application. Pour modifier le contenu, _Donnez-nous votre avis_ (recherchez le lien en haut à droite). Pour obtenir des instructions sur les contributions, voir [Coder les contributions](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Options globales

#### `--help`, `-h`

Affichez l’aide pour la commande donnée. Lorsqu’aucune commande n’est fournie, afficher l’aide pour la commande de liste

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--quiet`, `-q`

Ne génère aucun message

- Faire défaut: `false`
- N’accepte aucune valeur

#### `--verbose`, `-v|-vv|-vvv`

Augmentez la verbosité des messages : 1 pour une sortie normale, 2 pour une sortie plus verbose et 3 pour le débogage

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--version`, `-V`

Afficher cette version de l&#39;application

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--ansi`

Forcer (ou désactiver —no-ansi) la sortie ANSI

- N’accepte aucune valeur

#### `--no-ansi`

Ignorer l’option « —ansi »

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-interaction`, `-n`

Ne posez aucune question interactive

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

Commande interne pour fournir des suggestions d&#39;achèvement du shell

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--shell`, `-s`

Le type de coque (« bash », « fish », « zsh »)

- Requiert une valeur

#### `--input`, `-i`

Tableau de jetons d’entrée (par exemple COMP_WORDS ou argv)

- Valeur par défaut : `[]`
- Nécessite une valeur

#### `--current`, `-c`

Index du tableau « input » dans lequel se trouve le curseur (par exemple COMP_CWORD)

- Requiert une valeur

#### `--api-version`, `-a`

Version API du script d&#39;achèvement

- Requiert une valeur

#### `--symfony`, `-S`

obsolète

- Requiert une valeur


## `build`

```bash
ece-tools build
```

Construit l’application.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

Vider le script de fin du shell

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### Arguments

#### `shell`

Le type de shell (par exemple « bash »), la valeur de la variable d&#39;environnement « $SHELL » sera utilisée si elle n&#39;est pas indiquée

### Options

Pour connaître les options globales, reportez-vous à la section [Options globales](#global-options).

#### `--debug`

Suivez le journal de débogage de fin

- Faire défaut: `false`
- N’accepte pas de valeur


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

Crée des sauvegardes de base de données.

### Arguments

#### `databases`

Bases de données pour la sauvegarde. Valeurs disponibles : [vente du devis principal]. Si la valeur de l’argument n’est pas spécifiée, les sauvegardes de la base de données sont créées à l’aide des informations d’identification stockées dans la variable d’environnement `MAGENTO_CLOUD_RELATIONSHIP` ou/et la propriété `stage.deploy.DATABASE_CONFIGURATION` du fichier de configuration .magento.env.yaml.

- Faire défaut: `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--remove-definers`, `-d`

Supprimer les définisseurs de l&#39;image mémoire de la base de données

- Faire défaut: `false`
- N’accepte pas de valeur

#### `--dump-directory`, `-a`

Utiliser un autre répertoire pour enregistrer le vidage

- Requiert une valeur


## `deploy`

```bash
ece-tools deploy
```

Déploie l’application.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

Affichage de l’aide d’une commande

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### Arguments

#### `command_name`

Nom de la commande

- Valeur par défaut : `help`

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--format`

Le format de sortie (txt, xml, json ou md)

- Valeur par défaut : `txt`
- Requiert une valeur

#### `--raw`

Pour générer l’aide de la commande brute

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

Commandes de liste

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### Arguments

#### `namespace`

Nom de l’espace de noms

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--raw`

Pour générer la liste de commandes brute

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--format`

Le format de sortie (txt, xml, json ou md)

- Valeur par défaut : `txt`
- Requiert une valeur

#### `--short`

Pour ignorer la description des arguments des commandes

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `patch`

```bash
ece-tools patch
```

Applique des correctifs personnalisés.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `post-deploy`

```bash
ece-tools post-deploy
```

Effectue après les opérations de déploiement.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `run`

```bash
ece-tools run <scenario>...
```

Exécuter scénario(s).

### Arguments

#### `scenario`

Scenario(s)

- Valeur par défaut : `[]`
- Obligatoire

- Tableau

### Options

Pour connaître les options globales, reportez-vous à la section [Options globales](#global-options).


## `backup:list`

```bash
ece-tools backup:list
```

Affiche la liste des fichiers de sauvegarde.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

Restaurez les fichiers de configuration importants. Exécutez backup:list pour afficher la liste des fichiers de sauvegarde.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--force`, `-f`

Ecraser des fichiers existants lors de la restauration d’une sauvegarde

- Faire défaut: `false`
- N’accepte aucune valeur

#### `--file`

Chemin de récupération de fichier spécifique

- Accepte une valeur


## `build:generate`

```bash
ece-tools build:generate
```

Génère tous les fichiers nécessaires à la phase de génération.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `build:transfer`

```bash
ece-tools build:transfer
```

Transfère les fichiers générés dans le répertoire init.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

Crée un fichier `.magento.env.yaml` avec la configuration de variable de build, de déploiement et de post-déploiement spécifiée. Remplace tout fichier `.magento.env.yaml` existant.

### Arguments

#### `configuration`

Configuration au format JSON

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

Met à jour le fichier `.magento.env.yaml` existant avec la configuration spécifiée. Crée `.magento.env.yaml` fichier s&#39;il n&#39;existe pas.

### Arguments

#### `configuration`

Configuration au format JSON

- Obligatoire

### Options

Pour connaître les options globales, reportez-vous à la section [Options globales](#global-options).


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

Valide le `.magento.env.yaml` fichier de configuration

### Options

Pour les options globales, voir [Options globales](#global-options).


## `config:dump`

```bash
ece-tools config:dumpdump
```

Configuration du vidage pour le déploiement de contenu statique.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `cron:disable`

```bash
ece-tools cron:disable
```

Disable all Magento cron processes and terminates all running processes.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `cron:enable`

```bash
ece-tools cron:enable
```

Active les processus Magento cron.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `cron:kill`

```bash
ece-tools cron:kill
```

Termine tous les processus cron de Magento.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

Déverrouillez les tâches cron qui sont bloquées au statut « en cours d’exécution ».

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--job-code`

Code de traitement cron à déverrouiller.

- Valeur par défaut : `[]`
- Accepte plusieurs valeurs


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

Génère le fichier dist/error-codes.md à partir du fichier schema.error.yaml.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Met à jour le compositeur pour le déploiement à partir de Git.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

Affichez les variables d’environnement de configuration de cloud codées.

### Arguments

#### `variable`

Variables d’environnement à afficher, options possibles : services,itinéraires,variables

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

Affiche des informations sur l’erreur par ID d’erreur ou des informations sur toutes les erreurs du dernier déploiement.

### Arguments

#### `error-code`

Code d’erreur, s’il n’est pas transmis. La commande affiche des informations sur toutes les erreurs du dernier déploiement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--json`, `-j`

Utilisé pour obtenir un résultat au format JSON

- Faire défaut: `false`
- N’accepte pas de valeur


## `module:refresh`

```bash
ece-tools module:refresh
```

Actualise la configuration pour activer les modules nouvellement ajoutés.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `schema:generate`

```bash
ece-tools schema:generate
```

Génère le fichier *.dist du schéma.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

Vérifie l’état idéal de la configuration.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

Vérifie la configuration maître-esclave.

### Options

Pour connaître les options globales, reportez-vous à la section [Options globales](#global-options).


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

Vérifie SCD sur la configuration de build.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

Vérifie la configuration SCD on Demand.

### Options

Pour connaître les options globales, reportez-vous à la section [Options globales](#global-options).


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

Vérifie le SCD lors de la configuration du déploiement.

### Options

Pour les options globales, voir [Options globales](#global-options).


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

Vérifie la possibilité de fractionner la base de données et si cette dernière a déjà été fractionnée ou non.

### Options

Pour les options globales, voir [Options globales](#global-options).
