---
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---
# Fragments de code Cloud

## avertissement Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch version 7.11 et ultérieure n’est pas pris en charge pour Adobe Commerce sur les infrastructures cloud. Les versions 2.3.7-p3, 2.4.3-p2 et 2.4.4 d’Adobe Commerce et ultérieures prennent en charge le service OpenSearch. Les installations sur site continuent à prendre en charge l’Elasticsearch.

## Intégration améliorée {#enhanced-integration-envs}

>[!NOTE]
>
>Les projets configurés avant le 5 juin 2020 disposaient de plusieurs environnements d’intégration plus petits. Si vous avez besoin d’un environnement d’intégration plus grand pour les tests et le développement, demandez une mise à niveau vers les environnements d’intégration améliorés. Pour plus d’informations, consultez l’article [Demande d’environnement d’intégration](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) dans le Centre d’aide d’_Adobe Commerce_.

## Options de fusion {#merge-options}

Par défaut, le processus de déploiement remplace tous les paramètres du fichier `env.php`. Vous pouvez toutefois choisir de fusionner une ou plusieurs valeurs pour une configuration de service sans remplacer toutes les valeurs.

Définissez l’option `_merge` sur l’une des options suivantes :

- `true`—**Fusionner** les valeurs de service configurées avec les valeurs de variable d&#39;environnement.
- `false`—**Remplacer** les valeurs de service configurées par les valeurs de variable d&#39;environnement.

## Référentiel privé {#private-repository}

>[!NOTE]
>
>Adobe recommande vivement d’utiliser un référentiel privé pour votre projet d’infrastructure Adobe Commerce on cloud afin de protéger toute information propriétaire ou tout travail de développement, tel que les extensions et les configurations sensibles.

## Avertissement pro en libre-service {#pro-self-service-warning}

>[!WARNING]
>
>Certains projets **Pro** nécessitent un ticket d’assistance pour mettre à jour la configuration d’itinéraire dans le fichier `routes.yaml` et la configuration cron dans le fichier `.magento.app.yaml`. Adobe recommande de mettre à jour et de tester les fichiers de configuration YAML dans un environnement d’intégration, puis de déployer les modifications dans l’environnement d’évaluation. Si vos modifications ne sont pas appliquées aux sites d’évaluation après le redéploiement et qu’il n’existe aucun message d’erreur associé dans le journal, vous **DEVEZ** [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) qui décrit les modifications de configuration tentées. Incluez tous les fichiers de configuration YAML mis à jour dans le ticket.

## Assistance des services professionnels {#pro-update-service}

>[!TIP]
>
>Pour les projets Pro, vous devez [envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour installer ou mettre à jour les [services](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html) dans les environnements `Staging` et `Production` uniquement.
>
>Indiquez les changements de service nécessaires, incluez vos fichiers `.magento.app.yaml` et `services.yaml` mis à jour, et indiquez la version PHP dans le ticket. Pour les modifications en libre-service de la version PHP, des extensions ou des paramètres d&#39;environnement, voir [paramètres PHP](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html) dans _Configuration de l&#39;application_.
>
>Pour apporter des modifications à un environnement de production en ligne (**Pro uniquement**), un préavis d’au moins 48 heures est requis. L’équipe en charge de l’infrastructure cloud dispose ainsi de suffisamment de temps pour rassembler les ressources et effectuer une mise à niveau sécurisée. La période de préavis commence lorsque l’équipe d’infrastructure accuse réception de la demande et planifie la mise à niveau, à l’exclusion des week-ends. Par exemple, pour que les mises à niveau de service soient terminées un lundi, un accusé de réception de la mise à niveau prévue doit être reçu avant le mercredi. Pendant les périodes de pointe, le traitement de votre demande peut prendre plus de temps.

## Sauvegardes Pro {#pro-backups}

>[!TIP]
>
>Sur les environnements d’évaluation et de production Pro, vous devez [envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour récupérer une sauvegarde spécifique indiquant la date, l’heure et le fuseau horaire dans le ticket.
>
>L’Adobe ne restaure **pas** les environnements à partir d’une sauvegarde automatique. Consultez [Restaurer un instantané de base de données à partir de l&#39;évaluation ou de la production](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) pour choisir une méthode de restauration d&#39;un instantané d&#39;évaluation ou de production.

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
>[Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour modifier la configuration du service dans les environnements de production et d’évaluation Pro.

## Changement de service {#service-change-tip}

>[!TIP]
>
>Après la configuration initiale du service, vous pouvez modifier la version du logiciel d’un service installé en mettant à jour les fichiers de configuration `services.yaml` et `.magento.app.yaml`. Consultez [Modifier la version du service](/help/cloud-guide/services/services-yaml.md#change-service-version) pour obtenir des conseils sur la mise à niveau ou la rétrogradation d’un service.

## Conseil de déploiement bloqué {#stuck-deployment-tip}

>[!TIP]
>
>Pour obtenir de l’aide sur les déploiements bloqués, utilisez l’utilitaire de dépannage de déploiement [Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) dans le Centre d’aide de _Commerce_.

## Mise à jour des outils de la CEE {#ece-tools-package}

>[!NOTE]
>
>Si vous utilisez une version d’Adobe Commerce sur une infrastructure cloud qui ne contient pas le package `ece-tools`, vous devez effectuer une [mise à niveau ponctuelle](/help/cloud-guide/dev-tools/install-package.md) sur votre projet cloud pour supprimer les packages obsolètes. Si vous utilisez actuellement le module `ece-tools` et que vous devez le mettre à jour, voir [Mettre à jour le module ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Conseil de mise à niveau {#upgrade-tip}

>[!TIP]
>
>Avant de commencer une mise à niveau ou un processus d’application de correctifs, créez une branche active à partir de l’environnement d’intégration et extrayez la nouvelle branche sur votre station de travail locale. Dédier une branche à la mise à niveau ou au processus de correctif permet d’éviter toute interférence avec votre travail en cours.

<!-- Fastly-related snippets begin -->

## Login de l’administrateur {#admin-login-step}

1. [Connectez-vous](/help/get-started/onboarding.md#access-your-admin-panel) à l’administrateur.

## Automatiser le déploiement de fragments de code VCL personnalisés {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Au lieu de charger manuellement des fragments de code VCL personnalisés, vous pouvez les ajouter au répertoire `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` dans votre environnement. Les fragments de code présents dans ce répertoire sont chargés automatiquement lorsque vous cliquez sur _charger un fichier VCL vers Fastly_ dans Commerce Admin. Consultez la section [Déploiement automatisé de fragments de code VCL personnalisés](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) dans la documentation du module Fastly CDN pour Magento 2 .

<!-- Fastly-related snippets end -->
