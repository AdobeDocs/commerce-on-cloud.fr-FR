---
title: Exemple de gestion de paramètres spécifiques au système
description: Consultez un exemple de gestion et de synchronisation des paramètres de configuration de magasin dans tous les environnements d’infrastructure cloud d’Adobe Commerce.
hidefromtoc: true
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 0%

---


# Exemple de gestion de paramètres spécifiques au système

Cet exemple montre comment utiliser la gestion des configurations pour garantir la cohérence des paramètres de stockage dans tous les environnements.

L’exemple utilise la procédure suivante définie dans [Paramètres du magasin](store-settings.md) :

1. Saisissez vos configurations dans l’administration du magasin de votre environnement d’intégration.
1. Créez un fichier `config.php` et transférez-le vers votre station de travail locale.
1. Envoyez les `config.php` vers l’environnement d’intégration distant.
1. Vérifiez que vos paramètres ne sont pas modifiables dans l’administration.
1. Apportez les modifications nécessaires :

   * Modifiez les paramètres de configuration dans l’environnement d’intégration.
   * Pour ajouter des configurations, exécutez la commande pour créer à nouveau des `config.php`. Les nouvelles configurations sont ajoutées au fichier .
   * Pour supprimer ou modifier des configurations existantes, modifiez manuellement le fichier .
   * Validez et envoyez des notifications push.

Par exemple, vous pouvez définir les paramètres suivants :

* Désactivation des paramètres régionaux et des paramètres d’optimisation des fichiers statiques dans votre environnement d’intégration
* Activation de l’optimisation des fichiers statiques dans les environnements d’évaluation et de production
* Configurez Fastly dans les environnements d’évaluation et de production avec des informations d’identification spécifiques pour chacun

_Optimisation de fichiers statiques_ signifie fusionner et réduire des feuilles de style JavaScript et en cascade, et réduire des modèles d’HTML. Voir [Stratégies de déploiement de contenu statique](../deploy/static-content.md).

## Conditions préalables

Pour effectuer ces tâches de gestion de la configuration, vous devez disposer des éléments suivants :

* Rôle de lecteur de projet avec des privilèges [d’« administrateur »](../project/user-access.md) d’environnement
* URL d’administration et informations d’identification pour les environnements d’intégration, d’évaluation et de production

## Configuration de l’administration Commerce

