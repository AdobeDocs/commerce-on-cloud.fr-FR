---
title: Propriété des variables
description: Utilisez la propriété variables pour personnaliser les options de configuration du magasin pour l’application  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Propriété des variables

Vous pouvez utiliser des variables d’environnement basées sur une application pour personnaliser les configurations de magasin. Ces variables utilisent une syntaxe spécifique. Voir [Remplacer les paramètres de configuration](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=fr) dans le _Guide de configuration_.

Les variables d’environnement suivantes incluses dans le fichier `.magento.app.yaml` sont requises pour des versions spécifiques de l’application [!DNL Commerce].

Requis pour Adobe Commerce 2.2.x à 2.3.x :

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Pour Adobe Commerce 2.4.x, définissez les variables suivantes :

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
