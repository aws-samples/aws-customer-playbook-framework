# Guide de sécurité pour répondre aux événements de sécurité d'Amazon Bedrock
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date de publication de ce document, qui sont susceptibles d'être modifiées sans préavis. Les clients sont tenus de procéder à leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « tel quel » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses filiales, fournisseurs ou concédants de licence. Les responsabilités et obligations d'AWS envers ses clients sont régies par les accords AWS, et le présent document ne fait partie d'aucun accord conclu entre AWS et ses clients et ne les modifie pas.

© 2024 Amazon Web Services, Inc. ou ses filiales. Tous droits réservés. Ce travail est sous licence internationale Creative Commons Attribution 4.0.

Ce contenu AWS est fourni conformément aux termes du contrat client AWS disponible sur http://aws.amazon.com/agreement ou de tout autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL, ou les deux.

## Points de contact

Auteur : `Nom de l'auteur` \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé

Dans le cadre de notre engagement continu envers les clients, AWS fournit ce
manuel de réponse aux incidents de sécurité qui décrit les étapes nécessaires pour
étudiez les événements de sécurité dans lesquels Amazon Bedrock est la source ou la cible d'une utilisation non autorisée au sein de vos comptes AWS. Le but de ce document est de fournir des conseils prescriptifs sur les mesures à prendre lorsque vous soupçonnez qu'un événement de sécurité s'est produit.

! [Photo] (/images/nist_life_cycle.png)

*Aspects de la réponse aux incidents AWS*

## Préparation

Afin d'exécuter rapidement et efficacement les activités de réponse aux incidents, il est essentiel de préparer le personnel, les processus et les technologies au sein de votre organisation. Consultez le [*AWS Security Incident Response Guide*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html) et mettez en œuvre les étapes nécessaires pour garantir la préparation des trois domaines.

## Comment faire appel au support AWS pour obtenir de l'aide

Il est important que vous informiez AWS dès que vous soupçonnez que des informations d'identification ont été compromises au sein de votre compte AWS ou de votre organisation. Voici les étapes à suivre pour faire appel à AWS Support :

### Ouvrez un dossier de support AWS

- Connectez-vous à votre compte AWS :
- Il s'agit du premier compte AWS concerné par l'événement de sécurité, afin de valider la propriété du compte AWS.
- Remarque : Si vous ne parvenez pas à accéder au compte, utilisez [ce formulaire] (https://support.aws.amazon.com/#/contacts/aws-account-support/) pour soumettre une demande d'assistance.
- Sélectionnez « *Services* », « *Centre de support* », « *Créer un dossier* ».
- Sélectionnez le type de problème « *Compte et facturation » *.
- Sélectionnez le service et la catégorie concernés :
- Exemple :
- Service : Compte
- Catégorie : Sécurité
- Choisissez une gravité :
- Support aux entreprises ou clients en phase de démarrage : *Question critique sur les risques commerciaux*.
- Clients du service d'assistance aux entreprises : *Question urgente sur les risques commerciaux*.
- Décrivez votre question ou problème :
- Fournissez une description détaillée du problème de sécurité que vous rencontrez, des ressources concernées et de l'impact commercial.
- Heure à laquelle l'événement de sécurité a été reconnu pour la première fois.
- Résumé de la chronologie de l'événement jusqu'à présent.
- Artefacts que vous avez collectés (captures d'écran, fichiers journaux).
- L'ARN des utilisateurs ou des rôles IAM que vous soupçonnez d'être compromis.
- Description des ressources affectées et de l'impact commercial.
- Niveau d'engagement au sein de votre organisation (ex : « Cet événement de sécurité bénéficie de la visibilité du PDG et du conseil d'administration »).
- Facultatif : ajoutez des contacts supplémentaires au dossier.
- Sélectionnez « *Contactez-nous* ».
- Langue de contact préférée (sous réserve de disponibilité)
- Méthode de contact préférée : Web, téléphone ou chat (recommandé)
- Facultatif : contacts supplémentaires pour le boîtier
- *Cliquez sur « Soumettre » *

- **Escalations** : veuillez en informer l'équipe chargée de votre compte AWS dès que possible, afin qu'elle puisse engager les ressources nécessaires et intervenir en cas de besoin.

**Remarque :** Il est très important de vérifier que votre « contact de sécurité alternatif » est défini pour chaque compte AWS. Pour plus de détails, veuillez consulter [ce
article] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).


## Amazon Bedrock

Amazon Bedrock est compromis avec plusieurs composants/services :

