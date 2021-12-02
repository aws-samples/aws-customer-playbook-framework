# Répondre aux attaques de rançon au sein d'AWS

## Avis

Ce document est fourni à titre informatif uniquement. Il représente
les offres de produits et les pratiques actuelles d'Amazon Web Services
(AWS) à la date de publication de ce document, qui sont assujettis à
changer sans préavis. Les clients sont responsables de créer leurs propres
évaluation indépendante des informations contenues dans ce document et de toute utilisation
des produits ou services AWS, chacun étant fourni « tel quel » sans
garantie de toutes sortes, expresses ou implicites. Ce document n'est pas
créer des garanties, des représentations, des engagements contractuels,
des conditions ou des assurances de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou
concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients
sont contrôlés par des accords AWS, et ce document ne fait pas partie, ni
modifie-t-il tout accord entre AWS et ses clients. \
\
© 2021, Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits
réservé.

## Ransomware

Depuis la première attaque de ransomware enregistrée en 1989 appelée PC Cyborg,
les ransomwares sont devenus une stratégie de monétisation de premier plan utilisée par les criminels
organisations et acteurs de menaces sur Internet. Autres exemples de
les ransomwares incluent :

- CryptoLocker \ [2014 \]
- Petya \ [2016 \]
- WannaCry \ [2017 \]
- NotPetya \ [2017 \]
- Ryuk \ [2019 \])

Les acteurs de menaces exploitent les problèmes et les faiblesses de l'ensemble du client
infrastructure, exploitation de points de terminaison vulnérables, services non sécurisés et
employés d'ingénierie sociale.

Les attaques par ransomware coûtent cher aux gouvernements, aux organisations à but non lucratif et aux entreprises
des milliards de dollars et des opérations interrompues. NotPetya forcé
Maersk, géant de l'expédition, réinstallera 4 000 serveurs et 45 000 PC pour
\ 300 millions de dollars en raison d'une « grave interruption d'activité ». L'attaque de ransomware contre
la ville de Baltimore a coûté plus de 18 millions de dollars, et les gouvernements locaux de
Riviera Beach et Lake City, en Floride, paieront les pirates informatiques \ 1M $ combinés à
récupérer ses systèmes et ses données. Le Federal Bureau of Investigation des États-Unis
(FBI) prévoit que la menace des ransomwares deviendra « plus ciblée,
sophistiqué et coûteux » dans un avenir proche. Ces avertissements atteignent
au-delà des frontières américaines, Europol qualifiant également les ransomwares de « plus
forme de cyberattaque généralisée et financièrement dommageable. »

## Que sont les attaques de rançon ?

Les attaques de rançon sont une stratégie de monétisation et non une technologie spécifique.
Les attaques font souvent appel à des logiciels malveillants spécifiques, ou ransomware, qui
menace de publier des informations sur les victimes ou de bloquer l'accès jusqu'à la rançon
est payé.

En théorie, si la rançon est payée dans le délai imparti, les systèmes et
les données sont déchiffrées et rendues disponibles une fois de plus et les opérations normales
continuer. Toutefois, si la rançon n'est pas satisfaite, les organisations risquent
destruction permanente ou fuites de données publiques contrôlées par le
attaquant. Les attaquants peuvent également extorquer davantage le client pour la libération de
données et systèmes sensibles aux clients à des tiers ou au public.

## Les ransomwares ne s'en soucient pas

De nombreuses attaques de rançon sont opportunistes, ce qui signifie que les rançongiciels
infecte sans discernement tous les réseaux accessibles par l'intermédiaire d'humains et/ou
vecteurs de machines. Les équipes de sécurité pour les établissements d'enseignement, l'État et
les gouvernements locaux et les organisations de soins de santé intensifient les mesures
pour protéger leurs données contre une augmentation des attaques de ransomwares. Menace
les acteurs comprennent comment identifier les points faibles dans les secteurs verticaux de l'industrie. Nombreux
l'éducation et les organisations gouvernementales sont plus sujettes aux ransomwares dus
à une combinaison de budgets en baisse, de lacunes dans les ressources de sécurité et
systèmes informatiques hérités présentant des vulnérabilités connues non corrigées. De même,
les attaques de rançon peuvent cibler des industries intolérantes aux temps d'arrêt, comme
hôpitaux, dans l'espoir d'augmenter la probabilité de paiement.

## Pourquoi les attaques de rançon sont-elles efficaces ?

- La sensibilisation des employés à la sécurité est faible
- Les organisations ne sauvegardent pas de données ou ne parviennent pas à tester les sauvegardes existantes
- Les attaques nécessitent peu de compétences et entraînent des gains importants
- Les entreprises sont lentes à corriger les vulnérabilités et expositions courantes critiques (CVE)
- Le personnel technique surchargé ne peut pas combler ou anticiper toutes les failles de sécurité
- Plusieurs vecteurs ou canaux sont utilisés dans une seule attaque
- Le vol des données (exfiltration de données, copie non autorisée) peut ne pas arrêter un processus commercial, et les clients peuvent ne pas réagir tant que le chiffrement ou la suppression des informations

## Pour payer ou ne pas payer ?

Des débats actifs existent entre les professionnels de la cybersécurité concernant le
décision de payer des rançons. De nombreux experts, dont le FBI, conseillent
organisations de ne pas payer la rançon, arguant que le paiement n'est pas
garantir que les systèmes verrouillés ou les données chiffrées seront disponibles
et ne fera que continuer à motiver des comportements néfastes. Même si
l'accès au système et aux données ne sont pas garantis après avoir payé la rançon, certains
les organisations prennent un risque calculé pour payer dans l'espoir de reprendre la normale
opérations. Ce faisant, ils espèrent réduire les coûts accessoires potentiels
des attaques, y compris une perte de productivité, une diminution des revenus au fil du temps,
exposition à des données sensibles et atteinte à la réputation. Le ransomware
la menace est grave, mais une préparation intelligente et une vigilance continue sont
des compteurs efficaces contre elle. L'armure complète de la sécurité des données comprend :
contrôles humains et techniques, mais il existe des fonctionnalités d'AWS
Cloud qui aide à atténuer les attaques par ransomware. AWS s'engage à :
vous fournir des outils, des meilleures pratiques et des services pour vous aider à
disponibilité, sécurité et résilience pour s'attaquer aux mauvais acteurs du
Internet.

## La sécurisation des systèmes et des données sur AWS est une [responsabilité partagée] (https://aws.amazon.com/compliance/shared-responsibility-model)

Lorsque vous déployez des applications et une infrastructure dans le cloud AWS, AWS
aide en partageant les responsabilités en matière de sécurité avec vous. Ingénieurs AWS
l'infrastructure cloud sous-jacente utilisant des principes de conception sécurisée et
les clients doivent implémenter leur propre architecture de sécurité pour les charges de travail
déployé sur AWS. AWS est responsable de la protection de l'infrastructure.
qui exécute tous les services proposés sur le cloud AWS. Cette infrastructure
est composé du matériel, du logiciel, de la mise en réseau et des installations qui
exécutez les services AWS Cloud. Les services cloud AWS sélectionnés par un client
déterminer la quantité de travail de configuration que le client doit effectuer
une partie de leurs responsabilités en matière de sécurité. Les clients sont toujours
responsable de la gestion et de la sécurisation de leurs données, en classant leurs
et en utilisant AWS Identity Access Management (IAM) pour appliquer le
autorisations appropriées.

## Support AWS

Si vous ou vos clients croyez qu'un compte fait l'objet d'une rançon active
attaque, le client concerné doit créer un dossier à partir d'AWS Support
Centrez dans le compte touché. Si l'utilisateur racine ne peut pas être
accédé ou est compromis, le client doit alors ouvrir un nouvel AWS
et ouvrez un ticket d'assistance à partir de là. Ce processus permet à AWS de
effectuer une vérification de l'identité avec le client et commencer à s'engager
ressources internes pour vous aider au mieux à remédier à l'événement.

