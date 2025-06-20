---
title: Déploiement de contenu statique
description: Découvrez les stratégies de déploiement de contenu statique, tel que des images, des scripts et des feuilles CSS, sur Adobe Commerce dans les projets d’infrastructure cloud.
feature: Cloud, Build, Deploy, SCD
exl-id: 8f30cae7-a3a0-4ce4-9c73-d52649ef4d7a
source-git-commit: 325b7584daa38ad788905a6124e6d037cf679332
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Stratégies de déploiement de contenu statique

Le déploiement de contenu statique (SCD) a un impact significatif sur le processus de déploiement du magasin. Cela dépend de la quantité de contenu à générer (images, scripts, CSS, vidéos, thèmes, paramètres régionaux et pages web) et du moment où le contenu doit être généré. Par exemple, la stratégie par défaut génère du contenu statique pendant la [phase de déploiement](process.md#deploy-phase-deploy-phase) lorsque le site est en mode de maintenance. Toutefois, cette stratégie de déploiement prend du temps pour écrire le contenu directement dans le répertoire de `pub/static` monté. Plusieurs options ou stratégies vous permettent d’améliorer le temps de déploiement en fonction de vos besoins.

## Optimisation du contenu JavaScript et HTML

Vous pouvez utiliser le regroupement et la minimisation pour créer du contenu JavaScript et HTML optimisé lors du déploiement de contenu statique.

### Minimiser le contenu

Vous pouvez réduire le temps de chargement du SCD pendant le processus de déploiement si vous ignorez la copie des fichiers de vue statiques dans le répertoire `var/view_preprocessed` et si vous générez _miniaturisé_ HTML lorsque cela est nécessaire. Vous pouvez l’activer en définissant la variable d’environnement global [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) sur `true` dans le fichier `.magento.env.yaml`.

>[!NOTE]
>
>À partir de la version 2002.0.13 du package `ece-tools`, la valeur par défaut de la variable SKIP_HTML_MINIFICATION est définie sur `true`.

Vous pouvez économiser **plus** de temps de déploiement et d’espace disque en réduisant le nombre de fichiers de thème inutiles. Par exemple, vous pouvez déployer le thème `magento/backend` en anglais et un thème personnalisé dans d’autres langues. Vous pouvez configurer ces paramètres de thème avec la variable d’environnement [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix).

## Choisir une stratégie de déploiement

Les stratégies de déploiement varient selon que vous choisissez de générer du contenu statique pendant la phase _build_, la phase _déploiement_ ou _à la demande_. Comme le montre le graphique suivant, la génération de contenu statique pendant la phase de déploiement est le choix le moins optimal. Même avec HTML miniaturisé, chaque fichier de contenu doit être copié dans le répertoire de `~/pub/static` monté, ce qui peut prendre beaucoup de temps. La génération de contenu statique à la demande semble être le choix optimal. Cependant, si le fichier de contenu n’existe pas dans le cache, il est généré au moment où il est demandé, ce qui ajoute du temps de chargement à l’expérience utilisateur. Par conséquent, la génération de contenu statique pendant la phase de création est la plus optimale.

![Comparaison de charge SCD](../../assets/scd-load-times.png)

### Définition du SCD sur la version

La génération de contenu statique pendant la phase de création avec HTML miniaturisé est la configuration optimale pour des déploiements [**zéro temps d’arrêt**](reduce-downtime.md), également appelés **état idéal**. Au lieu de copier des fichiers sur un lecteur monté, il crée un lien symbolique à partir du répertoire `./init/pub/static`.

La génération de contenu statique nécessite l’accès aux thèmes et aux paramètres régionaux. Adobe Commerce stocke les thèmes dans le système de fichiers, qui est accessible pendant la phase de création. Toutefois, Adobe Commerce stocke les paramètres régionaux dans la base de données. La base de données n’est _pas_ disponible pendant la phase de création. Pour générer le contenu statique pendant la phase de création, vous devez utiliser la commande `config:dump` du package de `ece-tools` pour déplacer les paramètres régionaux vers le système de fichiers. Il lit les paramètres régionaux et les enregistre dans le fichier `app/etc/config.php`.

>[!NOTE]
>Après avoir exécuté la commande `config:dump` dans le package `ece-tools`, les configurations qui sont vidées dans le fichier `config.php` [sont verrouillées (grisées) dans le tableau de bord Admin](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/locked-fields-in-magento-admin). la seule façon de mettre à jour ces configurations dans l’administration consiste à les supprimer du fichier localement et à redéployer le projet.
>>En outre, chaque fois que vous ajoutez un nouveau magasin/groupe de magasins/site web à votre instance, vous devez penser à exécuter la commande `config:dump` pour vous assurer que la base de données est synchronisée. Vous pouvez également choisir [les configurations à vider](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/configuration-management/export-configuration?lang=en) dans le fichier `config.php`.
>>Si vous supprimez la configuration magasin/groupe de magasins/site web du fichier `config.php` parce que les champs sont grisés mais que vous négligez d’effectuer cette étape, les nouvelles entités qui n’ont pas été extraites sont supprimées de la base de données lors du déploiement suivant.

**Pour configurer votre projet afin de générer un SCD sur la version** :

1. Sur votre station de travail locale, accédez au répertoire du projet.
1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Déplacez les paramètres régionaux vers le système de fichiers, puis mettez à jour le fichier [`config.php`](../development/commerce-version.md#create-a-configphp-file).

1. Le fichier de configuration `.magento.env.yaml` doit contenir les valeurs suivantes :

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) est `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) sur l’étape de création est `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) est `compact`

1. Vérifiez la configuration du hook [Post-déploiement](../application/hooks-property.md) dans le fichier `.magento.app.yaml`.

1. Vérifiez vos paramètres en exécutant l’assistant [ Smart pour obtenir l’état idéal](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### Définition du SCD à la demande

La génération de SCD à la demande est optimale pour un workflow de développement dans l’environnement d’intégration. Cela réduit le temps de déploiement afin que vous puissiez rapidement vérifier vos implémentations et exécuter des tests d’intégration. Activez la variable d’environnement [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) dans l’étape globale du fichier `.magento.env.yaml`. La variable SCD_ON_DEMAND remplace toutes les autres configurations liées au SCD et efface le contenu existant du répertoire `~/pub/static`.

Lors de l’utilisation de la stratégie SCD à la demande, il est utile de précharger le cache avec les pages que vous prévoyez de demander, telles que la page d’accueil. Ajoutez votre liste de pages attendues dans la variable d’environnement [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) à l’étape de post-déploiement du fichier `.magento.env.yaml`.

>[!WARNING]
>
>N’utilisez pas la stratégie SCD à la demande dans l’environnement de production.

### SCD ignoré

Parfois, vous pouvez choisir d’ignorer complètement la génération du contenu statique. Vous pouvez définir la variable d’environnement [SKIP_SCD](../environment/variables-build.md#skipscd) dans l’étape globale pour ignorer les autres configurations liées à SCD. Cela n’affecte pas le contenu existant dans le répertoire `~/pub/static`.
