# Rationalisation des alertes d'incidents de sécurité
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2024 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

> « Si vous protégez vos trombones et vos diamants avec la même vigueur, vous aurez bientôt plus de trombones et moins de diamants. » attribué à l'ancien secrétaire d'État américain Dean Rusk

Les alertes sont déterminées par la modélisation des menaces d'une charge de travail lors du développement de contrôles de sécurité. L'utilisation des alertes est définie par les contrôles Responsive, mais sa définition repose sur l'ensemble de la perspective de sécurité : Directive, Préventive, Détective et Responsive.

## Définitions

### Modélisation des menaces :

* **Charge de travail** : ce que vous essayez de protéger
* **Menace** : ce que vous craignez de se produire
* **Impact** : comment l'entreprise est affectée lorsque la menace rencontre la charge de travail
* **Vulnérabilité** : ce qui peut faciliter la menace
* **Atténuation** : quels contrôles sont en place pour compenser la vulnérabilité
* **Probabilité** : quelle est la probabilité que la menace se produise avec des mesures d'atténuation en place

**La hiérarchisation des menaces est un facteur d'impact et de probabilité. **

### Contrôles de sécurité :

* **Les contrôles directif** établissent les modèles de gouvernance, de risque et de conformité dans lesquels l'environnement fonctionnera.
* **Les contrôles préventif** protègent vos charges de travail et atténuent les menaces et les vulnérabilités.
* Les contrôles **Detective** offrent une visibilité et une transparence complètes sur le fonctionnement de vos déploiements dans AWS.
* **Responsive** contrôle la correction des écarts potentiels par rapport à vos lignes de base de sécurité.


! [Image] (/images/image-caf-sec.png)

## Pour minimiser la fatigue des alertes et améliorer la gestion des événements de sécurité, le contexte de modélisation des menaces doit prendre en compte :

* Pertinence de la charge de travail (*) pour l'entreprise.
* Classification des données (*) de chaque composant de charge de travail.
* Méthodologie systématiquement utilisée comme STRIDE (usurpation, falsification, divulgation d'informations, répudiation, déni de service et élévation de privilège)
* Préparation et maturité de la sécurité dans le cloud.
* Capacité de réponse aux incidents et de chasse aux menaces.
* Capacités de correction automatique.
* Maturité de l'automatisation du traitement des incidents


(*) ***La pertinence de la charge de travail et la classification des données*** sont définies par l'initiative du cadre de risque de la société. Exemples :

### Pertinence de la charge de travail :

**Élevé** : perte monétaire majeure et dommages à la perception de l'image, impact sur l'entreprise à long terme, faible succès de récupération
**Moyen** : perte monétaire durable et dommages à la perception de l'image, impact commercial à court terme, succès de reprise élevé
**Faible** : aucune perte monétaire mesurable et aucun dommage à la perception de l'image, aucun impact commercial, la reprise n'est pas applicable

### Classification des données :

**Secret** : perte monétaire majeure et dommages à la perception de l'image, impact sur l'entreprise à long terme, faible succès de récupération
**Confidentiel** : perte monétaire durable et dommages à la perception de l'image, impact commercial à court terme, succès de récupération élevé
**Non classifié** : aucune perte monétaire mesurable et aucun dommage à la perception de l'image, aucun impact commercial, la récupération n'est pas applicable


## L'objectif de la priorisation des alertes est de les envoyer dans la file d'attente appropriée :

* File d'attente de tri des alertes
* File d'attente de chasse aux menaces
* File d'attente d'archives


Le processus de définition de l'emplacement de l'alerte dépend de tous les facteurs énumérés précédemment. Une décision de base de mise en file d'attente est la suivante :

### File d'attente de triage des alertes :

1. Mandat spécifique du cadre de risque, y compris, mais sans s'y limiter, la réglementation de l'industrie, la conformité réglementaire et les exigences commerciales. Dans ce scénario, la charge de travail a une pertinence **élevée** ou les données sont classées comme **secrèt**.
2. Exigences spécifiques du modèle de menace pour lancer un processus formel de réponse aux incidents.
3. La correction automatique a échoué pour les alertes où la charge de travail a une pertinence **moyenne** ou **élevée** ou la classification des données est **secrète** ou **confidentiel**.

### **File d'attente de chasse aux menaces** :

1. La correction automatique réussie pour une charge de travail avec une pertinence **moyenne** ou **élevée** ou la classification des données est **secrèt** ou **confidentiel**.
2. La correction automatique a échoué pour la charge de travail avec une pertinence **faible** ou la classification des données est **non classifiée**.

### File d'archivage :

1. La correction automatique a réussi pour la charge de travail avec une pertinence **faible** ou la classification des données est **non classifiée**.