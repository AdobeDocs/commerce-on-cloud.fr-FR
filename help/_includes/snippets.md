---
source-git-commit: 67ed09e3b7c5f5218407b6648e8ca2c32933bbda
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---
# Fragments de code Cloud

## Avertissement Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7 et versions ultérieures ne sont pas prises en charge pour Adobe Commerce sur les infrastructures cloud. Adobe Commerce 2.4.4 et versions ultérieures prennent en charge le service OpenSearch.

## Intégration améliorée {#enhanced-integration-envs}

>[!NOTE]
>
>Les projets configurés avant le 5 juin 2020 disposaient de plusieurs environnements d’intégration plus petits. Si vous avez besoin d’un environnement d’intégration plus grand pour les tests et le développement, demandez une mise à niveau vers les environnements d’intégration améliorés. Pour plus d’informations, consultez l’article [Demande d’environnement d’intégration](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27242) dans le Centre d’aide d’_Adobe Commerce_.

## Options de fusion {#merge-options}

Par défaut, le processus de déploiement remplace tous les paramètres du fichier `env.php`. Vous pouvez toutefois choisir de fusionner une ou plusieurs valeurs pour une configuration de service sans remplacer toutes les valeurs.

Définissez l’option `_merge` sur l’une des options suivantes :

- `true`—**Fusionner** les valeurs de service configurées avec les valeurs de variable d&#39;environnement.
- `false`—**Remplacer** les valeurs de service configurées par les valeurs de variable d&#39;environnement.

## Référentiel privé {#private-repository}

>[!NOTE]
>
>Adobe recommande d’utiliser un référentiel privé pour votre projet d’infrastructure Adobe Commerce on cloud afin de protéger toute information propriétaire ou travail de développement, comme les extensions et les configurations sensibles.

## Avertissement pro en libre-service {#pro-self-service-warning}

