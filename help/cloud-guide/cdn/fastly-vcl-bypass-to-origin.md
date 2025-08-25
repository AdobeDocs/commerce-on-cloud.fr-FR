---
title: VCL personnalisé pour contourner le cache Fastly
description: Dépannez le trafic de requête vers le serveur d’origine en créant un fragment de code VCL personnalisé pour contourner le cache Fastly.
feature: Cloud, Configuration, Cache
exl-id: 4e19d6d4-b5a1-4623-b0be-804ddc81ff3d
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# VCL personnalisé pour contourner le cache Fastly

Vous pouvez créer un fragment de code VCL personnalisé pour contourner le cache Fastly afin de résoudre les problèmes de trafic de requête vers le serveur d’origine. Par exemple, vous pouvez créer un fragment de code pour déterminer si les problèmes de site sont causés par la mise en cache ou pour résoudre les problèmes liés aux en-têtes.

Vous pouvez configurer le fragment de code pour contourner la mise en cache rapide pour les requêtes provenant d’une adresse IP ou d’une URL spécifique.

>[!NOTE]
>
>Avant de fusionner une configuration VCL personnalisée dans un environnement de production, veillez à tester le code dans l’environnement d’évaluation.

**Conditions préalables :**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Pour contourner le cache Fastly en fonction de l’adresse IP ou de l’URL** :

{{admin-login-step}}

1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Système**.

1. Développez **Cache de page complet** > **Configuration rapide** > **Fragments de code VCL personnalisés**.

1. Cliquez sur **Créer un fragment de code personnalisé**.

1. Ajoutez les valeurs de fragment de code VCL :

   - **Nom** — `bypass_fastly`

   - **Type** — `recv`

   - **Priorité** — `5`

   - **VCL** contenu de fragment de code —

     L’exemple suivant contourne Fastly pour une adresse IP spécifique :

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     L’exemple suivant contourne Fastly pour un modèle d’URL spécifique :

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     Pour une correspondance d’URL exacte, utilisez l’opérateur `==` au lieu de l’opérateur `~`. Voir la [référence Fastly VCL] pour plus d’informations.

1. Cliquez sur **Créer**.

   ![Créer un fragment de code VCL de contournement Fastly](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. Une fois la page rechargée, cliquez sur **Télécharger VCL vers Fastly** dans la section *Configuration Fastly*.

1. Une fois le chargement terminé, actualisez le cache en fonction de la notification en haut de la page.

   Valide rapidement la version VCL mise à jour pendant le processus de chargement. Si la validation échoue, modifiez votre fragment de code VCL personnalisé pour résoudre les problèmes. Ensuite, chargez à nouveau le VCL.

Après avoir ajouté le fragment de code VCL, vous pouvez utiliser des commandes cURL pour envoyer des requêtes au serveur d’origine à partir de l’adresse IP ou de l’URL spécifiée, comme indiqué dans l’exemple suivant :

```bash
curl -svo /dev/null www.example.com/index.html
```

Ensuite, examinez la réponse pour résoudre les problèmes liés au contenu non mis en cache.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Référence Fastly VCL]: https://docs.fastly.com/vcl/

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
