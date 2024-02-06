# Manuel de sécurité en cas de compromission des informations d'identification d'un compte AWS

## Présentation

Dans le cadre de notre engagement continu envers nos clients, AWS fournit ce
manuel de réponse aux incidents de sécurité qui décrit les étapes nécessaires pour
détecter et traiter les informations d'identification compromises au sein de votre AWS
compte (s). Le but de ce document est de fournir des informations prescriptives
des conseils sur les mesures à prendre une fois que vous pensez qu'un événement de sécurité est
a eu lieu.

![Image](/images/nist_life_cycle.png)

*Aspects de la réponse aux incidents AWS*

## Préparation

Afin de répondre rapidement et efficacement aux incidents
activités, il est crucial de préparer les personnes, les processus, et
technologie au sein de votre organisation. Passez en revue le [*Incident de sécurité AWS]
Réponse
Guide*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html),
et mettre en œuvre les mesures nécessaires pour garantir la préparation de tous
trois domaines.

## Comment faire appel à AWS Support pour obtenir de l'aide

Il est important que vous informiez AWS dès que vous suspectez une compromission
informations d'identification associées à votre compte AWS ou à votre organisation. Voici les
étapes pour faire appel à AWS Support :

### Ouvrez un dossier d'assistance AWS

- Connectez-vous à votre compte AWS :

- C'est le premier compte AWS concerné par la sécurité
événement, pour valider la propriété du compte AWS.

- Remarque : Si vous ne parvenez pas à accéder au compte, utilisez [ceci
formulaire] (https://support.aws.amazon.com/#/contacts/aws-account-support/)
pour soumettre une demande d'assistance.

- Sélectionnez « *Services* », « *Centre d'assistance* », « *Créer un dossier* ».

- Sélectionnez le type de problème « *Compte et facturation » *.

- Sélectionnez le service et la catégorie concernés :

- Exemple :

- Service : Compte

- Catégorie : Sécurité

- Choisissez un degré de gravité :

- Support aux entreprises ou clients sur le marché : *Risque commercial critique
Question*.

- Clients du service d'assistance aux entreprises : *Question urgente sur les risques commerciaux*.

- Décrivez votre question ou votre problème :

- Fournissez une description détaillée du problème de sécurité que vous rencontrez
l'expérience, les ressources touchées et l'impact commercial.

- Heure à laquelle l'événement de sécurité a été reconnu pour la première fois.

- Résumé de la chronologie de l'événement jusqu'à présent.

- Artefacts que vous avez collectés (captures d'écran, fichiers journaux).

- L'ARN des utilisateurs ou des rôles IAM que vous pensez être compromis.

- Description des ressources concernées et de l'impact commercial.

- Niveau d'engagement au sein de votre organisation (par exemple : « Cette sécurité
l'événement bénéficie de la visibilité du PDG et du conseil d'administration de
Réalisateurs »).

- Facultatif : ajoutez des contacts de dossier supplémentaires.

- Sélectionnez « *Contactez-nous* ».

- Langue de contact préférée (peut être soumise à la disponibilité)

- Mode de contact préféré : Internet, téléphone ou chat (recommandé)

- Facultatif : contacts supplémentaires

<!-- -->

- *Cliquez sur « Soumettre » *

- **Escalations** : veuillez en informer l'équipe chargée de votre compte AWS dès que
possible, afin qu'ils puissent engager les ressources nécessaires et augmenter au fur et à mesure
nécessaire.

**Remarque :** Il est très important de vérifier votre « sécurité alternative »
Le « contact » est défini pour chaque compte AWS. Pour plus de détails, veuillez vous référer
à [ceci
article] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).

## Détection

Il existe plusieurs méthodes pour détecter les informations d'identification compromises dans votre
Environnement AWS. Voici quelques moyens de détecter les indicateurs potentiels de
Compromis (iOC). Pour une liste détaillée des IoC, veuillez vous référer à [annexe]
A.] (#appendix -a-reviewing-logs) **Remarque** : Documentez les détails de chaque
J'ai soupçonné l'IoC d'effectuer une analyse plus approfondie.

1. Donnez votre avis sur Amazon [GuardDuty]
résultats] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html).
Les résultats ci-dessous sont liés à une activité suspecte ou anormale
par des entités IAM. Passez en revue le [résultat]
détails] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html),
et si l'activité est inattendue, cela peut indiquer une compromission
titre de compétence.

1. CredentialAccess:IAMUser/AnomalousBehavior

2. DefenseEvasion:IAMUser/AnomalousBehavior

3. Discovery:IAMUser/AnomalousBehavior

4. Exfiltration:IAMUser/AnomalousBehavior

5. Impact:IAMUser/AnomalousBehavior

6. InitialAccess:IAMUser/AnomalousBehavior

7. PenTest:IAMUser/KaliLinux

8. PenTest:IAMUser/ParrotLinux

9. PenTest:IAMUser/PentooLinux

10. Persistence:IAMUser/AnomalousBehavior

11. Policy:IAMUser/RootCredentialUsage

12. PrivilegeEscalation:IAMUser/AnomalousBehavior

13. Recon:IAMUser/MaliciousIPCaller

14. Recon:IAMUser/MaliciousIPCaller.Custom

15. Recon:IAMUser/TorIPCaller

16. Stealth:IAMUser/CloudTrailLoggingDisabled

17. Stealth:IAMUser/PasswordPolicyChange

18. UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B

19. UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration

20. UnauthorizedAccess:IAMUser/MaliciousIPCaller

21. UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom

22. UnauthorizedAccess:IAMUser/TorIPCaller

2. Consultez AWS Billing pour détecter des pics inhabituels qui peuvent être le signe d'un compte
et compromission des informations d'identification en suivant ces étapes :

1. Connectez-vous à la console de gestion AWS et ouvrez le [Facturation]
console] (https://console.aws.amazon.com/billing/).

2. Choisissez « *Factures » * pour voir le détail de vos frais actuels.

3. Choisissez « *Paiements » * pour consulter l'historique de vos transactions de paiement.

4. Choisissez « *Rapports sur les coûts et l'utilisation d'AWS » * pour voir les rapports défectueux
réduisez vos dépenses.

3. Téléchargez le rapport sur les informations d'identification IAM en accédant à la console IAM et
choisissez le « *Rapport d'identification » * sur la gauche sous « *Accès »
rapports » * puis passez en revue les points suivants :

1. Identifiez la création inhabituelle d'un utilisateur IAM en consultant la date de création
et les dernières colonnes du mot de passe utilisées/modifiées.

2. Vérifiez si des utilisateurs de l'IAM possèdent deux clés d'accès ou plus.

3. Vérifiez si des utilisateurs d'IAM ont
*AWS ExposedCredentialPolicy \ _DO \ _NOT \ _REMOVE* ci-joint. Si c'est le cas,
faites pivoter ses clés d'accès.

4. Passez en revue les rôles IAM sur le compte AWS, afin d'identifier ceux qui ne vous sont pas familiers
rôles qui ont été créés ou auxquels on a accédé

1. Dans la console AWS, sélectionnez « *Service*s », « *IAM* », et
« *Rôles* »

2. Passez en revue tous les « *noms de rôle* » inconnus.

3. Cliquez sur le nom du rôle et passez en revue les informations répertoriées :

1. Date de création du rôle

2. ARN

3. Dernière activité

4. Cliquez sur « *Autorisations* » pour consulter les politiques IAM ci-jointes
au rôle.

5. Cliquez sur « *Relations de confiance* » pour voir les entités qui peuvent assumer
le rôle.

6. Cliquez sur « *Access Advisor* » pour voir quels services ont été
accessible par le rôle et par la « *Date du dernier accès* ».

5. Détectez toutes les ressources non reconnues ou non autorisées au sein de votre AWS
un compte tel que :

1. instances EC2 en exécutant la commande suivante dans l'AWS CLI
terminal « *aws ec2 describe-instances* » et validez le
résultats. Recherchez l'heure de création de chaque instance à l'aide du
*—query 'Reservations \ [\] .Instances \ [\]. {ip : adresse IP publique,
tm : LaunchTime} '--filters 'name=tag:name, Values=
MyInstanceName' | jq 'sort \ _by (.tm) | reverse |. \ [0 \] '* filtre.

2. Lambda fonctionne en exécutant la commande suivante dans l'AWS CLI
terminal « *aws lambda list-functions* » et validez le dernier
a modifié l'heure en utilisant le filtre « | *grep « LastModified* » ».