>[!WARNING]
>
>Certains projets **Pro** nécessitent l’assistance de l’assistance Adobe pour mettre à jour les configurations d’itinéraire dans le fichier `routes.yaml` et les configurations cron dans le fichier `.magento.app.yaml`. Adobe recommande d’effectuer et de valider toutes les modifications de configuration YAML dans un environnement d’intégration avant de les déployer dans l’environnement d’évaluation.
>
>
>Si vos modifications ne sont pas répercutées sur les sites d’évaluation après le redéploiement et qu’il n’existe aucun message d’erreur associé dans le journal, vous **devez** [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Dans le ticket, décrivez clairement les modifications de configuration que vous avez tentées et joignez tout fichier de configuration YAML mis à jour dans le ticket.

## Sauvegardes Pro {#pro-backups}

>[!TIP]
>
>Pour récupérer une sauvegarde spécifique sur les environnements d’évaluation et de production Pro, [envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) en indiquant la date, l’heure et le fuseau horaire dans le ticket.
>
>Adobe ne restaure **pas** les environnements à partir d’une sauvegarde automatique. Consultez [Restaurer un instantané de base de données à partir de l&#39;évaluation ou de la production](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production) pour choisir une méthode de restauration d&#39;un instantané d&#39;évaluation ou de production.

## Avertissement de redéploiement {#redeploy-warning}

>[!WARNING]
>
>Le processus de déploiement commence lorsque vous effectuez une fusion, une notification push ou une synchronisation de votre environnement, ou lorsque vous déclenchez un redéploiement manuel, au cours duquel l’application [!DNL Commerce] est en mode de maintenance. Pour un environnement de production, Adobe recommande d’effectuer ce travail en dehors des heures de pointe afin d’éviter toute interruption de service.

## Espace réservé d’itinéraire {#route-placeholder}

>[!NOTE]
>
>Les exemples de configuration d’itinéraire suivants utilisent des modèles d’itinéraire avec des espaces réservés. L’espace réservé `{default}` représente le domaine par défaut configuré pour votre site. Si votre projet comporte plusieurs domaines, utilisez l’espace réservé `{all}` pour configurer le routage du domaine par défaut et de tous les alias. Voir [Configurer les itinéraires](/help/cloud-guide/routes/routes-yaml.md).

## Synchronisation SCD {#scd-timing-warning}

>[!WARNING]
>
>Si vous rencontrez des problèmes avec les fichiers de contenu statique dans votre application après le déploiement, tels que des fichiers de thème personnalisé manquants, augmentez le temps d’exécution maximal attendu à 900 secondes ou plus.

## Déploiement basé sur un scénario {#scenarios}

>[!NOTE]
>
>Avec la version 2002.1.0 d’[!DNL ECE-Tools] et les versions ultérieures, vous pouvez utiliser la fonctionnalité de déploiement basée sur des scénarios pour personnaliser les processus de création, de déploiement et de post-déploiement pour votre projet d’infrastructure cloud d’Adobe Commerce. Voir [Déploiement basé sur un scénario](/help/cloud-guide/deploy/scenario-based.md).

## Deuxième évaluation {#second-staging}

>[!NOTE]
>
>Certains projets exigent un processus de développement plus sophistiqué. Pour répondre à ce besoin, Adobe propose un [environnement d’évaluation supplémentaire](/help/cloud-guide/test/second-staging.md) en tant qu’option complémentaire de votre infrastructure cloud.

## Instruction de service {#service-instruction}

Utilisez les instructions suivantes pour la configuration du service sur les environnements Pro Integration et les environnements de démarrage, y compris la branche `master`.

>[!NOTE]
>
>Pour modifier la configuration du service dans les environnements de production et d’évaluation Pro, [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket). Pour les exigences de planification et les conseils de disponibilité du client, consultez [Assistance des services Pro](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) dans _Configurer les services_.

## Changement de service {#service-change-tip}

>[!TIP]
>
>Après la configuration initiale du service, vous pouvez modifier la version du logiciel d’un service installé en mettant à jour les fichiers de configuration `services.yaml` et `.magento.app.yaml`. Consultez [Modifier la version du service](/help/cloud-guide/services/services-yaml.md#change-service-version) pour obtenir des conseils sur la mise à niveau ou la rétrogradation d’un service. Cette méthode en libre-service ne s’applique pas aux environnements d’évaluation ou de production Pro. Voir [Prise en charge des services Pro](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support) dans _Configuration des services_.

## Conseil de déploiement bloqué {#stuck-deployment-tip}

>[!TIP]
>
>Pour obtenir de l’aide sur les déploiements bloqués, utilisez l’utilitaire de dépannage de déploiement [&#128279;](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-29640) dans le Centre d’aide de _Commerce_.

## Mise à jour des outils de la CEE {#ece-tools-package}

>[!NOTE]
>
>Pour supprimer les packages obsolètes des versions d’Adobe Commerce sur l’infrastructure cloud qui ne contiennent pas le package `ece-tools`, vous devez effectuer une [mise à niveau ponctuelle](/help/cloud-guide/dev-tools/install-package.md) sur votre projet cloud. Si vous utilisez actuellement le module `ece-tools` et que vous devez le mettre à jour, voir [Mettre à jour le module ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Conseil de mise à niveau {#upgrade-tip}

>[!TIP]
>
>Avant de commencer une mise à niveau ou un processus d’application de correctifs, créez une branche active à partir de l’environnement d’intégration et extrayez la nouvelle branche sur votre station de travail locale. Dédier une branche à la mise à niveau ou au processus de correctif permet d’éviter toute interférence avec votre travail en cours.

## Valkey dans New Relic {#valkey-newrelic}

>[!NOTE]
>
>New Relic peut toujours afficher Redis même après la migration vers Valkey.
>
>Il est prévu que New Relic continue de faire référence au service de cache en tant que Redis même après la migration de l’environnement vers Valkey.
>
>Valkey est une forme open source de Redis, et certains outils et intégrations continuent à identifier le service à l’aide de l’appellation Redis plutôt que d’un libellé Valkey distinct. Ce comportement n’indique pas nécessairement que Redis est toujours installé.

<!-- Fastly-related snippets begin -->

## Login de l’administrateur {#admin-login-step}

1. [Connectez-vous](/help/get-started/onboarding.md#access-your-admin-panel) à l’administrateur.

## Automatiser le déploiement de fragments de code VCL personnalisés {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Au lieu de charger manuellement des fragments de code VCL personnalisés, vous pouvez les ajouter au répertoire `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` dans votre environnement. Les fragments de code présents dans ce répertoire sont chargés automatiquement lorsque vous cliquez sur _charger un fichier VCL vers Fastly_ dans Commerce Admin. Consultez la section [Déploiement automatisé de fragments de code VCL personnalisés](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) dans la documentation du module Fastly CDN pour Magento 2 .

<!-- Fastly-related snippets end -->
