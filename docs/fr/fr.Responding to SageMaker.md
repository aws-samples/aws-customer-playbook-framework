# Manuel de réponse aux incidents : réponse aux événements de sécurité de SageMaker
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date de publication de ce document, qui sont susceptibles d'être modifiées sans préavis. Les clients sont tenus de procéder à leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « tel quel » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses filiales, fournisseurs ou concédants de licence. Les responsabilités et obligations d'AWS envers ses clients sont régies par les accords AWS, et le présent document ne fait partie d'aucun accord conclu entre AWS et ses clients et ne les modifie pas.

© 2024 Amazon Web Services, Inc. ou ses filiales. Tous droits réservés. Ce travail est mis à disposition selon les termes de la licence internationale Creative Commons Attribution 4.0.

Ce contenu AWS est fourni conformément aux termes du contrat client AWS disponible à l'adresse http://aws.amazon.com/agreement ou à tout autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL, ou les deux.


## Points de contact

Auteur : `Nom de l'auteur` \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé
Dans le cadre de son engagement continu envers ses clients, AWS fournit ce manuel de réponse aux incidents de sécurité qui décrit les étapes nécessaires pour enquêter sur les événements de sécurité dans lesquels Amazon SageMaker est la source ou la cible d'une utilisation non autorisée au sein de vos comptes AWS. Le but de ce document est de fournir des conseils prescriptifs sur les mesures à prendre lorsque vous soupçonnez qu'un événement de sécurité s'est produit.

! [Photo] (sagemaker_images/nist_life_cycle.png)

*Aspects de la réponse aux incidents AWS*


### Amazon SageMaker

Amazon SageMaker est un service d'apprentissage automatique (ML) entièrement géré. Avec SageMaker, les data scientists et les développeurs peuvent créer, former et déployer rapidement et en toute confiance des modèles de machine learning dans un environnement hébergé prêt pour la production. Il fournit une interface utilisateur pour exécuter des flux de travail ML qui rend les outils SageMaker ML disponibles dans plusieurs environnements de développement intégrés (IDE).

Avec SageMaker, vous pouvez stocker et partager vos données sans avoir à créer et à gérer vos propres serveurs. Grâce à la prise en charge intégrée des algorithmes et des frameworks « Bring-your-own », SageMaker propose des options de formation distribuées flexibles qui s'adaptent à des flux de travail spécifiques. Pour plus d'informations, consultez le guide du développeur pour Amazon SageMaker ici : https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html



## Préparation
Préparez votre environnement de manière proactive en mettant en œuvre des contrôles préventifs ([Service Control Policies] (https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)) et de détection (voir la section Détection pour les règles de configuration).


**Préventif (SCP) **

VPC FrequLogs
* Par exemple, le SCP suivant empêchera les utilisateurs de lancer des ordinateurs portables, des formations ou des tâches de traitement à moins qu'un VPC ne soit spécifié.

```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Déploiement d'un VPC »,
« Effet » : « Refuser »,
« Action » : [
« SageMaker : créer une tâche de réglage d'hyperparamètres »,
« SageMaker : Créer un modèle »,
« SageMaker : créer une instance de bloc-notes »,
« Sage Maker : créer une tâche de traitement »,
« Sage Maker : créer un poste de formation »
],
« Ressource » : [
« * »
],
« État » : {
« Null » : {
« SageMaker:VPCSecurityGroupIds » : « vrai »,
« SageMaker:VPCSubnets » : « vrai »
}
}
}
]
}
```


* Appliquer le chiffrement des tâches

