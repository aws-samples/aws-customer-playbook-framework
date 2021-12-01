# Incident Response Playbook: Nicht autorisierte Netzwerkänderungen
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt den Prozess zur Reaktion auf nicht autorisierte oder verdächtige Änderungen an Netzwerkressourcen in einer AWS-Umgebung.

## Mögliche Indikatoren für Kompromisse
- Neue oder nicht erkannte Serviceressourcen
- Unerkannte oder nicht autorisierte Ressourcen (z. B. EC2, Lambda)
- Ungewöhnliche Abrechnungssteigerungen
- Benachrichtigung von Sicherheitsforschern
- Benachrichtigung, dass meine AWS-Ressourcen oder mein Konto möglicherweise gefährdet sind

## Mögliche AWS GuardDuty-Ergebnisse
- Backdoor:EC2/C & CActivity.B
- Backdoor:EC2/C&CActivity.B! DNS
- Backdoor:EC2/SpamBot
- Verhalten: EC2/NetworkportUngewöhnlich
- Verhalten: EC2/TrafficVolumeUngewöhnlich
- Kryptowährung:EC2/BitcoinTool.B
- Kryptowährung:EC2/BitcoinTool.B! DNS
- Auswirkung:EC2/abusedDomainRequest.Reputation
- Auswirkung:EC2/BitcoinDomainRequest.Reputation
- Auswirkung:EC2/böswilligeDomainRequest.Reputation
- Auswirkung:EC2/verdächtigDomainRequest.Reputation
- Auswirkungen: EC2/WinrmbruteForce
- recon:EC2/PortProbeemrunProtectedPort
- recon:EC2/portProbeUnprotectedPort
- Trojan:EC2/Blackholetraffic
- Trojan:EC2/BlackholeTraffic! DNS
- Trojan:EC2/DgadomainRequest.B
- Trojan:EC2/DgadomainRequest.C! DNS
- Trojan:EC2/DNSDataExfiltration
- Trojan:EC2/DriveBySourceTraffic! DNS
- Trojan:EC2/DropPoint
- Trojan:EC2/DropPoint! DNS
- Trojan:EC2/PhishingDomainRequest! DNS
- UnauthorizedAccess:EC2/MaliciousipCaller.Custom
- UnauthorizedAccess:ec2/metadatadnsrebind
- UnauthorizedAccess:EC2/rdpBruteForce
- UnauthorizedAccess:EC2/sshBruteForce
- UnauthorizedAccess:EC2/TorClient
- UnauthorizedAccess:EC2/TorRelay
- Entdeckung:S3/maliciousipCaller
- CredentialAccess:iamuser/anomalousBehavior
- defenseevasion:iamuser/anomalousBehavior
- Entdeckung:Iamuser/AnomalousBehavior
- Exfiltration:Iamuser/AnomalousBehavior
- Auswirkung: Iamuser/AnomalousBehavior
- initialAccess:iamuser/anomalousBehavior
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
1. [**VORBEREITUNG**] Öffentlich exponierter Vermögensbestand
2. [**VORBEREITUNG**] Setup-Protokollierung
3. [**VORBEREITUNG**] Implementieren Sie ein Schulungsprogramm zur Identifizierung und Bewertung von Cloud-Forensik-Funktionen
4. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
5. [**ERKENNUNG UND ANALYSE**] Überprüfen Sie CloudTrail
6. [**ERKENNUNG UND ANALYSE**] Überprüfen Sie CloudWatch
7. [**ERKENNUNG UND ANALYSE**] Verwenden Sie AWS Config, um die Konfiguration einer Sicherheitsgruppe zu überprüfen
8. [**ERKENNUNG UND ANALYSE**] VPC Flow Logs in der Konsole anzeigen
9. [**ERKENNUNG UND ANALYSE**] Flussprotokolle mit Amazon Athena abfragen
10. [**CONTAINMENT**] Identifizieren Sie, welcher Benutzer oder welche Rolle die nicht autorisierten Netzwerkänderungen ausgeführt hat
11. [**ERADICATION**] Überprüfen Sie die Ergebnisse aus dem CloudTrail-Ereignisverlauf überprüfen auf Aktivitäten anhand des kompromittierten Zugriffsschlüssels
12. [**ERADIKATION**] Überprüfen Sie die Vermeidung unerwarteter Anklagen
13. [**WIEDERHERSTELLUNG**] Stellen Sie alle Änderungen wieder her, die an Netzwerkressourcen aufgetreten sind
14. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: AWS Config und AWS System Manager
15. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: AWS Security Hub
16. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Allgemeine Sicherheitslage