6. Consultez toutes les notifications de sécurité d'AWS concernant votre compte en
se connecter à [AWS Support]
Center] (https://support.console.aws.amazon.com/support/home#/) puis
lire et répondre au (x) message (s).

7. Passez en revue les résultats en suivant ces étapes :

1. Ouvrez la [console IAM] (https://console.aws.amazon.com/iam/).

2. Choisissez « *Analyseur d'accès » * dans la colonne de gauche sous Access
rapports.

3. Sous « *Résultats actifs » * passez en revue les résultats pour identifier les ressources
dans votre organisation, comme les compartiments S3, les rôles IAM ou Lambda
fonctions partagées avec des entités externes.

## Analyse

Une fois que vous aurez identifié des ressources ou des activités suspectes susceptibles
indiquer un compromis, effectuer une analyse plus approfondie dans votre SIEM ou enregistrer
outils d'analyse. AWS dispose de différents outils de services de sécurité pour vous aider à
analyse des événements liés à la sécurité. Certains de ces outils incluent :

1. **Amazon Guardduty** - Amazon GuardDuty est un outil de détection des menaces
un service qui surveille en permanence les activités malveillantes et
comportement non autorisé visant à protéger vos comptes AWS, Amazon Elastic
Charges de travail Compute Cloud (EC2), applications de conteneurs, Amazon Aurora
bases de données et données stockées dans Amazon Simple Storage Service (S3).

2. **AWS Security Hub** - Le hub de sécurité AWS est une solution de sécurité dans le cloud
service de gestion (CSPM) qui effectue des opérations automatisées et continues
les meilleures pratiques de sécurité comparées à vos ressources AWS pour vous aider
identifier les erreurs de configuration et agréger vos alertes de sécurité
(c'est-à-dire les résultats) dans un format standardisé afin que vous puissiez
les enrichir, les étudier et les corriger.

3. **Amazon Detective** - Amazon Detective facilite les analyses,
enquêter et identifier rapidement la cause première du potentiel
des problèmes de sécurité ou des activités suspectes.

Vous pouvez également effectuer une recherche dans l'historique de vos événements AWS CloudTrail, en suivant
ces étapes pour analyser et recueillir des preuves :

1. Analysez les journaux AWS CloudTrail pour détecter les éléments suivants :

1. Toute activité inhabituelle associée aux connexions :

1. Accédez au [Sentier dans le cloud]
Tableau de bord] (https://console.aws.amazon.com/cloudtrail).

2. Sur le côté gauche, sélectionnez Historique des événements.

3. Dans le menu déroulant, remplacez « En lecture seule » par « Nom de l'événement ».

4. Passez en revue les événements disponibles pour détecter toute activité suspecte en
en recherchant les termes suivants via le champ de recherche :
« *ConsoleLogin* », « *AssumeRole* », « *GetFederationToken* »,
« *GetCredentialReport* », « *GenerateCredentialReport* ». Également
il est important de noter que « *UserIdentity* » doit apparaître
sous la forme « *type* » : « *Root* » pour l'utilisateur root ou « *type* » :
« *IAMuser* » pour tous les utilisateurs IAM locaux du compte.

<!-- -->

1. Localisez l'identifiant de clé d'accès et le nom d'utilisateur IAM utilisés pour lancer un
instance Amazon EC2 suspecte :

1. Ouvrez la console AWS CloudTrail et choisissez « *Event
historique » * depuis le volet de navigation.

2. Sélectionnez le menu déroulant « *Attributs de recherche » *, puis
choisissez « *Nom de la ressource » *.

3. Dans le champ Entrez le nom de la ressource, collez l'ID de l'instance EC2,
puis appuyez sur Entrée sur votre appareil.

4. Développez le nom de l'événement pour *RunInstances*.

5. Copiez la clé d'accès AWS et notez le nom d'utilisateur.

2. Consultez l'historique des événements AWS CloudTrail pour connaître l'activité du
clé d'accès compromise :

1. Ouvrez la console CloudTrail et choisissez « *Événement »
> historique » * depuis le volet de navigation.

2. Sélectionnez le menu déroulant « *Attributs de recherche » *, puis
> choisissez « *Clé d'accès AWS » *.

3. Dans le champ « *Entrez une clé d'accès AWS » *, entrez
> ID de clé d'accès IAM compromis.

4. Développez le nom de l'événement pour l'appel d'API *RunInstances*.

3. Si vous accédez à AWS via un fournisseur d'identité tiers, soyez
assurez-vous de vérifier les journaux d'accès et de suivre les conseils du fournisseur
sur la réponse à un événement et la sécurisation de votre environnement.

<!-- -->

1. Analysez les informations d'identification compromises par IAM Access Advisor pour la dernière fois
des informations pour savoir quel est le dernier service auquel elle a accédé en suivant
ces étapes :

1. Accédez à la [console IAM] (https://console.aws.amazon.com/iam).

2. Accédez aux utilisateurs ou aux rôles, cliquez sur le nom du compromis
principal, choisissez l'onglet « *conseiller d'accès » * et regardez lequel
ressources auxquelles il a accédé pour la dernière fois.

## Confinement

Après avoir analysé et recueilli plus d'informations sur le compromis
informations d'identification et toutes les autres ressources concernées, il est temps de contrôler et
supprimer l'événement de sécurité.

1. Désactivez le ou les utilisateurs IAM, créez une clé d'accès IAM de sauvegarde, puis
désactivez la clé d'accès compromise en suivant ces étapes :

1. Ouvrez la [console IAM] (https://console.aws.amazon.com/iam/) et
collez l'identifiant de la clé d'accès IAM dans la barre de recherche IAM.

2. Choisissez le nom d'utilisateur, puis choisissez « *Sécurité »
onglet « accréditations* ».

3. Dans le champ du mot de passe de la console, choisissez « *Gérer* ».

4. Dans Accès à la console, choisissez « *Désactiver* », puis sélectionnez Appliquer.

5. Pour la clé d'accès IAM compromise, choisissez « *Rendre inactive* ».

2. Faites pivoter les clés d'accès en suivant ces étapes :

1. Tout d'abord, créez une deuxième clé en accédant au [IAM
console] (https://console.aws.amazon.com/iam/).

2. Dans le volet de navigation, choisissez « *Utilisateurs » *.

3. Choisissez le nom de l'utilisateur prévu, puis choisissez
l'onglet « *Informations de sécurité » *.

4. Dans la section « *Clés d'accès » *, choisissez « *Créer une clé d'accès » *. Sur
la page « *Accédez aux meilleures pratiques* *et aux alternatives » *,
choisissez « *Autre » *, puis choisissez « *Suivant » . *

5. Modifiez ensuite votre application pour utiliser la nouvelle clé.

6. Désactivez (mais ne supprimez pas) la première touche.

7. En cas de problème avec votre candidature, réactivez le
clé temporairement. Lorsque votre application sera pleinement fonctionnelle, et
la première touche est désactivée. Ce n'est qu'alors que vous pouvez supprimer le
première clé. Assurez-vous de conserver une trace de tous les accès supprimés
des clés pour continuer à les rechercher dans les journaux d'AWS CloudTrail.

3. Révoquez les sessions actives du ou des rôles IAM en suivant ces étapes :

1. Ouvrez la [console IAM] (https://console.aws.amazon.com/iam/) et
accédez au rôle et cliquez sur le rôle IAM que vous souhaitez activer
sessions pour.

2. Cliquez sur le nom du rôle IAM et accédez à l'onglet « *révoquer les sessions » *.

3. Cliquez sur le bouton « *révoquer les sessions actives* » et confirmez l'étape.

4. Isolez les ressources concernées en suivant ces étapes :

1. Pour les instances Amazon EC2, accédez à la console des instances EC2, vérifiez
dans la case à côté de l'instance EC2 que vous souhaitez isoler, cliquez sur
*actions*, cliquez sur *sécurité*, puis sur *modifier la sécurité
groupes*. Détachez tous les groupes de sécurité actuels et associez un
groupe de sécurité isolé qui bloque les entrées et les sorties
communication avec l'EC2.

2. Pour les compartiments Amazon S3, utilisez [bucket
politiques] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)
pour empêcher toute adresse IP suspecte d'accéder au S3
des seaux.

5. Révoquer les sessions de l'Identity Center :

Avec Identity Center, il y a deux sessions qui peuvent être préoccupantes
qui sont la session sur le portail d'accès et les sessions de rôle/de candidature :

1. Séance sur le portail d'accès :

1. Désactiver l'utilisateur dans Identity Center :

1. Accédez à la console Identity Center et sélectionnez « Utilisateurs »

2. Choisissez le nom d'utilisateur de l'utilisateur à désactiver

3. Dans la zone Informations générales de l'utilisateur, cliquez sur « Désactiver »
> accès utilisateur »

2. Révoquer toutes les sessions actives :

1. Sur la page de l'utilisateur dans Identity Center, sélectionnez « Actif »
> onglet des sessions

2. Sélectionnez toutes les sessions répertoriées, puis cliquez sur « Supprimer la session »

2. Révoquer les sessions de rôle :

1. Identifiez le ou les ensembles d'autorisations utilisés par l'utilisateur.

1. Depuis la console Identity Center, cliquez sur Ensembles d'autorisations

2. Sélectionnez le nom de l'ensemble d'autorisations.

3. Faites défiler la page jusqu'à « Politique en ligne » et cliquez sur le bouton Modifier

4. Ajoutez la politique suivante :

{

« Version » : « 17/10/2012 »,

« Déclaration » : \ [

{

« Effet » : « Refuser »,

« Action » : « \ * »,

« Ressource » : « \ * »,

« État » : {

« StringEquals » : {

« IdentityStore:UserId » : « exemple »

},

« DateLessThan » : {

« AWS : Heure d'émission du jeton » : « 26/09/2023 15:00:00.000 Z »

}

}

}

\]

}

Pour cette politique, remplacez « exemple » par le nom d'utilisateur Identity Center de l'utilisateur.
Le nom d'utilisateur se trouve dans le champ « Informations générales » du
Page utilisateur. La valeur de AWS:TokenIssueTime doit être égale au temps en
à laquelle vous appliquez cette politique.

1. Sessions de candidature

Les sessions d'application sont créées lorsqu'un utilisateur accède à un tiers
application ou service AWS directement depuis le portail d'accès.

Pour révoquer les sessions de candidature, consultez la documentation du
application à laquelle vous avez accédé.

1. Centre d'identité compromis

Si votre fournisseur d'identité est compromis, vous devez bloquer l'accès à
Identity Center provenant d'un fournisseur d'identité compromis (c'est-à-dire externe)
Fournisseur d'identité basé sur SAML (ou Active Directory) en modifiant le
source d'identité vers le « répertoire des sources d'identité ».

Pour changer votre source d'identité :

6.1 Accédez à la console Identity Center

6.2 Sélectionnez « Paramètres »

6.3 Sur la page des paramètres, cliquez sur l'onglet « Source d'identité »

6.4 Cliquez sur le bouton « Actions » puis sur « Changer d'identité »
source'

6.5 Sélectionnez « Répertoire du centre d'identité » dans « Choisir une source d'identité »
page

6.6 Cliquez sur le bouton « Suivant »

6.7 Lisez les informations contenues dans le champ « Vérifier et confirmer »,

6.8 Tapez « ACCEPTER » dans le champ approprié, puis cliquez sur « Modifier »
bouton « source d'identité »

Remarque : si vous accédez à AWS via un fournisseur d'identité tiers,
assurez-vous de vérifier les journaux d'accès et de suivre les directives du fournisseur sur
répondre à un événement et sécuriser votre environnement.

De plus, si vous passez d'Active Directory à Identity Center
annuaire, tous les utilisateurs et groupes seront supprimés. Les ensembles d'autorisations seront
ne pas être supprimée, mais les attributions d'ensembles d'autorisations seront supprimées. Si vous
passent d'un fournisseur d'identité externe basé sur SAML, tous les utilisateurs
et les groupes resteront renseignés dans Identity Center, tout comme le
assignations d'ensembles d'autorisations

## Éradication

Une fois que vous aurez fini de contenir l'événement de sécurité, il sera temps de travailler
sur la suppression et le nettoyage de la cause de l'événement de sécurité.

1. Supprimez toutes les ressources créées par les clés compromises qui étaient
détectée lors de la « phase d'analyse, étape 1 ». Vérifiez même toutes les régions AWS
régions dans lesquelles vous ne lancez pas de ressources AWS. Si vous en avez besoin
conservez une ressource pour les enquêtes, pensez à la sauvegarder.

2. Vérifiez et
[supprimer] (https://repost.aws/knowledge-center/terminate-resources-account-closure)
tous les services non reconnus utilisés sur votre compte. Rémunération
attention particulière aux ressources suivantes :

1. Instances EC2 et AMI, y compris les instances situées dans le
État.

2. Volumes et instantanés EBS.

3. Fonctions et couches AWS Lambda.

3. Supprimez les autorisations inutiles lors de la journalisation des buckets ou du journal S3
ressources d'agrégation identifiées à l'étape 6 de la « phase de détection » qui
pourrait être utilisé pour éviter d'être repérée. Cela vous permet d'identifier ce qui n'est pas intentionnel
accès à vos ressources et à vos données.

4. Supprimez toutes les données exposées qui ne sont pas nécessaires aux opérations.

5. Effectuez des analyses des failles de sécurité sur les sites destinés au public
ressources. Vous pouvez utiliser des outils tels que [Amazon
Inspecteur] (https://docs.aws.amazon.com/inspector/latest/user/scanning-resources.html)
pour scanner le système d'exploitation et une application logicielle <sup>tierce</sup> pour
vulnérabilités.

## Récupération

Une fois que vous aurez terminé d'éliminer les causes des événements de sécurité. C'est l'heure
pour remettre les ressources concernées dans un état connu.

1. Restaurez les données nécessaires à partir de sauvegardes propres connues antérieures au
événement :

1. [Restauration à partir d'un instantané Amazon EBS ou d'un
AMI] (https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/restore.html).

2. [Restauration depuis une base de données
instantané] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) Amazon
RDS.

3. [Restaurer le précédent
versions] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RestoringPreviousVersions.html) Amazon
Versions des objets S3.

2. Si nécessaire, reconstruisez les systèmes à partir de zéro, y compris en les redéployant
auprès d'une source fiable grâce à l'automatisation, parfois sur un nouveau compte AWS.

3. Si l'accès à l'authentification multifactorielle « MFA » est perdu et que vous ne le faites pas
vous avez un autre appareil MFA enregistré sur votre compte root, s'il vous plaît
suivez ces étapes :

1. Connectez-vous à [AWS Management]
Console] (https://console.aws.amazon.com/) en tant que propriétaire du compte
en choisissant Utilisateur Racine et en saisissant l'adresse e-mail de votre compte AWS
adresse. Sur la page suivante, entrez votre mot de passe.

2. Sur la page Vérification supplémentaire requise, sélectionnez un MFA
méthode pour vous authentifier et choisir Next.

3. Selon le type de MFA que vous utilisez, vous devriez voir un
page différente, mais l'option « *Résoudre les problèmes liés au MFA » * fonctionne
pareil. Sur la page « *Vérification supplémentaire requise » *
ou page « *Authentification multifactorielle » *, choisissez « *Résoudre les problèmes
MFA » *.

4. Si nécessaire, saisissez à nouveau votre mot de passe et choisissez *Se connecter*.

5. Sur la page « *Résoudre les problèmes liés à votre dispositif d'authentification » *, dans
le « *Connectez-vous en utilisant d'autres facteurs de
section « Authentification » *, choisissez « *Connectez-vous en utilisant une alternative
facteurs » *.

6. Sur le bouton « *Connectez-vous en utilisant un autre facteur » de
page d'authentification » *, authentifiez votre compte en vérifiant
l'adresse e-mail, choisissez « *Envoyer un e-mail de vérification » *

7. Vérifiez l'adresse e-mail associée à votre compte AWS pour trouver un
message d'Amazon Web Services
(recover-mfa-no-reply@verify.signin.aws). Suivez les indications
dans l'e-mail.

> Si vous ne voyez pas l'e-mail sur votre compte, consultez votre dossier de courriers indésirables, ou
> retournez sur votre navigateur et choisissez « *Renvoyer l'e-mail* ».

1. Après avoir vérifié votre adresse e-mail, vous pouvez continuer à vous authentifier
votre compte. Pour vérifier votre numéro de téléphone principal,
choisissez Call me now.

2. Répondez à l'appel d'AWS et, lorsque vous y êtes invité, entrez les 6 chiffres
numéro indiqué sur le site Web d'AWS sur le clavier de votre téléphone.

> Si vous ne recevez aucun appel d'AWS, choisissez Se connecter pour vous connecter au
> Encore une fois sur console et recommencez à zéro. Ou consultez [Multifactoriel perdu ou inutilisable]
> Authentification (MFA)
> appareil] (https://support.aws.amazon.com/#/contacts/aws-mfa-support) pour
> contactez l'assistance pour obtenir de l'aide.

1. Après avoir vérifié votre numéro de téléphone, vous pouvez vous connecter à votre compte
> en choisissant Se connecter à la console.

2. L'étape suivante varie en fonction du type de MFA que vous utilisez :

1. Pour un appareil MFA virtuel, supprimez le compte de votre appareil.
> Ensuite, rendez-vous sur [AWS Security]
> Informations d'identification] (https://console.aws.amazon.com/iam/home ? (#security_credential) page
> et supprimez l'ancienne entité d'appareil virtuel MFA avant de créer
> un nouveau.

2. Pour obtenir une clé de sécurité FIDO, rendez-vous sur le [AWS Security]
> Informations d'identification] (https://console.aws.amazon.com/iam/home ? (#security_credential) page
> et désactivez l'ancienne clé de sécurité FIDO avant d'en activer une nouvelle
> un.

3. Pour un jeton TOTP matériel, contactez le fournisseur tiers pour
> aider à réparer ou à remplacer l'appareil. Vous pouvez continuer à signer
> en utilisant d'autres facteurs d'authentification jusqu'à ce que vous
> recevez votre nouvel appareil. Une fois que vous aurez le nouveau matériel MFA
> appareil, accédez au [AWS Security
> Informations d'identification] (https://console.aws.amazon.com/iam/home ? (#security_credential) page
> et supprimez l'ancienne entité matérielle MFA avant vous
> créez-en un nouveau.

<!-- -->

1. Vérifiez que les responsables de l'IAM disposent des droits d'accès et des autorisations appropriés
à la suite de l'événement de sécurité.

2. Corrigez les vulnérabilités et installez les correctifs nécessaires à l'aide de [AWS]
Patch SSM
manager] (https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html)
ou tout autre outil <sup>tiers</sup> que vous utilisez.

3. Remplacez tous les fichiers compromets/endommagés par des versions propres de
sauvegarde.

## Activité après un incident

Une fois que vous vous êtes remise de l'incident de sécurité, vous
devrait effectuer les exercices décrits dans [*Incident de sécurité AWS
Guide de réponse, après l'incident
activité*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/establish-framework-for-learning.html).
Cela vous aidera à documenter les leçons tirées de l'incident, à mesurer
et améliorez l'efficacité de vos capacités de réponse aux incidents,
et mettre en œuvre des contrôles supplémentaires pour empêcher qu'un incident similaire ne se produise
récurrent.

## Conclusion

Dans ce playbook, nous avons décrit les premières étapes à suivre lorsqu'un
un événement de sécurité des informations d'identification compromis se produit sur votre/vos compte (s) AWS.
Cela inclut le fait de faire appel au support AWS, de détecter un compromis, d'analyser
événements liés à votre ou vos comptes, contenant l'événement de sécurité,
éradiquer la menace, récupérer votre/vos compte (s) pour un compte « dont le fonctionnement a été vérifié »
état opérationnel et activités après l'incident, y compris les leçons
appris. À l'étape suivante, veuillez consulter les ressources AWS suivantes, pour
contribuer à améliorer vos capacités de réponse aux incidents :

1. [Réponse aux incidents de sécurité d'AWS
Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.html)

2. [Framework AWS Customer Playbook pour les utilisateurs en cas de compromission de l'IAM
Informations d'identification] (https://github.com/aws-samples/aws-customer-playbook-framework/blob/main/docs/Compromised_IAM_Credentials.md)

3. [Bonnes pratiques de sécurité AWS en matière de
IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

4. [Framework AWS Well Architected — Sécurité
Pilier] (https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

## Annexe A — Révision des journaux

**Structure du journal CloudTrail**

Lorsque vous effectuez des recherches dans les journaux de CloudTrail, il est important de comprendre
les différents champs contenus dans un enregistrement d'événement CloudTrail. Pour un
liste complète, veuillez consulter le [<u>officiel] Sentier dans le cloud
</u>documentation] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Heure de l'événement</td>
<td>La date et l'heure auxquelles la demande a été traitée, en coordination
heure universelle (UTC).</td>
</tr>
<tr class="even">
<td>Nom de l'événement</td>
<td>L'action demandée, qui est l'une des actions de l'API pour
ce service</td>
</tr>
<tr class="odd">
<td>Source de l'événement</td>
<td>Le service auprès duquel la demande a été faite. Ce nom est généralement
forme abrégée du nom du service sans espaces, plus .amazonaws.com.</td>
</tr>
<tr class="even">
<td>Identité de l'utilisateur</td>
<td>Informations sur l'identité IAM à l'origine de la demande. Pour en savoir plus
informations, voir <a
<u>href= » https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html « > Sentier dans le cloud
Élément Identité de l'utilisateur.</u></a></td>
</tr>
<tr class="odd">
<td>ressources</td>
<td><p>Une liste des ressources consultées lors de l'événement. Le champ peut contenir
les informations suivantes :</p>
<ul>
<li><p>ARN des ressources</p></li>
<li><p>ID de compte du propriétaire de la ressource</p></li>
<li><p>Identifiant du type de ressource au format :
AWS : :aws-service-name : :data-type-name</p></li>
</ul></td>
</tr>
<tr class="even">
<td>Région AWS</td>
<td>La région AWS à laquelle la demande a été envoyée, telle que us-east-2</td>
</tr>
<tr class="odd">
<td>Adresse IP source</td>
<td>L'adresse IP à partir de laquelle la demande a été faite. Pour les actions qui
proviennent de la console de service, l'adresse indiquée est celle du
la ressource client sous-jacente, et non le serveur Web de la console.</td>
</tr>
</tbody>
</table>

**Événements**

Les acteurs de la menace effectueront un certain nombre d'actions par la suite
compromettre les informations d'identification du compte. Bien qu'il ne soit pas pratique de répertorier tous
action possible, voici quelques modèles à rechercher lors de votre
enquête.

**Remarque :** Les actions de l'API ci-dessous n'indiquent pas nécessairement une sécurité
un incident s'est produit. Évaluez toutes les activités enregistrées dans le contexte
de votre enquête.

**Modifications apportées à la configuration du fournisseur d'identité SAML/OIDC**

Les acteurs de la menace peuvent créer ou modifier le fournisseur d'identité SAML/OIDC
configurations pour éviter d'être détectée et maintenir la persistance au sein de votre
environnement cloud.

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Action relative à l'API</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>IAM : Créer un fournisseur AML</td>
<td>Crée une ressource IAM qui décrit un fournisseur d'identité (IdP)
qui prend en charge le protocole SAML 2.0</td>
</tr>
<tr class="even">
<td>IAM : Supprimer le fournisseur SAML</td>
<td>Supprime une ressource de fournisseur SAML dans IAM.</td>
</tr>
<tr class="odd">
<td>IAM : met à jour le fournisseur AML</td>
<td>Met à jour le document de métadonnées d'un fournisseur SAML existant</td>
</tr>
<tr class="even">
<td>IAM : Créer un fournisseur de connexion OpenID</td>
<td>Crée une entité IAM pour décrire un fournisseur d'identité (IdP) qui
supporte OpenID Connect (OIDC)</td>.
</tr>
<tr class="odd">
<td>IAM : Ajouter l'identifiant du client au fournisseur OpenID Connect</td>
<td>Ajoute un nouvel identifiant client (également appelé audience) à la liste des clients
Identifiants déjà enregistrés pour l'IAM OpenID Connect (OIDC) spécifié
ressource pour les fournisseurs</td>
</tr>
<tr class="even">
<td>IAM : Supprimer le fournisseur de connexion OpenID</td>
<td>Supprime un objet de ressource de fournisseur d'identité (IdP) OpenID Connect dans
IAM</td>
</tr>
<tr class="odd">
<td>IAM : Supprimer l'identifiant du client du fournisseur OpenIdConnect</td>
<td>Supprime l'identifiant client spécifié (également appelé audience) du
liste des identifiants clients enregistrés pour l'IAM OpenID Connect spécifié
objet de ressource du fournisseur (OIDC)</td>
</tr>
<tr class="even">
<td>IAM : Mettre à jour l'empreinte numérique du fournisseur OpenID Connect</td>
<td>Remplace la liste existante des empreintes digitales des certificats de serveur
associé à un objet de ressource du fournisseur OpenID Connect (OIDC) avec un
nouvelle liste d'empreintes digitales</td>
</tr>
</tbody>
</table>

**Modifications apportées à la configuration IAM**

Les acteurs de la menace peuvent créer ou modifier des principes IAM, des informations d'identification ou
autorisations pour éviter d'être détectée ou pour maintenir la persistance dans votre cloud
environnement.

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Action relative à l'API</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>IAM : Changer le mot de passe</td>
<td>Change le mot de passe de l'utilisateur IAM qui appelle
</td>opération
</tr>
<tr class="even">
<td>IAM : Créer un utilisateur</td>
<td>Crée un nouvel utilisateur IAM pour votre compte AWS</td>
</tr>
<tr class="odd">
<td>IAM : Créer un rôle</td>
<td>Crée un nouveau rôle pour votre compte AWS</td>
</tr>
<tr class="even">
<td>IAM : Créer un groupe</td>
<td>Crée un nouveau groupe</td>
</tr>
<tr class="odd">
<td>IAM : Politique relative aux pièces jointes</td>
<td>Attache la politique gérée spécifiée à l'utilisateur spécifié</td>
</tr>
<tr class="even">
<td>IAM : Politique relative aux rôles attachés</td>
<td>Attache la politique gérée spécifiée au rôle IAM spécifié</td>
</tr>
<tr class="odd">
<td>IAM : Politique du groupe Attacher</td>
<td>Attache la politique gérée spécifiée à l'IAM spécifié
</td>groupe
</tr>
<tr class="even">
<td>IAM : Répertorier les clés d'accès</td>
<td>Renvoie des informations sur les identifiants de clé d'accès associés au
utilisateur IAM spécifié</td>
</tr>
<tr class="odd">
<td>IAM : Créer une version de la politique</td>
<td>Crée une nouvelle version de la politique gérée spécifiée</td>
</tr>
<tr class="even">
<td>IAM : Mettre à jour le profil de connexion</td>
<td>Change le mot de passe de l'utilisateur IAM indiqué</td>
</tr>
<tr class="odd">
<td>IAM : Créer une clé d'accès</td>
<td>Crée une nouvelle clé d'accès secrète AWS et la clé d'accès AWS correspondante
ID de l'utilisateur indiqué</td>
</tr>
<tr class="even">
<td>IAM : Mettre à jour la clé d'accès</td>
<td>Fait passer le statut de la clé d'accès spécifiée d'Actif à
Inactif, ou vice versa.</td>
</tr>
<tr class="odd">
<td>IAM : Désactiver l'appareil MFA</td>
<td>Désactive le dispositif MFA spécifié et le supprime de toute association
avec le nom d'utilisateur pour lequel il a été initialement activé</td>
</tr>
</tbody>
</table>

**Modifications apportées aux configurations de journalisation et de surveillance**

Les acteurs de la menace peuvent désactiver la surveillance des ressources ou supprimer des journaux pour éviter
détecter ou dissuader les enquêtes sur les incidents.

<table>
<colgroup>
<col style="width: 46%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header">
<th>CloudTrail : Supprimer le sentier</th>
<th>Supprime un parcours, en désactivant la diffusion des événements CloudTrail à un
bucket Amazon S3, CloudWatch Logs ou Amazon EventBridge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>CloudTrail : arrêtez de vous connecter</td>
<td>Suspend l'enregistrement des appels d'API AWS et la livraison des fichiers journaux pour
le sentier spécifié</td>
</tr>
<tr class="even">
<td>Cloud Trail : Update Trail</td>
<td>Met à jour les paramètres qui spécifient la livraison des fichiers journaux</td>
</tr>
<tr class="odd">
<td>CloudTrail : Supprimer la banque de données d'événements</td>
<td>Supprime un <a
<u>href= » https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store.html « > Sentier dans le cloud
Lake Event Date Store</u></a></td>
</tr>
<tr class="even">
<td>Devoir de garde : supprimer le détecteur</td>
<td>Supprime un détecteur Amazon GuardDuty et désactive GuardDuty dans un
région en particulier</td>
</tr>
<tr class="odd">
<td>Devoir de garde : supprimer la destination de publication</td>
<td>Supprime la destination de publication, où les résultats sont exportés
</td>à
</tr>
<tr class="even">
<td>Analyseur d'accès : DeleteAnalyzer</td>
<td>Supprime l'analyseur spécifié, désactivant l'analyseur d'accès IAM
service pour cette région.</td>
</tr>
<tr class="odd">
<td>Configuration : Supprimer la règle de configuration</td>
<td>Supprime la règle de Configuration AWS spécifiée et toutes ses évaluations
</td>résultats
</tr>
<tr class="even">
<td>Configuration : Supprimer l'enregistreur de configuration</td>
<td>Supprime l'enregistreur de configuration AWS Config</td>
</tr>
<tr class="odd">
<td>Configuration : Supprimer le canal de livraison</td>
<td>Supprime le canal de diffusion d'AWS Config</td>
</tr>
<tr class="even">
<td>Configuration : Arrêter l'enregistreur de configuration</td>
<td>Arrête d'enregistrer les configurations et les modifications de configuration pour
groupe d'enregistrement spécifié</td>
</tr>
</tbody>
</table>

**Modifications apportées à la configuration du compartiment S3**

Les acteurs menaçants peuvent modifier ou supprimer les configurations du compartiment S3 afin de
exfiltrer des données.

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>S3 : List Buckets</th>
<th>Renvoie la liste de tous les buckets appartenant à l'expéditeur authentifié de
la demande</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>S3 : Create Bucket</td>
<td>Crée un nouveau bucket</td>
</tr>
<tr class="even">
<td>S3 : Supprimer le bloc d'accès public du compartiment</td>
<td>Supprime la configuration PublicAccessBlock pour un Amazon S3
</td>seau
</tr>
<tr class="odd">
<td>S3 : Politique relative au puttbucket</td>
<td>Ajoute ou met à jour une politique sur un bucket</td>
</tr>
<tr class="even">
<td>S3 : Politique de suppression du compartiment</td>
<td>Supprime la politique du bucket</td>
</tr>
<tr class="odd">
<td>S3 : Mettre un objet ACL</td>
<td>Définit les autorisations de la liste de contrôle d'accès (ACL) pour un objet dans un
compartiment Amazon S3</td>
</tr>
<tr class="even">
<td>S3 : Supprimer l'objet ACL</td>
<td>Supprimer la liste de contrôle d'accès (ACL) d'un objet</td>
</tr>
<tr class="odd">
<td>S3 : Putbucket Cors</td>
<td>Définit la configuration CORS de votre bucket</td>
</tr>
<tr class="even">
<td>S3 : Supprimer le Bucket Cors</td>
<td>Supprime les informations de configuration CORS définies pour le bucket</td>
</tr>
<tr class="odd">
<td>S3 : Chiffrement du bucket</td>
<td>Configurer le chiffrement par défaut et les clés de compartiment Amazon S3 pour un
seau existant</td>
</tr>
<tr class="even">
<td>S3 : Supprimer le cryptage du bucket</td>
<td>Réinitialise le cryptage par défaut du bucket côté serveur
chiffrement avec des clés gérées par Amazon S3 (SSE-S3</td>)
</tr>
</tbody>
</table>

**Autres actions**

Voici d'autres actions que vous devriez souligner lors de votre
enquête

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>KMS : DisableKey</th>
<th>Définit l'état d'une clé KMS sur Disabled. Ce changement est temporaire
empêche l'utilisation de la clé KMS pour les opérations cryptographiques</th>.
</tr>
</thead>
<tbody>
<tr class="odd">
<td>KMS : Suppression de la clé de planification</td>
<td>Planifie la suppression d'une clé KMS.</td>
</tr>
<tr class="even">
<td>KMS : Politique de Putkey</td>
<td>Attache une politique clé à la clé KMS spécifiée. Les principales politiques sont les
principal moyen de contrôler l'accès aux clés KMS.</td>
</tr>
<tr class="odd">
<td>KMS : CreateKey</td>
<td>Crée une clé KMS unique gérée par le client sur votre compte AWS et
Région.</td>
</tr>
<tr class="even">
<td>EC2 : Décrivez les instances</td>
<td>Décrit les instances EC2 de votre compte.</td>
</tr>
<tr class="odd">
<td>EC2 : Exécuter des instances</td>
<td>Lance des instances EC2 sur votre compte.</td>
</tr>
<tr class="even">
<td>EC2 : Créer un VPC</td>
<td>Crée un VPC sur votre compte.</td>
</tr>
<tr class="odd">
<td>EC2 : Décrivez les VPC</td>
<td>Décrit les VPC de votre compte.</td>
</tr>
<tr class="even">
<td>EC2 : Créer un groupe de sécurité</td>
<td>Crée un groupe de sécurité. Un groupe de sécurité agit comme un groupe virtuel
pare-feu pour votre instance afin de contrôler le trafic entrant et sortant</td>.
</tr>
<tr class="odd">
<td>EC2 : Supprimer le groupe de sécurité</td>
<td>Supprime un groupe de sécurité.</td>
</tr>
<tr class="even">
<td>RDS : instance de base de données créée</td>
<td>Crée une instance de base de données RDS.</td>
</tr>
<tr class="odd">
<td>RDS : instance B supprimée</td>
<td>Supprime une instance de base de données RDS.</td>
</tr>
</tbody>
</table>