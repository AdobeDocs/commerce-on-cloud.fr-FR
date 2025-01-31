---
title: Configuration de Commerce sur le cloud
description: Découvrez comment préparer un conseiller technique client Adobe à mettre en service votre projet d’infrastructure cloud Adobe Commerce.
recommendations: noDisplay, catalog
role: Admin
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# Conditions préalables à l’approvisionnement de Commerce on Cloud

Commençons et initialisons votre projet Commerce sur l’infrastructure cloud.

Avant qu’Adobe ne configure votre Commerce sur des environnements de projet cloud, il est recommandé de tenir compte des stratégies suivantes et de préparer les réponses pour votre première consultation avec l’équipe en charge de votre compte Adobe. Utilisez les sections suivantes comme liste de contrôle pour vous aider à préparer votre conversation avec un conseiller technique client en vue de la configuration d’un projet cloud :

## Définition du domaine

**Question 1** : _Quel(s) domaine(s) avez-vous l’intention d’utiliser pour le lancement du site ?_

Préparez-vous à intégrer les services Fastly et Nginx en définissant vos domaines et sous-domaines de premier niveau pour les environnements d&#39;évaluation et de production Pro. Après la configuration initiale, vous ne pouvez ajouter des domaines qu’en envoyant un ticket d’assistance Adobe Commerce. Il est donc recommandé de disposer des informations de votre domaine.

Exemples pour les domaines de production et d’évaluation :

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

Voir [Configuration de plusieurs sites web ou magasins](../cloud-guide/store/multiple-sites.md) dans le guide _Commerce sur les infrastructures cloud_ pour obtenir des conseils supplémentaires sur les domaines multiples ou uniques.

Voir [Comptes Fastly multiples et domaines attribués](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"} si vous disposez d’un compte Fastly existant qui lie les mêmes apex et sous-domaines utilisés sur votre site Adobe Commerce.

## Domaine d’e-mail transactionnel

**Question 2** : _Quel(s) domaine(s) avez-vous l&#39;intention d&#39;utiliser pour les e-mails transactionnels ?_

Adobe Commerce on Cloud utilise le service proxy SMTP (Simple Mail Transfer Protocol) SendGrid, qui fournit des services d’authentification des e-mails sortants et de surveillance de la réputation. SendGrid envoie des e-mails transactionnels en votre nom et nécessite donc des informations de domaine.

Exemple pour le domaine SendGrid : `example@your-store.com`

Voir [Service de messagerie SendGrid](../cloud-guide/project/sendgrid.md) dans le guide _Commerce sur les infrastructures cloud_ pour plus d’informations sur les e-mails transactionnels et les paramètres de domaine.

## Allocation de stockage

**Question 3** : _Quelle quantité de stockage sous contrat prévoyez-vous d’allouer au chargement de fichiers et à la base de données ?_

Adobe Commerce sur les infrastructures cloud utilise le stockage pour la structure des fichiers, l’indexation des recherches et la base de données. Vous pouvez subdiviser le stockage selon vos besoins en commençant par 10 Go pour chaque partition.

La quantité de stockage que vous avez sous-traitée pour votre projet cloud représente le stockage total pour chaque environnement. Par exemple, si vous avez acheté 50 Go d’espace de stockage, vous disposez de 50 Go de stockage pour chaque environnement. Il existe 50 Go de stockage distinct pour la production, l’évaluation et chaque environnement d’intégration, respectivement.

Vous pouvez augmenter votre capacité de stockage à tout moment. Pour les environnements de production et d’évaluation Pro, vous devez envoyer un ticket de support Adobe Commerce pour modifier l’allocation de l’espace disque. Une augmentation de la taille des environnements de production et d&#39;évaluation Pro ne peut se produire qu&#39;à certains intervalles. Selon l’utilisation actuelle de votre espace disque, l’équipe d’assistance peut vous recommander d’augmenter l’allocation d’espace disque d’au moins 10 Go. Une fois alloué, l’augmentation du stockage pour l’évaluation et la production pro ne peut **pas** être rétablie.

Voir [Gérer l’espace disque](../cloud-guide/storage/manage-disk-space.md) dans le guide _Commerce sur les infrastructures cloud_.

## Région du service cloud

**Question 4** : _Quelle région de service cloud est la plus adaptée à votre proximité ?_

Choisissez Amazon Web Services (AWS) ou Microsoft Azure comme base d’infrastructure en tant que service (IaaS) pour vos projets professionnels d’infrastructure cloud Adobe Commerce. Chaque fournisseur opère dans plusieurs régions et fournit plusieurs zones de disponibilité. Sélectionnez une région adaptée à votre emplacement et réduisez le risque de latence et d’augmentation des coûts.

Voir [carte des régions cloud d’Adobe Commerce](../cloud-guide/overview.md).

## Service de connexion

**Question 5** : _Avez-vous besoin d’un service PrivateLink ? Si oui, dans quelle région la connexion PrivateLink se trouve-t-elle ?_

Adobe Commerce sur les infrastructures cloud prend en charge l’intégration au service AWS PrivateLink ou Azure Private Link. Bien que ce service soit facultatif, PrivateLink est utilisé pour établir une communication sécurisée et privée entre les environnements d’infrastructure cloud avec des services et des applications hébergés sur des systèmes externes.

Il est important de prendre en compte votre stratégie de service cloud (AWS ou Azure) afin que l’instance Adobe Commerce soit accessible dans la même région. Consultez la section [Service PrivateLink](../cloud-guide/development/privatelink-service.md) dans le guide _Commerce sur les infrastructures cloud_ pour plus d’informations sur l’accessibilité régionale.

## Lancement du site cible

**Question 6** : _Quelle est la date de lancement prévue ?_

Le lancement d’un site nécessite une configuration et des tests itératifs pour garantir la réussite du lancement. Définir une date cible vous aide, vous et l’équipe de votre compte d’Adobe, à vous préparer aux activités finales de prélancement, qui comprennent un appel pour coordonner les étapes finales.

Consultez la [présentation du site Launch](../cloud-guide/launch/overview.md) dans le guide _Commerce sur les infrastructures cloud_ pour consulter l’ensemble du processus et télécharger une copie de la liste de contrôle Launch.

>[!TIP]
>
> Jetez un coup d’œil au portail Cloud et accédez à votre nouveau projet cloud.
>
>**Étape suivante** : [Intégration à Commerce](onboarding.md)
