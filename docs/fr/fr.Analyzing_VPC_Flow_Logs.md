# Playbook de réponse aux incidents : analyse des VPC Flow Logs
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# Points de contact

Auteur : `Nom de l'auteur \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé exécutif
Ce manuel présente des exemples de processus et de requêtes permettant d'analyser les journaux de flux AWS VPC.

## Préparation
Ce playbook fait référence et s'intègre, dans la mesure du possible, à [Prowler] (https://github.com/toniblyx/prowler) qui est un outil de ligne de commande qui vous aide dans l'évaluation de la sécurité AWS, l'audit, le renforcement et la réponse aux incidents.

Il suit les directives de la norme CIS Amazon Web Services Foundations Benchmark (49 contrôles) et comporte plus de 100 contrôles supplémentaires, notamment liés au RGPR, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 et autres.

Cet outil fournit un instantané rapide de l'état actuel de la sécurité dans un environnement client. En outre, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) permet une analyse automatisée de la conformité et peut [s'intégrer à Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

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
[Perspective de sécurité AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directif**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Outil : Athena
* **Catégorie** : Analyse des journaux
* **Ressource** : Athena
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers
* **Sources de journal** : VPC Flow Logs
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

### Étapes de réponse
1. [**PREPARATION**] Créer un inventaire d'actifs publiquement exposé
2. [**PREPARATION**] Journalisation de l'installation
3. [**PREPARATION**] Mettre en œuvre un programme de formation pour identifier et évaluer les capacités d'analyse des journaux
4. [**DÉTECTION ET ANALYSE**] Affichage des VPC Flow Logs dans la console
5. [**DÉTECTION ET ANALYSIS**] Interrogation des journaux de flux à l'aide d'Amazon Athena
6. [**DÉTECTION ET ANALYSIS**] Créer la table des VPC Flow Logs en tant que DDL
7. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : créez la nouvelle table Logs en tant que DDL
8. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : vérifiez si elle est chargée et combien de lignes se trouvent dans un fichier
9. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : vérifiez si des données visualisables et utilisables sont renvoyées :
10. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : vérifier les adresses IP les plus importantes
11. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : requêtes standard pour effectuer des enquêtes
12. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : recherche du rapport de données pour les adresses IP source/destination
13. [**DÉTECTION ET ANALYSIS**] Exemple de requêtes Athena : recherche de CIO connus
14. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : table des menaces
15. [**DÉTECTION ET ANALYSIS**] Exemples de requêtes Athena : vue de recherche sur le type de menace cible

## Préparation
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

Prowler peut également vous aider à trouver de nombreuses ressources exposées à Internet : ». /rôdeur -g groupe 17"

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
- Par exemple, pour spécifier un sous-dossier nommé my-logs dans un compartiment nommé my-bucket, utilisez l'ARN suivant : arn:aws:s3። :my-bucket/my-logs/
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
- Par exemple, pour spécifier un sous-dossier nommé my-logs dans un compartiment nommé my-bucket, utilisez l'ARN suivant : « arn:aws:s3። :my-bucket/my-logs/
- Si vous êtes propriétaire du compartiment, nous créons automatiquement une stratégie de ressources et nous l'attachons au compartiment. Pour plus d'informations, consultez Autorisations du compartiment Amazon S3 pour les journaux de flux.
1. Pour Format d'enregistrement de journal, sélectionnez le format de l'enregistrement de journal de flux.
* Pour utiliser le format par défaut, choisissez le AWS default format.
* Pour utiliser un format personnalisé, choisissez Format personnalisé, puis sélectionnez des champs dans Format journal.
1. Choisissez Create flow log.

**Surveillance des passerelles NAT :** Vous pouvez surveiller votre passerelle NAT à l'aide de CloudWatch, qui collecte des informations de votre passerelle NAT et crée des mesures lisibles et quasi en temps réel.
1. Les mesures de passerelle NAT sont envoyées à CloudWatch à intervalles d'une minute.
1. Vous pouvez afficher les mesures de vos passerelles NAT comme suit.
* Ouvrez la [console Amazon CloudWatch] (https://console.aws.amazon.com/cloudwatch/)
* Dans le volet de navigation, choisissez Metrics.
* Sous Toutes les mesures, choisissez l'espace de noms des mesures de passerelle NAT.
* Pour afficher les mesures, sélectionnez la dimension de mesure.
1. Pour créer une alarme pour le trafic sortant via la passerelle NAT, procédez comme suit :
* Ouvrez la [console Amazon CloudWatch] (https://console.aws.amazon.com/cloudwatch/)
* Dans le volet de navigation, choisissez Alarmes, Create Alarm.
* Choisissez la passerelle NAT.
* Sélectionnez la passerelle NAT et la métrique « BytesOutToDestination », puis choisissez Suivant.
* Configurez l'alarme comme suit, puis choisissez Créer une alarme lorsque vous avez terminé :
* Sous seuil d'alarme, entrez le nom et la description de votre alarme. Pour Whenever, choisissez >= et entrez 5000000. Saisissez 1 pour les périodes consécutives.
* Sous Actions, sélectionnez une liste de notifications existante ou choisissez Nouvelle liste pour en créer une nouvelle.
* Sous Aperçu de l'alarme, sélectionnez une période de 15 minutes et spécifiez une statistique de Somme.
1. Pour créer une alarme afin de surveiller les erreurs d'allocation de ports
* Ouvrez la [console Amazon CloudWatch] (https://console.aws.amazon.com/cloudwatch/)
* Dans le volet de navigation, choisissez Alarmes, Create Alarm.
* Choisissez NAT Gateway.
* Sélectionnez la passerelle NAT et la mesure « ErrorPortAllocation », puis choisissez Suivant.
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

## Détection et analyse
### Affichage des VPC Flow Logs dans la console
**Pour afficher des informations sur les journaux de flux de vos interfaces réseau**
1. Ouvrez la console Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. Dans le volet de navigation, choisissez Interfaces réseau.
1. Sélectionnez une interface réseau, puis choisissez Flow Logs. Les informations relatives aux journaux de flux sont affichées dans l'onglet. La colonne Type de destination indique la destination vers laquelle les journaux de flux sont publiés.

**Pour afficher des informations sur les journaux de flux de vos VPC ou sous-réseaux**
1. Ouvrez la [console Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. Dans le volet de navigation, choisissez Vos VPC ou Subnets.
1. Sélectionnez votre VPC ou votre sous-réseau, puis choisissez Flow Logs.
- Les informations relatives aux journaux de flux sont affichées dans l'onglet.
- La colonne Type de destination indique la destination vers laquelle les journaux de flux sont publiés.

**Pour afficher les enregistrements du journal de flux publiés sur Amazon S3**
1. Ouvrez la [console Amazon S3] (https://console.aws.amazon.com/s3/)
1. Pour Nom du compartiment, sélectionnez le compartiment dans lequel les journaux de flux sont publiés.
1. Pour Nom, activez la case à cocher en regard du fichier journal. Dans le panneau de présentation des objets, choisissez Download.

### Interrogation de journaux de flux à l'aide d'Amazon Athena
Amazon Athena est un service de requête interactif qui vous permet d'analyser des données dans Amazon S3, telles que vos journaux de flux, à l'aide de SQL standard. Vous pouvez utiliser Athena avec les VPC Flow Logs pour obtenir rapidement des informations exploitables sur le trafic circulant dans votre VPC. Par exemple, vous pouvez identifier les ressources de vos Clouds privés virtuels (VPC) qui parlent le mieux ou identifier les adresses IP avec les connexions TCP les plus rejetées.

Vous pouvez rationaliser et automatiser l'intégration de vos journaux de flux VPC avec Athena en générant un modèle CloudFormation qui crée les ressources AWS requises et les requêtes prédéfinies que vous pouvez exécuter pour obtenir des informations sur le trafic circulant dans votre VPC.

1. Le modèle CloudFormation crée les ressources suivantes :
- Une base de données Athena. Le nom de la base de données est vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>.
- Un groupe de travail Athena. Le nom du <flow-log-subscription-id><partition-load-frequency><start-date><flow-log-subscription-id><partition-load-frequency><start-date><end-date><end-date>workgroup est Workgroup
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
- Ouvrez la console Amazon VPC. Dans le volet de navigation, choisissez Your VPC, puis sélectionnez votre VPC.
- Ouvrez la console Amazon VPC. Dans le volet de navigation, choisissez Subnets, puis sélectionnez votre sous-réseau.
- Ouvrez la console Amazon EC2. Dans le volet de navigation, choisissez Interfaces réseau, puis sélectionnez votre interface réseau.
1. Dans l'onglet Journaux de flux, sélectionnez un journal de flux qui publie sur Amazon S3, puis choisissez Actions, Générer l'intégration Athena.
1. Spécifiez la fréquence de chargement de la partition.
- Si vous choisissez Aucun, vous devez spécifier la date de début et de fin de la partition, à l'aide de dates antérieures.
- Si vous choisissez Quotidien, Hebdomadaire ou Mensuel, les dates de début et de fin de la partition sont facultatives.
- Si vous ne spécifiez pas de dates de début et de fin, le modèle CloudFormation crée une fonction Lambda qui charge de nouvelles partitions selon une planification récurrente.
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
- Sélectionnez l'une des requêtes prédéfinies, modifiez les paramètres selon vos besoins, puis exécutez la requête.
- Ouvrez la console Amazon S3. Accédez au compartiment que vous avez spécifié pour les résultats de la requête et affichez les résultats de la requête.

Voici les requêtes nommées Athena fournies par le modèle CloudFormation généré :
* VpcFlowLogsAcceptedTraffic — Connexions TCP autorisées en fonction de vos groupes de sécurité et de vos listes de contrôle d'accès réseau.
* VpcFlowLogsAdminPortTraffic — Le trafic enregistré sur les ports d'application Web d'administration.
* VpcFlowLogsIPv4Traffic — Le nombre total d'octets de trafic IPv4 enregistrés.
* VpcFlowLogsIPv6Traffic — Le nombre total d'octets de trafic IPv6 enregistrés.
* VpcFlowLogsRejectedTCPTraffic — Connexions TCP rejetées en fonction de vos groupes de sécurité ou des listes ACL réseau.
* VpcFlowLogsRejectedTraffic — Le trafic qui a été rejeté en fonction de vos groupes de sécurité ou des listes de contrôle d'accès réseau.
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

## Analyse Deep Dive :

Référence : https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html

### Créer la table des VPC Flow Logs en tant que DDL
Le code suivant permet de créer la table en tant que DDL pour les VPC Flow Logs qui seront examinés. (Veuillez prendre note des articles TODO en ligne) :
```
/*
Crée une table de flux VPC pour les journaux de flux VPC livrés directement à un compartiment S3
Inclut la configuration du partitionnement pour le déploiement multi-comptes et multi-régions ainsi que la date au format AAAA/MM/JJ
Inclut tous les champs disponibles, y compris les champs v3 et v4 autres que ceux par défaut.
REMARQUE : Les champs de flux VPC doivent être configurés dans l'ordre indiqué lors de leur configuration. S'ils ont été configurés dans un ordre différent, vous devrez ajuster l'ordre dans lequel ils sont répertoriés dans le DDL ci-dessous
*/

