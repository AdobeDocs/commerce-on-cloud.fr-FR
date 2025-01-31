---
title: Bloc de protection
description: Découvrez la fonction de blocage protecteur d’Adobe Commerce sur l’infrastructure cloud et son fonctionnement pour protéger votre site contre les vulnérabilités de sécurité connues.
feature: Cloud, Configuration, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# Bloc de protection

Adobe Commerce sur les infrastructures cloud dispose d’une fonction de blocage protectrice qui, dans certaines circonstances, limite l’accès aux sites web présentant des failles de sécurité. Cette méthode de blocage partiel empêche l’exploitation des vulnérabilités de sécurité connues. Les logiciels obsolètes contiennent souvent des exploits. Il est donc important de se protéger contre ces exploits en bloquant partiellement l’accès à ces sites.

## Fonctionnement du bloc de protection

Adobe Commerce conserve une base de données de signatures de vulnérabilités de sécurité connues dans les logiciels open source couramment déployés sur les infrastructures cloud. Le contrôle de sécurité analyse uniquement les vulnérabilités connues dans les projets open source ; il ne peut pas examiner les personnalisations que vous écrivez.

L’analyse de sécurité s’exécute :

- Lorsque vous poussez un nouveau code vers Git
- Lorsque de nouvelles vulnérabilités sont ajoutées à la base de données

Si une vulnérabilité critique est détectée dans votre application, elle rejette la notification push Git.

Deux types de blocs s’exécutent :

1. **Bloc complet**—pour les sites Web de développement. Le message d’erreur accompagnant `git push` fournit des informations détaillées sur la vulnérabilité.

1. **Blocage partiel** : pour les sites Web de production, ce qui permet au site de rester principalement en ligne. Selon la nature de la vulnérabilité, des parties d’une requête, telles qu’une chaîne de requête, des cookies ou des en-têtes supplémentaires, peuvent être supprimées des requêtes GET. Toutes les autres requêtes peuvent être entièrement bloquées, telles que la connexion, l’envoi de formulaire ou le passage en caisse du produit.

Le déblocage est automatisé lors de la résolution du risque de sécurité. Le bloc est supprimé peu de temps après l’application d’une mise à niveau de sécurité qui supprime la vulnérabilité.

## Exclusion du bloc de protection

Le bloc de protection est là pour vous protéger contre les vulnérabilités connues du logiciel que vous déployez Adobe Commerce sur l’infrastructure cloud.

Cependant, vous pouvez vous exclure du bloc de protection en ajoutant ce qui suit à [`.magento.app.yaml`](../application/configure-app-yaml.md) :

```yaml
   preflight:
      enabled: false
```

Vous pouvez explicitement vous exclure d’un contrôle spécifique, par exemple :

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
