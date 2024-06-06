# Playbook de réponse aux incidents : réponse à la rançon pour EC2 Microsoft Windows
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2024 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# Points de contact

Auteur : `Nom de l'auteur
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus de réponse aux attaques Ransom contre des instances EC2.

Pour plus d'informations, veuillez consulter le [Guide de réponse aux incidents de sécurité AWS] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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
4. [**DÉTECTION ET ANALYSIS**] Utiliser les procédures de l'Microsoft 365 Defender Threat Intelligence Team
5. [**DÉTECTION ET ANALYSIS**] Utilisez les mesures CloudWatch pour déterminer si les données ont pu être exfiltrées
6. [**DÉTECTION ET ANALYSIS**] Utiliser VPCFlowLogs pour identifier les accès inappropriés à la base de données à partir d'adresses IP externes
7. [**CONFINEMENT ET ÉRADICATION**] Supprimez tous les systèmes compromis du réseau.
8. [**CONFINEMENT ET ÉRADICATION**] Appliquer les NACL basés sur les IOCs réseau pour empêcher tout trafic supplémentaire
9. [**CONFINEMENT ET ÉRADICATION**] Autres sujets d'intérêt
10. [**RECOVERY**] Exécutez les procédures de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Rançon et destruction des données
* **Catégorie** : Attaque de rançon
* **Ressource** : EC2
* **Indicateurs** : Cyber Threat Intelligence, avis tiers, mesures Cloudwatch
* **Sources de journaux : CloudTrail, CloudWatch, AWS Config
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

## Préparation
* Évaluer la posture de sécurité du compte pour identifier et corriger les failles de sécurité
* AWS a développé un nouvel outil Open Source Self-Service Security Assessment (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS compte
* Pour protéger votre organisation, Microsoft recommande d'utiliser les informations contenues dans la présentation PowerPoint [Human-Operated Ransomware Mitigation Project Plan] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx), qui inclut [sécurisation accès privilégié] (https://aka.ms/spa)
* Maintenir un inventaire complet des ressources de toutes les ressources, y compris les contrôleurs de domaine, les instances Microsoft Windows EC2, les serveurs et bases de données Microsoft Windows, et toute intégration avec des fournisseurs d'identité externes
* Effectuez une analyse de vulnérabilité récurrente de vos hôtes à l'aide d'utilitaires tels que [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
* Activer et mettre à jour Windows Defender Antivirus : les solutions EDR (Endpoint Detection and Response) commerciales payantes par abonnement sont préférables
* Effectuer des sauvegardes d'instances EC2
* Envisagez d'utiliser [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) ou [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
* [Sauvegardez vos fichiers avec l'historique des fichiers] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
* Vérifiez vos sauvegardes et assurez-vous que l'infection ne s'y est pas propagée
* Sauvegardez régulièrement des fichiers importants. Utilisez la règle 3-2-1. Conservez trois sauvegardes de vos données, sur deux types de stockage différents, et au moins une sauvegarde hors site
* Appliquer les dernières mises à jour à vos systèmes d'exploitation et applications
* Éduquez vos employés afin qu'ils puissent identifier les attaques d'ingénierie sociale et d'hameçonnage de lance
* Implémenter [accès contrôlé aux dossiers] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). Il peut empêcher les ransomwares de crypter des fichiers et de conserver les fichiers pour une rançon.
* [Bloquer les macros dans les documents Office] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
* Bloquer les types de fichiers de ransomware connus
* Dans Windows 10, activez [Controlled Folder Access] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) pour protéger vos dossiers locaux importants contre les programmes non autorisés tels que les rançongiciels ou autres logiciels malveillants
* Suivez [Meilleures pratiques pour sécuriser vos services Active Directory] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
* Suivez [Top 10 des paramètres de stratégie de groupe les plus importants pour prévenir les failles de sécurité] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
* L'Microsoft 365 Defender Threat Intelligence Team a fourni un [rapport complet] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifiant les actions visant à sécuriser vos ressources Microsoft avant un événement de ransomware
* Renforcez les actifs connectés à Internet et assurez-vous qu'ils disposent des dernières mises à jour de sécurité. Utilisez la gestion des menaces et des vulnérabilités pour vérifier régulièrement ces ressources afin de détecter les vulnérabilités, les erreurs de configuration et les activités suspectes.
* Secure Remote Desktop Gateway à l'aide de solutions telles que l'authentification multifacteur Azure (MFA). Si vous ne disposez pas d'une passerelle MFA, activez l'authentification au niveau du réseau (NLA)
* Pratiquez le principe du moindre privilège et maintenez l'hygiène des accréditations. Évitez l'utilisation de comptes de service de niveau administrateur à l'échelle du domaine. Appliquez des mots de passe d'administrateur locaux robustes randomisés juste à temps
* Surveillez les tentatives de force brute. Vérifier les tentatives excessives d'authentification échouées (ID d'événement de sécurité Windows 4625)
* Surveillez la suppression des journaux d'événements, en particulier le journal des événements de sécurité et les journaux opérationnels PowerShell. Microsoft Defender ATP déclenche l'alerte « Event log was cleared » et Windows génère un ID d'événement 1102 lorsque cela se produit.
* Activez les fonctions de protection contre la sabotage pour empêcher les attaquants d'arrêter les services de sécurité
* Déterminez où les comptes hautement privilégiés se connectent et exposent les informations d'identification. Surveillez et étudiez les événements d'ouverture de session (ID d'événement 4624) pour les attributs de type d'ouverture de session. Les comptes d'administrateur de domaine et autres comptes dotés de privilèges élevés ne doivent pas être présents sur les postes de travail
* Activez la protection fournie par le cloud et la soumission automatique des échantillons sur Windows Defender Antivirus. Ces fonctionnalités utilisent l'intelligence artificielle et l'apprentissage automatique pour identifier et arrêter rapidement les menaces nouvelles et inconnues.
* Activez les règles de réduction de la surface d'attaque, y compris les règles qui bloquent le vol d'informations d'identification, l'activité de ransomware et l'utilisation suspecte de PsExec et de WMI. Pour traiter les activités malveillantes initiées via des documents Office armés, utilisez des règles qui bloquent l'activité avancée des macros, le contenu exécutable, la création de processus et l'injection de processus initiée par les applications Office. Pour évaluer l'impact de ces règles, déployez-les en mode audit
* Activez AMSI pour Office VBA si vous disposez d'Office 365
* Utilisez le pare-feu Windows Defender et votre pare-feu réseau pour empêcher les communications RPC et SMB entre les terminaux lorsque cela est possible. Cela limite les mouvements latéraux ainsi que les autres activités d'attaque
* Utilisez [Systems Manager et Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) pour vérifier si les instances EC2 exécutant Microsoft Windows contiennent des vulnérabilités et expositions courantes (CVE)

### Utilisez AWS Config pour afficher la conformité de la configuration :
1. Connectez-vous à AWS Management Console et ouvrez la console AWS Config à l'adresse https://console.aws.amazon.com/config/
1. Dans le menu AWS Management Console, vérifiez que le sélecteur de région est défini sur une région prenant en charge les règles AWS Config. Pour obtenir la liste des régions prises en charge, consultez AWS Config Regions and Endpoints dans la référence générale Amazon Web Services.
1. Dans le volet de navigation, choisissez Ressources. Sur la page Inventaire des ressources, vous pouvez filtrer par catégorie de ressources, par type de ressource et par statut de conformité. Choisissez Inclure les ressources supprimées le cas échéant. Le tableau affiche l'identificateur de ressource pour le type de ressource et l'état de conformité des ressources pour cette ressource. L'identificateur de ressource peut être un ID de ressource ou un nom de ressource
1. Choisissez une ressource dans la colonne Identificateur de ressource
1. Cliquez sur le bouton Chronologie des ressources. Vous pouvez filtrer par événements de configuration, événements de conformité ou événements CloudTrail.
1. Concentrez-vous spécifiquement sur les événements suivants :
* ebs-in-backup-plan
* ebs-optimized-instance
* ebs-snapshot-public-restorable-check
* ec2-ebs-encryption-by-default
* ec2-imdsv2-check
* ec2-instance-detailed-monitoring-enabled
* ec2-instance-managed-by-systems-manager
* ec2-instance-multiple-eni-check
* ec2-instance-no-public-ip
* ec2-instance-profile-attached
* ec2-managedinstance-applications-blacklisted
* ec2-managedinstance-applications-required
* ec2-managedinstance-association-compliance-status-check
* ec2-managedinstance-inventory-blacklisted
* ec2-managedinstance-patch-compliance-status-check
* ec2-managedinstance-platform-check
* ec2-security-group-attached-to-eni
* ec2-stopped-instance
* ec2-volume-inuse-check

## Procédures d'escalade
- « J'ai besoin d'une décision commerciale quant au moment où la criminalistique EC2 devrait être effectuée »
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun d'eux ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Détection et analyse
### [Utiliser les mesures CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
Recherchez des « pics » d'exfiltration de données. Il est possible qu'un attaquant ait détruit des données et laissé une note de rançon, et dans ce cas, il n'y a aucune possibilité de récupération de données en travaillant avec l'acteur malveillant.
1. Ouvrez la console CloudWatch à l'adresse https://console.aws.amazon.com/cloudwatch/
1. Dans le volet de navigation, choisissez Mesures, puis Toutes les mesures
1. Dans l'onglet Toutes les mesures, sélectionnez la région dans laquelle l'instance est déployée
1. Dans l'onglet Toutes les mesures, entrez le terme de recherche « NetworkPacketsout » et appuyez sur Entrée
1. Sélectionnez l'un des résultats de votre recherche pour afficher les mesures
1. Pour représenter un graphique d'une ou de plusieurs mesures, cochez la case en regard de chaque mesure. Pour sélectionner toutes les mesures, cochez la case dans la ligne d'en-tête du tableau
1. (Facultatif) Pour modifier le type de graphique, choisissez Options de graphique. Vous pouvez ensuite choisir entre un graphique en courbes, un graphique en aires empilées, un graphique à barres, un graphique à secteurs ou un nombre.
1. (Facultatif) Pour ajouter une bande de détection d'anomalies qui affiche les valeurs attendues pour la mesure, choisissez l'icône de détection des anomalies sous Actions en regard de la mesure

### L'Microsoft 365 Defender Threat Intelligence Team
L'équipe a fourni un rapport complet (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifiant les éléments à rechercher par rapport à plusieurs variantes de ransomware

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
GROUP PAR adresse source, adresse de destination, port source, port de destination
COMMANDE PAR BYTE_COUNT DESC
```
1. D'autres exemples de requêtes sont fournis dans le [vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql)

