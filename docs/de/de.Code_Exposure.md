Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2024 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt den Prozess zur Identifizierung von Besitzern von Code-Ressourcen, zur Ermittlung, wie sie exponiert wurden, wer möglicherweise auf den exponierten Code zugegriffen hat, die Auswirkungen der Exposition ermitteln, Behebungsmaßnahmen erforderlich und die Ursache für die Code-Exposition ermitteln kann.

## Mögliche Indikatoren für Kompromisse
- Warnungen zur Verhinderung von Datenverlust (DLP)
- Bekannte Code-Veröffentlichung oder Exposition
- Verdächtige CloudTrail-Protokolle

## Mögliche AWS GuardDuty GuardDuty-Ergebnisse
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Exfiltration:S3/MaliciousIPCaller
- Exfiltration:S3/ObjectRead.Unusual
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- Persistence:IAMUser/AnomalousBehavior
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnis***_ und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Codekopien, Transfers und Publikation**
* **Belichtungsberechtigung**
* **Schwachstellen**
* **Umweltintelligenz exponiert**
* **Verwendete Tools
* **Konfigurationsinformationen**
* **Absichten des Schauspieler**
* **Zuordnung des Schauspieler**
* **Sonstiger Schaden, der Umwelt und dem Geschäft verursacht**
* **Risiko für Umwelt und Geschäfts**

#### Wiederherstellen:
* **Alle exponierten Authentifizierungsanmeldeinformationen ablaufen oder zurücksetzen**
* **Risiken für potenzielle Angriffsvektoren basierend auf exponierter Umweltintelligenz aufzählen**
* **Risiken des exponierten Codes minimieren oder eliminieren**