## AWS Security Reference Architecture (AWS SRA)

Architecture de référence de sécurité [Amazon Web Services (AWS) [Amazon Web Services (AWS)
SRA)] (https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
est un ensemble holistique de lignes directrices pour déployer l'intégralité d'AWS
services de sécurité dans un environnement multi-comptes. Il peut être utilisé pour aider
concevoir, implémenter et gérer les services de sécurité AWS afin qu'ils s'alignent
avec les meilleures pratiques AWS.

## Agence américaine de cybersécurité et de sécurité des infrastructures

Le guide [Ransomware » (sept.
2020)] (https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf)
fournit des bonnes pratiques et des références pour aider à gérer le risque posé par
ransomware et soutien coordonné et efficace d'une organisation
réponse à un incident de ransomware.

## L'Agence de cybersécurité de l'Union européenne (ENISA)

Le paysage des menaces [ENISA 2020 -
Ransomware] (https://www.enisa.europa.eu/publications/ransomware)
présente les résultats sur les ransomwares, fournit une description et une analyse
du domaine, et répertorie les incidents récents pertinents. Une série de propositions
des mesures d'atténuation sont fournies.

# # Centre australien de cybersécurité (ACSC)

L'ACSC fournit plusieurs guides, y compris la réponse à [Ransomware dans
Australie] (https://www.cyber.gov.au/sites/default/files/2020-10/Ransomware%20in%20Australia%20%28October%202020%29.pdf), [RANSOMWARE
RÉPONSE D'URGENCE AUX ATTAQUES
GUIDE] (https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Emergency-Response-Guide_Accessible_08.12.20.pdf),
et [PRÉVENTION ET PROTECTION DES ATTAQUES DE RANSOMWARE]
GUIDE] (https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Prevention-And-Protection-Guide_Accessible_08.12.20.pdf).

## Cadre de cybersécurité NIST

Le guide NIST Cybersecurity Framework (CSF) : Alignement sur le CSF NIST
dans le cloud AWS, est conçu pour aider les secteurs commercial et public
des entités de toutes tailles et de n'importe quelle partie du monde s'alignent sur le CSF par
à l'aide de services et de ressources AWS.

## Atténuation des ransomwares : les 5 principales mesures de protection et de préparation de la récupération

AWS a fourni un blog public sur la sécurité couvrant [l'atténuation des ransomwares :
Top 5 des protections et de la préparation à la récupération
actions.] (https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)
Il s'agit notamment de :

- Configurez la possibilité de récupérer vos applications et vos données
- Cryptez vos données
- Appliquer des correctifs critiques
- Respectez une norme de sécurité
- Assurez-vous de surveiller et d'automatiser les réponses

