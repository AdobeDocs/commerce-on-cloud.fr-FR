---
title: Prise en main des fragments de code VCL personnalisés
description: Découvrez comment utiliser des fragments de code du langage de contrôle Varnish pour personnaliser la configuration du service Fastly pour Adobe Commerce.
feature: Cloud, Configuration, Services
exl-id: 90f0bea6-4365-4657-94e9-92a0fd1145fd
source-git-commit: a51946f65ccd606cde6fbb4278f625a49ae42dad
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 0%

---

# Prise en main du VCL personnalisé

Fastly prend en charge une version personnalisée du langage de configuration de vernis (VCL) pour adapter la configuration de service Fastly à vos besoins.

Les fragments de code VCL personnalisés sont des blocs de logique VCL ajoutés à la version VCL active chargée sur votre site Adobe Commerce. Un extrait de code VCL personnalisé modifie la manière dont les services de mise en cache rapide répondent au trafic de requêtes. Par exemple, vous pouvez ajouter un fragment de code VCL personnalisé pour autoriser le trafic de requête uniquement à partir d’adresses IP client spécifiées. Vous pouvez également créer un fragment de code pour bloquer le trafic provenant de sites web connus pour envoyer du spam de référence à vos sites Adobe Commerce.

Les fragments de code VCL personnalisés, générés, compilés et transmis à tous les caches Fastly, se chargent et s&#39;activent sans temps d&#39;arrêt du serveur.

>[!NOTE]
>
>Avant d’ajouter du code VCL personnalisé, des dictionnaires Edge et des listes de contrôle d’accès à votre configuration de module Fastly, vérifiez que le service de mise en cache Fastly fonctionne avec la configuration par défaut. Voir [Configuration des services Fastly](fastly-configuration.md).

Fastly prend en charge deux types de fragments de code VCL personnalisés :

- [Fragments de code standard](https://docs.fastly.com/en/guides/about-vcl-snippets) : les fragments de code VCL standard personnalisés sont codés pour des versions VCL spécifiques. Vous pouvez créer, modifier et déployer des fragments de code VCL standard à partir de l’API Admin ou Fastly.

- [Extraits dynamiques](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) : extraits VCL créés à l’aide de l’API Fastly. Vous pouvez modifier et déployer des fragments de code dynamiques sans avoir à mettre à jour la version Fastly VCL pour votre service.

Nous vous recommandons d’utiliser des fragments de code VCL personnalisés avec les dictionnaires Edge et les listes de contrôle d’accès (ACL) pour stocker les données utilisées dans votre code personnalisé.

- [**Dictionnaire Edge**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries)—Stocke les données sous forme de paires clé-valeur dans un conteneur de dictionnaire qui peut être référencé à partir de fragments de code VCL personnalisés

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) : stocke les données d’adresse IP du client qui définissent la liste de contrôle d’accès pour les règles de blocage ou d’autorisation implémentées à l’aide de fragments de code VCL personnalisés

Le dictionnaire et les données ACL sont déployés sur les nœuds Fastly Edge accessibles sur plusieurs régions du réseau. En outre, les données peuvent être mises à jour dynamiquement sur le réseau sans que vous ayez à redéployer le code VCL pour votre environnement d’évaluation ou de production.

>[!NOTE]
>
>Vous ne pouvez ajouter des fragments de code VCL personnalisés à un environnement d’évaluation ou de production que si vous avez [configuré les services Fastly](fastly-configuration.md) pour cet environnement.

## Tutoriel

Ce tutoriel et les exemples montrent l’utilisation de fragments de code VCL personnalisés standard avec les dictionnaires Edge et les listes de contrôle d’accès Edge pour personnaliser la configuration de service Fastly pour Adobe Commerce. Pour plus d’informations, voir la documentation Fastly :

