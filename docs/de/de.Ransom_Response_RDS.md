# Incident Response Playbook: Lösegeldantwort für RDS
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt den Prozess für Reaktionen auf Ransom Angriffe auf Amazon Relational Database Service (RDS)

Weitere Informationen finden Sie im [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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
1. [**PREPARATION**] Verwenden Sie AWS Config, um die Konformität der Konfiguration anzuzeigen
2. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
3. [**CONTAINMENT**] Betroffene Ressourcen sofort isolieren
5. [**ERKENNUNG UND ANALYSE**] Verwenden Sie CloudWatch-Metriken, um festzustellen, ob Daten möglicherweise exfiltriert wurden
6. [**ERKENNUNG UND ANALYSE**] Verwenden Sie vPCFlowLogs, um unangemessenen Datenbankzugriff von externen IP-Adressen zu identifizieren
7. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsverfahren aus

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Leitfaden zur Behandlung von Computersicherheitsvorfällen] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Ransom & Data Destruction
* **Kategorie**: Lösegeldangriff
* **Ressourcen**: RDS
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken
* **Protokollquellen**: RDS-Datenbank-Protokolldateien, S3-Zugriffsprotokolle, CloudTrail, CloudWatch, AWS Config
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Handhabung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

## Vorbereitung
* Bewerten Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
* AWS hat ein neues Open Source-Tool zur Self-Service-Sicherheitsbewertung (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten konto.
* Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen einschließlich Server, Netzwerkgeräte, Netzwerk-/Dateifreigaben und Entwicklermaschinen
* Erwägen Sie die Verwendung von [AWS Config] (https://aws.amazon.com/config/), einem Service, mit dem Sie die Konfigurationen Ihrer AWS-Ressourcen bewerten, prüfen und bewerten können
* Erwägen Sie, [AWS GuardDuty] (https://aws.amazon.com/guardduty/) zu implementieren, um kontinuierlich auf bösartige Aktivitäten und nicht autorisiertes Verhalten zu überwachen, um Ihre AWS-Konten, Workloads und Daten zu schützen, die in Amazon RDS gespeichert sind
* [Führen Sie Ihre DB-Instance in einer Virtual Private Cloud (VPC) aus] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html) basierend auf dem Amazon VPC-Dienst für die größtmögliche Netzwerkzugriffskontrolle
* Verwenden Sie AWS Identity and Access Management (IAM) -Richtlinien, um Berechtigungen zuzuweisen, die bestimmen, wer Amazon RDS-Ressourcen verwalten darf. Sie können beispielsweise IAM verwenden, um festzustellen, wer DB-Instanzen erstellen, beschreiben, ändern und löschen, Ressourcen kennzeichnen oder Sicherheitsgruppen ändern darf.
* Verwenden Sie Sicherheitsgruppen, um zu steuern, welche IP-Adressen oder Amazon EC2-Instances eine Verbindung zu Ihren Datenbanken auf einer DB-Instance herstellen können. Wenn Sie zum ersten Mal eine DB-Instance erstellen, verhindert ihre Firewall jeglichen Datenbankzugriff, außer durch Regeln, die von einer zugehörigen Sicherheitsgruppe festgelegt wurden.
* [Secure Socket Layer (SSL) oder Transport Layer Security (TLS) -Verbindungen verwenden] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html) mit DB-Instanzen, auf denen die MySQL-, MariaDB-, PostgreSQL-, Oracle- oder Microsoft SQL Server-Datenbank-Engines ausgeführt werden
* [Verwenden Sie Amazon RDS-Verschlüsselung] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html), um Ihre DB-Instances und Snapshots im Ruhezustand zu sichern. Die Amazon RDS-Verschlüsselung verwendet den Industriestandard AES-256-Verschlüsselungsalgorithmus, um Ihre Daten auf dem Server zu verschlüsseln, der Ihre DB-Instance hostet
* [Netzwerkverschlüsselung verwenden] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) und [transparente Datenverschlüsselung] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html) mit Oracle DB-Instanzen
* Verwenden Sie die Sicherheitsfunktionen Ihrer DB-Engine, um zu steuern, wer sich in einer DB-Instance bei den Datenbanken anmelden kann. Diese Funktionen funktionieren genauso, als ob sich die Datenbank in Ihrem lokalen Netzwerk befand.
* Es wird dringend empfohlen, eine Host-basierte Intrusion Detection System (HIDS) -Lösung eines Drittanbieters zu haben (wie OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/))
* Weitere Referenzen und Schritte sind unter [Sicherheit in Amazon RDS] verfügbar (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)

### Verwenden Sie AWS Config, um die Konformität der Konfiguration anzuzeigen:
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die AWS Config-Konsole unter https://console.aws.amazon.com/config/
1. Stellen Sie im Menü der AWS Management Console sicher, dass die Regionsauswahl auf eine Region festgelegt ist, die AWS Config-Regeln unterstützt. Eine Liste der unterstützten Regionen finden Sie unter AWS Config Regions and Endpoints in der allgemeinen Referenz von Amazon Web Services
1. Wählen Sie im Navigationsbereich Ressourcen aus. Auf der Seite Ressourcenbestand können Sie nach Ressourcenkategorie, Ressourcentyp und Konformitätsstatus filtern. Wählen Sie gegebenenfalls Gelöschte Ressourcen einbeziehen aus. In der Tabelle werden die Ressourcenkennung für den Ressourcentyp und den Ressourcenkonformitätsstatus für diese Ressource angezeigt. Die Ressourcenkennung könnte eine Ressourcen-ID oder ein Ressourcenname sein
1. Wählen Sie eine Ressource aus der Spalte „Ressourcenkennung“
1. Wählen Sie die Schaltfläche „Ressourcen-Zeitleiste“. Sie können nach Konfigurationsereignissen, Compliance-Ereignissen oder CloudTrail-Ereignissen filtern
1. Konzentrieren Sie sich insbesondere auf die folgenden Ereignisse:
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
* rds-snapshots-public verboten
* rds-Snapshot-verschlüsselt
* rds-storage-verschlüsselt

