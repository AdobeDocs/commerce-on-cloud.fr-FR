---
title: Préparation au développement
description: Rassemblez les informations d’identification et découvrez les outils disponibles pour configurer un espace de travail de développement à utiliser avec votre projet d’infrastructure cloud Commerce.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Préparation au développement

Que vous soyez débutant dans Commerce ou propriétaire d’un Commerce existant et que vous migriez vers l’infrastructure cloud, suivez ces étapes pour préparer un espace de travail de développement pour votre projet cloud. Si vous avez déjà effectué certaines de ces étapes ou si vous disposez déjà d’un environnement de développement Adobe Commerce, passez en revue les points suivants pour connaître les résultats attendus et passez à l’étape suivante. Certaines configurations et certains workflows diffèrent d’une installation locale standard.

## Informations d’identification

Avant de configurer un espace de travail, collectez les clés et l’accès au compte suivants :

- **Clés d’authentification (clés du compositeur)**

  Les clés d’authentification sont des jetons d’authentification de 32 caractères qui fournissent un accès sécurisé au référentiel du compositeur d’Adobe Commerce (`repo.magento.com`) et à tout autre service Git requis pour le développement d’applications, tel que GitHub. Votre compte peut avoir plusieurs clés d’authentification. Pour la configuration de l’espace de travail, commencez par une clé spécifique pour votre référentiel de code. Si vous ne disposez d’aucune clé, contactez le propriétaire du projet ou créez vous-même les [ clés d’authentification ](../cloud-guide/development/authentication-keys.md).

- **Compte de projet cloud**

  Le propriétaire du projet doit vous inviter à rejoindre le projet d’infrastructure cloud d’Adobe Commerce. Lorsque vous recevez l’invitation par courrier électronique, cliquez sur le lien et suivez les invites pour créer votre compte. Voir [ Intégration ](onboarding.md).

- **Clé de chiffrement Adobe Commerce**

  Lors de l’importation d’un système existant uniquement, capturez la clé de chiffrement utilisée pour protéger l’accès et les données de la base de données. Pour plus d’informations sur cette clé, voir [Résoudre les problèmes liés à la clé de chiffrement](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html?lang=fr)

## Outils de développement

- **Installation de l’interface de ligne de commande Cloud**

  Installez l’interface de ligne de commande `magento-cloud` afin de pouvoir gérer les environnements cloud et exécuter des tâches d’automatisation. Voir [Cloud CLI](../cloud-guide/dev-tools/cloud-cli-overview.md) pour obtenir des instructions d’installation.

- **Installez Docker pour le développement et les tests locaux**

  Vous pouvez éventuellement utiliser l’environnement Docker pour émuler l’environnement de `integration` Commerce sur l’infrastructure cloud pour le développement local. Il existe trois composants essentiels : un modèle Adobe Commerce v2, Docker Compose et le package `ece-tools`.

   - [Architecture Docker et commandes courantes](../cloud-guide/dev-tools/cloud-docker.md)
   - [Environnement de développement Docker Launch](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [Ensemble d&#39;outils de la CEE](../cloud-guide/dev-tools/package-overview.md)

- **Intégration de services basés sur Git**

  Vous pouvez éventuellement intégrer un service d’hébergement basé sur Git, tel que GitHub ou GitLab, à Adobe Commerce sur l’infrastructure cloud. Pour plus d&#39;informations, consultez la section [ Intégrations ](../cloud-guide/integrations/overview.md).

## Code du projet

Une connexion sécurisée est essentielle pour interagir avec les environnements distants. Pour un nouveau projet, [connectez-vous à [!DNL Cloud Console]](https://console.adobecommerce.com) puis cliquez sur **[!UICONTROL No SSH key]**. Cette icône se trouve à droite du champ de commande et est visible lorsque le projet ne contient pas de clé SSH. Voir [Connexions sécurisées](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account).

**Pour cloner votre base de code sur votre station de travail locale** :

1. Dans la [[!DNL Cloud Console]](https://console.adobecommerce.com), cliquez sur **[!UICONTROL code]** et sélectionnez l’onglet **[!UICONTROL Git]** .

   ![Clonez votre code](../assets/ui-git-code.png){width="450"}

1. Copiez la commande `git clone ...` fournie.

1. Dans un terminal, créez et modifiez votre répertoire de travail.

1. Collez et exécutez la commande `git clone ...`.

>[!TIP]
>
>Adobe approvisionne votre environnement de projet initial à l’aide d’un référentiel de modèles qui inclut des instructions de package pour une version spécifique d’Adobe Commerce. Consultez la rubrique [structure de fichiers du projet](../cloud-guide/project/file-structure.md) et en savoir plus sur les fichiers de projet importants et les modèles cloud.