#### Komponenten der CAF-Sicherheitsperspektive verbessern:
[Sicherheitsperspektive des AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Richtlinie**
* **Detektiv**
* **Verantwortlich**
* **Präventiv**

! [Bild] (/images/aws_caf.png)
* *

### Antwortschritte
1. [**VORBEREITUNG**] Erstellen Sie ein Asset Inventar eines Kontos
2. [**VORBEREITUNG**] Erstellen Sie ein Repository-Inventar
3. [**PREPARATION**] Logging nach Bedarf aktivieren
4. [**PREPARATION**] Identifizieren Sie, welche Art von Daten sich in jedem Repository befindet
5. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
6. [**VORBEREITUNG**] Implementieren Sie Schulungen zur Bekämpfung von Exfiltrationsangriffen
7. [**ERKENNUNG UND ANALYSE**] Exfiltrations- und DLP-Prüfungen durchführen
8. [**ERKENNUNG UND ANALYSE**] Repository (CodeCommit) Lese- und Schreibaktionen überprüfen
9. [**ERKENNUNG UND ANALYSE**] DNS-Protokolle überprüfen
10. [**ERKENNUNG UND ANALYSE**] VPC-Flussprotokolle überprüfen
11. [**ERKENNUNG UND ANALYSE**] Endpunkt/Host-basierte Protokolle überprüfen
12. [**CONTAINMENT**] Zugriff für betroffene Konten blockieren
13. [**ERADICATION**] Entfernen Sie nicht erkannte und nicht autorisierte Objekte in Repositorys
14. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsverfahren durch

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Datenexfiltration, AWS Service Missbrauch
* **Kategorie**: Datenverlust
* **Ressource**: CodeCommit, S3, EC2
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, CloudWatch Metrics
* **Protokollquellen**: DNS-Protokolle, VPC Flow Logs, CloudTrail, CloudWatch
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

Es folgt den Richtlinien des CIS Amazon Web Services Foundations Benchmark (49 Schecks) und verfügt über mehr als 100 zusätzliche Kontrollen, darunter im Zusammenhang mit DSGVO, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 und anderen.

Dieses Tool bietet eine schnelle Momentaufnahme des aktuellen Sicherheitszustands in einer Kundenumgebung. Darüber hinaus bietet [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) ein automatisiertes Compliance-Scannen und kann [in Prowler integrieren] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventar für Vermögenswerte
Identifizieren Sie alle vorhandenen Ressourcen und verfügen Sie über eine aktualisierte Asset-Inventarliste, die mit wem die einzelnen Ressourcen gehören. Der Quellcode kann in einem der folgenden Assets gespeichert werden:
- (CodeCommit) [https://aws.amazon.com/codecommit/] Repositorys
- (S3) [https://aws.amazon.com/s3/] unterstützter Codespeicher
- (EC2) [https://aws.amazon.com/ec2/] Selbstgehostete Code-Speichermechanismen

#### Identifizieren Sie, welche Daten von jedem Asset exfiltriert wurden
- CloudWatch- und CloudTrail-Protokolle, die in S3 gespeichert sind, können mit (Amazon Athena) [https://aws.amazon.com/athena/] abgefragt werden, um unerwünschte Aktionen zu identifizieren
- (Amazon GuardDuty) [https://aws.amazon.com/guardduty/] erkennt möglicherweise automatisch anomalen Datenverkehr. Dies kann mit (AWS Security Hub) [https://aws.amazon.com/security-hub/] oder (Amazon Detective) [https://aws.amazon.com/detective/] zugegriffen werden
- In S3 gespeicherte Anwendungs- und Instanzprotokolle können auch mit Athena abgefragt werden.

### Schulung
- `Welche Schulung gibt es für Analysten innerhalb des Unternehmens, um sich mit der AWS API (Befehlszeilenumgebung), CodeCommit, S3, RDS und anderen AWS-Services vertraut zu machen? `
>>>
Zu den Möglichkeiten zur Bedrohungserkennung und zur Reaktion auf Vorfälle gehören:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Welche Rollen können Änderungen an Diensten in Ihrem Konto vornehmen? `
- `Welchen Benutzern haben diese Rollen ihnen zugewiesen? Wird das geringste Privileg befolgt oder gibt es Super-Admin-Benutzer? `
- `Wurde eine Sicherheitsbewertung gegen Ihre Umgebung durchgeführt? Haben Sie eine bekannte Baseline, um „neue“ oder „verdächtige“ Dinge zu erkennen? `

### Kommunikationstechnologie
- `Welche Technologie wird innerhalb des Team/Unternehmens verwendet, um Probleme zu kommunizieren? Ist irgendetwas automatisiert? `
>>>
Telefon\
E-Mail\
SMS\
AWS SES\
AWS SNS\
Slack\
Glockenspiel\
Teams\
Andere?
>>>

### DLP-Implementierung

Die Implementierung einer Data Loss Prevention (DLP) -Lösung bietet möglicherweise zusätzliche Erkennungsfunktionen und Warnungen. Eine DLP-Lösung kann in EC2-Umgebungen den größten Wert bieten oder den Netzwerkverkehr auswerten. DLP-Lösungen finden Sie im [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp).

## Erkennung

### Protokollierung und S3-Bucket-Prüfungen
- Stellen Sie sicher, dass CloudTrail in allen Regionen aktiviert ist: `. /prowler -c check21`
- Stellen Sie sicher, dass die Überprüfung der CloudTrail-Logdatei aktiviert ist: `. /prowler -c check22`
- Stellen Sie sicher, dass die S3-Bucket-Zugriffsprotokollierung im CloudTrail S3-Bucket aktiviert ist: `. /prowler -c check26`
- Stellen Sie sicher, dass keine S3-Buckets für alle oder jeden AWS-Benutzer geöffnet sind: `. .prowler -c extra73`
- Identifizieren Sie die Ressourcen in Ihrer Organisation und Ihren Konten, wie Amazon S3 S3-Buckets oder IAM-Rollen, die mit einer externen Entität geteilt werden: `. .prowler -c extra769`
- Finden Sie Ressourcen, die dem Internet ausgesetzt sind: `. /prowler -g group17`

### Protokollierung und CodeCommit-Ereignisse
- [AWS CodeCommit-Ereignisse in EventBridge überwachen] (https://docs.aws.amazon.com/codecommit/latest/userguide/monitoring-events.html), das einen Strom von Echtzeitdaten liefert. Diese Ereignisse entsprechen denen, die in Amazon CloudWatch Events angezeigt werden, die einen nahezu Echtzeitstrom von Systemereignissen liefern, die Änderungen an AWS-Ressourcen beschreiben.
- [Erstellen Sie einen CloudTrail-Trail, um die kontinuierliche Bereitstellung von CodeCommit-Ereignissen an einen S3-Bucket zu ermöglichen] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html). CloudTrail erfasst alle API-Aufrufe für CodeCommit als Ereignisse.

### DLP-Warnungen

Die Implementierung einer DLP-Lösung bietet möglicherweise zusätzliche Erkennungsfunktionen und Warnungen. Details sind beim DLP-Lösungsanbieter verfügbar.

## Eskalationsverfahren
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Analyse
Es wird dringend empfohlen, Protokolle in eine SIEM-Lösung (Security Incident Event Management) (wie Splunk, ELK stack usw.) zu exportieren, um eine Vielzahl von Protokollen für eine umfassendere Analyse der Angriffszeitleiste anzuzeigen und zu analysieren.

### CloudTrail
CloudTrail bietet bis zu 90 Tage Ereignisprotokolle für alle AWS API-Aufrufe. Diese Informationen können verwendet werden, um bösartige oder anomale Aktionen zu identifizieren und zu verfolgen. Weitere Informationen finden Sie in der [CloudTrail-Log-Ereignisreferenz] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html).

### CloudTrail: Öffentlicher S3-Bucket
Standardmäßig protokolliert CloudTrail API-Aufrufe, die in den letzten 90 Tagen getätigt wurden, jedoch keine Anfragen an Objekte protokollieren. Sie können Ereignisse auf Bucket-Ebene auf der CloudTrail-Konsole sehen. Sie können jedoch keine Datenereignisse (Aufrufe auf Amazon S3 S3-Objektebene) standardmäßig anzeigen — Sie müssen die Protokollierung auf Objektebene aktivieren, bevor diese Ereignisse in CloudTrail angezeigt werden.

1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail-Protokolle für die Eventnamen `getPublicAccessBlock` und `DeletePublicAccessBlock`

### CloudTrail: Öffentliches S3-Objekt
Sie können auch CloudTrail-Protokolle für Amazon S3 S3-Aktionen auf Objektebene abrufen. Um dies zu tun, [Aktivieren Sie Datenereignisse für Ihren S3-Bucket] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html) oder alle Buckets in Ihrem Konto. Wenn in Ihrem Konto eine Aktion auf Objektebene auftritt, wertet CloudTrail Ihre Trail-Einstellungen aus. Wenn das Ereignis mit dem Objekt übereinstimmt, das Sie in einem Trail angegeben haben, wird das Ereignis protokolliert.

1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail-Protokolle für die Eventnamen `getObjectCl` und `PutObjectCl`

### VPC Flow Logs
VPC Flow Logs ist eine Funktion, mit der Sie Informationen über den IP-Datenverkehr zu und von Netzwerkschnittstellen in Ihrer VPC erfassen können. Dies kann für in CloudTrail ermittelte IP-Adressen nützlich sein, um die Arten externer Verbindungen zu öffentlichen Ressourcen zu bestimmen.

Weitere Informationen und Schritte, einschließlich der Abfrage bei Athena, finden Sie in der [AWS-Dokumentation für VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Es wird empfohlen, die Athena-Analyse in ein separates Playbook aufzunehmen und mit anderen relevanten Elementen zu verknüpfen.

### DNS-Protokolle
DNS Logs ist eine Funktion, mit der Sie Informationen über den DNS-Datenverkehr zu und von Netzwerkschnittstellen in Ihrer VPC erfassen können. Dies kann nützlich sein, um Anomalien oder Bereiche mit hohem Risiko zu identifizieren.

### CloudWatch
Daten von EC2-Instanzen und anderen Quellen können [in CloudWatch aufgenommen] werden (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html). Diese Daten können verwendet werden, um Alarme auszulösen oder Analysen durchzuführen. CloudWatch bietet auch [Anomalieerkennung] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html), sofern aktiviert.

### Endpunkt/Host-basiert
1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail auf `putObject` und `DeleteObject` Anfragen von öffentlichen IP-Adressen

- Überprüfen Sie das EC2-Betriebssystem- und Anwendungsprotokolle auf unangemessene Anmeldungen, Installation unbekannter Software oder das Vorhandensein nicht erkannter Dateien.

- Es wird dringend empfohlen, eine Host-basierte Intrusion Detection System (HIDS) -Lösung eines Drittanbieters zu haben (wie OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), andere)

### DLP

DLP-Lösungen erkennen und warnen möglicherweise wie konfiguriert. DLP-Lösungen sind auf dem [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) verfügbar.

## Eindämmung

### S3 öffentlichen Zugriff blockieren
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration „BlockPublicAcls=true, IgnorePublicAcls=true, BlockPublicPolicy=true, RestrictPublicBuckets=true“

Sie können auch [Blockieren des öffentlichen Zugriffs auf Ihren Amazon S3 S3-Speicher] (https://aws.amazon.com/s3/features/block-public-access/) für zusätzliche Informationen zum Sperren des öffentlichen S3-Zugriffs in Ihrem Konto überprüfen.

### CodeCommit

Prüfen Sie die Zugriffskontrolle für alle Benutzer mit dem Service [AWS Identity and Access Management (IAM) in Verbindung mit CodeCommit] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html).

Berechtigungen können auch mit der [CodeCommit-Berechtigungsreferenz] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-permissions-reference.html) geändert oder eingeschränkt werden.

## Ausrottung

### S3 Unerkannte /Unautorisierte Objekte entfernen
Entferne alle nicht erkannten Objekte aus Buckets

1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die Amazon S3 S3-Konsole unter https://console.aws.amazon.com/s3/
1. Wählen Sie in der Liste Bucket-Name den Namen des Buckets aus, aus dem Sie ein Objekt löschen möchten.
1. Wählen Sie den Namen des Objekts aus, das Sie löschen möchten.
1. Um die aktuelle Version des Objekts zu löschen, wählen Sie Neueste Version und wählen Sie das Papierkorbsymbol.
1. Um eine frühere Version des Objekts zu löschen, wählen Sie Neueste Version und wählen Sie das Papierkorbsymbol neben der Version, die Sie löschen möchten.

### AWS CodeCommit

[Audit-Identitäten und Zugriff auf CodeCommit.] (https://docs.aws.amazon.com/codecommit/latest/userguide/security-iam.html)

### Amazon EC2

Wo möglich:

1. [Starten Sie eine Ersatz-EC2-Instance] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-launch-snapshot.html) mit [EBS Snapshots] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) oder [Amazon Machine Image (AMI) Backups] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) aus der Quelle erstellt
1. [Fügen Sie ein EBS-Volume an] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) von der beendeten Instance an die neue EC2-Instance.

## Erholung
Gleiche Verfahren wie die für Tilgung aufgeführten

## Vorbeugende Maßnahmen

### Authentifizierung Prowler Checks
- Stellen Sie sicher, dass die Multi-Faktor-Authentifizierung (MFA) aktiviert ist:
`. /prowler -c check12`

### S3 Prowler Schecks
**Verschlüsselung**
- Prüfen Sie, ob S3-Buckets die Standardverschlüsselung (SSE) aktiviert haben: `. .prowler -c extra734`

**Notfallwiederherstellung**
- Prüfen Sie, ob S3-Buckets die Objektversionierung aktiviert haben: `. /prowler -c extra763`

### S3-Dienstaktionen
[Verhindern Sie, dass Benutzer die Einstellungen für den öffentlichen Zugriff S3 ändern] (https://asecure.cloud/a/scp_s3_block_public_access/)

Überprüfen Sie regelmäßig den Bucket-Zugriff und die Richtlinien monatlich und nutzen Sie [CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) oder Security Hub für automatisierte Erkennungen

[Versionierung in S3-Buckets verwenden] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html), um das versehentliche oder absichtliche Löschen von Objekten der obersten Ebene zu mildern

[Zugriff mit ACLs verwalten] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) zur Beschränkung des unbefugten Zugriffs auf Ressourcen auf Bucket- und Objektebene

### Amazon EC2

[Verwenden Sie die Multi-Faktor-Authentifizierung (MFA) mit jedem Konto.] (https://aws.amazon.com/iam/features/mfa/)

Verwenden Sie TLS, um mit AWS-Ressourcen zu kommunizieren. Wir empfehlen TLS 1.2 oder höher. Einige Dienste haben dies standardmäßig aktiviert, andere müssen implementiert werden (z. B. im [JavaScript-SDK] (https://docs.aws.amazon.com/sdk-for-script/v2/developer-guide/enforcing-tls.html)).

[Richten Sie die API- und Benutzeraktivitätsprotokollierung mit AWS CloudTrail ein.] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitor-with-cloudtrail.html)

[Verwenden Sie AWS-Verschlüsselungslösungen wie KMS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) sowie alle Standardsicherheitskontrollen innerhalb von AWS-Services.

[EBS-Snapshots aktivieren] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)

### AWS CodeCommit

[Verwenden Sie die Multi-Faktor-Authentifizierung (MFA) mit jedem Konto.] (https://aws.amazon.com/iam/features/mfa/)

Verwenden Sie TLS, um mit AWS-Ressourcen zu kommunizieren. Wir empfehlen TLS 1.2 oder höher. Einige Dienste haben dies standardmäßig aktiviert, andere müssen implementiert werden (z. B. im [JavaScript-SDK] (https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/enforcing-tls.html)).

[Richten Sie die API- und Benutzeraktivitätsprotokollierung mit AWS CloudTrail ein.] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)

[Verwenden Sie AWS-Verschlüsselungslösungen wie KMS] (https://docs.aws.amazon.com/codecommit/latest/userguide/encryption.html) sowie alle Standardsicherheitskontrollen innerhalb von AWS-Services.

[Verwenden Sie erweiterte verwaltete Sicherheitsdienste wie Amazon Macie] (https://aws.amazon.com/blogs/compute/discovering-sensitive-data-in-aws-codecommit-with-aws-lambda-2/), die bei der Entdeckung und Sicherung personenbezogener Daten helfen, die in Amazon S3 gespeichert sind.

### Amazon Macie
[Amazon Macie] (https://aws.amazon.com/macie/) kann gespeicherte Anmeldeinformationen, private Schlüssel und andere Zugriffsdaten mithilfe [unter Verwendung verwalteter Datenkennungen] (https://docs.aws.amazon.com/macie/latest/user/managed-data-identifiers.html) erkennen.

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) verfügt über mehrere automatisierte Regeln zur Verwaltung der Code-Exposition einschließlich [codebuild-project-envvar-awscred-check] (https://docs.aws.amazon.com/config/latest/developerguide/codebuild-project-envvar-awscred-check.html) prüfen Sie, ob die Anmeldeinformationen im Code gespeichert sind.

### Allgemeine Sicherheitslage
Führen Sie eine [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) gegen die Umgebung durch, um andere Risiken und möglicherweise andere öffentliche Exposition, die in diesem Playbook nicht identifiziert wurden, weiter zu identifizieren.

### DLP-Implementierung

Die Implementierung einer Data Loss Prevention (DLP) -Lösung bietet möglicherweise zusätzliche Erkennungsfunktionen und Warnungen. DLP-Lösungen finden Sie im [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) und sollten wie vorgeschrieben konfiguriert werden.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook zur Erkennung von Code-Exposition
- Als Incident Responder brauche ich ein Runbook zur Erkennung von Codeexfiltration
- Als Incident Responder muss ich in der Lage sein, öffentliche Ressourcen (AMIs, EBS Volumes, ECR-Repos usw.) zu erkennen
- Als Incident Responder muss ich wissen, welche Rollen kritische Änderungen in AWS vornehmen können
- Als Incident Responder brauche ich ein Playbook zur Minderung einer Code-Exposition und erforderliche Eskalationspunkte
- Als Incident Responder benötige ich Dokumentation zu Protokollen, die für verschiedene Datenklassifikationen erforderlich sind

## Aktuelle Backlog-Artikel