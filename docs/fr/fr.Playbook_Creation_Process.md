# Playbook : Processus de création de Playbook/Runbook
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date d'émission de ce document, qui peuvent être modifiées sans préavis. Les clients sont responsables de faire leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « en l'état » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses sociétés affiliées, de ses fournisseurs ou de ses concédants de licence. Les responsabilités et responsabilités d'AWS envers ses clients sont contrôlées par des accords AWS, et ce document ne fait pas partie ni ne modifie un accord entre AWS et ses clients.

© 2021 Amazon Web Services, Inc. ou ses sociétés affiliées. Tous droits réservés. Cette œuvre est sous licence Creative Commons Attribution 4.0 International License.

Ce contenu AWS est fourni sous réserve des termes de l'accord client AWS disponible à l'adresse http://aws.amazon.com/agreement ou d'un autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL ou les deux.

# POINTS DE CONTACT

Auteur : `Nom de l'auteur \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation : 25/03/2021

# Phase NIST 800-61r2

Préparation

## Résumé exécutif
Ce playbook décrit le processus de création d'un nouveau playbook/runbook, de la transition du document de DRAFT à APPROVED et des exigences de revalidation à l'aide de GitLab.

Pour une création de playbook plus avancée, veuillez consulter [AWS Incident Response Playbooks Workshop] (https://gitlab.aws.dev/fredski/aws-incident-response-playbooks-workshop/-/blob/main/playbooks/crypto_mining/EC2_crypto_mining.md)

## Conseils

> Il est important de maintenir nos runbooks pour plonger en profondeur et fournir rapidement des résultats aux clients. Par Matthew Helmke « Les Runbooks nous aident dans le cadre d'un ensemble plus large de pratiques, toutes conçues pour renforcer la fiabilité. Garder les runbooks à jour est un élément essentiel de l'ingénierie de la fiabilité du site. »
(< https://www.gremlin.com/blog/ensuring-runbooks-are-up-to-date/ >)

Notre équipe utilise Github pour gérer la documentation. Regardez [Références] (.. /Playbook_Creation_Process.md/ #references) pour voir quelques étapes abrégées de notre flux de processus.

## Nouveau Playbook/Runbook

Procédure de l'GUI ###

* Créer un problème pour le suivi des processus
* Créer une nouvelle branche source dans laquelle travailler
* Accédez à la racine de votre branche
* Sélectionnez le bouton « Web IDE »
* Pour créer un élément, mettez en surbrillance le dossier de marge gauche et sélectionnez la flèche déroulante pour afficher les options.
* Sélectionnez « Nouveau fichier »
* Remarque : Il peut être utile de copier le contenu d'un playbook existant pour le formater
* Nos documents utilisent [GitHub Flavored Markdown] (https://github.github.com/gfm/)

* Assurez-vous de respecter la convention de dénomination standard pour notre bibliothèque
* Playbooks : `# Playbook : Playbook/Runbook`
* Guides d'outils : `# Outillage : ToolName`
* Il s'agit de la seule fois où Header 1 (H1) est utilisé dans le document

* Créer un lien vers le problème associé
* Exemple : `Problème associé : (. /problèmes/1) `

* Une fois que vous avez terminé votre document :
* Ajoutez un lien vers votre document dans le numéro sous forme de commentaire
* Ajouter l'étiquette « REVIEW » dans le numéro
* Publiez un message contenant un lien vers votre problème dans la « méthode de correspondance client » et demandez un avis ou un commentaire.

* Pour qu'un Playbook soit approuvé, au moins deux (2) membres de l'équipe doivent avoir examiné et ajouté leur nom et un commentaire dans le numéro associé.

* Mettre à jour et mettre en œuvre toutes les recommandations ou corrections apportées au document

* Une fois que votre document a été examiné et que toutes les corrections ont été apportées, soumettez votre document pour que le processus d'approbation soit finalisé.

* Entrez dans le problème et remplacez le cessionnaire en approbateur
* Parcourez le problème
* Ajoutez l'étiquette SOUMISE POUR APPROBATION à votre problème
* Dans la marge de droite à côté de « Attributaire », sélectionnez « Modifier »
* Changer le destinataire en « approbateur »
* Envoyez une note à l'approbateur pour lui faire savoir que le document est prêt pour examen final
* Après approbation, ajoutez un lien vers le document/le playbook dans le fichier Lisez-moi
* Exemple : `[Processus de création de playbook] (. /docs/Playbook_Creation_Process.md) `
* Soumettre une demande de fusion

* Remarque : N'oubliez pas que toutes les étapes antérieures à la demande de fusion DOIVENT être effectuées au sein de votre branche de travail

* Le SLA de cette tâche est de 7 jours calendaires à compter de la soumission du document.
* Si aucune réponse n'a été reçue au cours de la période SLA pour approbation, augmentez en envoyant un carillon à `utilisateur A`, `utilisateur B`, `utilisateur C `.

### Procédure CLI

À DÉTERMINER

## Mises à jour Playbook/Runbook

### Procédure

