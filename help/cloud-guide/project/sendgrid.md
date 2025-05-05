---
title: Service de messagerie SendGrid
description: Découvrez le service de messagerie SendGrid pour Adobe Commerce sur l’infrastructure cloud et comment tester votre configuration DNS.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1317'
ht-degree: 0%

---

# Service de messagerie SendGrid

Le service proxy SMTP (Simple Mail Transfer Protocol) SendGrid fournit des services d&#39;authentification des e-mails sortants et de surveillance de la réputation, notamment la prise en charge des éléments suivants :

* Tous les emails transactionnels sortants
* Adresses IP dédiées
* Enregistrement de domaine, signatures DomainKeys Identified Mail (DKIM) pour la validation de domaine d’e-mail (pour les environnements d’évaluation et de production Pro uniquement)
* Enregistrement de domaine personnalisé (pour Pro uniquement)
* Intégration automatisée pour les environnements d’intégration Starter et Pro. Les environnements de production et d&#39;évaluation Pro nécessitent un approvisionnement et une configuration manuels pendant le processus d&#39;approvisionnement matériel d&#39;Infrastructure as a Service (IaaS)

Le proxy SMTP SendGrid n&#39;est pas destiné à être utilisé comme serveur de messagerie à usage général pour recevoir des e-mails entrants ou pour être utilisé avec des campagnes marketing par e-mail.

>[!TIP]
>
>Assurez-vous d’avoir configuré les adresses e-mail de magasin appropriées dans l’administrateur en accédant à Magasins > Configuration > Général pour éviter des problèmes de délivrabilité et de vérification du domaine. Vous devez décocher la case **[!UICONTROL Use Default]** et remplacer les valeurs par défaut par un domaine que vous possédez. Les services de messagerie publics/à domaine partagé, tels que gmail.com et outlook.com, ne doivent pas être configurés comme adresse e-mail de l’expéditeur lors de l’envoi d’e-mails via Sendgrid.

## Activer ou désactiver l’e-mail

Vous pouvez activer ou désactiver les e-mails sortants pour chaque environnement à partir de la console Cloud ou de la ligne de commande.

