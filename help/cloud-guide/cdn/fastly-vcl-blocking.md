---
title: VCL personnalisé pour les requêtes de blocage
description: Bloquez les requêtes entrantes par adresse IP à l’aide d’une liste de contrôle d’accès (ACL) Edge avec un fragment de code VCL personnalisé.
feature: Cloud, Configuration, Security
exl-id: eb21c166-21ae-4404-85d9-c3a26137f82c
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# VCL personnalisé pour les requêtes de blocage

Vous pouvez utiliser le module de réseau CDN Fastly pour Magento 2 pour créer une liste de contrôle d’accès Edge avec une liste d’adresses IP que vous souhaitez bloquer. Vous pouvez ensuite utiliser cette liste avec un extrait de code VCL pour bloquer les requêtes entrantes. Le code vérifie l’adresse IP de la requête entrante. S’il correspond à une adresse IP incluse dans la liste ACL, Fastly bloque l’accès à votre site à la requête et renvoie une `403 Forbidden error`. Toutes les autres adresses IP client sont autorisées à y accéder.

**Conditions préalables :**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Liste des adresses IP client à bloquer

## Créer une liste de contrôle d’accès Edge pour bloquer les adresses IP des clients

Vous créez une liste de contrôle d’accès Edge pour définir la liste des adresses IP à bloquer. Après avoir créé la liste de contrôle d’accès, vous pouvez l’utiliser dans un fragment de code VCL personnalisé pour gérer l’accès à votre site d’évaluation ou de production.

Gérez l’accès pour les sites d’évaluation et de production en créant la liste de contrôle d’accès Edge portant le même nom dans les deux environnements. Le code de fragment de code VCL s’applique aux deux environnements.

1. Connectez-vous à l’administrateur.
1. Accédez à **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système** > **Cache de page complet** > **Fastly Configuration**.
1. Développez la section **ACL Edge** .
1. Cliquez sur **Ajouter une liste de contrôle d’accès** pour créer une liste. Placer sur la liste bloquée Pour cet exemple, nommez la liste « ».
1. Saisissez les valeurs des adresses IP dans la liste. Toutes les adresses IP client ajoutées à cette liste sont bloquées et ne peuvent pas accéder au site.
1. Si nécessaire, cochez la case **Annulé**.

Vous référencez la liste de contrôle d’accès Edge par son nom dans votre code de fragment de code VCL.

## Créer le VCL personnalisé pour la place sur la liste bloquée

>[!NOTE]
>
>Cet exemple montre aux utilisateurs expérimentés comment créer un fragment de code VCL pour configurer des règles de blocage personnalisées à charger vers le service Fastly. Vous pouvez configurer une liste bloquée ou une liste autorisée en fonction du pays à partir de l’Administration d’Adobe Commerce à l’aide de la fonctionnalité [Blocage](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) disponible dans le module Fastly CDN pour Magento 2 .

Après avoir défini la liste de contrôle d’accès Edge, vous pouvez l’utiliser pour créer le fragment de code VCL afin de bloquer l’accès aux adresses IP spécifiées dans la liste de contrôle d’accès. Vous pouvez utiliser le même fragment de code VCL dans les environnements d’évaluation et de production, mais vous devez charger le fragment de code dans chaque environnement séparément.

Le code de fragment de code VCL personnalisé suivant (format JSON) illustre la logique de blocage des requêtes entrantes avec une adresse IP client correspondant à une adresse dans la liste de contrôle d’accès de la liste bloquée.

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

Avant de créer un fragment de code basé sur cet exemple, passez en revue les valeurs pour déterminer si vous devez apporter des modifications :

- `name` : nom du fragment de code VCL. Pour cet exemple, nous avons utilisé le nom `blocklist`.

- `priority` : détermine à quel moment le fragment de code VCL s&#39;exécute. La priorité est `5` pour s’exécuter immédiatement et vérifier si une requête d’administration provient d’une adresse IP autorisée. Le fragment de code s’exécute avant que l’un des fragments de code VCL Magento par défaut (`magentomodule_*`) se voie attribuer une priorité de 50. Définissez une priorité supérieure ou inférieure à 50 pour chaque fragment de code personnalisé en fonction du moment où vous souhaitez que votre fragment de code s’exécute. Les fragments de code dont le numéro de priorité est inférieur s’exécutent en premier.

