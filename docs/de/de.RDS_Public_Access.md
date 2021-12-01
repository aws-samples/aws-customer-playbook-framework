# Incident Response Playbook: Exposition öffentlicher Ressourcen - RDS
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Namensnennung 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“\
Genehmiger: `Name des Genehmigers“\
Letztes Datum genehmigt:

## Zusammenfassung
In diesem Playbook wird der Prozess beschrieben, um Eigentümer öffentlicher Ressourcen zu identifizieren, festzustellen, wer auf diese zugegriffen hat, während sie exponiert wurden, die Auswirkungen des Widerrufs des Zugriffs auf die Ressource zu ermitteln und die Ursache für die öffentliche Zugänglichkeit zu ermitteln.

## Mögliche Indikatoren für Kompromisse
- Warnung für öffentlichen Zugriff vom AWS-Service-Dashboard
- CloudTrail-Ereignisname `PubliclyAccessible`
- Benachrichtigung von Security Researcher über den Zugang der Öffentlichkeit zu Ressourcen
- Löschen von Ressourcen aus der IP-Adresse des öffentlichen Internetprotokolls

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnis***_ und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Ausgenutzte Schwachstellen**
* **Exploits und Tools beobachtet**
* **Absicht des Schauspieler**
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
1. [**VORBEREITUNG**] Erstellen Sie ein Asset-Inventar
2. [**PREPARATION**] Erstellen eines RDS-Instanz-Inventars
3. [**PREPARATION**] RDS-Sicherheits- und Protokollierungsprüfungen einrichten
4. [**VORBEREITUNG**] Implementieren Sie ein Schulungsprogramm zur Identifizierung und Bewertung von RDS-Zugriffs- und Protokollanalyse
5. [**VORBEREITUNG**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren [**ERKENNUNG UND ANALYSE**]
6. [**ERKENNUNG UND ANALYSE**] Instanzprüfungen durchführen
7. [**ERKENNUNG UND ANALYSE**] CloudTrail für öffentliche RDS Resources
8. [**ERKENNUNG UND ANALYSE**] VPC Flow Logs überprüfen
9. [**ERKENNUNG UND ANALYSE**] Überprüfen Sie RDS Endpoint/Host-basierte Protokolle
10. [**CONTAINMENT**] RDS Public Exposure enthalten
11. [**ERADICATION**] Löschen Sie alle nicht erkannten oder nicht autorisierten öffentlichen Snapshots oder Datenbanken
12. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: RDS-Sicherheitsprüfungen
13. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Richtlinie zur Sicherheitskontrolle - RDS-Verschlüsselung
14. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: AWS Config
15. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Allgemeine Sicherheitslage

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Leitfaden zur Behandlung von Computersicherheitsvorfällen] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: AWS Service Public Access
* **Kategorie**: Öffentlicher Zugang
* **Ressourcen**: RDS
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken
* **Protokollquellen**: RDS-Datenbank-Protokolldateien, CloudTrail, CloudWatch
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
Identifizieren Sie alle vorhandenen Ressourcen und verfügen Sie über eine aktualisierte Asset-Inventarliste, die mit wem gehört

#### RDS-Instanzinventar
- Um RDS-Instanzen zu inventarisieren, verwenden Sie die AWS-API [describe-db-instances] (https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html), um die Namen aller Instanzen in einer bestimmten Region aufzulisten: `aws rds describe-db-instances —region us-east-1 —query 'dbInstances [*]. [DbInstanceIdentifier, readReplicaDbInstanceIdentifiers] '`

#### RDS-Sicherheits- und Protokollierungsüberprüfungen
- Überprüfen Sie, ob der Speicher der RDS-Instanzen verschlüsselt ist: `. /prowler -c extra735`
- Prüfen Sie, ob RDS-Instanzen Backup aktiviert haben: `. /prowler -c extra739`
- Prüfen Sie, ob RDS-Instanzen in CloudWatch Logs integriert sind: `. /prowler -c extra747`
- Stellen Sie sicher, dass RDS-Instanzen ein Upgrade für kleinere Versionen aktiviert haben: ` /prowler -c extra7131`
- Prüfen Sie, ob RDS-Instanzen [Enhanced Monitoring] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html) aktiviert hat: `. /prowler -c extra7132`
- RDS-Sicherheitsprüfungen: `. /prowler -g gruppe13`

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

### RDS-Instanzprüfungen

#### AWS Config
AWS Config verfügt über mehrere [verwaltete Regeln zur Bewertung Ihrer RDS-Instanzen] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* rds-automatik-minor-version-upgrade-aktiviert
* rds-cluster-lösch-schutz aktiviert
* rds-cluster-iam-authentifizierungs-aktiviert
* rds-cluster-multiaz-fähig
* rds-verbessertes Monitoring-aktiviert
* rds-instance-löschung-schutz aktiviert
* rds-instanz-iam-authentifizierungs-aktiviert
* rds-instance-öffentlich-zugriff-check
* rds-in-Backup-Plan
* rds-logging-aktiviert
* rds-multiaz-unterstützung
* rds-ressourcen-geschützt durch Backup-Plan
* rds-snapshots-public verboten
* rds-Snapshot-verschlüsselt
* rds-storage-verschlüsselt
#### Prowler
- Stellen Sie sicher, dass es keine öffentlich zugänglichen RDS-Instanzen gibt: `. /prowler -c extra78`
- Überprüfen Sie, ob RDS-Snapshots und Cluster-Snapshots öffentlich sind: `. /prowler -c extra723`
- Identifizieren Sie die Ressourcen in Ihrer Organisation und Ihren Konten, wie Amazon S3-Buckets oder IAM-Rollen, die mit einer externen Entität geteilt werden: `. /prowler -c extra769`
- Finden Sie Ressourcen, die dem Internet ausgesetzt sind: `. /prowler -g gruppe17`

## Eskalationsverfahren
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Analyse
Es wird dringend empfohlen, Protokolle in eine SIEM-Lösung (Security Incident Event Management) (wie Splunk, ELK-Stack usw.) zu exportieren, um bei der Anzeige und Analyse einer Vielzahl von Protokollen für eine vollständigere Angriffszeitachsenanalyse zu helfen.

### CloudTrail: RDS Public
1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Prüfen Sie CloudTrail-Protokolle auf den Ereignisnamen `modifydBInstance`, `modifyDbSnapshotatTribute` oder `modifyDbClusterSnapshotatTribute` und suchen Sie nach dem Wert `PubliclyAccessible` Ereignisse von öffentlichen IP-Adressen

### VPC Flow Logs
VPC Flow Logs ist eine Funktion, mit der Sie Informationen über den IP-Datenverkehr zu und von Netzwerkschnittstellen in Ihrer VPC erfassen können. Dies kann für in CloudTrail ermittelte IP-Adressen nützlich sein, um die Arten externer Verbindungen zu öffentlichen Ressourcen zu bestimmen.

Weitere Informationen und Schritte, einschließlich der Abfrage bei Athena, finden Sie in der [AWS-Dokumentation für VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Es wird empfohlen, die Athena-Analyse in ein separates Playbook aufzunehmen und mit anderen relaventen Elementen zu verknüpfen.

### Endpunkt/Host-basiert
- Überprüfen Sie das EC2-Betriebssystem- und Anwendungsprotokolle auf unangemessene Anmeldungen, Installation unbekannter Software oder das Vorhandensein nicht erkannter Dateien.

- Es wird dringend empfohlen, eine Host-basierte Intrusion Detection System (HIDS) -Lösung eines Drittanbieters zu haben (wie OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), andere)

## Eindämmung

### RDS Öffentliche Exposition
Wenn Sie eine DB-Instance innerhalb einer VPC starten, verfügt die DB-Instance über eine private IP-Adresse für den Datenverkehr innerhalb der VPC. Diese private IP-Adresse ist nicht öffentlich zugänglich. Sie können die Option Öffentlicher Zugriff verwenden, um anzugeben, ob die DB-Instance zusätzlich zur privaten IP-Adresse auch über eine öffentliche IP-Adresse verfügt.

Die folgende Abbildung zeigt die Option Öffentlicher Zugriff im Abschnitt Zusätzliche Konnektivitätskonfiguration. Um die Option festzulegen, öffnen Sie den Abschnitt Zusätzliche Konnektivitätskonfiguration im Abschnitt Konnektivität.

! [Bild] (/images/VPC-example.png)

## Ausrottung

### RDS Unautorisiert/Unerkannte Ressourcen
Löschen Sie alle nicht erkannten oder nicht autorisierten öffentlichen Snapshots oder Datenbanken

1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die Amazon RDS-Konsole unter https://console.aws.amazon.com/rds/
1. Wählen Sie im Navigationsbereich Snapshots aus.
1. Die Liste Manuelle Snapshots wird angezeigt.
1. Wählen Sie den zu löschenden DB-Snapshot aus.
1. Wählen Sie für Aktionen die Option Snapshot löschen aus.
1. Wählen Sie auf der Bestätigungsseite Löschen aus.

## Erholung
Gleiche Verfahren wie die für Tilgung aufgeführten

## Vorbeugende Maßnahmen

### RDS-Sicherheitsprüfungen
- RDS-Sicherheitsprüfungen: `. /prowler -g gruppe13`

### Richtlinie zur Sicherheitskontrolle: RDS-Verschlüsselung
[Obligatorische RDS-Verschlüsselung erzwingen] (https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) verfügt über mehrere automatisierte Regeln zum Schutz vor öffentlichem Zugriff, einschließlich [rds-snapshots-public-verboten] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)

### Allgemeine Sicherheitslage
Führen Sie eine [Self-Service-Sicherheitsbewertung] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) gegen die Umgebung durch, um andere Risiken und möglicherweise andere öffentliche Exposition, die in diesem Playbook nicht identifiziert wurden, weiter zu identifizieren.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook zur Minderung von Ressourcen, die falsch veröffentlicht wurden
- Als Incident Responder muss ich in der Lage sein, öffentliche Ressourcen (AMIs, EBS Volumes, ECR-Repos usw.) zu erkennen
- Als Incident Responder muss ich wissen, welche Rollen kritische Änderungen in AWS vornehmen können
- Als Incident Responder muss ich sicherstellen, dass alle Snapshots (RDS, EBS, ECR) eine Verschlüsselung benötigen

## Aktuelle Backlog-Artikel