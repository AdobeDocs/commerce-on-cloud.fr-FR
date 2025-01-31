---
title: Étapes de lancement
description: Découvrez comment terminer le lancement du site.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Étapes de lancement

Après avoir testé et terminé la liste de contrôle de lancement, vous pouvez commencer les dernières étapes du lancement. Ces étapes incluent la saisie des tickets, l’accès au site de transfert et le test de votre boutique en direct.

Le personnel du soutien de l&#39;Adobe travaille avec vous tout au long du processus, en vérifiant le statut et en vous aidant en cas de questions ou de problèmes. Tous les problèmes doivent être suivis à l’aide de tickets afin de capturer au mieux ce qui s’est passé et comment cela a été résolu. Lorsque vous commencez des itérations continues déployant des mises à jour vers la boutique que vous avez lancée, des problèmes similaires peuvent se reproduire. Ces tickets peuvent vous aider à identifier les problèmes et à ajuster vos tâches de déploiement.

## Contactez l’Adobe pour lancer votre site

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

L’Adobe vérifie et surveille le site pour s’assurer que tous les services et accès sont en vert. Nous restons à votre disposition pour examiner et vérifier que tous les journaux système, les services, la mise en cache et les fonctions fonctionnent comme vous et vos clients le souhaitez.

En cas de problème, créez et suivez les problèmes avec l’assistance. Incluez autant d’informations que possible, y compris la date et l’heure, la fonctionnalité spécifique avec un problème, les symptômes et les comportements étranges, les extensions, etc. Nous examinons les logs, le problème et travaillons avec vous pour résoudre rapidement le problème.
