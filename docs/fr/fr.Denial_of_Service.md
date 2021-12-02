# Playbook de réponse aux incidents : déni de service /déni de service distribué (DOS/DDoS)
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

## Points de contact

Auteur : `Nom de l'auteur \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé exécutif
Ce manuel décrit le processus de réponse aux attaques par déni de service ou par déni de service distribué (DOS/DDoS) contre des ressources hébergées AWS.

## Indicateurs potentiels de compromis
- Une application orientée sur Internet est soumise à une charge importante, soit en raison de sa popularité, soit d'une intention malveillante
- Une seule instance EC2 est utilisée pour héberger une application et nous réduisons le trafic (le client est averti via AWS Abuse)
- Résultats présentés dans le tableau de bord AWS Shield

## Constatations potentielles d'AWS GuardDuty
- Backdoor:EC2/DenialOfService.Dns
- Backdoor:EC2/DenialOfService.Tcp
- Backdoor:EC2/DenialOfService.Udp
- Backdoor:EC2/DenialOfService.UdpOnTcpPorts
- Backdoor:EC2/DenialOfService.UnusualProtocol
- Backdoor:EC2/Spambot
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual

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
1. [**PREPARATION**] Créer un inventaire d'actifs publiquement exposé
2. [**PREPARATION**] Mettre en œuvre une formation pour traiter les attaques DOS/DDoS
3. [**PREPARATION**] Élaborer une stratégie de communication pour escalader et signaler les événements
4. [**PREPARATION**] Effectuer des examens de documentation pour s'assurer que les procédures sont maintenues et à jour
5. [**DÉTECTION ET ANALYSIS**] Implémenter AWS Shield
6. [**DÉTECTION ET ANALYSIS**] Utiliser le tableau de Global Threat Environment Dashboard avec AWS Shield
7. [**DÉTECTION ET ANALYSE**] Envisagez d'utiliser AWS Shield Advanced
8. [**PREPARATION**] Mettre en œuvre des procédures d'escalade et de notification
9. [**DÉTECTION ET ANALYS**] Effectuer la détection et l'atténuation
10. [**DÉTECTION ET ANALYS**] Identifier les principaux contributeurs
11. [**CONFINEMENT**] Effectuer le confinement
12. [**ÉRADICATION**] Effectuer l'éradication
13. [**ACTIVITÉS POST-INCIDENTS**] Demander un crédit dans AWS Shield Advanced
14. [**PREPARATION**] Actions préventives supplémentaires : techniques d'atténuation
15. [**PREPARATION**] Actions préventives supplémentaires : réduction de la surface d'attaque
16. [**PREPARATION**] Actions préventives supplémentaires : techniques opérationnelles
17. [**PREPARATION**] Actions préventives supplémentaires : posture générale de sécurité

***Les étapes de réponse suivent le cycle de vie de réponse aux incidents du [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Image] (/images/nist_life_cycle.png) ***

### Classification et manipulation des incidents

* **Tactiques, techniques et procédures** : attaques par déni de service /attaques par déni de service distribué
* **Catégorie** : DoS/DDoS
* **Ressource** : Réseau
* **Indicateurs** : Cyber Threat Intelligence, avis de tiers, mesures Cloudwatch, AWS Shield
* **Sources de journaux : AWS CloudTrail, AWS Config, VPC Flow Logs, Amazon GuardDuty
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

Cet outil fournit un instantané rapide de l'état actuel de la sécurité dans un environnement client. En outre, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) permet une analyse automatisée de la conformité et peut [s'intégrer à Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventaire des actifs publiquement exposés
Trouvez des ressources exposées à Internet : `. /rôdeur -g groupe 17`

Vous pouvez utiliser Security Hub et Firewall Manager pour [configurer la surveillance centralisée des événements DDoS et corriger automatiquement les ressources non conformes] (https://aws.amazon.com/blogs/security/set-up-centralized-monitoring-for-ddos-events-and-auto-remediate-noncompliant-resources/)

### Formation
- `Quelle est la formation mise en place pour que les analystes de l'entreprise se familiarisent avec l'AWS API (environnement de ligne de commande), S3, RDS et d'autres services AWS ? `
>>>
Voici les possibilités de détection des menaces et de réponse aux incidents : \
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/) \
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Quels rôles peuvent apporter des modifications aux services de votre compte ? `
- `Quels utilisateurs ces rôles leur sont attribués ? Le moindre privilège est-il respecté ou existe-t-il des utilisateurs de super administrateurs ? `
- `Une évaluation de sécurité a-t-elle été effectuée sur votre environnement, avez-vous une base de référence connue pour détecter des choses « nouvelles » ou « suspectes » ? `

### Technologie de communication
- `Quelle technologie est utilisée au sein de l'équipe/de l'entreprise pour communiquer les problèmes ? Y a-t-il quelque chose d'automatisé ? `
>>>
Téléphone \
E-mail \
AWS SES \
AWS SNS \
Slack \
Carillon \
Autre ?
>>>