Par défaut, les e-mails sortants sont activés sur les environnements de production et d’évaluation Pro. Cependant, [!UICONTROL Outgoing emails] peut sembler désactivé dans les paramètres d’environnement jusqu’à ce que vous définissiez la propriété `enable_smtp` via [ligne de commande](outgoing-emails.md#enable-emails-in-the-cli) ou [console cloud](outgoing-emails.md#enable-emails-in-the-cloud-console). Vous pouvez activer les e-mails sortants pour les environnements d’intégration et d’évaluation afin d’envoyer une authentification à deux facteurs ou réinitialiser les e-mails de mot de passe pour les utilisateurs de projets cloud. Voir [Configuration des e-mails pour les tests](outgoing-emails.md).

Si les e-mails sortants doivent être désactivés ou réactivés dans les environnements de production ou d’évaluation Pro, vous pouvez envoyer un [ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>La mise à jour de la valeur de la propriété [!UICONTROL enable_smtp] par [ligne de commande](outgoing-emails.md#enable-emails-in-the-cli) modifie également la valeur du paramètre [!UICONTROL Enable outgoing emails] pour cet environnement sur la [console cloud](outgoing-emails.md#enable-emails-in-the-cloud-console).

## Tableau de bord SendGrid

Tous les projets cloud sont gérés sous un compte central, de sorte que seul le support technique a accès au tableau de bord SendGrid. SendGrid ne fournit pas de fonctionnalités de restriction de sous-compte.

Pour consulter les logs d’activité concernant le statut de la diffusion ou une liste d’adresses e-mail de rebond, de rejet ou de blocage, [envoyez un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). L’équipe d’assistance **ne peut pas** récupérer les journaux d’activité datant de plus de 30 jours.

Si possible, joignez les informations suivantes à votre demande :

* la ou les adresses e-mail concernées
* le délai en question (au cours des 30 derniers jours uniquement)
* objet de l’e-mail

## Message identifié DomainKeys (DKIM)

DKIM est une technologie d’authentification des e-mails qui permet aux fournisseurs d’accès Internet (FAI) d’identifier les adresses d’expéditeur légitimes et falsifiées. Cette technique est couramment utilisée dans les cas d’hameçonnage et d’escroquerie par e-mail. DKIM repose sur un propriétaire de domaine qui gère les enregistrements DNS. Lors de l’utilisation de DKIM, le serveur expéditeur utilise une clé privée pour signer les messages. En outre, le propriétaire du domaine ajoute un enregistrement DKIM, qui est un enregistrement `TXT` modifié, aux enregistrements DNS du domaine expéditeur. Cet enregistrement `TXT` contient une clé publique que les serveurs de messagerie des destinataires utilisent pour vérifier la signature d&#39;un message. La procédure de chiffrement à clé publique DKIM permet aux destinataires de vérifier l’authenticité d’un expéditeur. Voir [Explication Des Enregistrements DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Les signatures SendGrid DKIM et la prise en charge de l’authentification de domaine ne sont disponibles que dans les environnements de production et d’évaluation pour les projets Pro, mais pas dans tous les environnements de démarrage. Par conséquent, les e-mails transactionnels sortants sont susceptibles d&#39;être marqués par des filtres de spam. L’utilisation de DKIM améliore le taux de diffusion en tant qu’expéditeur d’e-mail authentifié. Pour améliorer le taux de diffusion des messages, vous pouvez effectuer une mise à niveau de Starter vers Pro ou utiliser votre propre serveur SMTP ou votre fournisseur de services de diffusion par e-mail. Voir [Configurer les connexions par e-mail](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications) dans le guide _Admin Systems_.

### Authentification de l’expéditeur et du domaine

Pour que SendGrid envoie des e-mails transactionnels en votre nom à partir d&#39;environnements de production ou d&#39;évaluation Pro, vous devez configurer vos paramètres DNS pour inclure les trois entrées DNS du sous-domaine SendGrid. Chaque compte SendGrid se voit attribuer un enregistrement `TXT` unique qui est utilisé pour authentifier les e-mails sortants.

>[!TIP]
>
>Assurez-vous de configurer l’option **[!UICONTROL Sstocker les adresses e-mail]** avec le domaine approprié dans **[!UICONTROL Stores > Configuration > General > Store Email Addresses]**. L’authentification de domaine est effectuée sur l’adresse e-mail de l’expéditeur. Si le paramètre par défaut (`example.com`) est configuré, les e-mails provenant de `example.com` sont bloqués par Sendgrid.

**Pour activer l’authentification de domaine** :

1. Envoyez un [ ticket d’assistance ](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) pour demander l’activation de DKIM pour un domaine spécifique (**environnements d’évaluation et de production Pro uniquement**).
1. Mettez à jour votre configuration DNS avec les enregistrements `TXT` et `CNAME` qui vous ont été fournis dans le ticket de support.

**Exemple d&#39;enregistrement `TXT` avec l&#39;identifiant de compte** :

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Exemple `CNAME` enregistrements** :

| Domaine | Pointe Vers | Type d’enregistrement |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### Signatures DKIM et sécurité automatisée

Vous pouvez choisir entre la sécurité automatisée et manuelle lors de la configuration d’un domaine authentifié. Si vous choisissez une sécurité automatisée, SendGrid gère automatiquement vos enregistrements DKIM et SPF. Lorsque vous ajoutez une nouvelle adresse IP d&#39;envoi dédiée à votre compte, SendGrid met immédiatement à jour vos paramètres DNS et votre signature DKIM. Si vous désactivez la sécurité automatisée, vous êtes responsable de la mise à jour de votre signature DKIM chaque fois que vous modifiez votre domaine d’envoi.

**Exemple de sécurité automatisée activée** :

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Exemple de sécurité automatisée désactivée** :

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

Une fois l’authentification de domaine configurée, SendGrid gère automatiquement les enregistrements SPF (Security Policy Framework) et DKIM pour vous. Une fois que SendGrid a fourni les enregistrements `CNAME` à ajouter à vos enregistrements DNS, vous pouvez ajouter des adresses IP dédiées et effectuer d&#39;autres mises à jour de compte sans avoir à gérer manuellement vos enregistrements SPF. Voir [Sécurité automatisée et votre signature DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Pour tester votre configuration DNS :

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Seuil d’e-mails transactionnels

Le seuil des e-mails transactionnels fait référence au nombre d’e-mails transactionnels que vous pouvez envoyer à partir d’environnements Pro au cours d’une période spécifique, par exemple 12 000 e-mails par mois provenant d’environnements hors production. Le seuil est conçu pour vous protéger contre l&#39;envoi de spam et les dommages potentiels à la réputation de vos emails.

Il n’existe aucune limite stricte au nombre d’e-mails pouvant être envoyés dans l’environnement de production, tant que le score de réputation de l’expéditeur est supérieur à 95 %. La réputation est affectée par le nombre d&#39;e-mails rejetés ou rejetés et si les registres de spam basés sur DNS ont signalé votre domaine comme source potentielle de spam. Voir [E-mails non envoyés lorsque les crédits SendGrid sont dépassés sur Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) dans la base de connaissances de la prise en charge de _Commerce_.

**Pour vérifier si le nombre maximal de crédits est dépassé** :

1. Sur votre station de travail locale, accédez au répertoire du projet.

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Recherchez des entrées de `authentication failed : Maxium credits exceeded` dans la `/var/log/mail.log`.

   Si vous voyez des entrées de journal `authentication failed` et que la **réputation d’envoi d’e-mail** est d’au moins 95, vous pouvez [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) pour demander une augmentation de l’allocation de crédit.

### Réputation d&#39;envoi des emails

Une réputation d&#39;envoi d&#39;email est un score attribué par un fournisseur d&#39;accès Internet (FAI) à une société qui envoie des emails. Plus le score est élevé, plus un FAI est susceptible de diffuser des messages dans la boîte de réception d’un destinataire. Si le score est inférieur à un certain niveau, le FAI peut acheminer les messages vers le dossier des courriers indésirables des destinataires, voire les rejeter complètement. Le score de réputation est déterminé par plusieurs facteurs, tels que la moyenne sur 30 jours de votre classement des adresses IP par rapport aux autres adresses IP et le taux de plaintes pour spam. Voir [8 méthodes pour vérifier la réputation de l’envoi d’e-mails](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).

### Listes de suppression des e-mails

Une liste de suppression des e-mails est une liste de destinataires auxquels les e-mails ne doivent pas être envoyés si cela devait nuire à votre réputation d&#39;envoi et à vos taux de diffusion. La Loi CAN-SPAM l&#39;exige pour s&#39;assurer que les expéditeurs de courriels disposent d&#39;une méthode de désinscription des destinataires qui se sont désabonnés ou qui ont marqué un courriel comme étant du pourriel. La liste de suppression collecte également les e-mails qui rebondissent, sont bloqués ou non valides.

Pour empêcher l&#39;envoi d&#39;e-mails vers le dossier des courriers indésirables, suivez l&#39;article Bonnes pratiques de Sendgrid, [Pourquoi mes e-mails vont-ils dans le courrier indésirable ?](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder).

Si certains destinataires ne reçoivent pas vos e-mails, vous pouvez [Envoyer un ticket d’assistance Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) pour demander une révision des listes de suppression et supprimer le(s) destinataire(s) si nécessaire.

Pour plus d’informations, voir [Qu’est-ce qu’une liste de suppression ?](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
