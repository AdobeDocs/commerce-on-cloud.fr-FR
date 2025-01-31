---
title: Données d’exemple
description: Découvrez comment installer des données d’exemple avec Adobe Commerce sur une infrastructure cloud.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Données d’exemple

Si vous avez besoin de données d’exemple lors du développement de votre boutique, vous pouvez installer des données d’exemple. Ces données simulent un magasin Adobe Commerce actif avec des clients, des produits et d’autres données. Ces exemples de données fonctionnent mieux avec une nouvelle installation d’Adobe Commerce sur un modèle d’infrastructure cloud.

Il est recommandé d’installer des données d’exemple dans les environnements de développement et d’intégration. Si vous utilisez des données d’exemple dans les environnements d’évaluation ou de production, vous devez [supprimer](#reset-or-uninstall-sample-data) les informations et les produits avant la mise en ligne.

## Installer des exemples de données

Pour installer des données d’exemple :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. À la racine, saisissez la commande suivante pour ajouter des données d’exemple :

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. Attendez que les composants soient mis à jour.

1. Validez et envoyez les modifications :

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Attendez que le projet soit déployé.

1. Vérifiez que l’installation a réussi en accédant à la page de votre storefront dans l’environnement d’intégration. Vous pouvez localiser le lien URL vers le storefront via le [!DNL Cloud Console].

1. Prenez un instantané de votre environnement :

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

Vous pouvez commencer à tester votre développement avec des données actives.

## Réinitialisation ou désinstallation des exemples de données

Vous pouvez réinitialiser ou supprimer des exemples de données en suivant la même procédure que celle utilisée pour installer les exemples de données :

- Supprimer les données d’exemple : `./bin/magento sampledata:remove`
- Réinitialiser les données d’exemple : `./bin/magento sampledata:reset`
