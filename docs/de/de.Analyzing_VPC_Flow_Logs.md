# Incident Response Playbook: Analysieren von VPC Flow Logs
Dieses Dokument wird nur zu Informationszwecken zur Verfügung gestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) ab dem Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2024 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird vorbehaltlich der Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“\
Genehmiger: `Name des Genehmigers“\
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt Beispielprozesse und Abfragen zur Analyse von AWS VPC Flow Logs.

## Vorbereitung
Dieses Playbook verweist und integriert sich, wenn möglich, in [Prowler] (https://github.com/toniblyx/prowler), einem Befehlszeilen-Tool, das Ihnen bei der AWS-Sicherheitsbewertung, -überwachung, -härtung und Reaktion auf Vorfälle hilft.

Es folgt den Richtlinien des CIS Amazon Web Services Foundations Benchmark (49 Schecks) und verfügt über mehr als 100 zusätzliche Kontrollen, einschließlich der DSGVO, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 und anderen.

Dieses Tool bietet eine schnelle Momentaufnahme des aktuellen Sicherheitszustands in einer Kundenumgebung. Darüber hinaus bietet [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) ein automatisiertes Compliance-Scannen und kann [in Prowler integrieren] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

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
[Sicherheitsperspektive des AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Richtlinie**
* **Detektiv**
* **Verantwortlich**
* **Präventiv**

! [Bild] (/images/aws_caf.png)
* *

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Tool: Athena
* **Kategorie**: Loganalyse
* **Ressource**: Athena
* **Indikatoren**: Cyber Threat Intelligence, Mitteilung Dritter
* **Protokollquellen**: VPC Flow Logs
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Handhabung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

### Antwortschritte
1. [**VORBEREITUNG**] Erstellen Sie ein öffentlich exponiertes Asset-Inventar
2. [**VORBEREITUNG**] Setup-Protokollierung
3. [**VORBEREITUNG**] Implementieren Sie ein Schulungsprogramm zur Identifizierung und Bewertung von Funktionen zur Protokollanalyse
4. [**ERKENNUNG UND ANALYSE**] VPC Flow Logs in der Konsole anzeigen
5. [**ERKENNUNG UND ANALYSE**] Flussprotokolle mit Amazon Athena abfragen
6. [**ERKENNUNG UND ANALYSE**] Erstellen Sie die VPC Flow Logs-Tabelle als DDL
7. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Erstellen Sie die Tabelle NEW Logs als DDL
8. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Prüfen Sie, ob es geladen ist und wie viele Zeilen sich in einer Datei befinden
9. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Prüfen Sie, ob sichtbare und nutzbare Daten zurückgegeben werden:
10. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Nach den Top-IPs suchen
11. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Standardanfragen zur Durchführung von Untersuchungen
12. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Datenverhältnis für Quell-/Ziel-IPs nachschlagen
13. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Suche nach bekannten IoCs
14. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Bedrohungstabelle
15. [**ERKENNUNG UND ANALYSE**] Beispiel Athena-Abfragen: Ansicht zum Ziel-Bedrohungstyp

## Vorbereitung
### Börtlich exponiertes Asset-
Sie können das [VPC Dashboard] (https://console.aws.amazon.com/vpc/home) und das [Network Manager-Dashboard] (https://console.aws.amazon.com/networkmanager/home) verwenden, mit denen Sie Ihre Netzwerkressourcen visualisieren und überwachen können. Das Network Manager-Dashboard enthält Informationen über die Ressourcen in Ihrem globalen Netzwerk, ihren geografischen Standort, die Netzwerktopologie, Amazon CloudWatch-Metriken und Ereignisse und ermöglicht Ihnen die Durchführung von Routenanalysen.

Sie sollten eine Bestandsaufnahme der folgenden Ressourcen führen und diese überwachen:
- Virtuelle Private Clouds (VPCs)
- VPC-Netzwerkzugriffskontrolllisten (NACLs)
- VPC-Sicherheitsgruppen
- VPC-Routentabellen
- VPC Elastic Network Interfaces (EnIs)
- VPC Internet-Gateways
- VPC-Peering-Verbindungen
- VPC NAT-Gateways
- VPC-Endpunkte
- VPN-Kunden-Gateways
- VPC Elastic IP-Adressen (EIPs)

Prowler kann Ihnen auch helfen, viele der Ressourcen zu finden, die dem Internet ausgesetzt sind:“. /prowler -g group17"

### Protokollierung einrichten
**Flow-Protokolle: **
- Flow-Protokolle erfassen Informationen über den IP-Datenverkehr, der zu und von Netzwerkschnittstellen in Ihrer VPC fließt.
- Sie können ein Flow-Log für eine VPC, ein Subnetz oder eine einzelne Netzwerkschnittstelle erstellen.
- Flow-Protokolldaten werden in CloudWatch Logs oder Amazon S3 veröffentlicht und können Ihnen helfen, übermäßig restriktive oder übermäßig zulässige Sicherheitsgruppen- und Netzwerk-ACL-Regeln zu diagnostizieren.

**So erstellen Sie ein Flow-Log für eine Netzwerkschnittstelle mit der Konsole**
1. Öffnen Sie die Amazon [EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Netzwerkschnittstellen aus.
1. Aktivieren Sie die Kontrollkästchen für eine oder mehrere Netzwerkschnittstellen und wählen Sie Aktionen, Create flow log.
1. Geben Sie unter Filter die Art des zu protokollierenden Datenverkehrs an.
- Wählen Sie Alle aus, um akzeptierten und abgelehnten Datenverkehr zu protokollieren, Ablehnen, um nur abgelehnten Datenverkehr zu protokollieren, oder Akzeptieren, um nur
1. Wählen Sie für Maximales Aggregationsintervall den maximalen Zeitraum aus, in dem ein Flow erfasst und in einem Flow-Log-Datensatz zusammengefasst wird.
1. Wählen Sie für Ziel Send to an Amazon S3 bucket aus.
1. Geben Sie für S3 bucket ARN den Amazon Resource Name (ARN) eines vorhandenen Amazon S3 S3-Buckets an.
- Sie können einen Unterordner in den Bucket ARN aufnehmen. Der Bucket kann AWSLogs nicht als Unterordnernamen verwenden, da dies ein reservierter Begriff ist.
- Um beispielsweise einen Unterordner namens my-logs in einem Bucket namens my-bucket anzugeben, verwenden Sie den folgenden ARN: arn:aws:s3። :my-bucket/my-logs/
- Wenn Sie den Bucket besitzen, erstellen wir automatisch eine Ressourcenrichtlinie und hängen sie an den Bucket an. Weitere Informationen finden Sie unter Amazon S3 S3-Bucket-Berechtigungen für Flow-Logs.
1. Wählen Sie für Log-Datensatzformat das Format für den Flow-Log-Datensatz aus.
* Um das Standardformat zu verwenden, wählen Sie das AWS default format.
* Um ein benutzerdefiniertes Format zu verwenden, wählen Sie Benutzerdefiniertes Format und wählen Sie dann Felder aus dem Protokollformat aus.
1. Wählen Sie Create flow log aus.

**So erstellen Sie ein Flow-Log für eine VPC oder ein Subnetz mit der Konsole**
1. Öffnen Sie die Amazon [VPC-Konsole] (https://console.aws.amazon.com/vpc/)
1. Wählen Sie im Navigationsbereich Ihre VPCs oder wählen Sie Subnets aus.
1. Aktivieren Sie das Kontrollkästchen für ein oder mehrere VPCs oder Subnetze und wählen Sie dann Aktionen, Create flow log.
1. Geben Sie unter Filter die Art des zu protokollierenden Datenverkehrs an.
- Wählen Sie Alle aus, um akzeptierten und abgelehnten Datenverkehr zu protokollieren, Ablehnen, um nur abgelehnten Datenverkehr zu protokollieren, oder Akzeptieren, um nur
1. Wählen Sie für Maximales Aggregationsintervall den maximalen Zeitraum aus, in dem ein Flow erfasst und in einem Flow-Log-Datensatz zusammengefasst wird.
1. Wählen Sie für Ziel Send to an Amazon S3 bucket aus.
1. Geben Sie für S3 bucket ARN den Amazon Resource Name (ARN) eines vorhandenen Amazon S3 S3-Buckets an.
- Sie können einen Unterordner in den Bucket ARN aufnehmen. Der Bucket kann AWSLogs nicht als Unterordnernamen verwenden, da dies ein reservierter Begriff ist.
- Um beispielsweise einen Unterordner namens my-logs in einem Bucket namens my-bucket anzugeben, verwenden Sie den folgenden ARN: „arn:aws:s3። :my-bucket/my-logs/
- Wenn Sie den Bucket besitzen, erstellen wir automatisch eine Ressourcenrichtlinie und hängen sie an den Bucket an. Weitere Informationen finden Sie unter Amazon S3 S3-Bucket-Berechtigungen für Flow-Logs.
1. Wählen Sie für Log-Datensatzformat das Format für den Flow-Log-Datensatz aus.
* Um das Standardformat zu verwenden, wählen Sie das AWS default format.
* Um ein benutzerdefiniertes Format zu verwenden, wählen Sie Benutzerdefiniertes Format und wählen Sie dann Felder aus dem Protokollformat aus.
1. Wählen Sie Create flow log aus.

**NAT-Gateways überwachen: ** Sie können Ihr NAT-Gateway mit CloudWatch überwachen, das Informationen von Ihrem NAT-Gateway sammelt und lesbare Metriken in der Nähe von Echtzeit-Metriken erstellt.
1. NAT-Gateway-Metriken werden in Intervallen von 1 Minute an CloudWatch gesendet.
1. Sie können die Metriken für Ihre NAT-Gateways wie folgt anzeigen.
* Öffnen Sie die [Amazon CloudWatch-Konsole] (https://console.aws.amazon.com/cloudwatch/)
* Wählen Sie im Navigationsbereich Metriken aus.
* Wählen Sie unter Alle Metriken den Namespace der NAT-Gateway-Metrik aus.
* Um die Metriken anzuzeigen, wählen Sie die Metrik-Dimension aus.
1. So erstellen Sie einen Alarm für ausgehenden Datenverkehr über das NAT-Gateway:
* Öffnen Sie die [Amazon CloudWatch-Konsole] (https://console.aws.amazon.com/cloudwatch/)
* Wählen Sie im Navigationsbereich Alarme, Alarm erstellen aus.
* Wählen Sie NAT-Gateway.
* Wählen Sie das NAT-Gateway und die Metrik „BytesOutToDestination“ aus und wählen Sie Weiter.
* Konfigurieren Sie den Alarm wie folgt und wählen Sie Alarm erstellen, wenn Sie fertig sind:
* Geben Sie unter Alarmschwelle einen Namen und eine Beschreibung für Ihren Alarm ein. Wählen Sie für Wann immer >= und geben Sie 5000000 ein. Geben Sie 1 für die aufeinanderfolgenden Perioden ein.
* Wählen Sie unter Aktionen eine vorhandene Benachrichtigungsliste aus oder wählen Sie Neue Liste aus, um eine neue zu erstellen.
* Wählen Sie unter Alarmvorschau einen Zeitraum von 15 Minuten aus und geben Sie eine Summenstatistik an.
1. So erstellen Sie einen Alarm zur Überwachung von Portzuweisungsfehlern
* Öffnen Sie die [Amazon CloudWatch-Konsole] (https://console.aws.amazon.com/cloudwatch/)
* Wählen Sie im Navigationsbereich Alarme, Alarm erstellen aus.
* Wählen Sie NAT Gateway.
* Wählen Sie das NAT-Gateway und die Metrik „ErrorPortAllocation“ aus und wählen Sie Weiter.
* Konfigurieren Sie den Alarm wie folgt und wählen Sie Alarm erstellen, wenn Sie fertig sind:
* Geben Sie unter Alarmschwelle einen Namen und eine Beschreibung für Ihren Alarm ein. Wählen Sie für Wann immer > und geben Sie 0 ein. Geben Sie 3 für die aufeinanderfolgenden Perioden ein.
* Wählen Sie unter Aktionen eine vorhandene Benachrichtigungsliste aus oder wählen Sie Neue Liste aus, um eine neue zu erstellen.
* Wählen Sie unter Alarmvorschau einen Zeitraum von 5 Minuten aus und geben Sie eine Statistik von Maximum an.

### Schulung
- `Welche Schulung gibt es für Analysten innerhalb des Unternehmens, um sich mit der AWS API (Befehlszeilenumgebung), S3, RDS und anderen AWS-Services vertraut zu machen? `
>>>
Zu den Möglichkeiten zur Bedrohungserkennung und zur Reaktion auf Vorfälle gehören:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `Welche Rollen können Änderungen an Diensten in Ihrem Konto vornehmen? `
- `Welchen Benutzern haben diese Rollen ihnen zugewiesen? Wird das geringste Privileg befolgt oder gibt es Super-Admin-Benutzer? `
- `Wurde eine Sicherheitsbewertung für Ihre Umgebung durchgeführt, haben Sie eine bekannte Baseline, um „neue“ oder „verdächtige“ Dinge zu erkennen? `

## Erkennung und Analyse
### Anzeigen von VPC Flow Logs in der Konsole
**So zeigen Sie Informationen über Flow-Logs für Ihre Netzwerkschnittstellen an**
1. Öffnen Sie die Amazon [EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Netzwerkschnittstellen aus.
1. Wählen Sie eine Netzwerkschnittstelle aus und wählen Sie Flow Logs. Informationen zu den Flow-Logs werden auf der Registerkarte angezeigt. Die Spalte Zieltyp gibt das Ziel an, an dem die Flow-Logs veröffentlicht werden.

**So zeigen Sie Informationen über Flow-Logs für Ihre VPCs oder Subnetze an**
1. Öffnen Sie die [Amazon VPC-Konsole] (https://console.aws.amazon.com/vpc/)
1. Wählen Sie im Navigationsbereich Ihre VPCs oder Subnets aus.
1. Wählen Sie Ihre VPC oder Ihr Subnetz aus und wählen Sie Flow Logs.
- Informationen zu den Flow-Logs werden auf der Registerkarte angezeigt.
- Die Spalte Zieltyp gibt das Ziel an, an dem die Flow-Logs veröffentlicht werden.

**So zeigen Sie Flow-Log-Datensätze an, die auf Amazon S3** veröffentlicht wurden
1. Öffnen Sie die [Amazon S3 S3-Konsole] (https://console.aws.amazon.com/s3/)
1. Wählen Sie unter Bucket-Name den Bucket aus, in dem die Flow-Logs veröffentlicht werden.
1. Aktivieren Sie für Name das Kontrollkästchen neben der Protokolldatei. Wählen Sie im Objektübersichtsfenster die Option Herunterladen aus.

### Abfragen von Flow-Logs mit Amazon Athena
Amazon Athena ist ein interaktiver Abfrageservice, mit dem Sie Daten in Amazon S3, wie z. B. Ihre Flow-Logs, mithilfe von Standard-SQL analysieren können. Sie können Athena mit VPC Flow Logs verwenden, um schnell umsetzbare Einblicke in den Datenverkehr zu erhalten, der durch Ihre VPC fließt. Sie können beispielsweise ermitteln, welche Ressourcen in Ihren Virtual Private Clouds (VPCs) die Top-Talker sind, oder die IP-Adressen mit den am meisten abgelehnten TCP-Verbindungen identifizieren.

Sie können die Integration Ihrer VPC-Flussprotokolle mit Athena rationalisieren und automatisieren, indem Sie eine CloudFormation-Vorlage erstellen, die die erforderlichen AWS-Ressourcen und vordefinierten Abfragen erstellt, die Sie ausführen können, um Einblicke in den Datenverkehr zu erhalten, der durch Ihre VPC fließt.

1. Die CloudFormation-Vorlage erstellt die folgenden Ressourcen:
- Eine Athena-Datenbank. Der Datenbankname lautet vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>.
- Eine Athena-Arbeitsgruppe. Der Arbeitsgruppenname ist <flow-log-subscription-id><partition-load-frequency><start-date><end-date>Workgroup
- Eine partitionierte Athena-Tabelle, die Ihren Flow-Log-Datensätzen entspricht. Der Tabellenname lautet <flow-log-subscription-id><partition-load-frequency><start-date><end-date>.
- Eine Reihe von Athena benannten Abfragen. Weitere Informationen finden Sie unter Vordefinierte Abfragen.
- Eine Lambda-Funktion, die neue Partitionen nach dem angegebenen Zeitplan (täglich, wöchentlich oder monatlich) in die Tabelle lädt.
- Eine IAM-Rolle, die die Berechtigung zum Ausführen der Lambda-Funktionen erteilt.
1. Voraussetzungen
- Sie müssen eine Region auswählen, die AWS Lambda und Amazon Athena unterstützt.
- Die Amazon S3 S3-Buckets müssen sich in der ausgewählten Region befinden.
1. Preisgestaltung
- Für das Ausführen von Abfragen fallen Standardgebühren für Amazon Athena an.
- Für die Lambda-Funktion, die neue Partitionen nach einem wiederkehrenden Zeitplan lädt, fallen Standardgebühren für AWS Lambda-Gebühren an (wenn Sie eine Partitionslastfrequenz angeben, aber kein Start- und Enddatum angeben).
1. Führen Sie einen der folgenden Schritte aus:
- Öffnen Sie die Amazon VPC-Konsole. Wählen Sie im Navigationsbereich Ihre VPCs aus und wählen Sie dann Ihre VPC aus.
- Öffnen Sie die Amazon VPC-Konsole. Wählen Sie im Navigationsbereich Subnets aus und wählen Sie dann Ihr Subnetz aus.
- Öffnen Sie die Amazon EC2 EC2-Konsole. Wählen Sie im Navigationsbereich Netzwerkschnittstellen aus und wählen Sie dann Ihre Netzwerkschnittstelle aus.
1. Wählen Sie auf der Registerkarte Flow-Protokolle ein Flow-Log aus, das in Amazon S3 veröffentlicht wird, und wählen Sie dann Aktionen, Athena-Integration generieren aus.
1. Geben Sie die Lastfrequenz der Partition an.
- Wenn Sie Keine auswählen, müssen Sie das Start- und Enddatum der Partition unter Verwendung von Datumsangaben angeben, die in der Vergangenheit liegen.
- Wenn Sie Täglich, Wöchentlich oder Monatlich wählen, sind das Start- und Enddatum der Partition optional.
- Wenn Sie kein Start- und Enddatum angeben, erstellt die CloudFormation-Vorlage eine Lambda-Funktion, die neue Partitionen nach einem wiederkehrenden Zeitplan lädt.
1. Wählen oder erstellen Sie einen S3-Bucket für die generierte Vorlage und einen S3-Bucket für die Abfrageergebnisse.
1. Wählen Sie Athena-Integration generieren aus.
1. (Optional) Wählen Sie in der Erfolgsnachricht den Link aus, um zu dem Bucket zu navigieren, den Sie für die CloudFormation-Vorlage angegeben haben, und passen Sie die Vorlage an.
1. Wählen Sie in der Erfolgsmeldung den Befehl CloudFormation-Stack erstellen aus, um den Assistenten zum Erstellen von Stack in der AWS CloudFormation-Konsole zu öffnen.
- Die URL für die generierte CloudFormation-Vorlage wird im Abschnitt Vorlage angegeben.
- Schließen Sie den Assistenten aus, um die in der Vorlage angegebenen Ressourcen zu erstellen.

**Eine vordefinierte Abfrage ausführen**

Die generierte CloudFormation-Vorlage enthält eine Reihe vordefinierter Abfragen, die Sie ausführen können, um schnell aussagekräftige Einblicke in den Datenverkehr in Ihrem AWS-Netzwerk zu erhalten. Nachdem Sie den Stack erstellt und überprüft haben, dass alle Ressourcen korrekt erstellt wurden, können Sie eine der vordefinierten Abfragen ausführen.

1. So führen Sie eine vordefinierte Abfrage mit der Konsole aus
- Öffne die Athena-Konsole. Wählen Sie im Arbeitsgruppenbedienfeld die von der CloudFormation-Vorlage erstellte Arbeitsgruppe aus.
- Wählen Sie eine der vordefinierten Abfragen aus, ändern Sie die Parameter nach Bedarf und führen Sie dann die Abfrage aus.
- Öffnen Sie die Amazon S3 S3-Konsole. Navigieren Sie zu dem Bucket, den Sie für die Abfrageergebnisse angegeben haben, und zeigen Sie die Ergebnisse der Abfrage an.

Im Folgenden sind die von der generierten CloudFormation-Vorlage bereitgestellten Abfragen mit dem Namen Athena aufgeführt:
* vpcFlowLogsAcceptedTraffic — Die TCP-Verbindungen, die basierend auf Ihren Sicherheitsgruppen und Netzwerk-ACLs zulässig waren.
* vpcFlowLogSadminportTraffic — Der auf den Ports der administrativen Web-App aufgezeichnete Datenverkehr.
* VpcFlowLogsIPv4Traffic — Die Gesamtbytes des aufgezeichneten IPv4-Datenverkehrs.
* VpcFlowLogsIPv6Traffic — Die Gesamtbytes des aufgezeichneten IPv6-Datenverkehrs.
* vpcFlowLogsRejectedTCPTraffic — Die TCP-Verbindungen, die basierend auf Ihren Sicherheitsgruppen oder Netzwerk-ACLs abgelehnt wurden.
* vpcFlowLogsRejectedTraffic — Der Datenverkehr, der basierend auf Ihren Sicherheitsgruppen oder Netzwerk-ACLs abgelehnt wurde.
* VpcFlowLogsSshRdpTraffic — Der SSH- und RDP-Datenverkehr.
* VpcFlowLogsTopTalkers — Die 50 IP-Adressen mit dem meisten aufgezeichneten Datenverkehr.
* VpcFlowLogsTopTalkersPacketLevel — Die 50 IP-Adressen auf Paketebene mit dem meisten aufgezeichneten Datenverkehr.
* VpcFlowLogsTopTalkingInstances — Die IDs der 50 Instanzen mit dem meisten aufgezeichneten Datenverkehr.
* VpcFlowLogsTopTalkingSubnets — Die IDs der 50 Subnetze mit dem meisten aufgezeichneten Datenverkehr.
* vpcFlowLogStopTCPTraffic — Der gesamte TCP-Datenverkehr, der für eine Quell-IP-Adresse aufgezeichnet wurde.
* VpcFlowLogsTotalBytesTransferred — Die 50 Paare von Quell- und Ziel-IP-Adressen mit den meisten aufgezeichneten Bytes.
* VpcFlowLogsTotalBytesTransferredPacketLevel — Die 50 Paare von Quell- und Ziel-IP-Adressen auf Paketebene mit den meisten aufgezeichneten Bytes.
* VpcFlowLogsTrafficFrmSrcAddr — Der Datenverkehr, der für eine bestimmte Quell-IP-Adresse aufgezeichnet wurde.
* VpcFlowLogsTrafficToDstAddr — Der Datenverkehr, der für eine bestimmte Ziel-IP-Adresse aufgezeichnet wurde

## Deep Dive Analysis:

Referenz: https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html

### Erstellen Sie die VPC Flow Logs-Tabelle als DDL
Der folgende Code wird verwendet, um die Tabelle als DDL für die VPC Flow Logs zu erstellen, die untersucht werden. (Bitte beachten Sie die TODO-Artikel inline):
```
/*
Erstellt eine VPC-Flusstabelle für VPC-Flussprotokolle, die direkt an einen S3-Bucket geliefert werden
Umfasst die Partitionierungskonfiguration für Multi-Account-, Multi-Regions-Bereitstellung sowie Datum im JJJJ/MM/TT-Format
Schließt alle verfügbaren Felder ein, einschließlich der nicht standardmäßigen v3- und v4-Felder.
HINWEIS: VPC-Flussfelder müssen in der Reihenfolge konfiguriert werden, die bei der Konfiguration aufgeführt ist. Wenn sie in einer anderen Reihenfolge konfiguriert wurden, müssen Sie die Reihenfolge anpassen, die in der DDL unten aufgeführt ist
*/