Vous devez également utiliser une authentification forte pour l'accès distant exposé
services, notamment le protocole Bureau à distance et le Secure Shell. [Multifactoriel
authentification combinée à un seul
connexion] (https://docs.aws.amazon.com/singlesignon/latest/userguide/mfa-how-to.html)
est un mécanisme technique qui permet de protéger les connexions distantes
et les demandes de ressources.

## Développement, sécurité et opérations (DevSecOps)

*Arrière* : Le but de DevSecOps est d'aider les clients en toute sécurité
accélérer les boucles de rétroaction avec leurs clients (c'est-à-dire les utilisateurs finaux). Par
en accélérant ces boucles de rétroaction, les clients peuvent expédier plus de logiciels
changements apportés à la production avec une qualité, une sécurité et une stabilité supérieures. Par
en intégrant la sécurité dans le processus de développement, les clients peuvent empêcher
déploiement de vulnérabilités auprès des utilisateurs finaux dans des environnements de production.
Selon le NIST, le coût de la correction d'un défaut de sécurité une fois qu'il est
fabriqué à la production peut être jusqu'à 60 fois plus cher que pendant
le cycle de développement
\ [[Source] (https://securityboulevard.com/2020/09/the-importance-of-fixing-and-finding-vulnerabilities-in-development/) \].

Sur la base des données de l'équipe de réponse aux incidents client AWS, la plupart des
les attaques de rançon des clients relèvent de l'une des trois catégories suivantes : 1/ Un mauvais
actor analyse les référentiels de code source (par exemple, GitHub) à la recherche de clés d'accès AWS.
À l'aide de ces clés d'accès à long terme, ils ont accès aux ressources AWS dans
comptes clients. Les risques augmentent en fonction du niveau d'accès
autorisé par les clés d'accès. 2/ En scannant les godets S3 publics et
objets, un mauvais acteur peut bloquer l'accès et chiffrer les compartiments S3 empêchant
les propriétaires n'accèdent pas aux données. 3/ Un mauvais acteur analyse et exploite le Web
vulnérabilités d'applications (p. ex. injection de code et intersite
script) sur les ports publics pour accéder aux données des clients.

Les clients croient à tort qu'AWS fournit bon nombre de ces mesures de protection
automatiquement dans le cadre de la création d'un compte AWS. Alors qu'AWS fournit
des milliers de fonctionnalités de sécurité, y compris des fonctionnalités privées par défaut
(par exemple, les groupes de sécurité EC2 et les compartiments S3), dans le cadre du [partagé
responsabilité
modèle] (https://aws.amazon.com/compliance/shared-responsibility-model/),
les clients doivent détecter et corriger les vulnérabilités de sécurité dans leur
code et/ou dans leurs environnements AWS. Il existe des services et des outils AWS
pour détecter et corriger ces ressources non conformes, mais
les clients doivent avoir des connaissances dans l'application des pratiques de manière automatique
à travers leurs ressources AWS.

Un pipeline de déploiement rompt les actions de génération, de test, de déploiement et de publication
en une série d'étapes. Chaque étape apporte une confiance croissante,
généralement au prix du temps supplémentaire. Les premiers stades peuvent trouver la plupart des problèmes
générant un retour d'information plus rapide, tandis que les étapes ultérieures offrent plus lentement et plus
sondage approfondi. Les constructeurs automatisent bon nombre des actions de ces
pipelines de déploiement ; les pipelines de déploiement sont une architecture clé
construction de [Continuous
Livraison] (https://aws.amazon.com/devops/continuous-delivery/). Le
le travail du pipeline de déploiement consiste à détecter tout changement qui conduira à
problèmes de production. Il peut s'agir de performances, de sécurité ou
problèmes d'utilisabilité
\ [[Source] (https://martinfowler.com/bliki/DeploymentPipeline.html) \].
Pour les besoins de ce document, nous nous référons à la pratique de la construction
tests de sécurité et vérifications dans les pipelines de déploiement afin qu'ils puissent empêcher
et atténuez les attaques de rançon en tant que sécurité continue.

Les principes de l'utilisation de la sécurité continue pour prévenir et
atténuer les attaques de rançon sont les suivantes : 1/ Empêcher de commettre des secrets à la source
référentiels de code afin que les mauvais acteurs ne disposent pas de clés pour accéder à AWS
ressources. 2/ Prévenir les erreurs courantes des développeurs qui font du Web
applications vulnérables (statiques et d'exécution). 3/ Assurez-vous qu'il y a
sauvegardes pour des ressources AWS compromises afin que les mauvais acteurs n'aient pas
beaucoup à gagner en bloquant l'accès aux ressources. 4/ Chiffrer tout
des données pertinentes afin que si un mauvais acteur ait accès aux données, il a
beaucoup moins à gagner grâce à l'extorsion. 5/ Détecter quand des utilisateurs non autorisés
accéder aux ressources AWS pour que des mesures d'atténuation puissent avoir lieu. 6/ Assurez-vous que IAM
les clés d'accès sont de courte durée en utilisant les rôles IAM, de sorte que l'accès est limité
la durée des ressources compromises. 7/ Assurez-vous que les autorisations IAM utilisent le moins
privilège d'empêcher que de très mauvaises choses ne se produisent si un mauvais acteur
obtient l'accès aux clés. 8/ Empêcher ou détecter les erreurs de configuration
rendre les ressources AWS vulnérables.

*Prévenir et détecter lors de la validation de secrets dans le dépôt de code source*** :**
À partir de leurs environnements de développement (par exemple, leurs EDI), les clients doivent
Exécuter la détection de secrets Outils de test de sécurité des applications statiques (SAT)
avant de valider le code dans un dépôt de code source. En outre, le premier
étape d'un pipeline de déploiement automatisé exécute une détection de secrets SAST
pour détecter quand des secrets ont été validés dans le dépôt de code source.
Lorsque vous découvrez des secrets, configurez le pipeline pour qu'il échoue, notifiez
développeurs, et arrêtez les validations futures jusqu'à l'application d'un correctif. En outre,
exécutez l'automatisation pour nettoyer l'historique git des secrets et désactiver AWS
clés d'accès. Voici des exemples d'outils pour cette solution : AWS CodePipeline, AWS
CodeBuild, git-secrets et code personnalisé pour corriger les secrets.

*Prévenez et détectez les erreurs courantes des développeurs qui créent des applications
vulnérable*** :** À partir de leurs environnements de développement (par exemple, leurs IDE),
les clients doivent exécuter des outils SAT et de révision du code qui détectent le Web
vulnérabilités telles que l'injection de code, les scripts intersites et autres
pratiques de codage non sécurisées. En outre, la première étape d'un système automatisé
Deployment Pipeline exécute ces outils pour détecter les vulnérabilités Web
sont engagés dans le dépôt de code source. Lors de la découverte de ces sites Web
vulnérabilités, configurez le pipeline pour qu'il échoue, avertissez les développeurs et
arrêtez les prochains commits jusqu'à l'application d'un correctif. De plus, les clients peuvent
configurer la détection d'exécution et la correction de ces vulnérabilités Web
dans le cadre d'un pipeline de déploiement de services partagés qui approvisionne et
configure ces services en tant que code. Voici des exemples d'outils pour cette solution :
Amazon CodeGuru, AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
SonarQube/CheckMarx et AWS WAF et WAF Security Automations.

*Sauvegardes pour les ressources AWS compromises :* Lors de l'exécution, détectez les données non balisées
et/ou des ressources AWS pertinentes qui ne disposent pas de sauvegardes. En outre,
les clients peuvent configurer le provisionnement des ressources AWS qui fournissent
détection d'exécution dans le cadre d'un pipeline de déploiement de services partagés
provisionner et configurer ces services en tant que code. Exemples d'outils pour cela
sont AWS Backup, AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Règles] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(par exemple, required-tags, la redshift-backup-enabled,
aurora-resources-protected-by-backup-plan,
backup-plan-min-frequency-and-min-retention-check,
backup-recovery-point-manual-deletion-disabled,
db-instance-backup-enabled, dynamodb-in-backup-plan,
ebs-in-backup-plan), Amazon SNS, and -.

*Chiffrement des données en transit et au repos*** :** De leur développeur
environnements (par exemple, leurs IDE), les clients doivent exécuter des outils SAT qui
détecter les vulnérabilités Web telles que l'injection de code et intersite
script. En outre, la première étape d'un déploiement automatisé
pipeline exécute un outil de linting iAC pour détecter quand les développeurs ont défini
déchiffrer les ressources AWS sous forme de code et valider le code de configuration dans
le dépôt de code source. Lorsque vous découvrirez ces ressources non chiffrées,
configurer le pipeline pour qu'il échoue, avertisse les développeurs et arrête l'avenir
valide jusqu'à l'application d'un correctif. En outre, les clients peuvent configurer le
provisionnement de ressources AWS qui permettent la détection d'exécution et
correction de ces ressources AWS non chiffrées dans le cadre d'un partage
pipeline de déploiement de services qui approvisionne et configure ces
services sous forme de code. Voici des exemples d'outils pour cette solution : AWS
CloudFormation, AWS CodePipeline, AWS CloudFormation Guard, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Règles] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(par exemple, ec2-ebs-encryption-by-default, dynamodb-table-encrypted-kms,
rds-snapshot-encrypted, cloudwatch-log-group-encrypted,
cloud-trail-encryption-enabled Trail,
Encryptage côté serveur s3 compartiments, api-gw-ssl-enabled,
cloudfront-custom-ssl-certificate), Amazon SNS, AWS KMS et AWS Lambda.

*Détecter quand des utilisateurs non autorisés accèdent aux ressources AWS :* Au moment de l'exécution,
détecter l'exfiltration de données \ "pics \ » (par exemple, à l'aide de métriques CloudWatch
en particulier
[NetworkOut] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)).
Par exemple, il est possible qu'un attaquant ait procédé à la destruction des données et
a laissé une note de rançon, et dans ces cas, il n'y a pas de possibilité de données
récupération en travaillant avec l'acteur malveillant. Qui plus est, les clients
peut configurer le provisionnement des ressources AWS qui fournissent un environnement d'exécution
détection dans le cadre d'un pipeline de déploiement de services partagés qui
provisionner et configurer ces services en tant que code. Exemples d'outils pour cela
sont AWS CloudFormation, AWS CodePipeline, AWS CodeBuild, Amazon
EventBridge, Amazon GuardDuty, Amazon SNS et Amazon CloudWatch
métriques.

*Assurez-vous que les clés d'accès IAM sont courtes*** :** Au moment de l'exécution, exécutez des outils qui
détecter divers états des clés d'accès IAM et des stratégies, y compris la rotation
clés d'accès à longue durée de vie. Les clients peuvent configurer le provisionnement d'AWS
ressources qui permettent la détection d'exécution dans le cadre d'un service partagé
pipeline de déploiement qui approvisionne et configure ces services en tant que
code. Voici des exemples d'outils pour cette solution : AWS CloudFormation, AWS
CodePipeline, AWS CodeBuild, Amazon EventBridge, [AWS Config
Règles] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(p. ex., iam-policy-no-statements-with-admin-access,
iam-policy-no-statements-with-full-access, iam-root-access-key-check,
iam-user-no-policies-check, iam-user-unused-credentials-check,
access-keys-rotated) et AWS Lambda.

*Assurez-vous que les autorisations IAM utilisent le moindre privilège*** :** À partir des développeurs
environnements (par exemple, leurs IDE), les clients doivent exécuter des outils SAT qui
détecter les définitions d'autorisations trop permissives avant de valider
code vers un dépôt de code source. En outre, la première étape d'un système automatisé
pipeline de déploiement exécute l'outil SAT pour détecter les données trop permissives
définitions de stratégie une fois que le code a été validé dans un code source
dépôt. Lorsque vous découvrez des définitions de stratégies trop permissives, configurez
le pipeline doit échouer, avertir les développeurs et arrêter les validations futures jusqu'à
application d'un correctif. De plus, détectez les politiques trop permissives dans l'ensemble du
Compte ou comptes AWS (c.-à-d. en dehors des dépôts de code source). À son tour,
les clients peuvent configurer le provisionnement des ressources AWS qui fournissent
détection d'exécution dans le cadre d'un pipeline de déploiement de services partagés
provisionner et configurer ces services en tant que code. Exemples d'outils pour cela
sont AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
PMapper/AWSPX, AWS IAM Access Analyzer, Amazon EventBridge, AWS Config
Règles et AWS Lambda.

*Prévenir les erreurs de configuration qui rendent les ressources AWS vulnérables* : Le
une mauvaise configuration de certaines ressources AWS ou ne pas activer certaines ressources AWS peut entraîner
les clients vulnérables à une attaque de rançon. Les ressources les plus remarquables sont
ceux liés à la mise en réseau, au calcul, au stockage et aux bases de données. Qu'est-ce que
De plus, les clients peuvent configurer le provisionnement des ressources AWS qui
fournir une détection d'exécution dans le cadre d'un déploiement de services partagés
pipeline qui approvisionne et configure ces services en tant que code. Exemple
les outils de cette solution sont AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, AWS Config Rules (p. ex.
s3-account-level-public-access-blocks, s3-bucket-default-lock-enabled,
Accès public au niveau du compartiment s3 interdit, activation de la journalisation des compartiments s3,
s3-bucket-policy-not-more-permissive, s3-bucket-public-read-prohibited,
s3-bucket-public-write-prohibited, s3-bucket-replication-enabled,
cloud-trail-log-file-validation-enabled,
ec2-security-group-attached-to-eni, vpc-default-security-group-closed,
et les journaux de flux vpc, activés par les guardduty-enabled-centralized), Amazon
Inspector Network Reachability, Amazon VPC Reachability Analyzer, Amazon
S3 et AWS Lambda.

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Microsoft Windows

### Identifier
- Évaluer la posture de sécurité du compte afin d'identifier et de remédier aux failles de sécurité
- AWS a développé un nouvel outil open source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS compte.
- Maintenez un inventaire complet des ressources de toutes les ressources, y compris les contrôleurs de domaine, les instances Microsoft Windows EC2, les serveurs et bases de données Microsoft Windows, et toute intégration avec des fournisseurs d'identité externes.
- Effectuez une analyse de vulnérabilité récurrente de vos hôtes à l'aide d'utilitaires tels que [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
- Pour protéger votre organisation, Microsoft recommande d'utiliser les informations contenues dans la présentation PowerPoint [**Human-Operated Ransomware Mitigation Project Plan**] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx), qui inclut [sécurisation accès privilégié] (https://aka.ms/spa).
- Utilisez [métriques CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), en particulier NetworkPacketsOut, pour rechercher des « pics » d'exfiltration de données. Il est possible qu'un attaquant ait détruit des données et laissé une note de rançon, et dans ce cas, il n'y a aucune possibilité de récupération de données en travaillant avec l'acteur malveillant.

### Protéger
- Appliquez les dernières mises à jour à vos systèmes d'exploitation et applications
- Sauvegardez vos fichiers avec l'historique des fichiers si le fabricant de votre PC ne l'a pas déjà allumé. [En savoir plus sur l'historique des fichiers] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
- Sauvegardez régulièrement des fichiers importants. Utilisez la règle 3-2-1. Conservez trois sauvegardes de vos données, sur deux types de stockage différents, et au moins une sauvegarde hors site
- Bloquer les types de fichiers Ransomware connus
- [Bloquer les macros dans les documents Office] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
- Éduquez vos employés afin qu'ils puissent identifier les attaques d'ingénierie sociale et d'hameçonnage de lance
- [Suivez les bonnes pratiques pour sécuriser vos services Active Directory] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
- [Suivez les 10 principaux paramètres de stratégie de groupe pour prévenir les failles de sécurité] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
- [Implémenter l'accès contrôlé aux dossiers] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). Il peut empêcher les ransomwares de crypter des fichiers et de conserver les fichiers pour une rançon
- Effectuer des sauvegardes d'instances EC2
- Envisagez d'utiliser [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) ou [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
- L'Microsoft 365 Defender Threat Intelligence Team a fourni [rapport complet] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifiant les actions visant à sécuriser vos ressources Microsoft avant un événement de ransomware
- Renforcez les actifs connectés à Internet et assurez-vous qu'ils disposent des dernières mises à jour de sécurité. Utilisez la gestion des menaces et des vulnérabilités pour vérifier régulièrement ces ressources afin de détecter les vulnérabilités, les erreurs de configuration et les activités suspectes.
- Surveillez les tentatives de force brute. Vérifier les tentatives excessives d'authentification échouées (ID d'événement de sécurité Windows 4625)
- Surveillez la suppression des journaux d'événements, en particulier le journal des événements de sécurité et les journaux opérationnels PowerShell. Microsoft Defender ATP déclenche l'alerte « Event log was cleared » et Windows génère un ID d'événement 1102 lorsque cela se produit.
- Pratiquez le principe du moindre privilège et maintenez l'hygiène des accréditations. Évitez l'utilisation de comptes de service de niveau administrateur à l'échelle du domaine. Appliquez des mots de passe d'administrateur locaux robustes randomisés juste à temps. Utilisez des outils tels que LAPS
- Secure Remote Desktop Gateway à l'aide de solutions telles que l'authentification multifacteur Azure (MFA). Si vous ne disposez pas d'une passerelle MFA, activez l'authentification au niveau du réseau (NLA)
- Activez AMSI pour Office VBA si vous disposez d'Office 365
- Activez les règles de réduction de la surface d'attaque, y compris les règles qui bloquent le vol d'informations d'identification, l'activité de ransomware et l'utilisation suspecte de PsExec et de WMI. Pour traiter les activités malveillantes initiées via des documents Office armés, utilisez des règles qui bloquent l'activité avancée des macros, le contenu exécutable, la création de processus et l'injection de processus initiée par les applications Office. Pour évaluer l'impact de ces règles, déployez-les en mode audit
- Activez la protection fournie par le cloud et la soumission automatique des échantillons sur Windows Defender Antivirus. Ces fonctionnalités utilisent l'intelligence artificielle et l'apprentissage automatique pour identifier et arrêter rapidement les menaces nouvelles et inconnues
- Activez les fonctions de protection contre les sabots pour empêcher les attaquants d'arrêter les services de sécurité
- Utilisez le Windows Defender Firewall et votre système de sécurité réseau pour empêcher la communication RPC et SMB entre les terminaux chaque fois que possible. Cela limite les mouvements latéraux ainsi que les autres activités d'attaque.
- Activer et mettre à jour Windows Defender Antivirus - Les solutions EDR (Endpoint Detection and Response) commerciales payantes par abonnement sont préférées
- Activez [Controlled Folder Access] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) dans Windows 10 pour protéger vos dossiers locaux importants contre les programmes non autorisés tels que les rançongiciels ou autres logiciels malveillants
- Utilisez [Systems Manager et Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) pour vérifier si les instances EC2 exécutant Microsoft Windows contiennent des vulnérabilités et expositions courantes (CVE)
- Vérifiez vos sauvegardes et assurez-vous que l'infection ne s'y est pas propagée

### Détecter
- L'Microsoft 365 Defender Threat Intelligence Team a fourni un [rapport complet] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifiant les éléments à rechercher par rapport à plusieurs variantes de ransomware
- Utiliser VPCFlowLogs pour identifier les accès inappropriés à la base de données à partir d'adresses IP externes
- Vous pouvez [étudier le flux VPC avec Amazon Detective] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) ou [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Répondre
- Appliquer les NACL basés sur les IOCs réseau pour empêcher le trafic supplémentaire
- Isoler les systèmes infectés
- Inspecter les sauvegardes pour détecter une infection potentielle
- Supprimez tous les systèmes compromis du réseau.
- Respectez les exigences réglementaires ou la politique interne de l'entreprise pour déterminer si la police judiciaire de l'instance EC2 est requise
- Si la médico-légale des instances est requise OU que les données doivent être récupérées, [mettez en quarantaine l'instance EC2] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)
- [Supprimer les métadonnées du contrôleur de domaine compromises du domaine] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
- Utilisez les journaux de flux pour déterminer l'adresse IP d'exfiltration, rechercher toutes les autres instances communiquant avec les adresses IP auxquelles les données ont été exfiltrées vers CloudTrail et y ont accès

### Récupérer
- Il est recommandé de ne pas payer la rançon
- Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
- Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis données/réputation par rapport au paiement à l'attaquant
- Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
- Créer de nouvelles instances EC2 à partir d'une AMI de confiance
- Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
- Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware
- Visitez < https://www.nomoreransom.org/ > la page pour savoir si un déchiffrement est disponible pour la variante du logiciel malveillant qui a infecté vos données

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Unix/Linux

### Identifier
- Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
- AWS a développé un nouvel outil open source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle pour obtenir rapidement des informations précieuses sur la posture de sécurité de leur compte AWS.
- Maintenir un inventaire complet de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages réseau/fichiers et les machines de développement
- Effectuez une analyse de vulnérabilité récurrente de vos hôtes à l'aide d'utilitaires tels que [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
- Utilisez [métriques CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), en particulier NetworkOut, pour rechercher des « pics » d'exfiltration de données. Il est possible qu'un attaquant ait détruit des données et laissé une note de rançon, et dans ce cas, il n'y a aucune possibilité de récupération de données en travaillant avec l'acteur malveillant.

### Protéger
- Appliquez les dernières mises à jour à vos systèmes d'exploitation et applications
- Sauvegardez régulièrement des fichiers importants. Utilisez la règle 3-2-1. Conservez trois sauvegardes de vos données, sur deux types de stockage différents, et au moins une sauvegarde hors site
- Bloquer les types de fichiers Ransomware connus
- Envisagez d'utiliser AWS Config pour créer des règles permettant de surveiller les modifications apportées aux services AWS. AWS Config dispose de plusieurs règles gérées préconçues pour surveiller EBS et EC2, notamment :
- [ebs-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-in-backup-plan.html)
- [ebs-optimized-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-optimized-instance.html)
- [ebs-snapshot-public-restorable-check] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-snapshot-public-restorable-check.html)
- [ec2-ebs-encryption-by-default] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-ebs-encryption-by-default.html)
- [ec2-imdsv2-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-imdsv2-check.html)
- [ec2-instance-detailed-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-detailed-monitoring-enabled.html)
- [ec2-instance-managed-by-systems-manager] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-systems-manager.html)
- [ec2-instance-multiple-eni-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-multiple-eni-check.html)
- [ec2-instance-no-public-ip] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-no-public-ip.html)
- [ec2-instance-profile-attached] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-profile-attached.html)
- [ec2-managedinstance-applications-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-blacklisted.html)
- [ec2-managedinstance-applications-required] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-required.html)
- [ec2-managedinstance-association-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-association-compliance-status-check.html)
- [ec2-managedinstance-inventory-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-inventory-blacklisted.html)
- [ec2-managedinstance-patch-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-patch-compliance-status-check.html)
- [ec2-managedinstance-platform-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-platform-check.html)
- [ec2-security-group-attached-to-eni] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html)
- [ec2-stopped-instance ec2-stopped-] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-stopped-instance.html)
- [ec2-volume-inuse-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-volume-inuse-check.html)
- Éduquez vos employés afin qu'ils puissent identifier les attaques d'ingénierie sociale et d'hameçonnage de lance
- Activez la détection des logiciels malveillants et les antivirus sur tous les systèmes - les solutions de détection et de réponse des points de terminaison (EDR) commerciales payantes par abonnement sont préférées.
- Un exemple de guide peut être trouvé sur [How to Install and Use Linux Malware Detect (LMD) avec ClamAV comme moteur antivirus] (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
- Effectuer des sauvegardes d'instances EC2
- Envisagez d'utiliser [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) ou [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
- L'Microsoft 365 Defender Threat Intelligence Team a fourni un [rapport complet] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) identifiant les actions visant à sécuriser les ressources Microsoft avant un événement de ransomware. Bon nombre de ces recommandations peuvent être adaptées à Unix et Linux, notamment :
- Déterminez où les comptes hautement privilégiés se connectent et exposent les informations d'identification.
- Renforcez les actifs connectés à Internet et assurez-vous qu'ils disposent des dernières mises à jour de sécurité. Utilisez la gestion des menaces et des vulnérabilités pour vérifier régulièrement ces ressources afin de détecter les vulnérabilités, les erreurs de configuration et les activités suspectes.
- Surveillez les tentatives de force brute. Vérifier les tentatives excessives d'authentification échouées (/var/log/auth.log ou /var/log/secure) /var/log/auth.log
- Surveillance de l'effacement des journaux d'événements
- Pratiquez le principe du moindre privilège et maintenez l'hygiène des accréditations. Évitez l'utilisation de comptes de service de niveau administrateur à l'échelle du domaine. Appliquez des mots de passe d'administrateur locaux robustes randomisés juste à temps.
- Activez les fonctions de protection contre les sabots pour empêcher les attaquants d'arrêter les services de sécurité
- Utiliser un agent de sécurité des points de terminaison tel que [Wazuh] (https://documentation.wazuh.com/current/getting-started/components/index.html)
- Utilisez un moniteur d'intégrité des fichiers tel que [Tripwire] (https://github.com/Tripwire/tripwire-open-source) pour détecter les modifications apportées aux fichiers critiques
- Utilisez [Systems Manager et Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) pour vérifier si les instances EC2 contiennent des vulnérabilités et expositions courantes (CVE)
- Vérifiez vos sauvegardes et assurez-vous que l'infection ne s'y est pas propagée

### Détecter
- Le tableau 1 de [No Strings on Me : Linux and Ransomware] (https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870), de Richard Horne, identifie plusieurs indicateurs à surveiller, notamment :
- Un nouveau processus qui n'a jamais été vu sur le point de terminaison auparavant avec des demandes de connectivité réseau externe
- Tentative de communication avec des noms DNS où l'entropie est détectée ou directement avec des adresses IP nues
- Modification de la distribution des référentiels Yum ou Apt
- Création de fichiers .sh à l'intérieur de répertoires personnels non basés sur les utilisateurs
- Énumération des arborescences de répertoires Web, de bases de données ou de stockage de fichiers partagés
- Augmentation des privilèges ou tentatives d'accès au sudo
- Communications réseau externes et internes
- Noms de fichiers générés avec entropie ou qui se succèdent rapidement
- Fichiers renommés plusieurs fois dans la même arborescence de répertoires
- Fichiers créés avec des extensions de fichiers impaires
- L'accent est mis sur les valeurs aberrantes et les événements qui ne font pas partie des opérations quotidiennes habituelles
- Utilisation élevée de la mémoire pendant des périodes courtes ou prolongées
- Mesure de processus volumineuse où le processus parent est terminé avant les processus enfants
- Modification des options de démarrage
- Plusieurs copies du même fichier dans plusieurs répertoires
- Plusieurs modifications au sein d'un même répertoire
- Appels possibles de bibliothèques de chiffrement
- Possibilité de création et d'exécution de processus à l'intérieur du répertoire/tmp
- Énumération rapide et successive des répertoires
- Suppression de fichiers des répertoires à l'aide de caractères génériques ou sans confirmation
- L'utilisation de chmod avec des caractères génériques (ou chmod 777)
- L'utilisation de cmds wget ou curl
- Utilisation du cmd chmod pour modifier les fichiers exécutables en droits trop permissifs
- Utilisation des chaînes cmd pour tenter d'encoder les communications avant ou pendant l'infection
- Utiliser VPCFlowLogs pour identifier les accès inappropriés à la base de données à partir d'adresses IP externes
- Vous pouvez [étudier le flux VPC avec Amazon Detective] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) ou [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Répondre
- Inspecter les sauvegardes pour détecter une infection potentielle
- Isoler les systèmes infectés
- Supprimez tous les systèmes compromis du réseau.
- Respectez les exigences réglementaires ou la politique interne de l'entreprise pour déterminer si la police judiciaire de l'instance EC2 est requise
- Si la médico-légale des instances est requise OU que les données doivent être récupérées, [mettez en quarantaine l'instance EC2] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)

### Récupérer
- Il est recommandé de ne pas payer la rançon
- Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
- Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis données/réputation par rapport au paiement à l'attaquant
- Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
- Créer de nouvelles instances EC2 à partir d'une AMI de confiance
- Respectez les exigences réglementaires ou la politique interne de l'entreprise pour déterminer si la police judiciaire de l'instance EC2 est requise
- Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
- Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware
- Visitez < https://www.nomoreransom.org/ > la page pour savoir si un déchiffrement est disponible pour la variante du logiciel malveillant qui a infecté vos données

## Amazon Simple Storage Service (S3)

### Identifier
- Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
- AWS a développé un nouvel outil open source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS compte.
- Envisagez de mettre en œuvre [AWS GuardDuty] (https://aws.amazon.com/guardduty/) pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS, charges de travail et données stockées dans Amazon S3
- Envisagez d'utiliser [AWS Config] (https://aws.amazon.com/config/), un service qui vous permet d'évaluer, d'auditer et d'évaluer les configurations de vos ressources AWS
- Darkbit fournit un exemple de [prévention contre la perte de données AWS S3] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) qui peut être utilisé pour identifier les copies non autorisées d'objets. D'autres opérations spécifiques peuvent être ajoutées dans le modèle, en fonction de vos cas d'utilisation commerciaux.
- Maintenir un inventaire complet de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages réseau/fichiers et les machines de développement

### Protéger
- Envisagez [activer la réplication] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) - même région ou interrégion. La réplication entre régions protège les données contre les défauts d'application et les erreurs d'opérateur en conservant une deuxième copie dans une autre région
- Envisagez d'utiliser AWS Config pour créer des règles permettant de surveiller les modifications apportées aux services AWS. AWS Config dispose de plusieurs règles gérées préconçues pour surveiller EBS et EC2, notamment :
- [s3-account-level-public-access-blocks] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks.html)
- [s3-account-level-public-access-blocks-periodic] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks-periodic.html)
- [s3-bucket-blacklisted-actions-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-blacklisted-actions-prohibited.html)
- [s3-bucket-default-lock-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-default-lock-enabled.html)
- [s3-bucket-level-public-access-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)
- [S3-Bucket-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-logging-enabled.html)
- [s3-bucket-policy-grantee-check] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-grantee-check.html)
- [s3-bucket-policy-not-more-permissive] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-not-more-permissive.html)
- [s3-bucket-public-read-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-read-prohibited.html)
- [s3-bucket-public-write-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-write-prohibited.html)
- [s3-bucket-replication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-replication-enabled.html)
- [s3-bucket-server-side-encryption-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-server-side-encryption-enabled.html)
- [s3-bucket-ssl-requests-only] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-ssl-requests-only.html)
- [s3-bucket-versioning-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-versioning-enabled.html)
- [s3-default-encryption-kms] (https://docs.aws.amazon.com/config/latest/developerguide/s3-default-encryption-kms.html)
- Envisagez d'utiliser des clés [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) pour chiffrer tous les objets et empêcher un attaquant d'appliquer sa clé de chiffrement
- Envisagez d'utiliser [AWS S3 Block Public Access Feature] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) pour atténuer l'exposition involontaire des objets
- Envisagez d'utiliser [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) pour les sauvegardes d'objets et l'optimisation des coûts
- Envisagez d'utiliser [S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) afin de pouvoir stocker des objets à l'aide d'un modèle *write-once-read-many* (WORM). Object Lock peut aider à empêcher la suppression ou l'écrasement d'objets pendant une durée fixe ou indéfiniment.
- En mode de conformité de verrouillage d'objet**, ** une version d'objet protégé ne peut être remplacée ni supprimée par aucun utilisateur, y compris l'utilisateur racine de votre compte AWS. Lorsqu'un objet est verrouillé en mode de conformité, son mode de rétention ne peut pas être modifié et sa période de rétention ne peut pas être raccourcie. Le mode de conformité***garantit qu'une version d'objet ne peut pas être écrasée ou supprimée pendant la durée de la période de rétention.
- Activer [journalisation des événements CloudTrail pour les compartiments et objets S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) contenant des informations sensibles ou critiques. Par défaut, les pistes CloudTrail ne consignent pas les événements de données, mais vous pouvez configurer des pistes pour consigner les événements de données pour les compartiments S3 que vous spécifiez ou pour consigner les événements de données pour tous les compartiments Amazon S3 de votre compte AWS.
- Activez [la journalisation au niveau du serveur CloudTrail pour les compartiments S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) et les objets contenant des informations sensibles ou critiques. La journalisation de l'accès au serveur fournit des enregistrements détaillés pour les demandes envoyées à un compartiment.
- Activer [S3 Object Versioning] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) pour permettre la restauration d'objets modifiés
- Pour définir le nombre de versions conservées, [définir une stratégie de cycle de vie] (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) à appliquer aux versions non actuelles
- Activer [S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) pour empêcher un attaquant de désactiver le versionnement et de remplacer tous les objets d'un compartiment
- Appliquer l'authentification multifacteur (MFA)
- Appliquer les exigences de complexité des mots de passe et établir des périodes d'expiration
- Mettre en œuvre [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html), y compris l'expiration des comptes et les rotations d'informations d'identification obligatoires
- Exécutez un [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) pour répertorier tous les utilisateurs de votre compte et l'état de leurs différentes informations d'identification, y compris les mots de passe, les clés d'accès et les appareils MFA
- Prendre des mesures pour [empêcher toute nouvelle information d'identification d'être exposée publiquement] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
- Utilisez [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) pour identifier les ressources de votre organisation et de vos comptes, telles que les compartiments Amazon S3 ou les rôles IAM partagés avec une entité externe. Cela vous permet d'identifier l'accès involontaire à vos ressources et données, ce qui constitue un risque de sécurité.
- Utiliser les rôles IAM pour gérer les autorisations
- Implémentez le moindre privilège et n'autorisez pas les autorisations s3 : \ *
- Utiliser et auditer systématiquement les stratégies de compartiment
- Ne rendre public que ce qui doit être et veiller à ce que tous les objets soient protégés en étant privés
- Bien que nous ne puissions pas recommander des solutions tierces spécifiques, certains produits de nos partenaires, y compris Veritas et CommVault, et d'autres fournisseurs de logiciels de sauvegarde ont la capacité native de sauvegarder des objets dans S3 vers leurs emplacements de stockage de sauvegarde gérés à l'intérieur ou à l'extérieur de S3

