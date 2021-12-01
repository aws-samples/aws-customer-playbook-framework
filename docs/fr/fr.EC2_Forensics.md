# Playbook de réponse aux incidents : EC2 Forensics
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

[[_TOC_]]

# Points de contact

Auteur : `Nom de l'auteur »
Approbateur : `Nom de l'approbateur`
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus de médico-légal sur des instances EC2 identifiées comme malveillantes ou présentant des indicateurs de compromis justifiant une enquête supplémentaire.

Il est également recommandé d'effectuer l'enquête dans la même région AWS où l'événement s'est produit, plutôt que de répliquer les données vers une autre région AWS. Nous recommandons cette pratique principalement en raison du temps supplémentaire nécessaire au transfert des données entre les régions. Pour chaque région AWS dans laquelle vous choisissez d'opérer, assurez-vous que votre processus de réponse aux incidents et les intervenants respectent les lois pertinentes sur la confidentialité des données. Si vous devez déplacer des données entre les régions AWS, tenez compte des implications juridiques du déplacement des données entre juridictions. Il est généralement recommandé de conserver les données dans la même juridiction nationale.

Pour plus d'informations, veuillez consulter le [Guide de réponse aux incidents de sécurité AWS] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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
1. [**PREPARATION**] Identifier les meilleures pratiques avant d'exécuter des procédures judiciaires
2. [**PREPARATION**] Créer un VPC Forensics
3. [**PREPARATION**] Ajouter un point de terminaison VPC pour SSM
4. [**PREPARATION**] Créer une liste de contrôle d'accès réseau pour le VPC Forensics
5. [**PREPARATION**] Créer une AMI dorée Forensics EC2
6. [**PREPARATION**] Mettre en œuvre un programme de formation pour identifier et évaluer les capacités de criminalistique dans le cloud
7. [**PREPARATION**] Identifier, documenter et tester les procédures d'escalade
8. [**DÉTECTION ET ANALYS**] Créer un instantané Amazon EBS
9. [**DÉTECTION ET ANALYS**] Partager un instantané Amazon EBS
10. [**DÉTECTION ET ANALYSIS**] Partager un instantané non chiffré à l'aide de la console
11. [**DÉTECTION ET ANALYSIS**] Utilisez un instantané non chiffré qui a été partagé en privé avec vous
12. [**DÉTECTION ET ANALYSIS**] Partager un instantané chiffré à l'aide de la console
13. [**DÉTECTION ET ANALYSIS**] Utilisez un instantané chiffré qui a été partagé avec vous
14. [**DÉTECTION ET ANALYSIS**] Créer un EC2 Forensics à partir de Forensics Golden AMI
15. [**DÉTECTION ET ANALYSE**] Monter le volume EBS suspecté sur Forensics EC2
16. [**DÉTECTION ET ANALYSIS**] Exécution de processus médico-légaux
17. [**CONFINEMENT**] Isoler immédiatement les ressources affectées
18. [**RECOVERY**] Effectuer les processus de récupération le cas échéant

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents
* **Tactiques, techniques et procédures** : Outils : Forensics
* **Catégorie** : Médico-légale
* **Ressource** : EC2, VPC
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers, mesures Cloudwatch, AWS Shield
* **Sources du journal : Endpoint
* **Équipes** : Centre des opérations de sécurité (SOC), enquêteurs judiciaires, ingénierie du cloud

## Processus de gestion des incidents
### Le processus de réponse aux incidents comporte les étapes suivantes :
* Préparation
* Détection et analyse
* Confinement et éradication
* Récupération
* Activité post-incident

## Préparation
### [Meilleures pratiques] (https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)
* Activer AWS CloudTrail dans toutes les régions
Ne limitez pas votre journalisation CloudTrail aux régions AWS que vous utilisez activement. L'activation de CloudTrail dans chaque région AWS vous permettra d'identifier plus facilement les comportements inhabituels, tels que les services AWS provisionnés à partir d'une région AWS que votre organisation n'utilise pas.