/*
TODO: Aktualisieren Sie optional den Tabellennamen „vpcflow“ auf den Namen, den Sie für die VPC-Flow-Protokolltabelle verwenden möchten
*/
EXTERNE TABELLE ERSTELLEN WENN NICHT EXISTIERT vpcflow (
/*
TODO: Stellen Sie sicher, dass VPC-Flow-Protokolle so konfiguriert wurden, dass sie in dieser Reihenfolge generiert werden. Wenn dies nicht der Fall war, müssen Sie die folgende Reihenfolge neu anordnen, um der Reihenfolge zu entsprechen, in der sie generiert wurden
TIPP: Laden Sie ein Beispiel herunter und überprüfen Sie die erste Zeile jeder Protokolldatei für die Feldreihenfolge,
Machen Sie sich keine Sorgen, wenn die Feldnamen nicht genau mit den Namen in der obersten Zeile des Protokolls übereinstimmen, importiert Athena sie basierend auf ihrer Reihenfolge
HINWEIS: Diese v2-Felder haben das Standardformat und müssen normalerweise nicht angepasst werden. Achten Sie genauer auf die Reihenfolge der unten stehenden v3/v4/v5-Felder, da es keine Standardreihenfolge dieser Felder gibt. Felder und Beschreibungen finden Sie unter https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html
*/
Version int,
Konto-Zeichenfolge,
InterfaceId-Zeichenfolge,
sourceadress-Zeichenfolge,
Zieladress-Zeichenfolge,
sourceport int,
destinationport int,
protokoll int,
numpakete int,
Numbytes Bigint,
starttime int,
Endzeit int,
Aktionszeichenfolge,
Logstatus-Zeichenfolge,
/*
HINWEIS: Beginn der nicht standardmäßigen v3- und v4-Felder
mach dir keine Sorgen, wenn deine Quellprotokolle nicht alle diese Felder enthalten, sie werden nur als leer angezeigt
TODO: Wenn Ihre VPC-Flow-Protokolle diese Felder enthalten, überprüfen Sie, ob die Reihenfolge, in der sie unten aufgeführt sind, dieselbe Reihenfolge entspricht, in der sie im Flow-Log-Format konfiguriert sind
*/
vpcid-Zeichenfolge,
typ string,
tcpflags Smallint,
subnetid string,
sublocationtype string,
sublocationid string,
regions-String,
pktsrcaddr-Zeichenfolge,
pktdstaddr String,
instanzeid-Zeichenfolge,
azid String,
/*
HINWEIS: Beginn von nicht standardmäßigen v5-Feldern
mach dir keine Sorgen, wenn deine Quellprotokolle nicht alle diese Felder enthalten, sie werden nur als leer angezeigt
TODO: Wenn Ihre VPC-Flow-Protokolle diese Felder enthalten, überprüfen Sie, ob die Reihenfolge, in der sie unten aufgeführt sind, dieselbe Reihenfolge entspricht, in der sie im Flow-Log-Format konfiguriert sind
*/
pkt-src-aws-Service-String,
pkt-dst-aws-dienstzeichenfolge,
String für Fließrichtung,
verkehrspfad int
)
PARTITIONIERT VON
(
date_partition STRING,
region_partition STRING,
account_partition STRING
)
ZEILENFORMAT GETRENNT
FELDER, DIE DURCH '' BEENDET WURDEN
/*
TODO: ersetzt bucket_name und optional_prefix im LOCATION-Wert,
wenn es kein Präfix gibt, dann entferne das Extra/
Beispiel: s3: //my_Central_Log_Bucket/awsLogs/ oder s3: //my_Central_Log_Bucket/prod/awslogs/
*/
STANDORT 's3://<bucket_name>/<optional_prefix>/awslogs/ '
TBLPROPERTIES
(
„skip.header.line.count"="1",
„projection.enabled“ = „true“,
„projection.date_partition.type“ = „Datum“,
/* TODO: ersetze <YYYY><MM>//<DD>mit dem ersten Datum deiner Logs, Beispiel: 2020/11/30 */
„projection.date_partition.range“ = "<YYYY>/<MM>/<DD>, JETZT“,
„projection.date_partition.format“ = „JJJJ/MM/TT“,
„projection.date_partition.interval“ = „1",
„projection.date_partition.interval.unit“ = „TAGE“,
„projection.region_partition.type“ = „enum“,
„projection.region_partition.values“ = „us-ost-2, us-ost-1, us-west-1, us-west-2, af-Süd-1, ap-Ost-1, ap-Süd-1, ap-Nordost-3, ap-Nordost-2, ap-zentral-1, ap-südöst-2, ap-nordost-1, cn-nordost-1, cn-nordost-1, cn-nordost-1, cn-nordost-1 west-1, eu-zentral-1, EU-West-1, EU-West-2, EU-Süd-1, EU-West-3, EU-Nord-1, mich-Süd-1, sa-Ost-1",
„projection.account_partition.type“ = „enum“,
/*
TODO: Ersetzen Sie Werte in projection.account_partition.values durch die Liste der AWS-Kontonummern, die Sie in diese Tabelle aufnehmen möchten
Beispiel: „0123456789.0123456788.0123456777"
note: Verwenden Sie keine Leerzeichen, trennen Sie die Werte nur mit einem Komma (einschließlich Leerzeichen verursacht einen Syntaxfehler)
wenn es nur ein Konto gibt, schließen Sie es selbst ohne Komma ein, zum Beispiel: „0123456789"
*/
„projection.account_partition.values“ = "<account_num_1>,<account_num_2>, ... „,
/*
TODO: Ersetzen Sie wie LOCATION bucket_name und optional_prefix im Wert von storage.location template,
wenn es kein Präfix gibt, dann entferne das Extra/
Stellen Sie sicher, dass nur die Werte innerhalb der Klammern <> ersetzt werden, die Werte innerhalb der {} Klammern Variablen sind und wie unten definiert belassen werden müssen.
Beispiel: s3: //my_Central_Log_Bucket/awsLogs/... oder s3: //my_Central_Log_Bucket/prod/awslogs/...
*/
„storage.location template“ = „s3://<bucket_name>/<optional_prefix>/awsLogs/ $ {account_partition} /vpcflowlogs/$ {region_partition} /$ {date_partition}“
);
```
### Beispielabfragen:
#### Erstellen Sie die Tabelle NEW Logs als DDL
Erstellen Sie eine neue Athena-Tabelle, indem Sie die folgende Abfrage ausführen. Denken Sie daran:

* Ersetzen Sie <bucket_name>durch den Ziel-Bucket.
* Überprüfen Sie den STANDORT, um sicherzustellen, dass er alle relevanten Bucket-Präfixe und den nachfolgenden Schrägstrich („/“) enthält
```
EXTERNE TABELLE ERSTELLEN WENN NICHT EXISTIERT vpc_flow_logs (
is_rejected_packet STRING,
avg_in_packet_size INT,
vpc_id STRING,
local_port INT,
source_dc STRING,
ip STRING,
end_time STRING,
remote_port INT,
start_time STRING,
Protokoll INT,
in_bytes BIGINT,
source_tag STRING,
account_id STRING,
instance_id STRING,
interface_public_ips STRING,
out_bytes BIGINT,
is_skipped_rejected_packets STRING,
source_az STRING,
source_region STRING,
avg_out_packet_size INT,
instance_type STRING,
interface_local_ips STRING
)
ZEILENFORMAT SERDE 'org.Openx.Data.jsonSerde.jsonSerde'
MIT SERDEPROPERTIES ('ignore.malformed.json' = 'true')
STANDORT 's3://<bucket_name>/';
```
#### Prüfen Sie, ob es geladen ist und wie viele Zeilen in der Datei enthalten sind:
```
WÄHLEN Sie count (*) AUS vpc_flow_logs;
```
#### Prüfen Sie, ob sichtbare und nutzbare Daten zurückgegeben werden:
```
WÄHLEN SIE * AUS vpc_flow_logs Limit 1000;
```
#### Nach den Top-IPs suchen
```
wählen Sie ip, count (*) cnt
von vpc_flow_logs
gruppieren nach ip
sortieren nach cnt desc
begrenzen 100;

