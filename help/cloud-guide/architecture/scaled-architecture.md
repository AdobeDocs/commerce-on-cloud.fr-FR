---
title: Architecture à grande échelle
description: Découvrez l’architecture à deux niveaux et sa mise à l’échelle pour répondre à la demande.
feature: Cloud, Auto Scaling, Iaas, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Architecture à grande échelle

L’infrastructure cloud s’adapte en fonction des besoins de vos ressources pour atteindre une plus grande efficacité. Adobe Commerce sur les infrastructures cloud surveille vos applications et peut ajuster la capacité pour maintenir des performances stables et prévisibles. La conversion vers cette architecture permet de réduire les problèmes, tels que la latence ou les pics de trafic importants.

>[!NOTE]
>
>L’architecture évolutive est disponible pour Adobe Commerce sur les comptes d’infrastructure cloud avec le cluster Pro 48 ou une version ultérieure.

## Architecture à deux niveaux

Historiquement, l’architecture Pro se composait de trois nœuds, chacun contenant un tech stack complet. Il existe désormais une infrastructure évolutive qui fournit une architecture à plusieurs niveaux avec un minimum de six nœuds : trois nœuds pour la base de données et les services principaux et trois nœuds pour le serveur web. Cette architecture à plusieurs niveaux permet de dimensionner les niveaux indépendamment pour obtenir un équilibre optimal des performances.

### Niveau de service

Il existe trois nœuds de service pour le stockage des données, le cache et les services : **OpenSearch** ou **Elasticsearch**, **MariaDB**, **Redis**, etc. Lorsque le niveau de service approche de la capacité, la seule façon de procéder consiste à augmenter la taille du serveur, par exemple en augmentant l’alimentation et la mémoire du CPU. La capacité est limitée à la taille du nœud disponible. Le cluster de base de données étant conçu pour une haute disponibilité, vous ne pouvez pas effectuer une mise à l’échelle horizontale de manière fiable avec les technologies utilisées.

![Mise à l’échelle du niveau de service](../../assets/scaling-service.png)

Prenons un exemple où le type d’instance de nœud de service est _m5.2xlarge_ avec 32 Go de RAM. Un service, tel que la base de données, utilise une quantité de mémoire considérable (30 Go). La mise à l’échelle vers la taille d’instance disponible suivante _m5.4xlarge_ fournit 64 Go de RAM, ce qui double la mémoire et répond aux besoins croissants de la base de données.

Vous pouvez optimiser davantage les performances du niveau de service en acheminant le trafic en fonction du type de nœud. Par défaut, le nœud de base de données est isolé du trafic Web. Par exemple, vous pouvez choisir de diffuser le trafic web sur le nœud de base de données.

### Niveau web

Il existe trois nœuds web pour le traitement des requêtes et du trafic web : **php-fpm** et **NGINX**. En plus de la mise à l’échelle verticale en augmentant la puissance et la mémoire, le niveau web peut être mis à l’échelle horizontale en ajoutant des serveurs web à un cluster existant lorsqu’il est limité au niveau PHP. Voir [&#x200B; Mise à l’échelle automatique &#x200B;](autoscaling.md) pour savoir comment les nœuds web sont automatiquement mis à l’échelle.

![Mise à l’échelle de niveau web](../../assets/scaling-web.png)

Cela complète la mise à l’échelle verticale fournie par le niveau de service. À mesure que le niveau de service évolue en taille et en puissance pour s’adapter à une utilisation croissante des bases de données et des services, le niveau web évolue en taille, en puissance et en instances pour s’adapter à une augmentation des demandes de processus et à des exigences de trafic plus élevées.

Prenons un exemple où le type d’instance de nœud web est _C5.2xlarge avec huit processeurs et 16 Go de RAM_. Le nombre de requêtes sur le site a considérablement augmenté. Vous pouvez ajouter un nœud C5.2xlarge pour gérer l’augmentation des processus php-fpm ou changer chaque type d’instance en _C5.4xlarge avec 16 CPU et 32 Go de RAM_. L’ajout d’un nœud réduit le risque d’une capacité de pointe insuffisante.

## Structure du projet

Au minimum, les projets Pro avec l&#39;architecture à l&#39;échelle ont six nœuds disponibles.

- 3 nœuds web c5.2xlarge (8 CPU, 16 Go de RAM)

- 3 nœuds de service m5.2xlarge (8 CPU, 32 Go de RAM)

Cependant, chaque projet est unique et nécessite une surveillance des performances pour analyser correctement la gestion des ressources. Chaque compte comprend le service [New Relic](../monitor/new-relic-service.md), qui se connecte automatiquement aux données de l’application et à l’analyse des performances pour fournir une surveillance dynamique du serveur. Plus précisément, vous pouvez utiliser le service New Relic pour surveiller l’utilisation de CPU et de la RAM afin de déterminer les nœuds qui nécessitent des ressources supplémentaires. Lorsqu’une ressource atteint sa capacité maximale ou que vous constatez une dégradation des performances en fonction des analyses, vous pouvez créer une demande afin d’adapter votre infrastructure en fonction de la demande.

### Accès SSH

Certains fichiers et journaux, tels que le répertoire `/app/<project-id>/var/log`, ne sont pas partagés entre les nœuds. Chaque nœud dispose d’un accès SSH unique. Vous ne pouvez pas utiliser l’interface de ligne de commande `magento-cloud` pour vous connecter aux nœuds de service ou web, mais vous pouvez trouver les adresses des nœuds dans la liste Accès SSH de votre [!DNL Cloud Console].

```bash
ssh <node>.<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
```

- `node` 1 à 3 : adresses pour accéder aux nœuds de service

- `node` 4 à _n_ : adresses pour accéder aux nœuds web

>[!TIP]
>
>Après vous être connecté, vous pouvez confirmer l’ID de serveur et le rôle : les nœuds de service utilisent le rôle _unifié_ et les nœuds web utilisent le rôle _web_.

L’exemple de réponse lors de la connexion à un **nœud de service** inclut le rôle _unifié_ :

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:unified.

project-id@server-id:~$
```

L’exemple de réponse lors de la connexion à un **nœud web** inclut le rôle _web_ :

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:web.

project-id@server-id:~$
```

### Emplacements du journal

Les emplacements des journaux varient légèrement en fonction du nœud. Par exemple, un journal de base de données, tel que le journal d’erreurs **MySQL**, est disponible sur un nœud de service (`/var/log/mysql/mysql-error.log`), mais il n’est pas disponible sur un nœud web.

Chaque compte Pro inclut le service [Journaux New Relic](../monitor/new-relic-service.md), qui se connecte automatiquement aux données de journal de l&#39;application pour fournir une gestion dynamique des journaux. Les données de journal agrégées de tous les nœuds s’affichent dans l’application Journaux New Relic afin que vous puissiez résoudre les problèmes de performances sur des nœuds spécifiques à partir d’un seul tableau de bord.
