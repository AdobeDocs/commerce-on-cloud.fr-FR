---
title: Gestion des journaux New Relic
description: Découvrez comment utiliser le journal New Relic
feature: Cloud, Logs, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Gestion des journaux New Relic

Tous les projets d’infrastructure cloud incluent la [gestion des journaux New Relic](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). Le service est préconfiguré pour agréger toutes les données de journal de vos environnements d’évaluation et de production et les afficher dans un tableau de bord de gestion des journaux centralisé.

Les données agrégées incluent des informations provenant des journaux suivants :

- Tous les `ece-tools` et journaux d’application du répertoire `~/var/log`
- Journaux des services cloud à partir de l’annuaire `var/log/platform/<project-ID>`
- Réseau CDN et WAF Fastly

Lorsque votre projet est connecté à New Relic, vous pouvez utiliser le service Journaux New Relic pour effectuer des tâches comme celles-ci :

- Utiliser des requêtes New Relic pour rechercher des données de journal agrégées
- Visualiser les données de journal via l’application Journaux New Relic
- Créer des graphiques, des tableaux de bord et des alertes personnalisés
- Résolution des problèmes de performances à partir d’un seul tableau de bord

## Affichage et analyse des données du journal

Utilisez l’application Journaux New Relic pour rechercher dans les données de journal agrégées et résoudre les problèmes liés aux applications, à l’infrastructure, au réseau CDN et aux erreurs WAF. Vous pouvez créer des graphiques, des tableaux de bord et des alertes à l’aide des données de journal collectées auprès des services New Relic APM et Infrastructure.

**Pour utiliser l’application Journaux New Relic** :

1. Connectez-vous à votre compte [New Relic](https://login.newrelic.com/login).

1. Sélectionnez **Journaux** dans le menu de navigation de l’Explorateur.

1. Vérifiez que votre compte est sélectionné en haut de la vue _Tous les journaux_.

1. Sélectionnez une période pour la requête Journaux.

1. Pour passer en revue les données de journal d’infrastructure pour les services cloud (journaux de `~/var/log/`), saisissez la chaîne de requête `has: "filePath"` dans le champ _Rechercher des journaux_. Cliquez ensuite sur **[!UICONTROL Query logs]**.

   Les noms des fichiers journaux sont stockés dans la colonne `filePath` , avec les chemins d’accès complets au fichier journal.

   ![Données de journal du service Cloud Project New Relic](../../assets/new-relic/var-log-query.png)

1. Pour passer en revue les données du journal Fastly, saisissez la chaîne de requête `has: "client_ip"` dans le champ _Rechercher des journaux_. Cliquez ensuite sur **[!UICONTROL Query logs]**.

1. Pour filtrer les résultats du journal Fastly par code de pays, cliquez sur **[!UICONTROL Add column]**, puis sélectionnez **[!UICONTROL geo_country_code]**.

   ![Filtre d’attribut de journal CDN New Relic du projet cloud](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>Vous pouvez enregistrer la vue de la requête à partir de la liste déroulante _Vues enregistrées_. Cliquez sur **[!UICONTROL Create new]**, indiquez un nom, sélectionnez des options, puis cliquez sur **[!UICONTROL Save view]**.
>
>Consultez les sections [Prise en main de la gestion des journaux](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) et [Présentation du langage de requête New Relic](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) sur le site _Documents New Relic_.
