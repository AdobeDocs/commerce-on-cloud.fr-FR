---
source-git-commit: 79ac13115bd3f275651a5477f2939c8f00a5a985
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 0%

---
# Assistance des services professionnels et disponibilité des clients

## Assistance des services professionnels

Pour demander et effectuer une mise à niveau du service Pro dans les environnements d&#39;évaluation ou de production, procédez comme suit :

1. **Pour installer ou mettre à jour les [services](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/configure/service/services-yaml) dans les environnements `Staging` et `Production` uniquement**, envoyez un [ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/fr/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket).

   Dans le ticket, spécifiez les changements de service requis, incluez les fichiers `.magento.app.yaml` et `.magento/services.yaml` mis à jour et notez la version PHP cible.

   La version PHP, les mises à jour du compositeur, les extensions et les paramètres d&#39;environnement sont des changements en libre-service. Adobe peut avoir besoin de mettre à jour l’agent New Relic pour assurer la compatibilité des versions PHP. Voir [Paramètres PHP](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/configure/app/php-settings) dans _Configuration des applications_.

   >[!IMPORTANT]
   >
   >Lors de la sélection du champ **[!UICONTROL Environment]** dans le formulaire de ticket, utilisez la dénomination de l’environnement Adobe. Par exemple, sélectionnez Évaluation même si vous appelez cet environnement **Dev** en interne. Vous pouvez mentionner votre nom interne dans la description, mais le champ [!UICONTROL Environment] doit utiliser la nomenclature Adobe.

1. **Confirmez le planning de mise à niveau** via le processus en deux parties d’Adobe : vous confirmez d’abord la date et l’heure demandées, puis l’assistance les soumet à l’équipe d’infrastructure pour confirmation finale.

   Les changements de production (Pro uniquement) nécessitent un préavis d&#39;au moins deux jours ouvrables, à l&#39;exclusion des week-ends. Par exemple, l’équipe d’infrastructure cloud doit confirmer une mise à niveau du lundi avant le mercredi précédent. Attendez-vous à un délai d’avance supplémentaire pendant les pics de demande. Pour éviter les retards, répondez à la demande initiale au moins 48 heures avant la fenêtre. La mise à niveau n’est pas considérée comme planifiée tant que vous n’avez pas reçu la confirmation finale.

   >[!NOTE]
   >
   >Fournissez des fenêtres de maintenance en UTC. Les mises à niveau intermédiaires ne sont pas planifiées à l’avance et sont généralement terminées le même jour que la demande.
   >
   >Après une mise à niveau de RabbitMQ, redéployez l’environnement pour réinitialiser les files d’attente de messages.

1. **Validez la mise à niveau** dans un environnement d’évaluation ou d’intégration avant de la planifier en production.

   Les problèmes causés par des modules tiers, du code personnalisé ou la compatibilité des dépendances apparaissent souvent lors du redéploiement qui suit une mise à niveau du service. Pour valider plusieurs mises à niveau de service une par une, un ordre raisonnable est Valkey ou Redis, puis RabbitMQ, puis OpenSearch, puis MariaDB. Cette séquence n’est pas obligatoire. Les mises à niveau de bases de données ont l&#39;impact opérationnel le plus important et méritent la plus grande prudence.

   Adobe ne garantit pas à l’avance la durée exacte d’une fenêtre de maintenance de production, car le timing dépend de l’environnement et des services impliqués. Utilisez le temps nécessaire à la mise à niveau intermédiaire comme une estimation pratique lors de la planification de la fenêtre Production.

1. **Redéployez l’environnement** une fois que Adobe a terminé la mise à niveau du service afin que la modification prenne effet, même si la version de l’application Adobe Commerce ne change pas.

   Si la mise à niveau inclut OpenSearch, prévoyez également une réindexation complète. Adobe ne peut pas garantir un temps d’arrêt nul pour une mise à niveau du service. Planifiez donc une fenêtre de maintenance qui laisse le temps de redéployer, de réindexer si nécessaire et de valider le storefront et l’administrateur avant de rouvrir le site.

## Disponibilité du client pendant les mises à niveau

**Un représentant de votre équipe ou de votre partenaire d’implémentation doit être disponible en ligne pendant toute la durée de la période de mise à niveau de production planifiée.** La planification pendant une période de faible trafic ne désactive pas la mise à niveau. Adobe gère la mise à niveau de l’infrastructure cloud, mais ne peut pas valider le comportement de votre application, vos intégrations, votre code personnalisé ou vos workflows métier.

Le représentant disponible doit pouvoir :

- **Surveillez** le storefront et les transactions commerciales critiques pendant et après la mise à niveau.
- **Répondez** aux questions de l’assistance Adobe ou de l’équipe chargée de l’infrastructure cloud.
- **Confirmez** que les intégrations, les extensions, les personnalisations, les tâches cron, les files d’attente et d’autres fonctions spécifiques au client fonctionnent comme prévu.
- **Validez** les workflows critiques pour l’entreprise, tels que l’extraction, les vues de catalogue, la recherche, la connexion et le traitement des commandes.
- **Signaler** un comportement inattendu se produit rapidement, alors que le contexte et les journaux de mise à niveau sont toujours disponibles.

>[!TIP]
>
>Pour les projets Pro, les mises à niveau de service en production nécessitent également une planification préalable et un processus de confirmation en deux parties avec l’assistance d’Adobe. Voir [Assistance des services Pro](#pro-services-support).

### Mode de maintenance

**Le mode de maintenance ne remplace pas la disponibilité des clients.** Le mode de maintenance bloque l’accès au storefront, mais ne valide pas les services d’application, les intégrations, les files d’attente, les tâches cron, le passage en caisse ou d’autres fonctions spécifiques au client.

Si le travail prévu nécessite le mode de maintenance, coordonnez son utilisation avec l’assistance Adobe et suivez les instructions de cette mise à niveau. Ensuite, vérifiez que le storefront et les workflows critiques fonctionnent normalement avant de considérer le travail comme terminé.
