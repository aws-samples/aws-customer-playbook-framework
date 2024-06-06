Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2024 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# Points de contact

Auteur : `Nom de l'auteur »
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus permettant d'identifier les propriétaires des ressources de code, de déterminer comment ils ont été exposés, qui ont pu accéder au code exposé, déterminer l'impact de l'exposition, les actions de correction requises et déterminer la cause première de l'exposition au code.

## Indicateurs potentiels de compromis
- Alertes de prévention contre la perte de données (DLP)
- Publication ou exposition de code connue
- Journaux CloudTrail suspects

## Constatations potentielles d'AWS GuardDuty
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Exfiltration:S3/MaliciousIPCaller
- Exfiltration:S3/ObjectRead.Unusual
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- Persistence:IAMUser/AnomalousBehavior
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS

### Objectifs
Tout au long de l'exécution du playbook, concentrez-vous sur les résultats recherchés***_, en prenant des notes pour améliorer les capacités de réponse aux incidents.

#### Déterminer :
* **Copies de codes, transferts et publication**
* **Exposition des justificateurs**
* **Vulnérabilités exposées**
* **Intelligence environnementale exposée**
* **Outils utilisés**
* **Informations de configuration**
* **Intention de l'acteur**
* **Attribution de l'acteur**
* **Autres dommages infligés à l'environnement et à l'entreprise**
* **Risque créé pour l'environnement et les entreprises**

#### Récupérer :
* **Expire ou réinitialise toutes les informations d'identification d'authentification exposées**
* **Énumérer les risques associés aux vecteurs d'attaque potentiels en fonction de l'intelligence environnementale exposée**
* **Minimiser ou éliminer les risques liés au code exposé**

#### Améliorer les composants Perspectives de sécurité des FAC :
[Perspective de sécurité AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directive**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Étapes de réponse
1. [**PREPARATION**] Créer un inventaire des actifs de compte
2. [**PREPARATION**] Créer un inventaire de référentiel
3. [**PREPARATION**] Activer la journalisation, le cas échéant
4. [**PREPARATION**] Identifiez quel type de données se trouve dans chaque référentiel
5. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
6. [**PREPARATION**] Mettre en œuvre une formation pour lutter contre les attaques d'exfiltration
7. [**DÉTECTION ET ANALYSE**] Effectuer des contrôles d'exfiltration et de DLP
8. [**DÉTECTION ET ANALYSIS**] Révision des actions de lecture et d'écriture du référentiel (CodeCommit)
9. [**DÉTECTION ET ANALYSIS**] Consulter les journaux DNS
10. [**DÉTECTION ET ANALYSIS**] Consulter les journaux de flux VPC
11. [**DÉTECTION ET ANALYSIS**] Consulter les journaux basés sur les terminaux/sur l'hôte
12. [**CONTAINMENT**] Bloquer l'accès aux comptes affectés
13. [**ERADICATION**] Supprime les objets non reconnus et non autorisés dans les référentiels
14. [**RECOVERY**] Effectuer les procédures de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : exfiltration de données, abus de service AWS
* **Catégorie** : Perte de données
* **Ressource** : CodeCommit, S3, EC2
* **Indicateurs** : cyber-Threat Intelligence, avis tiers, CloudWatch Metrics
* **Sources de journal** : journaux DNS, journaux de VPC Flow Logs, CloudTrail, CloudWatch
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

## Préparation

Ce playbook fait référence et s'intègre, dans la mesure du possible, à [Prowler] (https://github.com/toniblyx/prowler) qui est un outil de ligne de commande qui vous aide dans l'évaluation de la sécurité AWS, l'audit, le renforcement et la réponse aux incidents.

Il suit les directives de la norme CIS Amazon Web Services Foundations Benchmark (49 contrôles) et comporte plus de 100 contrôles supplémentaires, notamment liés au RGPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 et autres.

Cet outil fournit un instantané rapide de l'état actuel de la sécurité dans un environnement client. En outre, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) permet une analyse automatisée de la conformité et peut [s'intégrer à Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventaire des actifs
Identifiez toutes les ressources existantes et disposez d'une liste d'inventaire des actifs mise à jour associée à la personne propriétaire de chaque ressource. Le code source peut être stocké dans l'une des ressources suivantes :
- (CodeCommit) [https://aws.amazon.com/codecommit/] référentiels
- stockage de code sauvegardé (S3) [https://aws.amazon.com/s3/]
- (EC2) [https://aws.amazon.com/ec2/] mécanismes de stockage de code auto-hébergés

#### Identifiez quelles données ont été exfiltrées de chaque actif
- Les journaux CloudWatch et CloudTrail stockés dans S3 peuvent être interrogés à l'aide de (Amazon Athena) [https://aws.amazon.com/athena/] pour identifier les actions indésirables
- (Amazon GuardDuty) [https://aws.amazon.com/guardduty/] peut détecter automatiquement le trafic anormal. Vous pouvez y accéder via (AWS Security Hub) [https://aws.amazon.com/security-hub/] ou (Amazon Detective) [https://aws.amazon.com/detective/]
- Les journaux d'applications et d'instances stockés dans S3 peuvent également être interrogés à l'aide d'Athena.

### Formation
- `Quelle est la formation mise en place pour que les analystes de l'entreprise se familiarisent avec l'AWS API (environnement de ligne de commande), CodeCommit, S3, RDS et d'autres services AWS ? `
>>>
Voici les possibilités de détection des menaces et de réponse aux incidents : \
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/) \
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Quels rôles peuvent apporter des modifications aux services de votre compte ? `
- `Quels utilisateurs ces rôles leur sont attribués ? Le moindre privilège est-il respecté ou existe-t-il des utilisateurs de super administrateurs ? `
- `Une évaluation de sécurité a-t-elle été effectuée par rapport à votre environnement ? Disposez-vous d'une base de référence connue pour détecter les choses « nouvelles » ou « suspectes » ? `