/*
À FAIRE : vous pouvez éventuellement mettre à jour le nom de table « vpcflow » avec le nom que vous souhaitez utiliser pour la table du journal de flux VPC
*/
CRÉER UNE TABLE EXTERNE S'IL N'EXISTE PAS vpcflow (
/*
À FAIRE : vérifiez que les journaux de flux VPC ont été configurés pour être générés dans cet ordre. Si ce n'est pas le cas, vous devrez réorganiser l'ordre ci-dessous pour correspondre à l'ordre dans lequel ils ont été générés
CONSEIL : téléchargez un échantillon et vérifiez la première ligne de chaque fichier journal pour l'ordre des champs,
Ne vous inquiétez pas si les noms des champs ne correspondent pas exactement aux noms figurant dans la première ligne du journal, Athena les importera en fonction de leur ordre.
REMARQUE : Ces champs v2 sont au format par défaut et n'ont généralement pas besoin d'être ajustés, faites attention à l'ordre des champs v3/v4/v5 ci-dessous, car il n'y a pas d'ordre par défaut de ces champs. Les champs et les descriptions peuvent être trouvés à l'adresse https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html
*/
version int,
chaîne de compte,
chaîne d'ID d'interface,
chaîne d'adresse source,
chaîne d'adresse de destination,
port source int,
port de destination int,
protocole int,
numpackets int,
chiffres biging,
starttime int,
endtime int,
chaîne d'action,
chaîne d'état du journal,
/*
REMARQUE : début des champs v3 et v4 autres que ceux par défaut
Ne vous inquiétez pas si vos journaux source n'incluent pas tous ces champs, ils seront simplement vides
À FAIRE : Si vos journaux de flux VPC incluent ces champs, assurez-vous de vérifier que l'ordre dans lequel ils sont répertoriés ci-dessous est identique à celui dans lequel ils sont configurés au format de journal de flux
*/
chaîne vpcid,
chaîne de type,
tcpflags small,
chaîne subnetid,
chaîne de type de sous-localisation,
chaîne sublocationid,
chaîne de région,
chaîne pktsrcaddr,
chaîne pktdstaddr,
chaîne d'ID d'instance,
chaîne azide,
/*
REMARQUE : début des champs v5 autres que ceux par défaut
Ne vous inquiétez pas si vos journaux source n'incluent pas tous ces champs, ils seront simplement vides
À FAIRE : Si vos journaux de flux VPC incluent ces champs, assurez-vous de vérifier que l'ordre dans lequel ils sont répertoriés ci-dessous est identique à celui dans lequel ils sont configurés au format de journal de flux
*/
chaîne de service pkt-src-aws-service,
chaîne de service pkt-dst-aws-service,
chaîne de direction de flux,
trafic-path int
)
PARTITIONNÉS PAR
(
STRING date_partition,
STRING region_partition,
string account_partition
)
FORMAT DE LIGNE DÉLIMITÉ
CHAMPS TERMINÉS PAR « »
/*
À FAIRE : remplacer nom_bucket et optional_prefix dans la valeur LOCATION,
s'il n'y a pas de préfixe, supprimez l'extra/
exemple : s3 : //my_Central_Log_Bucket/AWSLogs/ ou s3 : //My_Central_Log_Bucket/Prod/AWSLogs/
*/
EMPLACEMENT 'S3 ://<bucket_name>/<optional_prefix>/AWSLogs/ '
PROPRIÉTÉS DE LA TBL
(
« skip.header.line.count"="1",
« projection.enabled » = « vrai »,
« projection.date_partition.type » = « date »,
/* TODO : remplacez <YYYY><MM>//<DD>par la première date de vos journaux, par exemple : 30/11/2020*/
« projection.date_partition.range » = "<YYYY>/<MM>/<DD>, MAINTENANT »,
« projection.date_partition.format » = « AAAA/MM/JJ »,
« projection.date_partition.interval » = « 1",
« projection.date_partition.interval.unit » = « JOURS »,
« projection.region_partition.type » = « enum »,
« projection.region_partition.values » = « us-est-2, us-est-1, us-west-1, us-ouest-2, af-sud 1, ap-est-1, ap-sud 1, ap-sud-est 3, ap-northeast-2, ap-sud-est 1, ap-northeast-1, ca-centre 1, ap-southeast-2, ap-northeast-1, ca-centre-1 al-1, cn-nord-1, cn-nord-ouest 1, eu-central-1, eu-ouest-1, eu-ouest-2, eu-sud 1, eu-ouest-3, eu-nord-1, me-sud 1, sa-est-1",
« projection.account_partition.type » = « enum »,
/*
À FAIRE : remplacez les valeurs de projection.account_partition.values par la liste des numéros de compte AWS que vous souhaitez inclure dans cette table
exemple : « 0123456789,0123456788,0123456777"
Remarque : n'utilisez aucun espace, séparez uniquement les valeurs par une virgule (y compris les espaces entraînera une erreur de syntaxe)
s'il n'y a qu'un seul compte, incluez-le seul sans virgule, par exemple : « 0123456789 »
*/
« projection.account_partition.values » = "<account_num_1>,<account_num_2>, ... «,
/*
À FAIRE : Identique à LOCATION, remplacez nom_bucket et optional_prefix dans la valeur de storage.location.template,
s'il n'y a pas de préfixe, supprimez l'extra/
Assurez-vous que seules les valeurs des crochets <> sont remplacées, que les valeurs contenues dans les crochets {} sont des variables et doivent rester telles que définies ci-dessous.
exemple : s3 : //My_Central_Log_Bucket/AWSLogs/... ou s3 : //My_Central_Log_Bucket/Prod/AWSLogs/...
*/
« storage.location.template » = « s3 ://<bucket_name>/<optional_prefix>/awslogs/ $ {account_partition} /vpcflowlogs/$ {region_partition} /$ {date_partition} »
) ;
```
### Exemples de requêtes :
#### Créer la nouvelle table Logs en tant que DDL
Créez une nouvelle table Athena en exécutant la requête suivante ci-dessous. Gardez à l'esprit :

* Remplacez <bucket_name>par le compartiment cible.
* Vérifiez l'EMPLACEMENT pour vous assurer qu'il inclut tous les préfixes de compartiment pertinents et sa barre oblique de fin («/»)
```
CRÉER UNE TABLE EXTERNE S'IL N'EXISTE PAS vpc_flow_logs (
is_rejected_packet STRING,
avg_in_packet_size INT,
CHAÎNE vpc_id,
INT local_port,
CHAÎNE source_dc,
CHAÎNE IP,
chaîne end_time,
REMOTE_PORT INT,
String start_time,
protocole INT,
in_octets BIGINT,
chaîne source_tag,
string account_id,
string instance_id,
INTERFACE_public_ips STRING,
BIGINT out_octets,
is_skipped_rejected_packets STRING,
CHAÎNE source_az,
CHAÎNE source_region,
AVG_out_packet_size INT,
string de type instance,
INTERFACE_local_ips STRING
)
FORMAT DE LIGNE SERDE 'org.OpenX.Data.JSONSerde.JSONSerde'
AVEC SERDEPROPERTIES ('ignore.malformed.json' = 'vrai')
EMPLACEMENT 'S3 ://<bucket_name>/' ;
```
#### Vérifiez s'il est chargé et combien de lignes se trouvent dans le fichier :
```
SELECT count (*) FROM vpc_flow_logs ;
```
#### Vérifiez si des données visualisables et utilisables sont renvoyées :
```
SELECT * À PARTIR de la limite de 1000 vpc_flow_logs ;
```
#### Vérifiez les adresses IP les plus importantes
```
sélectionnez ip, count (*) cnt
à partir de vpc_flow_logs
groupe par IP
commande par cnt desc
limite 100 ;