### Détecter

- Vérifiez que votre journal CloudTrail n'y a pas d'activités non autorisées telles que la création d'utilisateurs IAM non autorisés, de stratégies, de rôles ou d'informations d'identification de sécurité temporaires
- Vérifiez votre journal CloudTrail pour vérifier que votre compte AWS n'est pas utilisé non autorisé, comme les instances EC2 non autorisées, les fonctions Lambda ou les enchères Spot EC2. Vous pouvez également vérifier l'utilisation en vous connectant à AWS Management Console et en consultant chaque page de service. La page \ « Bills \ » de la console de facturation peut également être vérifiée pour détecter une utilisation inattendue
- N'oubliez pas qu'une utilisation non autorisée peut se produire dans n'importe quelle région et que votre console peut ne vous afficher qu'une seule région à la fois. Pour basculer entre les régions, vous pouvez utiliser la liste déroulante dans le coin supérieur droit de l'écran de la console.
- Note de rançon fournie soit en tant qu'objet dans le compartiment, soit par e-mail au client
- Les objets S3 sont supprimés ou des compartiments S3 entiers sont supprimés
- Remarque : Dans le cas des événements de destruction de données, une note de rançon peut ou non être fournie. Assurez-vous également de vérifier les mesures CloudWatch et les événements CloudTrail S3 pour vérifier si l'exfiltration des données a eu lieu ou non pour délimiter entre une rançon ou une attaque de destruction de données.
- Les objets S3 sont chiffrés à l'aide d'une clé provenant d'un compte non appartenant au client