*** Die Antwortschritte folgen dem Lebenszyklus der Incident Response von NIST-Sonderpublikation 800-61r2
[NIST-Handbuch zur Handhabung von Vorfällen zur Computersicherheit] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
! [Bild] (images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Tools: Forensik
* **Kategorie**: Forensik
* **Ressourcen**: EC2, VPC
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken
* **Quellen**: Endpunkt
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

### öffentlich exponiertes Asset-Bestand
Sie können das [VPC Dashboard] (https://console.aws.amazon.com/vpc/home) und das [Network Manager Dashboard] (https://console.aws.amazon.com/networkmanager/home) verwenden, mit denen Sie Ihre Netzwerkressourcen visualisieren und überwachen können. Das Network Manager-Dashboard enthält Informationen über die Ressourcen in Ihrem globalen Netzwerk, ihren geografischen Standort, die Netzwerktopologie, Amazon CloudWatch-Metriken und Ereignisse und ermöglicht Ihnen die Durchführung von Routenanalysen.

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

Prowler kann Ihnen auch helfen, viele der Ressourcen zu finden, die dem Internet ausgesetzt sind: `. /prowler -g gruppe17`

### Protokollierung einrichten
**Flow-Protokolle: **
- Flow-Protokolle erfassen Informationen über den IP-Datenverkehr, der zu und von Netzwerkschnittstellen in Ihrer VPC fließt.
- Sie können ein Flow-Log für eine VPC, ein Subnetz oder eine einzelne Netzwerkschnittstelle erstellen.
- Flow-Protokolldaten werden in CloudWatch Logs oder Amazon S3 veröffentlicht und können Ihnen helfen, übermäßig restriktive oder übermäßig zulässige Sicherheitsgruppen- und Netzwerk-ACL-Regeln zu diagnostizieren.

**So erstellen Sie ein Flow-Log für eine Netzwerkschnittstelle mit der Konsole**
1. Öffnen Sie die Amazon [EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Netzwerkschnittstellen aus.
1. Aktivieren Sie die Kontrollkästchen für eine oder mehrere Netzwerkschnittstellen und wählen Sie Aktionen, Flowlog erstellen.
1. Geben Sie unter Filter die Art des zu protokollierenden Datenverkehrs an.
- Wählen Sie Alle aus, um akzeptierten und abgelehnten Datenverkehr zu protokollieren, Ablehnen, um nur abgelehnten Datenverkehr zu protokollieren, oder Akzeptieren, um nur
1. Wählen Sie für Maximales Aggregationsintervall den maximalen Zeitraum aus, in dem ein Flow erfasst und in einem Flow-Log-Datensatz aggregiert wird.
1. Wählen Sie für Ziel An einen Amazon S3-Bucket senden aus.
1. Geben Sie für S3-Bucket-ARN den Amazon-Ressourcennamen (ARN) eines vorhandenen Amazon S3-Buckets an.
- Sie können einen Unterordner in den Bucket ARN aufnehmen. Der Bucket kann AWSLogs nicht als Unterordnernamen verwenden, da dies ein reservierter Begriff ist.
- Um beispielsweise einen Unterordner namens my-logs in einem Bucket namens my-bucket anzugeben, verwenden Sie den folgenden ARN: `arn:aws:s3። :my-bucket/my-logs/`
- Wenn Sie den Bucket besitzen, erstellen wir automatisch eine Ressourcenrichtlinie und hängen sie an den Bucket an. Weitere Informationen finden Sie unter Amazon S3-Bucket-Berechtigungen für Flow-Logs.
1. Wählen Sie für Log-Datensatzformat das Format für den Flow-Log-Datensatz aus.
* Um das Standardformat zu verwenden, wählen Sie das AWS-Standardformat.
* Um ein benutzerdefiniertes Format zu verwenden, wählen Sie Benutzerdefiniertes Format und wählen Sie dann Felder aus dem Protokollformat aus.
1. Wählen Sie Flow-Log erstellen aus.

**So erstellen Sie ein Flow-Log für eine VPC oder ein Subnetz mit der Konsole**
1. Öffnen Sie die Amazon [VPC-Konsole] (https://console.aws.amazon.com/vpc/)
1. Wählen Sie im Navigationsbereich Ihre VPCs oder wählen Sie Subnetze aus.
1. Aktivieren Sie das Kontrollkästchen für ein oder mehrere VPCs oder Subnetze und wählen Sie dann Aktionen, Flow-Log erstellen.
1. Geben Sie unter Filter die Art des zu protokollierenden Datenverkehrs an.
- Wählen Sie Alle aus, um akzeptierten und abgelehnten Datenverkehr zu protokollieren, Ablehnen, um nur abgelehnten Datenverkehr zu protokollieren, oder Akzeptieren, um nur
1. Wählen Sie für Maximales Aggregationsintervall den maximalen Zeitraum aus, in dem ein Flow erfasst und in einem Flow-Log-Datensatz aggregiert wird.
1. Wählen Sie für Ziel An einen Amazon S3-Bucket senden aus.
1. Geben Sie für S3-Bucket-ARN den Amazon-Ressourcennamen (ARN) eines vorhandenen Amazon S3-Buckets an.
- Sie können einen Unterordner in den Bucket ARN aufnehmen. Der Bucket kann AWSLogs nicht als Unterordnernamen verwenden, da dies ein reservierter Begriff ist.
- Um beispielsweise einen Unterordner namens my-logs in einem Bucket namens my-bucket anzugeben, verwenden Sie den folgenden ARN: `arn:aws:s3። :my-bucket/my-logs/`
- Wenn Sie den Bucket besitzen, erstellen wir automatisch eine Ressourcenrichtlinie und hängen sie an den Bucket an. Weitere Informationen finden Sie unter Amazon S3-Bucket-Berechtigungen für Flow-Logs.
1. Wählen Sie für Log-Datensatzformat das Format für den Flow-Log-Datensatz aus.
* Um das Standardformat zu verwenden, wählen Sie das AWS-Standardformat.
* Um ein benutzerdefiniertes Format zu verwenden, wählen Sie Benutzerdefiniertes Format und wählen Sie dann Felder aus dem Protokollformat aus.
1. Wählen Sie Flow-Log erstellen aus.

**NAT-Gateways überwachen: ** Sie können Ihr NAT-Gateway mit CloudWatch überwachen, das Informationen von Ihrem NAT-Gateway sammelt und lesbare Metriken in der Nähe von Echtzeit erstellt.
1. NAT-Gateway-Metriken werden in Intervallen von 1 Minute an CloudWatch gesendet.
1. Sie können die Metriken für Ihre NAT-Gateways wie folgt anzeigen.
* Öffnen Sie die CloudWatch-Konsole unter https://console.aws.amazon.com/cloudwatch/
* Wählen Sie im Navigationsbereich Metriken aus.
* Wählen Sie unter Alle Metriken den Namespace der NAT-Gateway-Metrik aus.
* Um die Metriken anzuzeigen, wählen Sie die Metrik-Dimension aus.
1. So erstellen Sie einen Alarm für ausgehenden Datenverkehr über das NAT-Gateway:
* Öffnen Sie die CloudWatch-Konsole unter https://console.aws.amazon.com/cloudwatch/
* Wählen Sie im Navigationsbereich Alarme, Alarm erstellen aus.
* Wählen Sie NAT-Gateway.
* Wählen Sie das NAT-Gateway und die ByteSoutToDestination Metrik aus und wählen Sie Weiter.
* Konfigurieren Sie den Alarm wie folgt und wählen Sie Alarm erstellen, wenn Sie fertig sind:
* Geben Sie unter Alarmschwelle einen Namen und eine Beschreibung für Ihren Alarm ein. Wählen Sie für Wann immer >= und geben Sie 5000000 ein. Geben Sie 1 für die aufeinanderfolgenden Perioden ein.
* Wählen Sie unter Aktionen eine vorhandene Benachrichtigungsliste aus oder wählen Sie Neue Liste aus, um eine neue zu erstellen.
* Wählen Sie unter Alarmvorschau einen Zeitraum von 15 Minuten aus und geben Sie eine Summenstatistik an.
1. So erstellen Sie einen Alarm zur Überwachung von Portzuweisungsfehlern
* Öffnen Sie die CloudWatch-Konsole unter https://console.aws.amazon.com/cloudwatch/
* Wählen Sie im Navigationsbereich Alarme, Alarm erstellen aus.
* Wählen Sie NAT Gateway.
* Wählen Sie das NAT-Gateway und die ErrorPortallocation-Metrik aus und wählen Sie Weiter.
* Konfigurieren Sie den Alarm wie folgt und wählen Sie Alarm erstellen, wenn Sie fertig sind:
* Geben Sie unter Alarmschwelle einen Namen und eine Beschreibung für Ihren Alarm ein. Wählen Sie für Wann immer > und geben Sie 0 ein. Geben Sie 3 für die aufeinanderfolgenden Perioden ein.
* Wählen Sie unter Aktionen eine vorhandene Benachrichtigungsliste aus oder wählen Sie Neue Liste aus, um eine neue zu erstellen.
* Wählen Sie unter Alarmvorschau einen Zeitraum von 5 Minuten aus und geben Sie eine Statistik von Maximum an.

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

### CloudTrail
1. Navigieren Sie zu Ihrem [CloudTrail-Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. Wählen Sie im linken Rand `Ereignishistorie`
1. Wechseln Sie im Dropdown-Menü von `Nur-Lesen` zu `Ereignisname`
1. Überprüfen Sie CloudTrail-Protokolle auf die Eventnamen `AuthorizeSecurityGroupingRess`, `revokeSecurityGroupingRess`, `AuthorizeSecurityGroupeDress` und `revokeSecurityGroupeGress`

### CloudWatch
Erstellen Sie eine CloudWatch-Ereignisregel, die mit der CloudWatch-Konsole bei einem Ereignis ausgelöst wird

1. Öffnen Sie die CloudWatch-Konsole.
1. Wählen Sie im Navigationsbereich unter Ereignisse die Option Regeln aus, und wählen Sie dann Regel erstellen aus.
1. Wählen Sie das Ereignismuster aus.
1. Wählen Sie für Dienstname EC2 aus.
1. Wählen Sie für Ereignistyp AWS API-Aufruf über CloudTrail aus.
1. Wählen Sie Spezifische Operation und geben Sie die folgenden API-Aufrufe an. Diese API-Aufrufe werden verwendet, um Sicherheitsgruppenregeln hinzuzufügen oder zu entfernen.
- AuthorizeSecurityGroupinGress
- AuthorizeSecurityGroupeGress
- revokeSecurityGroupinGress
- revokeSecurityGroupeGress
1. Diese Einstellungen erzeugen das folgende Ereignismuster.
```json
{
„source“: [
„aws.ec2"
],
„Detailtyp“: [
„AWS API-Aufruf über CloudTrail“
],
„Detail“: {
„EventSource“: [
„ec2.amazonaws.com“
],
„eventName“: [
„AuthorizeSecurityGroupinGress“,
„AuthorizeSecurityGroupeGress“,
„revokeSecurityGroupinGress“,
„revokeSecurityGroupeGress“
]
}
}
```
1. Wählen Sie Ziel hinzufügen aus.
1. Wählen Sie in der Liste der Ziele das SNS-Thema aus.
1. Wählen Sie für Topic ein vorhandenes Thema aus, um ein neues zu erstellen.
1. Wählen Sie Details konfigurieren aus.
1. Geben Sie auf der Seite Regeldetails konfigurieren einen Namen und eine optionale Beschreibung ein.
- Lassen Sie für Status das Feld Aktiviert ausgewählt.
1. Wählen Sie Regel erstellen aus.

### Verwenden Sie AWS Config, um die Konfiguration einer Sicherheitsgruppe zu überprüfen
Wenn Sie planen, AWS Config und AWS Config Rules zu verwenden, legen Sie zunächst fest, ob der Service reaktiv oder proaktiv genutzt werden soll.
- Um AWS Config reaktiv zu verwenden, aktivieren Sie einfach den Service und er erstellt einen Bestand an AWS-Ressourcen und eine Historie von Änderungen.
- Um AWS Config für proaktive Änderungsbenachrichtigungen zu verwenden, führen Sie die folgenden Aufgaben aus:
1. Erstellen Sie ein Amazon SNS-Thema, um Benachrichtigungen über Konfigurationsänderungen zu erhalten.
1. Entwickeln Sie AWS Lambda-Funktionen oder EC2-basierte SQS-Nachrichtenprozessoren, um netzwerkspezifische Änderungen zu identifizieren und entsprechende Benachrichtigungen zu senden.
1. Berücksichtigen Sie mindestens zwei verschiedene Empfänger für Änderungsbenachrichtigungen:
- Eine Netzwerksicherheits-E-Mail oder eine Paging-Verteilerliste zum Empfangen von Benachrichtigungen, die ein sofortiges menschliches Eingreifen erfordern (z. B. das Ändern einer Sicherheitsgruppe, um allen Zugriff global zu ermöglichen oder ein Internet-Gateway an eine reine interne VPC anzuhängen).
- Ein zentrales Protokollierungs- oder Änderungsmanagementsystem, das verwendet werden kann, um genehmigte Änderungsanträge mit tatsächlichen Konfigurationsänderungen abzustimmen.
1. Abonnieren Sie Ihre AWS Lambda-Funktion (en) oder SQS-Warteschlangen für das SNS-Thema Änderungsbenachrichtigung.
1. Konfigurieren Sie AWS Config für die Veröffentlichung von Änderungsbenachrichtigungen für das SNS-Thema zur Änderungs

Es folgt ein Beispiel für die Verwendung von AWS Config in Kombination mit einer Lambda-Funktion:

1. Erstellen Sie eine [Lambda-Ausführungsrolle] (http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) und eine [AWS Config-Rolle] (http://docs.aws.amazon.com/config/latest/developerguide/iamrole-permissions.html).
1. Ermöglichen Sie AWS Config, die Konfiguration von Sicherheitsgruppen aufzuzeichnen, damit AWS Config Sicherheitsgruppen auf Änderungen an ihren Konfigurationen überwachen kann. Der folgende Screenshot zeigt ein Beispiel für die Einstellungen.
Screenshot zeigt ein Beispiel für die Konfigurationseinstellungen
1. [Erstellen Sie eine Sicherheitsgruppe] (http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html), die Ports ermöglicht, die sich auf Ihre Geschäftsanforderungen beziehen (z. B. 80/443 für HTTP, 25 für SMTP)
1. Erstellen Sie die Python-Lambda-Funktion mit dem [Code aus der AWS Config-Regelbibliothek auf GitHub] (https://github.com/awslabs/aws-config-rules/blob/master/python/ec2_security_group_ingress.py).
- Der Lambda-Funktionscode definiert eine Liste mit dem Namen REQUIRED_PERMISSIONS mit Elementen, die ein Protokoll, einen Portbereich und einen IP-Bereich darstellen, die zusammen eine Sicherheitsberechtigung definieren. In diesem Fall werden nur die Ports 80 und 443 autorisiert.
- Passen Sie diese Lmabda-Funktion mit den Ports und IP-Bereichen an, die für Ihre Geschäftsanforderungen erforderlich sind.
1. [Erstellen Sie die AWS Config-Regel] (https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html#creating-a-custom-rule-with-the-AWS-Config-console) mit der Lambda-Funktion, die Sie in Schritt 4 erstellt haben.
- Wählen Sie für Trigger-Typ die Option Konfigurationsänderungen aus.
- Wählen Sie für Umfang der Änderungen EC2: SecurityGroup und geben Sie dann die ID der Sicherheitsgruppe ein, die Sie in Schritt 3 erstellt haben.
1. Führen Sie die Config-Regel aus. Dies wird die Regel für die Ausführung in die Warteschlange stellen, und die Regel sollte in etwa 10 Minuten bis zum Abschluss ausgeführt werden.
1. Überprüfen Sie die Sicherheitsgruppe, die Sie in Schritt 3 erstellt haben.

## Eskalationsverfahren
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Analyse
### Anzeigen von VPC-Flow-Protokollen in der Konsole
**So zeigen Sie Informationen über Flow-Logs für Ihre Netzwerkschnittstellen an**
1. Öffnen Sie die Amazon [EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Netzwerkschnittstellen aus.
1. Wählen Sie eine Netzwerkschnittstelle aus und wählen Sie Flow Logs. Informationen zu den Flow-Logs werden auf der Registerkarte angezeigt. Die Spalte Zieltyp gibt das Ziel an, an dem die Flow-Logs veröffentlicht werden.

**So zeigen Sie Informationen über Flow-Logs für Ihre VPCs oder Subnetze an**
1. Öffnen Sie die Amazon VPC-Konsole unter https://console.aws.amazon.com/vpc/
1. Wählen Sie im Navigationsbereich Ihre VPCs oder Subnetze aus.
1. Wählen Sie Ihre VPC oder Ihr Subnetz aus und wählen Sie Flow Logs.
- Informationen zu den Flow-Logs werden auf der Registerkarte angezeigt.
- Die Spalte Zieltyp gibt das Ziel an, an dem die Flow-Logs veröffentlicht werden.

**So zeigen Sie Flow-Log-Datensätze an, die auf Amazon S3** veröffentlicht wurden
1. Öffnen Sie die Amazon S3-Konsole unter https://console.aws.amazon.com/s3/
1. Wählen Sie unter Bucket-Name den Bucket aus, in dem die Flow-Logs veröffentlicht werden.
1. Aktivieren Sie für Name das Kontrollkästchen neben der Protokolldatei. Wählen Sie im Objektübersichtsfenster die Option Herunterladen aus.

### Abfragen von Flow-Logs mit Amazon Athena
Amazon Athena ist ein interaktiver Abfrageservice, mit dem Sie Daten in Amazon S3, z. B. Ihre Flow-Logs, mithilfe von Standard-SQL analysieren können. Sie können Athena mit VPC Flow Logs verwenden, um schnell umsetzbare Einblicke in den Datenverkehr zu erhalten, der durch Ihre VPC fließt. Sie können beispielsweise ermitteln, welche Ressourcen in Ihren Virtual Private Clouds (VPCs) die Top-Talker sind, oder die IP-Adressen mit den am meisten abgelehnten TCP-Verbindungen identifizieren.

Sie können die Integration Ihrer VPC-Flussprotokolle mit Athena rationalisieren und automatisieren, indem Sie eine CloudFormation-Vorlage erstellen, die die erforderlichen AWS-Ressourcen und vordefinierten Abfragen erstellt, die Sie ausführen können, um Einblicke in den Datenverkehr zu erhalten, der durch Ihre VPC fließt.

1. Die CloudFormation-Vorlage erstellt die folgenden Ressourcen:
- Eine Athena-Datenbank. Der Datenbankname lautet vpcflowlogsathenadatabase<flow-logs-subscription-id>.
- Eine Athena-Arbeitsgruppe. Der Arbeitsgruppenname ist <flow-log-subscription-id><partition-load-frequency><start-date><end-date>Workgroup
- Eine partitionierte Athena-Tabelle, die Ihren Flow-Log-Datensätzen entspricht. Der Tabellenname lautet <flow-log-subscription-id><partition-load-frequency><start-date><end-date>.
- Eine Reihe von Athena benannten Abfragen. Weitere Informationen finden Sie unter Vordefinierte Abfragen.
- Eine Lambda-Funktion, die neue Partitionen nach dem angegebenen Zeitplan (täglich, wöchentlich oder monatlich) in die Tabelle lädt.
- Eine IAM-Rolle, die die Berechtigung zum Ausführen der Lambda-Funktionen erteilt.
1. Voraussetzungen
- Sie müssen eine Region auswählen, die AWS Lambda und Amazon Athena unterstützt.
- Die Amazon S3-Buckets müssen sich in der ausgewählten Region befinden.
1. Preisgestaltung
- Für das Ausführen von Abfragen fallen Standardgebühren für Amazon Athena an.
- Für die Lambda-Funktion, die neue Partitionen nach einem wiederkehrenden Zeitplan lädt, fallen Standardgebühren für AWS Lambda-Gebühren an (wenn Sie eine Partitionslastfrequenz angeben, aber kein Start- und Enddatum angeben).
1. Führen Sie einen der folgenden Schritte aus:
- Öffnen Sie die Amazon VPC-Konsole. Wählen Sie im Navigationsbereich Ihre VPCs aus und wählen Sie dann Ihre VPC aus.
- Öffnen Sie die Amazon VPC-Konsole. Wählen Sie im Navigationsbereich Subnetze aus und wählen Sie dann Ihr Subnetz aus.
- Öffnen Sie die Amazon EC2-Konsole. Wählen Sie im Navigationsbereich Netzwerkschnittstellen aus und wählen Sie dann Ihre Netzwerkschnittstelle aus.
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
- Öffnen Sie die Amazon S3-Konsole. Navigieren Sie zu dem Bucket, den Sie für die Abfrageergebnisse angegeben haben, und zeigen Sie die Ergebnisse der Abfrage an.

Im Folgenden sind die von der generierten CloudFormation-Vorlage bereitgestellten Abfragen mit dem Namen Athena aufgeführt:
* vpcFlowLogsAcceptedTraffic — Die TCP-Verbindungen, die basierend auf Ihren Sicherheitsgruppen und Netzwerk-ACLs zulässig waren.
* vpcFlowLogSadminportTraffic — Der auf den Ports der administrativen Web-App aufgezeichnete Datenverkehr.
* vpcFlowLogsiPv4Traffic — Die Gesamtbytes des aufgezeichneten IPv4-Datenverkehrs.
* vpcFlowLogsiPv6Traffic — Die Gesamtbytes des aufgezeichneten IPv6-Datenverkehrs.
* vpcFlowLogsRejectedTCPTraffic — Die TCP-Verbindungen, die basierend auf Ihren Sicherheitsgruppen oder Netzwerk-ACLs abgelehnt wurden.
* vpcFlowLogsRejectedTraffic — Der Datenverkehr, der basierend auf Ihren Sicherheitsgruppen oder Netzwerk-ACLs abgelehnt wurde.
* vpcFlowLogsShrdpTraffic — Der SSH- und RDP-Datenverkehr.
* vpcFlowLogStopTalkers — Die 50 IP-Adressen mit dem meisten aufgezeichneten Datenverkehr.
* vpcflowLogStopTalkerSpacketLevel — Die 50 IP-Adressen auf Paketebene mit dem meisten aufgezeichneten Datenverkehr.
* vpcFlowLogStopTalkingInstances — Die IDs der 50 Instanzen mit dem meisten aufgezeichneten Datenverkehr.
* vpcFlowLogStopTalkingSubnets — Die IDs der 50 Subnetze mit dem meisten aufgezeichneten Datenverkehr.
* vpcFlowLogStopTCPTraffic — Der gesamte TCP-Datenverkehr, der für eine Quell-IP-Adresse aufgezeichnet wurde.
* vpcFlowLogstotalByteStransFerred — Die 50 Paare von Quell- und Ziel-IP-Adressen mit den meisten aufgezeichneten Bytes.
* vpcFlowLogstotalByteStransFerredPacketLevel — Die 50 Paare von Quell- und Ziel-IP-Adressen auf Paketebene mit den meisten aufgezeichneten Bytes.
* vpcFlowLogstrafficFrmSrcaddr — Der Datenverkehr, der für eine bestimmte Quell-IP-Adresse aufgezeichnet wurde.
* vpcFlowLogstrafficTodStaddr — Der Datenverkehr, der für eine bestimmte Ziel-IP-Adresse aufgezeichnet wurde

## Eindämmung
### Benutzeridentifikation
Identifizieren Sie, welcher Benutzer oder welche Rolle die nicht autorisierten Netzwerkänderungen ausgeführt hat

Es folgt ein Beispiel, um den Benutzer oder das Profil hinter der Erstellung einer EC2-Instanz zu identifizieren:

1. Identifizieren Sie eine der Instanz-IDs für die verdächtigen EC2-Instanzen aus Schritt 1
1. Öffnen Sie AWS Console
1. Wählen Sie Dienste und dann CloudTrail
1. Wählen Sie im linken Rand „Ereignisverlauf“
1. Löschen Sie den aktuellen Filter, indem Sie das „X“ rechts von „false“ auswählen
1. Klicken Sie ganz rechts (unter Benutzerdefiniert) auf das Zahnradsymbol
1. Scrolle nach unten und aktiviere „AWS Access Key“
1. In der Mitte ändern Sie das Filter-Dropdown auf „Ressourcenname“
1. Geben Sie die EC2-Instance-ID in das Suchfeld ein und drücken Sie die Eingabetaste
1. Sie sollten ein RunInstances Ereignis sehen; klicken Sie darauf
1. Notieren Sie den/die Benutzernamen, die Sie entdeckt haben.
- Es kann mehrere Ereignisse geben, die durchlaufen werden müssen, um ursprüngliche Ereignisse zu finden, wie sie aus einer CloudFormation-Vorlage erstellt wurden
1. Gehe zurück zum Ereignisverlauf und ändere das Dropdown-Menü in „Benutzername“ und poste den Eintrag aus dem vorherigen Schritt in die Suche, dann drücke Enter
1. Identifizieren Sie alle anderen Ereignisse des verdächtigen oder kompromittierten Benutzers
1. Gehen wir nun zu Services, IAM (oben links auf dem Bildschirm)
1. Wählen Sie unter IAM-Ressourcen die Option Benutzer
1. Suchen Sie nach dem Konto, das wir in CloudTrail identifiziert haben, und wählen Sie es aus
1. Wählen Sie Sicherheitsnachweise
1. Scrollen Sie nach unten zur Zugriffstaste und wählen Sie das „x“ aus, um den Schlüssel zu löschen
1. Gehe zu deinem Challenge-Dashboard und wähle den Button Meine Fortschritte überprüfen

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
1. Wählen Sie Löschen aus.

## Erholung
Stellen Sie alle Änderungen wieder her, die an Netzwerkressourcen aufgetreten sind

Identifizieren Sie Ressourcen, die dem Internet ausgesetzt sind, und sichern Sie sie: `. /prowler -g gruppe17`

## Vorbeugende Maßnahmen
### AWS Config und AWS System Manager
[Wie man internetzugängliche Ports mit AWS Config und AWS System Manager automatisch repariert] (https://aws.amazon.com/blogs/security/how-to-auto-remediate-internet-accessible-ports-with-aws-config-and-aws-system-manager/)

### AWS Security Hub
[Aktivieren Sie den neuen Sicherheitsstandard für Best Practices von AWS Foundational Security] (https://aws.amazon.com/blogs/security/aws-foundational-security-best-practices-standard-now-available-security-hub/?secd_dir1)

### Allgemeine Sicherheitslage
Führen Sie eine [Self-Service-Sicherheitsbewertung] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) gegen die Umgebung durch, um andere Risiken und möglicherweise andere öffentliche Exposition, die in diesem Playbook nicht identifiziert wurden, weiter zu identifizieren.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder muss ich in der Lage sein, alle kritischen VPC-Umgebungen zu überwachen
- Als Incident Responder brauche ich eine Möglichkeit, mich mit dem Geschäftsteam, dem Infrastrukturteam und den Sicherheitsteams abzustimmen
- Als Incident Responder brauche ich ein Playbook zum Abfragen von VPC FlowLogs im großen Maßstab
- Als Incident Responder muss ich die ordnungsgemäße Verwendung von Config in allen Konten sicherstellen

## Aktuelle Backlog-Artikel