- [Guide de Fastly VCL](https://docs.fastly.com/guides/vcl/guide-to-vcl) : Informations sur l’implémentation de Fastly Varnish, les extensions de Fastly VCL et les ressources permettant d’en savoir plus sur le vernis et le VCL.
- [Référence Fastly VCL](https://docs.fastly.com/guides/vcl/) : référence de programmation détaillée pour développer et dépanner les fragments de code VCL et VCL personnalisés Fastly.

Vous pouvez créer et gérer des fragments de code VCL personnalisés à partir de l’administration Adobe Commerce ou à l’aide de l’API Fastly :

- [Administrateur Adobe Commerce ](#manage-custom-vcl-from-admin)—Nous vous recommandons d’utiliser l’administrateur Adobe Commerce pour gérer les fragments de code VCL personnalisés, car cela automatise le processus de validation, de chargement et d’application des modifications VCL à la configuration de service Fastly. Vous pouvez également afficher et modifier les fragments de code VCL personnalisés ajoutés à la configuration du service Fastly à partir de l’administrateur.

- [API Fastly](#manage-vcl-using-the-api) : si vous ne pouvez pas accéder à l’administration, utilisez l’API Fastly pour gérer les fragments de code VCL personnalisés. Par exemple, utilisez l’API pour résoudre les problèmes de configuration du service Fastly lorsque le site est hors service ou pour ajouter un fragment de code VCL personnalisé. En outre, certaines opérations ne peuvent être effectuées qu’à l’aide de l’API . Par exemple, vous devez utiliser l&#39;API pour réactiver une ancienne version de VCL ou pour afficher tous les fragments de code VCL inclus dans une version de VCL spécifiée. Voir [Référence rapide de l’API pour les fragments de code VCL](#api-quick-reference-for-vcl-snippets).

### Exemple de code de fragment de code VCL

L’exemple suivant illustre le fragment de code VCL personnalisé (format JSON) qui filtre le trafic par adresse IP client :

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>Dans cet exemple, le code VCL est formaté en tant que payload JSON qui peut être enregistrée dans un fichier et envoyée dans une requête API Fastly. Pour éviter les erreurs de validation JSON lors de l’envoi du fragment de code en tant que JSON pour une requête API, utilisez une barre oblique inverse pour échapper les caractères spéciaux dans le code. Voir [Utilisation de fragments de code VCL dynamiques](https://docs.fastly.com/vcl/vcl-snippets/) dans la documentation Fastly VCL. Si vous envoyez le fragment de code VCL à partir de l’administrateur, vous n’avez pas besoin d’échapper les caractères spéciaux.

La logique VCL du champ `content` effectue les actions suivantes :

- Vérifie l’adresse IP entrante, `client.ip` à chaque requête

- Bloque toute requête avec une adresse IP incluse dans la liste de contrôle d’accès Edge *ACLNAME*, renvoyant une erreur `403 Forbidden`

Le tableau suivant fournit des détails sur les données clés pour les fragments de code VCL personnalisés. Pour une référence plus détaillée, reportez-vous à la référence [Fragments de code VCL](https://docs.fastly.com/api/config#api-section-snippet) dans la documentation Fastly .

| Valeur | Description |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | La clé API pour accéder à votre compte Fastly. Voir [Obtenir des informations d’identification](fastly-configuration.md). |
| `active` | Statut actif du fragment de code ou de la version. Renvoie `true` ou `false`. Si la valeur est true, le fragment de code ou la version est en cours d’utilisation. Clonez un fragment de code actif à l’aide de son numéro de version. |
| `content` | Fragment de code VCL à exécuter. Fastly ne prend pas en charge toutes les fonctionnalités de langage VCL. En outre, Fastly fournit des extensions avec des fonctionnalités personnalisées. Pour plus d’informations sur les fonctionnalités prises en charge, consultez la [référence de programmation Fastly VCL](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | Statut dynamique d’un fragment de code. Renvoie `false` pour les [fragments de code standard](https://docs.fastly.com/en/guides/about-vcl-snippets) inclus dans le VCL avec version pour la configuration de service Fastly. Renvoie `true` pour un [fragment de code dynamique](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) qui peut être modifié et déployé sans nécessiter une nouvelle version de VCL. |
| `number` | Numéro de version VCL où le fragment de code est inclus. Fastly utilise *Version modifiable #* dans ses exemples de valeurs. Si vous ajoutez des fragments de code personnalisés à partir de l’API, incluez le numéro de version dans la requête API. Si vous ajoutez un VCL personnalisé à partir de l’administrateur, la version vous est fournie. |
| `priority` | Valeur numérique de `1` à `100` qui spécifie le moment d&#39;exécution du code de fragment de code VCL personnalisé. Les fragments de code dont les valeurs de priorité sont inférieures s’exécutent en premier. Si elle n’est pas spécifiée, la valeur `priority` est définie par défaut sur `100`.<p>Tout fragment de code VCL personnalisé avec une valeur de priorité de `5` s&#39;exécute immédiatement, ce qui est préférable pour le code VCL qui implémente le routage de requête (blocage et listes autorisées et redirections). La priorité `100` est préférable pour remplacer le code de fragment de code VCL par défaut.<p>Tous les [fragments de code VCL par défaut](fastly-configuration.md#upload-vcl-snippets) inclus dans le module Magento-Fastly ont `priority=50`.<ul><li>Attribuez une priorité élevée comme `100` pour exécuter le code VCL personnalisé après toutes les autres fonctions VCL et remplacer le code VCL par défaut.</li></ul> |
| `service_id` | Identifiant de service Fastly pour un environnement d’évaluation ou de production spécifique. Cet identifiant est attribué lorsque votre projet est ajouté à l’infrastructure cloud d’Adobe Commerce [compte de service Fastly](fastly.md#fastly-service-account-and-credentials). |
| `type` | Indique l’emplacement d’insertion du fragment de code généré, tel que `init` (au-dessus des sous-routines) et `recv` (dans les sous-routines). Pour plus d’informations, consultez la référence Fastly [fragments de code VCL](https://docs.fastly.com/api/config#api-section-snippet). |

## Gérer le VCL personnalisé à partir de l’administrateur

Vous pouvez [ajouter des fragments de code VCL personnalisés](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) à partir de la section *Configuration Fastly* > *Fragments de code VCL personnalisés* dans Admin.

![Gestion des fragments de code VCL personnalisés](../../assets/cdn/fastly-edit-snippets.png)

La vue *Fragments de code VCL personnalisés* affiche uniquement les fragments de code qui ont été ajoutés par l’intermédiaire de l’administrateur. Si des fragments de code sont ajoutés à l’aide de l’API Fastly, utilisez l’API pour [les gérer](#manage-vcl-using-the-api).

Les exemples suivants montrent comment créer et gérer des fragments de code VCL personnalisés à partir de l’administration, et utiliser les modules Fastly Edge et les dictionnaires Edge :

- [Réacheminer les requêtes vers un serveur principal CMS](fastly-vcl-wordpress.md)
- [Bloquer le spam de référence](fastly-vcl-badreferer.md)
- [Bloquer le spam de référence](fastly-vcl-badreferer.md)
- [VCL personnalisé pour la liste autorisée IP](fastly-vcl-allowlist.md)
- [VCL personnalisé pour la liste bloquée IP](fastly-vcl-blocking.md)
- [Contournement du cache Fastly](fastly-vcl-bypass-to-origin.md)

## Fragments de code ne pouvant pas être affichés/modifiés dans l’administration Commerce

Vous ne pouvez pas afficher ni modifier certains fragments de code directement dans Commerce Admin. Par exemple, [ extraits dynamiques ](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets). Dans la section Fragments de code VCL personnalisés , vous ne verrez pas les fragments de code qui ont été ajoutés directement par l’équipe de support cloud au tableau de bord de gestion [Fastly](fastly.md#fastly-service-account-and-credentials).


**Pour observer les fragments de code ajoutés par l’équipe d’assistance cloud, procédez comme suit**

1. Accédez à la section **Outils**.

1. Cliquez sur **Répertorier toutes les versions** en regard de _Historique des versions_.

1. Cliquez sur l’icône représentant un œil en regard de la version de VCL applicable pour afficher les fragments de code existants.


## Gérer le VCL à l’aide de l’API

La présentation suivante vous explique comment créer des fichiers de fragment de code VCL standard et les ajouter à votre configuration de service Fastly à l’aide de l’API Fastly. Vous pouvez créer et gérer les fragments de code à partir de l’application *terminal*. Vous n’avez pas besoin d’une connexion SSH dans un environnement spécifique.

**Conditions préalables :**

- Configurez votre environnement Adobe Commerce sur l’infrastructure cloud pour les services Fastly . Voir [ Configuration rapide ](fastly-configuration.md).

- [Obtenez les informations d’identification de l’API Fastly](fastly-configuration.md) pour authentifier les requêtes auprès de l’API Fastly. Assurez-vous d’obtenir les informations d’identification pour l’environnement approprié : évaluation ou production.

- Enregistrez les informations d’identification de service Fastly en tant que variables d’environnement Bash que vous pouvez utiliser dans les commandes cURL :

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  Les variables d’environnement exportées ne sont disponibles que dans la session bash en cours et sont perdues lorsque vous fermez le terminal. Vous pouvez redéfinir les variables en exportant une nouvelle valeur. Pour afficher la liste des variables exportées liées à Fastly :

  ```bash
  export | grep FASTLY
  ```

## Ajouter des fragments de code VCL

Ce tutoriel décrit les étapes de base pour ajouter des fragments de code personnalisés à l’aide de l’API Fastly .

>[!NOTE]
>
>Pour savoir comment gérer les fragments de code VCL personnalisés à partir de l’administration Adobe Commerce, consultez [Gérer le code VCL à partir de l’administration Adobe Commerce](#manage-custom-vcl-from-admin).


**Conditions préalables**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### Étape 1 : Rechercher la version VCL active

Utilisez l’opération Fastly API [get version](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) pour obtenir le numéro de version VCL actif :

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

Dans la réponse JSON, notez le numéro de version VCL actif renvoyé dans la clé `number`, par exemple `"number": 99`. Vous avez besoin du numéro de version lorsque vous clonez le VCL pour le modifier.

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Enregistrez le numéro de version actif dans une variable d’environnement Bash pour l’utiliser dans les requêtes API suivantes :

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### Étape 2 : clonez la version VCL active et tous les fragments de code

Avant de pouvoir ajouter ou modifier des fragments de code VCL personnalisés, vous devez créer une copie de la version VCL active pour la modifier. Utilisez l’opération Fastly API [clone](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) :

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

Dans la réponse JSON, le numéro de version est incrémenté et la valeur de clé *active* est `false`. Vous pouvez modifier localement la nouvelle version inactive de VCL.

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Enregistrez le nouveau numéro de version dans une variable d’environnement Bash à utiliser dans les commandes suivantes :

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### Étape 3 : créer un fragment de code VCL personnalisé

Créez et enregistrez votre code VCL personnalisé dans un fichier JSON avec le contenu et le format suivants :

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

Les valeurs sont les suivantes :

- `name` : nom du fragment de code VCL.

- `dynamic` : indique s&#39;il s&#39;agit d&#39;un [extrait de code normal](https://docs.fastly.com/en/guides/about-vcl-snippets) ou d&#39;un [extrait de code dynamique](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets).

- `type` : spécifie l&#39;emplacement d&#39;insertion du fragment de code généré, tel que `init` (au-dessus des sous-routines) et `recv` (dans les sous-routines). Voir [Valeurs d’objet de fragment de code VCL Fastly](https://docs.fastly.com/api/config#snippet) pour plus d’informations sur ces valeurs.

- `priority` : valeur comprise entre `1` et `100` qui détermine le moment d&#39;exécution du code de fragment de code VCL personnalisé. Les fragments de code VCL personnalisés avec des valeurs inférieures s’exécutent en premier.

  Tout le code VCL par défaut du module Fastly VCL a une `priority` de `50`. Si vous voulez qu&#39;une action se produise en dernier ou pour remplacer le code VCL par défaut, utilisez un numéro supérieur, tel que `100`. Pour exécuter immédiatement le code de fragment de code VCL personnalisé, définissez la priorité sur une valeur inférieure, telle que `5`.

- `content` : fragment de code VCL à exécuter sur une ligne, sans sauts de ligne. Voir [Exemple de fragment de code VCL personnalisé](#example-vcl-snippet-code).

### Étape 4 : Ajouter le fragment de code VCL à la configuration Fastly

Utilisez l’opération Fastly API [create snippet](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) pour ajouter le fragment de code VCL personnalisé à la version VCL.

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

Le `<filename.json>` correspond au nom du fichier que vous avez préparé à l’étape précédente. Répétez cette commande pour chaque extrait de code VCL.

Si vous recevez une réponse `500 Internal Server Error` du service Fastly , vérifiez la syntaxe du fichier JSON pour vous assurer que vous téléchargez un fichier valide.

### Étape 5 : valider et activer les fragments de code VCL personnalisés

Après avoir ajouté un fragment de code VCL personnalisé, Fastly insère le fragment de code dans la version VCL que vous modifiez. Pour appliquer les modifications, effectuez les étapes suivantes pour valider le code de fragment de code VCL et activer la version VCL.

1. Utilisez l’opération Fastly API [validate VCL version](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) pour vérifier le code VCL mis à jour.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Si l’API Fastly renvoie une erreur, corrigez le problème et validez à nouveau la version VCL mise à jour.

1. Utilisez l’opération Fastly API [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) pour activer la nouvelle version de VCL.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```


## Référence rapide d’API pour les fragments de code VCL

Ces exemples de requête API utilisent des variables d’environnement exportées pour fournir les informations d’identification permettant de s’authentifier avec Fastly. Pour plus d’informations sur ces commandes, voir [Référence de l’API Fastly](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>Utilisez ces commandes pour gérer les fragments de code que vous avez ajoutés à l’aide de l’API Fastly . Si vous avez ajouté des fragments de code depuis Admin, voir [Gérer les fragments de code VCL à l’aide de Admin](#manage-vcl-using-the-api).

- **Obtenir le numéro de version VCL actif**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **Liste de tous les fragments de code VCL standard associés à un service**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **Vérifier un fragment de code individuel**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  Le `<snippet_name>` est le nom d’un fragment de code, tel que `my_regular_snippet`.

- **Mettre à jour un fragment de code**

  Modifiez le [fichier JSON préparé](#step-3-create-a-custom-vcl-snippet) et envoyez la requête suivante :

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **Supprimer un fragment de code VCL individuel**

  Obtenez une liste de fragments de code et utilisez la commande `curl` suivante avec le nom de fragment de code spécifique à supprimer :

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **Remplacer les valeurs dans le [code VCL Fastly par défaut](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  Créez un fragment de code avec des valeurs mises à jour et attribuez-lui une priorité de `100`.