* Protection des informations du journal
Vérifiez que les pistes d'audit sont activées et actives pour les composants système, et sauvegardez-les régulièrement. Envisagez de les stocker dans un emplacement qui nécessite des informations d'identification différentes, afin que les attaquants ne puissent pas supprimer les fichiers journaux susceptibles de fournir des preuves de leur activité malveillante.

* Ne jamais ignorer la communication contre les abus AWS
AWS enverra automatiquement un e-mail à votre adresse e-mail enregistrée lorsqu'un cas d'abus est déposé. Répondez immédiatement et envisagez de configurer une adresse de réponse e-mail dédiée. Plus vous réagissez rapidement à un incident de sécurité, mieux vous pouvez limiter son impact sur votre entreprise. Il est bon d'avoir une [liste de distribution pour le contact alternatif pour la sécurité] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html#manage-account-payment-alternate-contacts) dans le compte qui est composé de plusieurs parties prenantes afin que les messages ne soient pas « bloqués » dans un seul boîte de réception de l'employé (par exemple en vacances, licenciement, malade, etc.).

### Créer un VPC Forensics
**NOTE** : Il est recommandé de disposer d'un VPC médico-légal dans chaque région où la médico-légale peut devoir être effectuée en fonction des ressources de production.

1. Ouvrez la [console Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. Dans le volet de navigation, choisissez Your VPC, Create VPC.
1. Spécifiez les détails suivants du VPC, le cas échéant.
* Balise de nom : indiquez éventuellement un nom pour votre VPC. Cela permet de créer une balise avec une clé Name et la valeur que vous spécifiez.
* Bloc CIDR IPv4 : spécifiez un bloc CIDR IPv4 pour le VPC. Le plus petit bloc CIDR que vous pouvez spécifier est /28, et le plus grand est /16. Nous vous recommandons de spécifier un bloc CIDR à partir des plages d'adresses IP privées (non routables publiquement) telles que spécifiées dans la RFC 1918, par exemple 10.0.0.0/16 ou 192.168.0.0/16.
* Remarque : Vous pouvez spécifier une plage d'adresses IPv4 publiquement routables. Toutefois, nous ne prenons actuellement pas en charge l'accès direct à Internet à partir de blocs CIDR routables publiquement dans un VPC.
* Les instances Windows ne peuvent pas démarrer correctement si elles sont lancées sur un VPC dont la plage est comprise entre 224.0.0.0 et 255.255.255.255 (plages d'adresses IP de classe D et E).
* Bloc CIDR IPv6 : associez éventuellement un bloc CIDR IPv6 à votre VPC. Choisissez l'une des options suivantes, puis sélectionnez Sélectionner le CIDR :
* Bloc CIDR IPv6 fourni par Amazon : demande un bloc CIDR IPv6 à partir du pool d'adresses IPv6 d'Amazon. Pour Network Border Group, sélectionnez le groupe à partir duquel AWS publie des adresses IP.
* CIDR IPv6 appartenant à moi : (BYOIP) Alloue un bloc CIDR IPv6 à partir de votre pool d'adresses IPv6. Pour Pool, choisissez le pool d'adresses IPv6 à partir duquel allouer le bloc CIDR IPv6.
* Location : sélectionnez une option de location.
* Sélectionnez Dédié pour vous assurer que les instances lancées dans ce VPC sont des instances de location dédiées, quel que soit l'attribut de location spécifié au lancement.
* Sélectionnez Par défaut pour vous assurer que les instances lancées dans ce VPC utilisent l'attribut de location spécifié au lancement.
* (Facultatif) Ajoutez ou supprimez une balise.
* Choisissez Créer.

### [Ajouter un point de terminaison VPC pour SSM] (https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1. Vérifiez que l'agent SSM est installé sur l'instance.
2. Créez un profil d'instance AWS Identity and Access Management (IAM) pour Systems Manager. Vous pouvez créer un nouveau rôle ou ajouter les autorisations nécessaires à un rôle existant.
3. Attachez le rôle IAM à votre instance EC2 privée.
4. Ouvrez la console Amazon EC2, puis sélectionnez votre instance. Dans l'onglet Description, notez l'ID du VPC et l'ID de sous-réseau.
5. Créez un point de terminaison VPC pour Systems Manager.
* Pour Nom du service, sélectionnez com.amazonaws. [région] .ssm (par exemple, com.amazonaws.us-east-1.ssm). Pour obtenir la liste complète des codes de région, voir Régions disponibles.
* Pour VPC, choisissez l'ID VPC de votre instance.
* Pour les sous-réseaux, choisissez un ID de sous-réseau dans votre VPC. Pour une haute disponibilité, choisissez au moins deux sous-réseaux provenant de différentes zones de disponibilité au sein de la région.
* Remarque : Si vous avez plusieurs sous-réseaux dans la même zone de disponibilité, vous n'avez pas besoin de créer des points de terminaison VPC pour les sous-réseaux supplémentaires. Tous les autres sous-réseaux de la même zone de disponibilité peuvent accéder à l'interface et l'utiliser.
* Pour Activer le nom DNS, sélectionnez Activer pour ce point de terminaison. Pour plus d'informations, voir DNS privé pour les points de terminaison d'interface.
* Pour le groupe de sécurité, sélectionnez un groupe de sécurité existant ou créez-en un nouveau. Le groupe de sécurité doit autoriser le trafic HTTPS entrant (port 443) à partir des ressources de votre VPC qui communiquent avec le service.
* Si vous avez créé un nouveau groupe de sécurité, ouvrez la console VPC, choisissez Groupes de sécurité, puis sélectionnez le nouveau groupe de sécurité. Dans l'onglet Règles entrantes, choisissez Modifier les règles entrantes. Ajoutez une règle avec les détails suivants, puis choisissez Enregistrer les règles :
* Pour Type, choisissez HTTPS.
* Pour Source, choisissez votre CIDR VPC. Pour une configuration avancée, vous pouvez autoriser le CIDR de sous-réseaux spécifiques utilisé par vos instances EC2.
* Notez l'ID du groupe de sécurité. Vous allez utiliser cet ID avec les autres points de terminaison.
6. Créez des stratégies pour les points de terminaison de l'interface VPC pour AWS Systems Manager.
* Répétez l'étape 5 avec la modification suivante : Pour Nom du service, sélectionnez com.amazonaws. Messages [région] .ec2.
* Répétez l'étape 5 avec la modification suivante : Pour Nom du service, sélectionnez com.amazonaws. [région] .ssmmessages. Vous devez le faire si vous souhaitez utiliser le Gestionnaire de session.

### Créer une liste de contrôle d'accès réseau pour le VPC Forensics
Ce processus isolera le VPC afin que le trafic ne puisse ni entrer ni quitter les instances contenues dans

1. Ouvrez la [console Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. Dans le volet de navigation, choisissez Sécurité, ACL réseau.
1. Activez la case à cocher en regard du VPC Forensics
1. En haut à droite, sélectionnez la liste déroulante Actions et Modifier les règles entrantes.
1. Changer la règle 100 de « Tout le trafic » en `SSH`
1. Sélectionnez Enregistrer les modifications.
1. Répétez ces étapes pour les règles sortantes

### Créer une AMI dorée EC2 Forensics
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Sélectionnez « Launch Instance »
1. Dans la barre de recherche, tapez `Ubuntu` et appuyez sur Entrée
1. Sélectionnez « Ubuntu Server 18.04 LTS »
1. Choisir un type d'instance
* Remarque : Pour des raisons judiciaires, il est recommandé de choisir une instance de type M5
1. Sélectionnez « Suivant : Configurer les détails de l'instance »
* Réseau : choisissez le réseau VPC de police légale
* Attribution automatique de l'adresse IP publique : désactiver
* Rôle IAM : sélectionnez un rôle approprié à des fins médico-légales
* Activer la protection contre la terminaison : Assurez-vous que cette case est cochée
1. Sélectionnez « Suivant : Ajouter un stockage »
* Sélectionnez « Ajouter un nouveau volume » et créez un volume de taille suffisante pour gérer les données médico-légales capturées. Une bonne valeur de départ est de 100 Go, mais fondez cette valeur sur les besoins et les expériences de votre organisation
1. Sélectionnez « Suivant : Ajouter des balises »
1. Sélectionnez « Vérifier et lancer »
1. Sélectionnez « Launch »
1. Connectez-vous aux instances EC2 via EC2 Session Manager
1. Installez vos outils médicolégaux sur l'instance. Voici quelques exemples d'outils médico-légaux Open Source :
1. Installation de la boîte à outils SANS SIFT
* Sudo su - Ubuntu
* sudo curl -Lo /usr/local/bin/sift https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux
* sudo chmod +x /usr/local/bin/sift
* installation sudo /usr/local/bin/sift —mode=server —user=ubuntu
* mise à niveau sudo /usr/local/bin/sift
1. Installation de Google Rapid Response
* sudo apt install mysql-server
* sudo mysql_secure_installation
* créer une base de données grr ; accorder tous les privilèges sur grr.* à grr @localhost identifié par « mot de passe » ; privilèges de vidage ;
* wget https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb
* installation sudo apt. /grr-server_3.2.4-6_amd64.deb
* redémarrage du serveur grr-server sudo systemctl
* sudo systemctl status grr-server
1. Pour plus d'informations, reportez-vous à [comment installer et configurer des clients GRR sur Ubuntu 18.04] (https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/)
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Accédez aux instances
1. Cliquez avec le bouton droit sur l'instance que vous souhaitez utiliser comme base de votre AMI, puis choisissez Créer une image dans le menu contextuel.
1. Dans la boîte de dialogue Créer une image, saisissez un nom et une description uniques, puis choisissez Create Image.
* Par défaut, Amazon EC2 arrête l'instance, prend des instantanés de tous les volumes attachés, crée et enregistre l'AMI, puis redémarre l'instance.
* Choisissez Pas de redémarrage si vous ne souhaitez pas que votre instance soit arrêtée.
* Si vous choisissez Pas de redémarrage, nous ne pouvons pas garantir l'intégrité du système de fichiers de l'image créée.

La création de l'AMI peut prendre quelques minutes. Une fois créé, il apparaîtra dans la vue AMI dans AWS Explorer. Pour afficher cette vue, double-cliquez sur le nœud Amazon EC2 | AMI dans AWS Explorer. Pour voir vos AMI, dans la liste déroulante Affichage, choisissez Owned By Me. Vous devrez peut-être choisir Actualiser pour voir votre AMI. Lorsque l'AMI apparaît pour la première fois, elle peut être en attente, mais après quelques instants, elle passe à un état disponible.

### Formation
- `Quelle est l'attente de connaissances pour le personnel ayant des responsabilités de médico-légal ? (connaissances/compétences et capacités) »
- `Quelles formations et procédures médico-légales reçoivent mon personnel ? `
- `Quelles sont les exigences réglementaires applicables à la criminalistique pour mon entreprise ? `
- `Quelle est la formation mise en place pour que les analystes de l'entreprise se familiarisent avec l'API AWS (environnement de ligne de commande), S3, RDS et d'autres services AWS ? `

**Il faut ajouter ici plus de contexte et d'éléments, car la médico-légale est une compétence, pas seulement un livre de jeu

## Procédures d'escalade
- « J'ai besoin d'une décision commerciale quant au moment où la criminalistique EC2 devrait être effectuée »
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

## Détection et analyse
### Sources possibles d'alertes
* Facturation
* CloudWatch (règle de mesure EC2/événement)
* Config
* Devoir de garde
* Inspecteur
* Macie
* Hub de sécurité
* Conseiller de confiance
* Notification externe

### Protection des données
Vous devriez **TOUWAYS** :
* Faire des preuves EN LECTURE SEULE
* Les preuves ne doivent JAMAIS être modifiées
* Créer des copies de preuves et de travaux uniquement à partir de ceux-ci
* **NEVER** travailler à partir de données probantes (accès direct) à des fins d'analyse
* Monter toutes les preuves EN LECTURE SEULE
* Gardez une trace de qui a accédé à QUOI et QUAND
* Prenez de nombreuses notes d'enquête sur :
* Ce que vous avez analysé (quel élément de données)
* Lorsque vous l'avez analysé (date/heure)
* Ce que vous avez effectué (commandes spécifiques/procédure/processus)
* Ce que vous avez trouvé (résultats)

### Acquisition de métadonnées
Capturez les informations relatives à l'instance qui peuvent être utiles pour l'enquête
* Type d'instance/ID
* Adresse (s) IP (s)
* Groupe (s) de sécurité
* ID VPC/sous-réseau
* Région
* ID AMI
* Heure de lancement

### Acquisition de mémoire
Il est recommandé d'utiliser un utilitaire tiers pour acquérir de la mémoire à partir d'instances en cours d'exécution.
Il est essentiel de capturer la mémoire **AVANT L'isolation/arrêt du système** afin de capturer toutes les données volatiles (et précieuses) disponibles

### [Créer un instantané Amazon EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Choisissez Snapshots sous Elastic Block Store dans le volet de navigation.
1. Choisissez Create Snapshot.
1. Pour Sélectionner un type de ressource, choisissez Volume.
1. Pour Volume, sélectionnez le volume.
1. (Facultatif) Entrez une description pour l'instantané.
1. (Facultatif) Choisissez Ajouter une balise pour ajouter des balises à votre instantané. Pour chaque balise, indiquez une clé de balise et une valeur de balise.
1. Choisissez Create Snapshot.

### [Partage d'un instantané Amazon EBS] (https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
#### Partager un instantané non chiffré à l'aide de la console
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Choisissez Snapshots dans le volet de navigation.
1. Sélectionnez l'instantané, puis choisissez Actions, Modifier les autorisations.
1. Rendez l'instantané public ou partagez-le avec des comptes AWS spécifiques comme suit :
* Pour rendre l'instantané public, choisissez Public.
* Cette option n'est pas valide pour les instantanés chiffrés ou les instantanés avec un code produit AWS Marketplace.
* Pour partager l'instantané avec un ou plusieurs comptes AWS, choisissez Privé, entrez l'ID de compte AWS (sans traits d'union) dans le numéro de compte AWS, puis choisissez Ajouter une autorisation. Répétez cette opération pour tous les comptes AWS supplémentaires.
1. Choisissez Enregistrer.
#### Pour utiliser un instantané non chiffré partagé en privé avec vous
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Choisissez Snapshots dans le volet de navigation.
1. Choisissez le filtre Instantanés privés.
1. Localisez l'instantané par ID ou par description. Vous pouvez utiliser cet instantané comme n'importe quel autre. Par exemple, vous pouvez créer un volume à partir de l'instantané ou copier l'instantané vers une autre région.

#### Partager un instantané chiffré à l'aide de la console
1. Ouvrez la [console AWS KMS] (https://console.aws.amazon.com/kms)
1. Pour modifier la région AWS, utilisez le sélecteur de région situé dans le coin supérieur droit de la page.
1. Choisissez Clés gérées par le client dans le volet de navigation.
1. Dans la colonne Alias, choisissez l'alias (lien texte) de la clé gérée par le client que vous avez utilisée pour chiffrer l'instantané. Les principaux détails s'ouvrent dans une nouvelle page.
1. Dans la section Stratégie de clé, vous voyez la vue de stratégie ou la vue par défaut. La vue de stratégie affiche le document de stratégie clé. La vue par défaut affiche des sections pour les administrateurs clés, la suppression de clés, l'utilisation de la clé et les autres comptes AWS. La vue par défaut s'affiche si vous avez créé la stratégie dans la console et que vous ne l'avez pas personnalisée. Si la vue par défaut n'est pas disponible, vous devez modifier manuellement la stratégie dans la vue de stratégie. Pour plus d'informations, consultez Viewing a Key Policy (Console) dans le manuel AWS Key Management Service Developer Guide.
Utilisez la vue de stratégie ou la vue par défaut, en fonction de la vue à laquelle vous pouvez accéder, pour ajouter un ou plusieurs ID de compte AWS à la stratégie, comme suit :
* (Vue par défaut) Faites défiler l'écran jusqu'à Autres comptes AWS. Choisissez Ajouter d'autres comptes AWS et entrez l'ID de compte AWS à la demande. Pour ajouter un autre compte, choisissez Ajouter un autre compte AWS et entrez l'ID de compte AWS. Lorsque vous avez ajouté tous les comptes AWS, choisissez Enregistrer les modifications.
* (Vue de la stratégie) Choisissez Modifier. Ajoutez un ou plusieurs ID de compte AWS aux instructions suivantes : « Autoriser l'utilisation de la clé » et « Autoriser l'attachement de ressources persistantes ». Choisissez Enregistrer les modifications. Dans l'exemple suivant, l'ID de compte AWS 444455556666 est ajouté à la stratégie.
```
{
« Sid » : « Autoriser l'utilisation de la clé »,
« Effect » : « Autoriser »,
« Principal » : {"AWS » : [
« arn:aws:iam። 111122223333 : utilisateur/utilisateur CMK »,
« arn:aws:iam። 44455556666:root »
]},
« Action » : [
« KMS : Crypter »,
« KMS : Décrypter »,
« KMS : Rechiffrer* »,
« KMS : Générer une clé de données* »,
« KMS : Describe Key »
],
« Ressource » : « * »
},
{
« Sid » : « Autoriser l'attachement de ressources persistantes »,
« Effect » : « Autoriser »,
« Principal » : {"AWS » : [
« arn:aws:iam። 111122223333 : utilisateur/utilisateur CMK »,
« arn:aws:iam። 44455556666 : root »
]},
« Action » : [
« KMS : Create Grant »,
« KMS : ListGrants »,
« KMS : Révoquer la subvention »
],
« Ressource » : « * »,
« Condition » : {"Bool » : {"kms:GrantisForAWSResource » : true}}
}
```
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Choisissez Snapshots dans le volet de navigation.
1. Sélectionnez l'instantané, puis choisissez Actions, Modifier les autorisations.
1. Pour chaque compte AWS, entrez l'ID de compte AWS dans le numéro de compte AWS et choisissez Ajouter une autorisation. Lorsque vous avez ajouté tous les comptes AWS, choisissez Enregistrer.

#### Pour utiliser un instantané chiffré qui a été partagé avec vous
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Choisissez Snapshots dans le volet de navigation.
1. Choisissez le filtre Instantanés privés. Vous pouvez également ajouter le filtre chiffré.
1. Localisez l'instantané par ID ou par description.
1. Sélectionnez l'instantané et choisissez Actions, Copier.
1. (Facultatif) Sélectionnez une région de destination.
1. La copie de l'instantané est chiffrée par la clé affichée dans la clé principale. Par défaut, la clé sélectionnée est la clé CMK par défaut de votre compte. Pour sélectionner un CMK géré par le client, cliquez à l'intérieur de la zone de saisie pour afficher la liste des clés disponibles.
1. Choisissez Copier.

### Créer un EC2 Forensics à partir de Forensics Golden AMI
Créez une nouvelle instance Forensics à l'aide de votre AMI Forensics Golden créée précédemment

### Monter le volume EBS suspecté sur Forensics EC2
1. Ouvrez la [console Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Dans le volet de navigation, choisissez Elastic Block Store, Volumes.
1. Sélectionnez un volume disponible et choisissez Actions, Attach Volume.
1. Par exemple, commencez à saisir le nom ou l'ID de l'instance.
1. Sélectionnez l'instance dans la liste des options (seules les instances se trouvant dans la même zone de disponibilité que le volume sont affichées).
1. Pour Device, vous pouvez conserver le nom suggéré de l'appareil ou saisir un autre nom d'appareil pris en charge.
* Pour plus d'informations, voir [Noms de périphériques sur les instances Linux] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/device_naming.html).
1. Choisissez Attach.
1. Connectez-vous à votre instance et montez le volume.
* Pour plus d'informations, voir [Rendre un volume Amazon EBS disponible pour utilisation sous Linux] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html).

### Exécuter des processus de police légale
* `Quels résultats médico-légaux recherche-t-elle l'entreprise ? `
* `Que sera inclus dans le rapport final ? `
* `À qui s'adresse le rapport ? `
* `Existe-t-il des exigences légales ou réglementaires identifiant les informations requises dans le cadre d'une enquête médico-légale ? `
* `Le personnel de l'entreprise est-il correctement formaté/agréé/certifié pour effectuer des études médico-légales ? `
* `Existe-t-il des exigences de conservation pour les journaux, les données ou les preuves ? Pensez potentiellement à Glacier Vaults pour prendre en charge »
* Un exemple de [Digital Forensic Analysis of Amazon Linux EC2 Instances] (https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235) est disponible auprès de l'institut SANS.

## Confinement
### Isolez immédiatement les ressources affectées
**REMARQUE** : Assurez-vous que vous disposez d'un processus en place pour demander une escalade et une approbation pour isoler les ressources afin de vous assurer qu'une analyse d'impact sur l'activité est effectuée en premier lieu sur l'impact de l'isolement sur les opérations et les flux de revenus actuels.

### Désaffectation de l'instance
Activer les protections de résiliation
* [Désactiver la fin de l'instance (via console/API/CLI)] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
1. Définir le comportement d'arrêt de l'instance sur Arrêter (ou Terminer)
1. Désactiver « DeleteOnTermination » pour tous les volumes d'instance
* [Supprimer (détacher) du groupe Auto Scaling] (https://docs.aws.amazon.com/autoscaling/ec2/userguide/detach-instance-asg.html)
1. Ouvrez la console Amazon EC2 Auto Scaling à l'adresse https://console.aws.amazon.com/ec2autoscaling/
1. Activez la case à cocher en regard de votre groupe Auto Scaling.
1. Un volet fractionné s'ouvre dans la partie inférieure de la page Groupes Auto Scaling, affichant des informations sur le groupe sélectionné.
1. Dans l'onglet Gestion des instances, dans Instances, sélectionnez une instance et choisissez Actions, Détacher.
1. Dans la boîte de dialogue Détacher une instance, activez la case à cocher pour lancer une instance de remplacement, ou laissez-la non cochée pour décrémenter la capacité souhaitée. Choisissez Détacher une instance.
* [Désenregistrement du ou des équilibreurs de charge] (https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)
1. Ouvrez la console Amazon EC2 à l'adresse https://console.aws.amazon.com/ec2/
1. Dans le volet de navigation, sous LOAD BALANCING, choisissez Load Balancers.
1. Sélectionnez votre équilibreur de charge.
1. Dans le volet inférieur, sélectionnez l'onglet Instances.
1. Choisissez Modifier les instances.
1. Sélectionnez l'instance à enregistrer auprès de votre équilibreur de charge.
1. Choisissez Enregistrer.
1. Créez un groupe de sécurité *nouveau* qui bloque tout le trafic d'entrée et de sortie. Assurez-vous de supprimer la règle par défaut « autoriser tout » pour le trafic de sortie
* Joindre le groupe de sécurité *nouveau* aux instances touchées

## Éradication
Il n'y a pas d'étapes pour ce processus applicable à ce runbook.

## Récupération
À la fin des activités médico-légales et à la suite de la publication du rapport final, vous devez mettre fin à l'instance EC2 médico-légale et supprimer l'instantané EBS **À MOINS qu'il n'y ait une obligation légale ou réglementaire de conserver les données.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas nécessairement besoin de « réparation », mais qui sont importants à savoir lors de l'exécution de ce livret de jeu en même temps que les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant qu'intervenant en cas d'incident, j'ai besoin d'un runbook pour effectuer EC2 Forensics
- En tant qu'intervenant en cas d'incident, j'ai besoin d'une décision commerciale quant au moment où la police judiciaire EC2 devrait être effectuée.
- En tant que répondeur d'incident, je dois activer la journalisation dans toutes les régions qui sont activées indépendamment de l'intention d'utilisation
- En tant qu'intervenant en cas d'incident, je dois m'assurer que seules les AMI approuvées sont utilisées.
- En tant que répondeur d'incident, je dois pouvoir détecter le crypto mining sur mes instances EC2 existantes
- En tant que répondeur d'incident, j'ai besoin d'une méthode préparée pour supprimer un nombre massif d'instances

## Articles en arriéré actuel