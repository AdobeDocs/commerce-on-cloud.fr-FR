---
source-git-commit: fddcfdb97aede07b2cd6ef12bda6d7998f941951
workflow-type: tm+mt
source-wordcount: '13721'
ht-degree: 0%

---
# magento-cloud (Adobe Commerce sur les infrastructures cloud)

<!-- The template to render with above values -->

**Version** : 1.47.0

Cette référence contient 123 commandes disponibles via l’outil de ligne de commande `magento-cloud`.
La liste initiale est générée automatiquement à l’aide de la commande `magento-cloud list` dans Adobe Commerce sur l’infrastructure cloud.

## Général

Cette référence est générée à partir de la base de code de l’application. Pour modifier le contenu, vous pouvez mettre à jour le code source pour l’implémentation de commande correspondante dans le référentiel [codebase](https://github.com/magento/magento-cloud-cli) et envoyer vos modifications pour révision. Une autre méthode consiste à _Nous faire part de vos commentaires_ (recherchez le lien en haut à droite). Pour obtenir des instructions sur les contributions, voir [Coder les contributions](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Options globales

#### `--help`, `-h`

Afficher ce message d&#39;aide

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--version`, `-V`

Afficher cette version de l&#39;application

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--verbose`, `-v|-vv|-vvv`

Augmenter la verbosité des messages

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--quiet`, `-q`

Imprimer uniquement la sortie nécessaire ; supprimer les autres messages et erreurs. Cela implique : aucune interaction. Il est ignoré en mode verbeux.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--yes`, `-y`

Répondez « oui » aux questions de confirmation ; acceptez la valeur par défaut pour les autres questions ; désactivez l’interaction

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-interaction`

Ne posez pas de questions interactives ; acceptez les valeurs par défaut. Équivalent à l’utilisation de la variable d’environnement : MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `clear-cache`

```bash
magento-cloud cc
```

Effacer le cache de l’interface de ligne de commande

### Options

Pour les options globales, voir [Options globales](#global-options).


## `console`

```bash
magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Ouvrez le projet dans la console

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--browser`

Navigateur à utiliser pour ouvrir l’URL. Réglez 0 sur aucun.

- Requiert une valeur

#### `--pipe`

Générez l’URL dans stdout.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `decode`

```bash
magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Décoder une chaîne codée telle que MAGENTO_CLOUD_VARIABLES

### Arguments

#### `value`

Valeur de variable à décoder

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

Propriété à afficher dans la variable

- Requiert une valeur


## `docs`

```bash
magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

Ouvrir la documentation en ligne

### Arguments

#### `search`

Termes de recherche

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--browser`

Navigateur à utiliser pour ouvrir l’URL. Réglez 0 sur aucun.

- Requiert une valeur

#### `--pipe`

Générez l’URL dans stdout.

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `help`

```bash
magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

Affiche l’aide d’une commande

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### Arguments

#### `command_name`

Nom de la commande

- Valeur par défaut : `help`

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--format`

Format de sortie (txt, json ou md)

- Valeur par défaut : `txt`
- Requiert une valeur

#### `--raw`

Pour générer l’aide de la commande brute

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `list`

```bash
magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

Répertorie les commandes

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### Arguments

#### `command`

Commande à exécuter

- Obligatoire


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

#### `--all`

Afficher toutes les commandes, y compris les commandes masquées

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `multi`

```bash
magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

Exécuter une commande sur plusieurs projets

### Arguments

#### `cmd`

Commande à exécuter

- Valeur par défaut : `[]`
- Obligatoire

- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--projects`, `-p`

Liste d’identifiants de projet, séparés par des virgules et/ou des espaces

- Requiert une valeur

#### `--continue`

Continuer à exécuter les commandes même si une exception est rencontrée

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--sort`

Une propriété selon laquelle trier la liste des options du projet

- Valeur par défaut : `title`
- Requiert une valeur

#### `--reverse`

Inverser l’ordre des options du projet

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `activity:cancel`

```bash
magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Annuler une activité

### Arguments

#### `id`

Identifiant de l’activité. La valeur par défaut est l’activité annulable la plus récente.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--type`, `-t`

Filtrez par type (lors de la sélection d’une activité par défaut). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour le type, par exemple « %var% » pour sélectionner des activités liées aux variables.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-type`, `-x`

Exclure par type (lors de la sélection d’une activité par défaut). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour exclure des types.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--all`, `-a`

Vérifier les activités récentes dans tous les environnements (lors de la sélection d’une activité par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `activity:get`

```bash
magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

Afficher des informations détaillées sur une seule activité

### Arguments

#### `id`

Identifiant de l’activité. La valeur par défaut est l’activité la plus récente.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété à afficher

- Requiert une valeur

#### `--type`, `-t`

Filtrez par type (lors de la sélection d’une activité par défaut). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour le type, par exemple « %var% » pour sélectionner des activités liées aux variables.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-type`, `-x`

Exclure par type (lors de la sélection d’une activité par défaut). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour exclure des types.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--state`

Filtrer par état (lors de la sélection d’une activité par défaut) : en cours, en attente, terminé ou annulé. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--result`

Filtrer par résultat (lors de la sélection d’une activité par défaut) : succès ou échec

- Requiert une valeur

#### `--incomplete`, `-i`

Incluez uniquement les activités incomplètes (lors de la sélection d’une activité par défaut). Il s’agit d’un raccourci de —state=in_progress,pending

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--all`, `-a`

Vérifier les activités récentes dans tous les environnements (lors de la sélection d’une activité par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `activity:list`

```bash
magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obtention d’une liste d’activités pour un environnement ou un projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--type`, `-t`

Filtrer les activités par type Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. La première partie du nom de l’activité peut être omise ; par exemple, « cron » peut sélectionner les activités « environment.cron ». Les caractères % ou * peuvent être utilisés comme caractère générique, par exemple « %var% » pour sélectionner des activités liées aux variables.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-type`, `-x`

Exclure les activités par type. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. La première partie du nom de l’activité peut être omise ; par exemple, « cron » peut exclure les activités « environment.cron ». Les caractères % ou * peuvent être utilisés comme caractère générique pour exclure des types.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--limit`

Limiter le nombre de résultats affichés

- Valeur par défaut : `10`
- Requiert une valeur

#### `--start`

Seules les activités créées avant cette date seront répertoriées

- Requiert une valeur

#### `--state`

Filtrer les activités par état : en cours, en attente, terminées ou annulées. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--result`

Filtrer les activités par résultat : succès ou échec

- Requiert une valeur

#### `--incomplete`, `-i`

Répertorier uniquement les activités incomplètes

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--all`, `-a`

Liste des activités dans tous les environnements

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : id*, créé*, description*, progression*, état*, résultat*, terminé, environnements, time_build, time_deploy, time_execute, time_wait, type (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `activity:log`

```bash
magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Affichage du journal d’une activité

### Arguments

#### `id`

Identifiant de l’activité. La valeur par défaut est l’activité la plus récente.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Intervalle d’actualisation des activités (secondes). Définissez sur 0 pour désactiver l’actualisation.

- Valeur par défaut : `3`
- Requiert une valeur

#### `--timestamps`, `-t`

Afficher un horodatage en regard de chaque message

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--type`

Filtrez par type (lors de la sélection d’une activité par défaut). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour le type, par exemple « %var% » pour sélectionner des activités liées aux variables.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-type`, `-x`

Exclure par type (lors de la sélection d’une activité par défaut). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour exclure des types.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--state`

Filtrer par état (lors de la sélection d’une activité par défaut) : en cours, en attente, terminé ou annulé. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--result`

Filtrer par résultat (lors de la sélection d’une activité par défaut) : succès ou échec

- Requiert une valeur

#### `--incomplete`, `-i`

Incluez uniquement les activités incomplètes (lors de la sélection d’une activité par défaut). Il s’agit d’un raccourci de —state=in_progress,pending

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--all`, `-a`

Vérifier les activités récentes dans tous les environnements (lors de la sélection d’une activité par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `app:config-get`

```bash
magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Affichage de la configuration d’une application

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété de configuration à afficher

- Requiert une valeur

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--identity-file`, `-i`

[Option obsolète, n’est plus utilisée]

- Requiert une valeur


## `app:list`

```bash
magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Liste des applications dans le projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--pipe`

Générer une liste de noms d’application uniquement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : nom*, type*, disque, chemin, taille (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `auth:api-token-login`

```bash
magento-cloud auth:api-token-login
```

Connectez-vous à Magento Cloud à l’aide d’un jeton API

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/fr/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### Options

Pour les options globales, voir [Options globales](#global-options).


## `auth:browser-login`

```bash
magento-cloud login [-f|--force] [--method METHOD] [--max-age MAX-AGE] [--browser BROWSER] [--pipe]
```

Connectez-vous à Magento Cloud via un navigateur

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--force`, `-f`

Reconnectez-vous, même si vous êtes déjà connecté

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--method`

Exiger des méthodes d’authentification spécifiques

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--max-age`

Âge maximal (en secondes) de la session d’authentification web

- Requiert une valeur

#### `--browser`

Navigateur à utiliser pour ouvrir l’URL. Réglez 0 sur aucun.

- Requiert une valeur

#### `--pipe`

Générez l’URL dans stdout.

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `auth:info`

```bash
magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

Afficher les informations de votre compte

### Arguments

#### `property`

La propriété de compte à afficher

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--no-auto-login`

Ignore la connexion automatique. Si vous n’êtes pas connecté, rien ne sera généré et le code de sortie sera 0, en supposant qu’il n’y ait aucune autre erreur.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--property`, `-P`

La propriété de compte à afficher (syntaxe alternative)

- Requiert une valeur

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `auth:logout`

```bash
magento-cloud logout [-a|--all] [--other]
```

Déconnectez-vous de Magento Cloud

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--all`, `-a`

Se déconnecter de toutes les sessions locales

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--other`

Se déconnecter des autres sessions locales

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `autoscaling:get`

```bash
magento-cloud autoscaling [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Afficher la configuration de mise à l’échelle automatique des applications et des programmes de travail sur un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : service*, mesure*, direction*, seuil*, durée*, activé*, nombre_d’instances*, recharge, instances_max, instances_min (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `autoscaling:set`

```bash
magento-cloud autoscaling:set [-s|--service SERVICE] [-m|--metric METRIC] [--enabled ENABLED] [--threshold-up THRESHOLD-UP] [--duration-up DURATION-UP] [--cooldown-up COOLDOWN-UP] [--threshold-down THRESHOLD-DOWN] [--duration-down DURATION-DOWN] [--cooldown-down COOLDOWN-DOWN] [--instances-min INSTANCES-MIN] [--instances-max INSTANCES-MAX] [--dry-run] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Définir la configuration de mise à l’échelle automatique des applications ou des programmes de travail dans un environnement

```
Configure automatic scaling for apps or workers in an environment.

You can also configure resources statically by running: magento-cloud resources:set
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--service`, `-s`

Nom de l’application ou du programme de travail pour lequel configurer la mise à l’échelle automatique

- Requiert une valeur

#### `--metric`, `-m`

Nom de la mesure à utiliser pour déclencher la mise à l’échelle automatique

- Requiert une valeur

#### `--enabled`

Activer la mise à l’échelle automatique en fonction de la mesure donnée

- Requiert une valeur

#### `--threshold-up`

Seuil au-delà duquel le service sera étendu

- Requiert une valeur

#### `--duration-up`

Durée pendant laquelle la mesure est évaluée par rapport au seuil de mise à l’échelle

- Requiert une valeur

#### `--cooldown-up`

Durée d’attente avant de tenter une mise à l’échelle supplémentaire après un événement de mise à l’échelle

- Requiert une valeur

#### `--threshold-down`

Seuil en dessous duquel le service sera réduit

- Requiert une valeur

#### `--duration-down`

Durée pendant laquelle la mesure est évaluée par rapport au seuil de réduction

- Requiert une valeur

#### `--cooldown-down`

Durée d’attente avant d’essayer de réduire davantage après un événement de mise à l’échelle

- Requiert une valeur

#### `--instances-min`

Nombre minimum d’instances qui seront réduites à

- Requiert une valeur

#### `--instances-max`

Nombre maximal d’instances qui seront mises à l’échelle

- Requiert une valeur

#### `--dry-run`

Afficher les modifications qui seraient apportées, sans rien changer

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `blackfire:setup`

```bash
magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Configuration de l’intégration Blackfire.io pour le projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--server_id`

Identifiant du serveur

- Requiert une valeur

#### `--server_token`

Jeton du serveur

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `certificate:add`

```bash
magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Ajouter un certificat SSL au projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--cert`

Chemin d’accès au fichier de certificat

- Requiert une valeur

#### `--key`

Chemin d’accès au fichier de clé privée du certificat

- Requiert une valeur

#### `--chain`

Chemin d’accès au fichier de chaîne de certificats

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `certificate:delete`

```bash
magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

Supprimer un certificat du projet

### Arguments

#### `id`

L’identifiant du certificat (ou son début)

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `certificate:get`

```bash
magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

Afficher un certificat

### Arguments

#### `id`

L’identifiant du certificat (ou son début)

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété de certificat à afficher

- Requiert une valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `certificate:list`

```bash
magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Liste des certificats de projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--domain`

Filtrer par nom de domaine (recherche non sensible à la casse)

- Requiert une valeur

#### `--exclude-domain`

Exclure des certificats, correspondance par nom de domaine (recherche non sensible à la casse)

- Requiert une valeur

#### `--issuer`

Filtrer par émetteur

- Requiert une valeur

#### `--only-auto`

Afficher uniquement les certificats configurés automatiquement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-auto`

Afficher uniquement les certificats ajoutés manuellement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--ignore-expiry`

Afficher les certificats expirés et non expirés

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--only-expired`

Afficher uniquement les certificats expirés

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-expired`

Afficher uniquement les certificats non expirés (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--pipe-domains`

Renvoie uniquement une liste de noms de domaine couverts par les certificats

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : créé, domaines, expiration, id, émetteur. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `commit:get`

```bash
magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

Afficher les détails de validation

### Arguments

#### `commit`

La validation SHA. Cette méthode accepte également les suffixes « HEAD », et signe d’insertion (^) ou tilde (~) pour les validations parentes.

- Valeur par défaut : `HEAD`

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété commit à afficher.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `commit:list`

```bash
magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

Liste des validations

### Arguments

#### `commit`

La validation Git de départ SHA. Cette méthode accepte également les suffixes « HEAD », et signe d’insertion (^) ou tilde (~) pour les validations parentes.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--limit`

Nombre de validations à afficher.

- Valeur par défaut : `10`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : auteur, date, partage, résumé. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `db:dump`

```bash
magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP]
```

Créer une image mémoire locale de la base distante

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--schema`

Le schéma à vider. Omettez d’utiliser le schéma par défaut (généralement « principal »).

- Requiert une valeur

#### `--file`, `-f`

Nom de fichier personnalisé pour l’image mémoire

- Requiert une valeur

#### `--directory`, `-d`

Un répertoire personnalisé pour l’image mémoire

- Requiert une valeur

#### `--gzip`, `-z`

Compresser l’image mémoire à l’aide de gzip

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--timestamp`, `-t`

Ajoutez une date et une heure au nom du fichier de vidage

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--stdout`, `-o`

Sortie vers STDOUT au lieu d&#39;un fichier

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--table`

Table(s) à inclure

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-table`

Table(s) à exclure

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--schema-only`

Ne vider que les schémas, pas de données

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--charset`

Encodage du jeu de caractères pour l’image mémoire

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur


## `db:sql`

```bash
magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--] [<query>]
```

Exécuter SQL sur la base distante

### Arguments

#### `query`

Instruction SQL à exécuter

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--raw`

Produire une sortie brute et non tabulaire

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--schema`

Schéma à utiliser. Omettez d’utiliser le schéma par défaut (généralement « principal »). Transmettez une chaîne vide pour ne pas utiliser de schéma.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur


## `domain:add`

```bash
magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Ajoutez un nouveau domaine au projet. Cette option n’est pas disponible pour les projets de plan Cloud Pro.

### Arguments

#### `name`

Le nom de domaine

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--cert`

Chemin d’accès à un fichier de certificat personnalisé

- Requiert une valeur

#### `--key`

Chemin d’accès à la clé privée pour le certificat personnalisé

- Requiert une valeur

#### `--chain`

Chemin d’accès aux fichiers de chaîne pour le certificat personnalisé

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--attach`

Domaine de production que celui-ci remplace dans les itinéraires de l’environnement. Requis pour les domaines d’environnement hors production.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `domain:delete`

```bash
magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Supprimez un domaine du projet. Cette option n’est pas disponible pour les projets de plan Cloud Pro.

### Arguments

#### `name`

Le nom de domaine

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `domain:get`

```bash
magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

Afficher des informations détaillées sur un domaine. Cette option n’est pas disponible pour les projets de plan Cloud Pro.

### Arguments

#### `name`

Le nom de domaine

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

Propriété de domaine à afficher

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `domain:list`

```bash
magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obtenez une liste de tous les domaines. Cette option n’est pas disponible pour les projets de plan Cloud Pro.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : name*, ssl*, created_at*, registered_name, replace_for, type, updated_at (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `domain:update`

```bash
magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Mettre à jour un domaine. Cette option n’est pas disponible pour les projets de plan Cloud Pro.

### Arguments

#### `name`

Le nom de domaine

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--cert`

Chemin d’accès à un fichier de certificat personnalisé

- Requiert une valeur

#### `--key`

Chemin d’accès à la clé privée pour le certificat personnalisé

- Requiert une valeur

#### `--chain`

Chemin d’accès aux fichiers de chaîne pour le certificat personnalisé

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:activate`

```bash
magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Activation d’un environnement

### Arguments

#### `environment`

Le ou les environnements à activer

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--parent`

Définir un nouveau parent d’environnement avant l’activation

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:branch`

```bash
magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [--no-checkout] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

Branchement d’un environnement

### Arguments

#### `id`

L’identifiant (nom de la branche) du nouvel environnement


#### `parent`

Le parent du nouvel environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--title`

Titre du nouvel environnement

- Requiert une valeur

#### `--type`

Type du nouvel environnement

- Requiert une valeur

#### `--no-clone-parent`

Ne pas cloner les données de l’environnement parent

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-checkout`

Ne pas extraire la branche localement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:checkout`

```bash
magento-cloud checkout [<id>]
```

Extraction d’un environnement

### Arguments

#### `id`

Identifiant de l’environnement à extraire. Par exemple : « sprint2 »

### Options

Pour les options globales, voir [Options globales](#global-options).


## `environment:delete`

```bash
magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--status STATUS] [--only-status ONLY-STATUS] [--exclude-status EXCLUDE-STATUS] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Supprimer un ou plusieurs environnements

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### Arguments

#### `environment`

Environnement(s) à supprimer. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--delete-branch`

Supprimer la ou les branches Git pour les environnements inactifs, sans confirmation

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-delete-branch`

Ne supprimez aucune branche Git (environnements inactifs).

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--type`

Supprimer tous les environnements d’un type (en les ajoutant aux autres sélectionnés) Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--only-type`, `-t`

Seuls les environnements de suppression d’un type spécifique Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude`

Environnement(s) à supprimer. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-type`

Type(s) d’environnement pour lequel(lesquels) ne pas supprimer Les valeurs peuvent être fractionnées par des virgules (par exemple « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--inactive`

Supprimer tous les environnements inactifs (en les ajoutant aux autres sélectionnés)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--status`

Supprimer tous les environnements d’un statut (en les ajoutant aux autres sélectionnés) Les valeurs peuvent être fractionnées par des virgules (par exemple « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--only-status`

Seuls les environnements de suppression ayant un statut spécifique Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-status`

Statut(s) d’environnement pour lequel/lesquels ne pas supprimer Les valeurs peuvent être fractionnées par des virgules (par exemple « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--merged`

Supprimer tous les environnements fusionnés (en les ajoutant aux autres sélectionnés)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--allow-delete-parent`

Autoriser la suppression des environnements ayant des enfants

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:deploy`

```bash
magento-cloud deploy [-s|--strategy STRATEGY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Déploiement des modifications intermédiaires d’un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--strategy`, `-s`

La stratégie de déploiement, arrêter le démarrage (par défaut, redémarrer avec un arrêt) ou en continu (aucun temps d’arrêt)

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:deploy:type`

```bash
magento-cloud environment:deploy:type [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<type>]
```

Afficher ou définir le type de déploiement de l’environnement

```
Choose automatic (the default) if you want your changes to be deployed immediately as they are made.
Choose manual to have changes staged until you trigger a deployment (including changes to code, variables, domains and settings).
```

### Arguments

#### `type`

Le type de déploiement de l’environnement : automatique ou manuel.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--pipe`

Générer le type de déploiement en sortie stdout

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:http-access`

```bash
magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Mise à jour des paramètres d’accès HTTP pour un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--access`

Restriction d’accès au format « autorisation :address. Utilisez 0 pour effacer toutes les adresses.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--auth`

Informations d’identification d’authentification de base HTTP au format « nom d’utilisateur :password ». Utilisez 0 pour effacer toutes les informations d’identification.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--enabled`

Indique si le contrôle d’accès doit être activé : 1 pour activer, 0 pour désactiver

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:info`

```bash
magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Lire ou définir des propriétés pour un environnement

### Arguments

#### `property`

Nom de la propriété


#### `value`

Définissez une nouvelle valeur pour la propriété

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:init`

```bash
magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

Initialisation d’un environnement à partir d’un référentiel Git public

### Arguments

#### `url`

Une URL vers un référentiel Git

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--profile`

Nom du profil

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:list`

```bash
magento-cloud environments [-I|--no-inactive] [--status STATUS] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Obtenir une liste des environnements

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--no-inactive`, `-I`

Ne pas afficher les environnements inactifs

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--status`

Filtrer les environnements par statut (actif, inactif, sale, en pause, suppression). Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--pipe`

Génère une liste simple d’identifiants d’environnement.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--refresh`

Actualiser la liste.

- Valeur par défaut : `1`
- Requiert une valeur

#### `--sort`

Propriété par laquelle effectuer le tri

- Valeur par défaut : `title`
- Requiert une valeur

#### `--reverse`

Tri dans l’ordre inverse (décroissant)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--type`

Filtrer la liste par type(s) d’environnement. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : id*, titre*, statut*, type*, créé, nom_machine, mis à jour (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `environment:logs`

```bash
magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

Lecture des journaux d’un environnement

### Arguments

#### `type`

Type de journal, par exemple « accès » ou « erreur »

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--lines`

Nombre de lignes à afficher

- Valeur par défaut : `100`
- Requiert une valeur

#### `--tail`

Enregistrer le journal en continu

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `environment:merge`

```bash
magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Fusion d’un environnement

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### Arguments

#### `environment`

Environnement à fusionner

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:pause`

```bash
magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Mettre en pause un environnement

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:push`

```bash
magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<source>]
```

Code push vers un environnement

### Arguments

#### `source`

La référence de source Git, par exemple un nom de branche ou un hachage de validation.

- Valeur par défaut : `HEAD`

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--target`

Nom de la branche cible. La valeur par défaut est la branche active.

- Requiert une valeur

#### `--force`, `-f`

Autoriser les mises à jour non rapides

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--force-with-lease`

Autoriser les mises à jour non rapides si la branche de tracking distant est à jour

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--set-upstream`, `-u`

Définissez l’environnement cible comme amont de la branche source. Cela définit également le projet cible comme distant pour le référentiel local.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--activate`

Activez l’environnement. Les environnements en pause reprendront. Cela permet de s’assurer que l’environnement est actif même si aucune modification n’a été transmise.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--parent`

Définir le parent de l’environnement (utilisé uniquement avec —activate)

- Requiert une valeur

#### `--type`

Définissez le type d’environnement (utilisé uniquement avec —activate )

- Requiert une valeur

#### `--no-clone-parent`

Ne clonez pas les données de la branche parent (utilisée uniquement avec —activate).

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `environment:redeploy`

```bash
magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Redéploiement d’un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:relationships`

```bash
magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<environment>]
```

Afficher les relations d’un environnement

### Arguments

#### `environment`

L&#39;environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété de relation à afficher

- Requiert une valeur

#### `--refresh`

Actualiser ou non les relations

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `environment:resume`

```bash
magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Reprise d’un environnement en pause

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:scp`

```bash
magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<files>]...
```

Copier des fichiers vers et depuis un environnement à l’aide de scp

### Arguments

#### `files`

Fichiers à copier. Utilisez le préfixe remote : pour définir les emplacements distants.

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--recursive`, `-r`

Copie récursive de répertoires entiers

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `environment:ssh`

```bash
magento-cloud ssh [--pipe] [--all] [-o|--option OPTION] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<cmd>]...
```

SSH vers l’environnement actuel

### Arguments

#### `cmd`

Commande à exécuter sur l’environnement.

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--pipe`

Générez uniquement l’URL SSH.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--all`

Générez toutes les URL SSH (pour chaque application).

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--option`, `-o`

Transmettre une option supplémentaire à SSH

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `environment:synchronize`

```bash
magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

Synchroniser le code et/ou les données d’un environnement à partir de son parent

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child.

Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### Arguments

#### `synchronize`

Éléments à synchroniser : « code », « données » ou les deux

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--rebase`

Synchroniser le code en le basant au lieu de le fusionner

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `environment:url`

```bash
magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Obtention des URL publiques d’un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--primary`, `-1`

Renvoyer uniquement l’URL de l’itinéraire principal

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--browser`

Navigateur à utiliser pour ouvrir l’URL. Réglez 0 sur aucun.

- Requiert une valeur

#### `--pipe`

Générez l’URL dans stdout.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `environment:xdebug`

```bash
magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Ouverture d’un tunnel vers Xdebug sur l’environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--port`

Le port local

- Valeur par défaut : `9000`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `integration:activity:get`

```bash
magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

Affichage d’informations détaillées sur une seule activité d’intégration

### Arguments

#### `integration`

Identifiant d’intégration. Laissez vide pour effectuer un choix dans une liste.


#### `activity`

Identifiant de l’activité. La valeur par défaut est l’activité d’intégration la plus récente.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété à afficher

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

[Option obsolète, non utilisée]

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `integration:activity:list`

```bash
magento-cloud integration:activities [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Obtention de la liste des activités pour une intégration

### Arguments

#### `id`

Identifiant d’intégration. Laissez vide pour effectuer un choix dans une liste.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--type`

Filtrez les activités par type. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--exclude-type`, `-x`

Exclure les activités par type. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces. Les caractères % ou * peuvent être utilisés comme caractère générique pour exclure des types.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--limit`

Limiter le nombre de résultats affichés

- Valeur par défaut : `10`
- Requiert une valeur

#### `--start`

Seules les activités créées avant cette date seront répertoriées

- Requiert une valeur

#### `--state`

Filtrez les activités par état. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--result`

Filtrer les activités par résultat

- Requiert une valeur

#### `--incomplete`, `-i`

Répertorier uniquement les activités incomplètes

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : id*, créé*, description*, type*, état*, résultat*, terminé, progression, time_build, time_deploy, time_execute, time_wait (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

[Option obsolète, non utilisée]

- Requiert une valeur


## `integration:activity:log`

```bash
magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

Affichage du journal d’une activité d’intégration

### Arguments

#### `integration`

Identifiant d’intégration. Laissez vide pour effectuer un choix dans une liste.


#### `activity`

Identifiant de l’activité. La valeur par défaut est l’activité d’intégration la plus récente.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--timestamps`, `-t`

Afficher un horodatage en regard de chaque message

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

[Option obsolète, non utilisée]

- Requiert une valeur


## `integration:add`

```bash
magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Ajouter une intégration au projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--type`

Le type d&#39;intégration (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumological&#39;, &#39;syslog&#39;, &#39;otlplog&#39;)

- Requiert une valeur

#### `--base-url`

URL de base de l’installation du serveur

- Requiert une valeur

#### `--bitbucket-url`

URL de base de l’installation de Bitbucket Server

- Requiert une valeur

#### `--username`

Nom d’utilisateur du serveur Bitbucket

- Requiert une valeur

#### `--token`

Jeton d’authentification ou d’accès pour l’intégration

- Requiert une valeur

#### `--key`

Clé de consommateur OAuth Bitbucket

- Requiert une valeur

#### `--secret`

Secret du client OAuth Bitbucket

- Requiert une valeur

#### `--license-key`

La clé de licence Journaux New Relic

- Requiert une valeur

#### `--server-project`

Le projet (par exemple &#39;namespace/repo&#39;)

- Requiert une valeur

#### `--repository`

Référentiel à suivre (par exemple, « propriétaire/référentiel »)

- Requiert une valeur

#### `--build-merge-requests`

GitLab : création de demandes de fusion en tant qu’environnements

- Valeur par défaut : `true`
- Requiert une valeur

#### `--build-pull-requests`

Création de chaque demande d’extraction en tant qu’environnement

- Valeur par défaut : `true`
- Requiert une valeur

#### `--build-draft-pull-requests`

Créer des brouillons de demandes d’extraction

- Valeur par défaut : `true`
- Requiert une valeur

#### `--build-pull-requests-post-merge`

Générer des demandes d’extraction en fonction de leur état après la fusion

- Valeur par défaut : `false`
- Requiert une valeur

#### `--build-wip-merge-requests`

GitLab : création de demandes de fusion de travaux en cours

- Valeur par défaut : `true`
- Requiert une valeur

#### `--merge-requests-clone-parent-data`

GitLab : clonage des données pour les demandes de fusion

- Valeur par défaut : `true`
- Requiert une valeur

#### `--pull-requests-clone-parent-data`

Clonez les données de l’environnement parent pour les demandes d’extraction

- Valeur par défaut : `true`
- Requiert une valeur

#### `--resync-pull-requests`

Resynchroniser les données de l’environnement de requête d’extraction sur chaque version

- Valeur par défaut : `false`
- Requiert une valeur

#### `--fetch-branches`

Récupérer toutes les branches à partir de l’environnement distant (en tant qu’environnements inactifs)

- Valeur par défaut : `true`
- Requiert une valeur

#### `--prune-branches`

Supprimer les branches qui n’existent pas sur la télécommande

- Valeur par défaut : `true`
- Requiert une valeur

#### `--resources-init`

Les ressources à utiliser lors de l&#39;initialisation d&#39;un nouveau service (&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- Requiert une valeur

#### `--url`

Point d’entrée d’URL ou d’API pour l’intégration

- Requiert une valeur

#### `--shared-key`

Webhook : clé secrète partagée JWS

- Requiert une valeur

#### `--file`

Nom d’un fichier local contenant le script à charger

- Requiert une valeur

#### `--events`

Liste des événements sur lesquels agir, par exemple environment.push

- Valeur par défaut : `*`
- Requiert une valeur

#### `--states`

Une liste des états sur lesquels agir, par exemple en attente, en cours, terminé

- Valeur par défaut : `complete`
- Requiert une valeur

#### `--environments`

Identifiants d’environnement à inclure

- Valeur par défaut : `*`
- Requiert une valeur

#### `--excluded-environments`

Identifiants d’environnement à exclure

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--from-address`

[Facultatif] Adresse d’expédition personnalisée pour les e-mails d’alerte

- Requiert une valeur

#### `--recipients`

Adresse(s) email du destinataire

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--channel`

Le canal Slack

- Requiert une valeur

#### `--routing-key`

La clé de routage PagerDuty

- Requiert une valeur

#### `--category`

La catégorie Logique Sumo , utilisée pour le filtrage.

- Requiert une valeur

#### `--index`

Index Splunk

- Requiert une valeur

#### `--sourcetype`

Le type de source d’événement Splunk

- Requiert une valeur

#### `--protocol`

Protocole de transport Syslog (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Valeur par défaut : `tls`
- Requiert une valeur

#### `--syslog-host`

Hôte de collecte/relais Syslog

- Requiert une valeur

#### `--syslog-port`

Port du collecteur/relais Syslog

- Requiert une valeur

#### `--facility`

Fonction Syslog

- Valeur par défaut : `1`
- Requiert une valeur

#### `--message-format`

Format du message Syslog (&#39;rfc3164&#39; ou &#39;rfc5424&#39;)

- Valeur par défaut : `rfc5424`
- Requiert une valeur

#### `--auth-mode`

Mode d&#39;authentification (&#39;prefix&#39; ou &#39;structured_data&#39;)

- Valeur par défaut : `prefix`
- Requiert une valeur

#### `--auth-token`

Jeton d’authentification

- Requiert une valeur

#### `--verify-tls`

Si la vérification du certificat HTTPS doit être activée (recommandé)

- Valeur par défaut : `true`
- Requiert une valeur

#### `--header`

En-tête(s) HTTP à utiliser dans les requêtes POST. Séparez les noms et les valeurs par deux points (:).

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `integration:delete`

```bash
magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Supprimer une intégration d’un projet

### Arguments

#### `id`

Identifiant d’intégration. Laissez vide pour effectuer un choix dans une liste.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `integration:get`

```bash
magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

Affichage des détails d’une intégration

### Arguments

#### `id`

Identifiant d’intégration. Laissez vide pour effectuer un choix dans une liste.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété d’intégration à afficher

- Accepte une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `integration:list`

```bash
magento-cloud integrations [-t|--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Afficher une liste des intégrations de projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--type`, `-t`

Filtrer par type

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : id, résumé, type. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `integration:update`

```bash
magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Mise à jour d’une intégration

### Arguments

#### `id`

Identifiant de l’intégration à mettre à jour

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--type`

Le type d&#39;intégration (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webhook&#39;, &#39;health.email&#39;, &#39;health.pagerduty&#39;, &#39;health.slack&#39;, &#39;health.webhook&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumological&#39;, &#39;syslog&#39;, &#39;otlplog&#39;)

- Requiert une valeur

#### `--base-url`

URL de base de l’installation du serveur

- Requiert une valeur

#### `--bitbucket-url`

URL de base de l’installation de Bitbucket Server

- Requiert une valeur

#### `--username`

Nom d’utilisateur du serveur Bitbucket

- Requiert une valeur

#### `--token`

Jeton d’authentification ou d’accès pour l’intégration

- Requiert une valeur

#### `--key`

Clé de consommateur OAuth Bitbucket

- Requiert une valeur

#### `--secret`

Secret du client OAuth Bitbucket

- Requiert une valeur

#### `--license-key`

La clé de licence Journaux New Relic

- Requiert une valeur

#### `--server-project`

Le projet (par exemple &#39;namespace/repo&#39;)

- Requiert une valeur

#### `--repository`

Référentiel à suivre (par exemple, « propriétaire/référentiel »)

- Requiert une valeur

#### `--build-merge-requests`

GitLab : création de demandes de fusion en tant qu’environnements

- Valeur par défaut : `true`
- Requiert une valeur

#### `--build-pull-requests`

Création de chaque demande d’extraction en tant qu’environnement

- Valeur par défaut : `true`
- Requiert une valeur

#### `--build-draft-pull-requests`

Créer des brouillons de demandes d’extraction

- Valeur par défaut : `true`
- Requiert une valeur

#### `--build-pull-requests-post-merge`

Générer des demandes d’extraction en fonction de leur état après la fusion

- Valeur par défaut : `false`
- Requiert une valeur

#### `--build-wip-merge-requests`

GitLab : création de demandes de fusion de travaux en cours

- Valeur par défaut : `true`
- Requiert une valeur

#### `--merge-requests-clone-parent-data`

GitLab : clonage des données pour les demandes de fusion

- Valeur par défaut : `true`
- Requiert une valeur

#### `--pull-requests-clone-parent-data`

Clonez les données de l’environnement parent pour les demandes d’extraction

- Valeur par défaut : `true`
- Requiert une valeur

#### `--resync-pull-requests`

Resynchroniser les données de l’environnement de requête d’extraction sur chaque version

- Valeur par défaut : `false`
- Requiert une valeur

#### `--fetch-branches`

Récupérer toutes les branches à partir de l’environnement distant (en tant qu’environnements inactifs)

- Valeur par défaut : `true`
- Requiert une valeur

#### `--prune-branches`

Supprimer les branches qui n’existent pas sur la télécommande

- Valeur par défaut : `true`
- Requiert une valeur

#### `--resources-init`

Les ressources à utiliser lors de l&#39;initialisation d&#39;un nouveau service (&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- Requiert une valeur

#### `--url`

Point d’entrée d’URL ou d’API pour l’intégration

- Requiert une valeur

#### `--shared-key`

Webhook : clé secrète partagée JWS

- Requiert une valeur

#### `--file`

Nom d’un fichier local contenant le script à charger

- Requiert une valeur

#### `--events`

Liste des événements sur lesquels agir, par exemple environment.push

- Valeur par défaut : `*`
- Requiert une valeur

#### `--states`

Une liste des états sur lesquels agir, par exemple en attente, en cours, terminé

- Valeur par défaut : `complete`
- Requiert une valeur

#### `--environments`

Identifiants d’environnement à inclure

- Valeur par défaut : `*`
- Requiert une valeur

#### `--excluded-environments`

Identifiants d’environnement à exclure

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--from-address`

[Facultatif] Adresse d’expédition personnalisée pour les e-mails d’alerte

- Requiert une valeur

#### `--recipients`

Adresse(s) email du destinataire

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--channel`

Le canal Slack

- Requiert une valeur

#### `--routing-key`

La clé de routage PagerDuty

- Requiert une valeur

#### `--category`

La catégorie Logique Sumo , utilisée pour le filtrage.

- Requiert une valeur

#### `--index`

Index Splunk

- Requiert une valeur

#### `--sourcetype`

Le type de source d’événement Splunk

- Requiert une valeur

#### `--protocol`

Protocole de transport Syslog (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Valeur par défaut : `tls`
- Requiert une valeur

#### `--syslog-host`

Hôte de collecte/relais Syslog

- Requiert une valeur

#### `--syslog-port`

Port du collecteur/relais Syslog

- Requiert une valeur

#### `--facility`

Fonction Syslog

- Valeur par défaut : `1`
- Requiert une valeur

#### `--message-format`

Format du message Syslog (&#39;rfc3164&#39; ou &#39;rfc5424&#39;)

- Valeur par défaut : `rfc5424`
- Requiert une valeur

#### `--auth-mode`

Mode d&#39;authentification (&#39;prefix&#39; ou &#39;structured_data&#39;)

- Valeur par défaut : `prefix`
- Requiert une valeur

#### `--auth-token`

Jeton d’authentification

- Requiert une valeur

#### `--verify-tls`

Si la vérification du certificat HTTPS doit être activée (recommandé)

- Valeur par défaut : `true`
- Requiert une valeur

#### `--header`

En-tête(s) HTTP à utiliser dans les requêtes POST. Séparez les noms et les valeurs par deux points (:).

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `integration:validate`

```bash
magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

Validation d’une intégration existante

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### Arguments

#### `id`

Identifiant d’intégration. Laissez vide pour effectuer un choix dans une liste.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `local:build`

```bash
magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

Générer le projet actuel localement

### Arguments

#### `app`

Spécifier la ou les applications à créer

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--abslinks`, `-a`

Utiliser des liens absolus

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--source`, `-s`

Le répertoire source. La valeur par défaut est la racine actuelle du projet.

- Requiert une valeur

#### `--destination`, `-d`

Destination vers laquelle la racine web de chaque application sera liée par un lien symbolique. Valeur par défaut : _www

- Requiert une valeur

#### `--copy`, `-c`

Copiez dans un répertoire de build, au lieu de créer un lien symbolique depuis la source

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--clone`

Utilisez Git pour cloner l’HEAD active dans le répertoire de build

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--run-deploy-hooks`

Exécution des hooks de déploiement et/ou de post_déploiement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-clean`

Ne pas supprimer les anciennes versions

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-archive`

Ne créez ou n’utilisez pas d’archive de build

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-backup`

Ne pas sauvegarder le build précédent

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-cache`

Désactiver la mise en cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-build-hooks`

N’exécutez pas les hooks de post-génération.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--no-deps`

N’installez pas les dépendances de build localement.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--working-copy`

Drush : utilisez git pour cloner un référentiel de chaque module Drupal plutôt que de simplement télécharger une version

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--concurrency`

Pincer : permet de définir le nombre de projets simultanés qui seront traités en même temps

- Valeur par défaut : `4`
- Requiert une valeur

#### `--lock`

Drush : permet de créer ou de mettre à jour un fichier de verrouillage (disponible uniquement avec les versions 7 et ultérieures de Drush).

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `local:dir`

```bash
magento-cloud dir [<subdir>]
```

Rechercher la racine du projet local

### Arguments

#### `subdir`

Le sous-répertoire à rechercher (&#39;local&#39;, &#39;web&#39; ou &#39;shared&#39;)

### Options

Pour les options globales, voir [Options globales](#global-options).


## `metrics:all`

```bash
magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Affichage des mesures de CPU, de disque et de mémoire pour un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--bytes`, `-B`

Afficher les tailles en octets

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--range`, `-r`

La période. Les mesures seront chargées pendant cette durée jusqu’à l’heure de fin (—à). Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 5m, maximum 8h ou plus (selon le projet), par défaut 10m.

- Requiert une valeur

#### `--interval`, `-i`

Intervalle de temps. La valeur par défaut est une division de la plage. Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 1m.

- Requiert une valeur

#### `--to`

L&#39;heure de fin. La valeur par défaut est maintenant.

- Requiert une valeur

#### `--latest`, `-1`

Afficher uniquement le dernier point de données unique

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--service`, `-s`

Filtrer par nom de service ou d’application Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--type`

Filtrez par type de service (si —service n’est pas fourni). La version n’est pas requise. Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : timestamp*, service*, cpu_percent*, mem_percent*, disk_percent*, tmp_disk_percent*, cpu_limit, cpu_used, disk_limit, disk_used, inodes_limit, inodes_percent, inodes_used, mem_limit, mem_used, tmp_disk_limit, tmp_disk_used, tmp_inodes_limit, tmp_inodes_percent, tmp_inodes_used, type (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `metrics:cpu`

```bash
magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Affichage de l’utilisation de CPU dans un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--range`, `-r`

La période. Les mesures seront chargées pendant cette durée jusqu’à l’heure de fin (—à). Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 5m, maximum 8h ou plus (selon le projet), par défaut 10m.

- Requiert une valeur

#### `--interval`, `-i`

Intervalle de temps. La valeur par défaut est une division de la plage. Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 1m.

- Requiert une valeur

#### `--to`

L&#39;heure de fin. La valeur par défaut est maintenant.

- Requiert une valeur

#### `--latest`, `-1`

Afficher uniquement le dernier point de données unique

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--service`, `-s`

Filtrer par nom de service ou d’application Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--type`

Filtrez par type de service (si —service n’est pas fourni). La version n’est pas requise. Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : horodatage*, service*, utilisé*, limite*, pourcentage*, type (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `metrics:disk-usage`

```bash
magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Afficher l’utilisation du disque d’un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--bytes`, `-B`

Afficher les tailles en octets

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--range`, `-r`

La période. Les mesures seront chargées pendant cette durée jusqu’à l’heure de fin (—à). Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 5m, maximum 8h ou plus (selon le projet), par défaut 10m.

- Requiert une valeur

#### `--interval`, `-i`

Intervalle de temps. La valeur par défaut est une division de la plage. Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 1m.

- Requiert une valeur

#### `--to`

L&#39;heure de fin. La valeur par défaut est maintenant.

- Requiert une valeur

#### `--latest`, `-1`

Afficher uniquement le dernier point de données unique

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--service`, `-s`

Filtrer par nom de service ou d’application Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--type`

Filtrez par type de service (si —service n’est pas fourni). La version n’est pas requise. Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--tmp`

Rapport sur l’utilisation temporaire du disque (affiche les colonnes : timestamp, service, tmp_used, tmp_limit, tmp_percent, tmp_ipercent)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : horodatage*, service*, utilisé*, limite*, pourcentage*, ipercent*, tmp_percent*, ilimit, iused, tmp_ilimit, tmp_ipercent, tmp_iused, tmp_iused, tmp_limit, tmp_used, type (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `metrics:memory`

```bash
magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Afficher l’utilisation de la mémoire d’un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--bytes`, `-B`

Afficher les tailles en octets

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--range`, `-r`

La période. Les mesures seront chargées pendant cette durée jusqu’à l’heure de fin (—à). Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 5m, maximum 8h ou plus (selon le projet), par défaut 10m.

- Requiert une valeur

#### `--interval`, `-i`

Intervalle de temps. La valeur par défaut est une division de la plage. Vous pouvez indiquer des unités : heures (h), minutes (m) ou secondes (s). Minimum 1m.

- Requiert une valeur

#### `--to`

L&#39;heure de fin. La valeur par défaut est maintenant.

- Requiert une valeur

#### `--latest`, `-1`

Afficher uniquement le dernier point de données unique

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--service`, `-s`

Filtrer par nom de service ou d’application Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--type`

Filtrez par type de service (si —service n’est pas fourni). La version n’est pas requise. Les caractères % ou * peuvent être utilisés comme caractère générique.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : horodatage*, service*, utilisé*, limite*, pourcentage*, type (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `mount:download`

```bash
magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Télécharger des fichiers à partir d’un montage, à l’aide de rsync

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--all`, `-a`

Télécharger depuis tous les montages

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--mount`, `-m`

Le montage (en tant que chemin relatif à l’application)

- Requiert une valeur

#### `--target`

Répertoire dans lequel les fichiers seront téléchargés. Si —all est utilisé, le chemin de montage est ajouté

- Requiert une valeur

#### `--source-path`

Utilisez le chemin source du montage (plutôt que le chemin de montage) comme sous-répertoire de la cible, lorsque —all est utilisé

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--delete`

Supprimer les fichiers superflus dans le répertoire cible

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--exclude`

Fichier(s) à exclure du téléchargement (motif)

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--include`

Fichier(s) à ne pas exclure (motif)

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `mount:list`

```bash
magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Obtention d’une liste des montages

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--paths`

Générez uniquement les chemins de montage (un par ligne).

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : définition, chemin. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `mount:upload`

```bash
magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Chargez des fichiers sur un montage à l’aide de rsync.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--source`

Un répertoire contenant les fichiers à charger

- Requiert une valeur

#### `--mount`, `-m`

Le montage (en tant que chemin relatif à l’application)

- Requiert une valeur

#### `--delete`

Permet de supprimer les fichiers superflus dans le montage

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--exclude`

Fichier(s) à exclure du chargement (modèle)

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--include`

Fichier(s) à ne pas exclure (motif)

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--instance`, `-I`

Identifiant d’une instance

- Requiert une valeur


## `operation:list`

```bash
magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Liste des opérations d’exécution sur un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--full`

Ne limitez pas la longueur de la commande à afficher. La limite par défaut est de 24 lignes.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : service*, nom*, démarrage*, rôle, arrêt (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `operation:run`

```bash
magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

Exécuter une opération sur l’environnement

### Arguments

#### `operation`

Nom de l’opération

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--worker`

Un nom de collaborateur

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `project:clear-build-cache`

```bash
magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

Effacer le cache de génération d’un projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `project:get`

```bash
magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [--] [<project>] [<directory>]
```

Cloner un projet localement

### Arguments

#### `project`

Identifiant du projet


#### `directory`

Répertoire vers lequel effectuer le clonage. La valeur par défaut est le titre du projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--environment`, `-e`

Identifiant d’environnement à cloner. La valeur par défaut est celle du projet ou du premier environnement disponible

- Requiert une valeur

#### `--depth`

Créer un clone superficiel : limite le nombre de validations dans l’historique.

- Requiert une valeur

#### `--build`

Générer le projet après le clonage

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `project:info`

```bash
magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Lire ou définir des propriétés pour un projet

### Arguments

#### `property`

Nom de la propriété


#### `value`

Définissez une nouvelle valeur pour la propriété

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `project:list`

```bash
magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Obtenir une liste de tous les projets actifs

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--pipe`

Génère une liste simple d’identifiants de projet. Désactive la pagination.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--region`

Filtrer par région (correspondance exacte)

- Requiert une valeur

#### `--title`

Filtrer par titre (recherche non sensible à la casse)

- Requiert une valeur

#### `--my`

Afficher uniquement les projets que vous possédez

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--refresh`

Actualisation de la liste

- Valeur par défaut : `1`
- Requiert une valeur

#### `--sort`

Propriété par laquelle effectuer le tri

- Valeur par défaut : `title`
- Requiert une valeur

#### `--reverse`

Tri dans l’ordre inverse (décroissant)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--page`

Numéro de page. Cela permet la pagination, malgré la configuration ou —count. Ignoré si —pipe est spécifié.

- Requiert une valeur

#### `--count`, `-c`

Nombre de projets à afficher par page. Utilisez 0 pour désactiver la pagination. Ignoré si —page est spécifié.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`

Colonnes à afficher. Colonnes disponibles : id*, titre*, région*, created_at, organization_id, organization_label, nom_organisation, type_organisation, statut (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `project:set-remote`

```bash
magento-cloud set-remote [<project>]
```

Définir le projet distant pour le référentiel Git actuel

### Arguments

#### `project`

Identifiant du projet

### Options

Pour les options globales, voir [Options globales](#global-options).


## `repo:cat`

```bash
magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

Lire un fichier dans le référentiel du projet

### Arguments

#### `path`

Chemin d’accès au fichier

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--commit`, `-c`

La validation SHA. Cette méthode accepte également les suffixes « HEAD », et signe d’insertion (^) ou tilde (~) pour les validations parentes.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `repo:ls`

```bash
magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Répertorier les fichiers dans le référentiel du projet

### Arguments

#### `path`

Chemin d’accès à un sous-répertoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--directories`, `-d`

Afficher uniquement les répertoires

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--files`, `-f`

Afficher uniquement les fichiers

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--git-style`

Sortie de style similaire à « git-tree »

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--commit`, `-c`

La validation SHA. Cette méthode accepte également les suffixes « HEAD », et signe d’insertion (^) ou tilde (~) pour les validations parentes.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `repo:read`

```bash
magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Lire un répertoire ou un fichier dans le référentiel du projet

### Arguments

#### `path`

Chemin d’accès au répertoire ou fichier

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--commit`, `-c`

La validation SHA. Cette méthode accepte également les suffixes « HEAD », et signe d’insertion (^) ou tilde (~) pour les validations parentes.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `resources:build:get`

```bash
magento-cloud build-resources:get [-p|--project PROJECT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Affichage des ressources de build d’un projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : processeur, mémoire. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `route:get`

```bash
magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

Afficher des informations détaillées sur un itinéraire

### Arguments

#### `route`

URL d’origine de l’itinéraire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--id`

Identifiant d’itinéraire à sélectionner

- Requiert une valeur

#### `--primary`, `-1`

Sélectionner l&#39;itinéraire principal

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--property`, `-P`

La propriété à afficher

- Requiert une valeur

#### `--refresh`

Contournement du cache des itinéraires

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

[Option obsolète, n’est plus utilisée]

- Requiert une valeur

#### `--identity-file`, `-i`

[Option obsolète, n’est plus utilisée]

- Requiert une valeur


## `route:list`

```bash
magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

Liste de tous les itinéraires pour un environnement

### Arguments

#### `environment`

Identifiant de l’environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Contournement du cache des itinéraires

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : itinéraire*, type*, vers*, URL (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `self:install`

```bash
magento-cloud self:install [--shell-type SHELL-TYPE]
```

Installation ou mise à jour des fichiers de configuration de l’interface de ligne de commande

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--shell-type`

Type de shell pour la saisie semi-automatique (bash ou zsh)

- Requiert une valeur


## `self:update`

```bash
magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

Mettre à jour l’interface de ligne de commande vers la dernière version

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--no-major`

Mise à jour uniquement entre les versions mineures ou de correctif

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--unstable`

Mettre à jour vers une nouvelle version instable, le cas échéant

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--manifest`

Remplacer l&#39;emplacement du fichier manifeste

- Requiert une valeur

#### `--current-version`

Remplacer la version actuelle

- Requiert une valeur

#### `--timeout`

Temporisation de la vérification de version

- Valeur par défaut : `30`
- Requiert une valeur


## `service:list`

```bash
magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Répertorier les services du projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--pipe`

Générer une liste de noms de service uniquement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : disque, nom, taille, type. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `service:mongo:dump`

```bash
magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Créer un vidage d’archives binaires de données à partir de MongoDB

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--collection`, `-c`

La collection à vider

- Requiert une valeur

#### `--gzip`, `-z`

Compresser l’image mémoire à l’aide de gzip

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--stdout`, `-o`

Sortie vers STDOUT au lieu d&#39;un fichier

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `service:mongo:export`

```bash
magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Exporter des données à partir de MongoDB

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--collection`, `-c`

Collection à exporter

- Requiert une valeur

#### `--jsonArray`

Exporter des données sous la forme d’un seul tableau JSON

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--type`

Type d’exportation, par exemple « csv »

- Requiert une valeur

#### `--fields`, `-f`

Les champs à exporter

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `service:mongo:restore`

```bash
magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Restaurer un vidage d’archives binaires de données dans MongoDB

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--collection`, `-c`

Collection à restaurer

- Requiert une valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `service:mongo:shell`

```bash
magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Utilisation du shell MongoDB

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--eval`

Transmettre un fragment JavaScript au shell

- Requiert une valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `service:redis-cli`

```bash
magento-cloud redis [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]...
```

Accès à l’interface de ligne de commande Redis

### Arguments

#### `args`

Arguments à ajouter à la commande redis-cli

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `service:valkey-cli`

```bash
magento-cloud valkey [-r|--relationship RELATIONSHIP] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]...
```

Accès à l’interface de ligne de commande Valkey

### Arguments

#### `args`

Arguments à ajouter à la commande valkey-cli

- Valeur par défaut : `[]`
- Tableau

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `snapshot:create`

```bash
magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Création d’un instantané d’un environnement

### Arguments

#### `environment`

L&#39;environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--live`

Instantané dynamique : n’arrêtez pas l’environnement. S’il est défini, l’environnement reste en cours d’exécution et ouvert aux connexions pendant l’instantané. Cela réduit les temps d’interruption, au risque de sauvegarder les données dans un état incohérent.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `snapshot:delete`

```bash
magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

Supprimer un instantané d’environnement

### Arguments

#### `id`

Identifiant de l’instantané. Obligatoire en mode non interactif.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `snapshot:get`

```bash
magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

Affichage d’un instantané d’environnement

### Arguments

#### `id`

Identifiant de l’instantané. La valeur par défaut est la plus récente.

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété à afficher.

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur


## `snapshot:list`

```bash
magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Liste des instantanés disponibles d’un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `snapshot:restore`

```bash
magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [--no-code] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

Restaurer un instantané d’environnement

### Arguments

#### `snapshot`

Identifiant de l’instantané. La valeur par défaut est la plus récente

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--target`

Environnement dans lequel effectuer la restauration. La valeur par défaut est l&#39;environnement actuel de l&#39;instantané

- Requiert une valeur

#### `--branch-from`

Si la cible —target n&#39;existe pas encore, cela indique le parent du nouvel environnement

- Requiert une valeur

#### `--no-code`

Ne restaurez pas le code, uniquement les données.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `source-operation:list`

```bash
magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Liste des opérations sources sur un environnement

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--full`

Ne limitez pas la longueur de la commande à afficher. La limite par défaut est de 24 lignes.

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : application, commande, opération. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `source-operation:run`

```bash
magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

Exécuter une opération source

### Arguments

#### `operation`

Nom de l’opération

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--variable`

Variable à définir pendant l’opération, au format type:name=value

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `ssh-cert:load`

```bash
magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

Générer un certificat SSH

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh-only`

Actualisez uniquement le certificat, si nécessaire (n’écrivez pas la configuration SSH).

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--new`

Forcer l’actualisation du certificat

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--new-key`

Forcer la génération d&#39;une nouvelle paire de clés

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `ssh-key:add`

```bash
magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

Ajouter une nouvelle clé SSH

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Arguments

#### `path`

Chemin d’accès à une clé publique SSH existante

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--name`

Nom permettant d’identifier la clé

- Requiert une valeur


## `ssh-key:delete`

```bash
magento-cloud ssh-key:delete [<id>]
```

Supprimer une clé SSH

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Arguments

#### `id`

Identifiant de la clé SSH à supprimer

### Options

Pour les options globales, voir [Options globales](#global-options).


## `ssh-key:list`

```bash
magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Obtention d’une liste de clés SSH dans votre compte

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : id*, titre*, chemin*, empreinte (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `subscription:info`

```bash
magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

Lire ou modifier les propriétés d’un abonnement

### Arguments

#### `property`

Nom de la propriété


#### `value`

Définissez une nouvelle valeur pour la propriété

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--id`, `-s`

L’ID d’abonnement

- Requiert une valeur

#### `--date-fmt`

Le format de date (sous forme de chaîne de format de date PHP)

- Valeur par défaut : `c`
- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `tunnel:close`

```bash
magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Fermer les tunnels SSH

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--all`, `-a`

Fermer tous les tunnels

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `tunnel:info`

```bash
magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Afficher les informations de relation pour les tunnels SSH

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

La propriété de relation à afficher

- Requiert une valeur

#### `--encode`, `-c`

Sortie au format JSON codé en base64

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `tunnel:list`

```bash
magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Liste des tunnels SSH

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--all`, `-a`

Afficher tous les tunnels

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : port*, projet*, environnement*, application*, relation*, URL (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `tunnel:open`

```bash
magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Ouverture de tunnels SSH vers les relations d’une application

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--gateway-ports`, `-g`

Autoriser les hôtes distants à se connecter aux ports transférés locaux

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur


## `tunnel:single`

```bash
magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP]
```

Ouverture d’un tunnel SSH unique vers une relation d’application

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--port`

Le port local

- Requiert une valeur

#### `--gateway-ports`, `-g`

Autoriser les hôtes distants à se connecter aux ports transférés locaux

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--app`, `-A`

Nom de l’application distante

- Requiert une valeur

#### `--relationship`, `-r`

La relation de service à utiliser

- Requiert une valeur


## `user:add`

```bash
magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Ajout d’un utilisateur au projet

### Arguments

#### `email`

Adresse e-mail de l’utilisateur

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--role`, `-r`

Le rôle de projet de l’utilisateur (’admin’ ou « observateur ») ou le rôle de type d’environnement (par exemple, « évaluation :contributor » ou « production :viewer). Pour supprimer un utilisateur d’un type d’environnement, définissez le rôle sur « aucun ». Les caractères % ou * peuvent être utilisés comme caractère générique pour le type d’environnement, par exemple « %:viewer » pour donner à l’utilisateur le rôle « Observateur » sur tous les types. Le rôle peut être abrégé, par exemple &#39;production:v.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--force-invite`

Envoyer une invitation, même si elle a déjà été envoyée

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `user:delete`

```bash
magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

Supprimer un utilisateur du projet

### Arguments

#### `email`

Adresse e-mail de l’utilisateur

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `user:get`

```bash
magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

Afficher le(s) rôle(s) d’un utilisateur

### Arguments

#### `email`

Adresse e-mail de l’utilisateur

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--level`, `-l`

Le niveau du rôle (&#39;projet&#39; ou &#39;environnement&#39;)

- Requiert une valeur

#### `--pipe`

Générer le rôle en stout (après avoir effectué les modifications)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--role`, `-r`

[Obsolète : utilisez utilisateur:update pour modifier le(s) rôle(s) d’un utilisateur]

- Requiert une valeur


## `user:list`

```bash
magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Liste des utilisateurs du projet

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : email*, nom*, rôle*, id*, granted_at, updated_at (* = colonnes par défaut). Le caractère « + » peut être utilisé comme espace réservé pour les colonnes par défaut. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur


## `user:update`

```bash
magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Mettre à jour le(s) rôle(s) utilisateur(s) pour un projet

### Arguments

#### `email`

Adresse e-mail de l’utilisateur

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--role`, `-r`

Le rôle de projet de l’utilisateur (’admin’ ou « observateur ») ou le rôle de type d’environnement (par exemple, « évaluation :contributor » ou « production :viewer). Pour supprimer un utilisateur d’un type d’environnement, définissez le rôle sur « aucun ». Les caractères % ou * peuvent être utilisés comme caractère générique pour le type d’environnement, par exemple « %:viewer » pour donner à l’utilisateur le rôle « Observateur » sur tous les types. Le rôle peut être abrégé, par exemple &#39;production:v.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `variable:create`

```bash
magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

Création d’une variable

### Arguments

#### `name`

Le nom de la variable

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--update`, `-u`

Mettez à jour la variable si elle existe déjà

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--level`, `-l`

Le niveau auquel définir la variable (&#39;projet&#39; ou &#39;environnement&#39;)

- Requiert une valeur

#### `--name`

Le nom de la variable

- Requiert une valeur

#### `--value`

Valeur de la variable

- Requiert une valeur

#### `--json`

Si la valeur de la variable est au format JSON

- Valeur par défaut : `false`
- Requiert une valeur

#### `--sensitive`

Indique si la valeur de la variable est sensible

- Valeur par défaut : `false`
- Requiert une valeur

#### `--prefix`

Préfixe du nom de la variable qui peut déterminer son type, par exemple « env ». Applicable uniquement si le nom ne contient pas déjà de préfixe. (ex : &#39;none&#39; ou &#39;env:&#39;)

- Valeur par défaut : `none`
- Requiert une valeur

#### `--enabled`

Indique si la variable doit être activée dans l’environnement.

- Valeur par défaut : `true`
- Requiert une valeur

#### `--inheritable`

Si la variable peut être héritée par les environnements enfants

- Valeur par défaut : `true`
- Requiert une valeur

#### `--visible-build`

Indique si la variable doit être visible au moment de la création

- Requiert une valeur

#### `--visible-runtime`

Indique si la variable doit être visible au moment de l’exécution

- Valeur par défaut : `true`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `variable:delete`

```bash
magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Suppression d’une variable

### Arguments

#### `name`

Le nom de la variable

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--level`, `-l`

Le niveau de variable (&#39;projet&#39;, &#39;environnement&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `variable:get`

```bash
magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

Affichage d’une variable

### Arguments

#### `name`

Nom de la variable

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--property`, `-P`

Afficher une seule propriété de variable

- Requiert une valeur

#### `--level`, `-l`

Le niveau de variable (&#39;projet&#39;, &#39;environnement&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--pipe`

[Option obsolète] générez uniquement la valeur de la variable

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `variable:list`

```bash
magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Variables de liste

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--level`, `-l`

Le niveau de variable (&#39;projet&#39;, &#39;environnement&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : is_enabled, level, name, value. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur


## `variable:update`

```bash
magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Mettre à jour une variable

### Arguments

#### `name`

Le nom de la variable

- Obligatoire

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--allow-no-change`

Retour de la réussite (zéro code de sortie) si aucune modification n’a été fournie

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--level`, `-l`

Le niveau de variable (&#39;projet&#39;, &#39;environnement&#39;, &#39;p&#39; ou &#39;e&#39;)

- Requiert une valeur

#### `--value`

Valeur de la variable

- Requiert une valeur

#### `--json`

Si la valeur de la variable est au format JSON

- Valeur par défaut : `false`
- Requiert une valeur

#### `--sensitive`

Indique si la valeur de la variable est sensible

- Valeur par défaut : `false`
- Requiert une valeur

#### `--enabled`

Indique si la variable doit être activée dans l’environnement.

- Valeur par défaut : `true`
- Requiert une valeur

#### `--inheritable`

Si la variable peut être héritée par les environnements enfants

- Valeur par défaut : `true`
- Requiert une valeur

#### `--visible-build`

Indique si la variable doit être visible au moment de la création

- Requiert une valeur

#### `--visible-runtime`

Indique si la variable doit être visible au moment de l’exécution

- Valeur par défaut : `true`
- Requiert une valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--no-wait`, `-W`

N’attendez pas la fin de l’opération

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--wait`

Attendre la fin de l’opération (par défaut)

- Valeur par défaut : `false`
- N’accepte aucune valeur


## `worker:list`

```bash
magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Obtention d’une liste de tous les collaborateurs déployés

### Options

Pour les options globales, voir [Options globales](#global-options).

#### `--refresh`

Actualisation du cache

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--pipe`

Générer une liste de noms de programmes de travail uniquement

- Valeur par défaut : `false`
- N’accepte aucune valeur

#### `--project`, `-p`

ID ou URL du projet

- Requiert une valeur

#### `--environment`, `-e`

Identifiant d’environnement. Utiliser « . » pour sélectionner l’environnement par défaut du projet.

- Requiert une valeur

#### `--format`

Format de sortie : tableau, csv, tsv ou brut

- Valeur par défaut : `table`
- Requiert une valeur

#### `--columns`, `-c`

Colonnes à afficher. Colonnes disponibles : commandes, nom, type. Les caractères % ou * peuvent être utilisés comme caractère générique. Les valeurs peuvent être fractionnées par des virgules (par exemple, « a, b, c ») et/ou des espaces.

- Valeur par défaut : `[]`
- Requiert une valeur

#### `--no-header`

Ne pas afficher l’en-tête du tableau

- Valeur par défaut : `false`
- N’accepte aucune valeur
