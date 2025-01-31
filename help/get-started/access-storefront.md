---
title: Accès à votre panneau d’administration Commerce
description: Découvrez comment accéder à votre panneau d’administration Commerce.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Accès à votre panneau d’administration Commerce

Les utilisateurs disposant d’un accès administratif au panneau d’administration de Commerce peuvent ajouter des utilisateurs, configurer les services de la boutique, terminer la configuration et le travail de personnalisation de la boutique, etc.

Pour un nouveau projet, la première étape après avoir reçu l’e-mail de bienvenue consiste à sécuriser l’accès administrateur au projet en modifiant le mot de passe sur le compte du propriétaire de la licence. Le nom d’utilisateur par défaut de ce compte est l’adresse e-mail du propriétaire de la licence.

Vous pouvez soumettre une demande de changement de mot de passe à l’aide de l’une des méthodes suivantes :

- Recherchez l’e-mail de bienvenue envoyé à l’adresse e-mail du propriétaire de la licence, suivez le lien et modifiez votre mot de passe.

- Copiez l’URL de la boutique depuis le [[!DNL Cloud Console]](../cloud-guide/project/overview.md) dans un navigateur. Ajoutez ensuite `/admin` à la fin de l’URL pour ouvrir la page de connexion. Cliquez sur le mot de passe **oublié ?** lien pour envoyer une demande de changement de mot de passe à l’adresse e-mail du propriétaire de la licence.

Après avoir envoyé la demande de changement de mot de passe, recherchez dans votre e-mail la notification de réinitialisation de mot de passe. Si vous ne recevez pas l’e-mail, vérifiez votre dossier spam.

>[!TIP]
>
>Si la réinitialisation du mot de passe échoue ou si vous ne pouvez pas vous connecter au panneau d’administration, un utilisateur disposant d’un accès administrateur peut se connecter au projet à l’aide de SSH et ajouter un utilisateur administrateur à l’aide de la commande d’interface de ligne de commande `admin:user:create`. Voir [Créer, modifier ou déverrouiller un compte administrateur](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) dans le _Guide d’installation_.

## Surveiller l’intégrité du site

L’[outil d’analyse à l’échelle du site](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro) est un outil en libre-service proactif et un référentiel central qui comprend des informations détaillées sur le système et des recommandations pour assurer la sécurité et l’opérabilité de votre installation Adobe Commerce. Il fournit une surveillance des performances en temps réel 24h/24 et 7j/7, des rapports et des conseils pour identifier les problèmes potentiels et une meilleure visibilité sur la santé, la sécurité et les configurations des applications du site. Cela permet de réduire le temps de résolution et d’améliorer la stabilité et les performances du site. Vous pouvez accéder à l’outil d’analyse à l’échelle du site directement à partir du [panneau d’administration](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