### Répondre
- Supprimer ou faire pivoter [Clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [clés utilisateur Root] (https://console.aws.amazon.com/iam/home#security_credential) ; vous pouvez faire pivoter toutes les clés de votre compte si vous ne pouvez pas identifier une ou plusieurs clés spécifiques qui ont été exposées
- Supprimer les [utilisateurs IAM] non autorisés (https://console.aws.amazon.com/iam/home#users.)
- Supprimer les [politiques] non autorisées (https://console.aws.amazon.com/iam/home#/policies)
- Supprimer les [rôles] non autorisés (https://console.aws.amazon.com/iam/home#/roles)
- Si votre application utilise des clés d'accès exposées, vous devez remplacer la clé. Pour remplacer la clé, créez d'abord une deuxième clé (à ce moment-là, les deux clés seront actives), puis modifiez votre application pour utiliser la nouvelle clé. Désactivez ensuite (mais ne supprimez pas) les clés exposées en cliquant sur l'option « Rendre inactif » dans la console. En cas de problème avec votre application, vous pouvez réactiver les clés exposées. Lorsque votre application est entièrement fonctionnelle à l'aide de la nouvelle clé, supprimez les clés exposées
- [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM. *REMARQUE :* La suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution

### Récupérer
- Il est recommandé de ne pas payer la rançon
- Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
- Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis données/réputation par rapport au paiement à l'attaquant
- Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
- Mettre en œuvre des procédures sous Protect avant de tenter de restaurer des données ou des objets
- Supprimer [supprimer les marqueurs] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) pour les objets versionnés
- Recréer des compartiments supprimés
- Restaurer les objets à partir de sauvegardes d'objets [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) ou d'un compartiment de région répliqué
- *Remarque :* Il n'existe actuellement aucune fonctionnalité \ "annuler la suppression \ » pour S3, et AWS n'a pas la possibilité de récupérer les données supprimées. À l'ère actuelle de conformité au stockage de données et de réglementations telles que [GDPR] (https://gdpr-info.eu/) et [CCPA] (https://oag.ca.gov/privacy/ccpa), Amazon S3 ne peut pas continuer à stocker les données client explicitement supprimées du compte du client. Une fois qu'un objet est supprimé, il ne peut plus être récupéré par AWS, quelle que soit la rapidité avec laquelle la suppression non intentionnelle est signalée à AWS

## Service de base de données relationnelle (RDS)

### Identifier
- Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
- AWS a développé un nouvel outil open source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS compte.
- Envisagez d'utiliser [AWS Config] (https://aws.amazon.com/config/), un service qui vous permet d'évaluer, d'auditer et d'évaluer les configurations de vos ressources AWS
- Envisagez de mettre en œuvre [AWS GuardDuty] (https://aws.amazon.com/guardduty/) pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS, charges de travail et données stockées dans Amazon RDS
- Maintenir un inventaire complet de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages réseau/fichiers et les machines de développement

### Protéger
- Des références et des étapes supplémentaires sont disponibles dans [Security in Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)
- Envisagez d'utiliser AWS Config pour créer des règles permettant de surveiller les modifications apportées aux services AWS. AWS Config dispose de plusieurs règles gérées préconçues pour surveiller RDS, notamment :
- [rds-automatic-minor-version-upgrade-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-automatic-minor-version-upgrade-enabled.html)
- [rds-cluster-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-deletion-protection-enabled.html)
- [rds-cluster-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-iam-authentication-enabled.html)
- [rds-cluster-multi-az-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-multi-az-enabled.html)
- [rds-enhanced-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-enhanced-monitoring-enabled.html)
- [rds-instance-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-deletion-protection-enabled.html)
- [rds-instance-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-iam-authentication-enabled.html)
- [rds-instance-public-access-check] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html)
- [rds-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/rds-in-backup-plan.html)
- [rds-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html)
- [rds-multi-az-support] (https://docs.aws.amazon.com/config/latest/developerguide/rds-multi-az-support.html)
- [rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)
- [rds-snapshot-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshot-encrypted.html)
- [rds-storage-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-storage-encrypted.html)
- Disposer d'une solution HIDS (système de détection d'intrusion) basée sur l'hôte tiers (OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/))
- Exécutez votre instance DB dans un cloud privé virtuel (VPC) basé sur le service Amazon VPC pour obtenir le meilleur contrôle d'accès réseau possible. Pour plus d'informations sur la création d'une instance DB dans un VPC, consultez [Amazon Virtual Private Cloud VPC and Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html).
- Utilisez les stratégies AWS Identity and Access Management (IAM) pour attribuer des autorisations qui déterminent qui peut gérer les ressources Amazon RDS. Par exemple, vous pouvez utiliser IAM pour déterminer qui peut créer, décrire, modifier et supprimer des instances DB, baliser des ressources ou modifier des groupes de sécurité.
- Utilisez le chiffrement Amazon RDS pour sécuriser vos instances DB et vos instantanés au repos. Le chiffrement Amazon RDS utilise l'algorithme de chiffrement AES-256 standard du secteur pour chiffrer vos données sur le serveur qui héberge votre instance DB. Pour plus d'informations, voir [Encrypting Amazon RDS resources] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html)
- Utilisez des connexions Secure Socket Layer (SSL) ou Transport Layer Security (TLS) avec des instances DB exécutant les moteurs de base de données MySQL, MariaDB, PostgreSQL, Oracle ou Microsoft SQL Server. Pour plus d'informations sur l'utilisation de SSL/TLS avec une instance DB, consultez [Utilisation de SSL/TLS pour chiffrer une connexion à une instance DB] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
- Utilisez des groupes de sécurité pour contrôler quelles adresses IP ou instances Amazon EC2 peuvent se connecter à vos bases de données sur une instance DB. Lorsque vous créez une instance DB pour la première fois, son système de sécurité empêche tout accès à la base de données, à l'exception des règles spécifiées par un groupe de sécurité associé.
- Utilisez le chiffrement réseau et le chiffrement transparent des données avec les instances de bases de données Oracle. Pour plus d'informations, consultez [chiffrement réseau natif Oracle] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) et [Oracle Transparent Data Encryption] ( https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html)
- Utilisez les fonctionnalités de sécurité de votre moteur de base de données pour contrôler qui peut se connecter aux bases de données sur une instance DB. Ces fonctionnalités fonctionnent comme si la base de données se trouvait sur votre réseau local.

### Détecter
- L'apprentissage automatique AWS GuardDuty peut détecter les comportements suspects, y compris la génération d'instantanés Amazon Relational Database Service (Amazon RDS) dans le cadre des tentatives d'exfiltration de données. Un exemple est fourni dans le blog de sécurité AWS [Comment utiliser Amazon GuardDuty pour détecter les activités suspectes au sein de votre compte AWS] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/)
- Consultez les journaux du système d'exploitation et des applications EC2 pour détecter les connexions inappropriées, l'installation de logiciels inconnus ou la présence de fichiers non reconnus
- Utiliser VPCFlowLogs pour identifier les accès inappropriés à la base de données à partir d'adresses IP externes
- Vous pouvez [étudier le flux VPC avec Amazon Detective] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) ou [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
- Le [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) permet aux clients d'effectuer des enquêtes de sécurité sur les journaux de services AWS en fournissant un environnement d'analyse Amazon Athena rapide à déployer, prêt à l'emploi et facile à entretenir

### Répondre
- Supprimer ou faire pivoter [Clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [clés utilisateur Root] (https://console.aws.amazon.com/iam/home#security_credential) ; vous pouvez faire pivoter toutes les clés de votre compte si vous ne pouvez pas identifier une ou plusieurs clés spécifiques qui ont été exposées
- Supprimer les [utilisateurs IAM] non autorisés (https://console.aws.amazon.com/iam/home#users.)
- Supprimer les [politiques] non autorisées (https://console.aws.amazon.com/iam/home#/policies)
- Supprimer les [rôles] non autorisés (https://console.aws.amazon.com/iam/home#/roles)
- Supprimer les instantanés publics ou les bases de données non reconnus ou non autorisés
- Identifier les instances EC2 ayant un accès permissif à la ou aux bases de données et enquêter sur celles-ci également
- Si votre application utilise des clés d'accès exposées, vous devez remplacer la clé. Pour remplacer la clé, créez d'abord une deuxième clé (à ce moment-là, les deux clés seront actives), puis modifiez votre application pour utiliser la nouvelle clé. Désactivez ensuite (mais ne supprimez pas) les clés exposées en cliquant sur l'option « Rendre inactif » dans la console. En cas de problème avec votre application, vous pouvez réactiver les clés exposées. Lorsque votre application est entièrement fonctionnelle à l'aide de la nouvelle clé, supprimez les clés exposées
- [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM. *REMARQUE :* La suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution

### Récupérer
- Il est recommandé de ne pas payer la rançon
- Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
- Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis données/réputation par rapport au paiement à l'attaquant
- Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
- Supprimez la base de données compromise et créez de nouvelles bases de données RDS
- Respectez les exigences réglementaires ou la politique interne de l'entreprise pour déterminer si la médico-légale de la base de données RDS est requise
- Visitez < https://www.nomoreransom.org/ > la page pour savoir si un déchiffrement est disponible pour la variante du logiciel malveillant qui a infecté vos données
- Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
- Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware

### Suite à un incident

### Signaler des attaques par rançon aux autorités
Le FBI encourage les organisations à signaler les incidents de rançon aux forces de l'ordre. L'Internet Crime Complaint Center (IC3) accepte les plaintes relatives à la criminalité en ligne provenant de la victime ou d'un tiers au plaignant et travaillera avec eux pour déterminer le meilleur plan d'action à l'avenir. Soyez prêt à partager :
- Toute information pertinente que vous estimez nécessaire à l'appui de votre plainte
- En-tête (s) d'e-mail
- Informations sur les transactions financières (informations de compte, date et montant de la transaction, détails du destinataire)
- Nom, adresse, téléphone, e-mail, site Web et adresse IP du sujet
- Des détails spécifiques sur la façon dont vous avez été victime
- Nom, adresse, téléphone et e-mail de la victime

### Effectuer une analyse des causes profondes
Dans la plupart des cas, vos outils médico-légaux existants fonctionnent dans l'environnement AWS. Les équipes judiciaires bénéficient du déploiement automatisé d'outils dans les régions AWS et de la possibilité de collecter rapidement de grands volumes de données avec un faible frottement à l'aide des mêmes services robustes et évolutifs sur lesquels reposent leurs applications stratégiques.

[Amazon Detective] (https://aws.amazon.com/detective/%20) facilite l'analyse, l'investigation et l'identification rapide des causes profondes des problèmes de sécurité potentiels ou des activités suspectes. Amazon Detective collecte automatiquement les données du journal des services AWS à partir de vos ressources AWS et utilise l'apprentissage automatique, l'analyse statistique et la théorie des graphiques pour créer un ensemble de données liées qui vous permet de mener des enquêtes de sécurité plus rapides et plus efficaces.

### Mettre à jour le programme de sécurité avec les leçons apprises
La documentation et le cycle des leçons apprises lors de simulations et d'incidents en direct dans de nouveaux processus et procédures « normaux » permettent aux entreprises de mieux comprendre comment elles ont été violées — par exemple où elles étaient vulnérables, où l'automatisation a pu échouer ou où la visibilité manquait — et le possibilité de renforcer leur posture générale en matière de sécurité.

## Former vos employés
Formez vos employés grâce à un programme efficace de sensibilisation à la sécurité. Cela doit être divertissant et engageant. Se concentrer sur le signalement du hameçonnage, les procédures de notification et la façon de signaler des « mauvaises » choses, puis attribuer ce comportement peut s'avérer très efficace.

## Conclusion
Les attaques de rançon évoluent, mais votre sensibilisation à la sécurité et votre préparation peuvent également évoluer. Les agences gouvernementales, les organisations à but non lucratif et les entreprises du monde entier font confiance à AWS pour alimenter leur infrastructure et assurer la sécurité de leurs systèmes et données. En utilisant les services AWS et les meilleures pratiques partagés dans ce playbook, vous pouvez prendre des mesures proactives pour réduire la probabilité et l'impact de la rançon dans vos environnements AWS.

## Annexe A : Services et outils pour prévenir ou détecter les attaques de rançon
[Amazon CloudWatch Synthetics] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)
[Amazon CloudWatch] (https://aws.amazon.com/cloudwatch)
[Amazon CodeGuru] (https://aws.amazon.com/codeguru/)
[Amazon EventBridge] (https://aws.amazon.com/eventbridge/)
[Amazon GuardDuty] (https://aws.amazon.com/guardduty/)
[Accessibilité réseau Amazon Inspector] (https://docs.aws.amazon.com/inspector/latest/userguide/inspector_network-reachability.html)
[Amazon Inspector] (https://aws.amazon.com/inspector/)
[Amazon Macie] (https://aws.amazon.com/macie/)
[Analyseur d'accessibilité Amazon VPC] (https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)
[CDK AWS] (https://aws.amazon.com/cdk/)
[AWS Cloud9] (https://aws.amazon.com/cloud9/)
[AWS CloudFormation Guard] (https://github.com/aws-cloudformation/cloudformation-guard)
[AWS CloudFormation] (https://aws.amazon.com/cloudformation/)
[AWS CodeArtifact] (https://aws.amazon.com/codeartifact/)
[AWS CodeBuild] (https://aws.amazon.com/codebuild/)
[AWS CodeCommit] (https://aws.amazon.com/codecommit/)
[AWS CodeDeploy] (https://aws.amazon.com/codedeploy/)
[AWS CodePipeline] (https://aws.amazon.com/codepipeline/)
[Règles AWS Config] (https://aws.amazon.com/blogs/aws/aws-config-rules-dynamic-compliance-checking-for-cloud-resources/)
[Tour de contrôle AWS] (https://aws.amazon.com/controltower/)
[AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)
[AWS IAM] (https://aws.amazon.com/iam/)
[AWS KMS] (https://aws.amazon.com/kms/)
[AWS Lambda] (https://aws.amazon.com/lambda/)
[Organisations AWS] (https://aws.amazon.com/organizations/)
[AWS Secrets Manager] (https://aws.amazon.com/secrets-manager/)
[AWS Security Hub] (https://aws.amazon.com/security-hub/)
[AWS Step Functions] (https://aws.amazon.com/step-functions/)
[AWS Systems Manager] (https://aws.amazon.com/systems-manager/)
[AWS WAF] (https://aws.amazon.com/waf/)
[AWSPX] (https://github.com/FSecureLABS/awspx)
[CheckMarx] (https://checkmarx.com/)
[EC2 Image Builder] (https://aws.amazon.com/image-builder/)
[Numérisation d'image de conteneur natif ECR] (https://aws.amazon.com/blogs/containers/amazon-ecr-native-container-image-scanning/)
[git-secrets] (https://github.com/awslabs/git-secrets)
[PMapper] (https://github.com/nccgroup/PMapper)
[Prowler] (https://github.com/toniblyx/prowler)
[SonarQube] (https://www.sonarqube.org/)