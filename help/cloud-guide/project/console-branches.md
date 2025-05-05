---
title: Gérez les branches avec  [!DNL Cloud Console]
description: Découvrez comment gérer les branches d’environnement pour Adobe Commerce sur les infrastructures cloud à l’aide de l’ [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Gérer les branches avec le [!DNL Cloud Console]

Vous pouvez gérer vos environnements à l’aide de l’interface de ligne de commande [!DNL Cloud Console] ou `magento-cloud`. Les fichiers de votre projet sont stockés dans un référentiel Git. Vous pouvez utiliser des commandes Git pour gérer votre code, mais l’interface de ligne de commande `magento-cloud` est conçue pour interagir avec les fonctionnalités de Platform, contrairement aux commandes Git. Voir [Commandes Git](../dev-tools/cloud-cli-overview.md#git-commands) dans la rubrique relative à l’interface de ligne de commande cloud.

Cette rubrique explique comment utiliser le [!DNL Cloud Console] pour :

- Ajout ou suppression d’un environnement
- Synchroniser (`git pull`) avec l’environnement parent
- Fusionner (`git push`) avec l’environnement parent

>[!TIP]
>
>Vous ne pouvez pas créer de branches à partir d’environnements d’évaluation et de production Pro. Vous pouvez créer une branche à partir de la branche `master` .

## Création d’un environnement

La stratégie d’embranchement utilise un workflow Git courant dans lequel vous développez du code et ajoutez des extensions dans une branche de développement. Consultez les présentations de l’architecture [Starter](../architecture/starter-architecture.md) et [Pro](../architecture/starter-develop-deploy-workflow.md).

- Pour Commencer, créez une branche `staging` à partir de la branche `master`, puis une branche à partir de `staging` pour le développement.
- Pour Pro, créez une branche de développement à partir de l’environnement `Integration`.

Votre compte prend en charge un nombre limité de branches de développement ![branche active](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (inactive). Gérez les branches actives et inactives en ajoutant ou en supprimant une branche à l’aide de l’interface de ligne de commande [!DNL Cloud Console] ou Cloud. Avant de pouvoir supprimer une branche, vous devez la désactiver, car elle reste dans la liste _Environnements_ marquée comme _inactive_. Vous pouvez réactiver la branche ultérieurement ou [supprimer la branche](../dev-tools/cloud-cli-overview.md#) dans les paramètres d’environnement ou à l’aide de l’interface de ligne de commande cloud.

Si vous avez besoin d’environnements actifs supplémentaires pour le développement, envoyez un ticket d’assistance [Support](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket).

**Pour ajouter une branche** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Sélectionnez un environnement.

   >[!TIP]
   >
   >Votre nouvelle branche est clonée à partir de cet environnement. Choisissez un environnement parent similaire à l’environnement que vous êtes sur le point de créer.

1. Cliquez sur **[!UICONTROL Branch]**.

   ![Créer une branche](../../assets/button-branch.png){width="150"}

1. Dans le formulaire _Branchement à partir de..._ , saisissez un nom de branche.

   L’environnement _nom_ est différent de l’environnement _ID_ uniquement si vous utilisez des espaces ou des majuscules dans le nom de l’environnement. Un identifiant d’environnement se compose de toutes les lettres minuscules, de tous les chiffres et de tous les symboles autorisés. Les majuscules dans le nom d’un environnement sont converties en minuscules dans l’identifiant ; les espaces dans le nom d’un environnement sont convertis en tirets.

   Un nom d’environnement **ne peut pas** inclure des caractères réservés à votre shell Linux ou à des expressions régulières. Les caractères interdits comprennent les accolades (`{ }`), les parenthèses, l’astérisque (`*`), les crochets (`>`), l’esperluette (`&`), le pourcentage (<code> %)</code>) et d’autres caractères.

1. Sélectionnez un **[!UICONTROL Environment type]**.

1. Cliquez sur **[!UICONTROL Create Branch]**.

1. Patientez pendant le déploiement de l’environnement.

   Pendant le déploiement, le statut de l’environnement est **En cours**. Après un déploiement réussi, le statut devient une coche verte pour **succès**.

## Créer une branche inactive

Vous ne pouvez pas créer de branche inactive à partir de la console Adobe Commerce Cloud ou de l’interface de ligne de commande. Si vous souhaitez créer une branche inactive, créez-la dans le référentiel Git et effectuez une notification push à l’aide de l’option `environment.Parent` de la commande .

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Suppression d’un environnement

Avant de pouvoir supprimer un environnement, vous devez le désactiver. Une fois qu’un environnement est inactif, vous pouvez le supprimer.

**Pour désactiver un environnement** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Sélectionnez l’environnement dans la liste barre de navigation _Environnement_.

1. Cliquez sur l’icône de configuration sur le côté droit de la barre de navigation supérieure, ce qui ouvre les paramètres de l’environnement.

1. Dans l’onglet _[!UICONTROL General]_, faites défiler l’écran jusqu’à la section&#x200B;_[!UICONTROL Deactivate environment]_ , cliquez sur **[!UICONTROL Deactivate environment and delete data]** et suivez les instructions.

## Synchroniser un environnement

La synchronisation d’un environnement (ou d’une branche) est identique à la `git pull origin <parent>`. Vous pouvez synchroniser le code mis à jour à partir d’un environnement parent. Vous pouvez utiliser cette fonctionnalité via l’[!DNL Cloud Console] pour tous les environnements Starter et Pro.

Pour le plan Pro, vous pouvez synchroniser les environnements d’évaluation et de production avec votre branche `master`. Cette synchronisation extrait et transmet uniquement le code, et non les données. Pour synchroniser les données, videz les données de la base de données et envoyez-les vers la base de données d’un autre environnement. Voir [ Migrer et déployer des fichiers et des données statiques](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Pour synchroniser un environnement** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Dans la liste des environnements, cliquez sur le nom de la branche à synchroniser.

1. Cliquez sur (synchroniser).

   ![Synchroniser un environnement](../../assets/button-sync.png){width="150"}

1. Sélectionnez les éléments à synchroniser.

   - Remplacer les données : (données et fichiers) synchronise les modifications apportées à la base de données et aux fichiers de contenu de la branche parent.
   - Fusionner : (code) synchronise le code mis à jour à partir de la branche parent.

   Vous créez ainsi une commande d’interface de ligne de commande que vous pouvez copier et utiliser.

1. Cliquez sur **Synchroniser**.

## Fusionner avec l’environnement parent

La fusion d’un environnement (ou d’une branche) est identique à la `git push origin`. Vous fusionnez pour pousser le code mis à jour d’un environnement vers son environnement parent. Vous pouvez fusionner ce code en `master`. Vous pouvez effectuer un déploiement dans les environnements d’évaluation et de production à l’aide de la commande `merge`.

**Pour fusionner avec l’environnement parent** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Dans la liste des environnements, cliquez sur le nom de la branche à fusionner.

1. Cliquez sur (fusionner).

   ![Fusion d’un environnement](../../assets/button-merge.png){width="150"}

1. Cliquez sur **Fusionner** et confirmez l’action.

## Afficher les journaux

Grâce à l’[!DNL Cloud Console], vous pouvez consulter divers journaux pour les environnements, y compris l’historique de création, de déploiement et de déploiement.

Pour **Starter**, vous pouvez consulter les journaux de génération et de déploiement, ainsi que l’historique de déploiement. Ces environnements incluent la branche `master` (Production) et toutes les branches créées à partir de celle-ci.

Pour **Pro**, vous pouvez consulter les journaux suivants dans chaque environnement :

- Intégration : création, déploiement et historique de déploiement
- Évaluation : création de journaux et d’un historique de déploiement. Utilisez SSH pour vous connecter au serveur afin d’afficher les journaux de déploiement.
- Production : création de journaux et historique de déploiement. Utilisez SSH pour vous connecter au serveur afin d’afficher les journaux de déploiement.

**Pour afficher les journaux dans le[!DNL Cloud Console]** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Sélectionnez un environnement.

   La vue d’environnement fournit une [liste d’activités](activity-stream.md) qui affiche les événements _récents_, une entrée par action tentée, y compris les synchronisations, les fusions, les branches, les sauvegardes, etc. Cliquez sur **Tout** pour consulter l’historique complet des déploiements.

1. Pour afficher le journal de génération, sélectionnez le lien Succès ou Échec par enregistrement de déploiement sur le compte.

>[!TIP]
>
>Cliquez sur l’icône **Filtrer par** d’une liste déroulante et sélectionnez le type de messages à afficher.

## Extraction de code d’un référentiel Git privé

Votre projet d’infrastructure cloud Adobe Commerce peut inclure du code provenant d’un référentiel Git privé. Par exemple, vous pouvez avoir du code pour un module ou un thème personnalisé dans un référentiel privé. Pour ce faire, vous devez ajouter la clé SSH publique de votre projet à votre référentiel Git privé et mettre à jour votre fichier `composer.json` de projet.

Pour ajouter une clé de déploiement à votre référentiel GitHub privé, vous devez être l’administrateur de ce référentiel. GitHub vous permet d’utiliser une clé de déploiement pour un seul référentiel.

Si vous préférez que votre projet accède à plusieurs référentiels, vous pouvez joindre une clé SSH à un compte utilisateur automatisé. Ce compte n’étant pas utilisé par un humain, il est appelé [utilisateur de l’ordinateur](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Ajoutez le compte d&#39;ordinateur en tant que collaborateur ou ajoutez l&#39;utilisateur d&#39;ordinateur à une équipe ayant accès aux référentiels.

>[!INFO]
>
>Adobe recommande d’ajouter et de fusionner ce code dans les référentiels Git de votre projet. Si vous ne configurez pas la connexion, vous pouvez rencontrer des problèmes de build.

**Pour trouver votre clé publique SSH** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Cliquez sur l’icône de configuration sur le côté droit de la barre de navigation supérieure.

1. Dans _Paramètres du projet_, cliquez sur **[!UICONTROL Deploy Key]**.

1. Copiez la clé de déploiement dans le presse-papiers pour l’utiliser dans l’une des méthodes Git suivantes :

>[!BEGINTABS]

>[!TAB  GitHub ]

### Saisissez votre clé de déploiement GitHub

Sur GitHub, les clés de déploiement sont en lecture seule par défaut.

**Pour entrer la clé publique de votre projet en tant que clé de déploiement GitHub** :

1. Connectez-vous à votre référentiel GitHub en tant qu’administrateur.
1. Cliquez sur l’onglet **[!UICONTROL Settings]** du référentiel .

   >[!NOTE]
   >
   >Si cette option ne s’affiche pas, cela signifie que vous n’êtes pas connecté en tant qu’administrateur de référentiel et que vous ne pouvez pas terminer cette tâche. Demandez à votre administrateur de référentiel GitHub de le faire.

1. Dans l’onglet _Paramètres_ du volet de navigation de gauche, cliquez sur **[!UICONTROL Deploy Keys]**.
1. Cliquez sur **[!UICONTROL Add deploy key]**.
1. Suivez les invites.

En `composer.json`, utilisez le format `<user>@<host>:<.git</code>` ou `ssh://<user>@<host>:<port>/<path>.git` si vous utilisez un port non standard.

>[!TAB Bitbucket]

### Entrez votre clé de déploiement Bitbucket

**Pour entrer la clé publique de votre projet en tant que clé de déploiement Bitbucket** :

1. Connectez-vous à votre référentiel Bitbucket en tant qu’administrateur.
1. Dans le volet de navigation de gauche, cliquez sur **[!UICONTROL Settings]**.
1. Cliquez sur Général > **[!UICONTROL Deployment Keys]**.
1. Cliquez sur **[!UICONTROL Add Key]**.
1. Suivez les invites.

>[!TAB  GitLab ]

### Saisissez votre clé de déploiement GitLab

**Pour ajouter la clé SSH publique de votre projet en tant que clé de déploiement GitLab** :

1. Connectez-vous à votre référentiel GitLab en tant que propriétaire.
1. Vérifiez que l’option _Pipelines_ est activée pour votre projet :

   1. Dans les paramètres du projet, développez la section **[!UICONTROL Visibility, project, features, permissions]** .
   1. Si nécessaire, cliquez sur **[!UICONTROL Pipelines]** pour activer l’option.

1. Ajoutez votre clé SSH publique aux paramètres CI/CD.

   1. Dans le volet de navigation de gauche, cliquez sur Paramètres > **[!UICONTROL CI / CD]**.
   1. Cliquez sur Déployer les clés **Développer** pour configurer la clé.
   1. Dans le formulaire _Déployer la clé_, ajoutez un nom de clé de déploiement au champ **[!UICONTROL Title]** et collez votre clé SSH publique dans le champ **[!UICONTROL Key]**.
   1. Cliquez sur **[!UICONTROL Add Key]** pour enregistrer la configuration.

>[!ENDTABS]

## Environnements et branches sécurisés

Vous pouvez accéder à votre projet et à vos environnements à partir de n’importe quel emplacement via un navigateur web à l’aide de l’[!DNL Cloud Console] . La sécurité de votre environnement de production, de vos magasins et de vos sites peut être définie. Cette section vous aide à sécuriser vos environnements d’intégration et d’évaluation pour les développeurs, les administrateurs de base de données, etc.

>[!WARNING]
>
>**À NE PAS** utilisez les méthodes suivantes pour sécuriser les environnements d’évaluation et de production Pro. Cela interrompt la mise en cache rapide. Utilisez la fonctionnalité [Blocage](../cdn/fastly-vcl-blocking.md) disponible dans le réseau CDN Fastly pour Adobe Commerce.

**Pour sécuriser les environnements** :

1. Connectez-vous à l’[[!DNL Cloud Console]](https://console.adobecommerce.com) .

1. Sélectionnez un projet dans la liste _Tous les projets_.

1. Sélectionnez un environnement et cliquez sur l’icône de configuration dans la barre de navigation.

1. Dans l’onglet Paramètres d’environnement _Général_, cliquez sur **ACTIVÉ** pour **[!UICONTROL HTTP access control enabled]** activer l’accès sécurisé. Vous pouvez choisir entre les informations d’identification ou les adresses IP à filtrer pour l’accès.

1. Pour filtrer par informations d’identification, cliquez sur **[!UICONTROL Add Login]**, saisissez un nom d’utilisateur et un mot de passe, puis cliquez sur **[!UICONTROL Add Login]** pour les ajouter.

1. Pour filtrer par adresse IP, saisissez les adresses IP dans une liste comportant `deny` ou `allow`. Par exemple :

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Cliquez sur **[!UICONTROL Save]**. Cela redéploie l’environnement pour mettre à jour la sécurité et les paramètres. Adobe recommande de tester l’environnement après avoir défini les paramètres de sécurité.
