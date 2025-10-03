---
title: Service PrivateLink
description: Découvrez comment utiliser le service PrivateLink pour établir une connexion sécurisée entre un cloud privé et la plateforme cloud d’Adobe Commerce dans la même région.
feature: Cloud, Iaas, Security
exl-id: 13a7899f-9eb5-4c84-b4c9-993c39d611cc
source-git-commit: 0e7f268de078bd9840358b66606a60b2a2225764
workflow-type: tm+mt
source-wordcount: '1616'
ht-degree: 0%

---

# Service PrivateLink

Adobe Commerce sur les infrastructures cloud prend en charge l’intégration au service [AWS PrivateLink](https://aws.amazon.com/privatelink/) ou [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/). Vous pouvez utiliser PrivateLink pour établir une communication sécurisée et privée entre Adobe Commerce sur les environnements d’infrastructure cloud avec des services et des applications hébergés sur des systèmes externes. L’application Adobe Commerce et les systèmes externes doivent être accessibles via les points d’entrée de cloud privé virtuel (VPC) configurés sur la même plateforme cloud (AWS ou Azure) dans la même région cloud.

>[!TIP]
>
>PrivateLink est idéal pour sécuriser les connexions pour les intégrations non HTTP(S), telles que les transferts de base de données ou de fichiers. Si vous prévoyez d’intégrer votre application aux API Adobe Commerce, consultez la section Création d’un [maillage API Adobe](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/) dans _maillage API pour Adobe Developer App Builder_.

## Fonctionnalités et assistance

L’intégration du service PrivateLink pour Adobe Commerce dans les projets d’infrastructure cloud d’offre les fonctionnalités et la prise en charge suivantes :

- Une connexion sécurisée entre un client Cloud privé virtuel (VPC) et Adobe VPC sur la même plateforme cloud (AWS ou Azure) dans la même région cloud.
- Prise en charge d’une communication unidirectionnelle ou bidirectionnelle entre les services de point d’entrée disponibles sur Adobe et les ordinateurs virtuels clients.
- Activation du service :

   - Ouvrez les ports requis dans l’environnement Adobe Commerce on cloud infrastructure
   - Établir la connexion initiale entre les ordinateurs virtuels du client et d’Adobe
   - Résolution des problèmes de connexion lors de l’activation

## Restrictions

- La prise en charge de PrivateLink est disponible uniquement sur les environnements de production et d’évaluation Pro. Elle n’est pas disponible dans les environnements locaux ou d’intégration, ni dans les projets de démarrage.
- Vous ne pouvez pas établir de connexions SSH à l’aide de PrivateLink. Voir [Activer les clés SSH](secure-connections.md).
- Au-delà de l’activation initiale, la prise en charge d’Adobe Commerce ne couvre pas le dépannage des problèmes AWS PrivateLink.
- Les clients sont responsables des coûts associés à la gestion de leur propre VPC.
- **Prise en charge du protocole HTTPS (port 443) par la plateforme :**
   - **Lien privé Azure** : vous ne pouvez pas utiliser le protocole HTTPS (port 443) pour vous connecter à Adobe Commerce sur une infrastructure cloud en raison du [cloaking d’origine Fastly](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html).
   - **AWS PrivateLink** : les connexions avec le protocole HTTPS (port 443) sont prises en charge.
- PrivateDNS non disponible.

## Types de connexion PrivateLink

Deux types de connexion PrivateLink sont disponibles (illustrés dans le diagramme réseau suivant) pour établir une communication sécurisée entre votre magasin et les systèmes externes hébergés en dehors de l’environnement Cloud.

![Diagramme de réseau PrivateLink](../../assets/privatelink-architecture-diagram.png)

Choisissez l’un des types de connexion PrivateLink les mieux adaptés à votre Adobe Commerce sur les environnements d’infrastructure cloud :

- **Lien privé unidirectionnel**-sélectionnez cette configuration pour récupérer en toute sécurité des données d’un magasin d’infrastructure cloud Adobe Commerce.
- **Lien privé bidirectionnel**-Sélectionnez cette configuration pour établir des connexions sécurisées vers et depuis des systèmes en dehors d’Adobe Commerce sur l’environnement d’infrastructure cloud. L&#39;option bidirectionnelle requiert deux connexions :

   - Une connexion entre le VPC client et le VPC Adobe
   - Une connexion entre le VPC Adobe et le VPC client

>[!TIP]
>
>Contactez votre administrateur réseau ou votre fournisseur de plateforme cloud pour obtenir de l’aide sur la sélection du type de connexion PrivateLink ou pour la configuration et l’administration de VPC. Consultez la documentation de Cloud Platform PrivateLink : [AWS PrivateLink](https://aws.amazon.com/privatelink/) ou [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/).

## Demander l’activation de PrivateLink

>[!WARNING]
>
>L’activation de PrivateLink peut prendre jusqu’à _cinq_ jours ouvrables. Fournir des informations incomplètes ou inexactes peut retarder le processus.

### Conditions préalables

![check](../../assets/fix.svg) Compte cloud (AWS ou Azure) dans la même région que l&#39;instance Adobe Commerce sur le cloud.

![check](../../assets/fix.svg) VPC de l’environnement client qui héberge les services à connecter via PrivateLink. Consultez la documentation d’AWS ou d’Azure pour obtenir de l’aide sur la configuration de VPC ou contactez votre administrateur réseau.

![check](../../assets/fix.svg) Pour les connexions bidirectionnelles PrivateLink, vous devez créer la configuration du service de point d’entrée pour votre application ou service, ainsi qu’un point d’entrée dans votre environnement VPC avant de demander l’activation de PrivateLink. Voir [Configuration des connexions PrivateLink bidirectionnelles](#set-up-for-bidirectional-privatelink-connections).

Collectez les données suivantes requises pour l’activation de PrivateLink :

- **Numéro de compte Customer Cloud** (AWS ou Azure) : doit se trouver dans la même région que l’instance Adobe Commerce sur l’infrastructure cloud
- **Région cloud** : indiquez la région cloud dans laquelle le compte est hébergé à des fins de vérification
- **Services et ports de communication** : Adobe doit ouvrir les ports pour permettre la communication de service entre les VPC, par exemple le port SQL 3306 et le port SFTP 2222
- **Identifiant de projet**—Fournissez l&#39;identifiant de projet Pro d&#39;Adobe Commerce on cloud infrastructure. Vous pouvez obtenir l’ID de projet et d’autres informations sur le projet à l’aide de la commande [Cloud CLI](../dev-tools/cloud-cli-overview.md) suivante : `magento-cloud project:info`
- **Type de connexion** : spécifiez le type de connexion unidirectionnel ou bidirectionnel
- **Service de point d’entrée** : pour les connexions PrivateLink bidirectionnelles, indiquez l’URL DNS du service de point d’entrée VPC auquel Adobe doit se connecter, par exemple : `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **Accès au service de point d’entrée accordé** : pour vous connecter à un service externe, autorisez l’accès au service de point d’entrée au principal de compte AWS suivant : `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >Si l’accès au service de point d’entrée n’est pas fourni, la connexion PrivateLink bidirectionnelle au service dans votre VPC n’est **pas** ajoutée, ce qui retarde la configuration.

#### Conditions préalables supplémentaires spécifiques à l’activation d’Azure Private Link

- Indiquez l’identifiant du cluster ; à l’aide de SSH, connectez-vous à l’instance distante et utilisez la commande : `cat /etc/platform_cluster`
- Pour qu’un service externe puisse se connecter à votre cluster Adobe Commerce Pro, vous devez disposer des éléments suivants :

   - Liste des ports de votre cluster Pro à exposer au nouveau point d&#39;entrée privé externe
   - Une liste des ID d’abonnement Azure pour les connexions de point d’entrée privé

- Pour connecter votre cluster Adobe Commerce Pro à un service externe, vous avez besoin des éléments suivants :

   - Liste des identifiants des ressources pour les services cibles. Les ID de service de lien privé externe ressemblent à ce qui suit :

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### Workflow d’activation

Le workflow suivant décrit le processus d’activation de l’intégration de PrivateLink à Adobe Commerce sur l’infrastructure cloud.

1. **Client** envoie un ticket d’assistance demandant l’activation de PrivateLink avec l’objet `PrivateLink support for <company>`. Incluez les [données requises pour l’activation](#prerequisites) dans le ticket. Adobe utilise le ticket d’assistance pour coordonner la communication pendant le processus d’activation.

1. **Adobe** permet au compte client d’accéder au service de point d’entrée dans Adobe VPC.

   - Mettez à jour la configuration du service de point d’entrée Adobe pour accepter les requêtes lancées à partir du compte AWS ou Azure du client.
   - Mettez à jour le ticket d’assistance pour fournir le nom du service auquel le point d’entrée Adobe VPC doit se connecter, par exemple `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`.

1. **Client** ajoute le service de point d’entrée Adobe à son compte Cloud (AWS ou Azure), ce qui déclenche une demande de connexion à Adobe. Consultez la documentation relative à la plateforme cloud pour obtenir des instructions :

   - Pour AWS, voir [Acceptation et rejet des demandes de connexion de point d’entrée de l’interface].
   - Pour Azure, voir [Gérer les demandes de connexion].

1. **Adobe** approuve la demande de connexion.

1. Après la validation de la demande de connexion, **le client** [&#x200B; vérifie la connexion](#test-vpc-endpoint-service-connection) entre son VPC et le VPC Adobe.

1. Étapes supplémentaires pour activer les connexions bidirectionnelles :

   - **Adobe** fournit l’entité de sécurité du compte Adobe (utilisateur racine du compte AWS ou Azure) et demande l’accès au service de point d’entrée VPC du client.
   - **Client** permet à Adobe d’accéder au service de point d’entrée dans le VPC client. Cela suppose que l’entité de sécurité du compte Adobe a accès à `arn:aws:iam::402592597372:root`, comme décrit précédemment dans la condition préalable **Accès au service de point d’entrée accordé**.

      - Mettez à jour la configuration du service de point d’entrée client pour accepter les requêtes lancées à partir du compte Adobe. Consultez la documentation relative à la plateforme cloud pour obtenir des instructions :

         - Pour AWS, voir [Ajout et suppression d’autorisations pour votre service de point d’entrée].
         - Pour Azure, consultez [Gérer une connexion de point d’entrée privé]

      - Indiquez à Adobe le nom du service de point d’entrée pour le VPC client.

   - **Adobe** ajoute le service de point d’entrée client au compte de plateforme Adobe (AWS ou Azure), ce qui déclenche une demande de connexion à VPC client.
   - **Client** approuve la demande de connexion d’Adobe pour terminer la configuration.
   - **Client** [vérifie la connexion](#test-vpc-endpoint-service-connection) à partir du VPC Adobe.

## Tester la connexion du service de point d’entrée VPC

Vous pouvez utiliser l’application Telnet pour tester la connexion au service de point d’entrée VPC.

**Pour tester la connexion au service de point d’entrée VPC** :

1. Dans le répertoire racine du projet, **extrayez** l’environnement d’évaluation ou de production configuré pour accéder au service de point d’entrée PrivateLink.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Exécutez la commande CURL suivante :

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   Exemple :

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   Exemple de réponse réussie :

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   Exemple de réponse d’échec :

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. Vérifiez que le service écoute sur la machine virtuelle.

   ```bash
   netstat -na | grep <port>
   ```

1. Vérifiez le flux des packages.

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   Vérifiez les paramètres internes suivants pour vous assurer que la configuration est valide :

   - Paramètres des points d’entrée et des services de point d’entrée
   - Paramètres de la répartition de charge réseau (NLB)
   - Les groupes cibles dans NLB et vérifient qu&#39;ils sont sains
   - URL du point d’entrée netcat/curl de chaque machine virtuelle (répertoriée ci-dessus)

   Consultez les articles suivants pour obtenir de l’aide sur la résolution des problèmes de connexion :

   - [AWS : dépannage des connexions du service de point d’entrée]
   - [Amazon : résolution des problèmes de connectivité de la liaison privée Azure]

   Si vous ne pouvez pas résoudre les erreurs, mettez à jour le ticket d’assistance Adobe Commerce pour demander de l’aide afin d’établir la connexion.

## Modifier la configuration de PrivateLink

[Envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) pour modifier une configuration de lien privé existante. Par exemple, vous pouvez demander des modifications comme celles-ci :

- Supprimez la connexion PrivateLink d’Adobe Commerce sur l’infrastructure cloud dans l’environnement de production ou d’évaluation Pro.
- Modifiez le numéro de compte de la plateforme cloud client pour accéder au service de point d’entrée Adobe.
- Ajoutez ou supprimez des connexions PrivateLink du VPC Adobe à d’autres services de point d’entrée disponibles dans l’environnement VPC du client.

## Configuration pour les connexions PrivateLink bidirectionnelles

Le VPC client doit disposer des ressources suivantes pour prendre en charge les connexions PrivateLink bidirectionnelles :

- Un répartiteur de charge réseau (NLB)
- Configuration du service de point d’entrée qui permet d’accéder à une application ou à un service à partir du VPC client
- Un [point d’entrée d’interface] (AWS) ou [point d’entrée privé] (Azure) qui permet à Adobe de se connecter aux services de point d’entrée hébergés dans votre VPC

Si ces ressources ne sont pas disponibles dans le VPC client, vous devez vous connecter à votre compte de plateforme cloud pour ajouter la configuration .

- Console Amazon VPC - `https://console.aws.amazon.com/vpc/`
- Portail Azure - `https://portal.azure.com`

Consultez la documentation de votre plateforme cloud pour obtenir des instructions sur la configuration de PrivateLink :

- **Documentation AWS PrivateLink**
   - [Créer une répartition de charge réseau]
   - [Créer une configuration de service de point d’entrée]
   - [Créer un point d’entrée d’interface]
   - [Cycle de vie du point d’entrée de l’interface]

- **Documentation Azure PrivateLink**
   - [Créer une répartition de charge]
   - [Workflow Azure Private Link]

<!--Link definitions-->

[Acceptation et rejet des demandes de connexion de point d’entrée de l’interface]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[Ajout et suppression d’autorisations pour votre service de point d’entrée]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon : résolution des problèmes de connectivité avec Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS : dépannage des connexions du service de point d’entrée]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Workflow Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[Création d’une répartition de charge]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[Création d’une répartition de charge réseau]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[Créer une configuration de service de point d’entrée]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[Créer un point d’entrée d’interface]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[point d’entrée de l’interface]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[Gérer une connexion de point d’entrée privé]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[Gérer les demandes de connexion]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[point d’entrée privé]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
