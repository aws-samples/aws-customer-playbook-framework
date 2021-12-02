# Cahier d'intervention en cas d'incident : exposition aux ressources publiques - S3
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

## Points de contact

Auteur : `Nom de l'auteur
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus permettant d'identifier les propriétaires de ressources publiques, de déterminer qui a pu y accéder pendant qu'ils étaient exposés, de déterminer l'impact de la révocation de l'accès à la ressource et de déterminer la cause première de l'accessibilité publique.

## Indicateurs potentiels de compromis
- Avertissement d'accès public depuis le tableau de bord du service AWS
- CloudTrail `GetPublic Access Block`, `DeletePublic Access Block`, `GetObjectAcl`, `PutObjectAcl`
- Notification du chercheur en sécurité concernant l'accès public aux ressources
- Suppression de ressources de l'adresse IP (Public Internet Protocol)

## Constatations potentielles d'AWS GuardDuty
- Discovery:S3/MaliciousIPCaller
- Discovery:S3/MaliciousIPCaller.Custom
- Discovery:S3/TorIPCaller
- Exfiltration:S3/MaliciousIPCaller
- Exfiltration:S3/ObjectRead.Unusual
- Impact:S3/MaliciousIPCaller
- PenTest:S3/KaliLinux
- PenTest:S3/ParrotLinux
- PenTest:S3/PentooLinux
- Policy:S3/AccountBlockPublicAccessDisabled
- Policy:S3/BucketAnonymousAccessGranted
- Policy:S3/BucketBlockPublicAccessDisabled
- Policy:S3/BucketPublicAccessGranted
- Stealth:S3/ServerAccessLoggingDisabled
- UnauthorizedAccess:S3/MaliciousIPCaller.Custom
- UnauthorizedAccess:S3/TorIPCaller

### Objectifs
Tout au long de l'exécution du playbook, concentrez-vous sur les résultats souhaités***_, en prenant des notes pour améliorer les capacités de réponse aux incidents.

#### Déterminer :
* **Vulnérabilités exploitées**
* **Exploits et outils observés**
* **Intention de l'acteur**
* **Attribution de l'acteur**
* **Dommages infligés à l'environnement et à l'entreprise**

#### Récupérer :
* **Retour à la configuration d'origine et durcie**

#### Améliorer les composants Perspectives de sécurité des FAC :
[Perspective de sécurité AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directif**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Étapes de réponse
1. [**PREPARATION**] Créer un compte Inventaire des actifs
2. [**PREPARATION**] Créer un inventaire de compartiments S3
3. [**PREPARATION**] Activer la journalisation, le cas échéant
4. [**PREPARATION**] Identifiez quel type de données se trouve dans chaque compartiment
5. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
6. [**PREPARATION**] Mettre en œuvre une formation pour traiter les attaques DOS/DDoS
7. [**DÉTECTION ET ANALYSE**] Effectuer des vérifications de compartiment S3
8. [**DÉTECTION ET ANALYSIS**] Consulter CloudTrail : compartiment public S3
9. [**DÉTECTION ET ANALYSIS**] Examen de CloudTrail : objet S3 public
10. [**DÉTECTION ET ANALYSIS**] Consulter les VPC Flow Logs
11. [**DÉTECTION ET ANALYSIS**] Examen du point de termin/de l'hôte
12. [**CONTAINMENT**] S3 Bloquer l'accès public
13. [**ERADICATION**] S3 Supprimer les objets non reconnus/non autorisés
14. [**RECOVERY**] Effectuer les procédures de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : AWS Service Public Access
* **Catégorie** : Accès public
* **Ressource** : S3
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers, mesures Cloudwatch
* **Sources de journal** : journaux du serveur S3, S3 Access Logs, CloudTrail, CloudWatch
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
Identifiez toutes les ressources existantes et disposez d'une liste d'inventaire des actifs mise à jour, associée à la personne propriétaire de chacune d'elles.

#### Stock des godets S3
- Utilisez l'AWS API [list-buckets] (https://docs.aws.amazon.com/cli/latest/reference/s3api/list-buckets.html) pour afficher les noms de tous vos compartiments Amazon S3 (dans toutes les régions) : `compartiments de liste aws s3api —query « Buckets [] .Name"`

