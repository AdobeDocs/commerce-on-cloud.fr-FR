---
title: Notifications d’intégrité
description: Découvrez comment configurer les notifications Slack, e-mail et PagerDuty pour l’utilisation de l’espace disque sur votre projet d’infrastructure cloud Adobe Commerce.
feature: Cloud, Observability, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# Notifications d’intégrité

Adobe Commerce sur l’infrastructure cloud surveille l’utilisation de l’espace disque sur toutes les applications et tous les services de votre environnement de démarrage ou de votre environnement d’intégration Pro. Un disque de base de données qui manque d&#39;espace peut provoquer la corruption des données. La vérification du statut d’intégrité se produit toutes les 5 minutes et peut vous avertir par e-mail ou par un autre service externe. Il existe trois avertissements de faible disque pour les notifications d’intégrité :

- **Avertissement** : l’espace disque disponible chute en dessous de 20 %
- **Critique** : l’espace disque disponible chute en dessous de 10 %
- **Tout effacé** : l’espace disque disponible est renvoyé à une valeur supérieure à 20 %, après un événement de disque faible

>[!NOTE]
>
>Sur les environnements de production Pro, vous pouvez utiliser la stratégie d’alerte Managed alerts for Adobe Commerce pour New Relic afin de surveiller l’espace disque. Voir [Surveillance des performances à l’aide d’alertes gérées](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

## Notifications par e-mail

L’intégration de l’e-mail d’intégrité nécessite une adresse d’origine et au moins une adresse de destinataire. Vous pouvez utiliser la même adresse e-mail pour le `from-address` et l’adresse `recipients`. L’exemple suivant enregistre une intégration d’e-mail d’intégrité auprès de deux destinataires :

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Notifications du canal Slack

Slack est un service externe qui utilise des applications interactives appelées robots pour publier des messages dans un salon de chat. Avant de pouvoir recevoir des notifications d’intégrité dans Slack, vous devez créer un [utilisateur de robot](https://api.slack.com/bot-users) personnalisé pour votre groupe de Slack. Après avoir configuré l’utilisateur de robot pour votre ou vos canaux, enregistrez le [ jeton de robot ](https://api.slack.com/docs/token-types#bot) fourni par Slack pour enregistrer votre intégration. L’exemple suivant enregistre les notifications d’intégrité dans un canal de Slack :

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## Notifications PagerDuty

PagerDuty est un service externe qui peut informer les membres de l&#39;équipe sur appel des problèmes importants. Avant de pouvoir recevoir des notifications d’intégrité dans PagerDuty, vous devez créer une intégration [PagerDuty](https://developer.pagerduty.com/v2/docs/integrating) qui utilise l’API d’événements version 2. Utilisez la clé d’intégration, ou _clé de routage_, pour enregistrer votre intégration. L&#39;exemple suivant enregistre des notifications pour PagerDuty à l&#39;aide d&#39;une clé de routage :

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```
