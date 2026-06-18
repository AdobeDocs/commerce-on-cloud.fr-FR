---
title: Connectez-vous à  [!DNL Cloud Console]
description: En savoir plus sur le  [!DNL Cloud Console]  d’Adobe Commerce sur les infrastructures cloud.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00.000Z
exl-id: 4c68508a-f25a-457b-be6d-36dc8ac428f1
TQID: https://experienceleague.adobe.com/5j8ZQakF2ZDWiO0and7Pa4iHp-3CCXbEHgYgZBZ6avo
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: cc72dcf1-72e1-48cc-b434-e7c27d62d67cid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 029e44cd82c07d692dbbfacb4573763dbb4b579a
workflow-type: tm+mt
source-wordcount: 393
ht-degree: 0%

---

# Connexion à [!DNL Cloud Console]

Le [!DNL Cloud Console] fournit des méthodes interactives pour créer, gérer et déployer le code Commerce. Le [!DNL Cloud Console] offre une expérience plus moderne et plus conviviale et jette les bases des futures améliorations de l’interface.

[Connectez-vous à [!DNL Cloud Console]](https://console.adobecommerce.com) pour afficher la liste de vos projets.

![Liste de projets](../assets/ui-allprojects-list.png)

## Fonctionnalités

Les fonctionnalités nouvelles ou améliorées sont les suivantes :

- Présentation claire des caractéristiques du projet et de l’environnement
- Flux d’activités avec historique triable
- Gestion et historique des sauvegardes manuelles pour les projets de démarrage
- Vues améliorées du journal
- Listes triables
- Formulaires simples et conseils pour ajouter des intégrations
- Conformité aux directives d’accessibilité du contenu web (WCAG)

![[!DNL Cloud Console]](../assets/CloudConsole.png)

Les fonctionnalités nouvelles ou améliorées sont les suivantes :

| Fonctionnalité | Améliorations |
| -------------- | ----------------------------------- |
| [Flux d’activité](../cloud-guide/project/activity-stream.md) | Interagissez avec une liste triable d’actions en cours, en attente ou historiques. Sélectionnez une activité et affichez les journaux ou annulez une version en cours d’exécution. |
| [Présentation du projet et de l’environnement](../cloud-guide/project/overview.md#project-overview) | Ouvrez votre projet et consultez la liste des détails du projet et de l’environnement Vue d’ensemble . La présentation de l’environnement fournit plus de détails sur le statut de l’environnement, l’accès aux applications et les activités récentes. |
| [Formulaires d’intégration](../cloud-guide/integrations/overview.md) | Utilisez des formulaires simples et des conseils pour ajouter des intégrations, telles que des notifications Bitbucket ou Slack. |
| [Liste de projets](../cloud-guide/project/overview.md#cloud-console) | La vue _Tous les projets_ répertorie tous les projets auxquels vous avez accès. Vous pouvez cliquer sur **[!UICONTROL Show filters]** et filtrer la liste des projets par type, région ou plan. |
| [Options de visibilité variable](../cloud-guide/environment/variable-levels.md) | Limitez la visibilité d’une variable au niveau du projet ou de l’environnement lors de la création ou de l’exécution. |

<!--
The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | 
-->

## Questions de la console

**_Où puis-je trouver la fonction Instantanés_** ?

Pour les projets [!DNL Starter], la fonction Instantanés est désormais appelée _Sauvegardes_. Vous pouvez créer une sauvegarde manuelle de votre environnement [!DNL Starter] à partir de l’[!DNL Cloud Console] ou créer un instantané à partir de l’interface de ligne de commande Cloud. Vous devez disposer d’un rôle d’administrateur pour l’environnement.

Sélectionnez un environnement dans la barre de navigation du projet. L’environnement doit être actif. Sélectionnez l’onglet **[!UICONTROL Backups]** . Actuellement, cette option n’est pas disponible pour les environnements Pro.

**_Où se trouve la liste des itinéraires configurés pour l’environnement_** ?

La liste des itinéraires configurés figure dans l’onglet _Services_ d’un environnement.

Sélectionnez un environnement dans la barre de navigation du projet. Sélectionnez l’onglet **[!UICONTROL Services]** . La présentation **Router** affiche les itinéraires configurés. Actuellement, vous ne pouvez pas ajouter d’itinéraire à partir du nouveau [!DNL Cloud Console].

## Menu Compte

Dans le coin supérieur droit se trouve le menu de votre compte. Cliquez sur la flèche vers le bas du menu et sélectionnez **[!UICONTROL My Profile]**. Dans la vue _Mon profil_, vous pouvez contrôler les détails de votre utilisateur et les paramètres d’affichage, gérer [l’authentification de sécurité](../cloud-guide/project/user-access.md#user-authentication-requirements), [les jetons d’API](../cloud-guide/project/user-access.md#create-an-api-token) et [les clés SSH](../cloud-guide/development/secure-connections.md).
