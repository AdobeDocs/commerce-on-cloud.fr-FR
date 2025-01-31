---
title: Assistants intelligents
description: Découvrez comment utiliser des Assistants intelligents pour évaluer si votre projet d’infrastructure cloud Adobe Commerce respecte les bonnes pratiques de déploiement.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Assistants intelligents

Les assistants intelligents peuvent vous aider à déterminer si votre configuration cloud suit les bonnes pratiques. Les assistants disponibles vous aident à effectuer les configurations suivantes :

- État idéal pour un temps d’arrêt de déploiement minimal
- Configuration de l’équilibrage de charge pour la base de données et Redis
- Déploiement de contenu statique (SCD) pour la demande, l’étape de création ou l’étape de déploiement

Chacune des commandes de l’assistant intelligent fournit une réponse de vérification et, le cas échéant, une recommandation pour la configuration appropriée.

| Commande | Description |
| ------- | ------------|
| `wizard:ideal-state` | Vérifiez que SCD se trouve sur l’étape _build_, que la variable `SKIP_HTML_MINIFICATION` est `true` et que le hook post_deploy est configuré dans l’environnement Cloud. Ne pas utiliser dans l’environnement de développement local. |
| `wizard:master-slave` | Vérifiez que la variable `REDIS_USE_SLAVE_CONNECTION` et la variable `MYSQL_USE_SLAVE_CONNECTION` sont `true`. |
| `wizard:scd-on-demand` | Vérifiez que la variable d’environnement globale `SCD_ON_DEMAND` est `true`. |
| `wizard:scd-on-build` | Vérifiez que la variable d’environnement globale `SCD_ON_DEMAND` est `false` et que la variable d’environnement `SKIP_SCD` est `false` pour l’étape _build_. Vérifie que le fichier `config.php` contient des informations pour les magasins, les groupes de magasins et les sites Web. |
| `wizard:scd-on-deploy` | Vérifiez que la variable d’environnement globale `SCD_ON_DEMAND` est `false` et que la variable d’environnement `SKIP_SCD` est `false` pour l’étape _deploy_. Vérifie que le fichier `config.php` ne contient _PAS_ la liste des magasins, des groupes de magasins et des sites Web avec des informations connexes. |

Par exemple, vous pouvez vérifier que votre configuration active correctement la fonctionnalité SCD à la demande :

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Une configuration réussie renvoie :

```
SCD on-demand is enabled
```

Une configuration ayant échoué renvoie :

```
SCD on-demand is disabled
```

## Vérification d’une configuration idéale

La configuration _idéale_ de votre projet cloud contribue à réduire le temps d’arrêt du déploiement en réchauffant le cache et en générant du contenu statique lorsque l’utilisateur le demande. Cet assistant s’exécute automatiquement pendant le processus de déploiement. Si votre cloud n’est pas configuré pour cet _état idéal_, vous recevez un message similaire à celui-ci :

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

En fonction de la sortie, vous devez apporter les corrections suivantes à votre configuration :

1. Activez la variable de minimisation Ignorer l’HTML .

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Configurez le hook de post-déploiement.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Envoyez vos modifications de code et réexécutez le test. Lorsque votre configuration est _idéale_, vous recevez le message suivant.

   ```
   Ideal state is configured
   ```
