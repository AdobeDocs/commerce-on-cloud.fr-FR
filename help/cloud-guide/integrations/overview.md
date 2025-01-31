---
title: Présentation des intégrations
description: Découvrez les options d’intégration tierces de votre projet d’infrastructure cloud Adobe Commerce.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Présentation des intégrations

Les intégrations sont utiles pour utiliser des services externes, tels que l’hébergement Git ou les robots de Slack, et pour maintenir vos processus de développement actuels, tels que l’utilisation de la fonction de révision du code et de demande de tirage dans GitHub. Vous pouvez ajouter les intégrations suivantes à votre projet d’infrastructure cloud Adobe Commerce :

![ Intégrations ](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Pour ajouter une intégration à l’aide de l’interface de ligne de commande Cloud** :

La commande suivante lance des invites interactives pour sélectionner le type et les options de la nouvelle intégration.

```bash
magento-cloud integration:add
```

**Pour répertorier les intégrations configurées pour votre projet** :

```bash
magento-cloud integration:list
```

Exemple de réponse :

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB  Console ]

**Pour ajouter une intégration à l’aide de l’[!DNL Cloud Console]** :

1. Dans _Paramètres du projet_, cliquez sur **[!UICONTROL Integrations]**.

1. Cliquez sur un type d’intégration ou sur **[!UICONTROL Add integration]**.

1. Parcourez les étapes de sélection et de configuration du type d’intégration.

1. Après avoir ajouté l’intégration, elle apparaît dans la liste de la vue Intégrations .

>[!ENDTABS]

## Webhooks Commerce

Vous pouvez configurer les Webhooks Commerce dans votre projet cloud avec la variable globale [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks). Les webhooks Commerce envoient des requêtes à un serveur externe en réponse aux événements générés par Commerce. Le [_guide des Webhooks_](https://developer.adobe.com/commerce/extensibility/webhooks) décrit cette fonctionnalité en détail.

## Webhooks génériques

Vous pouvez capturer et générer des rapports sur les événements d’infrastructure cloud et de référentiel à l’aide d’une intégration webhook personnalisée pour `POST` les messages JSON à une URL _webhook_.

**Pour ajouter une URL webhook, utilisez la syntaxe suivante**:

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` : spécifiez le type d&#39;intégration `webhook`.
- `url` : indiquez l&#39;URL webhook qui peut recevoir les messages JSON.

L’exemple de réponse affiche une série d’invites qui permettent de personnaliser l’intégration. L’utilisation de la réponse par défaut (vide) envoie des messages sur tous les événements dans tous les environnements d’un projet.

Vous pouvez personnaliser l’intégration pour signaler des [événements](#events-to-report) spécifiques, tels que l’envoi de code vers une branche. Par exemple, vous pouvez spécifier l’événement `environment.push` pour envoyer un message lorsqu’un utilisateur envoie du code vers une branche :

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

Vous pouvez choisir de générer des rapports sur des événements dont l’état est `pending`, `in_progress` ou `complete` :

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

Vous pouvez également _inclure_ ou _exclure_ des messages pour des environnements spécifiques :

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

Une fois l’intégration terminée, vous recevez un résumé des valeurs :

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Mettre à jour l’intégration existante

Vous pouvez mettre à jour une intégration existante. Par exemple, modifiez les états de `complete` en `pending` en procédant comme suit :

```bash
magento-cloud integration:update --states=pending <int-id>
```

Exemple de réponse :

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Événements à signaler

| Événement | Description |
| ----- | :-----------|
| `environment.access.add` | Un utilisateur s’est vu accorder l’accès à l’environnement |
| `environment.access.remove` | Un utilisateur a été supprimé de l’environnement. |
| `environment.activate` | Une branche a été « activée » avec un environnement. |
| `environment.backup` | Un utilisateur a déclenché un instantané |
| `environment.branch` | Une branche a été créée à l’aide de la console de gestion |
| `environment.deactivate` | Une branche a été « désactivée ». Le code est toujours là, mais l’environnement a été détruit |
| `environment.delete` | Une branche a été supprimée |
| `environment.initialize` | Branche `master` du projet initialisée avec une première validation |
| `environment.merge` | Une branche active a été fusionnée à l’aide de la console de gestion ou de l’API |
| `environment.push` | Un utilisateur a envoyé le code vers une branche |
| `environment.restore` | Un utilisateur a restauré un instantané |
| `environment.route.create` | Un itinéraire a été créé à l’aide de la console de gestion |
| `environment.route.delete` | Un itinéraire a été supprimé à l’aide de la console de gestion |
| `environment.route.update` | Un itinéraire a été modifié à l’aide de la console de gestion |
| `environment.subscription.update` | L’environnement `master` a été redimensionné car l’abonnement a été modifié, mais il n’y a aucune modification de contenu |
| `environment.synchronize` | Les données ou le code d’un environnement ont été copiés à partir de son environnement parent |
| `environment.update.http_access` | Les règles d’accès HTTP pour un environnement ont été modifiées |
| `environment.update.restrict_robots` | La fonction de blocage de tous les robots a été activée ou désactivée |
| `environment.update.smtp` | L’envoi d’e-mails a été activé ou désactivé pour un environnement |
| `environment.variable.create` | Une variable a été créée. |
| `environment.variable.delete` | Une variable a été supprimée |
| `environment.variable.update` | Une variable a été modifiée |
| `project.domain.create` | Un domaine a été créé et ajouté au projet |
| `project.domain.delete` | Un domaine associé au projet a été supprimé |
| `project.domain.update` | Un domaine associé au projet a été mis à jour |
