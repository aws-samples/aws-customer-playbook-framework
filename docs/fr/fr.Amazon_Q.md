# Manuel de réponse aux incidents : réponse aux événements de sécurité Amazon Q

Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date de publication de ce document, qui sont susceptibles d'être modifiées sans préavis. Les clients sont tenus de procéder à leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « tel quel » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses filiales, fournisseurs ou concédants de licence. Les responsabilités et obligations d'AWS envers ses clients sont régies par les accords AWS, et le présent document ne fait partie d'aucun accord conclu entre AWS et ses clients et ne les modifie pas.

© 2024 Amazon Web Services, Inc. ou ses filiales. Tous droits réservés. Ce travail est mis à disposition selon les termes de la licence internationale Creative Commons Attribution 4.0.

Ce contenu AWS est fourni conformément aux termes du contrat client AWS disponible sur http://aws.amazon.com/agreement ou de tout autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL, ou les deux.

## Points de contact

Auteur : `Nom de l'auteur` \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé
Ce manuel présente des exemples de processus et de requêtes pour étudier les événements de sécurité impliquant Amazon Q.

## Préparation
Ce manuel fait référence à [Prowler] (https://github.com/toniblyx/prowler) et s'intègre, dans la mesure du possible, à un outil de ligne de commande qui vous aide à évaluer la sécurité, à auditer, à renforcer et à répondre aux incidents sur AWS.

### Objectifs
Tout au long de l'exécution du playbook, concentrez-vous sur les _***résultats souhaités***_, en prenant des notes pour améliorer les capacités de réponse aux incidents.

#### Déterminez :
* **Vulnérabilités exploitées**
* **Exploits et outils observés**
* **Intention de l'acteur**
* **Attribution de l'acteur**
* **Dommages infligés à l'environnement et aux entreprises**

#### Restaurer :
* **Revenir à la configuration d'origine et durcie**

#### Améliorez les composants de CAF Security Perspective :
[Perspective de sécurité de l'AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directive**
* **Détective**
* **Réactif**
* **Préventif**

! [Photo] (/images/aws_caf.png)
* * *

### Classification et gestion des incidents
* **Tactiques, techniques et procédures** : Amazon Q
* **Catégorie** : Analyse du journal
* **Ressource** : Amazon Q
* **Indicateurs** : renseignements sur les cybermenaces, avis de tiers
* **Sources de journalisation** : CloudTrail
* **Équipes** : centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comprend les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité après l'incident

### Étapes de réponse
1. [**PRÉPARATION**]
2. [**PRÉPARATION**]
3. [**PRÉPARATION**]
4. [**DÉTECTION ET ANALYSE**] Effectuez la détection et analysez CloudTrail pour détecter les événements d'API non reconnus
5. [**DÉTECTION ET ANALYSE**] Effectuez la détection et analysez CloudWatch pour détecter les événements non reconnus
6. [**CONFINEMENT ET ÉRADICATION**] Supprimer ou faire pivoter les clés utilisateur IAM
7. [**CONFINEMENT ET ÉRADICATION**] Supprimer ou faire pivoter des ressources non reconnues

## Préparation
* Évaluez votre posture de sécurité pour identifier et corriger les failles de sécurité
* AWS a développé un nouvel outil open source Self-Service Security Assessment qui fournit aux clients une évaluation ponctuelle afin d'obtenir des informations précieuses sur le niveau de sécurité de leur compte AWS.
* Maintenir un inventaire complet des actifs de toutes les ressources, y compris les serveurs, les périphériques réseau, les partages de fichiers et les machines de développement
* Envisagez de mettre en œuvre AWS GuardDuty pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS, vos charges de travail et les données stockées sur Amazon SES
* Mettre en œuvre les fondations CIS AWS, y compris l'expiration des comptes et la rotation obligatoire des identifiants
* Appliquer l'authentification multifactorielle (MFA)
* Appliquer les exigences de complexité des mots de passe et établir des délais d'expiration
* Rédigez un IAM Credential Report pour répertorier tous les utilisateurs de votre compte et l'état de leurs différentes informations d'identification, y compris les mots de passe, les clés d'accès et les appareils MFA
* Utilisez AWS IAM Access Analyzer pour identifier les ressources de votre organisation et de vos comptes, telles que les rôles IAM partagés avec une entité externe. Cela vous permet d'identifier les accès involontaires à vos ressources et à vos données, qui constituent un risque pour la sécurité
* Envisagez de mettre en œuvre AWS GuardDuty pour surveiller en permanence les activités malveillantes et les comportements non autorisés afin de protéger vos comptes AWS et vos charges de travail.

## Procédures d'escalade
- `J'ai besoin d'une décision commerciale sur le moment où les analyses de l'EC2 devraient être effectuées'
- `Qui surveille les journaux/alertes, les reçoit et agit en conséquence ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et juridiques interviennent-elles dans le processus ? `
- `Quand contacteriez-vous le support AWS pour obtenir de l'aide ? `

## Modèle Amazon Q

Plusieurs composants/services d'Amazon Q sont compromis :

* Chat — Amazon Q répond à des questions en langage naturel en anglais sur AWS, notamment sur la sélection des services AWS, l'utilisation de l'interface de ligne de commande AWS (AWS CLI), la documentation et les meilleures pratiques. Amazon Q répond par des résumés d'informations ou des instructions détaillées, et inclut des liens vers ses sources d'informations.
* Mémoire — Amazon Q utilise le contexte de votre conversation pour éclairer les réponses futures pendant toute la durée de votre conversation.
* Améliorations du code et conseils — Dans les IDE, Amazon Q peut répondre aux questions concernant le développement de logiciels, améliorer votre code et générer du nouveau code.
* Résolution des problèmes et assistance : Amazon Q peut vous aider à comprendre les erreurs dans l'AWS Management Console et vous donne accès à des agents de support AWS en direct pour répondre à vos questions et problèmes liés à AWS.
* Commentaires des clients : Amazon Q utilise les informations et les commentaires que vous soumettez via les formulaires de commentaires pour fournir une assistance ou résoudre des problèmes techniques.

## Détection et analyse

### Remarques et considérations

* Q n'est pas disponible dans toutes les régions (2024-05-22). Consultez [le guide du développeur] (https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/regions.html) pour connaître la disponibilité actuelle dans la région
* La journalisation des appels par modèle n'est actuellement pas une fonctionnalité de Q. Il est prévu de l'ajouter à l'avenir. Il s'agit d'un paramètre au niveau de la région et il n'est pas défini par défaut
* Points de terminaison :
* Q Business (développeurs) : qbusiness.amazonaws.com
* Chat : q.amazonaws.com

### Noms des événements

Les noms d'événements répertoriés dans le tableau ci-dessous constituent une référence rapide pour les types d'événements généralement rencontrés dans CloudTrail lors de l'investigation d'un événement de sécurité impliquant Q. Parmi ceux répertoriés dans le tableau, les noms d'événements les plus courants enregistrés lors de l'utilisation normale du modèle sont les suivants :

* `Obtenir une conversation`
* `Démarrer la conversation`
* `GetIndex`
* `ListRetrievers`
* `Obtenir des applications`

| **Noms des événements** | **Description** |
| : -----------------| : ----------------------------------------------- |
| ***Autorisations générales*** | --- |
| `GetConversation` | Accorde l'autorisation de recevoir des messages individuels associés à une conversation spécifique avec Amazon Q |
| `GetTroubleshootingResults` | Accorde l'autorisation d'obtenir des résultats de dépannage avec Amazon Q |
| `SendMessage` | Accorde l'autorisation d'envoyer un message à Amazon Q |
| `StartConversation` | Accorde l'autorisation de démarrer une conversation avec Amazon Q |
| `StartTroubleshootingAnalysis` | StartTroubleshootingAnalysisAutorise à démarrer une analyse de dépannage avec Amazon Q
| `StartTroubleshootingResolutionExplanation` | Accorde l'autorisation de démarrer une explication de résolution des problèmes avec Amazon Q|
| --- | --- |
| ***Création d'une application*** | -------- |
| `CreateApplication` | Accorde l'autorisation de créer une application |
| `DeleteApplication` | Accorde l'autorisation de supprimer une application |
| `GetApplication` | Accorde l'autorisation d'obtenir une application |
| `ListApplications` | Accorde l'autorisation de répertorier les applications|
| `UpdateApplication` | Accorde l'autorisation de mettre à jour une application |
| --- | --- |
| ***Création d'un index*** | -------- |
| `CreateIndex` | Accorde l'autorisation de créer un index pour une application donnée |
| `DeleteIndex` | Accorde l'autorisation de supprimer un index |
| `getIndex` | Accorde l'autorisation d'obtenir un index|
| `ListIndices` | Autorise à répertorier les index d'une application |
| `UpdateIndex` | Accorde l'autorisation de mettre à jour un index |
| --- | --- |
| ***Création d'un retriever*** | -------- |
| `CreateRetriever` | Accorde l'autorisation de créer un récupérateur pour une application donnée |
| `DeleteRetriever` | Accorde l'autorisation de supprimer un récupérateur |
| `GetRetriever` | Accorde l'autorisation d'obtenir un retriever |
| `ListRetrievers` | Autorise à répertorier les récupérateurs d'une application |
| `UpdateRetriever` | Autorise la mise à jour d'un Retriever |
| --- | --- |
| ***Connexion de sources de données*** | -------- |
| `CreateDataSource` | Accorde l'autorisation de créer une source de données pour une application et un index donnés |
| `DeleteDataSource` | Accorde l'autorisation de supprimer une source de données |
| `GetDataSource` | Accorde l'autorisation d'obtenir une source de données |
| `ListDataSources` | Accorde l'autorisation de répertorier les sources de données d'une application et d'un index |
| `UpdateDataSource` | Accorde l'autorisation de mettre à jour une source de données |
| `StartDataSourceSyncJobs` | Accorde l'autorisation de démarrer le travail de synchronisation des sources de données |
| `StopDataSourceSyncJobs` | Accorde l'autorisation d'arrêter le travail de synchronisation des sources de données |
| `ListDataSourceSyncJobs` | Accorde l'autorisation d'obtenir l'historique des tâches de synchronisation des sources de données |
| --- | --- |
| ***Téléchargement de documents*** | -------- |
| `BatchPutDocument` | Accorde l'autorisation de mettre le document par lots |
| `BatchDeleteDocument` | Accorde l'autorisation de supprimer un document par lots |
| --- | --- |
| ***Gestion du chat et des conversations*** | -------- |
| `Chat` | Autorise le chat à l'aide d'une application|
| `ChatSync` | Accorde l'autorisation de discuter de manière synchrone à l'aide d'une application |
| `DeleteConversation` | Accorde l'autorisation de supprimer une conversation |
| `ListConversations` | Accorde l'autorisation de répertorier toutes les conversations pour une application |
| `ListMessages` | Accorde l'autorisation de répertorier tous les messages |
| --- | --- |
| ***Gestion des utilisateurs et des groupes*** | -------- |
| `CreateUser` | Accorde l'autorisation de créer un utilisateur |
| `DeleteUser` | Accorde l'autorisation de supprimer un utilisateur |
| `getUser` | Accorde l'autorisation d'obtenir un utilisateur |
| `UpdateUser` | Accorde l'autorisation de mettre à jour un utilisateur |
| `PutGroup` | Accorde l'autorisation de mettre un groupe d'utilisateurs |
| `DeleteGroup` | Accorde l'autorisation de supprimer un groupe |
| `getGroup` | Accorde l'autorisation d'obtenir un groupe |
| `ListGroups` | Accorde l'autorisation de lister les groupes |
| --- | --- |
| ***Plugins*** | -------- |
| `CreatePlugin` | Accorde l'autorisation de créer un plugin pour une application donnée |
| `DeletePlugin` | Accorde l'autorisation de supprimer un plugin |
| `GetPlugin` | Accorde l'autorisation d'obtenir un plugin |
| `UpdatePlugin` | Accorde l'autorisation de mettre à jour un plugin |
| --- | --- |
| ***Contrôles d'administration et garde-corps *** | -------- |
| `UpdateChatControlsConfiguration` | Accorde l'autorisation de mettre à jour la configuration des contrôles de chat pour une application |
| `DeleteChatControlsConfiguration` | Accorde l'autorisation de supprimer la configuration des contrôles de chat pour une application |
| `GetChatControlsConfiguration` | Accorde l'autorisation d'obtenir la configuration des contrôles de chat pour une application |

### Événements du plan de données dans CloudTrail

Par défaut, CloudTrail n'enregistre pas les événements liés aux données. Ce qui suit montre les opérations de l'API Amazon Q enregistrées dans CloudTrail en tant qu'événements de données. La colonne Type d'événement de données (console) indique la sélection appropriée dans la console CloudTrail. La colonne des types de ressources Amazon Q indique la valeur resources.type que vous devez spécifier pour enregistrer les événements de données relatifs à la ressource. De plus amples informations sont disponibles dans le [Q User Guide] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/logging-using-cloudtrail.html).

**Applications professionnelles Amazon Q** : AWS : :QBusiness : :Application
* Répertorier les tâches de synchronisation des sources de données
* Démarrer la tâche de synchronisation des sources de données
* Arrêter le travail de synchronisation des sources de données
* Document de sortie par lots
* Supprimer le document par lots
* Envoyer des commentaires
* Synchronisation du chat
* Supprimer la conversation
* Lister les conversations
* Lister les messages
* Groupes de listes
* Supprimer le groupe
* Obtenir un groupe
* Groupe PUT
* Créer un utilisateur
* Supprimer l'utilisateur
* Obtenir un utilisateur
* Mettre à jour l'utilisateur
* Lister les documents

**Ressource de données professionnelles Amazon Q** : AWS : :QBusiness : :DataSource
* Répertorier les tâches de synchronisation des sources de données
* Démarrer la tâche de synchronisation des sources de données
* Arrêter le travail de synchronisation des sources de données

**Indice Amazon Q** : AWS : :QBusiness : :Index
* Supprimer le groupe
* Obtenir un groupe
* Groupe PUT
* Groupes de listes
* Lister les documents
* Document de sortie par lots
* Supprimer le document par lots

### Analyse

En cas d'incident, outre l'étude des indicateurs de compromission, de l'auteur de la menace, du calendrier, etc., voici quelques questions supplémentaires à prendre en compte une fois qu'il a été confirmé qu'il s'agit d'un incident lié aux ressources Amazon Q :

1. Quelles ressources ont été créées et supprimées ?
2. Quels plugins ont été utilisés ?
3. À quelles applications Q a-t-il eu accès lors de l'incident de sécurité ?
4. Q a-t-il accédé à ces applications ?
5. Un type de fichier a-t-il été téléchargé sur Q ?
6. Q a-t-il accès à un environnement IDE ?

## Confinement et éradication

* [Supprimer ou faire pivoter les clés utilisateur IAM] (https://console.aws.amazon.com/iam/home#users) et [Root User Keys] (https://console.aws.amazon.com/iam/home#security_credential) ; vous souhaiterez peut-être faire pivoter toutes les clés de votre compte si vous ne parvenez pas à identifier une ou plusieurs clés spécifiques qui ont été exposées
* [Supprimer les utilisateurs IAM non autorisés] (https://console.aws.amazon.com/iam/home#users.)
* [Supprimer les politiques non autorisées] (https://console.aws.amazon.com/iam/home#/policies)
* [Supprimer les rôles non autorisés] (https://console.aws.amazon.com/iam/home#/roles)
* [Révoquer les informations d'identification temporaires] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Les informations d'identification temporaires peuvent également être révoquées en supprimant l'utilisateur IAM.
* REMARQUE : la suppression d'utilisateurs IAM peut avoir un impact sur les charges de travail de production et doit être effectuée avec précaution
* [Rotation des informations d'identification SMTP] (https://aws.amazon.com/premiumsupport/knowledge-center/ses-create-smtp-credentials/)
* [Comment supprimer une application Amazon Q] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-app-actions.html)
* [Comment supprimer Amazon Q Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-native-retriever-actions.html)
* [Comment supprimer Amazon Q Kenra Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-kendra-retriever-actions.html)
* [Comment supprimer des documents téléchargés] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/delete-doc-upload.html)
* [Comment supprimer une source de données Amazon Q] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-datasource-actions.html)
* [Comment supprimer Amazon Web Experience] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-exp-actions.html)

## Récupération

* Création de nouveaux utilisateurs IAM avec des politiques d'accès de moindre privilège


## Politiques de protection des données et d'IAM

### Protection des données :

* NE METTEZ JAMAIS d'informations confidentielles ou sensibles, telles que les adresses e-mail de vos clients, dans des balises ou des champs de texte libres tels que le champ Nom. Cela inclut lorsque vous travaillez avec Amazon Q ou d'autres services AWS à l'aide de la console, de l'API, de l'interface de ligne de commande AWS ou des kits SDK AWS. Toutes les données que vous saisissez dans les balises ou les champs de texte libre utilisés pour les noms peuvent être utilisées pour les journaux de facturation ou de diagnostic.
* Amazon Q stocke vos questions, leurs réponses et le contexte supplémentaire, tel que les métadonnées de console et le code dans votre IDE, afin de générer des réponses à vos questions.
* Amazon Q, les données sont envoyées et stockées dans une région des États-Unis. Les données collectées lors des conversations avec Amazon Q sont stockées dans la région USA Est (Virginie du Nord). Les données collectées lors des sessions d'erreur de la console de résolution des problèmes sont stockées dans la région de l'ouest des États-Unis (Oregon).

### Autorisations IAM :

* La politique gérée « AmazonQFullAccess » fournit un accès complet pour permettre les interactions avec Amazon Q. Tous les administrateurs et utilisateurs expérimentés y ont accès par défaut, mais pour les autres utilisateurs et rôles, le client devra joindre la politique gérée par AWS Q. Les autorisations peuvent être restreintes en fonction des actions d'Amazon Q.
* Pour poser des questions à Amazon Q dans la console AWS, une identité IAM a besoin d'autorisations pour effectuer les actions suivantes :
* StartConversation [Exemple : « Q : StartConversation"]
* Envoyer un message

* Pour résoudre les erreurs de console avec Amazon Q, une identité IAM a besoin d'autorisations pour les actions suivantes :
* Lancer l'analyse de résolution des problèmes
* Obtenir des résultats de dépannage
* Lancer l'analyse de résolution des problèmes

* Si l'une de ces actions n'est pas explicitement autorisée par une politique jointe, une erreur d'autorisation IAM est renvoyée lorsque vous essayez d'utiliser Amazon Q.


## Leçons apprises
`Il s'agit d'un endroit où vous pouvez ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin d'être « réparés », mais qu'il est important de connaître lors de l'exécution de ce manuel en fonction des exigences opérationnelles et commerciales. `

## Éléments du carnet de commandes traités

## Éléments du carnet de commandes en cours