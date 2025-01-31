---
title: Configuration des emails sortants
description: Découvrez comment activer les e-mails sortants pour Adobe Commerce sur les infrastructures cloud.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Configuration des emails sortants

Vous pouvez activer et désactiver les e-mails sortants pour les environnements d’intégration (et d’évaluation pour Starter uniquement) à partir du [!DNL Cloud Console] ou de la ligne de commande. Activez les e-mails sortants pour envoyer une authentification à deux facteurs ou réinitialiser les e-mails de mot de passe pour les utilisateurs de projets cloud.

Par défaut, les e-mails sortants sont activés dans les environnements de production et d’évaluation (Pro uniquement). Cependant, le paramètre **[!UICONTROL Enable outgoing emails]** peut sembler désactivé dans les paramètres d’environnement, quel que soit son statut, jusqu’à ce que vous définissiez la propriété `enable_smtp` via [ligne de commande](#enable-emails-in-the-cli) ou [console Cloud](outgoing-emails.md#enable-emails-in-the-cloud-console).

La mise à jour de la valeur de la propriété `enable_smtp` par [ligne de commande](#enable-emails-in-the-cli) modifie également la valeur du paramètre [!UICONTROL Enable outgoing emails] pour cet environnement sur la console cloud.

>[!NOTE]
>
>L’activation/la désactivation du paramètre **[!UICONTROL Enable outgoing emails]** n’active/désactive pas les e-mails dans les environnements d’évaluation ou de production Pro.

{{redeploy-warning}}

## Activer les e-mails dans la console Cloud

Utilisez le bouton **[!UICONTROL Outgoing emails]** dans la vue _Configurer l’environnement_ pour activer ou désactiver la prise en charge des e-mails.

Si les e-mails sortants doivent être désactivés ou réactivés dans les environnements de production ou d’évaluation Pro, vous pouvez envoyer un [ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Le statut des e-mails sortants peut ne pas être reflété pour les environnements d’évaluation ou de production Pro dans la console cloud.

**Pour gérer la prise en charge des e-mails à partir du[!DNL Cloud Console]** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .
1. Sélectionnez un projet dans la liste _Tous les projets_.
1. Dans le tableau de bord Projet, cliquez sur l’icône de configuration en haut à droite.
1. Cliquez sur **[!UICONTROL Environments]** et sélectionnez un environnement spécifique dans la liste (à l’exception de Évaluation et Production pour Pro).
1. Pour activer ou désactiver les e-mails sortants, activez _Activer les e-mails sortants_ **Activé** ou **Désactivé**.

   ![Activer la configuration du courrier sortant](../../assets/outgoing-emails.png)

Après avoir modifié le paramètre , l’environnement crée et déploie avec la nouvelle configuration.

## Activer les e-mails dans l’interface de ligne de commande

Vous pouvez modifier la configuration du canal e-mail pour un environnement actif à l’aide de la commande `magento-cloud` CLI `environment:info` pour définir la propriété `enable_smtp`. L’activation du protocole SMTP met à jour la variable d’environnement `MAGENTO_CLOUD_SMTP_HOST` avec l’adresse IP de l’hôte SMTP pour l’envoi du courrier.

**Pour gérer la prise en charge des e-mails, procédez comme suit** ligne de commande :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Vérifiez le paramètre d’e-mail sortant pour l’environnement.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Modifiez la configuration de la prise en charge du courrier électronique en définissant la variable d’environnement `enable_smtp` sur `true` ou `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Attendez que l’environnement soit créé et déployé.

1. Utilisez un SSH pour vous connecter à l’environnement distant.

1. Vérifiez que l’e-mail fonctionne ; envoyez un e-mail de test à une adresse que vous pouvez vérifier.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. Vérifiez que l&#39;email est récupéré par SendGrid.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