Nehmen Sie einige Top-IPs und bestellen Sie nach Zeit (tauschen Sie die IP unten aus)

wähle * aus vpc_flow_logs
wo ip = '123.123.123.123'
sortieren nach start_time;
```

### „S3-Gateway/ VPC Interface Notes“
Für „VPC Endpoints“ werden Verbindungen in den Flow-Logs als Datenverkehr über ihre privaten Adressen angezeigt

S3 Gateway wird in den Flow-Protokollen als private Adresse angezeigt, die sich mit öffentlichen Bereichen in der Präfixliste verbindet

### Standardanfragen zur Durchführung von Untersuchungen

#### Nachschlagen des Datenverhältnisses für Quell-/Ziel-IPs
Die folgende Abfrage aggregiert VPC Flow Log-Datensätze nach IP, Datensatzanzahl (Gesamtzahl der Datensätze derselben Quell- oder Zieladresse) und Gesamtbytes (Gesamtbytes der Kommunikation zwischen Quell- oder Zieladresse). Die Abfrage gibt auch das Datenverhältnis an, nämlich wie viele Bytes pro Kommunikation gesendet werden (Gesamtbytes geteilt durch Datensatzanzahl).
```
SELECT sourceaddress,
Zieladresse,
count (*) ALS record_count,
SUM (Numbyte) ALS total_Bytes,
SUMME (Numbyte) /count (*) ALS data_ratio,
date_format (from_unixtime (min (starttime)),
'%Y/%m/%d%H: %i: %s') AS first_seen, date_format (from_unixtime (max (Endzeit)), '%Y/%m/%d%H: %i: %s') WIE zuletzt gesehen
VON "<database>„.“ <table>“
WO (sourceaddress = '10.10.10.10'
ODER destinationaddress = '10.10.10.10')
GRUPPIEREN NACH sourceaddress, zieladresse
SORTIEREN NACH total_bytes DESC
```
#### Auf der Suche nach bekannten IoCs
Die folgende Abfrage geht davon aus, dass IoCs externe IP-Adressen sind, und verwendet eine Nachschlagetabelle, um diese zu verwalten

#### Bedrohungstabelle
```
EXTERNE TABELLE ERSTELLEN WENN NICHT EXISTIERT incident_iocs (
threat_ip-Zeichenfolge,
threat_comment-Zeichenfolge
)

