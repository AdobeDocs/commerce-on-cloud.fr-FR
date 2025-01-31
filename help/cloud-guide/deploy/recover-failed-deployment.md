---
title: Récupération après une panne de composant
description: Découvrez comment effectuer une récupération si un composant ne parvient pas à se déployer correctement dans Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Récupération après une panne de composant

Cette rubrique explique comment effectuer une récupération si un composant ne se déploie pas correctement. Des exemples typiques incluent des composants qui ont des dépendances qui ne sont pas satisfaites par votre environnement distant, telles que des versions PHP incompatibles.

Vous pouvez effectuer une récupération à la suite d’un déploiement ayant échoué de l’une des manières suivantes :

- [Restaurer une sauvegarde](../storage/snapshots.md#restore-a-snapshot)
- Nettoyer le projet et le code des modifications précédentes et les redéployer

## Nettoyer, supprimer et redéployer

Pour effectuer un nettoyage à partir du déploiement précédent, identifiez le composant qui a été ajouté ou mis à jour, puis supprimez-le. Tout d’abord, connectez-vous à l’environnement distant et effacez manuellement le contenu du répertoire `var`. Supprimez ensuite le composant du fichier `composer.json` et redéployez l’environnement.

**Pour nettoyer les répertoires `var`, procédez comme suit**

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Effacez les répertoires `var`.

   ```shell
   rm -rf var/*
   ```

1. Déconnectez-vous.

**Pour supprimer le composant** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Effacez le cache.

   ```bash
   composer clear-cache
   ```

1. Supprimez le composant du fichier `composer.json`.

   ```bash
   composer remove <component-name>:<version>
   ```

   Si le message suivant s’affiche, vous n’avez rien à faire d’autre :

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Patientez pendant la mise à jour des dépendances.

1. Ajout, validation et modifications de code push.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Pour en savoir plus sur la restauration d’un environnement sans sauvegarde, consultez [Restaurer un environnement](../development/restore-environment.md).

{{stuck-deployment-tip}}
