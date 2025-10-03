---
title: Projet d’infrastructure cloud
description: Lisez une présentation d’Adobe Commerce sur l’infrastructure cloud et apprenez  [!DNL Cloud Console]  accéder aux paramètres du compte.
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 8eed04c7-6469-45a4-aa89-dc594c977264
source-git-commit: 00b1b6578c226a304697963d17ba349ea17da260
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# Projet d’infrastructure cloud

Le projet d’infrastructure cloud d’Adobe Commerce comprend tout le code des branches Git, des environnements associés et des scripts pour déployer l’application [!DNL Commerce]. Les environnements contiennent des services pour prendre en charge l’application [!DNL Commerce], notamment une base de données, un serveur web et un serveur de mise en cache.

Adobe fournit un [!DNL Cloud Console] et des outils de développement pour gérer entièrement tous les aspects de votre projet. En tant que propriétaire du compte, vous disposez d’un accès complet à tous les environnements.

## [!DNL Cloud Console]

Le [!DNL Cloud Console] fournit des méthodes interactives pour créer, gérer et déployer le code Commerce dans un format convivial. [Connectez-vous à [!DNL Cloud Console]](https://console.adobecommerce.com) pour afficher la liste de vos projets. Vous pouvez uniquement afficher les projets auxquels vous avez l’autorisation d’accès en tant qu’administrateur ou pour des types d’environnement spécifiques. Si vous êtes un partenaire Adobe Solutions, plusieurs projets peuvent s’afficher pour les clients que vous prenez en charge.

>[!TIP]
>
>Si vous ne voyez aucun projet, vous devez contacter le [propriétaire du compte ou administrateur de projet](../project/user-access.md) associé au projet et demander l’accès. Pour les nouveaux utilisateurs, consultez la [rubrique Intégration](../../get-started/onboarding.md#cloud-console) dans le guide _Prise en main_.

La vue _Tous les projets_ répertorie tous les projets auxquels vous avez accès. Vous pouvez cliquer sur **[!UICONTROL Show filters]** et filtrer la liste des projets par type, région ou plan.

![Liste de projets](../../assets/ui-allprojects-list.png)

### Présentation du projet

Lorsque vous sélectionnez un projet dans la liste _Tous les projets_, vous accédez à la vue d’ensemble du projet. La présentation du projet affiche toujours une barre de navigation du projet, qui comprend un sélecteur d’environnement et un bouton de configuration :

![ Navigation dans le projet ](../../assets/project-nav.png)

Tant que vous n’avez pas sélectionné d’environnement, la vue d’ensemble du projet affiche un résumé des détails du projet dans la zone de prévisualisation :

- Nom du projet
- Région, ID de projet
- Planification, stockage alloué, environnements, utilisateurs
- URL du storefront avec bouton **[!UICONTROL Set a custom domain]**

Et dans la présentation du projet principal :

- La vue Environnements affiche une vue en liste ou en arborescence des environnements ![branche active](../../assets/icon-active.png){width="32"} (active) et ![branche inactive](../../assets/icon-inactive.png){width="32"} (inactive).
- Le [flux d’activités](activity-stream.md) affiche les activités en cours, en attente et récentes pour le projet.
<!-- - Apps & Services—Shows a topology of service containers -->

Pour les projets **de démarrage**, il existe une hiérarchie de branches commençant par `master` (production). Toute branche que vous créez s’affiche en tant qu’enfants à partir de la branche `master`. Adobe recommande de créer une branche `staging`, puis une branche `integration` pour le développement. Voir [ Architecture de démarrage ](../architecture/starter-architecture.md).

Pour **Pro**, il existe une hiérarchie de branches allant de `production` à `staging` et à `integration`. L’icône ![icône dédiée](../../assets/icon-dedicated.png){width="32"} indique que la branche se déploie vers un environnement dédié. Toutes les branches que vous créez s’affichent en tant qu’enfants de la branche `integration`. Voir [Architecture Pro](../architecture/pro-architecture.md).

![Liste des environnements Pro](../../assets/pro-environments.png)

### Présentation de l’environnement

La sélection d’un environnement dans la barre de navigation du projet modifie la vue d’ensemble et la barre de navigation pour placer le focus sur l’environnement sélectionné. La barre de navigation comprend des commandes de branche (Branche, Fusion et Synchronisation) et un bouton de configuration :

![Environnement sélectionné](../../assets/environment-selected.png)

La présentation de l’environnement affiche un résumé des détails de l’environnement dans la zone de prévisualisation :

- Nom de l’environnement, type
- Région, ID de projet
- Date et heure de la dernière activité, sauvegarde incluse
- Accès HTTP et statut du moteur de recherche
- Nom de la machine affecté à l’environnement
- Statut de l’environnement (Actif ou Inactif)
- URL du storefront avec bouton **[!UICONTROL Set a custom domain]**

Et dans la présentation de l’environnement principal :

- Le [flux d’activités](activity-stream.md) constitue la présentation de l’environnement principal et affiche les activités en cours, en attente et récentes pour l’environnement sélectionné.
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- L’onglet [ Sauvegardes ](../storage/snapshots.md#create-a-manual-backup) fournit une liste des sauvegardes stockées, un historique des actions de sauvegarde et le bouton Sauvegarder .

### Accéder au storefront

Chaque environnement actif possède un storefront. Sélectionnez un environnement dans le volet de navigation supérieur, puis cliquez sur l’URL dans la présentation de l’environnement. En outre, il existe une liste **[!UICONTROL URLs]** sur le côté droit, au-dessus de la liste Activité .

L&#39;URL d&#39;accès Web peut contenir les éléments suivants :

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **ID unique** = 7 caractères alphanumériques aléatoires
- **ID de projet** = ID de projet de 13 caractères
- **Region** = nom de la région AWS ou Azure. Voir [Adresses IP régionales](regional-ip-addresses.md)

Les environnements de production et d&#39;évaluation Pro comprennent trois nœuds auxquels vous pouvez accéder à l&#39;aide des liens suivants :

- URL de la répartition de charge :

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- Accès direct à l&#39;un des trois serveurs redondants :

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  L’URL de production est utilisée par le réseau de diffusion de contenu (CDN).

## Paramètres

Ouvrez le panneau _Paramètres_ en cliquant sur l’icône ![Configurer le projet](../../assets/icon-configure.png){width="36"} (configurer) sur le côté droit de la navigation du projet.

### Paramètres du projet

**[!UICONTROL Project Settings]** développe un menu de commandes au niveau du projet pour gérer les utilisateurs, les variables, etc. :

| Option | Description |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| Général | Gérez le fuseau horaire à utiliser pour la planification des sauvegardes ou de la maintenance. |
| Accès | Gérez l’[accès utilisateur](user-access.md) pour les types de projet et d’environnement. |
| Certificats | Affichez une liste des certificats SSL associés au projet. |
| Déployer la clé | Ajoutez et affichez la clé publique au référentiel de code du projet. |
| Domaines | Ajoutez un nom de domaine au projet. Voir [ Gestion des domaines ](../cdn/fastly-custom-cache-configuration.md#manage-domains). |
| Intégrations | Ajoutez et gérez les [intégrations](../integrations/overview.md), telles que les notifications d’intégrité et les webhooks. |
| Variables | Ajoutez des [variables au niveau du projet](../environment/variable-levels.md) qui sont disponibles lors de la génération et de l’exécution dans tous les environnements. |

{style="table-layout:auto"}

### Paramètres d’environnement

Cliquez sur **[!UICONTROL Environments]** et sélectionnez un environnement spécifique dans la liste pour les commandes de gestion des paramètres du site, des variables d’environnement, etc. :

| Option | Description |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Général | Configurez le nom d’affichage, le type d’environnement et l’environnement parent.<br>Activer/désactiver différents paramètres d’environnement : |
|           | **Activer les e-mails sortants** : envoyez des [e-mails sortants](outgoing-emails.md) à partir de l’environnement à l’aide du protocole SMTP. |
|           | **Masquer dans les moteurs de recherche** : bloquez les indexeurs et les robots d’exploration des moteurs de recherche du site. |
|           | **Contrôle d’accès HTTP** : activez la configuration de la sécurité du [!DNL Cloud Console] à l’aide d’un contrôle d’accès par connexion et adresse IP. |
|           | Le statut est `active` ou `inactive`. La majeure partie de votre travail se déroule dans un environnement actif. Vous pouvez désactiver ou supprimer l’environnement. |
| Variables | Afficher, créer et gérer [variables au niveau de l’environnement](../environment/variable-levels.md) disponibles au moment de l’exécution. |
| Domaines | Affichez une liste des [itinéraires configurés](../routes/routes-yaml.md). |

{style="table-layout:auto"}

>[!WARNING]
>
>**NE PAS** utilisez la méthode de contrôle d’accès HTTP pour sécuriser les environnements d’évaluation et de production Pro. Cela interrompt la mise en cache rapide. Utilisez plutôt la fonctionnalité [Blocage](../cdn/fastly-vcl-blocking.md) disponible dans le réseau CDN Fastly pour qu’Adobe Commerce bloque l’accès ou mette en œuvre le contrôle d’accès à l’aide de l’[Authentification de base Fastly](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md).

## Informations d’identification Fastly et New Relic

Votre projet comprend [Fastly](../cdn/fastly.md) et [New Relic](../monitor/new-relic-service.md). Les détails du projet affichent des informations pour votre plan de projet et les licences et jetons importants pour ces intégrations. Seul le propriétaire de la licence dispose d’un accès initial aux informations d’identification et aux services. Fournissez ces informations d’identification aux ressources techniques et de développement, si nécessaire.

- [Fastly](https://www.fastly.com/) fournit des services de diffusion de contenu (CDN), d’optimisation des images et de sécurité (DDoS et WAF) pour vos projets d’infrastructure cloud Adobe Commerce. Voir [Obtention des informations d’identification Fastly](../cdn/fastly-configuration.md#get-fastly-credentials).

- [New Relic](../monitor/new-relic-service.md) fournit des mesures d’application et des informations de performances pour les environnements d’évaluation et de production.

Utilisez l’[interface de ligne de commande Cloud](../dev-tools/cloud-cli-overview.md) pour passer en revue vos jetons d’intégration, vos identifiants, etc. :

```bash
magento-cloud subscription:info services
```
