---
title: Messages d'erreur concernant le module ECE-Tools
description: Consultez une liste des codes d’erreur et des messages qui peuvent se produire pendant les processus de création, de déploiement et de post-déploiement d’Adobe Commerce sur l’infrastructure cloud.
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Messages d&#39;erreur pour ECE-Tools

Cette référence de message d’erreur fournit des informations permettant de résoudre les erreurs qui peuvent se produire pendant les processus de création, de déploiement et de post-déploiement d’Adobe Commerce sur l’infrastructure cloud.

Tous les messages d’erreur critiques et d’avertissement qui se produisent pendant le déploiement sont écrits dans les fichiers `var/log/cloud.log` et `/var/log/cloud.error.log`. Le fichier journal des erreurs cloud contient uniquement les erreurs du dernier déploiement. Un fichier vide indique un déploiement réussi sans erreur.

Dans le fichier `cloud.error.log`, chaque entrée est formatée sous la forme d’une chaîne JSON pour une analyse plus facile :

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

Les messages d’erreur sont classés par l’une des étapes de déploiement : version, déploiement et post-déploiement. Chaque section fournit une liste des erreurs associées avec les informations suivantes pour chaque erreur :

- **Code d’erreur** : identifiant attribué par Adobe Commerce au message d’erreur
- **Phase** : indique si l’erreur s’est produite lors de l’étape de création, de déploiement ou de post-déploiement
- **Étape** : indique l’étape du scénario de déploiement qui peut renvoyer l’erreur. Si la colonne _Étape_ est vide, l’erreur est une erreur générale qui peut être renvoyée par plusieurs étapes ou lors d’opérations de prétraitement. Voir [Déploiement basé sur un scénario](../deploy/scenario-based.md) pour plus d’informations sur les étapes de création, de déploiement et de post-déploiement.
- **Suggestion** : fournit des conseils pour dépanner et résoudre l’erreur
- **Titre (description de l’erreur)** : description qui résume la cause de l’erreur
- **Type** : indique si l’erreur est une erreur critique ou un avertissement

{{$include /help/_includes/automated/ece-tools-error-codes.md}}

<!-- Last updated from includes: 2025-05-28 21:01:41 -->