```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Refuser les volumes non chiffrés »,
« Effet » : « Refuser »,
« Action » : [
« SageMaker : créer une tâche de réglage d'hyperparamètres »,
« Sage Maker : créer un poste de formation »,
« SageMaker : créer une configuration de point de terminaison »,
« Sage Maker : CreateTransformJob »
],
« Ressource » : [
« * »
],
« État » : {
« Null » : {
« SageMaker : Volume KMSKey » : [
« vrai »
]
}
}
}
]
}
```
* Appliquer le chiffrement du trafic inter-conteneurs
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Refuser le trafic non chiffré »,
« Effet » : « Refuser »,
« Action » : [
« Sage Maker : créer un poste de formation »,
« SageMaker : créer une tâche de réglage d'hyperparamètres »
],
« Ressource » : [
« * »
],
« État » : {
« Livre » : {
« SageMaker : chiffrement du trafic interconteneur » : « faux »
}
}
}
]
}
```

* Appliquer l'isolation du réseau
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « DenyNoTisolated »,
« Effet » : « Refuser »,
« Action » : [
« Sage Maker : créer un poste de formation »,
« SageMaker : créer une tâche de réglage d'hyperparamètres »,
« SageMaker : créer un modèle »
],
« Ressource » : « * »,
« État » : {
« Livre » : {
« SageMaker:NetworkIsolation » : « faux »
}
}
}
]
}
```
* Restreindre l'URL pré-signée du bloc-notes aux adresses IP
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Restreindre l'URL à l'adresse IP »,
« Effet » : « Refuser »,
« Action » : « SageMaker : CreatePresignedNotebookInstanceURL »,
« Ressource » : « * »,
« État » : {
« Pour toutes les valeurs : pas d'adresse IP » : {
« AWS : adresse IP source » : [
« [ENTRER_ADRESSE_IP_PUBLIQUE] »
]
}
}
}
]
}
```
* Désactiver l'accès à Internet
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Refuser DirectInternet »,
« Effet » : « Refuser »,
« Action » : « SageMaker : créer une instance de bloc-notes »,
« Ressource » : « * »,
« État » : {
« StringEquals » : {
« SageMaker : accès direct à Internet » : [
« Activé »
]
}
}
}
]
}
```

* Désactiver l'accès Root dans les blocs-notes SageMaker
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « SageMaker refuse l'accès à Root »,
« Effet » : « Refuser »,
« Action » : [
« SageMaker : créer une instance de bloc-notes »,
« SageMaker : mettre à jour une instance de bloc-notes »
],
« Ressource » : « * »,
« État » : {
« StringEquals » : {
« SageMaker : Accès root » : [
« Activé »
]
}
}
}
]
}
```
* Restreindre les types d'instances qui peuvent être démarrés par les utilisateurs
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Types d'instances de SageMakerLimit»,
« Effet » : « Refuser »,
« Action » : « SageMaker : créer une instance de bloc-notes »,
« Ressource » : « * »,
« État » : {
« Pour toute valeur : StringNotLike » : {
« SageMaker : types d'instances » : [
« [EXEMPLE_TYPES D'INSTANCE] »,
« ml.c5.xlarge »,
« ml.m5.xlarge »,
« ml.t3.medium »
]
}
}
}
]
}
```

* De même, pour Studio, consultez l'exemple de politique suivant. Notez que les administrateurs doivent autoriser l'instance système pour les applications Jupyter Server par défaut.
```
{
« Version » : « 17/10/2012 »,
« Déclaration » : [
{
« Sid » : « Types d'instances autorisés par SageMaker »,
« Effet » : « Refuser »,
« Action » : [
« SageMaker : créer une application »
],
« Ressource » : « * »,
« État » : {
« Pour toute valeur : StringNotLike » : {
« SageMaker : types d'instances » : [
« ml.c5.large »,
« ml.m5.large »,
« ml.t3.medium »,
« système »
]
}
}
}
]
}
```


## Détection

### Contrôles SageMaker

### Configuration AWS

AWS Config dispose de plusieurs [règles gérées pour évaluer SageMaker] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* Configuration du point de terminaison SageMaker configurée par clé KMS
* sagemaker-endpoint-config-prod-instance-count
* instance de bloc-notes SageMaker à l'intérieur du VPC
* SageMaker Notebook-Instance-KMS configuré par clé
VPC Flow-Logs
* ordinateur portable SageMaker sans accès direct à Internet

### Événements CloudTrail

En cas d'accès non autorisé à votre environnement, consultez ci-dessous les scénarios possibles liés aux appels d'API SageMaker pertinents à titre de référence rapide (veuillez noter qu'il ne s'agit pas d'une liste complète des appels d'API SageMaker) :


**Exfiltration de données/modèles :**
- Copier ou télécharger des données sensibles depuis les magasins de données SageMaker ou les artefacts du modèle.
- Extraire les paramètres du modèle ou les données d'entraînement, exposant potentiellement des informations de propriété intellectuelle ou personnelles.

<ins>Exemples d'appels d'API :</ins>
- `DescribeModelPackage` pour récupérer des informations sur les modèles de packages.
- `DescribeTrainingJob` pour accéder aux détails des tâches de formation et à leurs données de sortie.
- `GetModelPackageModelMetrics` pour récupérer les métriques du modèle et les données potentiellement sensibles.



**Empoisonnement des données :**
* Modifier ou empoisonner des modèles entraînés en injectant des données malveillantes ou des exemples contradictoires.
* Déploiement de modèles compromis sur les terminaux SageMaker, entraînant des prédictions incorrectes ou des sorties malveillantes.

