# Playbook de réponse aux incidents : réponse à la rançon pour RDS
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2024 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# Points de contact

Auteur : `Nom de l'auteur
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus de réponse aux attaques de rançon contre Amazon Relational Database Service (RDS)

Pour plus d'informations, veuillez consulter le [Guide de réponse aux incidents de sécurité AWS] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

### Objectifs
Tout au long de l'exécution du playbook, concentrez-vous sur les résultats recherchés***_, en prenant des notes pour améliorer les capacités de réponse aux incidents.

#### Déterminer :
* **Vulnérabilités exploitées**
* **Exploits et outils observés**
* **L' intention de l'acteur**
* **Attribution de l'acteur**
* **Dommages infligés à l'environnement et à l'entreprise**

#### Récupérer :
* **Retour à la configuration d'origine et durcie**

#### Améliorer les composants Perspectives de sécurité des FAC :
[Perspective de sécurité AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directive**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Étapes de réponse
1. [**PREPARATION**] Utilisez AWS Config pour vérifier la conformité de la configuration
2. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
3. [**CONFINEMENT**] Isoler immédiatement les ressources affectées
5. [**DÉTECTION ET ANALYSIS**] Utilisez les mesures CloudWatch pour déterminer si les données ont pu être exfiltrées
6. [**DÉTECTION ET ANALYSIS**] Utiliser VPCFlowLogs pour identifier les accès inappropriés à la base de données à partir d'adresses IP externes
7. [**RECOVERY**] Exécutez les procédures de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Rançon et destruction des données
* **Catégorie** : Attaque de rançon
* **Ressource** : RDS
* **Indicateurs** : Cyber Threat Intelligence, avis tiers, métriques Cloudwatch
* **Sources de journaux : RDS Database Log Files, S3 Access Logs, CloudTrail, CloudWatch, AWS Config
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

## Préparation
* Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
* AWS a développé un nouvel outil Open Source Self-Service Security Assessment (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS. compte.
* Maintenir un inventaire complet de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages réseau/fichiers et les machines de développement
* Envisagez d'utiliser [AWS Config] (https://aws.amazon.com/config/), un service qui vous permet d'évaluer, d'auditer et d'évaluer les configurations de vos ressources AWS
* Envisagez de mettre en œuvre [AWS GuardDuty] (https://aws.amazon.com/guardduty/) pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS, charges de travail et données stockées dans Amazon RDS
* [Exécutez votre instance DB dans un cloud privé virtuel (VPC)] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html) basé sur le service Amazon VPC pour obtenir le meilleur contrôle d'accès réseau possible
* Utilisez les stratégies AWS Identity and Access Management (IAM) pour attribuer des autorisations qui déterminent qui est autorisé à gérer les ressources Amazon RDS. Par exemple, vous pouvez utiliser IAM pour déterminer qui est autorisé à créer, décrire, modifier et supprimer des instances DB, baliser des ressources ou modifier des groupes de sécurité.
* Utilisez des groupes de sécurité pour contrôler quelles adresses IP ou instances Amazon EC2 peuvent se connecter à vos bases de données sur une instance DB. Lorsque vous créez une instance DB pour la première fois, son pare-feu empêche tout accès à la base de données, à l'exception des règles spécifiées par un groupe de sécurité associé.
* [Utiliser les connexions Secure Socket Layer (SSL) ou Transport Layer Security (TLS)] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html) avec des instances DB exécutant les moteurs de bases de données MySQL, MariaDB, PostgreSQL, Oracle ou Microsoft SQL Server
* [Utilisez le chiffrement Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) pour sécuriser vos instances DB et vos instantanés au repos. Le chiffrement Amazon RDS utilise l'algorithme de chiffrement AES-256 standard du secteur pour chiffrer vos données sur le serveur hébergeant votre instance DB
* [Utiliser le chiffrement réseau] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) et [chiffrement transparent des données] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html) avec les instances de bases de données Oracle
* Utilisez les fonctionnalités de sécurité de votre moteur de base de données pour contrôler qui peut se connecter aux bases de données sur une instance DB. Ces fonctionnalités fonctionnent comme si la base de données se trouvait sur votre réseau local.
* Il est fortement recommandé de disposer d'une solution HIDS (système de détection d'intrusion basée sur l'hôte) tiers (telle que OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/))
* Des références et des étapes supplémentaires sont disponibles dans [Security in Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)

### Utilisez AWS Config pour afficher la conformité de la configuration :
1. Connectez-vous à AWS Management Console et ouvrez la console AWS Config à l'adresse https://console.aws.amazon.com/config/
1. Dans le menu AWS Management Console, vérifiez que le sélecteur de région est défini sur une région prenant en charge les règles AWS Config. Pour obtenir la liste des régions prises en charge, consultez AWS Config Regions and Endpoints dans la référence générale Amazon Web Services
1. Dans le volet de navigation, choisissez Ressources. Sur la page Inventaire des ressources, vous pouvez filtrer par catégorie de ressources, par type de ressource et par statut de conformité. Choisissez Inclure les ressources supprimées le cas échéant. Le tableau affiche l'identificateur de ressource pour le type de ressource et l'état de conformité des ressources pour cette ressource. L'identificateur de ressource peut être un ID de ressource ou un nom de ressource
1. Choisissez une ressource dans la colonne Identificateur de ressource
1. Cliquez sur le bouton Chronologie des ressources. Vous pouvez filtrer par événements de configuration, événements de conformité ou événements CloudTrail
1. Concentrez-vous spécifiquement sur les événements suivants :
* rds-automatic-minor-version-upgrade-enabled activé
* rds-cluster-deletion-protection-enabled
* rds-cluster-iam-authentication-enabled
* rds-cluster-multi-az-enabled
* rds-enhanced-monitoring-enabled
* rds-instance-deletion-protection-enabled
* rds-instance-iam-authentication-enabled
* rds-instance-public-access-check
* rds-in-backup-plan
* rds-logging-enabled
* rds-multi-az-support
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted

## Procédures d'escalade
- « J'ai besoin d'une décision commerciale quant au moment où la criminalistique EC2 devrait être effectuée »
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Détection et analyse
* [AWS GuardDuty Machine Learning] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) peut détecter les comportements suspects, y compris la génération d'instantanés Amazon Relational Database Service (Amazon RDS) dans le cadre de tentatives d'exfiltration de données
* Consultez les journaux du système d'exploitation et des applications EC2 pour détecter les connexions inappropriées, l'installation d'un logiciel inconnu ou la présence de fichiers non reconnus

### [Utiliser les mesures CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
Recherchez des « pics » d'exfiltration de données. Il est possible qu'un attaquant ait détruit des données et laissé une note de rançon, et dans ce cas, il n'y a aucune possibilité de récupération de données en travaillant avec l'acteur malveillant.
1. Ouvrez la console CloudWatch à l'adresse https://console.aws.amazon.com/cloudwatch/
1. Dans le volet de navigation, choisissez Mesures, puis Toutes les mesures
1. Dans l'onglet Toutes les mesures, sélectionnez la région dans laquelle l'instance est déployée.
1. Dans l'onglet Toutes les mesures, entrez le terme de recherche « NetworkPacketsout » et appuyez sur Entrée
1. Sélectionnez l'un des résultats de votre recherche pour afficher les mesures
1. Pour représenter un graphique d'une ou de plusieurs mesures, cochez la case en regard de chaque mesure. Pour sélectionner toutes les mesures, cochez la case dans la ligne d'en-tête du tableau
1. (Facultatif) Pour modifier le type de graphique, choisissez Options de graphique. Vous pouvez ensuite choisir entre un graphique en courbes, un graphique en aires empilées, un graphique à barres, un graphique à secteurs ou un nombre
1. (Facultatif) Pour ajouter une bande de détection d'anomalies qui affiche les valeurs attendues pour la mesure, choisissez l'icône de détection des anomalies sous Actions en regard de la mesure

### Utiliser VPCFlowLogs pour identifier les accès inappropriés à la base de données à partir d'adresses IP externes
1. Utilisez [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) pour analyser les données du journal
1. Obtenir un résumé indiquant le nombre d'octets pour chaque quadruple src_ip, src_port, dst_ip, dst_port sur tous les enregistrements à destination ou à partir d'une adresse IP spécifique
```
SÉLECTIONNEZ l'adresse source, l'adresse de destination, le port source, le port de destination, la somme (nombres) en octe_count DEPUIS le flux vpcflow
OÙ (sourceaddress = '192.0.2.1' OU adresse de destination = '192.0.2.1')
ET date_partition >= '01/07/2020'
ET date_partition <= '31/07/2020'
AND account_partition = '111122223333'
AND region_partition dans ('us-east-1', 'us-east-2', 'us-west-2', 'us-west-2')
GROUP BY adresse source, adresse de destination, port source, port de destination
COMMANDE PAR BYTE_COUNT DESC
```
1. D'autres exemples de requêtes sont fournis dans le [vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql)


## Récupération
* Il est recommandé de ne pas payer la rançon
* Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
* Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis de données/réputation par rapport au paiement à l'attaquant
* Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
* Rendez-vous sur https://www.nomoreransom.org/ pour savoir si un déchiffrement est disponible pour la variante du logiciel malveillant infectée par vos données
* [Supprimer ou faire pivoter les clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [clés utilisateur Root] (https://console.aws.amazon.com/iam/home#security_credential) ; vous pouvez faire pivoter toutes les clés de votre compte si vous ne pouvez pas identifier une ou plusieurs clés spécifiques qui ont été exposées
* [Supprimer les utilisateurs IAM non autorisés] (https://console.aws.amazon.com/iam/home#users.)
* [Supprimer les stratégies non autorisées] (https://console.aws.amazon.com/iam/home#/policies)
* [Supprimer les rôles non autorisés] (https://console.aws.amazon.com/iam/home#/roles)
* [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM. REMARQUE : La suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution
* Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
* Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware
* Supprimer les instantanés publics ou les bases de données non reconnus ou non autorisés
* Supprimez la base de données compromise et créez de nouvelles bases de données RDS
* Identifiez les instances EC2 ayant un accès permissif à la ou aux bases de données et enquêtez-les également

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant qu'intervenant en cas d'incident, j'ai besoin d'un runbook pour effectuer EC2 Forensics
- En tant qu'intervenant en cas d'incident, j'ai besoin d'une décision commerciale quant au moment où la police judiciaire EC2 devrait être effectuée.
- En tant que répondeur d'incident, je dois activer la journalisation dans toutes les régions qui sont activées indépendamment de l'intention d'utilisation
- En tant que répondeur d'incident, je dois pouvoir détecter le crypto mining sur mes instances EC2 existantes

## Articles en arriéré actuel