ZEILENFORMAT SERDE
'org.apache.hadoop.hive.serde2.opencsvserde'

MIT SERDEPROPERTIES (
'separatorChar' = ', ',
'quoteChar' = '\ "',
'EscapeChar' = '\\'
)
ALS TEXTFILE GESPEICHERT
STANDORT 's3: //bucket-analysis/iocs/iocfilehere/'
```
#### Lookup-Ansicht zum Ziel-Bedrohungstyp
```
ERSTELLEN SIE ANSICHT incident_vpc_flow_directional
WÄHLEN Sie vpc.account aus,
vpc.interfaceid,
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) ALS Quelle,
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) ALS Ziel,
'INBOUND' AS-Richtung,
vpc.sourceaddress AS connecting_ip,
vpc.sourceport ALS connecting_port,
vpc.protokoll,
vpc.numpakete,
vpc.numbytes,
vpc.action,
vpc.logstatus,
CAST (from_unixtime (vpc.starttime) ALS ZEITSTEMPEL) ALS StartTime,
CAST (from_unixtime (vpc.endtime) ALS ZEITSTEMPEL) ALS EndTime,
date_diff ('Sekunde', from_unixtime (vpc.starttime), from_unixtime (vpc.endtime)) als SessionTime

VON "<database>„.“ <table>"WIE vpc