<ins>Exemples d'appels d'API :</ins>
- `CreateModelPackage` ou `UpdateModelPackage` pour déployer un modèle de package compromis.
- `CreateTransformJob` pour exécuter une tâche de transformation avec un modèle empoisonné.
- `CreateEndpointConfig` et `CreateEndpoint` pour déployer un modèle malveillant sur un point de terminaison.
- `CreateTrainingJob` ou `UpdateTrainingJob` pour injecter des données malveillantes dans les tâches de formation.



**Utilisation abusive des ressources :**
* Lancement d'ordinateurs portables ou d'instances SageMaker non autorisés à des fins de cryptomining ou d'autres activités malveillantes.
* Utilisation des ressources SageMaker comme points d'entrée ou points de pivot pour les mouvements latéraux dans l'environnement AWS.

<ins>Exemples d'appels d'API :</ins>
- `CreateNotebookInstance` ou `UpdateNotebookInstance` pour lancer des instances de bloc-notes non autorisées.
- `CreateTrainingJob` ou `CreateHyperParameterTuningJob` pour lancer des tâches de formation excessives.
- `CreateEndpoint` ou `UpdateEndpoint` pour créer ou modifier des points de terminaison à des fins non autorisées.

**Déni de service (DoS) :**
* Épuisement des ressources de SageMaker (par exemple, instances de calcul, stockage) en lançant des tâches de formation ou des points de terminaison excessifs.
* Surcharger les API ou les services SageMaker avec un volume élevé de demandes, ce qui entraîne des interruptions de service.

<ins>Exemples d'appels d'API :</ins>
- `CreateTrainingJob` ou `CreateHyperParameterTuningJob` pour lancer de nombreuses tâches de formation et épuiser des ressources.
- `CreateEndpoint` ou `UpdateEndpoint` pour créer plusieurs points de terminaison et consommer des ressources de calcul excessives.

**Changements de configuration :**
* Modifier les rôles, les politiques ou les autorisations de SageMaker pour augmenter les privilèges ou accorder un accès non autorisé.
* Modification des configurations VPC, des groupes de sécurité ou des paramètres réseau de SageMaker pour contourner les contrôles de sécurité.

<ins>Exemples d'appels d'API :</ins>
- `CreateRole` ou `UpdateRole` pour modifier les rôles et les autorisations de SageMaker.
- `CreateNotebookInstanceLifeCycleConfig` ou `UpdateNotebookInstanceLifeCycleConfig` pour modifier les configurations des instances de bloc-notes.
- `CreateEndpointConfig` ou `UpdateEndpointConfig` pour modifier les configurations des terminaux ou les paramètres de sécurité.

**Altération du journal :**
* Modifier ou supprimer les journaux ou les pistes d'audit de SageMaker pour masquer les traces et entraver les enquêtes sur les incidents.
* Injection de fausses entrées de journal pour induire en erreur les analystes de sécurité ou masquer des activités malveillantes.

<ins>Exemples d'appels d'API :</ins>
- `PutModelPackageModelMetrics` pour injecter de fausses métriques de modèle dans les logs.
- `StopTrainingJob` ou `StopTransformJob` pour éventuellement modifier ou supprimer les données du journal.

**Déploiement de logiciels malveillants :**
* Déploiement de logiciels malveillants ou de portes dérobées dans les blocs-notes ou les instances SageMaker à des fins d'accès persistant ou de vol de données.
* Utilisation des ressources de SageMaker pour diffuser des malwares ou lancer des attaques contre d'autres systèmes ou réseaux.

<ins>Exemples d'appels d'API :</ins>
- `CreateNotebookInstance` ou `UpdateNotebookInstance` pour lancer des instances de bloc-notes contenant des logiciels malveillants.
- `CreateModelPackage` ou `UpdateModelPackage` pour déployer des modèles de packages contenant du code malveillant.

**Vol d'informations d'identification :**
* Vol d'informations d'identification AWS ou de clés d'API SageMaker stockées dans des blocs-notes ou des instances.
* Utilisation d'informations d'identification volées pour obtenir un accès non autorisé supplémentaire à d'autres ressources ou services AWS.

<ins>Exemples d'appels d'API :</ins>
- `DescribeNotebookInstance` ou `DescribeTrainingJob` pour accéder potentiellement aux informations d'identification ou aux clés d'API stockées.
- `GetModelPackageModelMetrics` ou `DescribeModelPackage` pour récupérer des informations sensibles ou des informations d'identification.

**Cryptojacking :**
* Détournement des ressources informatiques de SageMaker (instances, points de terminaison, par exemple) pour des activités d'extraction de cryptomonnaies non autorisées.
* Consommation excessive de ressources informatiques pouvant entraîner des interruptions de service ou une augmentation des coûts.

