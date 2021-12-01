# Playbook de réponse aux incidents : informations d'identification IAM compromises
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# Points de contact

Auteur : `Nom de l'auteur \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé exécutif
Ce playbook décrit le processus de réponse lorsque vous constatez une activité non autorisée au sein de votre compte AWS ou que vous pensez qu'une partie non autorisée a accédé à votre compte.

## Indicateurs potentiels de compromis
- Utilisateurs IAM nouveaux ou non reconnus
- Ressources non reconnues ou non autorisées (par exemple EC2, Lambda)
- Augmentations inhabituelles de la facturation
- Notification du chercheur en sécurité
- Notification indiquant que mes ressources ou mon compte AWS peuvent être compromis

## Constatations potentielles d'AWS GuardDuty
- Accès aux informations d'identification : Iutilisateur/comportement anormal
- Évasion de la défense : j'amuser/comportement anormal
- Découverte : Iamuser/comportement anormal
- Exfiltration : Iamuser/comportement anormal
- Impact : Iutilisateur/comportement anormal
- Accès initial : Iutilisateur/comportement anormal
- Pentest : IAMUSER/KaliLinux
- Pentest : je amuser/perroquet Linux
- Pentest : IAMUSER/Pen Toolinux
- Persistance : J'amuser/comportement anormal
- Politique : utilisation des informations d'identification de l'utilisateur et de la racine
- Augmentation des privilèges : Iutilisateur/comportement anormal
- Recon:IAMUSER/Malicious IPCaller
- Recon:Iamuser/Malicious IPcaller.Custom
- Recon:IAMUSER/Torip Caller
- Furtif : J'utilisateur/Cloud Traillogging désactivé
- Stealth : je modifie la politique d'utilisateur/mot de passe
- Accès non autorisé : Iutilisateur/console Login Success.B
- Accès non autorisé : Iutilisateur/Instance Credentialité Exfiltration
- Accès non autorisé : Iutilisateur/interlocuteur IP malveillant
- Accès non autorisé : Iutilisateur/IpCaller.Custom
- Accès non autorisé : IAMUSER/TPIP Caller

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
* **Directive**
* **Détective**
* **Responsible**
* **Préventif**

! [Image] (/images/aws_caf.png)
* * *

### Étapes de réponse
1. [**PREPARATION**] Effectuer un inventaire des actifs
2. [**PREPARATION**] Mettre en œuvre un plan de formation pour identifier et répondre aux informations d'identification IAM exposées
3. [**PREPARATION**] Mettre en œuvre une stratégie de communication pour la réponse aux incidents
4. [**DETECTION**] Identifier l'accès au compte racine (autorisé et non)
5. [**DETECTION**] Identifier les nouveaux utilisateurs IAM ou non reconnus
6. [**DETECTION**] Identifier les ressources non reconnues ou non autorisées (par exemple EC2, Lambda)
7. [**DÉTECTION**] Identifier et trouver les secrets exposés
8. [**DETECTION**] Identifier les augmentations de facturation inhabituelles
9. [**DETECTION**] Répondre à une notification d'AWS ou d'un tiers indiquant que mes ressources ou mon compte AWS pourraient être compromis
10. [**DÉTECTION**] Identifier les informations d'identification d'utilisateur IAM potentiellement non autorisées
11. [**PREPARATION**] Identifier les procédures d'escalade
12. [**DÉTECTION ET ANALYSIS**] Consulter les journaux CloudTrail
13. [**DÉTECTION ET ANALYSIS**] Consulter les journaux de flux VPC
14. [**DÉTECTION ET ANALYSIS**] Consulter les journaux basés sur les terminaux/hôtes
15. [**CONFINEMENT**] Effectuer les actions de confinement appropriées
16. [**ERADICATION**] Passez en revue les résultats de la revue de l'historique des événements CloudTrail pour l'activité par la clé d'accès compromise
17. [**ÉRADICATION**] Passez en revue le document Éviter les frais inattendus
18. [**RECOVERY**] Effectuer les actions de récupération appropriées
19. [**PREPARATION**] Effectuer un scan IAM Prowler
20. [**PREPARATION**] Activer MFA
21. [**PREPARATION**] Vérifiez les informations de votre compte
22. [**PREPARATION**] Utilisez les projets AWS Git pour rechercher des preuves d'utilisation non autorisée
23. [**PREPARATION**] Évitez d'utiliser l'utilisateur racine pour les opérations quotidiennes
24. [**PREPARATION**] Évaluez votre posture générale de sécurité

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Exposition des informations d'identification
* **Catégorie** : exposition aux informations d'identification IAM
* **Ressource** : IAM
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers
* **Sources de journaux : AWS CloudTrail, AWS Config, journaux de flux VPC, Amazon GuardDuty
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
Identifiez tous les utilisateurs existants et disposez d'une liste actualisée des objectifs pour chaque compte

1. Accédez à [AWS Console] (https://console.aws.amazon.com/)
1. Accédez à « Services » et sélectionnez « IAM »
1. Dans le menu de gauche, sélectionnez « Rapport d'identification »
1. Sélectionnez « Télécharger le rapport »
1. Portez une attention particulière à la date à laquelle les comptes ont été créés, le mot de passe utilisé pour la dernière fois et les colonnes du mot de passe
* **NOTE** Si un utilisateur dispose de plusieurs clés d'accès, validez l'objectif de chaque

### Formation
- `Quelle est la formation mise en place pour que les analystes de l'entreprise se familiarisent avec l'API AWS (environnement de ligne de commande), S3, RDS et d'autres services AWS ? `
>>>
Voici les possibilités de détection des menaces et de réponse aux incidents : \
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/) \
[Évaluation de la sécurité en libre-service] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Quels rôles peuvent apporter des modifications aux services de votre compte ? `
- `Quels utilisateurs ces rôles leur sont attribués ? Le moindre privilège est-il respecté ou existe-t-il des utilisateurs de super administrateurs ? `
- `Une évaluation de sécurité a-t-elle été effectuée sur votre environnement, avez-vous une base de référence connue pour détecter des choses « nouvelles » ou « suspectes » ? `

### Technologie de communication
- `Quelle technologie est utilisée au sein de l'équipe/de l'entreprise pour communiquer les problèmes ? Y a-t-il quelque chose d'automatisé ? `
>>>
Téléphone \
E-mail \
AS SES \
AWS SNS \
Slack \
Carillon \
Autre ?
>>>