### Technologie de communication
- `Quelle technologie est utilisée au sein de l'équipe/de l'entreprise pour communiquer les problèmes ? Y a-t-il quelque chose d'automatisé ? `
>>>
Téléphone \
E-mail \
SMS \
AWS SES \
AWS SNS \
Slack \
Carillon \
Équipes \
Autre ?
>>>

### Implémentation DLP

La mise en œuvre d'une solution de prévention contre la perte de données (DLP) peut fournir des capacités de détection et des alertes supplémentaires. Une solution DLP peut offrir la plus grande valeur ajoutée dans les environnements EC2 ou évaluer le trafic réseau. Les solutions DLP peuvent être trouvées dans [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp).

## Détection

### Journalisation et vérifications du compartiment S3
- Assurez-vous que CloudTrail est activé dans toutes les régions : `. /prowler -c check21`
- Assurez-vous que la validation du fichier journal CloudTrail est activée : `. /prowler -c check22`
- Assurez-vous que la journalisation de l'accès au compartiment S3 est activée sur le compartiment CloudTrail S3 : `. /prowler -c check 26`
- Assurez-vous qu'aucun compartiment S3 n'est ouvert à tous ou à n'importe quel utilisateur AWS : `. /rôdeur -c extra 73`
- Identifiez les ressources de votre organisation et de vos comptes, telles que des compartiments Amazon S3 ou des rôles IAM, qui sont partagées avec une entité externe : `. /rôdeur -c extra769`
- Trouvez des ressources exposées à Internet : `. /rôdeur -g groupe 17`

### Journalisation et événements CodeCommit
- [Surveiller les événements AWS CodeCommit dans EventBridge] (https://docs.aws.amazon.com/codecommit/latest/userguide/monitoring-events.html), qui fournit un flux de données en temps réel. Ces événements sont les mêmes que ceux qui apparaissent dans Amazon CloudWatch Events, qui fournit un flux quasi en temps réel d'événements système décrivant les modifications apportées aux ressources AWS.
- [Créer une piste CloudTrail pour permettre la livraison continue des événements CodeCommit vers un compartiment S3] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html). CloudTrail capture tous les appels API pour CodeCommit en tant qu'événements.

### Alertes DLP

La mise en œuvre d'une solution DLP peut fournir des capacités de détection et des alertes supplémentaires. Les détails sont disponibles auprès du fournisseur de solutions DLP.

## Procédures d'escalade
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Analyse
Il est fortement recommandé d'exporter les journaux vers une solution de gestion des événements d'incident de sécurité (SIEM) (telle que Splunk, ELK stack, etc.) pour faciliter l'affichage et l'analyse de divers journaux pour une analyse plus complète de la chronologie des attaques.

### CloudTrail
CloudTrail fournit jusqu'à 90 jours de journaux d'événements pour tous les appels d'AWS API. Ces informations peuvent être utilisées pour identifier et suivre les actions malveillantes ou anormales. Plus d'informations sont disponibles dans le [CloudTrail Log Event Reference] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html).

### CloudTrail : seau public S3
Par défaut, CloudTrail enregistre les appels d'API effectués au cours des 90 derniers jours, mais pas les demandes de journalisation effectuées sur des objets. Vous pouvez voir les événements au niveau du compartiment sur la console CloudTrail. Toutefois, vous ne pouvez pas afficher les événements de données (appels de niveau objet Amazon S3) par défaut. Vous devez activer la journalisation au niveau de l'objet avant que ces événements apparaissent dans CloudTrail.

