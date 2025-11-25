---
title: Conseils de test
description: Découvrez les types de tests et les bonnes pratiques pour lancer Adobe Commerce sur les infrastructures cloud.
exl-id: 70fdfbbd-1763-4b1b-9ffd-9ffdc92f4f91
source-git-commit: d48b1844305e72b7b4a37568f2358f3aa4cf2e24
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Conseils de test

Après avoir configuré et personnalisé votre projet d’infrastructure Adobe Commerce sur le cloud, il est recommandé de tester minutieusement votre application avant de lancer le site web de la boutique. Les tests permettent de gérer correctement les attentes relatives à la taille du cluster et de s’adapter aux besoins futurs de l’entreprise.

## Tests fonctionnels

Lors du développement, il est important d’effectuer des tests fonctionnels de bout en bout sur votre projet d’infrastructure cloud Adobe Commerce. Consultez les conseils suivants pour effectuer des tests fonctionnels dans l’environnement Docker :

- **Tests d’application** : utilisez la [structure de tests fonctionnels Magento (MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing) pour les tests d’application dans l’environnement Cloud Docker.

- **Test de code** : utilisez le cadre de test de [Codeception pour PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing) pour valider le code destiné à apporter une contribution aux référentiels de packages cloud.

## Bonnes pratiques avant le lancement

Envisagez d’effectuer les types de test suivants avant le lancement du site en tant que bonne pratique :

- **Test de charge** : effectuez un test de charge pour comprendre le comportement du système en cas de charge attendue. Par exemple, testez un nombre simultané d’utilisateurs et d’utilisatrices actifs dans l’application en demandant à chaque utilisateur ou utilisatrice d’effectuer un nombre spécifique de transactions pendant la durée définie. Ce test révèle le temps de réponse des transactions critiques importantes pour l’entreprise, telles que le comportement de la base de données ou du serveur d’applications. Un test de charge peut aider à identifier les goulots d’étranglement.

- **Épreuve de contrainte**—Remettez en question les limites supérieures de la capacité du système pour déterminer si le système fonctionne suffisamment lorsque la charge actuelle dépasse largement le maximum prévu.

- **Analyse de sécurité** : Adobe fournit gratuitement [Outil d’analyse de sécurité](../launch/overview.md#set-up-the-security-scan-tool) pour vos sites.

- **Test de pénétration**—Il s&#39;agit d&#39;une cyberattaque simulée autorisée contre un système informatique conçue pour évaluer la sécurité du système. Le test de pénétration permet d’identifier les faiblesses ou les vulnérabilités, y compris le risque que des parties non autorisées accèdent aux fonctionnalités et aux données du système.

>[!WARNING]
>
>Les clients ne sont pas autorisés à effectuer eux-mêmes des évaluations de la sécurité de l’infrastructure AWS ou des services AWS. Si vous découvrez un problème de sécurité dans l’un des services AWS observés dans votre évaluation de la sécurité, [contactez immédiatement la Sécurité AWS](mailto:aws-security@amazon.com). Voir [Politiques des clients AWS pour les tests de pénétration](https://aws.amazon.com/security/penetration-testing/).
