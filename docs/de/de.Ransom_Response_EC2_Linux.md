# Incident Response Playbook: Lösegeldantwort für EC2 Linux/Unix
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt den Prozess für Reaktionen auf Ransom-Angriffe auf EC2-Instanzen.

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
3. [**ERKENNUNG UND ANALYSE**] Verwenden Sie CloudWatch-Metriken, um festzustellen, ob Daten möglicherweise exfiltriert wurden
4. [**ERKENNUNG UND ANALYSE**] Verwenden Sie vPCFlowLogs, um unangemessenen Datenbankzugriff von externen IP-Adressen zu identifizieren
5. [**CONTAINMENT**] Betroffene Ressourcen sofort isolieren
6. [**ERADIKATION**] Entfernen Sie alle kompromittierten Systeme aus dem Netzwerk.
7. [**ERADICATION**] Erzwingen Sie NACLs basierend auf Netzwerk-IOCs, um weiteren Datenverkehr zu verhindern
8. [**ERADICATION**] Andere Interessengegenstände
9. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsverfahren aus


***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Leitfaden zur Behandlung von Computersicherheitsvorfällen] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Ransom & Data Destruction
* **Kategorie**: Lösegeldangriff
* **Ressourcen**: EC2
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken
* **Protokollquellen**: CloudTrail, CloudWatch, AWS Config
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Handhabung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

