---
title: Définir le cache pour les fichiers statiques
description: Découvrez comment définir les options de stockage du cache dans le fichier  [!DNL Commerce]  configuration de l’application.
feature: Cloud, Configuration, Cache, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Définir le cache pour les fichiers statiques

La durée de vie (TTL) du cache pour vos médias et fichiers statiques est définie dans le fichier de configuration `.magento.app.yaml` à l’aide de la clé `expires` .

>[!NOTE]
>
>Avant de mettre à jour votre environnement de production, il est important de tester les modifications dans votre environnement d’évaluation. [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour obtenir de l’aide sur la mise à jour de la configuration sur ces environnements.

1. Spécifiez la durée de vie (en secondes) dans la propriété [`web`](web-property.md) du fichier `.magento.app.yaml`. Vous pouvez ajouter la clé `expires` sous `locations` ou sous `"/media"` et `"/static"`.

   Pour empêcher le cache d’expirer, utilisez la paire clé-valeur `expires: -1`. Voir l’exemple suivant :

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```
