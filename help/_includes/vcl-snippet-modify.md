---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Inclure le fichier pour modifier le VCL personnalisé pour Fastly

## Modifier le fragment de code VCL personnalisé

1. [Connectez-vous](/help/get-started/onboarding.md#access-your-admin-panel) à l’administrateur.

1. Cliquez sur **Magasins** > **Paramètres** > **Configuration** > **Avancé** > **Système**.

1. Développez **Cache de page complet** > **Configuration rapide** > **Fragments de code VCL personnalisés**.

   ![Gestion des fragments de code VCL personnalisés](/help/assets/cdn/fastly-manage-snippets.png)

1. Dans la colonne _Action_, cliquez sur l’icône de paramètres en regard du fragment de code à modifier.

1. Une fois la page rechargée, cliquez sur **Télécharger VCL vers Fastly** dans la section _Configuration Fastly_.

1. Une fois le chargement terminé, actualisez le cache en fonction de la notification en haut de la page.

>[!WARNING]
>
>L’option d’interface utilisateur _Fragments de code VCL personnalisés_ affiche uniquement les fragments de code ajoutés par l’intermédiaire de l’administrateur Adobe Commerce. Si vous ajoutez des fragments de code à l’aide de l’API Fastly, utilisez l’API pour [les gérer](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
