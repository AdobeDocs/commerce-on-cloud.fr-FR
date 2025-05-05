---
title: Surveillance de New Relic
description: Découvrez comment accéder à votre tableau de bord New Relic et analyser les données de votre projet d’infrastructure Adobe Commerce on cloud.
feature: Cloud, Observability
topic: Performance
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# Surveillance de New Relic

New Relic connecte et surveille votre infrastructure et votre application [!DNL Commerce] à l’aide d’agents PHP. Une fois qu’un environnement cloud s’est connecté à New Relic, vous pouvez vous connecter à votre compte New Relic pour consulter les données collectées par l’agent.

Sur la page _APM &amp; Services_, sélectionnez le **Résumé** pour afficher les informations transactionnelles relatives à votre application. Cette vue vous permet d’identifier les échecs potentiels et de vérifier l’intégrité globale de votre application et de vos services.

![Page de présentation de Cloud Project New Relic](../../assets/new-relic/dashboard.png)

Cette vue vous permet de suivre les transactions qui rencontrent des réponses lentes ou des goulots d’étranglement, le débit de l’application, des erreurs web, etc.

Vérifier les données suivies :

- **Plus chronophage** : déterminez le temps nécessaire en suivant les demandes en parallèle. Par exemple, vous pouvez avoir le temps de transaction le plus élevé passé dans les vues de produit et de catégorie. Si la page d’un compte client apparaît soudainement comme une page à forte consommation de temps, il se peut que votre application soit affectée par une performance d’appel ou de glisser-déposer des requêtes.

- **Débit le plus élevé** : identifie les pages les plus visitées en fonction de la taille et de la fréquence des octets transmis.

Toutes les données collectées indiquent le temps passé sur les actions qui transmettent des données, des requêtes ou des données _Redis_. Si les requêtes provoquent des problèmes, New Relic fournit des informations pour suivre ces problèmes et y répondre.

>[!TIP]
>
>Pour plus d’informations sur l’utilisation de ces données pour résoudre les problèmes de performances des applications, voir [Résolution des problèmes de performances à l’aide de New Relic](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html?lang=fr) dans le _Centre d’aide Adobe Commerce_.

## Surveillance des performances à l’aide d’alertes gérées

Adobe fournit la stratégie d’alerte _Alertes gérées pour Adobe Commerce_ pour effectuer le suivi des mesures de performances. La politique comprend un ensemble d’alertes qui définissent des seuils et déclenchent des avertissements et des notifications critiques lorsque des problèmes d’infrastructure ou d’application affectent les performances du site. La politique effectue le suivi des mesures suivantes sur les environnements de production :

| Mesure | Collecte de données | Disponibilité |
|:-------------------|:----------------|:----------------|
| score [!DNL Apdex] | APM | Pro et Starter |
| Utilisation de CPU | NRI | Pro |
| Espace disque | NRI | Pro |
| Taux d’erreurs | APM | Pro et Starter |
| Utilisation de la mémoire | NRI | Pro |
| Chargement des requêtes MariaDB | NRI | Pro |
| Mémoire Redis | NRI | Pro |

Lorsque l’infrastructure du site ou les conditions d’application déclenchent un seuil d’alerte, New Relic envoie des notifications d’alerte afin que vous puissiez résoudre le problème de manière proactive. Voir [Alertes gérées pour Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=fr) dans le _Centre d’aide d’Adobe Commerce_ pour plus d’informations sur les seuils d’alerte et les étapes de dépannage afin de résoudre les problèmes qui ont déclenché l’alerte.

>[!TIP]
>
>Pour les environnements d’évaluation et d’intégration Pro et les environnements de démarrage, utilisez les notifications [Health](../integrations/health-notifications.md) pour surveiller l’espace disque.

