---
title: Configurer des notifications
description: Découvrez comment configurer des notifications pour Adobe Commerce sur les environnements d’infrastructure cloud.
feature: Cloud, Configuration, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Configurer des notifications

Par défaut, Adobe Commerce sur l’infrastructure cloud écrit des actions de génération et de déploiement dans le fichier `app/var/log/cloud.log` situé dans le répertoire de l’application racine Adobe Commerce. Vous pouvez éventuellement envoyer des journaux à un système de messagerie, tel qu’un Slack et un e-mail, pour recevoir des notifications en temps réel.

Par exemple, vous pouvez envoyer un message de Slack pour alerter un groupe de personnes lorsqu’un déploiement échoue et demander une enquête sur ce qui s’est passé.

## Notifications de plan

Avant de configurer des notifications, tenez compte des points suivants :

- Quels types de notifications voulez-vous recevoir (messages de Slack, e-mails, les deux) ?
- Quel niveau de détail voulez-vous voir dans les journaux ?
- Où souhaitez-vous configurer les notifications (intégration, évaluation, production) ?

Par exemple, pendant le développement initial, vous pouvez préférer les notifications par e-mail qui affichent des journaux détaillés sur votre environnement d’intégration pour vous aider à déboguer les problèmes avant le déploiement dans l’environnement d’évaluation. Lorsque vous êtes prêt à effectuer un déploiement dans l’environnement d’évaluation ou de production, vous pouvez préférer un message de Slack contenant des informations moins détaillées.

>[!NOTE]
>
>Le fichier de configuration utilisé pour configurer les notifications se trouve à la racine de votre répertoire de projet. Il s’applique donc lorsque vous poussez des modifications vers n’importe quel environnement. Si vous souhaitez personnaliser les notifications par environnement, vous devez modifier le fichier de configuration avant de le transmettre à cet environnement.

## Configuration des notifications

Pour configurer des notifications :

1. Sur votre station de travail locale, accédez au répertoire du projet.
1. Dans le fichier `.magento.env.yaml` de la racine de votre projet, ajoutez les paramètres de votre système de messagerie, y compris les notifications préférées [Niveaux de journal](log-handlers.md#log-levels).

   Par exemple, pour configurer à la fois les configurations de Slack _et_ d’e-mail, utilisez ce qui suit :

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >Adobe Commerce sur les infrastructures cloud envoie uniquement des e-mails pendant la phase de déploiement.

1. Validez et envoyez vos modifications au serveur distant.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Exemple de configuration de Slack

L’exemple suivant illustre une configuration réservée au Slack :

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` : votre Slack [jeton utilisateur](https://api.slack.com/docs/token-types#user). Votre jeton d’utilisateur autorise Adobe Commerce sur l’infrastructure cloud à envoyer des messages.
- `channel` : nom du canal Slack auquel Adobe Commerce sur l’infrastructure cloud envoie des notifications.
- `username` : nom d’utilisateur qu’Adobe Commerce sur l’infrastructure cloud utilise pour envoyer des messages de notification en Slack.
- `min_level` : niveau de journalisation minimal pour les messages de notification. Nous vous recommandons d’utiliser `info`.

### Exemple de configuration d’e-mail

L’exemple suivant illustre une configuration réservée aux e-mails :

>[!NOTE]
>
>Adobe Commerce sur les infrastructures cloud envoie uniquement des e-mails pendant la phase de déploiement.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` : adresse e-mail Adobe Commerce sur l’infrastructure cloud envoie des messages de notification.
- `from` : adresse e-mail à utiliser pour envoyer des messages de notification aux destinataires.
- `subject` : description de l&#39;e-mail.
- `min_level` : niveau de journalisation minimal pour les messages de notification. Nous vous recommandons d’utiliser `notice` ou `warning`.
