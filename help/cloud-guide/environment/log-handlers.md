---
title: Gestionnaires de journaux
description: Découvrez comment configurer des gestionnaires de journaux pour Adobe Commerce sur une infrastructure cloud.
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Gestionnaires de journaux

Vous pouvez configurer des gestionnaires de journaux pour envoyer des messages à un serveur de journalisation distant. Un gestionnaire de journaux envoie les journaux de version et de déploiement à d’autres systèmes, de la même manière que vous envoyez les journaux à un Slack et à un e-mail. Vous pouvez activer un gestionnaire _syslog_, idéal pour la journalisation des messages liés au matériel, ou un gestionnaire GELF (Graylog Extended Log Format), idéal pour la journalisation des messages provenant d’applications logicielles.

L’exemple suivant configure ces deux gestionnaires en ajoutant la configuration au fichier `.magento.env.yaml`. Pour les valeurs de niveau de journalisation minimal (`min_level`), voir [Niveaux de journalisation](#log-levels).

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## Niveaux de log

Les niveaux de journal déterminent le niveau de détail des messages de notification. Les catégories de niveau de journal suivantes incluent chaque niveau de journal en dessous. Par exemple, un niveau de `debug` inclut la journalisation de chaque niveau, tandis qu’un niveau de `alert` n’affiche que les alertes et les urgences.

- **debug** : informations de débogage détaillées
- **info** : événements intéressants, comme une connexion utilisateur ou un journal SQL
- **avis** : événements normaux, mais significatifs
- **avertissement** : événements exceptionnels qui ne sont pas des erreurs, tels que l’utilisation d’une API obsolète ou une mauvaise utilisation d’une API
- **error** : erreurs d&#39;exécution ne nécessitant pas d&#39;action immédiate
- **critique** : conditions critiques, telles qu&#39;un composant d&#39;application indisponible ou une exception inattendue
- **alerte** : action immédiate requise, par exemple si un site web est en panne ou si la base de données n’est pas disponible, qui déclenche une alerte par SMS.
- **urgence**—le système est inutilisable
