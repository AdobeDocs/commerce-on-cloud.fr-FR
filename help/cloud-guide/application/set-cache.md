---
title: Définir le cache pour les fichiers statiques
description: Découvrez comment définir les options de stockage du cache dans le fichier  [!DNL Commerce]  configuration de l’application.
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 131
ht-degree: 0%

---

# Définir le cache pour les fichiers statiques

La durée de vie (TTL) du cache pour vos médias et fichiers statiques est définie dans le fichier de configuration `.magento.app.yaml` à l’aide de la clé `expires` .

>[!NOTE]
>
>Avant de mettre à jour votre environnement de production, il est important de tester les modifications dans votre environnement d’évaluation. [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket) pour obtenir de l’aide sur la mise à jour de la configuration sur ces environnements.

1. Spécifiez la durée de vie (en secondes) dans la propriété [&#128279;](web-property.md) du fichier `.magento.app.yaml`. `web`Vous pouvez ajouter la clé `expires` sous `locations` ou sous `"/media"` et `"/static"`.

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