* Modèles : il peut s'agir de modèles de base issus de grandes startups d'IA ou de modèles personnalisés
* Inférence : processus de génération d'une sortie à partir d'une entrée fournie à un modèle
* Base de connaissances - vous permet d'amasser des sources de données dans un référentiel d'informations
* Agents : aide vos utilisateurs finaux à effectuer des actions en fonction des données de l'organisation et des entrées des utilisateurs

Lorsqu'un utilisateur d'un compte AWS (autorisé ou non) accède à Bedrock, il le fait finalement via un modèle, une base de connaissances ou un agent. Toutes les demandes émises et leurs réponses seront publiées dans un modèle de journal d'invocation à condition que celui-ci ait été configuré précédemment (https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html). Pour plus d'informations, consultez le guide de l'utilisateur d'Amazon Bedrock ici : https://docs.aws.amazon.com/bedrock/

L'image ci-dessous illustre la relation entre les invites, les [agents] (https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html) et les bases de connaissances et peut être un bon moyen de visualiser le flux de données (extrait du [Guide de l'utilisateur] (https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html))

! [Construction de l'agent pendant les opérations de l'API lors de la création] (/images/bedrock-01.png)


## Détection

### Remarques et considérations

* Bedrock n'est pas disponible dans toutes les régions (2024-06-04). Voir [link] (https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-regions.html) pour connaître la disponibilité actuelle dans la région
* La journalisation des appels de modèles est requise pour afficher les invites et les réponses envoyées aux modèles. Il s'agit d'un paramètre au niveau de la région et il n'est pas défini par défaut.
* Pour CloudTrail, EventSource = bedrock.amazonaws.com
* L'API***InvokeModel*** est un événement en **lecture seule**

### Noms des événements

Les noms d'événements répertoriés dans le tableau ci-dessous constituent une référence rapide pour les types d'événements généralement rencontrés dans CloudTrail lors de l'enquête sur un événement de sécurité impliquant Bedrock. Parmi ceux répertoriés dans le tableau, les noms d'événements les plus courants enregistrés lors de l'utilisation normale du modèle sont les suivants :

* `Obtenir la disponibilité du modèle de base`
* `InvokeModèle`
* `InvokeModelWithResponseStream`

