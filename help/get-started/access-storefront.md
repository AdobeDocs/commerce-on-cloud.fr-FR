---
title: Accès à votre panneau d’administration Commerce
description: Découvrez comment accéder à votre panneau d’administration Commerce.
recommendations: noDisplay, catalog
exl-id: 827417b0-9048-44d8-8c82-07befba476c7
TQID: https://experienceleague.adobe.com/V3BXuCc9aqT5YuyIS8WAZgUdPAYNhQunAgg2i2FCaOs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 361
ht-degree: 0%

---

# Accès à votre panneau d’administration Commerce

Les utilisateurs disposant d’un accès administratif au panneau d’administration de Commerce peuvent ajouter des utilisateurs, configurer les services de la boutique, terminer la configuration et le travail de personnalisation de la boutique, etc.

Pour un nouveau projet, la première étape après avoir reçu l’e-mail de bienvenue consiste à sécuriser l’accès administrateur au projet en modifiant le mot de passe sur le compte du propriétaire de la licence. Le nom d’utilisateur par défaut de ce compte est l’adresse e-mail du propriétaire de la licence.

Vous pouvez soumettre une demande de changement de mot de passe à l’aide de l’une des méthodes suivantes :

- Recherchez l’e-mail de bienvenue envoyé à l’adresse e-mail du propriétaire de la licence, suivez le lien et modifiez votre mot de passe.

- Copiez l’URL de la boutique depuis le [[!DNL Cloud Console]](../cloud-guide/project/overview.md) dans un navigateur. Ajoutez ensuite `/admin` à la fin de l’URL pour ouvrir la page de connexion. Cliquez sur le **Mot de passe oublié ?** lien permettant d’envoyer une demande de changement de mot de passe à l’adresse e-mail du propriétaire de la licence.

Après avoir envoyé la demande de changement de mot de passe, recherchez dans votre e-mail la notification de réinitialisation de mot de passe. Si vous ne recevez pas l’e-mail, vérifiez votre dossier spam.

>[!TIP]
>
>Si la réinitialisation du mot de passe échoue ou si vous ne pouvez pas vous connecter au panneau d’administration, un utilisateur disposant d’un accès administrateur peut se connecter au projet à l’aide de SSH et ajouter un utilisateur administrateur à l’aide de la commande d’interface de ligne de commande `admin:user:create`. Voir [Créer, modifier ou déverrouiller un compte administrateur](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) dans le _Guide d’installation_.

## Surveiller l’intégrité du site

L’[outil d’analyse à l’échelle du site](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro) est un outil en libre-service proactif et un référentiel central qui comprend des informations détaillées sur le système et des recommandations pour assurer la sécurité et l’opérabilité de votre installation Adobe Commerce. Il fournit une surveillance des performances en temps réel 24h/24 et 7j/7, des rapports et des conseils pour identifier les problèmes potentiels et une meilleure visibilité sur la santé, la sécurité et les configurations des applications du site. Cela permet de réduire le temps de résolution et d’améliorer la stabilité et les performances du site. Vous pouvez accéder à l’outil d’analyse à l’échelle du site directement à partir du [panneau d’administration](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