WOHER
vpc.destinationaddress IN (WÄHLEN Sie threat_ip FROM incident_iocs)

VEREINIGUNG ALLE

WÄHLEN Sie vpc.account aus,
vpc.interfaceid,
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) ALS Quelle,
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) ALS Ziel,
'OUTBOUND' AS-Richtung,
vpc.destinationaddress AS connecting_ip,
vpc.destinationport AS connecting_port,
vpc.protokoll,
vpc.numpakete,
vpc.numbytes,
vpc.action,
vpc.logstatus,
CAST (from_unixtime (vpc.starttime) ALS ZEITSTEMPEL) ALS StartTime,
CAST (from_unixtime (vpc.endtime) ALS ZEITSTEMPEL) ALS EndTime,
date_diff ('Sekunde', from_unixtime (vpc.starttime), from_unixtime (vpc.endtime)) als SessionTime

VON "<database>„.“ <table>"WIE vpc
WOHER
vpc.sourceaddress IN (WÄHLEN Sie threat_ip AUS incident_iocs)

SORTIEREN NACH StartTime, EndTime, SessionTime, interfaceid, connecting_ip, richtung
```

## Angesprochene Backlog-Elemente
- Als Incident Responder muss ich in der Lage sein, alle kritischen VPC-Umgebungen zu überwachen
- Als Incident Responder brauche ich ein Playbook zum Abfragen von VPC FlowLogs im großen Maßstab

## Aktuelle Backlog-Artikel