| **Noms des événements** | **ReadOnly** | **Description** |
| : -----------------| : -----------------| : -------------------------------- |
| ***Événements liés aux modèles*** | --- | --- |
| `GetFoundationModelAvailability` | TRUE | Récupère la disponibilité d'un modèle de base |
| `getUseCaseForModelAccess` | TRUE | Récupère un cas d'utilisation pour un modèle spécifié |
| `InvokeModel` | TRUE | Invoque le modèle Bedrock spécifié pour exécuter une inférence en utilisant l'entrée fournie dans le corps de la requête |
| `InvokeModelWithResponseStream` | TRUE | Invoque le modèle Bedrock spécifié pour exécuter une inférence en utilisant l'entrée fournie dans le corps de la requête à l'aide du découpage |
| `ListCustomModels` | TRUE | Répertorie les modèles personnalisés Bedrock qui ont été créés |
| `ListFoundationModels` | TRUE | Répertorie les modèles de base qui peuvent être utilisés |
| `ListProvisionedModelThroughputs` | TRUE | Répertorie les capacités provisionnées pour les modèles |
| --- | --- | --- |
| ***Événements liés à la journalisation des invocations par modèle*** | --- |
| `DeleteModelInvocationLoggingConfiguration` | FALSE | Supprime une configuration de journalisation des appels existante |
| `GetModelInvocationLoggingConfiguration` | TRUE | Récupère la configuration de journalisation des appels existante |
| `PutModelInvocationLoggingConfiguration` | FALSE | Crée une configuration de journalisation des appels existante |
| --- | --- | --- |
| ***Événements liés aux bases de connaissances*** | --- | --- |
| `AssociateAgentKnowledgebase` | FALSE | Cette action enregistre l'association d'une base de connaissances lors de la création d'un agent ou après la création d'un agent |
| `CreateDataSource` | FALSE | Crée une source de données |
| `CreateKnowledgeBase` | FALSE | Crée une base de connaissances |
| `DeleteKnowledgeBase` | FALSE | Supprime une base de connaissances |
| `GetDataSource` | TRUE | Obtient des informations sur une source de données |
| `GetKnowledgeBase` | TRUE | Récupère une base de connaissances existante |
| `ListDataSources` | TRUE | Liste les sources de données disponibles |
| `ListKnowledgeBases` | TRUE | Répertorie les bases de connaissances existantes |
| --- | --- | --- |
| ***Événements liés aux agents*** | --- | --- |
| `CreateAgent` | FALSE | Crée un nouvel agent et un alias d'agent de test pointant vers la version de l'agent DRAFT |
| `CreateAgentActionGroup` | FALSE | Crée un groupe d'actions pour un agent. Un groupe d'actions représente les actions qu'un agent peut effectuer pour le client en définissant les API qu'un agent peut appeler et la logique de leur appel |
| `CreateAgentAlias` | FALSE | Crée un nouvel alias pour un agent |
| `DeleteAgent` | FALSE | Supprime un agent créé précédemment |
| `getAgent` | TRUE | Récupère un agent existant |
| `GetAgentAlias` | TRUE | Récupère un alias existant |
| `InvokeAgent` | FALSE | Envoie une invite à l'agent pour qu'il traite et réponde à |
| `ListAgentActionGroups` | TRUE | Répertorie les groupes d'actions d'un agent et des informations sur chacun d'eux |
| `ListAgentAliases` | TRUE | Répertorie les alias d'un agent et les informations les concernant |
| `ListAgentKnowledgeBases` | TRUE | Répertorie les bases de connaissances associées à un agent et les informations sur chacune d'elles |
| `ListAgents` | TRUE | Répertorie les agents existants |
| `ListAgentVersions` | TRUE | Répertorie les versions d'un agent et les informations relatives à chaque version |
| `PrepareAgent` | FALSE | Prépare un agent Amazon Bedrock existant pour recevoir des demandes d'exécution |
| --- | --- | --- |
| ***Événements liés à l'embauche d'emploi*** | --- | --- |
| `getIngestionJob` | TRUE | Obtient des informations sur une tâche d'ingestion, dans laquelle une source de données est ajoutée à une base de connaissances |
| `ListinGestionJobs` | TRUE | Répertorie les tâches d'ingestion pour une source de données et des informations sur chacune d'elles |
| `StartingEstionJob` | FALSE | Commence une tâche d'ingestion, dans laquelle une source de données est ajoutée à une base de connaissances |
| --- | --- | --- |
| ***Événements liés à RAG*** | --- | --- |
| `Retrieve` | TRUE | Récupère les données ingérées depuis une base de connaissances (considérées comme des événements de données et n'apparaîtront pas dans CloudTrail) |
| `RetrieveAndGenerate` | FALSE | Envoie une entrée utilisateur pour effectuer la récupération et la génération (considérés comme des événements de données et n'apparaîtront pas dans CloudTrail) |

### Captures d'écran de l'utilisation de Bedrock

Les captures d'écran ci-dessous peuvent fournir une aide visuelle à un intervenant en cas d'incident afin de l'aider à interpréter les événements découverts au cours d'une enquête. Chaque image ci-dessous représente les actions entreprises qui correspondent au nom de l'événement enregistré.

---

** Obtenir la disponibilité du modèle Foundation**
<details>
<summary>Développer la capture d'écran</summary>
-
Cet événement est enregistré pour la première fois lorsque vous accédez à la page principale d'Amazon Bedrock

! [Obtenir la disponibilité du modèle de base] (/images/bedrock-02.png)

</details>

---

**ListFoundationModels** et **ListCustomModels**
<details>
<summary>Développer la capture d'écran</summary>
-
Ces événements sont enregistrés lorsque vous naviguez sur les pages « Modèles de base » et « Modèles personnalisés »

! [ListFoundationModels et ListCustomModels] (/images/bedrock-03.png)

</details>

---

** Modèle d'invoque**
<details>
<summary>Développer les captures d'écran</summary>
-
Cet événement est enregistré lors de l'invocation du modèle.

! [Exemple d'invocation de modèle] (/images/bedrock-04.png)

Exemple d'enregistrement d'événement CloudTrail pour l'invocation d'un modèle :

! [CloudTrail pour InvokeModel] (/images/bedrock-05.png)

</details>

---

**InvokeModel** - exemples supplémentaires
<details>
<summary>Développer les captures d'écran</summary>
-
Les images ci-dessous fournissent des exemples supplémentaires de la façon dont le nom de l'événement `InvokeModel` est enregistré.
La première image montre les différences entre les différentes méthodes d'invocation du modèle :
(a) Modéliser l'invocation à l'aide d'une base de connaissances
(b) Invocation directe du modèle
(c) Modèle d'invocation à l'aide d'un agent

Notez que le *Nom d'utilisateur* affiché est différent selon la manière dont le modèle est invoqué

! [Invocation du modèle d'agent et de base de connaissances] (/images/bedrock-06.png)

Cette différence est mise en évidence à l'aide de deux captures d'écran côte à côte de l'enregistrement d'événement « InvokeModel » dans CloudTrail
L'image de gauche = Invocation directe du modèle
L'image de droite = Modèle d'invocation à l'aide d'une base de connaissances

! [CloudTrail pour InvokeModel] (/images/bedrock-07.png)


</details>

---

**InvokeModel** ou **InvokeModelWithResponseStream** - invites et réponse
<details>
<summary>Développer la capture d'écran</summary>
-
Alors que les événements `InvokeModel` et `InvokeModelWithResponseStream` apparaissent dans CloudTrail, les journaux d'invocation des modèles (lorsqu'ils sont configurés) affichent également l'invite et la réponse correspondant à chaque événement `InvokeModel`. L'image ci-dessous est extraite des journaux d'invocation du modèle qui ont été envoyés à S3. Ils contiennent l'invite utilisée ainsi que le résultat ou la réponse du modèle (un DDL Athena est disponible ci-dessous) :

! [Exemple d'invocation du modèle S3] (/images/bedrock-09.png)

</details>

---

** Configuration de journalisation des invocations PUTMODEL**
<details>
<summary>Développer la capture d'écran</summary>
-
Cet événement est enregistré lorsque la journalisation des appels de modèles est configurée dans un compte AWS.

! [Configuration de la journalisation des appels du modèle] (/images/bedrock-10.png)

</details>

---

** Créer une base de connaissances**
<details>
<summary>Développer la capture d'écran</summary>
-
L'enregistrement d'événement CloudTrail suivant est enregistré lors de la création d'une base de connaissances. L'enregistrement de l'événement inclut l'identité qui a émis la demande et le nom de la base de connaissances. Le nom de la base de connaissances peut être référencé dans les journaux suivants qui capturent les invocations de modèles à l'aide de `InvokeModel`

! [Création d'une base de connaissances] (/images/bedrock-11.png)

</details>

---

** Créer un agent**
<details>
<summary>Développer la capture d'écran</summary>
-
L'image ci-dessous illustre la création d'un agent à l'aide de la console. Le nom de l'événement qui en résulte et qui est enregistré est `CreateAgent`

! [Création de l'agent] (/images/bedrock-12.png)

</details>

---

**Récupére** et **RetrieveandGenerate**
<details>
<summary>Développer la capture d'écran</summary>
-
Un exemple de la façon dont les noms d'événements « Retrieve` et `RetrieveAndGenerate` sont affichés dans l'image ci-dessous lors du test d'une base de connaissances. Notez que `Retrieve` teste uniquement la récupération des données de la base de connaissances, et `RetrieveAndGenerate` teste la récupération des données de la base de connaissances et la génération d'une réponse :

! [Récupérer et récupérer et générer] (/images/bedrock-13.png)

</details>

---

### Athéna DDL

Utilisez le code DDL ci-dessous pour créer une table dans Athena à partir des journaux d'invocation du modèle stockés dans un compartiment Amazon S3. Assurez-vous que l'URI S3 correct est utilisé pour l'emplacement du compartiment à la fin de l'extrait de code :

```
CRÉER UNE TABLE EXTERNE bedrock_model_invocation_metadata_unfiltered ()
chaîne SchemaType,
chaîne SchemaVersion,
chaîne d'horodatage,
chaîne AccountID,
<arn: string>structure d'identité,
chaîne de région,
chaîne RequestID,
chaîne d'opération,
chaîne ModelID,
Chaîne de code d'erreur,
structure d'entrée <InputBodyJSON : chaîne,
InputBodyS3Path : chaîne,
InputContentType : chaîne,
Nombre de jetons d'entrée : int>,
structure de sortie <outputBodyJSON : chaîne,
OutputBodyS3Path : chaîne,
OutputContentType : chaîne,
OutputTokenCount : int,
OutputImageBucketedStepSize : int,
Hauteur de l'image en sortie : int,
OutputImageWidth : int
>
)
FORMAT DE LIGNE SERDE 'org.openx.data.jsonserde.JSONSerde'
<insert_prefix>LIEU 's3 ://<insert_S3_bucket>//'
```

## Éléments du carnet de commandes traités
- En tant que responsable de la réponse aux incidents, je dois être capable de surveiller tous les événements critiques de Bedrock
- En tant que responsable de la réponse aux incidents, j'ai besoin d'un manuel pour interroger les événements Bedrock Cloudtrail à grande échelle

## Éléments du carnet de commandes en cours