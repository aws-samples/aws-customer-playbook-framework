# Incident Response Playbook: Exposition öffentlicher Ressourcen - S3
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

## Zusammenfassung
In diesem Playbook wird der Prozess beschrieben, um Eigentümer öffentlicher Ressourcen zu identifizieren, festzustellen, wer auf diese zugegriffen hat, während sie exponiert wurden, die Auswirkungen des Widerrufens des Zugriffs auf die Ressource zu ermitteln und die Ursache für die öffentliche Zugänglichkeit zu ermitteln.

## Mögliche Indikatoren für Kompromisse
- Warnung für öffentlichen Zugriff vom AWS-Service-Dashboard
- CloudTrail `getPublicAccessBlock`, `deletePublicAccessBlock`, `getObjectacl`, `putObjectCl`
- Benachrichtigung von Security Researcher über den Zugang der Öffentlichkeit zu Ressourcen
- Löschen von Ressourcen aus der IP-Adresse des öffentlichen Internetprotokolls

## Mögliche AWS GuardDuty-Ergebnisse
- Entdeckung:S3/maliciousipCaller
- Entdeckung:S3/MaliciousipCaller.Custom
- Entdeckung:S3/ToriPCaller
- Exfiltration:S3/maliciousipCaller
- Exfiltration:S3/ObjectRead.Ungewöhnlich
- Auswirkungen: S3/maliciousipCaller
- Pentest:S3/Kalilinux
- pentest:S3/ParrotLinux
- Pentest:S3/Pentoolinux
- policy:S3/AccountBlockPublicAccessDisabled
- Policy:S3/BucketAnonymousAccessGranted
- policy:S3/BucketBlockPublicAccessDisabled
- Policy:S3/BucketPublicAccessGranted
- stealth:S3/ServerAccessLoggingDisabled
- UnauthorizedAccess:S3/MaliciousipCaller.Custom
- UnauthorizedAccess:S3/toripCaller

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnisse und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Ausgenutzte Schwachstellen**
* **Exploits und Tools beobachtet**
* **Absicht des Schauspieler**
* **Zuordnung des Schauspieler**
* **Umweltschäden und Geschäfts**

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
1. [**VORBEREITUNG**] Konto erstellen Asset Inventar
2. [**VORBEREITUNG**] Erstellen Sie ein S3-Bucket-Inventar
3. [**PREPARATION**] Logging nach Bedarf aktivieren
4. [**PREPARATION**] Identifizieren Sie, welche Art von Daten sich in jedem Bucket befindet
5. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
6. [**VORBEREITUNG**] Implementieren Sie Schulungen zur Behebung von DOS/DDoS-Angriffen
7. [**ERKENNUNG UND ANALYSE**] Führen Sie S3-Bucket-Prüfungen durch
8. [**ERKENNUNG UND ANALYSE**] CloudTrail überprüfen: Public S3 Bucket
9. [**ERKENNUNG UND ANALYSE**] CloudTrail überprüfen: Public S3-Objekt
10. [**ERKENNUNG UND ANALYSE**] VPC Flow Logs überprüfen
11. [**ERKENNUNG UND ANALYSE**] Endpoint überprüfen/Host-basiert
12. [**EINDÄMMUNG**] S3 öffentlichen Zugang blockieren
13. [**ERADICATION**] S3 Unerkannte /Unautorisierte Objekte entfernen
14. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsverfahren durch

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Leitfaden zur Behandlung von Computersicherheitsvorfällen] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: AWS Service Public Access
* **Kategorie**: Öffentlicher Zugang
* **Ressource**: S3
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken
* **Protokollquellen **: S3-Serverprotokolle, S3-Zugriffsprotokolle, CloudTrail, CloudWatch
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

Dieses Tool bietet eine schnelle Momentaufnahme des aktuellen Sicherheitszustands in einer Kundenumgebung. Darüber hinaus bietet [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) ein automatisiertes Compliance-Scannen und kann [in Prowler integrieren] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventar für Vermögenswerte
Identifizieren Sie alle vorhandenen Ressourcen und verfügen Sie über eine aktualisierte Asset-Inventarliste, die mit wem gehört

