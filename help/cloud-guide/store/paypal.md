---
title: Configurer les modes de paiement PayPal
description: Configurez les modes de paiement PayPal pour Adobe Commerce sur l’infrastructure cloud.
feature: Cloud, Checkout, Payments
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Configurer les modes de paiement PayPal

Adobe Commerce sur l’infrastructure cloud fournit un outil d’intégration pour configurer les comptes de paiement PayPal Express directement via l’administrateur. Cet outil est disponible pour ECE 2.1.8 et versions ultérieures. Pour mieux prendre en charge la mise en ligne et le test des méthodes de paiement PayPal, vous pouvez activer et configurer votre compte de paiement PayPal Express pour les comptes sandbox ou de production.

Vous pouvez configurer le sandbox ou le compte de production dans chaque environnement :

* Pour les environnements d’intégration et d’évaluation, définissez les informations d’identification Sandbox.
* Pour votre environnement de production, définissez les informations d’identification Sandbox pour les tests initiaux, puis remplacez par les informations d’identification de production en direct pour un magasin lancé.

## Compte PayPal

Bien qu&#39;il soit préférable d&#39;utiliser un compte marchand PayPal préparé et configuré, vous pouvez créer un compte ou mettre à niveau un compte personnel par l&#39;intermédiaire de l&#39;administrateur.

[!DNL PayPal onboarding] prend en charge la connexion avec les comptes suivants :

* Compte professionnel PayPal
* Compte personnel PayPal, conversion en compte professionnel. Si vous disposez déjà d’un compte PayPal personnel, vous pouvez vous connecter à l’aide de ces informations d’identification et transformer ce compte en compte professionnel au fur et à mesure de la synchronisation.

Si vous ne disposez pas d&#39;un compte PayPal existant, créez-en un. Saisissez l’adresse e-mail du nouveau compte. Si aucun compte PayPal correspondant n&#39;est trouvé, suivez les invites pour créer un compte PayPal Business. Vous pouvez également créer un compte directement via [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

### Limites de PayPal

PayPal prend en charge la connexion à PayPal Express Checkout pour les pays du monde entier, à l&#39;exception des limitations suivantes :

* Inde et Japon (les futures mises à jour de PayPal pourront prendre en charge ces comptes)
* Israël

Pour le Brésil, vous devez disposer d&#39;un compte professionnel PayPal existant pour vous connecter. Vous ne pouvez pas convertir un compte PayPal personnel existant pour Brésil pendant ce processus. Si vous avez besoin d&#39;un compte, [créez un compte PayPal professionnel](https://www.paypal.com/us/webapps/mpp/account-selection).

## Configurer PayPal Express Checkout

Pour configurer PayPal Express Checkout :

1. Accédez à Admin pour l’environnement .
1. Dans le volet de navigation de gauche, sélectionnez **Magasins** > **Configuration**, puis sélectionnez **Ventes** > **Modes de paiement**.
1. Pour PayPal, sélectionnez **Configurer**. Les champs de configuration s’affichent dans des sections extensibles pour Paiement express, Annoncer le crédit PayPal et les paramètres de base et avancés.
1. Connectez votre compte PayPal. Tant que le compte n’est pas connecté, les options à activer sont désactivées. Pour plus d’informations sur les comptes disponibles et pris en charge pour la connexion et les limitations, voir [Compte PayPal](#paypal-account).

   * Pour connecter votre compte PayPal en direct, cliquez sur Se connecter avec PayPal et suivez les invites. Tous les achats des clients effectués à l&#39;aide d&#39;un PayPal actif sont effectués et facturés activement dans un magasin actif.
   * Pour connecter votre compte sandbox à des fins de test, cliquez sur Informations d’identification du sandbox et suivez les invites. Tout achat de client effectué à l’aide d’un sandbox PayPal se fait sans facturer activement les clients.

1. Configurez les paramètres de paiement express pour vous authentifier et utiliser l’API PayPal :

   * **Adresse e-mail associée au compte marchand PayPal** (facultatif) saisissez l&#39;adresse e-mail associée à votre compte marchand PayPal. Cet e-mail respecte la casse.
   * **Méthodes d’authentification d’API** comme signature d’API ou certificat d’API.
   * Nom d&#39;utilisateur, mot de passe et signature API capturés à partir de votre compte PayPal.
   * **Mode sandbox** sélectionnez Oui ou Non pour indiquer si les informations d’identification saisies concernent le sandbox. Si vous avez saisi les informations d’identification de production, sélectionnez Non.
   * **L’API utilise un proxy** sélectionnez Oui ou Non pour définir si le système utilise un serveur proxy pour établir une connexion entre Adobe Commerce et le système de paiement PayPal. Si Oui, saisissez l’hôte proxy et le port.

1. Pour obtenir des informations détaillées et connaître la procédure de configuration de votre compte, consultez [Paiement PayPal Express](https://experienceleague.adobe.com/fr/docs/commerce-admin/stores-sales/payments/paypal/paypal-express-checkout) à partir de l&#39;étape 2.

Une fois le compte configuré et authentifié, vous pouvez activer et désactiver les options de paiement PayPal sous Paramètres PayPal requis :

* **Activer cette solution** affiche le mode de paiement PayPal aux clients via le site web.
* **Activer l’expérience de paiement contextuelle**
* **Activer le crédit PayPal** permet aux clients de bénéficier d&#39;un financement par crédit PayPal sans frais supplémentaires. PayPal paie la commande en amont, en gérant tous les remboursements du crédit directement avec le client.

## Variables PayPal

Lors de l’utilisation de l’outil d’intégration PayPal avec Adobe Commerce sur l’infrastructure cloud, ajoutez la variable suivante à la section `variables:env` du fichier `magento.app.yaml` .

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

Si vous effectuez une mise à niveau vers la version 2.2 à partir de la version 2.1.8 ou ultérieure, vous devez toujours ajouter cette variable.