1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez les journaux CloudTrail pour les noms d'événements `GetPublicAccessBlock` et `DeletePublicAccessBlock`

### CloudTrail : objet S3 public
Vous pouvez également obtenir des journaux CloudTrail pour les actions Amazon S3 au niveau de l'objet. Pour ce faire, [activer les événements de données pour votre compartiment S3] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html) ou tous les compartiments de votre compte. Lorsqu'une action au niveau de l'objet se produit dans votre compte, CloudTrail évalue vos paramètres de piste. Si l'événement correspond à l'objet que vous avez spécifié dans une piste, l'événement est enregistré.

1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez les journaux CloudTrail pour les noms d'événements `GetObjectAcl` et `PutObjectAcl`

### VPC Flow Logs
Les VPC Flow Logs sont une fonctionnalité qui vous permet de capturer des informations sur le trafic IP vers et depuis les interfaces réseau de votre VPC. Cela peut être utile pour les adresses IP découvertes dans CloudTrail afin de déterminer les types de connexions externes à n'importe quelle ressource publique.

Pour plus d'informations et des étapes, y compris les requêtes avec Athena, reportez-vous à la [Documentation AWS pour les VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Il est recommandé que l'analyse d'Athena soit incluse dans un livre de jeu distinct et liée à d'autres éléments pertinents.

### Journaux DNS
Les journaux DNS sont une fonctionnalité qui vous permet de capturer des informations sur le trafic DNS à destination et en provenance des interfaces réseau de votre VPC. Cela peut être utile pour identifier des anomalies ou des domaines à haut risque.

### CloudWatch
Les données provenant d'instances EC2 et d'autres sources peuvent être [ingérées dans CloudWatch] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html). Ces données peuvent être utilisées pour déclencher des alarmes ou effectuer des analyses. CloudWatch fournit également la [détection des anomalies] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html) lorsque cette option est activée.

### Endpoint /basé sur l'hôte
1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez CloudTrail pour les requêtes `PutObject` et `DeleteObject` provenant d'adresses IP publiques

- Consultez les journaux du système d'exploitation et des applications EC2 pour détecter les connexions inappropriées, l'installation d'un logiciel inconnu ou la présence de fichiers non reconnus.

- Il est fortement recommandé de disposer d'une solution HIDS (système de détection d'intrusion basée sur l'hôte) tiers (telle que OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), autre)

### DLP

Les solutions DLP peuvent détecter et alerter telles que configurées. Les solutions DLP sont disponibles sur [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp).

## Confinement

### S3 Bloquer l'accès public
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration « BlockPublicAcls=true, IgnorePublicAcls=true, BlockPublicPolicy=true, RestrictPublicBuckets=true »

Vous pouvez également consulter [Blocage de l'accès public à votre stockage Amazon S3] (https://aws.amazon.com/s3/features/block-public-access/) pour plus de détails sur le blocage de l'accès public au S3 sur votre compte.

### CodeCommit

Audit le contrôle d'accès pour tous les utilisateurs avec le service [AWS Identity and Access Management (IAM) conjointement avec CodeCommit] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html).

Les autorisations peuvent également être modifiées ou restreintes avec la [référence des autorisations CodeCommit] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-permissions-reference.html).

## Éradication

### S3 Supprimer les objets non reconnus/non autorisés
Supprimez tous les objets non reconnus des compartiments

1. Connectez-vous à AWS Management Console et ouvrez la console Amazon S3 à l'adresse https://console.aws.amazon.com/s3/
1. Dans la liste Nom du compartiment, choisissez le nom du compartiment dont vous souhaitez supprimer un objet.
1. Choisissez le nom de l'objet que vous souhaitez supprimer.
1. Pour supprimer la version actuelle de l'objet, choisissez Dernière version et choisissez l'icône de la corbeille.
1. Pour supprimer une version précédente de l'objet, choisissez Dernière version et choisissez l'icône de la corbeille à côté de la version que vous souhaitez supprimer.

### AWS CodeCommit

[Audit les identités et l'accès à CodeCommit.] (https://docs.aws.amazon.com/codecommit/latest/userguide/security-iam.html)

### Amazon EC2

Dans la mesure du possible :

1. [Lancer une instance EC2 de remplacement] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-launch-snapshot.html) à l'aide de [Instantanés EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) ou [sauvegardes Amazon Machine Image (AMI)] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) créé à partir de la source
1. [Joindre un volume EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) de l'instance terminée à la nouvelle instance EC2.

## Récupération
Les mêmes procédures que celles énumérées pour l'éradication

## Actions préventives

### Vérifications de Prowler d'authentification
- Assurez-vous que l'authentification multifacteur (MFA) est activée :
`. /rôdeur -c check12`

