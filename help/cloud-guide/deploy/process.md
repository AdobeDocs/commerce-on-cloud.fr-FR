---
title: Processus de déploiement
description: Découvrez comment fonctionne le déploiement pour Adobe Commerce sur les projets d’infrastructure cloud.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Processus de déploiement

Le processus de déploiement commence lorsque vous effectuez une fusion, une notification push ou une synchronisation de votre environnement, ou lorsque vous déclenchez un [redéploiement manuel](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). Le processus de déploiement prend du temps, mais il existe des moyens d’optimiser le déploiement, selon que vous développez et testez ou travaillez sur un site en ligne. Vous pouvez notamment contrôler le [déploiement de contenu statique](static-content.md).

Le processus de déploiement comporte trois phases distinctes : création, déploiement et post-déploiement. Chaque phase effectue des actions spécifiques avec des ressources limitées :

## ![Phase de création](../../assets/status-build.png) Phase de création

La phase _build_ assemble les conteneurs pour les services définis dans les fichiers de configuration, installe les dépendances en fonction du fichier `composer.lock` et exécute les hooks de build définis dans le fichier `.magento.app.yaml`. Sans la possibilité de se connecter à un service ou d’accéder à la base de données, la phase de création dépend des ressources limitées à l’environnement.

## ![Phase de déploiement](../../assets/status-deploy.png) Phase de déploiement

La phase _déploiement_ place une suspension temporaire sur les requêtes entrantes et fait passer le site en [mode de maintenance](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=fr). La phase de déploiement utilise les nouveaux conteneurs et, après le montage du système de fichiers, ouvre les connexions réseau, active les services définis dans la section `relationships` du fichier `.magento.app.yaml` et exécute les hooks de déploiement définis dans le fichier `.magento.app.yaml`. Tout est _lecture seule_, à l’exception des répertoires définis dans le fichier `.magento.app.yaml`. Par défaut, la propriété [`mounts`](../application/properties.md#mounts) comprend les répertoires suivants :

- `app/etc` : contient les fichiers de configuration `env.php` et `config.php`
- `pub/media` : contient toutes les données multimédia, telles que les produits ou les catégories
- `pub/static` : contient les fichiers statiques générés.
- `var` : contient les fichiers temporaires créés lors de l&#39;exécution

Tous les autres répertoires disposent d&#39;autorisations en lecture seule. Le nouveau site devient actif à la fin de la phase de déploiement lorsqu’il quitte le mode de maintenance et lève la suspension temporaire des requêtes entrantes.

Lors de la phase de déploiement, des copies des fichiers de configuration de déploiement `app/etc/config.php` et `app/etc/env.php` sont enregistrées avec l’extension BAK. Voir [Paramètres du magasin](../store/store-settings.md#restore-configuration-files) pour en savoir plus sur la restauration de ces fichiers.

## ![Phase post-déploiement](../../assets/status-post-deploy.png) Phase post-déploiement

La phase _post-déploiement_ exécute les hooks de post-déploiement définis dans le fichier `.magento.app.yaml`. L’exécution d’une action au cours de cette phase peut affecter les performances du site. Vous pouvez toutefois utiliser la variable d’environnement [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) pour remplir le cache.

## ![Vérification de l’état](../../assets/status-verify.png) Vérification des configurations

Vous pouvez tester la configuration optimale pour l’état de votre projet en exécutant les [ Assistants intelligents ](smart-wizards.md).

>[!NOTE]
>
>Avec la version 2002.1.0 d’`ece-tools` et les versions ultérieures, vous pouvez utiliser la fonctionnalité de déploiement basée sur des scénarios pour personnaliser les processus de création, de déploiement et de post-déploiement pour votre projet d’infrastructure cloud d’Adobe Commerce. Voir [Déploiement basé sur un scénario](scenario-based.md).
