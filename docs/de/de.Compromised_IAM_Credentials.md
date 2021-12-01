# Incident Response Playbook: Kompromittierte IAM-Anmel
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“\
Genehmiger: `Name des Genehmigers“\
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt den zu reagierenden Prozess, wenn Sie nicht autorisierte Aktivitäten in Ihrem AWS-Konto beobachten oder glauben, dass eine nicht autorisierte Partei auf Ihr Konto zugegriffen hat.

## Mögliche Indikatoren für Kompromisse
- Neue oder nicht erkannte IAM-Benutzer
- Unerkannte oder nicht autorisierte Ressourcen (z. B. EC2, Lambda)
- Ungewöhnliche Abrechnungssteigerungen
- Benachrichtigung von Sicherheitsforschern
- Benachrichtigung, dass meine AWS-Ressourcen oder mein Konto möglicherweise gefährdet sind

## Mögliche AWS GuardDuty-Ergebnisse
- CredentialAccess:iamuser/anomalousBehavior
- defenseevasion:iamuser/anomalousBehavior
- Entdeckung:Iamuser/AnomalousBehavior
- Exfiltration:Iamuser/AnomalousBehavior
- Auswirkungen: Iamuser/AnomalousBehavior
- initialAccess:iamuser/anomalousBehavior
- Pentest:Iamuser/Kalilinux
- Pentest:Iamuser/ParrotLinux
- Pentest:Iamuser/Pentoolinux
- Persistenz:Iamuser/AnomalousBehavior
- Policy:iamuser/RootCredentialUsage
- PrivileGeeskalation:Iamuser/AnomalousBehavior
- recon:iamuser/maliciousipCaller
- recon:iamuser/maliciousipCaller.Custom
- recon:iamuser/toripCaller
- stealth:iamUser/CloudTrailLoggingDisabled
- stealth:iamuser/passwordPolicyChange
- UnauthorizedAccess:iamuser/consoleloginSuccess.B
- UnauthorizedAccess:iamuser/InstanceCredentialExFiltration
- UnauthorizedAccess:iamuser/maliciousipCaller
- UnauthorizedAccess:iamuser/maliciousipCaller.Custom
- UnauthorizedAccess:iamuser/toripCaller

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnis***_ und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Ausgenutzte Schwachstellen**
* **Exploits und Tools beobachtet**
* **Absichten des Schauspieler**
* **Zuordnung des Schauspieler**
* **Schaden der Umwelt und dem Unternehmen verursacht**

#### Wiederherstellen:

* **Zurück zur ursprünglichen und gehärteten Konfiguration**

#### Komponenten der CAF-Sicherheitsperspektive verbessern:
[Sicherheitsperspektive des AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Richtlinie**
* **Detektiv**
* **Verantwortlich**
* **Präventiv**

! [Bild] (/images/aws_caf.png)
* *

### Antwortschritte
1. [**VORBEREITUNG**] Führen Sie einen Vermögensbestand aus
2. [**VORBEREITUNG**] Implementieren Sie einen Trainingsplan zur Identifizierung und Reaktion auf exponierte IAM-Anmeldeinformationen
3. [**VORBEREITUNG**] Implementieren Sie eine Kommunikationsstrategie für die Reaktion auf Vorfälle
4. [**ERKENNUNG**] Identifizieren Sie den Root-Kontozugriff (autorisiert und nicht)
5. [**ERKENNUNG**] Identifizieren Sie neue oder nicht erkannte IAM-Benutzer
6. [**ERKENNUNG**] Identifizieren Sie nicht erkannte oder nicht autorisierte Ressourcen (z. B. EC2, Lambda)
7. [**ERKENNUNG**] Identifizieren und finden Sie exponierte Geheimnisse
8. [**ERKENNUNG**] Identifizieren Sie ungewöhnliche Abrechnungssteigerungen
9. [**ERKENNUNG**] Erteilen Sie die Benachrichtigung von AWS oder einem Dritten, dass meine AWS-Ressourcen oder mein Konto möglicherweise gefährdet sind
10. [**ERKENNUNG**] Identifizieren Sie potenziell nicht autorisierte IAM-Benutzeranmeldeinformationen
11. [**PREPARATION**] Identifizieren Sie Eskalationsverfahren
12. [**ERKENNUNG UND ANALYSE**] CloudTrail-Logs überprüfen
13. [**ERKENNUNG UND ANALYSE**] VPC Flow Logs überprüfen
14. [**ERKENNUNG UND ANALYSE**] Endpoint überprüfen/Host-basierte Protokolle
15. [**CONTAINMENT**] Führen Sie geeignete Eindämmungsmaßnahmen durch
16. [**ERADICATION**] Überprüfen Sie die Ergebnisse aus dem CloudTrail-Ereignisverlauf überprüfen auf Aktivitäten anhand des kompromittierten Zugriffsschlüssels
17. [**ERADIKATION**] Überprüfen Sie die Vermeidung unerwarteter Anklagen
18. [**WIEDERHERSTELLUNG**] Führen Sie geeignete Wiederherstellungsaktionen durch
19. [**VORBEREITUNG**] Führen Sie einen Prowler IAM-Scan durch
20. [**PREPARATION**] MFA aktivieren
21. [**VORBEREITUNG**] Überprüfen Sie Ihre Kontoinformationen
22. [**VORBEREITUNG**] Verwenden Sie AWS Git-Projekte, um nach Nachweisen für unbefugte Verwendung zu suchen
23. [**VORBEREITUNG**] Vermeiden Sie es, den Root-Benutzer für den täglichen Betrieb zu verwenden
24. [**PREPARATION**] Bewerten Sie Ihre allgemeine Sicherheitslage

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Leitfaden zur Behandlung von Computersicherheitsvorfällen] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Exposition von Anmeldeinformationen
* **Kategorie**: Exposition von IAM-Anmeldeinformationen
* **Ressourcen**: IAM
* **Indikatoren**: Cyber Threat Intelligence, Mitteilung Dritter
* **Protokollquellen**: AWS CloudTrail, AWS Config, VPC Flow Logs, Amazon GuardDuty
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Handhabung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

## Vorbereitung
Dieses Playbook verweist und integriert sich, wenn möglich, in [Prowler] (https://github.com/toniblyx/prowler), einem Befehlszeilen-Tool, das Ihnen bei der AWS-Sicherheitsbewertung, -überwachung, -härtung und Reaktion auf Vorfälle hilft.

Es folgt den Richtlinien des CIS Amazon Web Services Foundations Benchmark (49 Schecks) und verfügt über mehr als 100 zusätzliche Kontrollen, einschließlich der DSGVO, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 und anderen.

Dieses Tool bietet eine schnelle Momentaufnahme des aktuellen Sicherheitszustands in einer Kundenumgebung. Alternativ ermöglicht [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) ein automatisiertes Beschwerde-Scannen und kann [in Prowler integrieren] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventar für Vermögenswerte
Identifizieren Sie alle bestehenden Benutzer und haben Sie eine aktualisierte Liste des Zwecks für jedes Konto

1. Navigieren Sie zu [AWS Console] (https://console.aws.amazon.com/)
1. Gehen Sie zu `Dienstleistungen` und wählen Sie `IAM`
1. Wählen Sie im linken Menü „Credential Report`
1. Wählen Sie „Bericht herunterladen`
1. Achten Sie besonders darauf, wann Konten erstellt wurden, das Passwort zuletzt verwendet und das Passwort zuletzt geänderte Spalten.
* **HINWEIS** Wenn ein Benutzer mehrere Zugriffsschlüssel hat, validieren Sie den Zweck für jeden

### Schulung
- `Welche Schulung gibt es für Analysten innerhalb des Unternehmens, um sich mit der AWS API (Befehlszeilenumgebung), S3, RDS und anderen AWS-Services vertraut zu machen? `
>>>
Zu den Möglichkeiten zur Bedrohungserkennung und zur Reaktion auf Vorfälle gehören:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service-Sicherheitsbewertung] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Welche Rollen können Änderungen an Diensten in Ihrem Konto vornehmen? `
- `Welchen Benutzern haben diese Rollen ihnen zugewiesen? Wird das geringste Privileg befolgt oder gibt es Super-Admin-Benutzer? `
- `Wurde eine Sicherheitsbewertung für Ihre Umgebung durchgeführt, haben Sie eine bekannte Baseline, um „neue“ oder „verdächtige“ Dinge zu erkennen? `