Dans l’environnement d’intégration, vous pouvez vous connecter à l’administrateur pour modifier les paramètres de configuration système des magasins, des sites web, des modules ou des extensions, l’optimisation des fichiers statiques et les valeurs système liées au déploiement de contenu statique. Voir [ Données de configuration ](store-settings.md#scd-performance).

**Pour modifier les paramètres régionaux et les paramètres d’optimisation des fichiers statiques** :

1. Connectez-vous à l’administration de l’environnement d’intégration. Vous pouvez accéder à cette URL via l’[[!DNL Cloud Console]](../project/overview.md) .
1. Accédez à **Magasins** > Paramètres > **Configuration** > Général > **Général**.
1. Dans la navigation de la page, développez **Options locales**.
1. Dans la liste **Paramètres régionaux**, modifiez le paramètre régional. Vous pourrez la modifier ultérieurement.

   ![Modifier le paramètre régional](../../assets/locale-options.png)

1. Cliquez sur **Enregistrer la configuration**.
1. Si vous y êtes invité, [ videz le cache](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/tools/cache-management).
1. Déconnectez-vous de l’administrateur.

## Exporter les valeurs et transférer config.php à votre système local

Cette étape crée et transfère le fichier de configuration `config.php` sur l’environnement d’intégration à l’aide d’une commande que vous exécutez sur votre ordinateur local.

Cette procédure correspond à l&#39;étape 2 de la [procédure recommandée](store-settings.md). Après avoir créé `config.php`, transférez-le vers votre système local afin de pouvoir l’ajouter à Git.

**Pour créer et transférer des`config.php`** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Modification de l’environnement d’intégration.

   ```bash
   magento-cloud environment:checkout integration
   ```

1. Créez une image mémoire locale de la base de données distante.

   ```bash
   magento-cloud db:dump
   ```

Le fragment de code suivant provenant de l’adresse `config.php` illustre un exemple de modification du paramètre régional par défaut en `en_GB` et de modification des paramètres d’optimisation des fichiers statiques :

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## Intégrez et déployez config.php dans les environnements

Maintenant que vous avez créé `config.php` et que vous l’avez transféré sur votre système local, validez-le dans Git et envoyez-le à vos environnements. Cette procédure correspond aux étapes 3 et 4 de la [procédure recommandée](store-settings.md).

La commande suivante ajoute, valide et transmet à la branche `master` :

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

Terminez le déploiement du code dans les environnements d’évaluation et de production. Pour commencer, vous poussez vers les branches `staging` et `master`. Pour plus d’informations sur les commandes de déploiement, voir [Déployer votre boutique](../deploy/staging-production.md).

Attendez la fin du déploiement dans tous les environnements.

## Vérification des modifications apportées à la configuration

Après avoir envoyé le `config.php` vers vos environnements, toutes les valeurs que vous avez modifiées doivent être en lecture seule dans l’administrateur. Dans cet exemple, les paramètres régionaux par défaut modifiés et les paramètres d’optimisation des fichiers statiques ne doivent pas être modifiables dans l’administration. Ces paramètres de configuration sont définis dans `config.php`.

Pour vérifier vos modifications de configuration :

1. Déconnectez-vous de l’administrateur dans l’un des environnements.
1. Connectez-vous à nouveau à l’administrateur.
1. Cliquez sur **Magasins** > Paramètres > **Configuration** > Général > **Général**.
1. Dans le volet de droite, développez **Options locales**.

   Notez que plusieurs champs ne peuvent pas être modifiés, comme illustré dans l’exemple suivant. Ces paramètres de configuration sont conservés par `config.php`.

   ![Certaines valeurs ne sont plus modifiables dans l’administration](../../assets/locale-options-disabled.png)

1. Déconnectez-vous de l’administrateur.

## Modification et mise à jour des paramètres de configuration spécifiques au système

Si vous devez modifier l’un de ces paramètres, modifiez manuellement le fichier `config.php` à l’aide d’un éditeur de texte. Une fois les modifications ou les suppressions effectuées, vous pouvez valider et pousser l’élément vers l’environnement distant en suivant les étapes précédentes.

Pour ajouter des configurations, modifiez votre environnement d’intégration et exécutez à nouveau la commande pour générer le fichier. Toutes les nouvelles configurations sont ajoutées au code dans le fichier . Envoyez-le à Git en suivant les étapes précédentes.

Pour cet exemple, modifiez les paramètres d’optimisation des fichiers statiques et ajoutez un nouveau paramètre pour JavaScript.

### Ajout de configurations dans l’intégration

Pour ajouter des valeurs de configuration dans l’environnement d’intégration Admin. Cet exemple fusionne des fichiers JavaScript.

1. Déconnectez-vous de l’administrateur d’intégration.
1. Connectez-vous à nouveau à l’administrateur d’intégration.
1. Cliquez sur **Magasins** > Paramètres > **Configuration** > **Avancé** > **Développeur**.
1. Dans le volet de droite, développez **Paramètres JavaScript**.
1. Dans la liste **Fusionner des fichiers JavaScript**, cliquez sur **Oui**.
1. Cliquez sur **Enregistrer la configuration**.
1. Si vous y êtes invité, [ videz le cache](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/tools/cache-management).
1. Déconnectez-vous de l’administrateur.

En exécutant à nouveau la commande de vidage, la nouvelle configuration est ajoutée au fichier .

```bash
magento-cloud db:dump
```

### Modifier config.php avec de nouveaux paramètres

Sur votre ordinateur local, utilisez un éditeur de texte pour modifier le fichier `app/etc/config.php` mis à jour. Modifiez ces paramètres pour activer la minimisation pour les fichiers JavaScript, HTML et CSS.

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

Pour modifier les paramètres afin d’autoriser la minimisation, modifiez les `'0'` à `'1'` pour les `'minify_html'` et chaque option de `'minify_files'` :

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

Enregistrez les modifications dans le fichier .

### Envoyez les modifications à Git.

Pour pousser vos modifications, saisissez ce qui suit :

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

Attendez la fin du déploiement.

Répétez le processus de déploiement pour envoyer le code vers tous les environnements.