Prenez les meilleures adresses IP et commandez par temps (échangez l'adresse IP ci-dessous)

sélectionnez* dans vpc_flow_logs
où ip = '123.123.123.123'
commande par start_time ;
```

### « Notes sur l'interface de la S3 Gateway /VPC »
Pour les « points de terminaison VPC », les connexions s'affichent dans les journaux de flux sous forme de trafic sur leurs adresses privées

La S3 Gateway s'affiche dans les journaux de flux sous forme d'adresse privée se connectant à des plages publiques dans la liste de préfixes

### Requêtes standard pour effectuer des enquêtes

#### Recherche du rapport de données pour les adresses IP source/destination
La requête suivante regroupe les enregistrements du journal de flux VPC par IP, le nombre d'enregistrements (nombre total d'enregistrements de la même adresse source ou de destination) et le nombre total d'octets (nombre total d'octets de communication entre l'adresse source ou destination). La requête donne également le rapport de données, qui correspond au nombre d'octets envoyés par communication (nombre total d'octets divisés par le nombre d'enregistrements).
```
SÉLECTIONNEZ l'adresse source,
adresse de destination,
count (*) AS record_count,
SUM (nombres) AS total_octets,
SUM (nombres) /count (*) AS data_ratio,
date_format (from_unixtime (min (heure de début)),
'%Y/%M/%D%H : %i :%s') AS en premier, date_format (de_unixtime (max (heure de fin)), '%Y/%M/%D%H :%i :%s') AS dernier vu
DE <database>« ». <table>»
WHERE (sourceaddress = '10.10.10.10'
OU destinationaddress = '10.10.10.10')
ADRESSE SOURCE DE GROUPE PAR, adresse de destination
COMMANDE PAR DESC total_bytes
```
#### Recherche de CIO connus
La requête suivante suppose que les IOCs sont des adresses IP externes et utilisent une table de choix pour les gérer.

#### Tableau des menaces
```
CRÉER UNE TABLE EXTERNE S'IL N'EXISTE PAS incident_iocs (
chaîne de caractères threat_ip,
chaîne de caractères threat_comment
)

SERDE DE FORMAT DE LIGNE
'org.apache.hadoop.hive.serde2.opencsvSerde'

AVEC PROPRIÉTÉS SERDE (
'SeparatorChar '=', ',
'QuoteChar '=' \ "',
'EscapeChar '=' \ \ '
)
STOCKÉ SOUS FORME DE FICHIER TEXTE
EMPLACEMENT '3 : //bucket-analysis/iocs/iocfile ioc/ioc/'
```
#### Vue de recherche pour le type de menace cible
```
CREATE VIEW incident_vpc_flow_directional AS
SELECT vpc.account,
l'identifiant de l'interface vpc,
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) source,
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) comme destination,
« ENTRANT » COMME DIRECTION,
vpc.sourceaddress AS connecting_ip,
vpc.sourceport AS connecting_port,
protocole vpc,
paquets vpc.nump,
nombres vpc.,
vpc.action,
état de vpc.log,
CAST (from_unixtime (vpc.starttime) AS TIMESTAMP) COMME Heure de début,
CAST (from_unixtime (vpc.endtime) AS TIMESTAMP) AS EndTime,
date_diff ('second', from_unixtime (vpc.starttime), from_unixtime (vpc.endtime)) comme SessionTime