### Examen de la documentation
1. Familiarisez-vous avec la [documentation AWS Shield] (https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
1. Si vous êtes abonné, suivez le guide [Premiers pas avec AWS Shield Advanced guide] (https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html)

## Détection
### Mesures Cloudwatch pour détecter les événements DOS/DDoS
Le tableau Mesures Amazon CloudWatch recommandées répertorie les descriptions des mesures Amazon CloudWatch couramment utilisées pour détecter et réagir aux attaques DDoS.

* AWS Shield Advanced : DDoSDetected
* Indique un événement DDoS pour un Amazon Resource Name (ARN) spécifique.
* AWS Shield Advanced : DDoSAttackBitsPerSecond
* Le nombre d'octets observés lors d'un événement DDoS pour un ARN spécifique. Cette mesure est uniquement disponible pour les événements DDoS de couche 3/4.
* AWS Shield Advanced : DDoSAttackPacketsPerSecond
* Le nombre de paquets observés lors d'un événement DDoS pour un ARN spécifique. Cette mesure est uniquement disponible pour les événements DDoS de couche 3/4.
* AWS Shield Advanced : DDoSAttackRequestsPerSecond
* Le nombre de demandes observées lors d'un événement DDoS pour un ARN spécifique. Cette mesure n'est disponible que pour les événements DDoS de couche 7 et n'est signalée que pour les événements de couche 7 les plus importants.
* AWS WAF : AllowedRequests
* Le nombre de demandes Web autorisées.
* AWS WAF : BlockedRequests
* Le nombre de demandes Web bloquées.
* AWS WAF : CountedRequests
* Le nombre de demandes Web comptées.
* AWS WAF : PassedRequests
* Le nombre de demandes passées. Il est uniquement utilisé pour les demandes qui passent par une évaluation de groupe de règles sans correspondre à aucune des règles de groupe de règles.
* CloudFront : Requests
* Le nombre de demandes HTTP/S.
* CloudFront : TotalErrorRate
* Le pourcentage de toutes les demandes pour lesquelles le code d'état HTTP est 4xx ou 5xx.
* Amazon Route 53 : HealthCheckStatus
* L'état du point de terminaison du contrôle de santé.
* ALB : ActiveConnectionCount
* Le nombre total de connexions TCP simultanées actives depuis les clients vers l'équilibreur de charge, et de l'équilibreur de charge aux cibles.
* ALB : ConsumedLCUs
* Le nombre d'unités de capacité d'équilibrage de charge (LCU) utilisées par votre équilibreur de charge.
* ALB : HTTPCode_ELB_4XX_Count HTTPCode_ELB_5XX_Count
* Le nombre de codes d'erreur client HTTP 4xx ou 5xx générés par l'équilibreur de charge.
* ALB : NewConnectionCount
* Le nombre total de nouvelles connexions TCP établies entre les clients et l'équilibreur de charge, et de l'équilibreur de charge aux cibles.
* ALB : ProcessedBytes
* Le nombre total d'octets traités par l'équilibreur de charge.
* ALB : RejectedConnectionCount
* Le nombre de connexions rejetées parce que l'équilibreur de charge a atteint son nombre maximal de connexions.
* ALB : RequestCount
* Le nombre de demandes traitées.
* ALB : TargetConnectionErrorCount
* Le nombre de connexions qui n'ont pas été correctement établies entre l'équilibreur de charge et la cible.
* ALB : TargetResponseTime
* Temps écoulé, en secondes, après que la demande quitte l'équilibreur de charge jusqu'à ce qu'une réponse de la cible soit reçue.
* ALB : UnHealthyHostCount
* Le nombre de cibles considérées comme malsaines.
* NLB : ActiveFlowCount
* Le nombre total de flux TCP simultanés (ou connexions) entre les clients et les cibles.
* NLB : ConsumedLCUs
* Le nombre d'unités de capacité d'équilibrage de charge (LCU) utilisées par votre équilibreur de charge.
* NLB : NewFlowCount
* Le nombre total de nouveaux flux (ou connexions) TCP établis entre les clients et les cibles au cours de la période.
* NLB : ProcessedBytes
* Le nombre total d'octets traités par l'équilibreur de charge, y compris les en-têtes TCP/IP.
* Global Accelerator NewFlowCount
* Le nombre total de nouveaux flux TCP et UDP (ou connexions) établis entre les clients et les points de terminaison au cours de la période.
* Global Accelerator : ProcessedBytesIn
* Le nombre total d'octets entrants traités par l'accélérateur, y compris les en-têtes TCP/IP.
* Auto Scaling : GroupMaxSize
* La taille maximale du groupe Auto Scaling.
* Amazon EC2 : CPUUtilization
* Pourcentage d'unités de calcul EC2 allouées actuellement utilisées.
* Amazon EC2 : NetworkIn
* Le nombre d'octets reçus par l'instance sur toutes les interfaces réseau.
### AWS Shield
La [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/shieldv2) ou l'API fournissent un résumé et des détails sur les attaques détectées.
1. Connectez-vous à AWS Management Console et ouvrez la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. Dans la barre de navigation AWS Shield, choisissez Overview ou Events

### Tableau de Global Threat Environment Dashboard
Le tableau de Global Threat Environment Dashboard fournit des informations récapitulatives sur toutes les attaques DDoS détectées par AWS.
1. Connectez-vous à AWS Management Console et ouvrez la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. Dans la barre de navigation AWS Shield, choisissez Tableau de bord Global Threat.
1. Choisissez une période.

### AWS Shield Advanced
1. Connectez-vous à AWS Management Console et ouvrez la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. Dans la barre de navigation AWS Shield, choisissez Overview ou Events

Vous avez accès à un certain nombre de mesures CloudWatch qui peuvent indiquer que votre application est ciblée.
1. Vous pouvez configurer la mesure DDoSDetected pour vous indiquer si une attaque a été détectée.
1. Si vous souhaitez être alerté en fonction du volume d'attaque, vous pouvez également utiliser les mesures DDoSAttackBitsPerSecond, DDoSAttackPacketsPerSecond ou DDoSAttackRequestsPerSecond.

Une liste complète des mesures disponibles est disponible dans le tableau de la [Visibilité DDoS] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)

