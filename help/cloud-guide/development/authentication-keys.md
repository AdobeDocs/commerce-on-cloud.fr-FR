---
title: Clés d’authentification
description: Découvrez comment appliquer des clés d’authentification à un projet de développement dans Adobe Commerce sur une infrastructure cloud.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Clés d’authentification

Vous devez disposer d’une clé d’authentification pour accéder au référentiel Adobe Commerce et activer les commandes d’installation et de mise à jour pour votre projet d’infrastructure cloud Adobe Commerce. Il existe deux méthodes pour spécifier les informations d’identification d’autorisation du compositeur.

- **fichier d’authentification** : fichier qui contient vos informations d’identification d’Adobe Commerce [informations d’autorisation](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html?lang=fr) dans votre répertoire racine d’Adobe Commerce sur l’infrastructure cloud.
- **variable d’environnement** : variable d’environnement permettant de configurer des clés d’authentification dans votre projet d’infrastructure Adobe Commerce sur le cloud afin d’éviter toute exposition accidentelle.

>[!BEGINSHADEBOX]

**Note de sécurité**

Adobe recommande d’utiliser la méthode [variable d’environnement](#composer-auth-environment-variable) avec votre projet cloud pour éviter l’exposition accidentelle de vos informations d’identification d’autorisation.

La méthode du fichier d’authentification est idéale lors de l’utilisation de Cloud Docker pour Commerce en tant qu’outil de développement local, mais veillez à ne pas charger le fichier `auth.json` dans un référentiel public basé sur Git. Vous pouvez ajouter le fichier `auth.json` au fichier [`.gitignore`](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## Fichier d’authentification

**Pour créer un fichier `auth.json`** :

1. Si votre répertoire racine de projet ne contient pas de fichier `auth.json`, créez-en un.

   - À l’aide d’un éditeur de texte, créez un fichier `auth.json` dans le répertoire racine du projet.
   - Copiez le contenu de l’exemple de `auth.json`[&#128279;](https://github.com/magento/magento2/blob/2.3/auth.json.sample)  dans le nouveau fichier `auth.json`.

1. Remplacez `<public-key>` et `<private-key>` par vos informations d’authentification Adobe Commerce.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Enregistrez vos modifications et quittez l’éditeur de texte.

## Variable d’environnement d’authentification du compositeur

La méthode suivante est le meilleur moyen d’éviter l’exposition accidentelle d’informations d’identification sensibles dans un référentiel public basé sur Git.

**Pour ajouter des clés d’authentification à l’aide d’une variable d’environnement** :

1. Dans la _[!DNL Cloud Console]_, cliquez sur l’icône de configuration sur le côté droit de la navigation du projet.

   ![Configurer le projet](../../assets/icon-configure.png){width="36"}

1. Dans la liste _Paramètres du projet_, cliquez sur **[!UICONTROL Variables]**.

1. Cliquez sur **[!UICONTROL Create variable]**.

1. Dans le champ **[!UICONTROL Variable name]** , saisissez `env:COMPOSER_AUTH`.

1. Dans le champ _Valeur_, ajoutez ce qui suit et remplacez `<public-key>` et `<private-key>` par vos informations d’authentification Adobe Commerce :

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Sélectionnez **[!UICONTROL Available during buildtime]** et désélectionnez **[!UICONTROL Available during runtime]**.

1. Cliquez sur **[!UICONTROL Create variable]**.

1. Supprimez le fichier `auth.json` de chaque environnement.
