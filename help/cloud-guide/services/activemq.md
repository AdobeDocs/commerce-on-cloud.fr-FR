---
title: Configuration du service ActiveMQ
description: Découvrez comment activer le service ActiveMQ Artemis pour gérer les files d’attente de messages pour Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Configuration du service [!DNL ActiveMQ]

Le [Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=fr) est un système d’Adobe Commerce qui permet à un [module](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/glossary#module) de publier des messages dans les files d’attente. Il définit également les consommateurs qui reçoivent les messages de manière asynchrone.

Le MQF peut utiliser [ActiveMQ Artemis](https://activemq.apache.org/components/artemis/) comme courtier de messagerie, ce qui fournit une plateforme évolutive pour envoyer et recevoir des messages. Il comprend également un mécanisme de stockage des messages non diffusés. [!DNL ActiveMQ Artemis] prend en charge le protocole STOMP (Streaming Text Oriented Messaging Protocol) pour la messagerie.

[!DNL ActiveMQ Artemis] est disponible comme alternative à RabbitMQ pour gérer les files d’attente de messages. Il est particulièrement utile lorsque vous avez besoin de fonctionnalités spécifiques à ActiveMQ ou que vous souhaitez utiliser le protocole STOMP.

>[!IMPORTANT]
>
>Si vous préférez utiliser un service Message Broker existant, comme [!DNL ActiveMQ], au lieu de compter sur Adobe Commerce sur une infrastructure cloud pour le créer, utilisez la variable d’environnement [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) pour le connecter à votre site.

{{service-instruction}}

**Pour activer ActiveMQ Artemis** :

1. Ajoutez le nom, le type et la valeur de disque requis (en Mo) au fichier `.magento/services.yaml` avec la version ActiveMQ installée.

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. Configurez les relations dans le fichier `.magento.app.yaml`.

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. Ajouter, valider et transmettre vos modifications de code.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Vérifier les relations de service](services-yaml.md#service-relationships).

{{service-change-tip}}

## Se connecter à ActiveMQ pour le débogage

À des fins de débogage, vous pouvez vous connecter directement à une instance de service de l’une des manières suivantes :

- Connexion à partir de votre environnement de développement local
- Connexion depuis l&#39;application
- Connexion depuis votre application PHP

### Connexion à partir de votre environnement de développement local

1. Connectez-vous à l’interface de ligne de commande `magento-cloud` et au projet :

   ```bash
   magento-cloud login
   ```

1. Consultez l’environnement avec ActiveMQ Artemis installé et configuré.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Utilisez SSH pour vous connecter à l’environnement cloud :

   ```bash
   magento-cloud ssh
   ```

1. Récupérez les détails de la connexion ActiveMQ et les informations d’identification de connexion à partir de la variable [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) :

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Dans la réponse, recherchez les informations ActiveMQ. Le nom de la relation dépend de la manière dont vous l’avez configurée dans votre fichier `.magento.app.yaml`. Par exemple, si vous avez utilisé `activemq-artemis` comme nom de relation :

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Activez le transfert du port local vers ActiveMQ (si votre projet est situé dans une autre région telle que US-3, EU-5 ou AP-3, remplacez ``us-3``/``eu-5``/``ap-3`` par ``us``).

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Voici un exemple d’accès à la console web ActiveMQ Artemis à l’adresse `http://localhost:8161` :

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis utilise le port 61616 pour la messagerie STOMP et le port 8161 pour la console web.

1. Lorsque la session est ouverte, vous pouvez accéder à la console web ActiveMQ Artemis à l’adresse `http://localhost:8161` à l’aide du nom d’utilisateur et du mot de passe de la variable MAGENTO_CLOUD_RELATIONSHIPS .

### Connexion depuis l&#39;application

Pour vous connecter à ActiveMQ exécuté dans une application, installez une bibliothèque cliente STOMP dans votre application PHP.

### Connexion depuis votre application PHP

Pour vous connecter à ActiveMQ à l&#39;aide de votre application PHP, ajoutez une bibliothèque PHP STOMP à votre arborescence source. Adobe Commerce utilise le protocole STOMP pour la communication ActiveMQ et la configuration est automatiquement définie lors du déploiement lorsque ActiveMQ Artemis est détecté comme service configuré.

## Prise en charge des protocoles

ActiveMQ Artemis dans Adobe Commerce sur les infrastructures cloud utilise le protocole STOMP (Streaming Text Oriented Messaging Protocol) :

- **STOMP** : protocole de messagerie utilisé pour les opérations de file d’attente (port 61616)
- **Console web** : interface de gestion accessible via HTTP (port 8161)

## Différences par rapport à RabbitMQ

Bien que [!DNL ActiveMQ Artemis] et [!DNL RabbitMQ] servent de courtiers en messages pour Adobe Commerce, il existe quelques différences :

- **Protocol** : ActiveMQ Artemis utilise le protocole STOMP, tandis que RabbitMQ utilise AMQP
- **Configuration** : lorsqu’ActiveMQ Artemis est configuré, Adobe Commerce utilise automatiquement le protocole STOMP
- **Priorité** : si ActiveMQ et RabbitMQ sont configurés, ActiveMQ est prioritaire pour les opérations STOMP, tandis que les opérations AMQP utilisent RabbitMQ
- **Console web** : ActiveMQ fournit une console de gestion web pour la surveillance des files d’attente et des messages

## Configuration de la file d’attente

Lorsqu’ActiveMQ Artemis est configuré en tant que service, Adobe Commerce configure automatiquement le système de file d’attente pour utiliser le protocole STOMP. La configuration est enregistrée dans le fichier `app/etc/env.php` lors du déploiement :

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

Si nécessaire, vous pouvez remplacer cette configuration à l’aide de la variable d’environnement [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration).

