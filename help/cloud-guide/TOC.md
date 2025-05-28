---
user-guide-title: Guide de Commerce sur le cloud
user-guide-description: Découvrez comment gérer l’application Adobe Commerce sur l’infrastructure cloud.
product: magento
feature: Cloud
source-git-commit: 3347ad0a5fe202cbd80d08b7289c20a1c98ed1e3
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 8%

---


# Commerce sur les infrastructures cloud {#user-guide}

+ [Commerce](overview.md)
+ Architecture {#architecture}
   + [Infrastructure cloud](architecture/cloud-architecture.md)
   + [Sécurité](architecture/security.md)
   + [Pile technologique](architecture/tech-stack.md)
   + [Architecture de démarrage](architecture/starter-architecture.md)
   + [Workflow de démarrage](architecture/starter-develop-deploy-workflow.md)
   + [Architecture pro](architecture/pro-architecture.md)
   + [Workflow Pro](architecture/pro-develop-deploy-workflow.md)
   + [Architecture à grande échelle](architecture/scaled-architecture.md)
   + [Mise à l’échelle automatique](architecture/autoscaling.md)
+ [Commencer](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html?lang=fr)
+ Notes de mise à jour {#release-notes}
   + [Suite d’outils cloud](release-notes/cloud-tools-suite.md)
   + [Ensemble d&#39;outils de la CEE](release-notes/ece-tools-package.md)
   + [Correctifs cloud](release-notes/cloud-patches.md)
   + [Package Cloud Docker](release-notes/cloud-docker.md)
   + [Composants cloud](release-notes/cloud-components.md)
   + [Packages cloud](release-notes/cloud-packages.md)
   + [Modifications non rétrocompatibles](release-notes/backward-incompatible-changes.md)
   + [Archive des notes de mise à jour](release-notes/cloud-release-archive.md)
+ Projet cloud {#project}
   + [Présentation du projet](project/overview.md)
   + [Structure du projet](project/file-structure.md)
   + [Accès utilisateur](project/user-access.md)
   + [Authentification multifacteur](project/multi-factor-authentication.md)
   + [Flux d’activité](project/activity-stream.md)
   + [E-mails sortants](project/outgoing-emails.md)
   + [Service de messagerie SendGrid](project/sendgrid.md)
   + [Gestion des branches de console](project/console-branches.md)
   + [Adresses IP régionales](project/regional-ip-addresses.md)
+ Outils de développement {#dev-tools}
   + [Vue d’ensemble](dev-tools/overview.md)
   + Cloud CLI {#cloud-cli}
      + [Présentation de l’interface de ligne de commande](dev-tools/cloud-cli-overview.md)
      + [Référence de l’interface de ligne de commande](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + Outils CEE {#ece-tools}
      + [Présentation du package](dev-tools/package-overview.md)
      + [Mise à niveau ponctuelle pour l&#39;utilisation des outils CEE](dev-tools/install-package.md)
      + [Mise à jour du progiciel ECE-Tools](dev-tools/update-package.md)
      + [Référence de l’interface de ligne de commande](dev-tools/ece-tools-cli-reference.md)
      + [Référence d’erreur](dev-tools/error-reference.md)
   + Intégrations {#integrations}
      + [Vue d’ensemble](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [Notifications d’intégrité](integrations/health-notifications.md)
+ Développement {#develop}
   + [Vue d’ensemble](development/overview.md)
   + [Clés d’authentification](development/authentication-keys.md)
   + [Gestion des branches de l’interface de ligne de commande](development/cli-branches.md)
   + [Connexions sécurisées](development/secure-connections.md)
   + Déployer {#deploy}
      + [Processus de déploiement](deploy/process.md)
      + [Optimisation](deploy/optimization.md)
      + [Bonnes pratiques](deploy/best-practices.md)
      + [Déploiement basé sur un scénario](deploy/scenario-based.md)
      + [Déploiement sans temps d’arrêt](deploy/reduce-downtime.md)
      + [Déploiement de contenu statique](deploy/static-content.md)
      + [Assistants intelligents](deploy/smart-wizards.md)
      + [Déploiement dans les environnements d’évaluation et de production](deploy/staging-production.md)
      + [Récupération après une panne de composant](deploy/recover-failed-deployment.md)
   + Test {#test}
      + [Conseils de test](test/guidance.md)
      + [Logs](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [Données d’exemple](test/sample-data.md)
      + [Évaluation et production](test/staging-and-production.md)
      + [Deuxième environnement d’évaluation](test/second-staging.md)
   + [Service PrivateLink](development/privatelink-service.md)
   + [Bloc de protection](development/protective-block.md)
   + [Restaurer l’environnement](development/restore-environment.md)
   + Stockage {#storage}
      + [Gestion de l’espace disque](storage/manage-disk-space.md)
      + [Requêtes de base de données de profils](storage/profile-database-queries.md)
      + [Sauvegarde de la base de données](storage/database-dump.md)
      + [Gestion des sauvegardes](storage/snapshots.md)
   + Mises à niveau et correctifs {#upgrade}
      + [Bonnes pratiques](development/best-practices.md)
      + [Mettre à niveau la version de Commerce](development/commerce-version.md)
      + [Application de correctifs](development/apply-patches.md)
+ Configuration {#configure}
   + [Vue d’ensemble](environment/overview.md)
   + Application {#app}
      + [Configuration du déploiement de l’application](application/configure-app-yaml.md)
      + [Paramètres PHP](application/php-settings.md)
      + Propriétés {#properties}
         + [Propriétés de l’application](application/properties.md)
         + [Crons](application/crons-property.md)
         + [Pare-feu (Starter uniquement)](application/firewall-property.md)
         + [Crochets](application/hooks-property.md)
         + [Variables](application/variables-property.md)
         + [Web](application/web-property.md)
         + [Travailleurs](application/workers-property.md)
      + [Définir le cache pour les fichiers statiques](application/set-cache.md)
   + Environnement {#env}
      + [Configuration du déploiement de l’environnement](environment/configure-env-yaml.md)
      + [Niveaux et options variables](environment/variable-levels.md)
      + Remplacer les variables {#stage}
         + [Variables d’environnement](environment/variables-intro.md)
         + [ADMIN](environment/variables-admin.md)
         + [Variables cloud](environment/variables-cloud.md)
         + [Global](environment/variables-global.md)
         + [Build](environment/variables-build.md)
         + [Déployer](environment/variables-deploy.md)
         + [Après le déploiement](environment/variables-post-deploy.md)
      + Configuration des notifications {#log}
         + [Notifications](environment/set-up-notifications.md)
         + [Gestionnaires de journaux](environment/log-handlers.md)
   + Itinéraires {#routes}
      + [Configuration des itinéraires](routes/routes-yaml.md)
      + [Mise en cache](routes/caching.md)
      + [Redirections](routes/redirects.md)
      + [Inclusions côté serveur](routes/server-side-includes.md)
   + Services tertiaires {#service}
      + [Configuration des services](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Valkey](services/valkey.md)
+ Services Fastly {#cdn}
   + [Vue d’ensemble](cdn/fastly.md)
   + Configuration rapide {#setup-fastly}
      + [Configuration des services Fastly](cdn/fastly-configuration.md)
      + [Personnalisation de la configuration du cache](cdn/fastly-custom-cache-configuration.md)
      + [Personnaliser les pages d’erreur et de maintenance](cdn/fastly-custom-response.md)
   + [Pare-feu d’application web](cdn/fastly-waf-service.md)
   + [Optimisation des images](cdn/fastly-image-optimization.md)
   + Personnaliser avec VCL {#custom-vcl-snippets}
      + [Prise en main](cdn/fastly-vcl-custom-snippets.md)
      + [Réacheminer les requêtes vers un serveur principal CMS](cdn/fastly-vcl-wordpress.md)
      + [Bloquer le spam de référence](cdn/fastly-vcl-badreferer.md)
      + [IP, place sur la liste autorisée](cdn/fastly-vcl-allowlist.md)
      + [IP, place sur la liste bloquée](cdn/fastly-vcl-blocking.md)
      + [Contournement du cache Fastly](cdn/fastly-vcl-bypass-to-origin.md)
   + [Résolution rapide des problèmes](cdn/fastly-troubleshooting.md)
+ Paramètres de la boutique {#configure-store}
   + [Vue d’ensemble](store/overview.md)
   + [Bonnes pratiques](store/best-practices.md)
   + [Thème personnalisé](store/custom-theme.md)
   + [Extensions](store/extensions.md)
   + [Module B2B](store/b2b-module.md)
   + [Plusieurs sites](store/multiple-sites.md)
   + [Plan du site et moteurs de recherche robots](store/robots-sitemap.md)
   + [Modes de paiement PayPal](store/paypal.md)
   + [Gestion de la configuration](store/store-settings.md)
+ Site Launch {#launch}
   + [Vue d’ensemble](launch/overview.md)
   + [Liste de contrôle de Launch](launch/checklist.md)
   + [Étapes de lancement](launch/steps.md)
+ Surveiller le site {#monitor}
   + [Performances](monitor/performance.md)
   + [Télémétrie opérationnelle](monitor/operational-telemetry.md)
   + Service New Relic {#new-relic}
      + [Présentation de New Relic](monitor/new-relic-service.md)
      + [Gestion des comptes et des utilisateurs](monitor/account-management.md)
      + Examiner les performances {#investigate}
         + [Politiques, alertes et workflows](monitor/investigate-performance.md)
         + [Ingestion des données](monitor/ingest-data.md)
         + [Suivi des déploiements](monitor/track-deployments.md)
      + [Gestion des logs](monitor/log-management.md)
