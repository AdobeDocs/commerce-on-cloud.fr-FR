---
title: Bloquer le spam de référence
description: Bloquez le spam de référence de votre site à l’aide du dictionnaire Fastly Edge et d’un fragment de code VCL personnalisé.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Bloquer le spam de référence

L’exemple suivant montre comment configurer [Fastly Edge Dictionary](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) avec un fragment de code VCL personnalisé pour bloquer le spam de référence provenant de votre Adobe Commerce sur le site d’infrastructure cloud.

>[!NOTE]
>
>Nous vous recommandons d’ajouter des configurations VCL personnalisées à un environnement d’évaluation où vous pouvez les tester avant de les exécuter dans l’environnement de production.

**Conditions préalables :**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Vérifiez les journaux de votre site à la recherche de fausses URL de référence et dressez une liste de domaines à bloquer.

## Création d’une liste bloquée référent

Les dictionnaires Edge créent des paires clé-valeur accessibles aux fonctions VCL pendant le traitement des fragments de code VCL. Dans cet exemple, vous allez créer un dictionnaire Edge qui fournit la liste des sites web référents à bloquer.

{{admin-login-step}}

1. Cliquez sur **Magasins** > **Paramètres** > **Configuration** > **Avancé** > **Système**.

1. Développez **Cache de page complet** > **Configuration rapide** > **Dictionnaires Edge**.

1. Créez le conteneur Dictionnaire :

   - Cliquez sur **Ajouter un conteneur**.

   - Sur la page *Conteneur*, saisissez un **Nom du dictionnaire**—`referrer_blocklist`.

   - Sélectionnez **Activer après la modification** pour déployer vos modifications sur la version de la configuration de service Fastly que vous modifiez.

   - Cliquez sur **Télécharger** pour joindre le dictionnaire à votre configuration de service Fastly.

1. Ajoutez la liste des noms de domaine à bloquer dans le dictionnaire `referrer_blocklist` :

   - Cliquez sur l’icône Paramètres du dictionnaire de `referrer_blocklist`.

   - Ajoutez et enregistrez les paires clé-valeur dans le nouveau dictionnaire. Pour cet exemple, chaque **Clé** est le nom de domaine d’une URL référente à bloquer et **Valeur** est `true`.

     ![Ajouter des éléments du dictionnaire de référents incorrects](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - Cliquez sur **Annuler** pour revenir à la page de configuration du système.

1. Cliquez sur **Enregistrer la configuration**.

1. Actualisez le cache en fonction de la notification envoyée en haut de la page.

Pour plus d’informations sur les dictionnaires Edge, voir [Création et utilisation de dictionnaires Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) et [fragments de code VCL personnalisés](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) dans la documentation Fastly.

## Créer un fragment de code VCL personnalisé pour bloquer le spam des référents

Le code de fragment de code VCL personnalisé suivant (format JSON) montre la logique pour vérifier et bloquer les requêtes. Le fragment de code VCL capture l’hôte d’un site web référent dans un en-tête, puis compare le nom d’hôte à la liste des URL du dictionnaire de `referrer_blocklist`. Si le nom d’hôte correspond, la requête est bloquée avec une erreur `403 Forbidden`.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

Avant de créer un fragment de code basé sur cet exemple, passez en revue les valeurs pour déterminer si vous devez apporter des modifications :

- `name` — Nom du fragment de code VCL. Pour cet exemple, nous avons utilisé `block_bad_referrer`.

- `dynamic` — La valeur 0 indique un [fragment de code normal](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) à charger vers le VCL avec version pour la configuration Fastly.

- `priority` — Détermine quand le fragment de code VCL s&#39;exécute. La priorité est `5` pour exécuter ce code de fragment de code avant que l’un des fragments de code VCL de Magento par défaut (`magentomodule_*`) ne se voie attribuer une priorité de 50. Définissez une priorité supérieure ou inférieure à 50 pour chaque fragment de code personnalisé en fonction du moment où vous souhaitez que votre fragment de code s’exécute. Les fragments de code dont le numéro de priorité est inférieur s’exécutent en premier.

- `type` — Spécifie un emplacement pour insérer le fragment de code dans la version VCL. Dans cet exemple, le fragment de code VCL est un fragment de code `recv`. Lorsque le fragment de code est inséré dans la version VCL, il est ajouté à la sous-routine `vcl_recv`, sous le code VCL Fastly par défaut et au-dessus de tous les objets.

- `content` — Fragment de code VCL à exécuter sur une ligne, sans sauts de ligne.

Après avoir examiné et mis à jour le code de votre environnement, utilisez l’une des méthodes suivantes pour ajouter le fragment de code VCL personnalisé à votre configuration de service Fastly :

- [Ajoutez le fragment de code VCL personnalisé depuis Admin](#add-the-custom-vcl-snippet). Cette méthode est recommandée si vous pouvez accéder à Admin. (Nécessite [Fastly version 1.2.58](fastly-configuration.md#upgrade) ou ultérieure.)

- Enregistrez l’exemple de code JSON dans un fichier (par exemple, `allowlist.json`) et [téléchargez-le à l’aide de l’API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilisez cette méthode si vous ne pouvez pas accéder à l’administrateur.

## Ajouter le fragment de code VCL personnalisé

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système**.

1. Développez **Cache de page complet** > **Configuration rapide** > **Fragments de code VCL personnalisés**.

1. Cliquez sur **Créer un fragment de code personnalisé**.

1. Ajoutez les valeurs de fragment de code VCL :

   - **Nom** — `block_bad_referrer`

   - **Type** — `recv`

   - **Priorité** — `5`

   - **VCL** contenu de fragment de code —

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. Cliquez sur **Créer**.

   ![Créer un fragment de code VCL de bloc référent personnalisé](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. Une fois la page rechargée, cliquez sur **Télécharger VCL vers Fastly** dans la section *Configuration Fastly*.

1. Une fois le chargement terminé, actualisez le cache en fonction de la notification en haut de la page.

Valide rapidement la version VCL mise à jour pendant le processus de chargement. Si la validation échoue, modifiez votre fragment de code VCL personnalisé pour résoudre les problèmes. Ensuite, chargez à nouveau le VCL.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
