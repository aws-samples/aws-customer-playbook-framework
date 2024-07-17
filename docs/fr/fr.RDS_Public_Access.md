# Playbook de réponse aux incidents : exposition aux ressources publiques - RDS
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2024 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

## Points de contact

Auteur : `Nom de l'auteur \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus permettant d'identifier les propriétaires de ressources publiques, de déterminer qui a pu y accéder pendant qu'ils étaient exposés, de déterminer l'impact de la révocation de l'accès à la ressource et de déterminer la cause première de l'accessibilité publique.

## Indicateurs potentiels de compromis
- Avertissement d'accès public depuis le tableau de bord du service AWS
- Nom de l'événement CloudTrail `PubliclyAccessible`
- Notification du chercheur en sécurité concernant l'accès public aux ressources
- Suppression de ressources de l'adresse IP (Public Internet Protocol)

### Objectifs
Tout au long de l'exécution du playbook, concentrez-vous sur les résultats recherchés***_, en prenant des notes pour améliorer les capacités de réponse aux incidents.

#### Déterminer :
* **Vulnérabilités exploitées**
* **Exploits et outils observés**
* **Intention de l'acteur**
* **Attribution de l'acteur**
* **Dommages infligés à l'environnement et à l'entreprise**

#### Récupérer :
* **Retour à la configuration d'origine et durcie**

#### Améliorer les composants Perspectives de sécurité des FAC :
[Perspective de sécurité AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directif**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Étapes de réponse
1. [**PREPARATION**] Créer un inventaire d'actifs
2. [**PREPARATION**] Créer un inventaire d'instance RDS
3. [**PREPARATION**] Établir des contrôles de sécurité et de journalisation RDS
4. [**PREPARATION**] Mettre en œuvre un programme de formation pour identifier et évaluer l'accès RDS et l'analyse des journaux
5. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade [**DÉTECTION ET ANALYS**]
6. [**DÉTECTION ET ANALYSIS**] Effectuer des vérifications d'instance
7. [**DÉTECTION ET ANALYSIS**] Consultez CloudTrail for RDS Public Resources
8. [**DÉTECTION ET ANALYSIS**] Consulter les VPC Flow Logs
9. [**DÉTECTION ET ANALYSIS**] Consulter les journaux basés sur les points de terminaison RDS/sur l'hôte
10. [**CONTAINMENT**] Contient une exposition publique RDS
11. [**ÉRADICATION**] Supprimer les instantanés publics ou les bases de données non reconnus ou non autorisés
12. [**PREPARATION**] Actions préventives supplémentaires : contrôles de sécurité RDS
13. [**PREPARATION**] Actions préventives supplémentaires : stratégie de contrôle de sécurité - chiffrement RDS
14. [**PREPARATION**] Actions préventives supplémentaires : AWS Config
15. [**PREPARATION**] Actions préventives supplémentaires : posture générale de sécurité

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : AWS Service Public Access
* **Catégorie** : Accès public
* **Ressource** : RDS
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers, mesures Cloudwatch
* **Sources de journaux : RDS Database Log Files, CloudTrail, CloudWatch
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

Cet outil fournit un instantané rapide de l'état actuel de la sécurité dans un environnement client. À titre de solution de rechange, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) permet une analyse automatisée des plaintes et peut [s'intégrer à Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventaire des actifs
Identifiez toutes les ressources existantes et disposez d'une liste d'inventaire des actifs mise à jour, couplée à la personne qui possède chacune

#### Inventaire des instances RDS
- Pour inventorier les instances RDS, utilisez l'AWS API [describe-db-instances] (https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html) pour répertorier les noms de toutes les instances d'une région donnée : `aws rds describe-db-instances —region us-east-1 —query 'DBInstances [*]. [Identificateur d'instance DB, identificateurs d'instance RepliCAD en lecture] '`

#### Vérifications de sécurité et de journalisation RDS
- Vérifiez si le stockage des instances RDS est chiffré : `. /rôdeur -c extra 735`
- Vérifiez si la sauvegarde des instances RDS est activée : `. /rôdeur -c extra739`
- Vérifiez si les instances RDS sont intégrées à CloudWatch Logs : `. /rôdeur -c extra747`
- Assurez-vous que la mise à niveau de version mineure est activée dans les instances RDS : `. /rôdeur -c extra 7131`
- Vérifiez si [surveillance améliorée] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html) est activée dans les instances RDS : `. /rôdeur -c extra 7132`
- Contrôles de sécurité RDS : `. /prowler -g group13`

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

### Vérifications d'instance RDS

#### AWS Config
AWS Config dispose de plusieurs [règles gérées pour évaluer vos instances RDS] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
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
* rds-resources-protected-by-backup-plan
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted
#### Prowler
- Assurez-vous qu'il n'y a pas d'instances RDS accessibles au public : `. /rôdeur -c extra 78`
- Vérifiez si les instantanés RDS et les instantanés de cluster sont publics : `. /rôdeur -c extra 723`
- Identifiez les ressources de votre organisation et de vos comptes, telles que des compartiments Amazon S3 ou des rôles IAM, qui sont partagées avec une entité externe : `. /rôdeur -c extra769`
- Trouvez des ressources exposées à Internet : `. /rôdeur -g groupe 17`

## Procédures d'escalade
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Analyse
Il est fortement recommandé d'exporter les journaux vers une solution de gestion des événements d'incidents de sécurité (SIEM) (telle que Splunk, ELK stack, etc.) pour faciliter l'affichage et l'analyse de divers journaux pour une analyse plus complète de la chronologie des attaques.

### CloudTrail : RDS Public
1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez les journaux CloudTrail pour le nom d'événement `ModifyDBInstance`, `ModifyDBSnapshotAttribute` ou `ModifyDBClusterSnapshotAttribute` et recherchez la valeur des événements `PubliclyAccessible` provenant d'adresses IP publiques

### VPC Flow Logs
Les VPC Flow Logs sont une fonctionnalité qui vous permet de capturer des informations sur le trafic IP vers et depuis les interfaces réseau de votre VPC. Cela peut être utile pour les adresses IP découvertes dans CloudTrail afin de déterminer les types de connexions externes à n'importe quelle ressource publique.

Pour plus d'informations et des étapes, y compris les requêtes avec Athena, reportez-vous à la [Documentation AWS pour les VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Il est recommandé que l'analyse d'Athena soit incluse dans un livret de jeu distinct et liée à d'autres éléments pertinents.

### Endpoint /basé sur l'hôte
- Consultez les journaux du système d'exploitation et des applications EC2 pour détecter les connexions inappropriées, l'installation d'un logiciel inconnu ou la présence de fichiers non reconnus.

- Il est fortement recommandé de disposer d'une solution HIDS (système de détection d'intrusion basée sur l'hôte) tiers (telle que OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), autre)

## Confinement

### Exposition publique RDS
Lorsque vous lancez une instance DB dans un VPC, l'instance DB dispose d'une adresse IP privée pour le trafic à l'intérieur du VPC. Cette adresse IP privée n'est pas accessible au public. Vous pouvez utiliser l'option Accès public pour indiquer si l'instance DB possède également une adresse IP publique en plus de l'adresse IP privée.

L'illustration suivante illustre l'option Accès public dans la section Configuration supplémentaire de la connectivité. Pour définir cette option, ouvrez la section Configuration supplémentaire de la connectivité dans la section Connectivité.

! [Image] (/images/VPC-example.png)

## Éradication

### Ressources RDS non autorisées/non reconnues
Supprimez tous les instantanés publics ou les bases de données non reconnus ou non autorisés

1. Connectez-vous à AWS Management Console et ouvrez la console Amazon RDS à l'adresse https://console.aws.amazon.com/rds/
1. Dans le volet de navigation, choisissez Instantanés.
1. La liste des instantanés manuels apparaît.
1. Choisissez l'instantané DB que vous souhaitez supprimer.
1. Pour Actions, choisissez Delete Snapshot.
1. Choisissez Supprimer sur la page de confirmation.

## Récupération
Les mêmes procédures que celles énumérées pour l'éradication

## Actions préventives

### Vérifications de sécurité RDS
- Contrôles de sécurité RDS : `. /prowler -g group13`

### Politique de contrôle de sécurité : chiffrement RDS
[Appliquer le chiffrement RDS obligatoire] (https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) dispose de plusieurs règles automatisées pour protéger contre l'accès public, y compris [rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)

### Posture générale de sécurité
Exécutez un [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contre l'environnement afin d'identifier davantage d'autres risques et éventuellement d'autres risques publics non identifiés dans ce playbook.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant que répondeur aux incidents, j'ai besoin d'un runbook sur la façon d'atténuer les ressources qui ne sont pas correctement rendues publiques
- En tant que répondeur aux incidents, je dois être capable de détecter les ressources publiques (AMI, volumes EBS, Repos ECR, etc.)
- En tant que répondeur aux incidents, j'ai besoin de savoir quels rôles sont capables d'apporter des changements critiques au sein d'AWS
- En tant que répondeur aux incidents, je dois m'assurer que tous les instantanés (RDS, EBS, ECR) nécessitent un chiffrement

## Articles en arriéré actuel