DE <database>« ». <table>« AS vpc

OÙ
vpc.destinationaddress IN (SÉLECTIONNEZ threat_ip FROM incident_iocs)

UNION TOUS

SELECT vpc.account,
l'identifiant de l'interface vpc,
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) source,
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) comme destination,
« OUTBOUN » COMME DIRECTION,
vpc.adresse de destination AS connecting_ip,
vpc.destinationport AS connecting_port,
protocole vpc,
paquets vpc.nump,
nombres vpc.,
vpc.action,
état de vpc.log,
CAST (from_unixtime (vpc.starttime) AS TIMESTAMP) COMME Heure de début,
CAST (from_unixtime (vpc.endtime) AS TIMESTAMP) AS EndTime,
date_diff ('second', from_unixtime (vpc.starttime), from_unixtime (vpc.endtime)) comme SessionTime

DE <database>« ». <table>« AS vpc
OÙ
vpc.sourceaddress IN (SÉLECTIONNEZ threat_ip FROM incident_iocs)

ORDER BY StartTime, EndTime, SessionTime, Interfaceid, connecting_ip, direction
```

# Nombre d'articles de carnet de commandes réglés
- En tant que répondeur aux incidents, je dois être en mesure de surveiller tous les environnements VPC critiques
- En tant que répondeur d'incident, j'ai besoin d'un playbook pour interroger les VPC FlowLogs à grande échelle

## Articles en arriéré actuel