---
title: Configuration du service MySQL
description: Découvrez comment gérer le service MySQL pour le stockage persistant des données avec Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# Configuration du service MySQL

Le service `mysql` fournit un stockage de données persistant basé sur les versions 10.2 à 10.4 de [MariaDB](https://mariadb.com/), prenant en charge le moteur de stockage [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) et les fonctionnalités réimplémentées de MySQL 5.6 et 5.7.

La réindexation sur MariaDB 10.4 prend plus de temps que les autres versions de MariaDB ou MySQL. Voir [Indexeurs](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html?lang=fr#indexers) dans le guide _Bonnes pratiques de performance_.

>[!WARNING]
>
>Soyez prudent lors de la mise à niveau de MariaDB de la version 10.1 vers la version 10.2. MariaDB 10.1 est la dernière version qui prend en charge _XtraDB_ comme moteur de stockage. MariaDB 10.2 utilise _InnoDB_ pour le moteur de stockage. Après la mise à niveau de la version 10.1 vers la version 10.2, vous ne pouvez pas annuler la modification. Adobe Commerce prend en charge les deux moteurs de stockage. Vous devez toutefois vérifier les extensions et les autres systèmes utilisés par votre projet pour vous assurer qu’ils sont compatibles avec MariaDB 10.2. Voir [Modifications incompatibles entre les versions 10.1 et 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Pour activer MySQL** :

1. Ajoutez le nom, le type et la valeur de disque requis (en Mo) au fichier `.magento/services.yaml`.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >Les erreurs MySQL, telles que `PDO Exception: MySQL server has gone away`, peuvent se produire en raison d’un espace disque insuffisant. Vérifiez que vous avez alloué suffisamment d’espace disque au service dans le fichier [`.magento/services.yaml`](services-yaml.md#disk).

1. Configurez les relations dans le fichier `.magento.app.yaml`.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Vérifier les relations de service](services-yaml.md#service-relationships).

{{service-change-tip}}

## Configurer la base de données MySQL

Les options suivantes s’offrent à vous lors de la configuration de la base de données MySQL :

- **`schemas`** : un schéma définit une base de données. Le schéma par défaut est la base de données `main`.
- **`endpoints`** : chaque point d’entrée représente des informations d’identification avec des privilèges spécifiques. Le point d’entrée par défaut est `mysql`, qui a un accès `admin` à la base de données `main`.
- **`properties`** : les propriétés sont utilisées pour définir des configurations de base de données supplémentaires.

Voici un exemple de configuration de base dans le fichier `.magento/services.yaml` :

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

L’`properties` dans l’exemple ci-dessus modifie les paramètres de `optimizer` par défaut comme [recommandé dans le guide des bonnes pratiques de performance](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html?lang=fr#indexers).

**Options de configuration de MariaDB** :

| Options | Description | Valeur par défaut |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Jeu de caractères par défaut. | utf8mb4 |
| `default_collation` | Classement par défaut. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Taille maximale des paquets, en Mo. Plage `1` à `100`. | 16 |
| `optimizer_switch` | Définissez des valeurs pour l’optimiseur de requête. Voir [documentation MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Sélectionnez les statistiques utilisées par l&#39;optimiseur. Plage `1` à `5`. Voir [documentation MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 pour 10.4 et versions ultérieures |

### Configurer plusieurs utilisateurs de base de données

Vous pouvez éventuellement configurer plusieurs utilisateurs avec des autorisations différentes pour accéder à la base de données `main`.

Par défaut, un point d’entrée nommé `mysql` dispose d’un accès administrateur à la base de données. Pour configurer plusieurs utilisateurs de la base de données, vous devez définir plusieurs points d’entrée dans le fichier `services.yaml` et déclarer les relations dans le fichier `.magento.app.yaml`. Pour les environnements d’évaluation et de production Pro, [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour demander l’utilisateur supplémentaire.

Utilisez un tableau imbriqué pour définir les points d’entrée pour un accès utilisateur spécifique. Chaque point d’entrée peut désigner l’accès à un ou plusieurs schémas (bases de données) et différents niveaux d’autorisation sur chacun d’eux.

Les niveaux d’autorisation valides sont les suivants :

- `ro` : seules les requêtes SELECT sont autorisées.
- `rw` : les requêtes SELECT et les requêtes INSERT, UPDATE et DELETE sont autorisées.
- `admin` : toutes les requêtes sont autorisées, y compris les requêtes DDL (CREATE TABLE, DROP TABLE, etc.).

Par exemple :

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

Dans l’exemple précédent, le point d’entrée `admin` fournit un accès de niveau administrateur à la base de données `main`, le point d’entrée `reporter` fournit un accès en lecture seule et le point d’entrée `importer` un accès en lecture/écriture, ce qui signifie :

- L’utilisateur `admin` contrôle entièrement la base de données.
- L’utilisateur `reporter` dispose uniquement des privilèges SELECT.
- L’utilisateur `importer` dispose des privilèges SELECT, INSERT, UPDATE et DELETE.

Ajoutez les points d’entrée définis dans l’exemple ci-dessus à la propriété `relationships` du fichier `.magento.app.yaml`. Par exemple :

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Si vous configurez un utilisateur MySQL, vous ne pouvez pas utiliser le mécanisme de contrôle d’accès [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) pour les procédures stockées et les vues.

## Connexion à la base de données

Pour accéder directement à la base de données MariaDB, vous devez utiliser un SSH pour vous connecter à l’environnement cloud distant et vous connecter à la base de données.

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Récupérez les informations d’identification de connexion MySQL à partir des propriétés `database` et `type` de la variable [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships).

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Dans la réponse, recherchez les informations MySQL. Par exemple :

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Connexion à la base de données.

   - Pour Commencer, utilisez la commande suivante :

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Pour Pro, utilisez la commande suivante avec le nom d&#39;hôte, le numéro de port, le nom d&#39;utilisateur et le mot de passe récupérés de la variable `$MAGENTO_CLOUD_RELATIONSHIPS`.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>Vous pouvez utiliser la commande `magento-cloud db:sql` pour vous connecter à la base distante et exécuter des commandes SQL.

## Connexion à la base de données secondaire

>[!IMPORTANT]
>
>Cette fonctionnalité est disponible uniquement sur les clusters de production et d’évaluation Pro.

Parfois, vous devez vous connecter à la base de données secondaire pour améliorer les performances de la base de données ou résoudre les problèmes de verrouillage de la base de données. Si cette configuration est requise, utilisez `"port" : 3304` pour établir la connexion. Voir la rubrique [Bonnes pratiques pour configurer la connexion esclave MySQL](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html?lang=fr) dans le guide _Bonnes pratiques d’implémentation_.

## Dépannage

Consultez les articles d’assistance Adobe Commerce suivants pour obtenir de l’aide sur la résolution des problèmes MySQL :

- [Vérification des requêtes lentes et des processus MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html?lang=fr)
- [Créer une image mémoire de base de données sur le cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html?lang=fr)
- [ Dépannage de l’outil de migration de données ](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html?lang=fr)
- [Mise à niveau d’Adobe Commerce : tableaux compacts à dynamiques 2.2.x, 2.3.x vers 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html?lang=fr)