### Vérifications de Prowler S3
**Chiffrement **
- Vérifiez si le chiffrement par défaut (SSE) des compartiments S3 est activé : `. /rôdeur -c extra734`

**Reprise après sinistre**
- Vérifiez si le versionnement des objets est activé dans les compartiments S3 : `. /rôdeur -c extra763`

### Actions de service S3
[Empêcher les utilisateurs de modifier les paramètres S3 Block Public Access] (https://asecure.cloud/a/scp_s3_block_public_access/)

Consultez régulièrement l'accès au compartiment et les stratégies tous les mois et utilisez [CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) ou Security Hub pour les détections automatisées

[Utilisation du versionnement dans les compartiments S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) pour atténuer la suppression accidentelle ou intentionnelle d'objets de premier niveau

[Gestion de l'accès avec ACL] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) pour limiter l'accès non autorisé aux ressources au niveau du compartiment et de l'objet

### Amazon EC2

[Utilisez l'authentification multifacteur (MFA) avec chaque compte.] (https://aws.amazon.com/iam/features/mfa/)

Utilisez TLS pour communiquer avec les ressources AWS. Nous recommandons TLS 1.2 ou version ultérieure. Certains services ont cette option activée par défaut, d'autres devront être implémentés (par exemple dans le [SDK JavaScript] (https://docs.aws.amazon.com/sdk-for-script/v2/developer-guide/enforcing-tls.html)).

[Configurer la journalisation de l'activité des API et des utilisateurs avec AWS CloudTrail.] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitor-with-cloudtrail.html)

[Utilisez les solutions de chiffrement AWS telles que KMS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html), ainsi que tous les contrôles de sécurité par défaut des services AWS.

[Activer les instantanés EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)

### AWS CodeCommit

[Utilisez l'authentification multifacteur (MFA) avec chaque compte.] (https://aws.amazon.com/iam/features/mfa/)

Utilisez TLS pour communiquer avec les ressources AWS. Nous recommandons TLS 1.2 ou version ultérieure. Certains services ont cette option activée par défaut, d'autres devront être implémentés (par exemple dans le [SDK JavaScript] (https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/enforcing-tls.html)).

[Configurer la journalisation de l'activité des API et des utilisateurs avec AWS CloudTrail.] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)

[Utilisez les solutions de chiffrement AWS telles que KMS] (https://docs.aws.amazon.com/codecommit/latest/userguide/encryption.html), ainsi que tous les contrôles de sécurité par défaut des services AWS.

[Utilisez des services de sécurité gérés avancés tels qu'Amazon Macie] (https://aws.amazon.com/blogs/compute/discovering-sensitive-data-in-aws-codecommit-with-aws-lambda-2/), qui aident à découvrir et à sécuriser les données personnelles stockées dans Amazon S3.

### Amazon Macie
[Amazon Macie] (https://aws.amazon.com/macie/) peut détecter les informations d'identification stockées, les clés privées et d'autres données d'accès en [utilisant des identifiants de données gérés] (https://docs.aws.amazon.com/macie/latest/user/managed-data-identifiers.html).

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) dispose de plusieurs règles automatisées pour gérer l'exposition au code, y compris [codebuild-project-envvar-awscred-check] (https://docs.aws.amazon.com/config/latest/developerguide/codebuild-project-envvar-awscred-check.html) pour vérifiez si les informations d'identification sont stockées dans le code.

### Posture générale de sécurité
Exécutez un [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contre l'environnement afin d'identifier davantage d'autres risques et éventuellement d'autres risques publics non identifiés dans ce playbook.

### Implémentation DLP

La mise en œuvre d'une solution de prévention contre la perte de données (DLP) peut fournir des capacités de détection et des alertes supplémentaires. Les solutions DLP se trouvent dans [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) et doivent être configurées comme prescrit.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas besoin de « réparation », mais qui sont importants à savoir lorsque vous exécutez ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant que répondant aux incidents, j'ai besoin d'un runbook sur la façon de détecter l'exposition au code
- En tant que répondant aux incidents, j'ai besoin d'un runbook sur la façon de détecter l'exfiltration de code
- En tant que répondant aux incidents, je dois pouvoir détecter les ressources publiques (AMI, volumes EBS, Repos ECR, etc.)
- En tant que répondant aux incidents, j'ai besoin de savoir quels rôles sont capables d'apporter des changements critiques au sein d'AWS
- En tant que répondant aux incidents, j'ai besoin d'un manuel sur l'atténuation de l'exposition à un code et j'ai besoin de points d'escalade.
- En tant que répondant aux incidents, j'ai besoin de documentation sur les journaux requis pour différentes classifications de données

## Articles en arriéré actuel