<ins>Exemples d'appels d'API :</ins>
- `CreateNotebookInstance` ou `UpdateNotebookInstance` pour lancer des instances pour le minage de cryptomonnaies.
- `CreateTrainingJob` ou `CreateHyperParameterTuningJob` pour lancer des tâches gourmandes en calcul à des fins de minage.


**Remarque : il est important de noter que ces appels d'API peuvent également être utilisés à des fins légitimes, mais dans le contexte d'un accès non autorisé, ils peuvent être utilisés à mauvais escient pour effectuer des actions risquées. La mise en œuvre de mécanismes robustes de contrôle d'accès, de surveillance et d'audit est essentielle pour détecter et empêcher une telle utilisation abusive des API et des ressources de SageMaker. **

### Comprendre les entrées du journal SageMaker

Les captures d'écran ci-dessous fournissent une aide visuelle à un intervenant en cas d'incident afin de l'aider à interpréter les événements découverts au cours d'une enquête. Chaque image ci-dessous représente les actions entreprises qui correspondent au nom de l'événement enregistré

---

** Créer un domaine**
<details>
<summary>Développer la capture d'écran</summary>
-
Crée un domaine. Un domaine se compose d'un volume Amazon Elastic File System associé, d'une liste d'utilisateurs autorisés et de diverses configurations de sécurité, d'applications, de politiques et d'Amazon Virtual Private Cloud (VPC). Les utilisateurs d'un domaine peuvent partager des fichiers de bloc-notes et d'autres artefacts entre eux.

! [Créer un domaine] (/sagemaker_images/sagemaker-01.png)
</details>

---

**Détails du domaine SageMaker**
<details>
<summary>Développer la capture d'écran</summary>

Le domaine Amazon SageMaker prend en charge les environnements d'apprentissage automatique (ML) SageMaker. Un domaine SageMaker est composé des entités suivantes : domaine, profil utilisateur, espace partagé, application


! [Détails du domaine] (/sagemaker_images/sagemaker-02.png)
! [Détails du domaine 2] (/sagemaker_images/sagemaker-03.png)

</details>

---


**Événement CloudTrail pour CreateDomain**

<details>
<summary>Développer la capture d'écran</summary>

Notez que l'événement `CreateDomain` dans Cloudtrail contient toutes les informations suivantes : VPC, sous-réseaux, rôle d'exécution, applications, etc.

