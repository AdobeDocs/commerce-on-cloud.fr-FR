---
title: Configuration du déploiement de l’application
description: Découvrez comment configurer les propriétés dans le fichier de configuration de l’application qui contrôlent la manière dont l [!DNL Commerce] application crée et déploie l’environnement Cloud.
feature: Cloud, Configuration, Build, Deploy
exl-id: 47dcb13f-8873-495d-956f-08a5e04844d9
TQID: https://experienceleague.adobe.com/LCTu-HeJCO1pp9trlZ7h74CxSQtyBr5SDmYP9DgCtkU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 190
ht-degree: 0%

---

# Configuration du déploiement de l’application

Le fichier `.magento.app.yaml` contrôle la manière dont votre application crée et déploie. Bien qu’Adobe Commerce sur l’infrastructure cloud prenne en charge plusieurs applications par projet, en règle générale, un projet comporte une seule application avec le fichier `.magento.app.yaml` à la racine du référentiel.

Le `.magento.app.yaml` comporte de nombreuses valeurs par défaut. Voir [exemple de fichier `.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Vérifiez toujours le `.magento.app.yaml` de la version que vous avez installée. Ce fichier peut différer d’une version à l’autre d’Adobe Commerce sur les infrastructures cloud.

Utilisez le fichier `.magento.app.yaml` pour définir les valeurs de configuration suivantes :

- [Propriétés](properties.md) : définissez les valeurs de propriété pour l&#39;instance d&#39;application.
- [Propriété des variables](variables-property.md)—Examinez les variables d&#39;environnement requises pour la version de l&#39;application [!DNL Commerce].
- [Paramètres PHP](php-settings.md)—Configurez les options PHP d&#39;exécution.
- [Définir le cache pour les fichiers statiques](set-cache.md) : définissez la durée de vie du cache pour vos médias et fichiers statiques.

>[!NOTE]
>
>Le fichier `.magento.app.yaml` est géré localement ou dans le référentiel Git. La configuration n’est lue qu’à des fins de déploiement et de création et est supprimée une fois le déploiement terminé. Vous ne la trouverez donc pas sur le serveur.
