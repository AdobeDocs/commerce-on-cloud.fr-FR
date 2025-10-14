---
title: Variables ADMIN
description: Consultez une liste des variables d’environnement utilisées lors de l’installation d’Adobe Commerce sur une infrastructure cloud.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Variables admin

Les utilisateurs disposant d’un accès administratif au projet d’infrastructure d’Adobe Commerce sur le cloud peuvent utiliser les variables d’environnement de projet suivantes pour remplacer les paramètres de configuration du compte utilisateur d’administration afin d’accéder à l’interface utilisateur d’administration.

## Informations d’identification de l’administrateur

Vous pouvez remplacer les informations d’identification de l’utilisateur administrateur lors de l’installation de Commerce par les variables ADMIN dans le tableau suivant.

Si vous souhaitez modifier les valeurs après l’installation, connectez-vous à votre environnement à l’aide de SSH et utilisez la commande de [`admin:user` de l’interface de ligne de commande Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=fr) pour créer ou modifier les informations d’identification de l’utilisateur administrateur.

| Variable | Par défaut | Description |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Adresse électronique du propriétaire de la licence | Nom d’utilisateur de l’utilisateur administrateur pouvant créer d’autres utilisateurs, y compris des utilisateurs administrateurs. |
| `ADMIN_EMAIL` |                             | Adresse électronique de l’utilisateur administrateur. Cette adresse est utilisée pour envoyer des notifications de réinitialisation de mot de passe. |
| `ADMIN_PASSWORD` |                             | Mot de passe de l’utilisateur administrateur. Lorsque le projet est créé, un mot de passe aléatoire est généré et un e-mail est envoyé au propriétaire de la licence. Lors de la création du projet, le propriétaire de la licence aurait déjà dû modifier le mot de passe. Contactez le propriétaire de la licence pour obtenir le nouveau mot de passe. |
| `ADMIN_LOCALE` | `en_US` | Paramètres régionaux par défaut utilisés par l’administrateur. |

## URL de l’administrateur

Utilisez la variable d’environnement suivante pour sécuriser l’accès à votre interface utilisateur d’administration. Si elle est spécifiée, cette valeur remplace l’URL par défaut lors de l’installation.

`ADMIN_URL` : URL relative pour accéder à l’interface utilisateur d’administration. L’URL par défaut est `/admin`. Pour des raisons de sécurité, Adobe vous recommande de modifier la valeur par défaut en une URL d’administration personnalisée et unique, difficile à deviner.

### Modifier l’URL d’administration

Adobe recommande de modifier la variable au niveau de l’environnement pour l’URL d’administration après l’installation. Configurez ce paramètre pour des raisons de sécurité avant d’effectuer une ramification à partir de l’environnement de `master` cloné. Toutes les branches créées à partir de la branche `master` héritent des variables au niveau de l’environnement et de leurs valeurs.

Utilisez la commande `magento-cloud variable:update` pour mettre à jour la valeur de la variable. (La commande `variable:set` a été abandonnée et n’est pas disponible.) L’exemple suivant met à jour ADMIN_URL vers `newAdmin_A8v10` :

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>La valeur `ADMIN_URL` accepte les lettres (a-z ou A-Z), les chiffres (0-9) et le caractère de soulignement (_) pour un chemin d’administration personnalisé. Les espaces ou autres caractères ne sont **pas** acceptés.

**Pour modifier l’URL à l’aide de l’[!DNL Cloud Console]** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Dans la présentation du projet, sélectionnez l’environnement et cliquez sur l’icône de configuration.

   ![&#x200B; Configuration du projet &#x200B;](../../assets/icon-configure.png){width="36"}

1. Sélectionnez l’onglet **Variables**.

1. Cliquez sur **Créer une variable**.

1. Saisissez ce qui suit :

   - **Nom de la variable** = `ADMIN_URL`
   - **value** = Nouvelle URL. Par exemple, définissez l’URL d’administration sur `magento_A8v10`.

   Par défaut, `Available during runtime` et `Make inheritable` sont sélectionnés.

1. Cliquez sur **Créer une variable** et attendez la fin du déploiement. Ce bouton n’est visible que lorsque les champs obligatoires contiennent des valeurs.
