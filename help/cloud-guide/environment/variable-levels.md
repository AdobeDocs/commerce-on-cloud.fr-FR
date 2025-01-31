---
title: Niveaux et options variables
description: Découvrez les différents niveaux de variable et options utilisés pour personnaliser votre environnement d’exécution de projet Adobe Commerce on cloud infrastructure.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Niveaux variables

Les variables de projet s’appliquent à tous les environnements du projet. Les variables d’environnement s’appliquent à un environnement ou à une branche spécifique. Un environnement _hérite_ des définitions de variable de l’environnement parent.

Vous pouvez remplacer une valeur héritée en définissant la variable spécifique à l’environnement. Par exemple, pour définir des variables pour le développement, définissez les valeurs de variable dans le fichier `.magento.env.yaml` de l’environnement d’intégration. Tous les environnements ramifiés à partir de l’environnement d’intégration héritent de ces valeurs. Voir [Configuration du déploiement](configure-env-yaml.md) pour plus d’informations sur la configuration de votre environnement à l’aide du fichier `.magento.env.yaml`.

>[!BEGINTABS]

>[!TAB CLI]

**Pour définir des variables à l’aide de l’interface de ligne de commande Cloud** :

- **Variables spécifiques au projet** : pour définir la même valeur pour _tous_ les environnements de votre projet. Ces variables sont disponibles lors de la création et de l’exécution dans tous les environnements.

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **Variables spécifiques à un environnement** : pour définir une valeur unique pour un environnement _spécifique_. Ces variables sont disponibles au moment de l’exécution et sont héritées par les environnements enfants. Spécifiez l’environnement dans votre commande à l’aide de l’option `-e` .

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

Après avoir défini des variables spécifiques au projet, vous devez redéployer manuellement l’environnement distant pour que la modification soit prise en compte. Envoyez les nouvelles validations pour déclencher un redéploiement.

>[!TAB  Console ]

**Pour définir des variables à l’aide de l’[!DNL Cloud Console]** :

1. Dans la _[!DNL Cloud Console]_, cliquez sur l’icône de configuration sur le côté droit de la navigation du projet.

   ![Configurer le projet](../../assets/icon-configure.png){width="36"}

1. Pour définir une variable au niveau du projet, sous _Paramètres du projet_ cliquez sur **Variables**.

   ![Variables du projet](../../assets/ui-project-variables.png)

1. Pour définir une variable au niveau de l’environnement, dans la liste _Environnements_, sélectionnez un environnement et cliquez sur l’onglet **[!UICONTROL Variables]** .

   ![Onglet Variables d’environnement](../../assets/ui-environment-variables.png)

1. Cliquez sur **[!UICONTROL Create variable]**.

1. Attribuez un nom et une valeur à la variable. Choisissez l’une des options suivantes :

   - Disponible pendant l’exécution
   - Disponible au moment de la création
   - Valeur JSON
   - Variable sensible (valeur masquée dans les réponses de la console et de l’interface de ligne de commande)
   - Rendre héritable (les environnements enfants peuvent hériter de variables au niveau de l’environnement)

1. Cliquez sur **[!UICONTROL Create variable]**.

>[!CAUTION]
>
>La définition de variables spécifiques à un environnement dans le [!DNL Cloud Console] redéploie automatiquement l’environnement.

>[!ENDTABS]

## Visibilité

Vous pouvez limiter la visibilité d’une variable lors de la création ou de l’exécution à l’aide de la commande `--visible-<build|runtime>`. Il existe également des options pour définir l’héritage et la sensibilité.

Utilisez les options suivantes pour empêcher qu’une variable ne soit vue ou héritée :

- `--inheritable false` : désactive l&#39;héritage pour les environnements enfants. Cela s’avère utile pour définir des valeurs de production uniquement sur la branche `master` et permettre à tous les autres environnements d’utiliser une variable au niveau du projet du même nom.
- `--sensitive true` : marque la variable comme _non lisible_ dans le [!DNL Cloud Console]. Vous ne pouvez pas afficher la variable dans l’interface utilisateur. Cependant, vous pouvez afficher la variable à partir du conteneur d’applications, comme toute autre variable.

L’exemple suivant illustre un cas spécifique où une variable ne peut pas être vue ou héritée. Vous pouvez uniquement spécifier ces options dans l’interface de ligne de commande. Ce cas ne concerne pas toutes les variables d’environnement disponibles.

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## Vérifier les niveaux et les valeurs des variables

Vous pouvez afficher une liste de variables existantes à l’aide de l’interface de ligne de commande Cloud.

```bash
magento-cloud variables
```

```
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