! [Domaine SageMaker associé au VPC, au rôle d'exécution, aux applications, etc.] (/sagemaker_images/sagemaker-04.png)


</details>

---

**Événement CloudTrail pour CreateEndpoint**

<details>
<summary>Développer les captures d'écran</summary>

Notez que l'événement `CreateEndpoint` pour SageMaker dans CloudTrail est appelé par le rôle de service `SageMaker-ExecutionRole`

! [Exemple de création d'un point de terminaison] (/sagemaker_images/sagemaker-05.png)

</details>

---



## Analyse

En cas d'incident, en plus d'étudier les indicateurs de compromission, l'auteur de la menace, le calendrier, etc., voici quelques questions supplémentaires à prendre en compte une fois qu'il a été confirmé qu'il s'agit d'un incident lié aux ressources de SageMaker :

1. Quelles ressources SageMaker ont été consultées sans autorisation ? (ordinateurs portables, modèles, terminaux, magasins de données, etc.)
2. Comment l'accès non autorisé a-t-il été obtenu ? (par exemple, informations d'identification compromises, autorisations mal configurées, vulnérabilités exploitées)
3. Quelles actions ont été effectuées sur les ressources SageMaker concernées lors de l'accès non autorisé ?
4. Des modèles ou des données ont-ils été exfiltrés ou falsifiés ?
5. En cas d'accès aux modèles, y a-t-il un risque d'empoisonnement des modèles ou d'attaques antagonistes ?
6. De nouvelles ressources (par exemple, des blocs-notes, des terminaux) ont-elles été créées ou modifiées lors de l'accès non autorisé ?
7. Des API ou des SDK SageMaker ont-ils été utilisés lors de l'accès non autorisé, et quelles actions ont été effectuées par leur intermédiaire ?
8. Des journaux ou des pistes d'audit de SageMaker ont-ils été modifiés ou supprimés pour couvrir les traces ?
9. Des rôles SageMaker ou des politiques IAM ont-ils été modifiés ou utilisés à mauvais escient au cours de l'incident ?
10. Des configurations de SageMaker VPC ou des paramètres réseau ont-ils été modifiés ?
11. Des blocs-notes ou des instances SageMaker ont-ils été utilisés comme points d'entrée ou points de pivot pour un accès non autorisé ultérieur ?
12. Des ressources SageMaker ont-elles été utilisées pour lancer des attaques ou des activités malveillantes contre d'autres ressources AWS ou des systèmes externes ?
13. Quel est l'impact potentiel d'un accès non autorisé sur la confidentialité, l'intégrité et la disponibilité des ressources SageMaker et des données associées ?
14. Comment les ressources SageMaker concernées peuvent-elles être isolées, sauvegardées et éventuellement restaurées ou reconstruites en toute sécurité ?
15. Quelles sont les meilleures pratiques ou configurations spécifiques de SageMaker en matière de sécurité qui n'ont pas été suivies, ce qui a entraîné un accès non autorisé ?

## Confinement et éradication

Si des ressources sont créées par un utilisateur non autorisé ou si leur création n'est pas autorisée, suivez les instructions suivantes pour supprimer/modifier les ressources ou autorisations créées :

- [Comment supprimer un domaine SageMaker (console)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio)
- [Comment supprimer un domaine SageMaker (CLI)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-cli)
- [Comment supprimer un point de terminaison SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint)
- [Comment supprimer une configuration de point de terminaison SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint-config)
- [Comment supprimer un modèle SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-model)
- [Supprimer l'accès Root des blocs-notes SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html) (Accédez à l'instance de bloc-notes souhaitée/Arrêter l'instance/Une fois terminé, cliquez sur Modifier/Sous Autorisations, sélectionnez Désactiver/Mettre à jour l'instance de bloc-notes ou Exécuter l'instance)


## Éléments du carnet de commandes traités
- En tant que responsable de la réponse aux incidents, je dois être capable de surveiller tous les événements critiques de SageMaker
- En tant que responsable de la réponse aux incidents, j'ai besoin d'un manuel pour interroger les événements SageMaker Cloudtrail à grande échelle

## Éléments du carnet de commandes en cours

## Annexe - Meilleures pratiques

**Fortement recommandé**

- [Déployer dans un VPC isolé] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Utiliser le point de terminaison VPC pour accéder aux ressources
- [Les ordinateurs portables Sagemaker devraient restreindre l'accès aux connexions depuis le VPC] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Utilisez des groupes de sécurité et des NACL pour contrôler le trafic entrant et sortant de votre environnement
- [Chiffrement du trafic inter-conteneurs pour les tâches de formation avec plusieurs instances de calcul] (https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)
- [Activer le chiffrement au repos à l'aide de KMS] (https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest.html)
- [Désactiver l'accès root aux blocs-notes s'il n'est pas nécessaire] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)
- [Meilleures pratiques en matière de configuration du cycle de vie] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html)
- Créez une liste d'autorisations de packages que l'équipe peut utiliser pour réduire le risque d'exécution de code malveillant
- [Privilège minimal en utilisant les rôles IAM et les politiques basées sur les ressources (par exemple, les politiques de compartiment pour accéder aux données du compartiment S3), tirer parti de la gouvernance du machine learning] (https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html)
- [Utiliser le centre d'identité IAM] (https://docs.aws.amazon.com/sagemaker/latest/dg/domain-user-profile-add-remove.html)
- [Stockez et faites pivoter les informations d'identification dans Secrets Manager] (https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating-sagemaker.html)
- [Surveillez l'entrée et la sortie du modèle à l'aide de SageMaker Model Monitor] (https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html)
- [Activer la journalisation des événements de données CloudTrail S3 pour l'audit des données et des artefacts du modèle S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)
- [Activez SageMaker Experiments et tirez parti du contrôle de version pour les artefacts du modèle] (https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)
- [Activez VPC Flow Logs pour surveiller le trafic réseau dans votre VPC] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [Utilisez CodeArtifact pour télécharger les bibliothèques/packages nécessaires sur Internet] (https://aws.amazon.com/blogs/machine-learning/secure-aws-codeartifact-access-for-isolated-amazon-sagemaker-notebook-instances/)
- [CloudWatch peut également être utilisé pour surveiller SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)

**Encourage**

- [Utilisez Amazon Macie pour protéger les données S3 sensibles] (https://docs.aws.amazon.com/macie/latest/user/data-classification.html)
- [Utilisez le catalogue de services pour vendre des ressources SageMaker prévalidées, telles que des blocs-notes, afin de limiter la taille des instances et de renforcer l'utilisation de configurations sécurisées] (https://aws.amazon.com/blogs/machine-learning/automate-a-centralized-deployment-of-amazon-sagemaker-studio-with-aws-service-catalog/)