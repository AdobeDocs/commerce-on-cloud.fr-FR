---
title: Deuxième environnement d’évaluation
description: Découvrez les avantages et les utilisations d’un deuxième environnement d’évaluation pour les tests parallèles, l’isolation des problèmes, le contrôle de version, etc.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Deuxième environnement d’évaluation

Dans l’infrastructure de Adobe Commerce Cloud, l’environnement d’évaluation assure des tests et une validation complets dans un paramètre qui peut refléter l’environnement de production. Cet environnement vous permet d’identifier et de résoudre les bogues en toute sécurité, tout en veillant à ce que toutes les nouvelles fonctionnalités ou modifications soient intégrées de manière transparente avant qu’elles n’affectent votre site en ligne.

Certains projets exigent un processus de développement plus sophistiqué. Pour répondre à ce besoin, Adobe propose un environnement d’évaluation supplémentaire en tant qu’option complémentaire de votre infrastructure cloud. Disposer de deux environnements d’évaluation est avantageux pour les projets complexes et la collaboration simultanée entre les équipes.

Disposer d’un environnement d’évaluation secondaire offre les avantages suivants :

- **Tests parallèles** : avec deux environnements d’évaluation, les équipes peuvent tester simultanément de nouvelles fonctionnalités et des mises à jour critiques sans interférer avec le développement de l’autre. Par exemple, vous pouvez dédier un environnement aux tests de fonctionnalités, tout en réservant l’autre environnement aux tests de performance et de contrainte.

- **Isolement des problèmes** : lorsque des bugs ou des problèmes surviennent dans l’environnement d’évaluation principal, vous pouvez les répliquer et les diagnostiquer dans l’environnement secondaire sans interrompre la progression du test. Cela garantit que le dépannage n’interfère pas avec les tests en cours.

- **Gestion de version** : gérez plus efficacement les différentes versions de votre application. L’environnement d’évaluation principal peut exécuter la dernière build stable, tandis que les tests secondaires testent des fonctionnalités expérimentales. Cela vous permet de mieux gérer les cycles de publication et de vous assurer que les modifications expérimentales n’interrompent pas les builds stables.

- **Planification améliorée du déploiement** : vous pouvez simuler différents scénarios de déploiement. L’environnement d’évaluation principal peut imiter la configuration de production actuelle, tandis que l’environnement secondaire peut être configuré pour tester les prochaines versions.

- **Test d’acceptation utilisateur (UAT)** : tout en utilisant l’environnement principal pour le développement en cours, vous pouvez dédier l’environnement secondaire à l’UAT. Cela permet aux utilisateurs et utilisatrices, ou aux clients et clientes, de tester et de commenter les nouvelles fonctionnalités dans un environnement contrôlé, sans affecter les tests.

- **Tests de régression** : vous pouvez utiliser un environnement pour les tests orientés vers l’avant et l’autre pour les tests de régression, en vous assurant que les nouvelles modifications n’interrompent pas les fonctionnalités existantes.

- **Formation et démonstrations** : utilisez un environnement pour former de nouveaux membres de l’équipe ou présenter de nouvelles fonctionnalités aux parties prenantes. Cela permet de s’assurer que les activités de formation n’interrompent pas les tests.

- **Configuration de la liaison privée** : les environnements de Principal et d’intégration ne prennent pas en charge la configuration de la liaison privée. Si votre workflow de développement nécessite des environnements supplémentaires connectés via un lien privé pour le développement, le test ou toute autre finalité, pensez à ajouter un environnement d’évaluation supplémentaire.

Disposer de deux environnements d’évaluation peut améliorer considérablement l’efficacité de vos workflows, la gestion des risques et la qualité globale de votre application. Ces environnements offrent la sécurité et la flexibilité qu’un environnement d’évaluation ne pourrait pas offrir.

>[!NOTE]
>
>Cette configuration est un module complémentaire. Les clients qui souhaitent un environnement secondaire doivent contacter leur représentant commercial pour en demander un. Le représentant commercial peut fournir des informations sur les prix et le processus de provisionnement.

Si votre projet comporte déjà un environnement d’évaluation supplémentaire ou si vous envisagez de mettre à niveau votre projet actuel, tenez compte des responsabilités et du calendrier d’approvisionnement suivants :

- Le processus de provisionnement peut prendre jusqu’à deux semaines après la confirmation de la demande auprès de votre représentant commercial d’Adobe.

- Les nouveaux environnements d’évaluation ne sont disponibles que dans la même région que vos environnements d’évaluation et de production actuels.

- Vous devez mettre à jour votre environnement d’intégration ou de Principal et spécifier les environnements sur lesquels reposera votre environnement d’évaluation secondaire. L’environnement d’évaluation secondaire ne peut pas être copié à partir des environnements d’évaluation ou de production.

- Après avoir reçu votre nouvel environnement d’évaluation, vous pouvez l’inclure dans votre flux de développement. Comme pour les autres environnements, les mises à jour de code et de base de données relèvent de votre responsabilité.

## Demander l’environnement

Pour faciliter votre requête, procédez comme suit :

1. Mettez à niveau votre branche.

   Mettez à niveau votre branche d’intégration ou de Principal avec le code et la base de données que vous souhaitez utiliser pour le nouvel environnement.

1. Contactez votre représentant commercial.

   Contactez votre représentant commercial d’Adobe et indiquez-lui la branche spécifique que vous souhaitez utiliser comme base pour votre nouvel environnement d’évaluation (intégration ou Principal).

1. Confirmer les détails.

   Après avoir confirmé les détails avec votre représentant commercial, attendez qu&#39;il confirme que la demande de provisioning a été reçue et qu&#39;elle est en cours de traitement. Une fois le processus d’approvisionnement terminé, l’équipe d’Adobe vous enverra une confirmation.

1. Effectuez un déploiement dans votre nouvel environnement.

   Suivez les [étapes de déploiement standard](../deploy/staging-production.md) pour déployer votre code et votre base de données dans le nouvel environnement d’évaluation.