#### Activer la journalisation
- Vérifiez si la journalisation au niveau du serveur est activée dans CloudTrail dans les compartiments S3 : `. /rôdeur -c extra 718`
- Vérifiez si la journalisation au niveau objet est activée dans CloudTrail dans les compartiments S3 : `. /rôdeur -c extra 725`

#### Identifiez quel type de données se trouve dans chaque compartiment
**Option A**
- Pour voir tous les fichiers d'un compartiment S3, utilisez la commande suivante : `aws s3 ls s3 : //votre_bucket_name —recursive`
- **NOTE** Cet appel d'API est limité aux 1000 premières correspondances et n'inclut pas d'objets dépassant ce seuil
- Classer manuellement les compartiments et les objets importants pour l'entreprise
- Ajoutez des balises aux compartiments critiques afin que les métadonnées puissent exister à des fins de référence ultérieure

**Option B**
- [Amazon Macie] (https://aws.amazon.com/macie/) est un service de sécurité des données et de confidentialité des données entièrement géré qui utilise l'apprentissage automatique et la correspondance de modèles pour découvrir et protéger vos données sensibles dans AWS. Amazon Macie utilise l'apprentissage automatique et la correspondance de modèles pour découvrir les données sensibles à grande échelle de manière rentable.

### Formation
- `Quelle est la formation mise en place pour que les analystes de l'entreprise se familiarisent avec l'AWS API (environnement de ligne de commande), S3, RDS et d'autres services AWS ? `
>>>
Voici les possibilités de détection des menaces et de réponse aux incidents : \
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/) \
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Quels rôles peuvent apporter des modifications aux services de votre compte ? `
- `Quels utilisateurs ces rôles leur sont attribués ? Le moindre privilège est-il respecté ou existe-t-il des utilisateurs de super administrateurs ? `
- `Une évaluation de sécurité a-t-elle été effectuée sur votre environnement, avez-vous une base de référence connue pour détecter des choses « nouvelles » ou « suspectes » ? `

### Technologie de communication
- `Quelle technologie est utilisée au sein de l'équipe/de l'entreprise pour communiquer les problèmes ? Y a-t-il quelque chose d'automatisé ? `
>>>
Téléphone \
E-mail \
AWS SES \
AWS SNS \
Slack \
Carillon \
Autre ?
>>>

## Détection

### Vérifications de godets S3
- Assurez-vous que la journalisation de l'accès au compartiment S3 est activée sur le compartiment CloudTrail S3 : `. /prowler -c check 26`
- Assurez-vous qu'aucun compartiment S3 n'est ouvert à Tout le monde ou à n'importe quel utilisateur AWS : `. /rôdeur -c extra 73`
- Identifiez les ressources de votre organisation et de vos comptes, telles que des compartiments Amazon S3 ou des rôles IAM, qui sont partagées avec une entité externe : `. /rôdeur -c extra769`
- Trouvez des ressources exposées à Internet : `. /rôdeur -g groupe 17`

## Procédures d'escalade
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Analyse
Il est fortement recommandé d'exporter les journaux vers une solution de gestion des événements d'incidents de sécurité (SIEM) (telle que Splunk, ELK stack, etc.) pour faciliter l'affichage et l'analyse de divers journaux pour une analyse plus complète de la chronologie des attaques.

### CloudTrail : seau public S3
Par défaut, CloudTrail enregistre les appels d'API effectués au cours des 90 derniers jours, mais pas les requêtes de journalisation effectuées sur des objets. Vous pouvez voir les événements au niveau du compartiment sur la console CloudTrail. Toutefois, vous ne pouvez pas afficher les événements de données (appels au niveau objet Amazon S3). Vous devez analyser ou interroger les journaux CloudTrail pour eux.

1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez les journaux CloudTrail pour les noms d'événements `GetPublicAccessBlock` et `DeletePublicAccessBlock`

### CloudTrail : objet S3 public
Vous pouvez également obtenir des journaux CloudTrail pour les actions Amazon S3 au niveau de l'objet. Pour ce faire, activez les événements de données pour votre compartiment S3 ou tous les compartiments de votre compte. Lorsqu'une action au niveau de l'objet se produit dans votre compte, CloudTrail évalue vos paramètres de piste. Si l'événement correspond à l'objet que vous avez spécifié dans une piste, l'événement est enregistré.

1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez les journaux CloudTrail pour les noms d'événements `GetObjectAcl` et `PutObjectAcl`

