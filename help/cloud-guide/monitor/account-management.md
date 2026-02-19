---
title: Gestion de compte New Relic
description: Découvrez comment accéder à votre compte New Relic et gérer l’accès, les intégrations et l’utilisation des outils pour votre projet d’infrastructure Adobe Commerce sur cloud.
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 558c645e353e38ce8455ef17e1d0e9fa99b22c6e
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# Gestion des comptes New Relic

Lorsqu’Adobe met en service votre projet d’infrastructure cloud, le propriétaire de la licence reçoit un courrier électronique de New Relic contenant des informations d’identification et des instructions pour accéder au compte New Relic. Si vous n’avez pas reçu l’e-mail, utilisez l’adresse e-mail du propriétaire de la licence pour réinitialiser le mot de passe New Relic.

Si le Propriétaire de la licence a été modifié et que le nouveau Propriétaire de la licence n’a pas actuellement accès à New Relic, [envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

## Gérer l’accès des utilisateurs (rôle d’administrateur)

>[!NOTE]
>
>Accordez uniquement un accès complet aux utilisateurs et utilisatrices qui ont strictement besoin d’accéder à l’ensemble des fonctionnalités.

**Pour accéder à User Management dans New Relic** :

1. Connectez-vous à votre compte [New Relic](https://login.newrelic.com/login).

1. Sélectionnez votre nom d’utilisateur dans le volet de navigation inférieur gauche.

1. Cliquez sur **[!UICONTROL Administration]** et sélectionnez l’une des options suivantes dans la liste :

   - **[!UICONTROL User management]** d’ajouter un utilisateur et de gérer les utilisateurs actifs et les invitations en attente.

   - **[!UICONTROL Access management]** de gérer les groupes d’utilisateurs, les rôles et les comptes.

Voir [Gestion des utilisateurs](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) dans la documentation de _New Relic_.

## Configuration de New Relic pour l’environnement de démarrage

>[!NOTE]
>
>Les **environnements Pro** sont préconfigurés pour utiliser les services New Relic et peuvent ignorer les instructions d’activation et de connexion. Si New Relic APM n’est pas installé sur les environnements d’évaluation et de production ou si l’infrastructure New Relic n’est pas disponible dans l’environnement de production, [envoyez un ticket de support Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour demander l’installation.

Pour les environnements de démarrage, vous devez vérifier le fichier `.magento.app.yaml` pour vous assurer que la section `runtime` inclut l’extension New Relic. Si l’extension n’a pas été configurée, ajoutez ce qui suit :

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Appliquer la clé de licence

Pour connecter un environnement cloud à New Relic, ajoutez la clé de licence New Relic à l’environnement.

- Pour les projets **Pro**, Adobe ajoute la clé de licence à vos environnements de production et d’évaluation pendant le processus d’approvisionnement. Vous pouvez vous connecter à votre compte [New Relic](https://login.newrelic.com/login) pour vérifier la connectivité entre votre Adobe Commerce sur le site d’infrastructure cloud et New Relic.

- Pour les **projets de démarrage**, vous disposez d’une clé de licence New Relic qui prend en charge jusqu’à _trois_ environnements. Vous devez ajouter manuellement la clé aux configurations de votre environnement. Les environnements de démarrage ne sont pas préconfigurés pour utiliser le service New Relic.

Pour les environnements de démarrage, activez l’intégration de New Relic en ajoutant la clé de licence New Relic à la configuration de l’environnement. Ajoutez la clé aux environnements d’évaluation et de production et à un autre environnement de votre choix. Seule la clé de licence New Relic est requise pour la configuration. Vous trouverez des informations sur les options de configuration supplémentaires dans la rubrique [Rapports New Relic](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) dans le _Guide de l’utilisateur d’Adobe Commerce_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Identifiants de connexion pour la page compte Adobe Commerce ou pour la licence New Relic associée à votre projet
>- [Accès de niveau administrateur](../project/user-access.md) aux environnements de démarrage à configurer
>- Informations d’identification pour accéder à l’[administrateur](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) pour l’environnement

**Pour configurer New Relic pour les environnements de démarrage** :

1. Recherchez votre clé de licence New Relic à partir de l’interface de ligne de commande [!DNL Cloud Console] ou Cloud.

   **[!DNL Cloud Console]méthode** :

   - Ouvrez votre projet cloud [page du compte](https://accounts.magento.cloud/user).

   - Dans l’onglet _Projets_, recherchez votre projet.

   - Cliquez sur **Afficher les détails** pour obtenir des informations sur l’infrastructure du projet.

   - Développez la section **Service New Relic** pour afficher la clé de licence.

   - Copiez la clé de licence.

   **Méthode d’interface de ligne de commande Cloud** :

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Ajoutez la clé de licence New Relic à un environnement à l’aide de l’interface de ligne de commande `magento-cloud`.

   - Modification de l’environnement qui nécessite la clé de licence.
   - Mettez à jour la valeur de la variable à l’aide de la commande d’interface de ligne de commande `magento-cloud` suivante :

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Vous pouvez éventuellement l’ajouter à partir de l’[administration Commerce](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store).

1. Connectez-vous à votre compte [New Relic](https://login.newrelic.com/login) pour vérifier que vous pouvez afficher les données de l’environnement Adobe Commerce. Voir [Étude des performances](investigate-performance.md).

### Supprimer la clé de licence

Vous ne pouvez utiliser votre clé de licence New Relic que sur trois environnements actifs. Si la clé est utilisée dans trois environnements, vous devez supprimer la clé de l’un des environnements avant de pouvoir l’ajouter à un autre environnement.

**Pour supprimer une clé de licence d’un environnement** :

1. Liste des variables d’environnement.

   ```bash
   magento-cloud variable:list
   ```

   Exemple de réponse :

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Si vous avez ajouté la clé de licence sous la forme d’une variable _project_, vous devez supprimer cette variable au niveau du projet. Une variable de projet ajoute la licence à _chaque_ branche d’environnement créée, ce qui peut consommer ou dépasser la limite de licence. Pour répertorier les variables de projet : `magento-cloud variable:list --level project`

1. Supprimez la variable de licence.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```

## Modification du propriétaire de compte pour New Relic sur Cloud

Pour modifier le propriétaire du compte New Relic de votre projet d’infrastructure cloud Adobe Commerce :

1. **Modifiez le propriétaire** dans l’interface utilisateur de New Relic. Consultez [Modification du propriétaire du compte](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/account-user-mgmt-tutorial/) dans la documentation de New Relic.

2. **Ajoutez d’abord l’utilisateur** s’il ne figure pas déjà sur votre compte. Voir [Ajouter et mettre à jour des utilisateurs](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/#add-users) dans la documentation de New Relic.

3. **Besoin d’aide ?** Si aucun propriétaire ou administrateur existant ne peut vous aider, tout utilisateur Adobe Commerce ayant accès au compte [Propriétaire du partenariat Adobe Commerce](https://account.newrelic.com/accounts/1311131/users) peut ajouter des utilisateurs en votre nom.

Pour plus d’informations, consultez la présentation du service New Relic [&#128279;](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service).