## Eskalationsverfahren
- „Ich brauche eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll“
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Erkennung und Analyse
* [AWS GuardDuty Machine Learning] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) kann verdächtiges Verhalten erkennen, einschließlich Snapshots des Amazon Relational Database Service (Amazon RDS) im Rahmen von Datenexfiltrationsversuchen
* Überprüfen Sie das EC2-Betriebssystem- und Anwendungsprotokolle auf unangemessene Anmeldungen, Installation unbekannter Software oder das Vorhandensein nicht erkannter Dateien

### [CloudWatch-Metriken verwenden] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
Suchen Sie nach „Spikes“ der Datenexfiltration. Es ist möglich, dass ein Angreifer die Datenvernichtung durchführte und einen Lösegeldschein hinterlassen hat, und in diesen Fällen gibt es keine Möglichkeit zur Datenrettung durch die Zusammenarbeit mit dem böswilligen Akteur
1. Öffnen Sie die CloudWatch-Konsole unter https://console.aws.amazon.com/cloudwatch/
1. Wählen Sie im Navigationsbereich Metriken und dann Alle Metriken
1. Wählen Sie auf der Registerkarte Alle Metriken die Region aus, in der die Instanz bereitgestellt wird
1. Geben Sie auf der Registerkarte Alle Metriken den Suchbegriff `NetworkPacketSout` ein und drücken Sie die Eingabetaste
1. Wählen Sie eines der Ergebnisse für Ihre Suche aus, um die Metriken anzuzeigen
1. Um eine oder mehrere Metriken darzustellen, aktivieren Sie das Kontrollkästchen neben jeder Metrik. Um alle Metriken auszuwählen, aktivieren Sie das Kontrollkästchen in der Überschriftenzeile der Tabelle
1. (Optional) Um den Diagrammtyp zu ändern, wählen Sie Diagrammoptionen. Sie können dann zwischen einem Liniendiagramm, einem gestapelten Flächendiagramm, einem Balkendiagramm, einem Kreisdiagramm oder einer Zahl wählen
1. (Optional) Um ein Anomalieerkennungsband hinzuzufügen, das die erwarteten Werte für die Metrik anzeigt, wählen Sie das Symbol zur Anomalieerkennung unter Aktionen neben der Metrik

### Verwenden Sie vpcFlowLogs, um unangemessenen Datenbankzugriff von externen IP-Adressen zu identifizieren
1. Verwenden Sie den [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap), um Protokolldaten zu analysieren
1. Holen Sie sich eine Zusammenfassung mit der Anzahl der Bytes für jedes src_ip, src_port, dst_ip, dst_port Quad über alle Datensätze zu oder von einer bestimmten IP
```
SELECT sourceaddress, destinationaddress, sourceport, destinationport, sum (numbyte) als byte_count FROM vpcflow
WO (sourceaddress = '192.0.2.1' ODER destinationaddress = '192.0.2.1')
UND date_partition >= '2020/07/01'
UND date_partition <= '2020/07/31'
UND account_partition = '111122223333'
UND region_partition in („us-ost-1“, „us-ost--2“, „us-west-2“, „us-west-2“)
GROUP BY sourceaddress, destinationaddress, sourceport, destinationport
ORDER NACH byte_count DESC
```
1. Andere Beispielabfragen werden in der [vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql) bereitgestellt


## Erholung
* Es wird empfohlen, das Lösegeld nicht zu zahlen
* Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
* Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
* Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
* Besuchen Sie https://www.nomoreransom.org/, um festzustellen, ob ein Entschlüsseler für die Malware-Variante verfügbar ist, die Ihre Daten infiziert haben
* [Löschen oder Drehen von IAM-Benutzerschlüsseln] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); Sie können alle Schlüssel in Ihrem Konto drehen, wenn Sie einen bestimmten Schlüssel oder Schlüssel nicht identifizieren können, die freigelegt wurden
* [Unautorisierte IAM-Benutzer löschen] (https://console.aws.amazon.com/iam/home#users.)
* [Unautorisierte Richtlinien löschen] (https://console.aws.amazon.com/iam/home#/policies)
* [Unautorisierte Rollen löschen] (https://console.aws.amazon.com/iam/home#/roles)
* [temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden. HINWEIS: Das Löschen von IAM-Benutzern kann sich auf Produktions-Workloads auswirken und sollte mit Vorsicht durchgeführt werden
* Verwenden Sie Cloudendure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
* Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, ob die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her
* Löschen Sie alle nicht erkannten oder nicht autorisierten öffentlichen Snapshots oder Datenbanken
* Löschen Sie die kompromittierte Datenbank und erstellen Sie neue RDS-Datenbanken
* Identifizieren Sie EC2-Instanzen, die zulässigen Zugriff auf die Datenbank (en) hatten, und untersuchen Sie diese ebenfalls

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook, um EC2 Forensics durchzuführen
- Als Incident Responder brauche ich eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll
- Als Incident Responder muss ich die Protokollierung in allen Regionen aktiviert haben, die unabhängig von der Nutzungsabsicht aktiviert sind
- Als Incident Responder muss ich in der Lage sein, Krypto-Mining auf meinen bestehenden EC2-Instanzen zu erkennen

## Aktuelle Backlog-Artikel
