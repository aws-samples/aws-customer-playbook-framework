# Playbook de réponse aux incidents : modifications réseau non autorisées
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2024 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# Points de contact

Auteur : `Nom de l'auteur
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus de réponse aux modifications non autorisées ou suspectes apportées aux ressources réseau dans un environnement AWS.

## Indicateurs potentiels de compromis
- Ressources de service nouvelles ou non reconnues
- Ressources non reconnues ou non autorisées (par exemple EC2, Lambda)
- Augmentations inhabituelles de la facturation
- Notification d'un chercheur en sécurité
- Notification indiquant que mes ressources ou mon compte AWS peuvent être compromis

## Constatations potentielles d'AWS GuardDuty
- Backdoor:EC2/C&CActivity.B
- Backdoor:EC2/C&CActivity.B ! DNS
- Backdoor:EC2/Spambot
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual
- CryptoCurrency:EC2/BitcoinTool.B
- CryptoCurrency:EC2/BitcoinTool.B ! DNS
- Impact:EC2/AbusedDomainRequest.Reputation
- Impact:EC2/BitcoinDomainRequest.Reputation
- Impact:EC2/MaliciousDomainRequest.Reputation
- Impact:EC2/SuspiciousDomainRequest.Reputation
- Impact:EC2/WinRMBruteForce
- Recon:EC2/PortProbeEMRUnprotectedPort
- Recon:EC2/PortProbeUnprotectedPort
- Trojan:EC2/BlackholeTraffic
- Trojan:EC2/BlackholeTraffic ! DNS
- Trojan:EC2/DGADomainRequest.B
- Trojan : EC2/DGADOMAIN Request.c ! DNS
- Trojan:EC2/DNSDataExfiltration
- Trojan : EC2/Drive by Source Traffic ! DNS
- Trojan:EC2/DropPoint
- Trojan:EC2/DropPoint ! DNS
- Trojan : demande de domaine EC2/hameçonnage ! DNS
- UnauthorizedAccess:EC2/MaliciousIPCaller.Custom
- UnauthorizedAccess:EC2/MetadataDNSRebind
- UnauthorizedAccess:EC2/RDPBruteForce
- UnauthorizedAccess:EC2/SSHBruteForce
- UnauthorizedAccess:EC2/TorClient
- UnauthorizedAccess:EC2/TorRelay
- Discovery:S3/MaliciousIPCaller
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- Persistence:IAMUser/AnomalousBehavior
- Policy:IAMUser/RootCredentialUsage
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- Recon:IAMUser/MaliciousIPCaller
- Recon:IAMUser/MaliciousIPCaller.Custom
- Recon:IAMUser/TorIPCaller
- Stealth:IAMUser/CloudTrailLoggingDisabled
- Stealth:IAMUser/PasswordPolicyChange
- UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration
- UnauthorizedAccess:IAMUser/MaliciousIPCaller
- UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom
- UnauthorizedAccess:IAMUser/TorIPCaller

### Objectifs
Tout au long de l'exécution du playbook, concentrez-vous sur les résultats recherchés***_, en prenant des notes pour améliorer les capacités de réponse aux incidents.

#### Déterminer :
* **Vulnérabilités exploitées**
* **Exploits et outils observés**
* **Intention de l'acteur**
* **Attribution de l'acteur**
* **Dommages infligés à l'environnement et aux entreprises**

#### Récupérer :
* **Retour à la configuration d'origine et durcie**

#### Améliorer les composants Perspectives de sécurité des FAC :
[Perspective de sécurité AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directive**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Étapes de réponse
1. [**PREPARATION**] Inventaire des actifs publiquement exposés
2. [**PREPARATION**] Journalisation de l'installation
3. [**PREPARATION**] Mettre en œuvre un programme de formation pour identifier et évaluer les capacités de criminalistique dans le cloud
4. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
5. [**DÉTECTION ET ANALYSIS**] Passez en revue CloudTrail
6. [**DÉTECTION ET ANALYS**] Revoir CloudWatch
7. [**DÉTECTION ET ANALYSIS**] Utilisez AWS Config pour vérifier la configuration d'un groupe de sécurité
8. [**DÉTECTION ET ANALYSE**] Affichage des VPC Flow Logs dans la console
9. [**DÉTECTION ET ANALYSIS**] Interrogation des journaux de flux à l'aide d'Amazon Athena
10. [**CONTAINMENT**] Identifiez quel utilisateur ou quel rôle a exécuté les modifications réseau non autorisées
11. [**ERADICATION**] Passez en revue les résultats de la revue de l'historique des événements CloudTrail pour l'activité par la clé d'accès compromise
12. [**ÉRADICATION**] Passez en revue le document Éviter les frais inattendus
13. [**RECOVERY**] Réinitialiser les modifications apportées aux ressources réseau
14. [**PREPARATION**] Actions préventives supplémentaires : AWS Config et AWS System Manager
15. [**PREPARATION**] Actions préventives supplémentaires : AWS Security Hub
16. [**PREPARATION**] Actions préventives supplémentaires : posture générale de sécurité

***Les étapes de réponse suivent le cycle de vie de la réponse aux incidents de la publication spéciale 800-61r2 du NIST
[Guide de gestion des incidents de sécurité informatique NIST] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
! [Image] (images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Outils : Forensics
* **Catégorie** : Forensics
* **Ressource** : EC2, VPC
* **Indicateurs** : cyber-Threat Intelligence, avis tiers, mesures Cloudwatch
* **Sources du journal : Endpoint
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie cloud

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

### Inventaire des actifs publiquement exposés
Vous pouvez utiliser le [tableau de bord du VPC] (https://console.aws.amazon.com/vpc/home) et le [Tableau de bord Network Manager] (https://console.aws.amazon.com/networkmanager/home) qui vous permettent de visualiser et de surveiller vos ressources réseau. Le tableau de bord Network Manager inclut des informations sur les ressources de votre réseau mondial, leur emplacement géographique, la topologie du réseau, les mesures et événements Amazon CloudWatch, et vous permet d'effectuer une analyse de routage.

Vous devez tenir à jour un inventaire et surveiller les modifications apportées aux ressources suivantes :
- Clouds privés virtuels (VPC)
- Listes de contrôle d'accès réseau (NACL) VPC
- Groupes de sécurité VPC
- Tables de routage VPC
- Interfaces réseau Elastic VPC (ENI)
- Passerelles Internet VPC
- Connexions d'appairage VPC
- Passerelles NAT VPC
- Points de terminaison VPC
- Passerelles client VPN
- Adresses IP Elastic (EIP) VPC

Prowler peut également vous aider à trouver de nombreuses ressources exposées à Internet : `. /rôdeur -g groupe 17`

### Journalisation de l'installation
**Journaux de flux :**
- Les journaux de flux capturent des informations sur le trafic IP vers et depuis les interfaces réseau de votre VPC.
- Vous pouvez créer un journal de flux pour un VPC, un sous-réseau ou une interface réseau individuelle.
- Les données du journal de flux sont publiées dans CloudWatch Logs ou Amazon S3, et peuvent vous aider à diagnostiquer des règles de groupe de sécurité et d'ACL réseau trop restrictives ou trop permissives.

**Pour créer un journal de flux pour une interface réseau à l'aide de la console**
1. Ouvrez la console Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. Dans le volet de navigation, choisissez Interfaces réseau.
1. Cochez les cases d'une ou de plusieurs interfaces réseau et choisissez Actions, Create flow log.
1. Pour Filter, spécifiez le type de trafic à consigner.
- Choisissez Tout pour consigner le trafic accepté et rejeté, Rejeter pour consigner uniquement le trafic rejeté ou Accepter pour consigner uniquement le trafic accepté.
1. Pour Intervalle d'agrégation maximal, choisissez la période maximale pendant laquelle un flux est capturé et agrégé dans un enregistrement de journal de flux.
1. Pour Destination, choisissez Send to an Amazon S3 bucket.
1. Pour l'S3 bucket ARN, spécifiez l'Amazon Resource Name (ARN) d'un compartiment Amazon S3 existant.
- Vous pouvez inclure un sous-dossier dans l'ARN du compartiment. Le compartiment ne peut pas utiliser AWSLogs comme nom de sous-dossier, car il s'agit d'un terme réservé.
- Par exemple, pour spécifier un sous-dossier nommé my-logs dans un compartiment nommé my-bucket, utilisez l'ARN suivant : `arn:aws:s3። :my-bucket/my-logs/`
- Si vous êtes propriétaire du compartiment, nous créons automatiquement une stratégie de ressources et nous l'attachons au compartiment. Pour plus d'informations, consultez Autorisations du compartiment Amazon S3 pour les journaux de flux.
1. Pour Format d'enregistrement de journal, sélectionnez le format de l'enregistrement de journal de flux.
* Pour utiliser le format par défaut, choisissez le AWS default format.
* Pour utiliser un format personnalisé, choisissez Format personnalisé, puis sélectionnez des champs dans Format journal.
1. Choisissez Create flow log.

**Pour créer un journal de flux pour un VPC ou un sous-réseau à l'aide de la console**
1. Ouvrez Amazon [console VPC] (https://console.aws.amazon.com/vpc/)
1. Dans le volet de navigation, choisissez Your VPC ou Subnets.
1. Activez la case à cocher pour un ou plusieurs VPC ou sous-réseaux, puis choisissez Actions, Create flow log.
1. Pour Filter, spécifiez le type de trafic à consigner.
- Choisissez Tout pour consigner le trafic accepté et rejeté, Rejeter pour consigner uniquement le trafic rejeté ou Accepter pour consigner uniquement le trafic accepté.
1. Pour Intervalle d'agrégation maximal, choisissez la période maximale pendant laquelle un flux est capturé et agrégé dans un enregistrement de journal de flux.
1. Pour Destination, choisissez Send to an Amazon S3 bucket.
1. Pour l'S3 bucket ARN, spécifiez l'Amazon Resource Name (ARN) d'un compartiment Amazon S3 existant.
- Vous pouvez inclure un sous-dossier dans l'ARN du compartiment. Le compartiment ne peut pas utiliser AWSLogs comme nom de sous-dossier, car il s'agit d'un terme réservé.
- Par exemple, pour spécifier un sous-dossier nommé my-logs dans un compartiment nommé my-bucket, utilisez l'ARN suivant : `arn:aws:s3። :my-bucket/my-logs/`
- Si vous êtes propriétaire du compartiment, nous créons automatiquement une stratégie de ressources et nous l'attachons au compartiment. Pour plus d'informations, consultez Autorisations du compartiment Amazon S3 pour les journaux de flux.
1. Pour Format d'enregistrement de journal, sélectionnez le format de l'enregistrement de journal de flux.
* Pour utiliser le format par défaut, choisissez le AWS default format.
* Pour utiliser un format personnalisé, choisissez Format personnalisé, puis sélectionnez des champs dans Format journal.
1. Choisissez Create flow log.

**Surveillance des passerelles NAT :** Vous pouvez surveiller votre passerelle NAT à l'aide de CloudWatch, qui collecte des informations de votre passerelle NAT et crée des mesures lisibles et quasi en temps réel.
1. Les mesures de passerelle NAT sont envoyées à CloudWatch à intervalles d'une minute.
1. Vous pouvez afficher les mesures de vos passerelles NAT comme suit.
* Ouvrez la console CloudWatch à l'adresse https://console.aws.amazon.com/cloudwatch/
* Dans le volet de navigation, choisissez Metrics.
* Sous Toutes les mesures, choisissez l'espace de noms des mesures de passerelle NAT.
* Pour afficher les mesures, sélectionnez la dimension de mesure.
1. Pour créer une alarme pour le trafic sortant via la passerelle NAT, procédez comme suit :
* Ouvrez la console CloudWatch à l'adresse https://console.aws.amazon.com/cloudwatch/
* Dans le volet de navigation, choisissez Alarmes, Create Alarm.
* Choisissez la passerelle NAT.
* Sélectionnez la passerelle NAT et la mesure BytesOutToDestination, puis choisissez Suivant.
* Configurez l'alarme comme suit, puis choisissez Créer une alarme lorsque vous avez terminé :
* Sous seuil d'alarme, entrez le nom et la description de votre alarme. Pour Whenever, choisissez >= et entrez 5000000. Saisissez 1 pour les périodes consécutives.
* Sous Actions, sélectionnez une liste de notifications existante ou choisissez Nouvelle liste pour en créer une nouvelle.
* Sous Aperçu de l'alarme, sélectionnez une période de 15 minutes et spécifiez une statistique de Somme.
1. Pour créer une alarme pour surveiller les erreurs d'allocation de ports
* Ouvrez la console CloudWatch à l'adresse https://console.aws.amazon.com/cloudwatch/
* Dans le volet de navigation, choisissez Alarmes, Create Alarm.
* Choisissez NAT Gateway.
* Sélectionnez la passerelle NAT et la mesure ErrorPortAllocation, puis choisissez Suivant.
* Configurez l'alarme comme suit, puis choisissez Créer une alarme lorsque vous avez terminé :
* Sous seuil d'alarme, entrez le nom et la description de votre alarme. Pour chaque fois, choisissez > et entrez 0. Saisissez 3 pour les périodes consécutives.
* Sous Actions, sélectionnez une liste de notifications existante ou choisissez Nouvelle liste pour en créer une nouvelle.
* Sous Aperçu de l'alarme, sélectionnez une période de 5 minutes et spécifiez une statistique de Maximum.

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

### CloudTrail
1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Consultez les journaux CloudTrail pour les noms d'événements `AutoriseSecurityGroupIngress`, `RevokeSecurityGroupIngress`, `AutoriseSecurityGroupgress` et `RevokeSecurityGroupeGress`

### CloudWatch
Créer une règle CloudWatch Events qui se déclenche sur un événement à l'aide de la console CloudWatch

1. Ouvrez la console CloudWatch.
1. Dans le volet de navigation, choisissez Règles sous Événements, puis Create rule.
1. Sélectionnez le modèle d'événement.
1. Pour Nom du service, choisissez EC2.
1. Pour Type d'événement, choisissez AWS API Call via CloudTrail.
1. Choisissez Opération spécifique et fournissez les appels d'API suivants. Ces appels d'API sont utilisés pour ajouter ou supprimer des règles de groupe de sécurité.
- AuthorizeSecurityGroupIngress
- AuthorizeSecurityGroupEgress
- RevokeSecurityGroupIngress
- RevokeSecurityGroupEgress
1. Ces paramètres créent le modèle d'événement suivant.
```json
{
« source » : [
« aws.ec2"
],
« type de détail » : [
« Appel d'AWS API via CloudTrail »
],
« détail » : {
« Source de l'événement » : [
« ec2.amazonaws.com »
],
« EventName » : [
« AuthorizeSecurityGroupIngress »,
« AuthorizeSecurityGroupEgress »,
« RevokeSecurityGroupIngress »,
« RevokeSecurityGroupEgress »
]
}
}
```
1. Choisissez Ajouter une cible.
1. Dans la liste des cibles, choisissez la rubrique SNS.
1. Pour Sujet, choisissez un sujet existant ou créez-en un nouveau.
1. Choisissez Configurer les détails.
1. Sur la page Configurer les détails de la règle, entrez un nom et une description facultative.
- Pour State, laissez la case Activé cochée.
1. Choisissez Créer une règle.

### Utiliser AWS Config pour vérifier la configuration d'un groupe de sécurité
Lorsque vous envisagez d'utiliser AWS Config et AWS Config Rules, déterminez d'abord s'il faut utiliser le service de manière réactive ou proactive.
- Pour utiliser AWS Config de manière réactive, il suffit d'activer le service et il commencera à créer un inventaire des ressources AWS et un historique des modifications.
- Pour utiliser AWS Config pour une notification proactive de modification, effectuez les tâches suivantes :
1. Créez une rubrique Amazon SNS pour recevoir des notifications de modification de configuration.
1. Développez des fonctions AWS Lambda ou des processeurs de messages SQS basés sur EC2 pour identifier les modifications spécifiques au réseau et envoyer des notifications appropriées.
1. Tenez compte d'au moins deux destinataires différents pour les notifications de modification :
- Un e-mail de sécurité réseau ou une liste de distribution de pagination pour recevoir des notifications nécessitant une intervention humaine immédiate (par exemple, modifier un groupe de sécurité pour allow all accès globalement ou attacher une passerelle Internet à un VPC interne uniquement).
- Un système central de journalisation ou de gestion des modifications qui peut être utilisé pour concilier les demandes de modification approuvées avec les modifications de configuration réelles.
1. Abonnez vos fonctions AWS Lambda ou vos files d'attente SQS à la rubrique SNS de notification de modification.
1. Configurez AWS Config pour publier des notifications de modification dans la rubrique SNS de notification de modification.

Voici un exemple d'utilisation d'AWS Config en combinaison avec une fonction Lambda :

1. Créez un [rôle d'exécution Lambda] (http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) et un [rôle AWS Config] (http://docs.aws.amazon.com/config/latest/developerguide/iamrole-permissions.html).
1. Permettez à AWS Config d'enregistrer la configuration des groupes de sécurité afin qu'AWS Config puisse surveiller les groupes de sécurité à la recherche de modifications apportées à leurs configurations. La capture d'écran suivante illustre un exemple de paramètres.
Capture d'écran montrant un exemple de paramètres de configuration
1. [Créer un groupe de sécurité] (http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) qui active les ports adaptés aux besoins de votre entreprise (par exemple 80/443 pour HTTP, 25 pour SMTP)
1. Créez la fonction Python Lambda à l'aide du [code de la bibliothèque de règles AWS Config sur GitHub] (https://github.com/awslabs/aws-config-rules/blob/master/python/ec2_security_group_ingress.py).
- Le code de la fonction Lambda définit une liste nommée REQUIRED_PERMISSIONS avec des éléments représentant un protocole, une plage de ports et une plage IP qui définissent ensemble une autorisation de sécurité. Dans ce cas, seuls les ports 80 et 443 seront autorisés.
- Personnalisez cette fonction Lambda avec les ports et les plages IP nécessaires pour répondre aux besoins de votre entreprise.
1. [Créer la règle AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html#creating-a-custom-rule-with-the-AWS-Config-console) à l'aide de la fonction Lambda que vous avez créée à l'étape 4.
- Pour Type de déclencheur, choisissez Modifications de configuration.
- Pour Étendue des modifications, choisissez EC2 : SecurityGroup, puis tapez l'ID du groupe de sécurité que vous avez créé à l'étape 3.
1. Exécutez la règle Config. La règle sera exécutée en file d'attente et la règle devrait être terminée dans environ 10 minutes.
1. Vérifiez le groupe de sécurité que vous avez créé à l'étape 3.

## Procédures d'escalade
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Analyse
### Affichage des VPC Flow Logs dans la console
**Pour afficher des informations sur les journaux de flux de vos interfaces réseau**
1. Ouvrez la console Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. Dans le volet de navigation, choisissez Interfaces réseau.
1. Sélectionnez une interface réseau, puis choisissez Flow Logs. Les informations relatives aux journaux de flux sont affichées dans l'onglet. La colonne Type de destination indique la destination vers laquelle les journaux de flux sont publiés.

**Pour afficher des informations sur les journaux de flux de vos VPC ou sous-réseaux**
1. Ouvrez la console Amazon VPC à l'adresse https://console.aws.amazon.com/vpc/
1. Dans le volet de navigation, choisissez Vos VPC ou Subnets.
1. Sélectionnez votre VPC ou votre sous-réseau, puis choisissez Flow Logs.
- Les informations relatives aux journaux de flux sont affichées dans l'onglet.
- La colonne Type de destination indique la destination vers laquelle les journaux de flux sont publiés.

**Pour afficher les enregistrements du journal de flux publiés sur Amazon S3**
1. Ouvrez la console Amazon S3 à l'adresse https://console.aws.amazon.com/s3/
1. Pour Nom du compartiment, sélectionnez le compartiment dans lequel les journaux de flux sont publiés.
1. Pour Nom, cochez la case en regard du fichier journal. Dans le panneau de présentation de l'objet, choisissez Télécharger.

### Interrogation de journaux de flux à l'aide d'Amazon Athena
Amazon Athena est un service de requête interactif qui vous permet d'analyser des données dans Amazon S3, telles que vos journaux de flux, à l'aide de SQL standard. Vous pouvez utiliser Athena avec les VPC Flow Logs pour obtenir rapidement des informations exploitables sur le trafic circulant dans votre VPC. Par exemple, vous pouvez identifier les ressources de vos Clouds privés virtuels (VPC) qui parlent le mieux ou identifier les adresses IP avec les connexions TCP les plus rejetées.

Vous pouvez rationaliser et automatiser l'intégration de vos journaux de flux VPC avec Athena en générant un modèle CloudFormation qui crée les ressources AWS requises et les requêtes prédéfinies que vous pouvez exécuter pour obtenir des informations sur le trafic circulant dans votre VPC.

1. Le modèle CloudFormation crée les ressources suivantes :
- Une base de données Athena. Le nom de la base de données est vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>.
- Un groupe de travail Athena. Le nom du <flow-log-subscription-id><partition-load-frequency><start-date><flow-log-subscription-id><partition-load-frequency><start-date><end-date><end-date>workgroup est Workgroup.
- Table Athena partitionnée qui correspond à vos enregistrements de journaux de flux. Le nom de la table est <flow-log-subscription-id><partition-load-frequency><start-date><end-date>.
- Un ensemble de requêtes nommées Athena. Pour plus d'informations, voir Requêtes prédéfinies.
- Fonction Lambda qui charge de nouvelles partitions dans la table selon le calendrier spécifié (quotidien, hebdomadaire ou mensuel).
- Un rôle IAM qui autorise l'exécution des fonctions Lambda.
1. Exigences
- Vous devez sélectionner une région qui prend en charge AWS Lambda et Amazon Athena.
- Les compartiments Amazon S3 doivent se trouver dans la région sélectionnée.
1. Tarification
- Vous engagez des frais Amazon Athena standard pour l'exécution de requêtes.
- Vous entraînez des frais AWS Lambda standard pour la fonction Lambda qui charge de nouvelles partitions selon un calendrier récurrent (lorsque vous spécifiez une fréquence de chargement de partition mais que vous ne spécifiez pas de date de début et de fin).
1. Effectuez l'une des opérations suivantes :
- Ouvrez la console Amazon VPC. Dans le volet de navigation, choisissez Vos VPC, puis sélectionnez votre VPC.
- Ouvrez la console Amazon VPC. Dans le volet de navigation, choisissez Subnets, puis sélectionnez votre sous-réseau.
- Ouvrez la console Amazon EC2. Dans le volet de navigation, choisissez Interfaces réseau, puis sélectionnez votre interface réseau.
1. Dans l'onglet Journaux de flux, sélectionnez un journal de flux qui publie sur Amazon S3, puis choisissez Actions, Générer l'intégration Athena.
1. Spécifiez la fréquence de chargement de la partition.
- Si vous choisissez Aucun, vous devez spécifier la date de début et de fin de la partition, à l'aide de dates antérieures.
- Si vous choisissez Quotidien, Hebdomadaire ou Mensuel, les dates de début et de fin de la partition sont facultatives.
- Si vous ne spécifiez pas de dates de début et de fin, le modèle CloudFormation crée une fonction Lambda qui charge de nouvelles partitions selon un programme récurrent.
1. Sélectionnez ou créez un compartiment S3 pour le modèle généré, et un compartiment S3 pour les résultats de la requête.
1. Choisissez Générer l'intégration Athena.
1. (Facultatif) Dans le message de réussite, choisissez le lien pour accéder au compartiment que vous avez spécifié pour le modèle CloudFormation, puis personnalisez le modèle.
1. Dans le message de réussite, choisissez Create CloudFormation stack pour ouvrir l'assistant Create Stack dans la console AWS CloudFormation.
- L'URL du modèle CloudFormation généré est spécifiée dans la section Modèle.
- Terminez l'assistant pour créer les ressources spécifiées dans le modèle.

**Exécution d'une requête prédéfinissé**

Le modèle CloudFormation généré fournit un ensemble de requêtes prédéfinies que vous pouvez exécuter pour obtenir rapidement des informations significatives sur le trafic de votre réseau AWS. Une fois que vous avez créé la pile et vérifié que toutes les ressources ont été créées correctement, vous pouvez exécuter l'une des requêtes prédéfinies.

1. Pour exécuter une requête prédéfinie à l'aide de la console
- Ouvrez la console Athena. Dans le panneau Groupes de travail, sélectionnez le groupe de travail créé par le modèle CloudFormation.
- Sélectionnez l'une des requêtes prédéfinies, modifiez les paramètres si nécessaire, puis exécutez la requête.
- Ouvrez la console Amazon S3. Accédez au compartiment que vous avez spécifié pour les résultats de la requête et affichez les résultats de la requête.

Voici les requêtes nommées Athena fournies par le modèle CloudFormation généré :
* VpcFlowLogsAcceptedTraffic — Connexions TCP autorisées en fonction de vos groupes de sécurité et de vos listes de contrôle d'accès réseau.
* VpcFlowLogsAdminPortTraffic — Le trafic enregistré sur les ports d'application Web d'administration.
* VpcFlowLogsIPv4Traffic — Le nombre total d'octets de trafic IPv4 enregistrés.
* VpcFlowLogsIPv6Traffic — Le nombre total d'octets de trafic IPv6 enregistrés.
* VpcFlowLogsRejectedTCPTraffic — Connexions TCP rejetées en fonction de vos groupes de sécurité ou des listes ACL réseau.
* VPCFlowLogsRejectedTraffic — Le trafic rejeté en fonction de vos groupes de sécurité ou ACL réseau.
* VPCFlowLogssShrdpTraffic — Trafic SSH et RDP.
* VpcFlowLogsTopTalkers — Les 50 adresses IP avec le plus de trafic enregistré.
* VpcFlowLogsTopTalkersPacketLevel — Les 50 adresses IP au niveau du paquet avec le plus de trafic enregistré.
* VpcFlowLogsTopTalkingInstances — ID des 50 instances ayant le plus de trafic enregistré.
* VpcFlowLogsTopTalkingSubnets — ID des 50 sous-réseaux ayant le plus de trafic enregistré.
* VpcFlowLogsTopTCPTraffic — Tout le trafic TCP enregistré pour une adresse IP source.
* VpcFlowLogsTotalBytesTransferred — Les 50 paires d'adresses IP source et de destination avec le plus d'octets enregistrés.
* VpcFlowLogsTotalBytesTransferredPacketLevel — 50 paires d'adresses IP source et de destination au niveau du paquet avec le plus d'octets enregistrés.
* VpcFlowLogsTrafficFrmSrcAddr — Le trafic enregistré pour une adresse IP source spécifique.
* VpcFlowLogsTrafficToDstAddr — Le trafic enregistré pour une adresse IP de destination spécifique

## Confinement
### Identification de l'utilisateur
Identifiez quel utilisateur ou quel rôle a exécuté les modifications réseau non autorisées

Voici un exemple permettant d'identifier l'utilisateur ou le profil derrière la création d'une instance EC2 :

1. Identifiez l'un des ID d'instance des instances EC2 suspectes à l'étape 1
1. Ouvrez AWS Console
1. Sélectionnez Services puis CloudTrail
1. Dans la marge de gauche, sélectionnez « Event History »
1. Effacez le filtre actuel en sélectionnant le « X » à droite de « faux »
1. À l'extrême droite (sous Personnalisé), cliquez sur l'icône en forme d'engrenage
1. Faites défiler vers le bas et activez « AWS Access Key »
1. Au milieu, remplacez la liste déroulante du filtre en « Nom de la ressource »
1. Entrez l'ID d'instance EC2 dans la zone de recherche et appuyez sur Entrée
1. Vous devriez voir un événement RunInstances ; cliquez sur ce bouton.
1. Enregistrez le ou les noms d'utilisateur que vous avez découverts.
- Il peut y avoir plusieurs événements à parcourir pour trouver des événements originaux tels que ceux créés à partir d'un modèle CloudFormation
1. Retournez à l'Event History et modifiez la liste déroulante en « Nom d'utilisateur » et publiez l'entrée de l'étape précédente dans la recherche, puis appuyez sur Entrée
1. Identifiez tout autre événement de la part de l'utilisateur suspect ou compromis
1. Passons maintenant à Services, IAM (en haut à gauche de l'écran)
1. Sous Ressources IAM, sélectionnez Utilisateurs
1. Recherchez le compte que nous avons identifié dans CloudTrail et sélectionnez-le.
1. Sélectionner les identifiants de sécurité
1. Faites défiler jusqu'à la clé d'accès et sélectionnez le « x » pour supprimer la clé
1. Accédez à votre tableau de bord Challenge et cliquez sur le bouton Vérifier ma progression.

## Éradication
### Examinez les résultats de [Examiner l'historique des événements CloudTrail pour connaître l'activité par la clé d'accès compromise] (. /#cloudtrail)
Supprimez toutes les ressources créées par la ou les clés compromises. Assurez-vous de vérifier toutes les régions AWS, même les régions où vous n'avez jamais lancé de ressources AWS.
* **Important** : Si vous devez conserver des ressources pour l'enquête, envisagez de les sauvegarder. Par exemple, si vous avez un besoin réglementaire, de conformité ou juridique de conserver une instance EC2, prenez un instantané EBS avant de mettre fin à l'instance.

### Consultez le [Éviter les charges imprévues] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
Vérifiez et supprimez tous les services reconnus dans votre compte. Portez une attention particulière aux ressources suivantes :
* Instances EC2 et AMI, y compris les instances à l'état arrêté
* Volumes et instantanés EBS
* Fonctions et couches AWS Lambda

Pour supprimer des fonctions et des couches Lambda, procédez comme suit :
1. Ouvrez la console Lambda.
1. Dans le volet de navigation, choisissez Fonctions.
1. Sélectionnez les fonctions que vous souhaitez supprimer.
1. Pour Actions, choisissez Supprimer.
1. Dans le volet de navigation, choisissez Calques.
1. Sélectionnez la couche que vous souhaitez supprimer.
1. Choisissez Supprimer.

## Récupération
Restaurer toutes les modifications apportées aux ressources réseau

Identifiez les ressources exposées à Internet et sécurisez-les : `. /rôdeur -g groupe 17`

## Actions préventives
### AWS Config et AWS System Manager
[Comment corriger automatiquement les ports accessibles à Internet avec AWS Config et AWS System Manager] (https://aws.amazon.com/blogs/security/how-to-auto-remediate-internet-accessible-ports-with-aws-config-and-aws-system-manager/)

### AWS Security Hub
[Activer la nouvelle norme AWS Foundational Security Best Practices Security Security Security Security Security] (https://aws.amazon.com/blogs/security/aws-foundational-security-best-practices-standard-now-available-security-hub/?secd_dir1)

### Posture générale de sécurité
Exécutez un [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contre l'environnement afin d'identifier davantage d'autres risques et éventuellement d'autres risques publics non identifiés dans ce playbook.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas besoin de « réparation », mais qui sont importants à savoir lorsque vous exécutez ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant que répondeur aux incidents, je dois être en mesure de surveiller tous les environnements VPC critiques
- En tant que répondant aux incidents, j'ai besoin d'un moyen de coordonner avec l'équipe commerciale, l'équipe d'infrastructure et les équipes de sécurité
- En tant que répondeur d'incident, j'ai besoin d'un playbook pour interroger les VPC FlowLogs à grande échelle
- En tant que répondeur aux incidents, je dois garantir l'utilisation correcte de Config dans tous les comptes

## Articles en arriéré actuel