### VPC Flow Logs
Les VPC Flow Logs sont une fonctionnalité qui vous permet de capturer des informations sur le trafic IP vers et depuis les interfaces réseau de votre VPC. Cela peut être utile pour les adresses IP découvertes dans CloudTrail afin de déterminer les types de connexions externes à toutes les ressources publiques.

Pour plus d'informations et des étapes, y compris les requêtes avec Athena, reportez-vous à la [Documentation AWS pour les VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Il est recommandé que l'analyse d'Athena soit incluse dans un livre de jeu distinct et liée à d'autres éléments relatifs.

### Endpoint /basé sur l'hôte
1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez CloudTrail pour les requêtes `PutObject` et `DeleteObject` provenant d'adresses IP publiques

- Consultez les journaux du système d'exploitation et des applications EC2 pour détecter les connexions inappropriées, l'installation de logiciels inconnus ou la présence de fichiers non reconnus.

- Il est fortement recommandé de disposer d'une solution HIDS (système de détection d'intrusion basée sur l'hôte) tiers (telle que OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), autre)

## Confinement

### S3 Bloquer l'accès public
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration « blockpublicACLS=True, IgnorePublicACLS=True, BlockPublicPolicy=True, RestrictPublicBuckets=True »

Vous pouvez également consulter [Blocage de l'accès public à votre stockage Amazon S3] (https://aws.amazon.com/s3/features/block-public-access/) pour plus de détails sur le blocage de l'accès public au S3 sur votre compte.

## Éradication

### S3 Supprimer les objets non reconnus/non autorisés
Supprimez tous les objets non reconnus des compartiments

1. Connectez-vous à AWS Management Console et ouvrez la console Amazon S3 à l'adresse https://console.aws.amazon.com/s3/
1. Dans la liste Nom du compartiment, choisissez le nom du compartiment dont vous souhaitez supprimer un objet.
1. Choisissez le nom de l'objet que vous souhaitez supprimer.
1. Pour supprimer la version actuelle de l'objet, choisissez Dernière version et choisissez l'icône de la corbeille.
1. Pour supprimer une version précédente de l'objet, choisissez Dernière version et choisissez l'icône de la corbeille à côté de la version que vous souhaitez supprimer.

## Récupération
Les mêmes procédures que celles énumérées pour l'éradication

## Actions préventives

### Vérifications de Prowler S3
**Cryptement**
- Vérifiez si le chiffrement par défaut (SSE) des compartiments S3 est activé : `. /rôdeur -c extra734`

**Reprise après sinistre**
- Vérifiez si le versionnement des objets est activé dans les compartiments S3 : `. /rôdeur -c extra763`

### Actions de service S3
[Empêcher les utilisateurs de modifier les paramètres S3 Block Public Access] (https://asecure.cloud/a/scp_s3_block_public_access/)

Consultez régulièrement l'accès au compartiment et les stratégies tous les mois et utilisez [CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) ou Security Hub pour les détections automatisées.

[Utilisation du versionnement dans les compartiments S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) pour atténuer la suppression accidentelle ou intentionnelle d'objets de premier niveau

[Gestion de l'accès avec ACL] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) pour limiter l'accès non autorisé aux ressources au niveau du compartiment et de l'objet

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) dispose de plusieurs règles automatisées pour protéger contre l'accès public, y compris [s3-bucket-level-public-access-prohibited] ( https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html).

### Posture générale de sécurité
Exécutez un [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contre l'environnement afin d'identifier davantage d'autres risques et potentiellement autres risques publics non identifiés dans ce manuel.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas besoin de « réparation », mais qui sont importants à savoir lorsque vous exécutez ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant que répondeur aux incidents, j'ai besoin d'un runbook sur la façon d'atténuer les ressources mal rendues publiques.
- En tant que répondeur aux incidents, je dois pouvoir détecter les ressources publiques (AMI, volumes EBS, Repos ECR, etc.)
- En tant que répondeur aux incidents, j'ai besoin de savoir quels rôles sont capables d'apporter des changements critiques au sein d'AWS.
- En tant qu'intervenant en cas d'incident, j'ai besoin d'un manuel sur l'atténuation de l'exposition d'un compartiment public et j'ai besoin de points d'escalade.
- En tant que répondeur d'incident, j'ai besoin de documentation sur les journaux requis pour différentes classifications de compartiments.

## Articles en arriéré actuel