## Procédures d'escalade

### AWS Shield
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun d'eux ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Quand souhaitez-vous contacter AWS Support pour obtenir de l'aide ? `

### AWS Shield Advanced
- `Qui surveille les logs/alertes, les reçoit et agit sur chacun d'eux ? `
- `Qui est averti lorsqu'une alerte est découverte ? `
- `Quand les relations publiques et les services juridiques s'impliquent-ils dans le processus ? `
- `Avez-vous spécifié les ressources AWS que vous souhaitez protéger à l'aide de Shield Advanced ? `
1. Connectez-vous à AWS Management Console et ouvrez la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. Dans la barre de navigation AWS Shield, choisissez Ressources protégées
- `Avez-vous autorisé l'équipe de réponse DDoS (DRT) à accéder à vos règles et journaux WAF ? Cela les aide à atténuer les attaques lorsque vous demandez de l'aide. `
1. Connectez-vous à AWS Management Console et ouvrez la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. Dans la barre de navigation AWS Shield, choisissez Overview
1. Configurer la prise en charge AWS DRT `Modifier l'accès DRT
- `Avez-vous besoin d'une assistance de la part de l'équipe de réponse AWS DDoS ? `
1. Vous pouvez ouvrir un dossier sous AWS Shield dans le centre de support AWS
* Pour bénéficier de la prise en charge de l'équipe de réponse DDoS (DRT), contactez le Centre de support AWS et sélectionnez les options suivantes :
1. Type de boîtier : Support technique
1. Service : Déni de service distribué (DDoS)
1. Catégorie : Entrante vers AWS
* Avec l'engagement proactif AWS Shield Advanced, le DRT vous contacte directement si le bilan de santé Amazon Route 53 associé à votre ressource protégée devient malsain lors d'un événement détecté par Shield Advanced.

## Analyse
1. Connectez-vous à AWS Management Console et ouvrez la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. Dans la barre de navigation AWS Shield, choisissez Events

### Détection et atténuation
L'onglet Détection et atténuation affiche des graphiques montrant le trafic évalué par AWS Shield avant de signaler cet événement et l'effet de toute atténuation placée. Il fournit également des mesures pour les événements de déni de service distribué (DDoS) de la couche d'infrastructure pour les ressources de vos propres accélérateurs Amazon Virtual Private Cloud VPC et AWS Global Accelerator. Les mesures de détection et d'atténuation ne sont pas incluses pour les ressources Amazon CloudFront ou Amazon Route 53.

### Meilleurs contributeurs
L'onglet Principaux contributeurs fournit des informations sur la provenance du trafic lors d'un événement détecté. Vous pouvez afficher les contributeurs de volume les plus élevés, triés par des aspects tels que le protocole, le port source et les indicateurs TCP. Vous pouvez télécharger les informations et ajuster les unités utilisées et le nombre de contributeurs à afficher.

## Confinement

### [Meilleures pratiques AWS pour la résilience DDoS] (https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
Dans ce livre blanc, AWS vous fournit des conseils DDoS normatifs pour améliorer la résilience des applications exécutées sur AWS. Cela inclut une architecture de référence résiliente DDoS qui peut être utilisée comme guide pour protéger la disponibilité des applications. Ce livre blanc décrit également différents types d'attaques, tels que les attaques de couches d'infrastructure et les attaques de couches applicatives. AWS explique quelles sont les meilleures pratiques qui sont les plus efficaces pour gérer chaque type d'attaque. En outre, les services et fonctionnalités qui s'intègrent dans une stratégie d'atténuation DDoS sont décrits et expliquent comment chacun peut être utilisé pour aider à protéger vos applications.
Les procédures de confinement supposent l'utilisation des pare-feu AWS Web Application Firewalls. Si un WAF ou un pare-feu tiers est utilisé, personnalisez ces procédures ici pour correspondre à votre cas d'utilisation.

### AWS WAF
1. Créez des conditions dans AWS WAF qui correspondent à un comportement inhabituel.
1. Ajoutez ces conditions à une ou plusieurs règles AWS WAF.
1. Ajoutez ces règles à une ACL Web et configurez l'ACL Web pour compter les demandes correspondant aux règles.
* **NOTE** Vous devez toujours tester vos règles d'abord en utilisant Count plutôt que Block. Une fois que vous êtes sûr que la nouvelle règle identifie les bonnes demandes, vous pouvez modifier votre règle pour bloquer ces demandes.
1. Surveillez ces dénombrements pour déterminer si la source des demandes doit être bloquée. Si le volume de demandes continue d'être exceptionnellement élevé, modifiez votre ACL Web pour bloquer ces demandes.

## Éradication
Non applicable

## Récupération
### Demander un crédit dans AWS Shield Advanced
Si vous êtes abonné à AWS Shield Advanced et que vous subissez une attaque DDoS qui augmente l'utilisation d'une ressource protégée Shield Advanced, vous pouvez demander un crédit pour les frais liés à l'utilisation accrue dans la mesure où elle n'est pas atténuée par Shield Advanced.

Vous trouverez des informations supplémentaires dans les documents AWS à l'adresse [Demander un crédit dans AWS Shield Advanced] (https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html)

## Actions préventives
### Techniques d'atténuation
Sur AWS, les capacités d'atténuation DDoS sont automatiquement fournies
1. AWS Shield Standard se défend contre les attaques DDoS les plus courantes et fréquentes des couches de transport et de réseau qui ciblent votre site Web ou vos applications. Il est proposé sur tous les services AWS et dans toutes les régions AWS sans frais supplémentaires.
1. Vous pouvez améliorer votre capacité à répondre aux attaques DDoS et à les atténuer en vous abonnant à AWS Shield Advanced. Ce service d'atténuation DDoS en option vous aide à protéger une application hébergée sur n'importe quelle région AWS. Le service est disponible dans le monde entier pour Amazon CloudFront et Amazon Route 53. Il est également disponible dans certaines régions AWS pour Classic Load Balancer (CLB), Application Load Balancer (ALB) et Elastic IP Addresses (EIP). L'utilisation d'AWS Shield Advanced avec EIP vous permet de protéger les instances Network Load Balancer (NLB) ou Amazon EC2.
1. Les principales considérations pour aider à atténuer les attaques DDoS volumétriques incluent la garantie que suffisamment de capacité de transit et de diversité sont disponibles, et la protection de vos ressources AWS, telles que les instances Amazon EC2, contre le trafic d'attaque.
1. Les attaques DDoS volumineuses peuvent surcharger la capacité d'une seule instance Amazon EC2. Par conséquent, l'ajout d'un équilibrage de charge peut améliorer votre résilience.
1. Amazon CloudFront vous permet de mettre en cache du contenu statique et de le diffuser depuis des emplacements périphériques AWS, ce qui peut contribuer à réduire la charge sur votre origine.
1. À l'aide d'AWS WAF, vous pouvez configurer des listes de contrôle d'accès Web (ACL Web) sur vos distributions CloudFront ou Application Load Balancers pour filtrer et bloquer les demandes en fonction des signatures de demandes.
### Réduction de la surface d'attaque
1. Vous pouvez spécifier des groupes de sécurité lorsque vous lancez une instance ou vous pouvez associer l'instance à un groupe de sécurité ultérieurement. Tout le trafic Internet vers un groupe de sécurité est implicitement refusé, sauf si vous créez une règle d'autorisation pour autoriser le trafic.
* Si vous êtes abonné à AWS Shield Advanced, vous pouvez enregistrer des adresses IP Elastic (EIP) en tant que ressources protégées.
1. Si vous utilisez Amazon CloudFront avec une origine située à l'intérieur de votre VPC, vous devez utiliser une fonction AWS Lambda pour mettre automatiquement à jour vos règles de groupe de sécurité afin d'autoriser uniquement le trafic Amazon CloudFront.
1. En règle générale, lorsque vous devez exposer une API au public, il existe un risque que le frontal de l'API soit ciblé par une attaque DDoS. Pour réduire le risque, vous pouvez utiliser Amazon API Gateway comme entrée d'applications exécutées sur Amazon EC2, AWS Lambda ou ailleurs.
* Lorsque vous utilisez Amazon CloudFront et AWS WAF avec Amazon API Gateway, configurez les options suivantes :
1. Configurez le comportement du cache de vos distributions pour transférer tous les en-têtes vers le point de terminaison régional API Gateway. Ce faisant, CloudFront traitera le contenu comme dynamique et ignorera la mise en cache du contenu.
1. Protégez votre API Gateway contre l'accès direct en configurant la distribution pour inclure l'en-tête personnalisé x-api-key d'origine, en définissant la keyvalue API dans API Gateway.
1. Protégez votre backend contre l'excès de trafic en configurant des limites de taux de rafale ou standard pour chaque méthode de votre RESTAPIs.

### Techniques opérationnelles
Lorsqu'une mesure opérationnelle clé s'écarte considérablement de la valeur attendue, un attaquant peut tenter de cibler la disponibilité de votre application. Si vous connaissez le comportement normal de votre application, vous pouvez agir plus rapidement lorsque vous détectez une anomalie.
1. Si vous avez architecturé votre application en suivant l'architecture de référence résiliente aux DDoS, les attaques courantes de couche d'infrastructure seront bloquées avant d'atteindre votre application.
1. Si vous utilisez AWS WAF, vous pouvez utiliser CloudWatch pour surveiller et alerter l'augmentation des demandes que vous avez configurées dans WAF pour être autorisées, comptées ou bloquées. Cela vous permet de recevoir une notification si le niveau de trafic dépasse celui que votre application peut gérer.

Pour obtenir une description des mesures Amazon CloudWatch couramment utilisées pour détecter et réagir aux attaques DDoS, consultez le tableau de la [Visibilité DDoS] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)
### Posture générale de sécurité
Exécutez un [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contre l'environnement afin d'identifier davantage d'autres risques et éventuellement d'autres risques publics non identifiés dans ce playbook.

## Leçons apprises
`Il s'agit d'un endroit où ajouter des éléments spécifiques à votre entreprise qui n'ont pas besoin de « réparation », mais qui sont importants à savoir lorsque vous exécutez ce livre de jeu en tandem avec les exigences opérationnelles et commerciales. `

# Nombre d'articles de carnet de commandes réglés
- En tant que répondeur d'incident, j'ai besoin d'un chemin d'escalade documenté à la fois en interne et en externe vers AWS

## Articles de carnet de commandes actuels