#### S3-Eimer-Inventar
- Verwenden Sie die AWS-API [list-buckets] (https://docs.aws.amazon.com/cli/latest/reference/s3api/list-buckets.html), um die Namen aller Ihrer Amazon S3-Buckets (über alle Regionen hinweg) anzuzeigen: `aws s3api list-buckets —query „Buckets [] .Name"`

#### Protokollierung aktivieren
- Prüfen Sie, ob S3-Buckets die Protokollierung auf Serverebene in CloudTrail aktiviert haben: `. /prowler -c extra718`
- Überprüfen Sie, ob S3-Buckets die Protokollierung auf Objektebene in CloudTrail aktiviert haben: `. /prowler -c extra725`

#### Identifizieren Sie, welche Art von Daten sich in jedem Bucket befindet
**Option A**
- Um alle Dateien eines S3-Buckets zu sehen, benutze den Befehl: `aws s3 ls s3: //your_bucket_name —recursive`
- **HINWEIS** Dieser API-Aufruf ist auf die ersten 1000 Übereinstimmungen beschränkt und enthält keine Objekte über diesem Schwellenwert
- Kategorisieren Sie manuell, welche Buckets und Objekte für das Unternehmen wichtig sind
- Fügen Sie kritischen Buckets Tags hinzu, damit Metadaten für zukünftige Referenzen vorhanden sind

**Option B**
- [Amazon Macie] (https://aws.amazon.com/macie/) ist ein vollständig verwalteter Datensicherheits- und Datenschutzdienst, der maschinelles Lernen und Musterabgleich verwendet, um Ihre sensiblen Daten in AWS zu erkennen und zu schützen. Amazon Macie verwendet maschinelles Lernen und Musterabgleich, um sensible Daten kosteneffizient im großen Maßstab zu erkennen.

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

### S3-Bucket-Prüfungen
- Stellen Sie sicher, dass die S3-Bucket-Zugriffsprotokollierung im CloudTrail S3-Bucket aktiviert ist: `. /prowler -c check26`
- Stellen Sie sicher, dass keine S3-Buckets für alle oder jeden AWS-Benutzer geöffnet sind: `. /prowler -c extra73`
- Identifizieren Sie die Ressourcen in Ihrer Organisation und Ihren Konten, wie Amazon S3-Buckets oder IAM-Rollen, die mit einer externen Entität geteilt werden: `. /prowler -c extra769`
- Finden Sie Ressourcen, die dem Internet ausgesetzt sind: `. /prowler -g gruppe17`

## Eskalationsverfahren
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Analyse
Es wird dringend empfohlen, Protokolle in eine SIEM-Lösung (Security Incident Event Management) (wie Splunk, ELK-Stack usw.) zu exportieren, um bei der Anzeige und Analyse einer Vielzahl von Protokollen für eine vollständigere Angriffszeitachsenanalyse zu unterstützen.

### CloudTrail: Öffentlicher S3-Bucket
Standardmäßig protokolliert CloudTrail API-Aufrufe, die in den letzten 90 Tagen getätigt wurden, jedoch keine Anfragen an Objekte protokollieren. Sie können Ereignisse auf Bucket-Ebene auf der CloudTrail-Konsole sehen. Sie können dort jedoch keine Datenereignisse (Anrufe auf Amazon S3-Objektebene) anzeigen — Sie müssen CloudTrail-Protokolle dafür analysieren oder abfragen.

1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail-Protokolle für die Eventnamen `getPublicAccessBlock` und `DeletePublicAccessBlock`

### CloudTrail: Öffentliches S3-Objekt
Sie können auch CloudTrail-Protokolle für Amazon S3-Aktionen auf Objektebene abrufen. Aktivieren Sie dazu Datenereignisse für Ihren S3-Bucket oder alle Buckets in Ihrem Konto. Wenn in Ihrem Konto eine Aktion auf Objektebene auftritt, wertet CloudTrail Ihre Trail-Einstellungen aus. Wenn das Ereignis mit dem Objekt übereinstimmt, das Sie in einem Trail angegeben haben, wird das Ereignis protokolliert.

1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail-Protokolle für die Eventnamen `getObjectCl` und `PutObjectCl`

### VPC Flow Logs
VPC Flow Logs ist eine Funktion, mit der Sie Informationen über den IP-Datenverkehr zu und von Netzwerkschnittstellen in Ihrer VPC erfassen können. Dies kann für in CloudTrail ermittelte IP-Adressen nützlich sein, um die Arten externer Verbindungen zu öffentlichen Ressourcen zu bestimmen.

Weitere Informationen und Schritte, einschließlich der Abfrage bei Athena, finden Sie in der [AWS-Dokumentation für VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Es wird empfohlen, die Athena-Analyse in ein separates Playbook aufzunehmen und mit anderen relaventen Elementen zu verknüpfen.

### Endpunkt/Host-basiert
1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail auf `putObject` und `DeleteObject` Anfragen von öffentlichen IP-Adressen

- Überprüfen Sie das EC2-Betriebssystem- und Anwendungsprotokolle auf unangemessene Anmeldungen, Installation unbekannter Software oder das Vorhandensein nicht erkannter Dateien.

- Es wird dringend empfohlen, eine Host-basierte Intrusion Detection System (HIDS) -Lösung eines Drittanbieters zu haben (wie OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), andere)

## Eindämmung

### S3 öffentlichen Zugriff blockieren
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration „blockPublicacls=True, ignoRepublicaCls=True, blockPublicPolicy=True, restrictPublicBuckets=True“

Sie können auch [Blockieren des öffentlichen Zugriffs auf Ihren Amazon S3-Speicher] (https://aws.amazon.com/s3/features/block-public-access/) für zusätzliche Informationen zum Sperren des öffentlichen S3-Zugriffs in Ihrem Konto überprüfen.

## Ausrottung

### S3 Unerkannte /Unautorisierte Objekte entfernen
Entferne alle nicht erkannten Objekte aus Buckets

1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die Amazon S3-Konsole unter https://console.aws.amazon.com/s3/
1. Wählen Sie in der Liste Bucket-Name den Namen des Buckets aus, aus dem Sie ein Objekt löschen möchten.
1. Wählen Sie den Namen des Objekts aus, das Sie löschen möchten.
1. Um die aktuelle Version des Objekts zu löschen, wählen Sie Neueste Version und wählen Sie das Papierkorbsymbol.
1. Um eine frühere Version des Objekts zu löschen, wählen Sie Neueste Version und wählen Sie das Papierkorbsymbol neben der Version, die Sie löschen möchten.

## Erholung
Gleiche Verfahren wie die für Tilgung aufgeführten

## Vorbeugende Maßnahmen

### S3 Prowler Schecks
**Verschlüsselung**
- Prüfen Sie, ob S3-Buckets die Standardverschlüsselung (SSE) aktiviert haben: `. /prowler -c extra734`

**Notfallwiederherstellung**
- Prüfen Sie, ob S3-Buckets die Objektversionierung aktiviert haben: `. /prowler -c extra763`

### S3-Dienstaktionen
[Verhindern Sie, dass Benutzer die Einstellungen für den öffentlichen Zugriff S3 ändern] (https://asecure.cloud/a/scp_s3_block_public_access/)

Überprüfen Sie regelmäßig den Bucket-Zugriff und die Richtlinien monatlich und nutzen Sie [CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) oder Security Hub für automatisierte Erkennungen

[Versionierung in S3-Buckets verwenden] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html), um versehentliches oder absichtliches Löschen von Objekten der obersten Ebene zu mildern

[Zugriff mit ACLs verwalten] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) zur Beschränkung des unbefugten Zugriffs auf Ressourcen auf Bucket- und Objektebene

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) verfügt über mehrere automatisierte Regeln zum Schutz vor öffentlichem Zugriff, einschließlich [öffentlicher Zugriff auf s3-Bucket-Ebene] ( https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html).

### Allgemeine Sicherheitslage
Führen Sie eine [Self-Service-Sicherheitsbewertung] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) gegen die Umgebung durch, um andere Risiken und möglicherweise andere öffentliche Exposition, die in diesem Playbook nicht identifiziert wurden, weiter zu identifizieren.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook zur Minderung von Ressourcen, die falsch veröffentlicht wurden
- Als Incident Responder muss ich in der Lage sein, öffentliche Ressourcen zu erkennen (AMIs, EBS Volumes, ECR-Repos usw.)
- Als Incident Responder muss ich wissen, welche Rollen kritische Änderungen in AWS vornehmen können
- Als Incident Responder brauche ich ein Playbook zur Minderung einer öffentlichen Bucket-Exposition und erforderliche Eskalationspunkte
- Als Incident Responder benötige ich Dokumentation zu Protokollen, die für verschiedene Bucket-Klassifikationen

## Aktuelle Backlog-Artikel