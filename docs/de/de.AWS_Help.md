# Sicherheits-Playbook für die Reaktion auf Amazon Bedrock-Sicherheitsereignisse
Dieses Dokument dient nur zu Informationszwecken. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Zeitpunkt der Veröffentlichung dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Kunden sind dafür verantwortlich, die Informationen in diesem Dokument und die Nutzung von AWS-Produkten oder -Services, die jeweils „wie sie sind“ und ohne jegliche ausdrückliche oder stillschweigende Garantie bereitgestellt werden, selbst zu beurteilen. Dieses Dokument stellt keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern dar. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Verträge geregelt, und dieses Dokument ist weder Teil einer Vereinbarung zwischen AWS und seinen Kunden noch ändert es diese.

© 2024 Amazon Web Services, Inc. oder seine Tochtergesellschaften. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors`\
Genehmiger: `Name des Genehmigers`\
Letztes Genehmigungsdatum:

## Zusammenfassung

Im Rahmen unseres kontinuierlichen Engagements für Kunden stellt AWS dies zur Verfügung
Playbook zur Reaktion auf Sicherheitsvorfälle, in dem die Schritte beschrieben werden, die erforderlich sind, um Unterstützung beim AWS-Support bei Sicherheitsereignissen anzufordern.

! [Bild] (/images/nist_life_cycle.png)

*Aspekte der AWS-Reaktion auf Incidents*
## Vorbereitung

Um Maßnahmen zur Reaktion auf Vorfälle schnell und effektiv durchführen zu können, ist es von entscheidender Bedeutung, die Mitarbeiter, Prozesse und Technologien in Ihrem Unternehmen darauf vorzubereiten. Lesen Sie den [*AWS Security Incident Response Guide*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html) und implementieren Sie die erforderlichen Schritte, um sicherzustellen, dass alle drei Domänen darauf vorbereitet sind.

## So wenden Sie sich an den AWS-Support, um Unterstützung zu erhalten

Es ist wichtig, dass Sie AWS benachrichtigen, sobald Sie vermuten, dass Ihre Anmeldeinformationen in Ihrem AWS-Konto oder Ihrer Organisation kompromittiert wurden. Gehen Sie wie folgt vor, um den AWS-Support in Anspruch zu nehmen:

### Einen AWS-Supportfall öffnen

- Melden Sie sich bei Ihrem AWS-Konto an:
- Dies ist das erste AWS-Konto, das von dem Sicherheitsereignis betroffen war, um die Inhaberschaft eines AWS-Kontos zu bestätigen.
- Hinweis: Wenn Sie nicht auf das Konto zugreifen können, verwenden Sie [dieses Formular] (https://support.aws.amazon.com/#/contacts/aws-account-support/), um eine Support-Anfrage einzureichen.
- Wählen Sie „*Services*“, „*Support Center*“, „*Fall erstellen*“ aus.
- Wählen Sie den Problemtyp „*Konto und Abrechnung“ *.
- Wählen Sie den betroffenen Service und die betroffene Kategorie aus:
- Beispiel:
- Service: Konto
- Kategorie: Sicherheit
- Wählen Sie einen Schweregrad:
- Enterprise Support- oder On-Ramp-Kunden: *Frage zu kritischem Geschäftsrisiko*.
- Business-Support-Kunden: *Dringende Frage zum Geschäftsrisiko*.
- Kunden mit Basic Support und Developer Support: *Allgemeine Frage*.
— Weitere Informationen finden Sie unter [AWS Support-Pläne vergleichen] (https://aws.amazon.com/premiumsupport/plans/).
- Beschreiben Sie Ihre Frage oder Ihr Problem:
- Beschreiben Sie ausführlich das aufgetretene Sicherheitsproblem, die betroffenen Ressourcen und die Auswirkungen auf Ihr Unternehmen.
— Zeitpunkt, zu dem das Sicherheitsereignis zum ersten Mal erkannt wurde.
— Zusammenfassung des Zeitplans des Ereignisses bis zu diesem Zeitpunkt.
- Artefakte, die du gesammelt hast (Screenshots, Protokolldateien).
— ARN von IAM-Benutzern oder -Rollen, von denen Sie vermuten, dass sie gefährdet sind.
- Beschreibung der betroffenen Ressourcen und der Auswirkungen auf das Geschäft.
- Grad des Engagements in Ihrem Unternehmen (z. B.: „Dieses Sicherheitsereignis ist für den CEO und den Vorstand sichtbar“).
- Optional: Fügen Sie weitere Ansprechpartner für den Fall hinzu.
- Wählen Sie „*Kontaktieren Sie uns*“.
- Bevorzugte Kontaktsprache (möglicherweise je nach Verfügbarkeit)
- Bevorzugte Kontaktmethode: Internet, Telefon oder Chat (empfohlen)
- Optional: Zusätzliche Ansprechpartner für den Fall
- *Klicken Sie auf „Senden“ *

- **Eskalationen**: Bitte benachrichtigen Sie Ihr AWS-Kundenbetreuungsteam so schnell wie möglich, damit es die erforderlichen Ressourcen einsetzen und bei Bedarf eskalieren kann.

**Hinweis: ** Es ist sehr wichtig, dass Sie überprüfen, ob Ihr „Alternativer Sicherheitskontakt“ für jedes AWS-Konto definiert ist. Weitere Informationen finden Sie unter [
Artikel] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).