- `type` : spécifie le type de fragment de code VCL qui détermine l&#39;emplacement du fragment de code dans le code VCL généré. Dans cet exemple, nous utilisons `recv`, qui insère le code VCL dans le sous-programme `vcl_recv`, sous le code VCL standard et au-dessus de tous les objets. Pour obtenir la liste des types de fragment de code, reportez-vous à la [Référence de fragment de code VCL Fastly](https://docs.fastly.com/api/config#api-section-snippet).

- `content` : fragment de code VCL à exécuter, qui vérifie l’adresse IP du client. Si l’adresse IP se trouve dans la liste de contrôle d’accès d’Edge, elle est bloquée avec une erreur de `403 Forbidden` pour l’ensemble du site web. Toutes les autres adresses IP client sont autorisées à y accéder.

Après avoir examiné et mis à jour le code de votre environnement, utilisez l’une des méthodes suivantes pour ajouter le fragment de code VCL personnalisé à votre configuration de service Fastly :

- [Ajoutez le fragment de code VCL personnalisé depuis Admin](#add-the-custom-vcl-snippet). Cette méthode est recommandée si vous pouvez accéder à Admin. (Nécessite [Fastly version 1.2.58](fastly-configuration.md#upgrade-fastly-module) ou ultérieure.)

- Enregistrez l’exemple de code JSON dans un fichier (par exemple, `blocklist.json`) et [téléchargez-le à l’aide de l’API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilisez cette méthode si vous ne pouvez pas accéder à l’administrateur.

## Ajouter le fragment de code VCL personnalisé

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système**.

1. Développez **Cache de page complet** > **Configuration rapide** > **Fragments de code VCL personnalisés**.

1. Cliquez sur **Créer un fragment de code personnalisé**.

1. Ajoutez les valeurs de fragment de code VCL :

   - **Nom** — `blocklist`

   - **Type** — `recv`

   - **Priorité** — `5`

   - Ajoutez le contenu du fragment de code **VCL** :

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. Cliquez sur **Créer** pour générer le fichier de fragment de code VCL avec le modèle de nom `type_priority_name.vcl`, par exemple `recv_5_blocklist.vcl`

1. Une fois la page rechargée, cliquez sur **Télécharger VCL vers Fastly** dans la section *Configuration Fastly* pour ajouter le fichier à la configuration de service Fastly.

1. Après les chargements, actualisez le cache en fonction de la notification en haut de la page.

Valide rapidement la version mise à jour du code VCL pendant le processus de chargement. Si la validation échoue, modifiez le fragment de code VCL personnalisé pour résoudre le problème. Ensuite, chargez à nouveau le VCL.

## Exemples VCL supplémentaires pour le blocage de requêtes

Les exemples suivants montrent comment bloquer des requêtes à l’aide d’instructions de condition intégrées au lieu d’une liste ACL.

>[!WARNING]
>
>Dans ces exemples, le code VCL est formaté en tant que payload JSON qui peut être enregistrée dans un fichier et envoyée dans une requête API Fastly. Vous pouvez envoyer le fragment de code [VCL à partir de l’Admin](#add-the-custom-vcl-snippet) ou sous la forme d’une chaîne JSON à l’aide de l’API Fastly. Pour éviter les erreurs de validation lorsque vous utilisez l’API Fastly avec une chaîne JSON, vous devez utiliser une barre oblique inverse pour échapper les caractères spéciaux.

>[!NOTE]
>Si vous soumettez le fragment de code VCL à partir de l&#39;administrateur, extrayez les valeurs individuelles de l&#39;exemple de code VCL et saisissez-les dans les champs correspondants. Par exemple :
>- Nom : `<name of the VCL>`
>- Dynamique : `<0/1>`
>- Type : `<type>`
>- Priorité : `<priority>`
>- Contenu : `<content>`

Voir [Utilisation de fragments de code VCL dynamiques](https://docs.fastly.com/vcl/vcl-snippets/) dans la documentation Fastly VCL.

### Exemple de code VCL : bloquer par code de pays

Cet exemple utilise le code de pays ISO 3166-1 à deux caractères pour le pays associé à l’adresse IP.

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>Au lieu d’utiliser un fragment de code VCL personnalisé, vous pouvez utiliser la fonction Fastly [Blocage](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) d’Adobe Commerce dans l’administration de l’infrastructure cloud pour configurer le blocage par code de pays ou par liste de codes de pays.

### Exemple de code VCL : Bloquer par en-tête de requête HTTP User-Agent

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