## Détection

### Accès au compte racine
Il est recommandé de supprimer toutes les clés d'accès associées au compte racine : `. /prowler -c check_112`

### Utilisateurs IAM nouveaux ou non reconnus
Consultez le rapport d'informations d'identification IAM à partir de votre [inventaire des actifs] (. /compromised_IAM_Credentials.md/ #asset -inventaire)
Vérifiez si les utilisateurs IAM disposent de deux clés d'accès actives : `. /prowler -c check_extra712`
Assurez-vous que les stratégies IAM qui autorisent les privilèges administratifs complets \ "* : * \ » ne sont pas créées : `. /prowler -c check_122`
Vérifiez si IAM Access Analyzer est activé et ses résultats sont les suivants : `. /prowler -c check_extra769`

### Ressources non reconnues ou non autorisées (par exemple EC2, Lambda)
instances descriptives AWS ec2
Fonctions de liste AWS Lambda

### À la recherche de secrets
Secret potentiel trouvé dans les données utilisateur de l'instance EC2 : `. /prowler -c check_extra741`
Secret potentiel trouvé dans les variables de fonction Lambda : `. /prowler -c check_extra759`
Secret potentiel trouvé dans les variables de définition de tâches ECS : `. /prowler -c check_extra768`
Secret potentiel trouvé dans la configuration Autoscaling : `. /prowler -c check_extra775`

### Augmentations inhabituelles de la facturation
Pour afficher votre facture AWS, ouvrez le volet [Bills] (https://console.aws.amazon.com/billing/home #) de la console Billing and Cost Management, puis choisissez le mois que vous souhaitez afficher dans le menu déroulant.

Vous pouvez afficher l'historique de vos paiements AWS dans le volet [Commandes et factures] (https://console.aws.amazon.com/billing/home?/paymenthistory) de la console Billing and Cost Management.

Vous pouvez également utiliser [AWS Cost Anomaly Detection] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) pour la surveillance dynamique et les alertes.

Vérifiez votre facture pour connaître les éléments suivants :
* Services AWS que vous n'utilisez normalement pas
* Ressources dans les régions AWS que vous n'utilisez normalement pas
* Un changement significatif dans la taille de votre facture

Vous pouvez utiliser ces informations pour vous aider à supprimer ou à mettre fin aux ressources que vous ne souhaitez pas conserver.

### Notification indiquant que mes ressources ou mon compte AWS peuvent être compromis
Si vous avez reçu une notification d'AWS concernant votre compte, connectez-vous à AWS Support Center, puis répondez à la notification.

Si vous ne parvenez pas à vous connecter à votre compte, utilisez le formulaire Contactez-nous pour demander de l'aide à AWS Support.

Si vous avez des questions ou des préoccupations, créez un nouveau dossier AWS Support dans AWS Support Center.
* **NOTE** : N'incluez pas d'informations sensibles dans votre correspondance, telles que les clés d'accès AWS, les mots de passe ou les informations de carte de crédit.
Utiliser les projets AWS Git pour rechercher des preuves d'utilisation non autorisée

### Identifier les informations d'identification d'utilisateur IAM potentiellement non autorisées
1. Ouvrez la console IAM.
1. Choisissez Utilisateurs dans le volet de navigation.
1. Choisissez chaque utilisateur IAM dans la liste, puis cochez sous Stratégies d'autorisations une stratégie nommée AWSExposedCredentialPolicy_DO_NOT_Remove. 1. Si l'utilisateur possède cette stratégie attachée, vous devez faire pivoter les clés d'accès de l'utilisateur.

## Procédures d'escalade
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun d'eux ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Analyse
Il est fortement recommandé d'exporter les journaux vers une solution de gestion des événements d'incidents de sécurité (SIEM) (telle que Splunk, ELK Stack, etc.) pour faciliter l'affichage et l'analyse de divers journaux pour une analyse plus complète de la chronologie des attaques.

### CloudTrail
Recherchez une activité de connexion inhabituelle
1. Accédez à votre [tableau de bord CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. Dans la marge de gauche, sélectionnez « Historique des événements »
1. Dans la liste déroulante, passez de « Lecture seule » à `Nom de l'événement`
1. Dans le champ de recherche, saisissez `ConsoleLogin` ou `AssumeRole` ou `GetFederationToken` ou `GetCredentialReport` ou `GenerateCredentialReport` et examinez les événements disponibles pour toute activité suspecte.
* **NOTE** UserIdentify apparaîtra sous la forme « type » : « Root"` pour root ou `"type » : « Iamuser"` pour les utilisateurs

Localisez l'ID de clé d'accès IAM et le nom d'utilisateur utilisés pour lancer une instance EC2 suspecte
1. Ouvrez la console CloudTrail, puis choisissez Historique des événements.
1. Sélectionnez le menu déroulant Filtre, puis choisissez Nom de la ressource.
1. Dans le champ Entrer le nom de la ressource, collez l'ID d'instance EC2, puis choisissez Entrée sur votre appareil.
1. Développez le nom de l'événement pour RunInstances.
1. Copiez la clé d'accès AWS et notez le nom d'utilisateur.

Consultez l'historique des événements CloudTrail pour connaître l'activité à l'aide de la clé d'accès compromise
1. Ouvrez la console CloudTrail, puis choisissez Historique des événements dans le volet de navigation.
1. Sélectionnez le menu déroulant Filtre, puis choisissez le filtre de clé d'accès AWS.
1. Dans le champ Enter AWS Access Key, entrez l'ID de clé d'accès IAM compromis.
1. Développez le nom de l'événement pour l'appel d'API RunInstances.
* **NOTE** : Vous ne pouvez afficher l'historique des événements des 90 derniers jours que si vous n'avez déjà configuré une piste qui est enregistrée dans un compartiment S3

### Journaux de flux VPC
Les journaux de flux VPC sont une fonctionnalité qui vous permet de capturer des informations sur le trafic IP vers et depuis les interfaces réseau de votre VPC. Cela peut être utile pour les adresses IP découvertes dans CloudTrail afin de déterminer les types de connexions externes à toutes les ressources publiques.

Pour plus d'informations et des étapes, y compris les requêtes avec Athena, reportez-vous à la [Documentation AWS pour les journaux de flux VPC] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Il est recommandé que l'analyse d'Athena soit incluse dans un livre de jeu distinct et liée à d'autres éléments relatifs.

### Endpoint /basé sur l'hôte
* **Identifiez la dernière heure de création pour un nom d'instance donné** : `aws ec2 describe-instances —query 'Reservations [] .Instances []. {ip : PublicAddress, tm : LaunchTime} '—filters 'Name=Tag:Name, Valeurs= MyInstanceName' | jq 'sort_by (.tm) | inverse |. [0] '`

* **Identifier l'heure de dernière modification pour les fonctions lambda** : `aws lambda list-functions | grep « LastModified"`

## Confinement
1. Désactivez l'utilisateur IAM, créez une clé d'accès IAM de sauvegarde, puis désactivez la clé d'accès compromise
1. Ouvrez la [console IAM] (https://console.aws.amazon.com/iam/), puis collez l'ID de clé d'accès IAM dans la barre de recherche IAM.
1. Choisissez le nom d'utilisateur, puis cliquez sur l'onglet Informations d'identification de sécurité.
1. Dans Mot de passe de console, choisissez Gérer.
* **REMARQUE** : Si le mot de passe AWS Management Console est défini sur Désactivé, vous pouvez ignorer cette étape.
1. Dans Accès à la console, choisissez Désactiver, puis Appliquer.
1. Important : Les utilisateurs dont les comptes sont désactivés ne peuvent pas accéder à AWS Management Console. Toutefois, si l'utilisateur dispose de clés d'accès actives, il peut toujours accéder aux services AWS à l'aide d'appels d'API.
1. Pour la clé d'accès IAM compromise, choisissez Rendre inactif.
1. Désactivez la clé IAM de l'application, créez une clé d'accès IAM de sauvegarde, puis désactivez la clé d'accès compromise
1. Commencez par créer une deuxième clé. Ensuite, modifiez votre application pour utiliser la nouvelle clé.
1. Désactivez (mais ne supprimez pas) la première clé.
1. En cas de problème avec votre application, réactivez temporairement la clé. Lorsque votre application est entièrement fonctionnelle et que la première clé est désactivée, supprimez la première clé.
1. Faire pivoter et supprimer toutes les clés d'accès AWS inactives/compromises
* ** ! ! Assurez-vous de conserver un enregistrement de toutes les clés d'accès supprimées afin de pouvoir continuer à les rechercher dans CloudTrail ! ! **

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

Supprimez tous les utilisateurs IAM que vous n'avez pas créés
1. Connectez-vous à AWS Management Console et ouvrez la [console IAM] (https://console.aws.amazon.com/iam/)
1. Dans le volet de navigation, choisissez Utilisateurs, puis cochez la case en regard du nom d'utilisateur que vous souhaitez supprimer, et non du nom ou de la ligne elle-même.
1. En haut de la page, choisissez Supprimer l'utilisateur.
1. Dans la boîte de dialogue de confirmation, attendez le chargement des dernières informations consultées avant de consulter les données. La boîte de dialogue indique quand chacun des utilisateurs sélectionnés a accédé à un service AWS pour la dernière fois. Si vous tentez de supprimer un utilisateur actif au cours des 30 derniers jours, vous devez cocher une case supplémentaire pour confirmer que vous souhaitez supprimer l'utilisateur actif. Si vous souhaitez continuer, choisissez Oui, Supprimer.

Vous trouverez de la documentation sur la suppression d'autres services sur la page [mettre fin à toutes mes ressources] (https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/).

## Récupération
AWS dispose d'un guide publié pour [Que dois-je faire si je constate une activité non autorisée dans mon compte AWS ?] (https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## Actions préventives
### Prowler IAM Scan
`. /prowler -g contrôle 122, contrôle 111, contrôle 110, contrôle 19, contrôle 18, contrôle 17, contrôle 16, contrôle 15, contrôle 11, contrôle 116, contrôle 12, contrôle 114, contrôle 115, contrôle 14, contrôle 13, contrôle 112, contrôle 119, extra71, extra7100, extra7125, extra7125, extra769, extra769, extra774`

### Activer MFA
Pour une sécurité accrue, il est recommandé de configurer MFA afin de protéger vos ressources AWS. Vous pouvez activer [MFA pour les utilisateurs IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) ou [utilisateur racine du compte AWS] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html). L'activation de MFA pour l'utilisateur racine affecte uniquement les informations d'identification de l'utilisateur racine. Les utilisateurs IAM du compte sont des identités distinctes avec leurs propres informations d'identification, et chaque identité possède sa propre configuration MFA.

### Vérifiez les informations de votre compte
AWS a besoin d'informations de compte précises pour vous contacter et aider à résoudre tout problème de compte. Vérifiez que les informations de votre compte sont correctes.
* Le nom du compte et l'adresse e-mail.
* Vos coordonnées, en particulier votre numéro de téléphone.
* Les autres contacts de votre compte.

### Utiliser les projets AWS Git pour rechercher des preuves d'utilisation non autorisée
AWS propose des projets Git que vous pouvez installer pour vous aider à protéger votre compte :
* [Git Secrets] (https://github.com/awslabs/git-secrets) peut analyser les fusions, les validations et les messages de validation à la recherche d'informations secrètes (c'est-à-dire des clés d'accès). Si Git Secrets détecte des expressions régulières interdites, il peut refuser la publication de ces commits dans des référentiels publics.
* Utilisez [AWS Step Functions et AWS Lambda pour générer Amazon CloudWatch Events] (https://aws.amazon.com/step-functions) à partir d'AWS Health ou d'AWS Trusted Advisor. S'il existe des preuves que vos clés d'accès sont exposées, les projets peuvent vous aider à détecter, enregistrer et atténuer automatiquement l'événement.

### Évitez d'utiliser l'utilisateur racine pour les opérations quotidiennes
La clé d'accès de l'utilisateur racine de votre compte AWS donne un accès complet à toutes vos ressources AWS, y compris vos informations de facturation. Vous ne pouvez pas réduire les autorisations associées à la clé d'accès utilisateur racine de votre compte AWS. Il est recommandé de ne pas utiliser l'accès utilisateur racine, sauf si cela est absolument nécessaire.

Si vous ne disposez pas déjà d'une clé d'accès pour l'utilisateur racine de votre compte AWS, n'en créez pas, sauf si cela est absolument nécessaire. Au lieu de cela, créez un utilisateur IAM pour vous-même avec des autorisations administratives. Vous pouvez vous connecter à AWS Management Console avec l'adresse e-mail et le mot de passe de votre compte AWS pour créer un utilisateur IAM.

### Posture générale de sécurité
Exécutez un [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contre l'environnement afin d'identifier davantage d'autres risques et éventuellement d'autres risques publics non identifiés dans ce playbook.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livret de jeu en même temps que les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant que répondeur aux incidents, j'ai besoin d'un manuel sur la façon de gérer les jetons de session et les clés d'accès après un incident.
- En tant que répondeur d'incident, j'ai besoin d'une méthode pour analyser et supprimer des clés des référentiels de code public
- En tant qu'intervenant en cas d'incident, j'ai besoin d'un décideur clair sur le moment de briser un environnement plutôt que de permettre une exposition continue.
## Articles en arriéré actuel