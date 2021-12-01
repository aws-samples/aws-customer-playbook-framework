# Playbook de réponse aux incidents : réponse à la rançon pour S3
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

## Points de contact

Auteur : `Nom de l'auteur
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

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
1. [**PREPARATION**] Utilisez AWS Config pour vérifier la conformité de la configuration
2. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
3. [**DÉTECTION ET ANALYSIS**] Effectuer la détection et l'analyse de CloudTrail pour détecter une API non reconnue
4. [**RECOVERY**] Exécutez les procédures de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Rançon et destruction des données
* **Catégorie** : Attaque de rançon
* **Ressource** : S3
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers, mesures Cloudwatch
* **Sources de journal** : journaux du serveur S3, journaux d'accès S3, CloudTrail, CloudWatch, AWS Config
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

## Résumé exécutif
Ce manuel décrit le processus de réponse aux attaques de rançon contre AWS Simple Storage Service (S3).

Pour plus d'informations, veuillez consulter le [Guide de réponse aux incidents de sécurité AWS] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Préparation
* Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
* AWS a développé un nouvel outil open source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS compte.
* Maintenir un inventaire complet de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages réseau/fichiers et les machines de développement
* Envisagez d'utiliser [AWS Config] (https://aws.amazon.com/config/), un service qui vous permet d'évaluer, d'auditer et d'évaluer les configurations de vos ressources AWS
* Envisagez de mettre en œuvre [AWS GuardDuty] (https://aws.amazon.com/guardduty/) pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS, charges de travail et données stockées dans Amazon S3
* Darkbit fournit un exemple de [prévention contre la perte de données AWS S3] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) qui peut être utilisé pour identifier les copies non autorisées d'objets. D'autres opérations spécifiques peuvent être ajoutées dans le modèle, en fonction de vos cas d'utilisation commerciaux
* Utiliser les rôles IAM pour gérer les autorisations
* Implémenter le moindre privilège et ne pas autoriser les autorisations s3 :*
* Implémenter [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html), y compris l'expiration des comptes et les rotations d'informations d'identification obligatoires
* Appliquer l'authentification multifacteur (MFA)
* Appliquer les exigences de complexité des mots de passe et établir des périodes d'expiration
* Exécutez un [rapport d'identification IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) pour répertorier tous les utilisateurs de votre compte et l'état de leurs différentes informations d'identification, y compris les mots de passe, les clés d'accès et les appareils MFA
* Utilisez [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) pour identifier les ressources de votre organisation et de vos comptes, telles que les compartiments Amazon S3 ou les rôles IAM partagés avec une entité externe. Cela vous permet d'identifier un accès non intentionnel à vos ressources et données, ce qui constitue un risque de sécurité.
* [Activer le versionnement des objets S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) pour permettre la restauration d'objets modifiés
* Pour définir le nombre de versions conservées, définissez une stratégie de cycle de vie (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) à appliquer aux versions non actuelles
* [Activer la suppression MFA S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) pour empêcher un attaquant de désactiver le versionnement et d'écraser tous les objets d'un compartiment
* Envisagez d'utiliser [S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) afin de pouvoir stocker des objets à l'aide d'un modèle WORM (Write-Once-Read-Many). Object Lock peut aider à empêcher la suppression ou l'écrasement d'objets pendant une durée fixe ou indéfiniment.
* En mode de conformité de verrouillage d'objet*, * une version d'objet protégé ne peut être écrasée ou supprimée par aucun utilisateur, y compris l'utilisateur racine de votre compte AWS. Lorsqu'un objet est verrouillé en mode de conformité, son mode de rétention ne peut pas être modifié et sa période de rétention ne peut pas être raccourcie. Le mode de conformité *garantit qu'une version d'objet ne peut pas être écrasée ou supprimée pendant la durée de la période de rétention.
* Envisagez d'utiliser [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) pour les sauvegardes d'objets et l'optimisation des coûts
* Utiliser et auditer régulièrement les stratégies de compartiment
* Ne rendre public que ce qui doit être et veiller à ce que tous les objets soient protégés en étant privés
* Envisagez d'utiliser des clés [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) pour chiffrer tous les objets et empêcher un attaquant d'appliquer sa clé de chiffrement
* Envisagez d'utiliser la [fonctionnalité AWS S3 Block Public Access] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) pour atténuer l'exposition involontaire des objets
* Activer [journalisation des événements CloudTrail pour les compartiments et les objets S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) contenant des informations sensibles ou critiques. Par défaut, les pistes CloudTrail ne consignent pas les événements de données, mais vous pouvez configurer des pistes pour consigner les événements de données pour les compartiments S3 que vous spécifiez ou pour consigner les événements de données pour tous les compartiments Amazon S3 de votre compte AWS.
* Activer [journalisation au niveau du serveur CloudTrail pour les compartiments S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) et les objets contenant des informations sensibles ou critiques. La journalisation de l'accès au serveur fournit des enregistrements détaillés pour les demandes envoyées à un compartiment.
* Pour les téléchargements de données critiques, envisagez [activer la réplication] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) - même région ou interrégion. La réplication entre régions protège les données contre les défauts d'application et les erreurs d'opérateur en conservant une deuxième copie dans une autre région
* Prenez des mesures pour [empêcher toute nouvelle information d'identification d'être exposée publiquement] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
* Bien que nous ne puissions pas recommander des solutions tierces spécifiques, certains produits de nos partenaires, y compris Veritas et CommVault, et d'autres fournisseurs de logiciels de sauvegarde ont la capacité native de sauvegarder des objets dans S3 vers leurs emplacements de stockage de sauvegarde gérés à l'intérieur ou à l'extérieur de S3

### Utilisez AWS Config pour afficher la conformité de la configuration :
1. Connectez-vous à AWS Management Console et ouvrez la console AWS Config à l'adresse https://console.aws.amazon.com/config/
1. Dans le menu AWS Management Console, vérifiez que le sélecteur de région est défini sur une région prenant en charge les règles AWS Config. Pour obtenir la liste des régions prises en charge, consultez AWS Config Regions and Endpoints dans la référence générale Amazon Web Services
1. Dans le volet de navigation, choisissez Ressources. Sur la page Inventaire des ressources, vous pouvez filtrer par catégorie de ressources, par type de ressource et par statut de conformité. Choisissez Inclure les ressources supprimées le cas échéant. Le tableau affiche l'identificateur de ressource pour le type de ressource et l'état de conformité des ressources pour cette ressource. L'identificateur de ressource peut être un ID de ressource ou un nom de ressource.
1. Choisissez une ressource dans la colonne Identificateur de ressource
1. Cliquez sur le bouton Chronologie des ressources. Vous pouvez filtrer par événements de configuration, événements de conformité ou événements CloudTrail
1. Concentrez-vous spécifiquement sur les événements suivants :
* blocs d'accès public au niveau du compte s3
* Blocs d'accès public au niveau du compte-s3-périodique
* S3-seau - Noirlistés-actions interdites
* S3-Bucket-Default-Lock activé
* Accès public au niveau du seau s3 interdits
* Activé pour la journalisation des compartiments s3
* chèque de subvention s3-godet-policy-grantee-check
* s3-godet politique-pas plus permissif
* S3-seau public-lecture-interdite
* S3-seau public-écriture interdite
* compatible avec la réplication de compartiments s3
* Activé pour le chiffrement côté serveur s3 compartiments
* s3-bucket-ssl-requests-uniquement
* compatible avec le versionnement du compartiment s3
* Chiffrement par défaut s3-kms

## Procédures d'escalade
- « J'ai besoin d'une décision commerciale quant au moment où la criminalistique EC2 devrait être effectuée »
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Détection et analyse
* Les objets S3 sont supprimés ou des compartiments S3 entiers sont supprimés
* Remarque : Dans le cas des événements de destruction de données, une note de rançon peut être fournie ou non. Assurez-vous également de vérifier les mesures CloudWatch et les événements CloudTrail S3 pour vérifier si l'exfiltration des données a eu lieu ou non pour délimiter entre une rançon ou une attaque de destruction de données.
* Les objets S3 sont chiffrés à l'aide d'une clé provenant d'un compte non appartenant au client
* Note de rançon fournie soit en tant qu'objet dans le compartiment, soit par e-mail au client
* Vérifiez que votre journal CloudTrail n'y a pas d'activités non autorisées telles que la création d'utilisateurs IAM non autorisés, de stratégies, de rôles ou d'informations d'identification de sécurité temporaires
* Consultez CloudTrail pour les appels d'API non reconnus. Plus précisément, recherchez les événements suivants :
* Supprimer le compartiment
* Supprimer les cors de compartiments
* Supprimer le chiffrement Bucket
* Supprimer le cycle de vie du compartiment
* Supprimer la stratégie de compartiment
* Supprimer la réplication par compartiment
* Supprimer le balisage des compartiments
* Supprimer le bloc d'accès public au compartiment
* Si les journaux d'accès au serveur S3 sont activés, recherchez le `REST.COPY.OBJECT_GET` élevé et séquentiel à partir de la même adresse IP distante et du même demandeur.
* Vérifiez votre journal CloudTrail pour vérifier que votre compte AWS n'est pas utilisé non autorisé, comme les instances EC2 non autorisées, les fonctions Lambda ou les enchères Spot EC2. Vous pouvez également vérifier l'utilisation en vous connectant à AWS Management Console et en passant en revue chaque page de service. La page « Factures » de la console de facturation peut également être vérifiée pour détecter une utilisation inattendue.
* N'oubliez pas qu'une utilisation non autorisée peut se produire dans n'importe quelle région et que votre console peut ne vous afficher qu'une seule région à la fois. Pour basculer entre les régions, vous pouvez utiliser la liste déroulante située dans le coin supérieur droit de l'écran de la console

## Récupération
* Il est recommandé de ne pas payer la rançon
* Le paiement de la rançon est un pari pour savoir si le criminel honorera la transaction après avoir reçu le paiement
* Si aucune sauvegarde de données n'existe, vous devez effectuer une analyse coûts-avantages et peser la valeur du compromis de données/réputation par rapport au paiement à l'attaquant
* Vous permettez directement à l'attaquant de poursuivre ses opérations contre votre entreprise ou d'autres personnes si vous choisissez de payer la rançon
* Visitez https://www.nomoreransom.org/ pour savoir si un décrypteur est disponible pour la variante du logiciel malveillant qui a infecté vos données
* [Supprimer ou faire pivoter les clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [clés utilisateur racine] (https://console.aws.amazon.com/iam/home#security_credential) ; vous pouvez faire pivoter toutes les clés de votre compte si vous ne pouvez pas identifier une ou plusieurs clés spécifiques qui ont été exposées
* [Supprimer les utilisateurs IAM non autorisés] (https://console.aws.amazon.com/iam/home#users.)
* [Supprimer les stratégies non autorisées] (https://console.aws.amazon.com/iam/home#/policies)
* [Supprimer les rôles non autorisés] (https://console.aws.amazon.com/iam/home#/roles)
* [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM. REMARQUE : La suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution * Utilisez CloudEndure Disaster Recovery pour sélectionner le dernier point de récupération avant l'attaque par ransomware ou la corruption des données pour restaurer vos charges de travail sur AWS
* Si vous utilisez une autre stratégie de sauvegarde des données, validez que les sauvegardes n'ont pas été infectées et restaurez à partir du dernier événement planifié avant l'événement ransomware
* Implémenter des procédures sous Protect avant de tenter de restaurer des données ou des objets
* [Supprimer les marqueurs de suppression] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) pour les objets versionnés
* Recréer des compartiments supprimés
* [Restaurer les objets à partir de l'utilisation de la hiérarchisation intelligente S3] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) sauvegardes d'objets ou compartiment de région répliqué
* Remarque : Il n'existe actuellement aucune fonctionnalité « annuler la suppression » pour S3, et AWS n'est pas en mesure de récupérer les données supprimées. À l'ère actuelle de conformité au stockage de données et de réglementations telles que le RGPR (https://gdpr-info.eu/) et CCPA (https://oag.ca.gov/privacy/ccpa), Amazon S3 ne peut pas continuer à stocker les données client explicitement supprimées du compte du client. Une fois qu'un objet est supprimé, il ne peut plus être récupéré par AWS, quelle que soit la rapidité avec laquelle la suppression non intentionnelle est signalée à AWS

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livret de jeu en même temps que les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant qu'intervenant en cas d'incident, j'ai besoin d'un runbook pour effectuer EC2 Forensics
- En tant qu'intervenant en cas d'incident, j'ai besoin d'une décision commerciale quant au moment où la police judiciaire EC2 devrait être effectuée.
- En tant que répondeur d'incident, je dois activer la journalisation dans toutes les régions qui sont activées indépendamment de l'intention d'utilisation
- En tant que répondeur d'incident, je dois pouvoir détecter le crypto mining sur mes instances EC2 existantes

## Articles de carnet de commandes actuels