### Kommunikationstechnologie
- `Welche Technologie wird innerhalb des Team/Unternehmens verwendet, um Probleme zu kommunizieren? Ist irgendetwas automatisiert? `
>>>
Telefon\
E-Mail\
AWS SES\
AWS SNS\
Slack\
Glockenspiel\
Andere?
>>>

## Erkennung

### Root-Kontozugriff
Es wird empfohlen, alle mit dem Root-Konto verknüpften Zugriffsschlüssel zu entfernen: `. /prowler -c check_112`

### Neue oder unerkannte IAM-Benutzer
Überprüfen Sie den IAM-Anmeldeinformationen aus Ihrem [Asset Inventar] (. /compromised_iam_Credentials.md/ #asset -inventar)
Prüfen Sie, ob IAM-Benutzer zwei aktive Zugriffsschlüssel haben: `. /prowler -c check_extra712`
Stellen Sie sicher, dass IAM-Richtlinien, die vollständige\ "*: *\“ Administratorrechte zulassen, nicht erstellt werden: `. /prowler -c check_122`
Prüfen Sie, ob der IAM Access Analyzer aktiviert ist und seine Ergebnisse: `. /prowler -c check_extra769`

### Unerkannte oder nicht autorisierte Ressourcen (z. B. EC2, Lambda)
aws ec2 describe-Instanzen
aws Lambda List-Funktionen

### Auf der Suche nach Geheimnissen
Mögliches Geheimnis in EC2-Instanz-Benutzerdaten gefunden: `. /prowler -c check_extra741`
Mögliches Geheimnis in Lambda-Funktionsvariablen gefunden: `. /prowler -c check_extra759`
Mögliches Geheimnis in ECS-Aufgabendefinitionsvariablen gefunden: `. /prowler -c check_extra768`
Mögliches Geheimnis in Autoscaling-Konfiguration gefunden: `. /prowler -c check_extra775`

### Ungewöhnliche Abrechnungssteigerungen
Um Ihre AWS-Rechnung anzuzeigen, öffnen Sie den Bereich [Rechnungen] (https://console.aws.amazon.com/billing/home #) der Rechnungs- und Kostenverwaltungskonsole und wählen Sie dann den Monat aus dem Dropdown-Menü aus.

Sie können den Verlauf Ihrer AWS-Zahlungen im Bereich [Bestellungen und Rechnungen] (https://console.aws.amazon.com/billing/home?/paymenthistory) der Rechnungs- und Kostenmanagement-Konsole anzeigen.

Sie können auch [AWS Cost Anomaly Detection] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) zur dynamischen Überwachung und Warnung verwenden.

Prüfen Sie Ihre Rechnung auf Folgendes:
* AWS-Services, die Sie normalerweise nicht verwenden
* Ressourcen in AWS-Regionen, die Sie normalerweise nicht verwenden
* Eine signifikante Änderung der Größe Ihrer Rechnung

Sie können diese Informationen verwenden, um Ressourcen zu löschen oder zu beenden, die Sie nicht behalten möchten.

### Benachrichtigung, dass meine AWS-Ressourcen oder mein Konto möglicherweise gefährdet sind
Wenn Sie von AWS eine Benachrichtigung über Ihr Konto erhalten haben, melden Sie sich beim AWS Support Center an und antworten Sie dann auf die Benachrichtigung.

Wenn Sie sich nicht bei Ihrem Konto anmelden können, verwenden Sie das Kontaktformular, um Hilfe vom AWS Support anzufordern.

Wenn Sie Fragen oder Bedenken haben, erstellen Sie einen neuen AWS-Supportfall im AWS Support Center.
* **HINWEIS**: Nehmen Sie keine sensiblen Informationen wie AWS-Zugangsschlüssel, Passwörter oder Kreditkarteninformationen in Ihre Korrespondenz ein.
Verwenden Sie AWS Git-Projekte, um nach Nachweisen für unbefugte Verwendung zu suchen

### Identifizieren Sie potenziell nicht autorisierte IAM-Benutzeranmeldeinformationen
1. Öffnen Sie die IAM-Konsole.
1. Wählen Sie im Navigationsbereich Benutzer aus.
1. Wählen Sie jeden IAM-Benutzer aus der Liste aus und suchen Sie dann unter Berechtigungsrichtlinien nach einer Richtlinie mit dem Namen awSexPosedCredentialPolicy_Do_Not_Remove. 1. Wenn der Benutzer über diese angehängte Richtlinie verfügt, müssen Sie die Zugriffsschlüssel für den Benutzer drehen.

## Eskalationsverfahren
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Analyse
Es wird dringend empfohlen, Protokolle in eine SIEM-Lösung (Security Incident Event Management) (wie Splunk, ELK-Stack usw.) zu exportieren, um bei der Anzeige und Analyse einer Vielzahl von Protokollen für eine vollständigere Angriffszeitachsenanalyse zu helfen.

### CloudTrail
Suchen Sie nach ungewöhnlichen Login-Aktivitäten
1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Geben Sie im Suchfeld `ConsoleLogin` oder `Assumerole` oder `GetFederationToken` oder `GetCredentialReport` oder `GenerateCredentialReport` ein und überprüfen Sie die verfügbaren Ereignisse auf verdächtige Aktivitäten
* **HINWEIS** UserIdentify wird als `"type“ angezeigt: „Root"` für root oder `"type“: „iamUser"` für Benutzer

Suchen Sie die IAM-Zugriffsschlüssel-ID und den Benutzernamen zum Starten einer verdächtigen EC2-Instance
1. Öffnen Sie die CloudTrail-Konsole und wählen Sie dann Ereignisverlauf aus.
1. Wählen Sie das Dropdown-Menü Filter aus und wählen Sie dann Ressourcenname aus.
1. Fügen Sie im Feld Ressourcenname eingeben die EC2-Instanz-ID ein, und wählen Sie dann auf Ihrem Gerät eingeben aus.
1. Erweitern Sie den Ereignisnamen für RunInstances.
1. Kopieren Sie den AWS-Zugriffsschlüssel und notieren Sie sich den Benutzernamen.

Überprüfen Sie den CloudTrail-Ereignisverlauf auf Aktivitäten anhand des kompromittierten Zugriffsschlüssels
1. Öffnen Sie die CloudTrail-Konsole und wählen Sie dann im Navigationsbereich Ereignisverlauf aus.
1. Wählen Sie das Dropdown-Menü „Filter“ und wählen Sie dann den AWS-Zugriffsschlüsselfilter aus.
1. Geben Sie im Feld AWS-Zugriffsschlüssel eingeben die ID des kompromittierten IAM-Zugriffsschlüssels ein.
1. Erweitern Sie den Ereignisnamen für den API-Aufruf RunInstances.
* **HINWEIS**: Sie können den Ereignisverlauf der letzten 90 Tage nur anzeigen, es sei denn, Sie haben zuvor einen Trail konfiguriert, der in einem S3-Bucket gespeichert wird

### VPC Flow Logs
VPC Flow Logs ist eine Funktion, mit der Sie Informationen über den IP-Datenverkehr zu und von Netzwerkschnittstellen in Ihrer VPC erfassen können. Dies kann für in CloudTrail ermittelte IP-Adressen nützlich sein, um die Arten externer Verbindungen zu öffentlichen Ressourcen zu bestimmen.

Weitere Informationen und Schritte, einschließlich der Abfrage bei Athena, finden Sie in der [AWS-Dokumentation für VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Es wird empfohlen, die Athena-Analyse in ein separates Playbook aufzunehmen und mit anderen relaventen Elementen zu verknüpfen.

### Endpunkt/Host-basiert
* **Identifizieren Sie die letzte Erstellungszeit für einen bestimmten Instanznamen**: `aws ec2 describe-instanzen —query 'Reservierungen [] .Instances []. {ip: publicipAddress, tm: launchTime} '—filters 'name=Tag:Name, Values= MyInstanceName' | jq 'sort_by (.tm) | rückwärts |. [0] '`

* **Letzte Änderung für Lambda-Funktionen identifizieren**: `aws lambda list-funktionen | grep „lastModified"`

## Eindämmung
1. Deaktivieren Sie den IAM-Benutzer, erstellen Sie einen IAM-Zugriffsschlüssel für die Sicherung und deaktivieren Sie dann den kompromittierten Zugriffsschlüssel
1. Öffnen Sie die [IAM-Konsole] (https://console.aws.amazon.com/iam/) und fügen Sie dann die IAM-Zugriffsschlüssel-ID in die IAM-Leiste für die Suche ein.
1. Wählen Sie den Benutzernamen und wählen Sie dann die Registerkarte Sicherheitsanmeldeinformationen.
1. Wählen Sie unter Konsolenpasswort Verwalten aus.
* **HINWEIS**: Wenn das Kennwort der AWS Management Console auf Deaktiviert festgelegt ist, können Sie diesen Schritt überspringen.
1. Wählen Sie unter Konsolenzugriff Deaktivieren und dann Anwenden aus.
1. Wichtig: Benutzer, deren Konten deaktiviert sind, können nicht auf die AWS Management Console zugreifen. Wenn der Benutzer jedoch über aktive Zugriffsschlüssel verfügt, kann er weiterhin über API-Aufrufe auf AWS-Services zugreifen.
1. Wählen Sie für den kompromittierten IAM-Zugriffsschlüssel die Option Inaktiv machen aus.
1. Deaktivieren Sie den IAM-Schlüssel der Anwendung, erstellen Sie einen IAM-Zugriffsschlüssel für die Sicherung und deaktivieren Sie dann den kompromittierten Zugriffsschlüssel
1. Erstellen Sie zuerst einen zweiten Schlüssel. Ändern Sie dann Ihre Anwendung, um den neuen Schlüssel zu verwenden.
1. Deaktivieren (aber löschen Sie nicht) den ersten Schlüssel.
1. Wenn es Probleme mit Ihrer Anwendung gibt, aktivieren Sie den Schlüssel vorübergehend erneut. Wenn Ihre Anwendung voll funktionsfähig ist und sich der erste Schlüssel im deaktivierten Zustand befindet, löschen Sie den ersten Schlüssel.
1. Drehen und löschen Sie alle inaktiven/kompromsierten AWS-Zugriffsschlüssel
* **!! Stellen Sie sicher, dass Sie alle gelöschten Zugriffsschlüssel aufzeichnen, damit Sie sie weiterhin in CloudTrail durchsuchen können!! **

## Ausrottung
### Überprüfen Sie die Ergebnisse von [CloudTrail-Ereignisverlauf auf Aktivitäten anhand des kompromittierten Zugriffsschlüssels überprüfen] (. /#cloudtrail)
Entfernen Sie alle Ressourcen, die von den kompromittierten Schlüsseln erstellt wurden. Überprüfen Sie unbedingt alle AWS-Regionen, sogar Regionen, in denen Sie nie AWS-Ressourcen eingeführt haben.
* **Wichtig**: Wenn Sie Ressourcen für die Untersuchung behalten müssen, sollten Sie diese sichern. Wenn Sie beispielsweise eine regulatorische, konformitätsrechtliche oder rechtliche Notwendigkeit haben, eine EC2-Instance beizubehalten, erstellen Sie einen EBS-Snapshot, bevor Sie die Instance beenden.

### Prüfen Sie das [Unerwartete Gebühren vermeiden] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
Suchen Sie nach Diensten, die in Ihrem Konto erkannt werden, und entfernen Sie sie. Achten Sie besonders auf die folgenden Ressourcen:
* EC2-Instanzen und AMIs, einschließlich Instanzen im gestoppten Status
* EBS-Volumes und Snapshots
* AWS Lambda-Funktionen und Layer

Gehen Sie wie folgt vor, um Lambda-Funktionen und -Layer zu löschen:
1. Öffne die Lambda-Konsole.
1. Wählen Sie im Navigationsbereich Funktionen aus.
1. Wählen Sie die Funktionen aus, die Sie löschen möchten.
1. Wählen Sie für Aktionen Löschen aus.
1. Wählen Sie im Navigationsbereich Layer aus.
1. Wählen Sie den Layer aus, den Sie löschen möchten.
1. Wählen Sie „Löschen“.

Löschen Sie alle IAM-Benutzer, die Sie nicht erstellt haben
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [IAM-Konsole] (https://console.aws.amazon.com/iam/)
1. Wählen Sie im Navigationsbereich Benutzer aus, und aktivieren Sie dann das Kontrollkästchen neben dem Benutzernamen, den Sie löschen möchten, nicht den Namen oder die Zeile selbst.
1. Wählen Sie oben auf der Seite Benutzer löschen aus.
1. Warten Sie im Bestätigungsdialogfeld, bis die zuletzt aufgerufenen Informationen geladen wurden, bevor Sie die Daten überprüfen. Das Dialogfeld zeigt an, wann jeder der ausgewählten Benutzer zuletzt auf einen AWS-Service zugegriffen hat. Wenn Sie versuchen, einen Benutzer zu löschen, der innerhalb der letzten 30 Tage aktiv war, müssen Sie ein zusätzliches Kontrollkästchen aktivieren, um zu bestätigen, dass Sie den aktiven Benutzer löschen möchten. Wenn Sie fortfahren möchten, wählen Sie Ja, Löschen.

Dokumentation zum Löschen anderer Dienste finden Sie auf der Seite [Alle meine Ressourcen beenden] (https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/).

## Erholung
AWS hat einen veröffentlichten Leitfaden für [was mache ich, wenn ich nicht autorisierte Aktivitäten in meinem AWS-Konto merke?] (https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## Vorbeugende Maßnahmen
### Prowler IAM-Scan
`. /prowler -g check122, check111, check110, check19, check18, check17, check16, check15, check11, check116, check12, check114, check114, check115, check14, check13, check112, check119, extra71, extra7100, extra7123, extra7125, extra769, extra769, extra774`

### MFA aktivieren
Um die Sicherheit zu erhöhen, empfiehlt es sich, MFA zum Schutz Ihrer AWS-Ressourcen zu konfigurieren. Sie können [MFA für IAM-Benutzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) oder den [Root-Benutzer des AWS-Kontos] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) aktivieren. Das Aktivieren von MFA für den Root-Benutzer wirkt sich nur auf die Anmeldeinformationen des Root-Benutzers aus. IAM-Benutzer im Konto sind unterschiedliche Identitäten mit ihren eigenen Anmeldeinformationen, und jede Identität hat ihre eigene MFA-Konfiguration.

### Verifizieren Sie Ihre Kontoinformationen
AWS benötigt genaue Kontoinformationen, um Sie zu kontaktieren und bei der Behebung von Kontoproblemen zu helfen. Vergewissern Sie sich, dass die Informationen in Ihrem Konto korrekt sind.
* Der Kontoname und die E-Mail-Adresse.
* Ihre Kontaktinformationen, insbesondere Ihre Telefonnummer.
* Die alternativen Kontakte für Ihr Konto.

### Verwenden Sie AWS Git-Projekte, um nach Nachweisen für eine unbefugte Verwendung zu suchen
AWS bietet Git-Projekte an, die Sie installieren können, um Ihr Konto zu schützen:
* [Git Secrets] (https://github.com/awslabs/git-secrets) kann Zusammenführungen, Commits und Nachrichten auf geheime Informationen (dh Zugriffsschlüssel) scannen. Wenn Git Secrets verbotene reguläre Ausdrücke erkennt, kann es die Veröffentlichung dieser Commits in öffentlichen Repositorys ablehnen.
* Verwenden Sie [AWS Step Functions und AWS Lambda, um Amazon CloudWatch Events] (https://aws.amazon.com/step-functions) aus AWS Health oder von AWS Trusted Advisor zu generieren. Wenn es Hinweise darauf gibt, dass Ihre Zugriffsschlüssel verfügbar sind, können die Projekte Ihnen helfen, das Ereignis automatisch zu erkennen, zu protokollieren und zu mildern.

### Vermeiden Sie es, den Root-Benutzer für den täglichen Betrieb zu verwenden
Der Zugriffsschlüssel für den Root-Benutzer Ihres AWS-Kontos bietet vollen Zugriff auf alle Ihre AWS-Ressourcen, einschließlich Ihrer Rechnungsinformationen. Sie können die mit dem Root-Benutzerzugriffsschlüssel Ihres AWS-Kontos verknüpften Berechtigungen nicht reduzieren. Es empfiehlt sich, den Root-Benutzerzugriff nicht zu verwenden, es sei denn, dies ist absolut erforderlich.

Wenn Sie noch keinen Zugriffsschlüssel für den Root-Benutzer Ihres AWS-Kontos haben, erstellen Sie keinen, es sei denn, dies ist absolut erforderlich. Erstellen Sie stattdessen einen IAM-Benutzer mit Administratorberechtigungen für sich selbst. Sie können sich mit der E-Mail-Adresse und dem Kennwort Ihres AWS-Kontos bei der AWS Management Console anmelden, um einen IAM-Benutzer zu erstellen.

### Allgemeine Sicherheitslage
Führen Sie eine [Self-Service-Sicherheitsbewertung] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) gegen die Umgebung durch, um andere Risiken und möglicherweise andere öffentliche Exposition, die in diesem Playbook nicht identifiziert wurden, weiter zu identifizieren.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Playbook zum Umgang mit Session-Token und Zugriffsschlüsseln nach einem Vorfall
- Als Incident Responder benötige ich eine Methode zum Scannen und Entfernen von Schlüsseln aus öffentlichen Code-Repositorys
- Als Incident Responder brauche ich einen klaren Entscheidungsträger, wann ich eine Umgebung durchbrechen soll, anstatt eine kontinöse Exposition zu ermöglichen
## Aktuelle Backlog-Artikel