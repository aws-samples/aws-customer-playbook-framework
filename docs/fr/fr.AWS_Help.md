# Guide de sécurité pour répondre aux événements de sécurité d'Amazon Bedrock
Ce document est fourni à titre informatif uniquement. Il représente les offres de produits et les pratiques actuelles d'Amazon Web Services (AWS) à la date de publication de ce document, qui sont susceptibles d'être modifiées sans préavis. Les clients sont tenus de procéder à leur propre évaluation indépendante des informations contenues dans ce document et de toute utilisation des produits ou services AWS, chacun étant fourni « tel quel » sans garantie d'aucune sorte, expresse ou implicite. Ce document ne crée aucune garantie, représentation, engagement contractuel, condition ou assurance de la part d'AWS, de ses filiales, fournisseurs ou concédants de licence. Les responsabilités et obligations d'AWS à l'égard de ses clients sont régies par les accords AWS, et le présent document ne fait partie d'aucun accord conclu entre AWS et ses clients et ne les modifie pas.

© 2024 Amazon Web Services, Inc. ou ses filiales. Tous droits réservés. Ce travail est mis à disposition selon les termes de la licence internationale Creative Commons Attribution 4.0.

Ce contenu AWS est fourni conformément aux termes du contrat client AWS disponible à l'adresse http://aws.amazon.com/agreement ou à tout autre accord écrit entre le client et Amazon Web Services, Inc. ou Amazon Web Services EMEA SARL, ou les deux.

## Points de contact

Auteur : `Nom de l'auteur` \
Approbateur : `Nom de l'approbateur` \
Dernière date d'approbation :

## Résumé

Dans le cadre de notre engagement continu envers les clients, AWS fournit ce
manuel de réponse aux incidents de sécurité qui décrit les étapes nécessaires pour demander l'assistance d'AWS Support pour des événements de sécurité.

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
- Clients du support de base et du support aux développeurs : *Question générale*.
- Pour plus d'informations, vous pouvez [comparer les offres de support AWS] (https://aws.amazon.com/premiumsupport/plans/).
- Décrivez votre question ou problème :
- Fournissez une description détaillée du problème de sécurité que vous rencontrez, des ressources concernées et de l'impact sur l'entreprise.
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

**Remarque :** Il est très important de vérifier que votre « contact de sécurité alternatif » est défini pour chaque compte AWS. Pour plus de détails, veuillez vous référer à [ce
article] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).