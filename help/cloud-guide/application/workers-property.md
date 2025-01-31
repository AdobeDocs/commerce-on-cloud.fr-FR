---
title: Travailleurs
description: Découvrez comment configurer la propriété workers dans le fichier  [!DNL Commerce]  configuration de l’application.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Propriété des collaborateurs

Vous pouvez définir un programme de travail qui s’exécute indépendamment de l’instance web sans instance Nginx en cours d’exécution. Cependant, le programme de travail utilise le même stockage réseau que celui utilisé par l’application [!DNL Commerce]. Vous n’avez pas besoin de configurer un serveur web sur l’instance de travail (à l’aide de Node.js ou de Go), car le routeur ne peut pas diriger les requêtes publiques vers l’instance de travail. Cela rend l’instance de travail idéale pour les tâches en arrière-plan ou les tâches en cours d’exécution qui risquent de bloquer un déploiement.

## Configuration d’un programme de travail

Les programmes de travail sont disponibles uniquement pour les environnements d’évaluation et de production Pro. Les environnements d’intégration Pro et de démarrage peuvent choisir d’utiliser la variable [CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner).

Pour configurer un programme de travail dans Pro Staging ou Production, [Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) et renseignez les informations suivantes :

- ID de projet
- Identifiant de l’environnement
- Nom du collaborateur
- Commandes de démarrage

Vous pouvez configurer un processus par programme de travail. Une configuration de travail de base et commune dans le fichier `.magento.app.yaml` peut se présenter comme suit :

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

Cet exemple définit un programme de travail unique nommé `queue`, avec un faible niveau (taille S) d&#39;allocation de ressources, et exécute la commande `php ./bin/magento` au démarrage. Le `queue` de travail s’exécute ensuite sur chaque nœud en tant que processus de travail. Si la commande se ferme, elle est automatiquement redémarrée.

## Commandes et remplacements

La clé `commands.start` est requise pour lancer des commandes avec l’application de travail. Vous pouvez utiliser n’importe quelle commande shell valide, bien qu’il soit idéal d’utiliser la langue de votre application. Si la commande spécifiée par la clé `start` s&#39;arrête, elle redémarre automatiquement.

>[!IMPORTANT]
>
>Les hooks `deploy` et `post_deploy` et les commandes `crons` s’exécutent uniquement sur le conteneur web, et non dans les instances de travail.

### Héritage

Les définitions des propriétés `size`, `relationships`, `access`, `disk` et `mount`, et `variables` sont héritées par un programme de travail, sauf si elles sont explicitement remplacées.

Les propriétés suivantes sont les plus couramment utilisées pour remplacer les [paramètres de niveau supérieur](properties.md) :

- `size` : allouer moins de ressources à un seul processus en arrière-plan
- `variables` : indiquez à l&#39;application de s&#39;exécuter différemment

### Planning et mise en file d’attente

Bien que chaque programme de travail fasse la file derrière un autre, la configuration suivante produit une séparation cohérente de deux secondes dans les horodatages du fichier `var/time.txt`, indépendamment de la mise en veille de huit secondes dans le code PHP :

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
