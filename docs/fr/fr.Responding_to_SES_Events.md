# Playbook de réponse aux incidents : réponse à des événements simples de service de messagerie

# Points de contact

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
[Perspective de sécurité AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directive**
* **Détective**
* **Responsible**
* **Préventif**

### Étapes de réponse
1. [**PREPARATION**] Utiliser les détections AWS GuardDuty pour IAM
2. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
3. [**DÉTECTION ET ANALYSIS**] Effectuer la détection et l'analyse de CloudTrail pour les événements d'API non reconnus
4. [**DÉTECTION ET ANALYSIS**] Effectuer la détection et l'analyse de CloudWatch pour détecter les événements non reconnus
3. [**CONFINEMENT ET ÉRADICATION**] Supprimer ou faire pivoter les clés utilisateur IAM
4. [**CONFINEMENT ET ÉRADICATION**] Supprimer ou faire pivoter des ressources non reconnues
5. [**RECOVERY**] Exécutez les procédures de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** :
* **Catégorie** :
* **Ressource** : SES
* **Indicateurs** :
* **Sources du journal :
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

## Résumé exécutif
Ce manuel décrit le processus de réponse aux attaques contre AWS Simple Email Service (SES). En combinaison avec ce guide, veuillez consulter la [FAQ sur le processus d'envoi d'Amazon SES] (https://docs.aws.amazon.com/ses/latest/DeveloperGuide/faqs-enforcement.html) pour obtenir des réponses aux mesures d'application et des réponses à une utilisation défavorable de SES.

Pour plus d'informations, veuillez consulter le [Guide de réponse aux incidents de sécurité AWS] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Préparation - Général
* Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
* AWS a développé un nouvel outil open source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur la posture de sécurité de leur AWS compte.
* Maintenir un inventaire complet de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages réseau/fichiers et les machines de développement
* Envisagez de mettre en œuvre [AWS GuardDuty] (https://aws.amazon.com/guardduty/) pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS, charges de travail et données stockées dans Amazon SES
* Implémenter [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html), y compris l'expiration des comptes et les rotations d'informations d'identification obligatoires
* **Appliquer l'authentification multifacteur (MFA) **
* Appliquer les exigences de complexité des mots de passe et établir des périodes d'expiration
* Exécutez un [rapport d'identification IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) pour répertorier tous les utilisateurs de votre compte et l'état de leurs différentes informations d'identification, y compris les mots de passe, les clés d'accès et les appareils MFA
* Utilisez [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) pour identifier les ressources de votre organisation et de vos comptes, tels que les rôles IAM partagés avec une entité externe. Cela vous permet d'identifier un accès non intentionnel à vos ressources et données, ce qui constitue un risque de sécurité.

## Préparation - Spécifique aux SES
* Utilisez les stratégies d'autorisation d'envoi pour contrôler qui peut envoyer des e-mails et d'où
* https://docs.aws.amazon.com/ses/latest/dg/sending-authorization.html
* Utiliser des jeux de configuration pour contrôler les adresses IP et les identités autorisées à envoyer des messages
* https://docs.aws.amazon.com/ses/latest/dg/using-configuration-sets.html
* Envisagez d'utiliser des adresses IP dédiées pour Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html
* Gérez vos propres listes pour le publipostage et les abonnements, ainsi que pour la suppression des e-mails dans Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/lists-and-subscriptions.html
* Configurer la publication d'événements Amazon SES pour les notifications en temps réel
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-using-event-publishing-setup.html
* Utiliser la gestion des identités et des accès les moins privilégiés dans Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html
* Examiner les meilleures pratiques SES, en mettant l'accent sur la sécurité et l'accès
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html (Classique)
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/control-user-access.html (Classique)
* Configurez SPF, DKIM et DMARC pour votre propre domaine afin de prévenir le hameçonnage et l'usurpation d'identité
* https://docs.aws.amazon.com/ses/latest/dg/email-authentication-methods.html

### Détections potentielles d'AWS GuardDuty
Les résultats suivants sont spécifiques aux entités IAM et aux clés d'accès et possèdent toujours un type de ressource AccessKey. La gravité et les détails des résultats diffèrent selon le type de recherche. Pour plus d'informations sur chaque type de recherche, veuillez consulter la page Web [types de recherche IAM GuardDuty] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html).

* Accès aux informations d'identification : Iutilisateur/comportement anormal
* Évasion de la défense : j'amuser/comportement anormal
* Découverte : Iamuser/comportement anormal
* Exfiltration : Iamuser/comportement anormal
* Impact : Iutilisateur/comportement anormal
* Accès initial : Iutilisateur/comportement anormal
* Pentest : IAMUSER/KaliLinux
* Pentest : Iamuser/ParrotLinux
* Pentest : je utilisateur/stylo Linux
* Persistance : j'utilisateur/comportement anormal
* Politique : utilisation des informations d'identification de l'utilisateur et de la racine
* Augmentation des privilèges : Iutilisateur/comportement anormal
* Recon:IAMUSER/Malicious IPCaller
* Recon:IAMUSER/IP Caller malveillant. Personnalisé
* Recon:IAMUSER/TPIP Caller
* Furtif : J'utilisateur/Cloud Traillogging désactivé
* furtivité : je modifie la politique d'utilisateur/mot de passe
* Accès non autorisé : j'utilisateur/consoleLoginSuccess.b
* Accès non autorisé : filtrage des informations d'identification d'utilisateur/instance. En dehors d'AWS
* Accès non autorisé : Iutilisateur/interlocuteur IP malveillant
* Accès non autorisé : Iutilisateur/appel IP malveillant. Personnalisé
* Accès non autorisé : Iutilisateur/Torip Caller

## Procédures d'escalade
- « J'ai besoin d'une décision commerciale quant au moment où la criminalistique EC2 devrait être effectuée »
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Détection et analyse
### CloudTrail
Amazon SES est intégré à AWS CloudTrail, un service qui fournit un enregistrement des actions entreprises par un utilisateur, un rôle ou un service AWS dans Amazon SES. CloudTrail capture les appels API pour Amazon SES en tant qu'événements. Les appels capturés incluent des appels depuis la console Amazon SES et des appels de code vers les opérations de l'API Amazon SES.

Voici des événements CloudTrail `EventName` spécifiques pour rechercher les modifications apportées à votre compte en rapport avec SES :

* Supprimer l'identité
* Stratégie de suppression d'identité
* Supprimer le filtre de réception
* Supprimer la règle de réception
* Supprimer le jeu de règles de réception
* Supprimer l'adresse e-mail vérifiée
* Obtient le quota d'envoi
* Politique d'identité Put
* Mettre à jour la règle de réception
* Vérifiez le domaine DKim
* Vérifier l'identité du domaine
* Vérifier l'adresse e-mail
* Vérifier l'identité e-mail

[Affichage d'événements avec l'historique des événements CloudTrail] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)

### Autres contrôles de détective
* Enregistrez et surveillez les événements SES (par exemple, envoi, rebonds, plaintes, etc.)
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Surveiller les mesures de réputation des expéditeurs
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Configurer les alarmes ou le tableau de bord Cloudwatch appropriés pour les mesures SES
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Confinement et éradication
* [Supprimer ou faire pivoter les clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [clés utilisateur racine] (https://console.aws.amazon.com/iam/home#security_credential) ; vous pouvez faire pivoter toutes les clés de votre compte si vous ne pouvez pas identifier une ou plusieurs clés spécifiques qui ont été exposées
* [Supprimer les utilisateurs IAM non autorisés] (https://console.aws.amazon.com/iam/home#users.)
* [Supprimer les stratégies non autorisées] (https://console.aws.amazon.com/iam/home#/policies)
* [Supprimer les rôles non autorisés] (https://console.aws.amazon.com/iam/home#/roles)
* [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM.
* REMARQUE : La suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution

## Récupération
* Créer de nouveaux utilisateurs IAM avec des stratégies d'accès aux moindres privilèges
* Mettre en œuvre les étapes et les ressources trouvées dans la section « Préparation - Spécifique SES »
* Enregistrez et surveillez les événements SES (par exemple, envoi, rebonds, plaintes, etc.)
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Surveiller les mesures de réputation des expéditeurs
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Configurer les alarmes ou le tableau de bord Cloudwatch appropriés pour les mesures SES
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livret de jeu en même temps que les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés

## Articles de carnet de commandes actuels