* Créer un problème pour le suivi des processus
* Créer une nouvelle branche source dans laquelle travailler
* Accédez à la racine de votre branche
* Sélectionnez le bouton « Web IDE »
* Ouvrez un nouveau numéro pour suivre la progression de vos modifications
* Supprimer toutes les étiquettes existantes
* Ajouter l'étiquette DRAFT
* Créer un lien vers le problème associé
* Vous pouvez ajouter vos problèmes à la liste
* Exemple : (numéro 1), (numéro 1), (numéro 3)

* Une fois que vous avez terminé votre document :
* Ajoutez un lien vers votre document dans le numéro sous forme de commentaire
* Ajouter l'étiquette « REVIEW » dans le numéro
* Publiez un message contenant un lien vers votre problème dans le « mécanisme de correspondance client » et demandez un avis ou un commentaire.

* Pour qu'un Playbook soit approuvé, au moins deux (2) membres de l'équipe doivent avoir examiné et ajouté leur nom et un commentaire dans le numéro associé.

* Mettre à jour et mettre en œuvre toutes les recommandations ou corrections apportées au document

* Une fois que votre document a été examiné et que toutes les corrections ont été apportées, soumettez votre document pour que le processus d'approbation soit finalisé.

* Entrez dans le problème et remplacez le cessionnaire en approbateur
* Parcourez le problème
* Ajoutez l'étiquette SOUMISE POUR APPROBATION à votre problème
* Dans la marge de droite à côté de « Attributaire », sélectionnez « Modifier »
* Changer le destinataire en « approbateur »
* Envoyez une note dans Chime à l' « approbateur » pour leur faire savoir que le document est prêt pour examen final
* Après approbation, ajoutez un lien vers le document/le playbook dans le fichier Lisez-moi
* Exemple : `[Processus de création de playbook] (. /docs/Playbook_Creation_Process.md) `
* Soumettre une demande de fusion

* Remarque : N'oubliez pas que toutes les étapes antérieures à la demande de fusion DOIVENT être effectuées au sein de votre branche de travail

* Le SLA de cette tâche est de 7 jours calendaires à compter de la soumission du document.
* Si aucune réponse n'a été reçue pendant la période SLA pour approbation, augmentez en envoyant une note à `utilisateur A`, `utilisateur B`, `utilisateur C `

### Procédure CLI

À DÉTERMINER

## Processus d'approbation

### Propriétaire de la tâche :

Propriétaires d'équipe

### Accord de niveau de service

- 7 jours civils à compter de la soumission de l'auteur

### Procédure

* Consultez le document et le problème associé dans GitHub

* Supprimer REVIEW du titre du document

* Supprimer l'instruction THIS IS A DRAFT PLAYBOOK du document

* Sous les points de contact
- Ajouter/mettre à jour votre nom à l'approbateur
- Ajouter/mettre à jour « Dernière date approuvée » en tant que AAAA/MM/JJ pour le moment où vous approuvez le document

* Réattribuer le problème au demandeur
* Ajouter un commentaire dans le problème selon lequel le playbook est approuvé et pour procéder à une demande de fusion

## Références

### Utilisation de GitHub : Créer un problème

Nous utilisons les problèmes pour suivre les travaux en cours et terminés. De plus, cela aide l'équipe à passer par notre processus de révision et d'approbation. Cela permet également aux commentaires des réviseurs de traiter tout ce qui concerne les objets du référentiel dans le cadre de notre processus de levage de barres de documentation.

* Interface utilisateur graphique : l'GUI vous permet d'interagir avec GitHub à partir d'un navigateur Web pour interagir avec ce référentiel
1. Accédez à votre référentiel GitHub à l'aide de votre navigateur Web préféré
1. Ouvrez un problème en sélectionnant Problèmes dans le menu supérieur.
1. Sélectionnez « Nouveau problème »
1. Ajoutez un titre et une description de ce sur quoi vous travaillez
1. Assignez-vous à vous-même (cela permettra le suivi dans les étapes ultérieures)
1. Pour Milestone, sélectionnez « Jalon client ici »
1. Pour les étiquettes, sélectionnez celles qui conviennent à votre problème.
* Pour les nouveaux documents, veillez à sélectionner BROUILLON
1. Sélectionnez « Soumettre le problème »
1. Sur la nouvelle page Problème, dans la marge de droite Problème de verrouillage > Modifier > Verrouiller

* Interface de ligne de commande : l'CLI de ligne de commande vous permet d'interagir avec GitHub depuis la ligne de commande

### Utilisation de GitHub : créer une branche dans laquelle travailler

Nous travaillons dans des branches individuelles, de sorte que les travaux en cours n'interfèrent pas avec ce que les autres peuvent faire en temps réel.

* Interface utilisateur graphique : l'GUI vous permet d'interagir avec [GitHub depuis un navigateur Web pour interagir avec ce référentiel] (https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)

* Interface de ligne de commande : l'CLI de ligne de commande vous permet d'interagir avec GitHub depuis la ligne de commande

### Utilisation de GitHub : Créer une demande de fusion

Les demandes de fusion autorisent une révision secondaire du relèvement de barres avant de fusionner avec le référentiel parent.

* [Interface utilisateur graphique] (https://docs.github.com/en/github/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges)

* Interface de ligne de commande : l'CLI de ligne de commande vous permet d'interagir avec GitHub depuis la ligne de commande

## Articles en arriéré