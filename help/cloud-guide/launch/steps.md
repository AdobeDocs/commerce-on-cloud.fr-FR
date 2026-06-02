---
title: Étapes de lancement
description: Découvrez comment terminer le lancement du site.
exl-id: e7a3cd6b-32de-4fd0-9fbd-da8299e77114
TQID: https://experienceleague.adobe.com/Nl8YFJUZUkxtsm0eqBgJklsTysKuUE31PkEDsJmvBMU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: c1256247-af4b-46d8-9dca-0c654ecfa157id: d1e21356-0064-4f48-9089-16e3f0dbd2a6id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 347
ht-degree: 0%

---

# Étapes de lancement

Après avoir testé et terminé la liste de contrôle de lancement, vous pouvez commencer les dernières étapes du lancement. Ces étapes incluent la saisie des tickets, l’accès au site de transfert et le test de votre boutique en direct.

Le personnel d’assistance d’Adobe vous accompagne tout au long du processus, en vérifiant le statut et en vous aidant en cas de questions ou de problèmes. Tous les problèmes doivent être suivis à l’aide de tickets afin de capturer au mieux ce qui s’est passé et comment cela a été résolu. Lorsque vous commencez des itérations continues déployant des mises à jour vers la boutique que vous avez lancée, des problèmes similaires peuvent se reproduire. Ces tickets peuvent vous aider à identifier les problèmes et à ajuster vos tâches de déploiement.

## Contactez Adobe pour lancer votre site

Contactez l’assistance Adobe Commerce et mettez à jour tous les tickets de lancement de site (en ligne) avec la date et l’heure prévues pour basculer et lancer votre boutique.

## Basculer le DNS vers le nouveau site

La valeur de la durée de vie modifiée est importante pour vérifier votre domaine modifié. Lorsque vous modifiez les enregistrements A et CNAME, la mise à jour prend le temps configuré pour la TTL pour se mettre à jour correctement. Pour plus d’informations sur les paramètres DNS, voir [Configurations DNS](checklist.md#update-dns-configuration-with-production-settings).

### Pour accéder au nouveau site :

1. Accédez à votre service DNS.

1. Mettez à jour vos enregistrements A et CNAME pour vos domaines et noms d’hôtes.

1. Patientez jusqu’à ce que le temps de TTL soit écoulé et redémarrez votre navigateur web.

1. Accédez à votre boutique à l’aide du domaine storefront.

## Tester le magasin en ligne

Effectuez quelques tests UAT dans votre boutique en ligne pour confirmer que tout est en cours de chargement et que les actions se terminent correctement. Pour obtenir la liste des tests, voir [Tester le déploiement](../test/staging-and-production.md#complete-uat-testing).

## Après le lancement

Adobe vérifie et surveille le site pour s’assurer que tous les services et accès sont en vert. Nous restons à votre disposition pour examiner et vérifier que tous les journaux système, les services, la mise en cache et les fonctions fonctionnent comme vous et vos clients le souhaitez.

En cas de problème, créez et suivez les problèmes avec l’assistance. Incluez autant d’informations que possible, y compris la date et l’heure, la fonctionnalité spécifique avec un problème, les symptômes et les comportements étranges, les extensions, etc. Nous examinons les logs, le problème et travaillons avec vous pour résoudre rapidement le problème.
