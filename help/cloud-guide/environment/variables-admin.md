---
title: Variables ADMIN
description: Consultez une liste des variables d’environnement utilisées lors de l’installation d’Adobe Commerce sur une infrastructure cloud.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: 4e751f02b92f954cd41d5523237da295a068661a
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Variables admin

Les utilisateurs disposant d’un accès administratif au projet d’infrastructure d’Adobe Commerce sur le cloud peuvent utiliser les variables d’environnement de projet suivantes pour remplacer les paramètres de configuration du compte utilisateur d’administration afin d’accéder à l’interface utilisateur d’administration.

## Informations d’identification de l’administrateur

Vous pouvez remplacer les informations d’identification de l’utilisateur administrateur lors de l’installation de Commerce par les variables ADMIN dans le tableau suivant.

Si vous souhaitez modifier les valeurs après l’installation, connectez-vous à votre environnement à l’aide de SSH et utilisez la commande de [`admin:user` de l’interface de ligne de commande Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) pour créer ou modifier les informations d’identification de l’utilisateur administrateur.

| Variable | Par défaut | Description |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Adresse électronique du propriétaire de la licence | Nom d’utilisateur de l’utilisateur administrateur pouvant créer d’autres utilisateurs, y compris des utilisateurs administrateurs. |
| `ADMIN_EMAIL` |                             | Adresse électronique de l’utilisateur administrateur. Cette adresse est utilisée pour envoyer des notifications de réinitialisation de mot de passe. |
| `ADMIN_PASSWORD` |                             | Mot de passe de l’utilisateur administrateur. Lorsque le projet est créé, un mot de passe aléatoire est généré et un e-mail est envoyé au propriétaire de la licence. Lors de la création du projet, le propriétaire de la licence aurait déjà dû modifier le mot de passe. Contactez le propriétaire de la licence pour obtenir le nouveau mot de passe. |
| `ADMIN_LOCALE` | `en_US` | Paramètres régionaux par défaut utilisés par l’administrateur. |

## URL de l’administrateur

Utilisez la variable d’environnement suivante pour sécuriser l’accès à votre interface utilisateur d’administration. Si elle est spécifiée, cette valeur remplace l’URL par défaut lors de l’installation. Dans [!DNL Adobe Commerce] sur l’infrastructure cloud, vous devez définir ou modifier l’URL d’administration à l’aide de la variable `ADMIN_URL` dans le ([!DNL Cloud Console] ou [!DNL Cloud CLI]). La modification du paramètre à partir du [!DNL Admin] s’applique uniquement aux installations sur site.

`ADMIN_URL` : URL relative pour accéder à l’interface utilisateur d’administration. L’URL par défaut est `/admin`.

### Modifier l’URL d’administration

Par défaut, l’URL [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html) est définie sur *&lt;nom_domaine>/admin*. Pour des raisons de sécurité, Adobe recommande de modifier ce champ en une URL d’administration personnalisée et unique, difficile à deviner.

**Dans [!DNL Adobe Commerce] sur l’infrastructure cloud**, vous devez modifier l’URL d’administration à l’aide de la variable d’environnement `ADMIN_URL` dans le ([!DNL Cloud Console] ou [!DNL Cloud CLI]). La modification du paramètre à partir du [!DNL Admin] s’applique uniquement aux installations sur site. Pour les installations sur site, suivez [utiliser une URL d’administration personnalisée](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html#use-a-custom-admin-url).

Adobe recommande de modifier la variable au niveau de l’environnement pour l’URL d’administration après l’installation. Configurez ce paramètre pour des raisons de sécurité avant d’effectuer une ramification à partir de l’environnement de `master` cloné. Toutes les branches créées à partir de la branche `master` héritent des variables au niveau de l’environnement et de leurs valeurs, sauf si vous définissez l’héritage sur false.

Utilisez l’[!DNL Cloud Console] ou l’[!DNL Cloud CLI] pour définir ou mettre à jour des `ADMIN_URL`.

#### Option A : modifier l’URL d’administration à l’aide de l’[!DNL Cloud Console]

##### Environnement d’intégration

Dans la [console cloud](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html), ajoutez une nouvelle variable avec :

- **Name:** `ADMIN_URL`
- **Valeur :** votre nouvelle URL d’administrateur (par exemple, `magento_A8v10`)

- Pour obtenir des instructions détaillées, consultez [ajout de variables d’environnement](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment) ou [variables d’environnement](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html) dans notre documentation destinée aux développeurs.

##### Définissez l’URL d’administration dans le [!DNL Cloud Console]

1. Connectez-vous à [Cloud Console](https://console.adobecommerce.com/).
2. Sélectionnez un projet dans la liste **[!UICONTROL All projects]**.
3. Dans la présentation du projet, sélectionnez l’environnement et cliquez sur l’icône de configuration.
4. Sélectionnez l’onglet **[!UICONTROL Variables]** .
5. Cliquez sur **[!UICONTROL Create Variable]** (ou modifiez la variable de `ADMIN_URL` existante, le cas échéant).
6. Saisissez ce qui suit :
   - **Nom de la variable :** `ADMIN_URL`
   - **Valeur :** votre nouveau chemin d’accès d’administrateur (par exemple, `magento_A8v10`).

   Par défaut, **[!UICONTROL Available during runtime]** et **[!UICONTROL Make inheritable]** sont sélectionnés. Pour empêcher les environnements enfants d’hériter de cette valeur, effacez les **[!UICONTROL Make inheritable]** de cette variable.
7. Cliquez sur **[!UICONTROL Create variable]** (ou **[!UICONTROL Save]**) et attendez la fin du déploiement. Le bouton n’est visible que lorsque les champs obligatoires contiennent des valeurs.

##### Lorsque les environnements d’évaluation et de production ne sont pas disponibles dans le [!DNL Cloud Console]

[Envoyez un ticket d’assistance](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) en demandant à ajouter la variable `ADMIN_URL` pour votre environnement d’évaluation ou de production. Si l’évaluation et la production sont accessibles à partir du [!DNL Cloud Console], ajoutez la variable comme décrit dans [Environnement d’intégration](#integration-environment).

#### Option B : modifier l’URL d’administration à l’aide de l’[!DNL Cloud CLI]

Utilisez la commande `magento-cloud variable:update` pour mettre à jour la variable. (La commande `variable:set` a été abandonnée et n’est pas disponible.)

L’exemple suivant met à jour la `ADMIN_URL` d’environnement `master` sur `newAdmin_A8v10` et empêche les environnements enfants d’hériter de la valeur :

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **Redéploiement :** la modification de la variable `ADMIN_URL` dans l’[!DNL Cloud CLI] déclenche un redéploiement de l’environnement.
- **Héritage :** les variables peuvent être héritées par défaut. Pour empêcher que la valeur ne soit héritée par les environnements enfants, utilisez l’option `--inheritable false` comme illustré ci-dessous. Pour plus de détails, voir [visibilité au niveau des variables](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html#visibility).

>[!NOTE]
>
>La valeur `ADMIN_URL` accepte les lettres (a-z, A-Z), les chiffres (0-9) et le caractère de soulignement (_). Les espaces ou autres caractères ne sont pas acceptés.