>[!PREREQUISITES]
>
>- **Informations d’identification New Relic** : informations d’identification de connexion au compte New Relic pour votre projet cloud.
>- **Intégration active de New Relic** : vérifiez que votre environnement cloud est connecté à New Relic.
>- **Notification du workflow** : configurez au moins un [workflow](#set-up-a-workflow-for-notifications) pour recevoir les notifications d&#39;alerte.

**Pour consulter la politique Alertes gérées pour Adobe Commerce** :

1. Connectez-vous à votre compte [New Relic](https://login.newrelic.com/login).

1. Recherchez la stratégie _Alertes gérées pour Adobe Commerce_ :

   - Dans le menu de navigation de l’Explorateur, cliquez sur **[!UICONTROL Alerts & AI]**.

   - Sous _Détecter_, cliquez sur **[!UICONTROL Alert Conditions & Policies]**.

   - Vérifiez que votre compte est sélectionné en haut de la vue _Conditions et politiques d’alerte_.

   - Dans la liste _Politique_, sélectionnez la politique **Alertes gérées pour Adobe Commerce**.

     ![Politiques d’alerte générées](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >Si la stratégie _Alertes gérées pour Adobe Commerce_ n’est pas disponible, consultez la section [Alertes gérées pour Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html?lang=fr) dans le _Centre d’aide d’Adobe Commerce_.

1. Cliquez sur l’onglet **[!UICONTROL Alert conditions]** pour consulter les conditions d’alerte définies dans la politique.

## Création de politiques d’alerte

Ne modifiez aucune alerte incluse dans la stratégie Alertes gérées pour Adobe Commerce . L’Adobe met à jour et améliore les conditions d’alerte de cette politique au fil du temps, ce qui remplace toutes les personnalisations que vous ajoutez à la politique.

Au lieu de modifier une alerte existante, vous pouvez créer une politique d’alerte. Copiez ensuite les conditions d’alerte dans la nouvelle politique.

>[!TIP]
>
>Consultez [Présentation des alertes](https://docs.newrelic.com/docs/alerts/overview/) dans la documentation de _New Relic_ pour plus d’informations sur les alertes, les politiques d’alerte et les workflows.

## Configurer un workflow pour les notifications

Vous pouvez désormais configurer un _workflow_, anciennement appelé canal de notification, pour recevoir des notifications sur les performances de votre site en fonction des données filtrées, telles qu’une politique d’alerte. Les notifications concernant les problèmes de performances sont envoyées à tous les workflows associés à une politique d’alerte lorsque les conditions de l’application ou de l’infrastructure déclenchent une alerte. Vous recevez également des notifications lorsqu’un problème est confirmé et clos.

New Relic fournit des modèles pour la configuration de différents types de notifications de workflow, notamment les e-mails, les Slack, PagerDuty, les Webhooks, etc.

**Pour configurer un workflow** :

1. Connectez-vous à votre compte [New Relic](https://login.newrelic.com/login).

1. Créez un workflow.

   - Dans le menu de navigation de l’Explorateur, cliquez sur **[!UICONTROL Alerts & AI]**.

   - Dans le volet de navigation de gauche, sous _Enrichir et informer_, cliquez sur **[!UICONTROL Workflows]**.

   - Cliquez sur **[!UICONTROL Add a workflow]** sur le côté droit.

     ![New Relic ajouter un workflow](../../assets/new-relic/add-a-workflow.png)

   - Sur la page _Configurer votre workflow_, saisissez un nom pour le workflow.

   - Dans la section _Filtrer les données_, sélectionnez **[!UICONTROL Managed Alerts for Adobe Commerce]** dans la liste déroulante **[!UICONTROL Policy]**.

   - Dans la section _Notifier_, sélectionnez un canal et suivez les instructions.

   - Cliquez sur **[!UICONTROL Test workflow]** pour vérifier votre configuration.

1. Cliquez sur **[!UICONTROL Activate workflow]**.

Consultez la documentation de New Relic sur les [workflows](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Les alertes de la stratégie Alertes gérées pour Adobe Commerce comportent des workflows par défaut configurés pour informer les équipes d’Adobe qui prennent en charge Adobe Commerce sur les clients d’infrastructure cloud. Ne modifiez pas la configuration de ces canaux par défaut et ne supprimez aucune stratégie d’alerte qui leur est attribuée.