## Vorbereitung
* Bewerten Sie die Sicherheitslage des Kontos, um Sicherheitslücken zu identifizieren und zu beheben
* AWS hat ein neues Open Source-Tool zur Self-Service-Sicherheitsbewertung (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten Konto
* Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen, einschließlich Domänencontroller, Microsoft Windows EC2-Instanzen, Microsoft Windows-Servern und Datenbanken sowie jeglicher Integration mit externen Identitätsanbietern
* Führen Sie wiederkehrende Schwachstellenanalysen Ihrer Hosts mithilfe von Dienstprogrammen wie [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/) durch
* Führen Sie Backups von EC2-Instanzen durch
* Erwägen Sie, [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) oder [AWS CloudeDure] (https://aws.amazon.com/cloudendure-disaster-recovery/) zu verwenden
* [Sichern Sie Ihre Dateien mit dem Dateiverlauf] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
* Überprüfen Sie Ihre Backups und stellen Sie sicher, dass sich die Infektion nicht auf sie ausgebreitet hat
* Sichern Sie wichtige Dateien regelmäßig. Verwenden Sie die 3-2-1-Regel. Behalten Sie drei Backups Ihrer Daten auf zwei verschiedenen Speichertypen und mindestens ein Backup außerhalb des Standorts auf
* Wenden Sie die neuesten Updates auf Ihre Betriebssysteme und Apps an
* Informieren Sie Ihre Mitarbeiter, damit sie Social Engineering- und Spear-Phishing-Angriffe identifizieren können
* Bekannte Ransomware-Dateitypen blockieren
* Verwenden Sie [Systems Manager und Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/), um zu überprüfen, ob EC2-Instanzen allgemeine Schwachstellen und Risikopositionen (CVes) enthalten
* Lassen Sie Malware-Erkennung und Antivirenprogramme auf allen Systemen aktiviert - kommerzielle, kostenpflichtige Endpoint Detection and Response (EDR) -Lösungen (EDR) werden bevorzugt.
* Ein Beispielleitfaden finden Sie unter So installieren und verwenden Sie Linux Malware Detect (LMD) mit ClamAV als Antivirus-Engine (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
* Verhärten Sie die internetbezogenen Assets und stellen Sie sicher, dass sie über die neuesten Sicherheitsupdates verfügen. Verwenden Sie Bedrohungs- und Schwachstellenmanagement, um diese Assets regelmäßig auf Schwachstellen, Fehlkonfigurationen und verdächtige Aktivitäten zu überprüfen.
* Üben Sie das Prinzip der geringsten Privilegien aus und bewahren Sie die Hygiene von Anmeldeinformationen bei. Vermeiden Sie die Verwendung von domänenweiten Dienstkonten auf Administratorebene. Erzwingen Sie starke randomisierte Just-in-Time-Administrator-Passwörter für lokale Administratoren.
* Überwachen Sie auf Brute-Force-Versuche. Übermäßige fehlgeschlagene Authentifizierungsversuche prüfen (/var/log/auth.log oder /var/log/secure)
* Überwachen zum Löschen von Ereignisprotokollen
* Aktivieren Sie die Manipulationsschutzfunktionen, um zu verhindern, dass Angreifer Sicherheitsdienste stoppen
* Bestimmen Sie, wo hoch privilegierte Konten sich anmelden und Anmeldeinformationen freigeben.
* Verwenden Sie einen Endpunkt-Sicherheitsagenten wie [Wazuh] (https://documentation.wazuh.com/current/getting-started/components/index.html)
* Verwenden Sie einen Dateiintegritätsmonitor wie [Tripwire] (https://github.com/Tripwire/tripwire-open-source), um Änderungen an kritischen Dateien zu erkennen

### Verwenden Sie AWS Config, um die Konformität der Konfiguration anzuzeigen:
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die AWS Config-Konsole unter https://console.aws.amazon.com/config/
1. Stellen Sie im Menü der AWS Management Console sicher, dass die Regionsauswahl auf eine Region festgelegt ist, die AWS Config-Regeln unterstützt. Eine Liste der unterstützten Regionen finden Sie unter AWS Config-Regionen und Endpoints in der allgemeinen Referenz von Amazon Web Services
1. Wählen Sie im Navigationsbereich Ressourcen aus. Auf der Seite Ressourcenbestand können Sie nach Ressourcenkategorie, Ressourcentyp und Konformitätsstatus filtern. Wählen Sie gegebenenfalls Gelöschte Ressourcen einbeziehen aus. In der Tabelle werden die Ressourcenkennung für den Ressourcentyp und den Ressourcenkonformitätsstatus für diese Ressource angezeigt. Die Ressourcenkennung könnte eine Ressourcen-ID oder ein Ressourcenname sein
1. Wählen Sie eine Ressource aus der Spalte „Ressourcenkennung“
1. Wählen Sie die Schaltfläche „Ressourcen-Zeitleiste“. Sie können nach Konfigurationsereignissen, Compliance-Ereignissen oder CloudTrail-Ereignissen filtern
1. Konzentrieren Sie sich insbesondere auf die folgenden Ereignisse:
* ebs-in-Backup-Plan
* ebs-optimierte Instanz
* ebs-snapshot-öffentlich-restorable-check
* ec2-ebs-Verschlüsselung standardmäßig
* ec2-imdsv2-check
* ec2-Instanz-Detailed-Monitoring-fähig
* ec2-instanz-managed-by systems-manager
* ec2-instanz-multiple-eni-check
* ec2-instanz-keine öffentliche IP
* ec2-Instance-Profil angehängt
* ec2-managedinstance-Anwendungen - auf die schwarze Liste gesetzt
* ec2-managedinstance-Anwendungen - erforderlich
* ec2-ManagedInstance-Assoziations-Konformitäts-Status-Check
* ec2-managedinstanz-inventary-blacklist
* ec2-managedinstanz-Patch-Compliance-Statuscheck
* ec2-managedinstanz-Plattform-Check
* ec2-Sicherheitsgruppe angehängt an eni
* ec2-gestoppte Instanz
* ec2-Volume-Inuse-Check

## Eskalationsverfahren
- „Ich brauche eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll“
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Erkennung und Analyse
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
WO (sourceaddress = '192.0.2.1' ODER zieladresse = '192.0.2.1')
UND date_partition >= '2020/07/01'
UND date_partition <= '2020/07/31'
UND account_partition = '111122223333'
UND region_partition in („us-ost-1“, „us-ost--2“, „us-west-2“, „us-west-2“)
GROUP BY sourceaddress, destinationaddress, sourceport, destinationport
SORTIEREN NACH byte_count DESC
```
1. Andere Beispielabfragen werden in der [vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql) bereitgestellt

## Eindämmung
### Betroffene Ressourcen sofort isolieren
**HINWEIS**: Stellen Sie sicher, dass Sie über einen Prozess verfügen, um eine Eskalation und Genehmigung zur Isolierung von Ressourcen anzufordern, um sicherzustellen, dass zuerst eine Analyse der geschäftlichen Auswirkungen darauf durchgeführt wird, wie sich die Isolation auf den aktuellen Betrieb und die Umsatzströme auswirken wird.
1. Bestimmen Sie, ob die Instanz Teil einer Auto Scaling-Gruppe ist oder an einen Load Balancer angehängt ist
* Autoscaling Group: Trennen Sie die Instanz von der Gruppe
* Elastic Load Balancer: Melden Sie die Instanz von der ELB ab und löschen Sie die Instanz aus Zielgruppen
1. Erstellen Sie eine *neue* Sicherheitsgruppe, die den gesamten Ein- und Ausstiegsverkehr blockiert. Stellen Sie sicher, dass Sie die Standardregel „Alle zulassen“ für den ausgehenden Datenverkehr entfernen
1. Hängen Sie die *neue* Sicherheitsgruppe an die betroffenen Instanz (en) an

## Ausrottung
### Entferne alle kompromittierten Systeme aus dem Netzwerk.
* Befolgen Sie regulatorische Anforderungen oder interne Unternehmensrichtlinien, um festzustellen, ob eine Forensik der EC2-Instanz erforderlich ist
* Wenn Forensik der Instanzen erforderlich ist ODER Daten wiederhergestellt werden müssen, folgen Sie dem [Playbook: EC2 Forensics] (. /docs/ec2_Forensics.md)

### Erzwingen Sie NACLs basierend auf Netzwerk-IOCs, um weiteren Datenverkehr zu verhindern
1. Öffnen Sie die [Amazon VPC-Konsole] (https://console.aws.amazon.com/vpc/)
1. Wählen Sie im Navigationsbereich Netzwerk-ACLs
1. Wählen Sie Netzwerk-ACL erstellen
1. Benennen Sie im Dialogfeld Netzwerk-ACL erstellen optional Ihre Netzwerk-ACL und wählen Sie die ID Ihrer VPC aus der VPC-Liste aus. Dann wähle Ja, Erstellen
1. Wählen Sie im Detailbereich je nach Art der Regel, die Sie hinzufügen müssen, entweder die Registerkarte Eingehende Regeln oder Ausgehende Regeln aus, und wählen Sie dann Bearbeiten
1. Geben Sie unter Regel # eine Regelnummer ein (z. B. Die Regelnummer darf nicht bereits in der Netzwerk-ACL verwendet werden. Wir verarbeiten die Regeln in der Reihenfolge, beginnend mit der niedrigsten Zahl
* Wir empfehlen, Lücken zwischen den Regelnummern (wie 100, 200, 300) zu lassen, anstatt fortlaufende Zahlen (101, 102, 103) zu verwenden. Dies erleichtert das Hinzufügen einer neuen Regel, ohne die vorhandenen Regeln neu nummerieren zu müssen
1. Wählen Sie eine Regel aus der Liste Typ aus. Um beispielsweise eine Regel für HTTP hinzuzufügen, wählen Sie HTTP aus. Um eine Regel hinzuzufügen, um den gesamten TCP-Datenverkehr zuzulassen, wählen Sie Alle TCP. Für einige dieser Optionen (z. B. HTTP) füllen wir den Port für Sie aus. Um ein Protokoll zu verwenden, das nicht aufgeführt ist, wählen Sie Benutzerdefinierte Protokollregel
1. (Optional) Wenn Sie eine benutzerdefinierte Protokollregel erstellen, wählen Sie die Nummer und den Namen des Protokolls aus der Protokollliste aus. Weitere Informationen finden Sie unter IANA Liste der Protokollnummern
1. (Optional) Wenn das ausgewählte Protokoll eine Portnummer benötigt, geben Sie die Portnummer oder den Portbereich ein, der durch einen Bindestrich getrennt ist (z. B. 49152-65535).
1. Geben Sie im Feld Quelle oder Ziel (abhängig davon, ob es sich um eine eingehende oder ausgehende Regel handelt) den CIDR-Bereich ein, für den die Regel gilt
1. Wählen Sie in der Liste Zulassen/Verweigern die Option ZULASSEN aus, damit der angegebene Datenverkehr oder DENY den angegebenen Datenverkehr verweigern kann
1. (Optional) Um eine weitere Regel hinzuzufügen, wählen Sie Andere Regel hinzufügen und wiederholen Sie die vorherigen Schritte nach Bedarf
1. Wenn Sie fertig sind, wählen Sie Speichern
1. Wählen Sie im Navigationsbereich Netzwerk-ACLs und wählen Sie dann die Netzwerk-ACL aus
1. Wählen Sie im Detailbereich auf der Registerkarte Subnetzzuordnungen die Option Bearbeiten aus. Aktivieren Sie das Kontrollkästchen Zuordnen für das Subnetz, das mit der Netzwerk-ACL verknüpft werden soll, und wählen Sie dann Speichern

### Andere interessante Gegenstände
* Tabelle 1 von [No Strings on Me: Linux und Ransomware] (https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870) von Richard Horne identifiziert mehrere Indikatoren, die überwacht werden sollen, einschließlich:
* Mögliche Erstellung und Ausführung von Prozessen innerhalb des/tmp-Verzeichnisses
* Ein neuer Prozess, der noch nie auf dem Endpunkt gesehen wurde und externe Netzwerkkonnektivitätsanforderungen hat
* Dateien, die mehrmals im selben Verzeichnisbaum umbenannt werden
* Große Prozess-Tress, bei der der übergeordnete Prozess vor den untergeordneten Prozessen beendet wird
* Eskalation von Privilegien oder Versuche, Sudo-Zugriff zu erhalten
* Verwendung der Strings cmd, um zu versuchen, die Kommunikation vor oder während der Infektion zu codieren
* Mit Entropie generierte Dateinamen oder die sich in schneller Folge befinden
* Änderung der Bootoptionen
* Versuchte Kommunikation mit DNS-Namen, bei denen Entropie gefunden wird, oder direkt mit bloßen IPs
* Erstellung von .sh-Dateien in nicht benutzerbasierten Home-Verzeichnissen
* Verwendung des chmod cmd, um ausführbare Dateien in übermäßig zulässige Rechte zu ändern
* Änderung der Verteilung Yum- oder Apt Repositorys
* Schnelle und aufeinanderfolgende Verzeichnisaufzählung
* Hohe Speicherauslastung für kurze oder anhaltende Zeiträume
* Fokus wird auf Ausreißer und Ereignisse gelegt, die außerhalb des typischen täglichen Betriebs liegen
* Externe sowie interne Netzwerkkommunikation
* Die Verwendung von wget oder curl cmds
* Aufzählung von freigegebenen Web-, Datenbank- oder Dateispeicher-Verzeichnisbäumen
* Mit ungeraden Dateierweiterungen erstellte Dateien
* Mehrere Änderungen in einem einzigen Verzeichnis
* Mehrere Kopien derselben Datei in mehreren Verzeichnissen
* Mögliche Aufrufe für Verschlüsselungsbibliotheken
* Entfernen von Dateien aus Verzeichnissen mit Platzhaltern oder ohne Bestätigung
* Die Verwendung von chmod mit Platzhaltern (oder chmod 777)

## Erholung
* Es wird empfohlen, das Lösegeld nicht zu zahlen
* Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
* Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
* Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
* Besuchen Sie https://www.nomoreransom.org/, um festzustellen, ob ein Entschlüsseler für die Malware-Variante verfügbar ist, die Ihre Daten infiziert haben
* [IAM-Benutzerschlüssel löschen oder drehen] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); Sie können alle Schlüssel in Ihrem Konto drehen, wenn Sie einen bestimmten Schlüssel oder Schlüssel nicht identifizieren können, die freigegeben wurden
* [Unautorisierte IAM-Benutzer löschen] (https://console.aws.amazon.com/iam/home#users.)
* [Unautorisierte Richtlinien löschen] (https://console.aws.amazon.com/iam/home#/policies)
* [Unautorisierte Rollen löschen] (https://console.aws.amazon.com/iam/home#/roles)
* [Temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden. HINWEIS: Das Löschen von IAM-Benutzern kann sich auf Produktions-Workloads auswirken und sollte mit Vorsicht durchgeführt werden* Verwenden Sie CloudEnure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
* Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, ob die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her
* Erstellen Sie neue EC2-Instanzen aus einem vertrauenswürdigen AMI
* Verwenden Sie Cloudendure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
* Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, ob die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook, um EC2 Forensics durchzuführen
- Als Incident Responder brauche ich eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll
- Als Incident Responder muss ich die Protokollierung in allen Regionen aktiviert haben, die unabhängig von der Nutzungsabsicht aktiviert sind
- Als Incident Responder muss ich in der Lage sein, Krypto-Mining auf meinen bestehenden EC2-Instanzen zu erkennen

## Aktuelle Backlog-Artikel