## Confinement
### Isolez immédiatement les ressources affectées
**REMARQUE** : Assurez-vous que vous disposez d'un processus en place pour demander une escalade et une approbation pour isoler les ressources afin de vous assurer qu'une analyse d'impact sur l'activité est effectuée en premier lieu sur l'impact de l'isolement sur les opérations et les flux de revenus actuels.
1. Déterminez si l'instance fait partie d'un groupe Auto Scaling ou si elle est attachée à un équilibreur de charge.
* Groupe Autoscaling : détachez l'instance du groupe
* Elastic Load Balancer : désenregistrez l'instance de l'ELB et supprimez l'instance des groupes cibles
1. Créez un groupe de sécurité *nouveau* qui bloque tout le trafic d'entrée et de sortie. Assurez-vous de supprimer la règle par défaut « autoriser tout » pour le trafic de sortie
1. Attachez le groupe de sécurité *nouveau* aux instances touchées

### Appliquer les NACL basés sur les IOCs réseau pour empêcher le trafic
1. Ouvrez la [console Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. Dans le volet de navigation, choisissez Network ACLs
1. Choisissez Create Network ACL
1. Dans la boîte de dialogue Créer une liste Network ACL, nommez éventuellement votre ACL réseau et sélectionnez l'ID de votre VPC dans la liste VPC. Choisissez ensuite Oui, Créer
1. Dans le volet d'informations, choisissez l'onglet Règles entrantes ou Règles sortantes, en fonction du type de règle que vous devez ajouter, puis choisissez Modifier
1. Dans le numéro de règle, entrez un numéro de règle (par exemple, 100). Le numéro de règle ne doit pas déjà être utilisé dans l'ACL réseau. Nous traitons les règles dans l'ordre, en commençant par le nombre le plus bas
* Nous vous recommandons de laisser des espaces entre les numéros de règles (par exemple 100, 200, 300), plutôt que d'utiliser des nombres séquentiels (101, 102, 103). Cela facilite l'ajout d'une nouvelle règle sans devoir renuméroter les règles existantes.
1. Sélectionnez une règle dans la liste Type. Par exemple, pour ajouter une règle pour HTTP, choisissez HTTP. Pour ajouter une règle permettant d'allow all le trafic TCP, choisissez All TCP. Pour certaines de ces options (par exemple, HTTP), nous remplissons le port pour vous. Pour utiliser un protocole qui n'est pas répertorié, choisissez Custom Protocol Rule
1. (Facultatif) Si vous créez une règle de protocole personnalisée, sélectionnez le numéro et le nom du protocole dans la liste Protocol. Pour plus d'informations, consultez la liste des numéros de protocole IANA
1. (Facultatif) Si le protocole que vous avez sélectionné nécessite un numéro de port, entrez le numéro de port ou la plage de ports séparés par un trait d'union (par exemple, 49152-65535).
1. Dans le champ Source ou Destination (selon qu'il s'agit d'une règle entrante ou sortante), entrez la plage CIDR à laquelle la règle s'applique
1. Dans la liste Autoriser/Refuser, sélectionnez AUTORISER pour autoriser le trafic spécifié ou REFUSER de refuser le trafic spécifié.
1. (Facultatif) Pour ajouter une autre règle, choisissez Ajouter une autre règle et répétez les étapes précédentes si nécessaire.
1. Lorsque vous avez terminé, choisissez Enregistrer
1. Dans le volet de navigation, choisissez Network ACLs, puis sélectionnez l'ACL réseau.
1. Dans le volet d'informations, sous l'onglet Associations de sous-réseaux, choisissez Modifier. Activez la case à cocher Associer du sous-réseau à associer à l'ACL réseau, puis choisissez Enregistrer.

## Éradication
### Supprimez tous les systèmes compromis du réseau.
* Respectez les exigences réglementaires ou la politique interne de l'entreprise pour déterminer si la médico-légale de l'instance EC2 est requise.
* Si la médico-légale des instances est requise OU si les données doivent être récupérées, suivez le [Playbook : EC2 Forensics] (. /docs/EC2_Forensics.md)

### Autres sujets d'intérêt
* [Supprimer les métadonnées du contrôleur de domaine compromises du domaine] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
* Inspecter les sauvegardes pour détecter une infection potentielle

## Récupération
* Il est recommandé de ne pas payer la rançon
* Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
* Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis de données/réputation par rapport au paiement à l'attaquant
* Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
* Visitez https://www.nomoreransom.org/ pour savoir si un déchiffrement est disponible pour la variante du logiciel malveillant infectée par vos données.
* [Supprimer ou faire pivoter les clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [clés utilisateur Root] (https://console.aws.amazon.com/iam/home#security_credential) ; vous pouvez faire pivoter toutes les clés de votre compte si vous ne pouvez pas identifier une ou plusieurs clés spécifiques qui ont été exposées
* [Supprimer les utilisateurs IAM non autorisés] (https://console.aws.amazon.com/iam/home#users.)
* [Supprimer les stratégies non autorisées] (https://console.aws.amazon.com/iam/home#/policies)
* [Supprimer les rôles non autorisés] (https://console.aws.amazon.com/iam/home#/roles)
* [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM. REMARQUE : La suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution * Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
* Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware
* Création de nouvelles instances EC2 à partir d'une AMI de confiance
* Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
* Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant qu'intervenant en cas d'incident, j'ai besoin d'un runbook pour effectuer EC2 Forensics
- En tant qu'intervenant en cas d'incident, j'ai besoin d'une décision commerciale quant au moment où la police judiciaire EC2 devrait être effectuée.
- En tant que répondeur d'incident, je dois activer la journalisation dans toutes les régions qui sont activées, quelle que soit l'intention d'utilisation.
- En tant que répondeur d'incident, je dois pouvoir détecter le crypto mining sur mes instances EC2 existantes

## Articles en arriéré actuel