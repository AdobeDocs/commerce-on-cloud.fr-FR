---
title: Sécurité des infrastructures cloud
description: Découvrez comment Adobe assure la sécurité d’Adobe Commerce sur les infrastructures cloud.
feature: Cloud, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# Sécurité

L’architecture du plan Adobe Commerce [Pro](pro-architecture.md) est conçue pour fournir un environnement hautement sécurisé. Chaque client est déployé dans son propre environnement de serveur isolé, séparé des autres clients. Les détails de sécurité de l’environnement de production sont décrits ci-dessous.

## Navigateurs web

La majeure partie du trafic entrant et sortant de l’environnement cloud provient des navigateurs web des consommateurs. Le trafic des clients peut être sécurisé à l’aide de HTTPS pour toutes les pages du site web (à l’aide d’une certification SSL partagée ou du propre certificat SSL du client, moyennant des frais supplémentaires). Les pages de passage en caisse et de compte sont toujours diffusées en HTTPS. La bonne pratique consiste à diffuser toutes les pages sous HTTPS.

## Réseau de diffusion de contenu

Fastly fournit un réseau de diffusion de contenu (CDN) et une protection contre le déni de service distribué (DDoS). Le réseau CDN Fastly permet d’isoler l’accès direct aux serveurs d’origine. Le DNS public pointe uniquement vers le Fast Network. La solution Fastly DDoS protège contre les attaques de couches 3 et 4 très perturbatrices, et les attaques de couches 7 plus complexes. Les attaques de couche 7 peuvent être bloquées à l’aide de règles personnalisées basées sur l’ensemble des requêtes HTTP/HTTPS et sur les critères du client et de la requête, y compris les en-têtes, les cookies, le chemin de requête et l’adresse IP du client, ou des indicateurs tels que la géolocalisation.

Voir [Présentation des services Fastly](../cdn/fastly.md).

## Pare-feu d’application web

Le pare-feu d’application web Fastly (WAF) est utilisé pour fournir une protection supplémentaire. Le WAF cloud de Fastly utilise des règles tierces issues de sources commerciales et open source telles que l’ensemble de règles principal OWASP. En outre, des règles spécifiques à Adobe Commerce sont utilisées. Les clients sont protégés contre les principales attaques de la couche applicative, notamment les attaques par injection et les entrées malveillantes, le cross-site scripting, l’exfiltration de données, les violations de protocole HTTP et d’autres menaces figurant dans le top dix d’OWASP.

Les règles WAF sont mises à jour par Adobe Commerce si de nouvelles vulnérabilités sont détectées, ce qui permet à Managed Services de corriger virtuellement les problèmes de sécurité avant d’appliquer les correctifs logiciels. Fastly WAF ne fournit pas de services de limitation de débit ni de détection de robots. Si vous le souhaitez, les clients peuvent obtenir une licence pour un service de détection de robots tiers compatible avec Fastly.

