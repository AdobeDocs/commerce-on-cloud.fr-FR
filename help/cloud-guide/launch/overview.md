---
title: Lancement du site
description: Découvrez comment commencer la préparation du lancement du site.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---

# Lancement du site

Une fois le déploiement et les tests terminés dans les environnements d’intégration et d’évaluation, vous pouvez commencer la préparation du lancement du site. Tout d’abord, vous devez terminer tous les développements et tests avant de travailler dans l’environnement de production. Vous vous sentez prêt pour le lancement ? Examinez les listes de contrôle, les bonnes pratiques et les étapes finales pour lancer votre site.

Si vous avez vérifié ces informations avant le déploiement et le test dans l’évaluation, pensez à examiner d’abord les avantages du test dans l’évaluation dans la section suivante. L’évaluation est un environnement de quasi-production qui s’exécute sur du matériel, des configurations, une architecture et des services similaires. Cela peut réduire votre temps d’arrêt et rendre vos composants d’extension, de service, de configurations personnalisées et de test d’acceptation des utilisateurs et utilisatrices commerçants essentiels à la publication de vos sites et magasins.

## Pourquoi effectuer des tests complets en matière d’intégration, d’évaluation et de production ?

Nous vous recommandons vivement de tester dans les environnements d’intégration, d’évaluation et de production en raison de la complexité liée à la nécessité de vous assurer que votre code personnalisé, vos thèmes, vos extensions et vos intégrations tierces fonctionnent tous ensemble pour faire fonctionner vos magasins. Voici des problèmes courants que vous pouvez découvrir et résoudre lorsque vous effectuez des tests dans les environnements d’intégration et d’évaluation avant de mettre à jour votre environnement de production :

- L’évaluation prend en charge tous les services de production, les fonctionnalités, les données de base de données, la pile technologique, l’architecture, etc. Elle reflète la production, ce qui signifie que si des erreurs se produisent dans l’évaluation, vous recevez un avertissement avant qu’elles ne se produisent dans la production.

- Le code qui fonctionne dans votre environnement d’intégration local peut ne pas fonctionner dans les environnements d’évaluation et de production.

- Les environnements d’intégration ne prennent pas en charge certains services disponibles dans les environnements d’évaluation et de production, tels que Fastly et New Relic.

- [Testez intégralement](../test/guidance.md) votre site à l’aide de divers outils dans le cadre de l’évaluation pour la charge, le stress, les performances et les ressources du site.

- Étant donné que les environnements d’intégration peuvent uniquement avoir des bases de données remplies de données de test, qui ne correspondent pas à un environnement de production, vous pouvez trouver des erreurs supplémentaires ou un comportement inattendu lors des tests dans les environnements d’évaluation ou de production.

## Conditions préalables pour le lancement du site

Vous avez besoin des informations et ressources suivantes pour préparer le lancement du site :

- Informations d’enregistrement CNAME pour la configuration du DNS

- Liste de tous les domaines storefront à ajouter au certificat

- Certificat SSL/TLS

Dans le cadre de l’abonnement à Adobe Commerce sur l’infrastructure cloud, Adobe fournit un certificat SSL/TLS validé par un domaine émis par Let’s Encrypt. Chaque environnement Pro Production, Staging et Starter Production (`master`) dispose d&#39;un certificat unique qui couvre tous les domaines et sous-domaines de cet environnement. Ces certificats sont configurés et chargés automatiquement sur votre site après la mise à jour de votre configuration DNS pour le développement et la production. Voir [Approvisionnement de certificats SSL/TLS](../cdn/fastly-configuration.md#provision-ssltls-certificates).

