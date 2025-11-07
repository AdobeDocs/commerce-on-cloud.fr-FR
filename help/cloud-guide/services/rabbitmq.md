---
title: Configuration du service RabbitMQ
description: Découvrez comment activer le service RabbitMQ pour gérer les files d’attente de messages pour Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 76a9721767cbd4328347311cc308810f0f7914c0
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Configuration du service [!DNL RabbitMQ]

Le [Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=fr) est un système d’Adobe Commerce qui permet à un [module](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/glossary#module) de publier des messages dans les files d’attente. Il définit également les consommateurs qui reçoivent les messages de manière asynchrone.

Le MQF utilise [RabbitMQ](https://www.rabbitmq.com/) comme courtier de messagerie, ce qui fournit une plateforme évolutive pour envoyer et recevoir des messages. Il comprend également un mécanisme de stockage des messages non diffusés. [!DNL RabbitMQ] est basé sur la spécification AMQP (Advanced Message Queuing Protocol) 0.9.1.

>[!NOTE]
>
>Adobe Commerce sur les infrastructures cloud prend également en charge [ActiveMQ Artemis](activemq.md) en tant que service de file d’attente de messages alternatif à l’aide du protocole STOMP.

>[!IMPORTANT]
>
>Si vous préférez utiliser un service basé sur AMQP existant, comme [!DNL RabbitMQ], au lieu de vous fier à Adobe Commerce sur une infrastructure cloud pour le créer, utilisez la variable d’environnement [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) pour le connecter à votre site.

{{service-instruction}}

**Pour activer RabbitMQ** :

1. Ajoutez le nom, le type et la valeur de disque requis (en Mo) au fichier `.magento/services.yaml` avec la version de RabbitMQ installée.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. Configurez les relations dans le fichier `.magento.app.yaml`.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Vérifier les relations de service](services-yaml.md#service-relationships).

{{service-change-tip}}

## Se connecter à RabbitMQ pour le débogage

À des fins de débogage, il est utile de se connecter directement à une instance de service de l’une des manières suivantes :

- Connexion à partir de votre environnement de développement local
- Connexion depuis l&#39;application
- Connexion depuis votre application PHP

### Connexion à partir de votre environnement de développement local

1. Connectez-vous à l’interface de ligne de commande `magento-cloud` et au projet :

   ```bash
   magento-cloud login
   ```

1. Consultez l’environnement avec RabbitMQ installé et configuré.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Utilisez SSH pour vous connecter à l’environnement cloud :

   ```bash
   magento-cloud ssh
   ```

1. Récupérez les informations de connexion et les informations d’identification RabbitMQ à partir de la variable [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) :

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Dans la réponse, recherchez les informations RabbitMQ, par exemple :

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Activez le transfert du port local vers RabbitMQ (si votre projet est situé dans une autre région telle que US-3, EU-5 ou région AP-3, remplacez ``us-3``/``eu-5``/``ap-3`` par ``us``).

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Voici un exemple d’accès à l’interface web de gestion de RabbitMQ à l’adresse `http://localhost:15672` :

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. Lorsque la session est ouverte, vous pouvez démarrer un client RabbitMQ de votre choix à partir de votre poste de travail local, configuré pour se connecter au `localhost:<portnumber>` à l’aide du numéro de port, du nom d’utilisateur et du mot de passe de la variable MAGENTO_CLOUD_RELATIONSHIPS .

### Connexion depuis l&#39;application

Pour vous connecter à RabbitMQ s’exécutant dans une application, installez un client, tel que [amqp-utils](https://github.com/dougbarth/amqp-utils), en tant que dépendance de projet dans votre fichier `.magento.app.yaml`.

Par exemple,

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

Lorsque vous vous connectez à votre container PHP, vous entrez n&#39;importe quelle commande `amqp-` disponible pour gérer vos files d&#39;attente.

### Connexion depuis votre application PHP

Pour vous connecter à RabbitMQ à l&#39;aide de votre application PHP, ajoutez une bibliothèque PHP à votre arborescence source.

## Résolution des problèmes liés au service [!DNL RabbitMQ]

Voir [Impossible de se connecter à RabbitMQ dans Adobe Commerce Cloud](https://experienceleague.adobe.com/fr/docs/experience-cloud-kcs/kbarticles/ka-27688).

## Mettre à niveau le service [!DNL RabbitMQ]

Pour obtenir des instructions de mise à niveau, voir [&#x200B; Modifier la version du service &#x200B;](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version).