Voir [&#x200B; Pare-feu d’application web (WAF)](../cdn/fastly-waf-service.md).

## Cloud privé virtuel

L’environnement de production du plan Adobe Commerce Pro est configuré en tant que cloud privé virtuel (VPC) afin que les serveurs de production soient isolés et aient une capacité limitée à se connecter à l’environnement cloud et à en sortir. Seules les connexions sécurisées aux serveurs cloud sont autorisées. Des protocoles sécurisés tels que SFTP ou rsync peuvent être utilisés pour les transferts de fichiers.

Les clients peuvent utiliser des tunnels SSH pour sécuriser les communications avec l’application. L’accès à AWS PrivateLink peut être fourni moyennant des frais supplémentaires. Toutes les connexions à ces serveurs sont contrôlées à l’aide des groupes de sécurité AWS, un pare-feu virtuel qui limite les connexions à l’environnement. Les ressources techniques des clients peuvent accéder à ces serveurs à l’aide de SSH.

## Chiffrement

Amazon Elastic Block Store (EBS) est utilisé pour le stockage. Tous les volumes EBS sont chiffrés à l&#39;aide de l&#39;algorithme AES-256, ce qui signifie que les données sont chiffrées au repos. Le système chiffre également les données en transit entre le réseau CDN et l’origine, et entre les serveurs d’origine. Les mots de passe client sont stockés sous forme de hachages. Les informations d’identification sensibles, y compris les informations d’identification de la passerelle de paiement, sont chiffrées à l’aide de l’algorithme SHA-256.

L’application Adobe Commerce ne prend pas en charge le chiffrement au niveau des colonnes ou des lignes lorsque les données ne sont pas au repos ou ne transitent pas entre les serveurs. Le client peut gérer les clés de chiffrement à partir de l’application. Les clés utilisées par le système sont stockées dans le système de gestion des clés d’AWS et doivent être gérées par Managed Services pour fournir des parties du service.

## Détection des points d’entrée et réponse

[!DNL CrowdStrike Falcon], un agent léger de nouvelle génération de détection des points d’entrée et de réponse (EDR) est installé sur tous les points d’entrée (serveurs inclus) d’Adobe. Les agents EDR protègent les données et les systèmes des Adobes grâce à une surveillance et à une collecte continues en temps réel, qui permettent d&#39;identifier et de réagir rapidement aux menaces.

## Test de pénétration

Managed Services effectue régulièrement des tests de pénétration du système cloud Adobe Commerce avec l’application prête à l’emploi. Les clients sont responsables de tout test de pénétration de leur application personnalisée.

## Passerelle de paiement

Adobe Commerce nécessite des intégrations de passerelle de paiement où les données de carte de crédit sont transmises directement du navigateur du client à la passerelle de paiement. Les données de la carte ne sont jamais disponibles dans les environnements Managed Services du plan Adobe Commerce Pro. Les actions sur les transactions effectuées par l’application d’e-commerce sont effectuées à l’aide d’une référence à la transaction provenant de la passerelle.

## Application Adobe Commerce

Adobe teste régulièrement le code de l’application principale pour détecter les vulnérabilités de sécurité. Des correctifs pour les défauts et les problèmes de sécurité sont fournis aux clients. L’équipe de sécurité des produits valide les produits Adobe Commerce en suivant les directives de sécurité des applications OWASP. Plusieurs outils d&#39;évaluation des vulnérabilités de sécurité et fournisseurs externes sont utilisés pour tester et vérifier la conformité. Les outils de sécurité incluent :

- Analyse statique et dynamique Veracode
- Analyse du code source RIPS
- Services d&#39;analyse de vulnérabilité de Trustwave et d&#39;Alert Logic
- Burp Suite Pro
- OWASPZAP
- etSqlMap

La base de code complète est analysée deux fois par semaine à l’aide de ces outils. Les clients sont informés des correctifs de sécurité par le biais d’e-mails directs, de notifications dans l’application et dans le [Centre de sécurité](https://helpx.adobe.com/fr/security.html).

Les clients doivent s’assurer que ces correctifs sont appliqués à leur application personnalisée dans les 30 jours suivant leur publication, conformément aux directives PCI. Adobe propose également un [outil de numérisation de sécurité](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/security/security-scan) qui permet aux commerçants de surveiller régulièrement leurs sites et de recevoir des mises à jour sur les risques de sécurité connus, les logiciels malveillants et les accès non autorisés. L’outil Security Scan Tool est un service gratuit et peut être exécuté dans n’importe quelle version d’Adobe Commerce.

Pour encourager les chercheurs en sécurité à identifier et à signaler les vulnérabilités, Adobe Commerce dispose d’un programme [&#x200B; bug-bounty](https://hackerone.com/magento) en plus des tests internes. En outre, le client reçoit le code source complet de l’application pour sa propre révision, si désiré.

## Système de fichiers en lecture seule

Tout le code exécutable est déployé dans une image de système de fichiers en lecture seule, ce qui réduit considérablement les surfaces disponibles pour les attaques. Le processus de déploiement crée une image Squash-FS pour réduire les possibilités d’injection de code PHP ou JavaScript dans le système ou de modification des fichiers d’application Adobe Commerce.

## Déploiement à distance

La seule façon d’obtenir du code exécutable dans l’environnement de production Managed Services consiste à l’exécuter via un processus d’approvisionnement. La mise en service consiste à transférer le code source de votre référentiel source vers un référentiel distant qui lance un processus de déploiement. L’accès à cette cible de déploiement est contrôlé, ce qui vous permet de contrôler entièrement qui peut y accéder. Tous les déploiements de code d’application dans l’environnement hors production sont contrôlés par le client.

## Journalisation

Toutes les activités AWS sont consignées dans AWS CloudTrail. Les journaux du système d’exploitation, du serveur d’applications et de la base de données sont stockés sur les serveurs de production et stockés dans des sauvegardes. Toutes les modifications du code source sont enregistrées dans un référentiel Git. L’historique des déploiements est disponible dans Adobe Commerce [interface web du projet](../project/overview.md). Tous les accès à l’assistance sont consignés et les sessions d’assistance sont enregistrées.

Voir [Afficher et gérer les journaux](../test/log-locations.md).

## Données sensibles

Les données sensibles peuvent couvrir soit les informations personnelles des clients, soit les données confidentielles des clients Managed Services. La protection des données sensibles des clients et des consommateurs est une obligation essentielle pour Adobe Commerce Managed Services. Les clients Managed Services et Adobe ont des obligations légales concernant les informations d’identification personnelle. Outre les fonctions de sécurité de l’architecture, d’autres contrôles permettent de limiter la distribution et l’accès aux données sensibles.

Les clients sont propriétaires de leurs données et contrôlent où elles se trouvent. Le client indique l’emplacement où se trouvent ses instances de production et de développement. Ils indiquent également l’emplacement utilisé pour l’environnement de création de rapports d’Adobe Commerce avec Commerce et si cette application de création de rapports d’Adobe Commerce a accès ou non aux informations d’identification personnelle. Les instances de production se trouvent dans la plupart des régions d’AWS, tandis que les environnements de développement et de création de rapports d’Adobe Commerce se trouvent actuellement aux États-Unis ou dans l’Union européenne.

Les données sensibles peuvent transiter par le réseau de serveurs Fast CDN, mais ne sont pas stockées dans le réseau Fastly. Tous les partenaires inclus dans l’offre Managed Services ont des obligations contractuelles visant à assurer la protection des données sensibles. Managed Services ne déplace pas les données client ou consommateur sensibles depuis les emplacements spécifiés par le client.

Dans le cadre de la prestation des services inclus dans l’offre Managed Services, le personnel de Managed Services a besoin d’accéder aux systèmes de production qui contiennent des données sensibles. Ces employés sont formés à leurs obligations de protection des données sensibles des clients et des consommateurs. L&#39;accès à ces systèmes est limité aux employés qui en ont besoin. Ces employés n&#39;y ont accès que pour le temps nécessaire à la prestation de ces services.

## Règlement général sur la protection des données

Le Règlement général sur la protection des données (RGPD) est un cadre juridique qui définit des directives pour la collecte et le traitement des informations personnelles des personnes dans l’Union européenne (UE). Ces réglementations s’appliquent quel que soit l’endroit où se trouve le site et les visiteurs de l’UE y ont accès.

Les visiteurs doivent être informés des données qu’un site collecte auprès d’eux et consentir explicitement à la collecte d’informations. Les sites doivent avertir les visiteurs en cas de violation des données personnelles détenues par le site.

Le commerçant ou la société exploitant le site doit disposer d&#39;un délégué à la protection des données qui supervise la sécurité des données du site. Le responsable de la confidentialité des données (ou l’équipe de gestion du site web) doit être disponible pour être contacté si un visiteur demande que ses données soient effacées.

Le RGPD exige que toutes les informations d’identification personnelle (telles que les noms, la race et la date de naissance) collectées soient rendues anonymes ou pseudonymes.

>[!NOTE]
>
>Cette page donne un aperçu général des éléments à prendre en compte pour le RGPD. Consultez le _[Guide de sécurité et de conformité &#x200B;](https://experienceleague.adobe.com/fr/docs/commerce-operations/security-and-compliance/overview)_ pour plus d’informations sur la manière dont Adobe Commerce stocke les informations personnelles. Pour déterminer comment votre entreprise devrait se conformer aux obligations légales, consultez votre conseiller juridique ou reportez-vous au [texte officiel](https://eur-lex.europa.eu/eli/reg/2016/679/oj).

## Sauvegardes

Des sauvegardes sont effectuées toutes les heures pendant les dernières 24 heures de fonctionnement. Après cette période de 24 heures, les sauvegardes sont conservées selon un planning défini à l’aide du service AWS EBS Snapshot. Voir [Politique de rétention](pro-architecture.md#retention-policy).

Le service crée une sauvegarde indépendante sur le stockage redondant. Les volumes EBS étant chiffrés, les sauvegardes le sont également. En outre, Managed Services effectue des sauvegardes à la demande sur demande des clients.

Votre approche de sauvegarde et de récupération Managed Services utilise une architecture de haute disponibilité associée à des sauvegardes système complètes. Chaque projet est répliqué (l’ensemble des données, du code et des ressources) dans trois zones de disponibilité AWS distinctes ; chaque zone dispose d’un centre de données distinct.

Voir [Snapshots et gestion des sauvegardes](../storage/snapshots.md).