>[!NOTE]
>
>Si vous souhaitez déployer votre propre certificat SSL de validation étendue pour votre société au lieu d’utiliser le certificat Let’s Encrypt, contactez votre CTA ou [Envoyez un ticket d’assistance pour Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=fr#submit-ticket).

## Configuration de l’outil d’analyse de sécurité

>[!NOTE]
>
>L’outil d’analyse de sécurité utilise les adresses IP publiques suivantes :
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>Ajoutez ces adresses IP à une liste autorisée dans les règles de pare-feu de votre réseau pour permettre à l’outil d’analyser votre site. L’outil affiche les requêtes sur les ports 80 et 443 uniquement.

L&#39;outil Security Scan Tool vous permet de surveiller régulièrement les sites Web de votre boutique et de recevoir des mises à jour pour les risques de sécurité connus, les logiciels malveillants et les logiciels obsolètes. Cet outil est un service gratuit disponible pour toutes les implémentations et versions d’Adobe Commerce sur les infrastructures cloud. Vous accédez à l’outil par l’intermédiaire de votre compte de Commerce Marketplace [&#128279;](https://account.magento.com/customer/account/login).

- Surveillance de l’état de sécurité de vos sites et des mises à jour de sécurité appliquées

- Recevoir des mises à jour de sécurité et des notifications spécifiques au site

Pour plus d’informations sur la configuration et l’utilisation de l’outil d’analyse de sécurité[&#128279;](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/security/security-scan) consultez le  Guide de l’utilisateur. En règle générale, vous commencez à utiliser cet outil lorsque vous commencez les tests d’acceptation utilisateur (UAT).

Chaque site que vous analysez doit être enregistré via l&#39;onglet Analyse de sécurité. Pendant le processus d&#39;enregistrement, vous devez accepter la clause de non-responsabilité avant de pouvoir commencer l&#39;analyse. Vous contrôlez à la fois le planning et l’autorisation donnée à l’utilisateur de recevoir des notifications une fois chaque analyse terminée. Vous pouvez planifier des analyses pour une date et une heure spécifiques et récurrentes, ou exécuter une analyse à la demande si nécessaire.

L&#39;outil Security Scan Tool utilise plusieurs chaînes d&#39;agent utilisateur pour simuler l&#39;activité réelle des programmes malveillants. Il se peut que les agents utilisateurs suivants s’affichent dans vos journaux d’analyse ou d’accès :

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## Analyser votre site

1. Accédez à votre compte de Commerce Marketplace [&#128279;](https://account.magento.com/customer/account/login).

1. Cliquez sur l’onglet Analyse de sécurité et sélectionnez **Accéder à l’analyse de sécurité**.

1. Dans la colonne _Actions_ du site, sélectionnez **Exécuter l’analyse**. Un état de notification affiche l’analyse planifiée.

### Pour consulter le rapport :

1. Une fois le rapport terminé, une notification s’affiche.

1. Sur la ligne du site, sélectionnez le rapport à afficher dans la colonne **Rapports**. La commande est de la plus récente à la plus ancienne.

Le rapport répertorie les problèmes, notamment les échecs d’analyse, les résultats non identifiés et les analyses réussies. Chaque entrée fournit des informations détaillées pour l’analyse, une liste des problèmes à examiner et des actions à entreprendre. Certaines de ces actions peuvent nécessiter le téléchargement et l&#39;installation de correctifs de sécurité. Ajoutez les correctifs nécessaires à une branche de développement de votre station de travail locale avant de les ajouter à la branche de production.

Les résultats de l’analyse incluent un libellé qui décrit le statut de réussite ou d’échec de l’analyse avec des informations détaillées sur les vérifications effectuées :

- « Failed » indique que le site web présente une vulnérabilité grave.

- « Non identifié » suggère qu’une révision plus approfondie est requise par votre équipe ou votre fournisseur d’hébergement pour déterminer si d’autres actions sont requises.

Les résultats de l’analyse fournissent également des suggestions d’étapes de correction pour chaque test de sécurité ayant échoué. Les résultats de l’analyse de sécurité ne sont protégés et visibles que par l’utilisateur enregistré. Seuls les utilisateurs désignés dans le processus d&#39;enregistrement du site reçoivent des notifications de fin d&#39;analyse.

## Prêt à lancer votre site

Lorsque vous êtes prêt à lancer le processus de lancement du site, reportez-vous aux sections suivantes :

- [Liste de contrôle de Launch](checklist.md)

- [Étapes de lancement](steps.md)
