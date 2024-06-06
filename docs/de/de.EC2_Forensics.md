# Playbook für Incident Response: EC2 Forensics
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2024 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

[[_TOC_]]

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

## Zusammenfassung
In diesem Playbook wird der Prozess zur Forensik bei EC2-Instanzen beschrieben, die als bösartig eingestuft wurden oder Kompromissindikatoren aufweisen, die eine zusätzliche Untersuchung rechtfertigen.

Es empfiehlt sich auch, die Untersuchung in derselben AWS-Region durchzuführen, in der das Ereignis stattgefunden hat, anstatt die Daten in eine andere AWS-Region zu replizieren. Wir empfehlen diese Vorgehensweise hauptsächlich aufgrund der zusätzlichen Zeit, die für die Übertragung der Daten zwischen Regionen erforderlich ist. Stellen Sie für jede AWS-Region sicher, in der Sie tätig sind, dass sowohl Ihr Vorfallsreaktionsprozess als auch die Responder die einschlägigen Datenschutzgesetze einhalten. Wenn Sie Daten zwischen AWS-Regionen verschieben müssen, sollten Sie die rechtlichen Auswirkungen des Verschiebens von Daten zwischen Gerichtsbarkeiten berücksichtigen. Es ist im Allgemeinen eine bewährte Vorgehensweise, die Daten in derselben nationalen Gerichtsbarkeit zu halten.

Weitere Informationen finden Sie im [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnis***_ und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Ausgenutzte Schwachstellen**
* **Exploits und Tools beobachtet**
* **Absicht des Schauspieler**
* **Attribution** des Schauspieler**
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
1. [**PREPARATION**] Identifizieren Sie Best Practices, bevor Sie Forensik-Verfahren ausführen
2. [**PREPARATION**] Erstellen Sie eine Forensik-VPC
3. [**VORBEREITUNG**] VPC Endpoint für SSM hinzufügen
4. [**VORBEREITUNG**] Erstellen Sie eine Netzwerkzugriffskontrollliste für die Forensics VPC
5. [**PREPARATION**] Erstellen Sie ein Forensics EC2 Golden AMI
6. [**VORBEREITUNG**] Implementieren Sie ein Schulungsprogramm zur Identifizierung und Bewertung von Cloud-Forensik-Funktionen
7. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
8. [**ERKENNUNG UND ANALYSE**] Erstellen Sie einen Amazon EBS Snapshot
9. [**ERKENNUNG UND ANALYSE**] Teilen Sie einen Amazon EBS Snapshot
10. [**ERKENNUNG UND ANALYSE**] Teilen Sie einen unverschlüsselten Snapshot über die Konsole
11. [**ERKENNUNG UND ANALYSE**] Verwenden Sie einen unverschlüsselten Snapshot, der privat mit Ihnen geteilt wurde
12. [**ERKENNUNG UND ANALYSE**] Teilen Sie einen verschlüsselten Snapshot über die Konsole
13. [**ERKENNUNG UND ANALYSE**] Verwenden Sie einen verschlüsselten Snapshot, der mit Ihnen geteilt wurde
14. [**ERKENNUNG UND ANALYSE**] Erstellen Sie eine Forensik EC2 von Forensics Golden AMI
15. [**ERKENNUNG UND ANALYSE**] Montieren Sie das vermutete EBS-Volume auf Forensics EC2
16. [**ERKENNUNG UND ANALYSE**] Forensik-Prozesse ausführen
17. [**CONTAINMENT**] Betroffene Ressourcen sofort isolieren
18. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsprozesse durch

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Tools: Forensik
* **Kategorie**: Forensik
* **Ressourcen**: EC2, VPC
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken, AWS Shield
* **Quellen**: Endpunkt
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Behandlung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

## Vorbereitung
### [Best Practices] (https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)
* Schalten Sie AWS CloudTrail in jeder Region ein
Beschränken Sie Ihre CloudTrail-Protokollierung nicht auf AWS-Regionen, die Sie aktiv verwenden. Wenn Sie CloudTrail in jeder AWS-Region aktivieren, können Sie ungewöhnliches Verhalten leichter identifizieren, z. B. AWS-Services, die aus einer AWS-Region bereitgestellt werden, die Ihr Unternehmen nicht verwendet.

* Protokollinformationen schützen
Stellen Sie sicher, dass Audit-Trails für Systemkomponenten aktiviert und aktiv sind, und sichern Sie sie regelmäßig. Erwägen Sie, sie an einem Ort zu speichern, der unterschiedliche Anmeldeinformationen erfordert, damit Angreifer keine Protokolldateien löschen können, die einen Nachweis für ihre böswilligen Aktivitäten liefern.

* Ignorieren Sie AWS die Kommunikation über
AWS sendet automatisch eine E-Mail an Ihre registrierte E-Mail-Adresse, wenn ein Missbrauchsfall eingereicht wird. Reagieren Sie sofort und erwägen Sie, eine dedizierte E-Mail-Adresse einzurichten. Je schneller Sie auf einen Sicherheitsvorfall reagieren, desto besser können Sie die Auswirkungen auf Ihr Unternehmen einschränken. Es ist gut, eine [Verteilerliste für den alternativen Kontakt aus Sicherheitsgründen] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html#manage-account-payment-alternate-contacts) in dem Konto zu haben, das aus mehreren Stakeholdern besteht, damit die Nachrichten nicht in einem einzigen „stecken“ bleiben Posteingang des Mitarbeiters (z. B. im Urlaub, Kündigung, krank usw.).

### Erstellen Sie eine Forensik-VPC
**HINWEIS**: Es wird empfohlen, in jeder Region, in der die Forensik möglicherweise gegen Produktionsressourcen durchgeführt werden muss, eine Forensik-VPC-Einrichtung durchzuführen.

1. Öffnen Sie die [Amazon VPC-Konsole] (https://console.aws.amazon.com/vpc/)
1. Wählen Sie im Navigationsbereich Ihre VPCs, VPC erstellen aus.
1. Geben Sie bei Bedarf die folgenden VPC-Details an.
* Namensschild: Geben Sie optional einen Namen für Ihre VPC an. Dadurch wird ein Tag mit dem Schlüssel Name und dem von Ihnen angegebenen Wert erstellt.
* IPv4 CIDR-Block: Geben Sie einen IPv4-CIDR-Block für die VPC an. Der kleinste CIDR-Block, den Sie angeben können, ist /28 und der größte ist /16. Es wird empfohlen, einen CIDR-Block aus den privaten (nicht öffentlich routierbaren) IP-Adressbereichen anzugeben, wie in RFC 1918 angegeben, beispielsweise 10.0.0.0/16 oder 192.168.0.0/16.
* Hinweis: Sie können einen Bereich öffentlich routierbarer IPv4-Adressen angeben. Derzeit unterstützen wir jedoch keinen direkten Zugang zum Internet von öffentlich routierbaren CIDR-Blöcken in einer VPC.
* Windows-Instanzen können nicht korrekt booten, wenn sie in eine VPC mit Bereichen von 224.0.0.0 bis 255.255.255 (IP-Adressbereiche der Klasse D und Klasse E) gestartet werden.
* IPv6 CIDR-Block: Ordnen Sie optional einen IPv6 CIDR-Block mit Ihrer VPC zu. Wählen Sie eine der folgenden Optionen aus und wählen Sie dann CIDR auswählen:
* Von Amazon bereitgestellter IPv6 CIDR-Block: Fordert einen IPv6-CIDR-Block aus Amazons Pool an IPv6-Adressen an. Wählen Sie für Network Border Group die Gruppe aus, aus der AWS IP-Adressen ankündigt.
* IPv6 CIDR im Besitz von mir: (BYOIP) Weist einen IPv6 CIDR-Block aus Ihrem IPv6-Adresspool zu. Wählen Sie für Pool den IPv6-Adresspool aus, aus dem der IPv6 CIDR-Block zugewiesen werden soll.
* Mietverhältnis: Wählen Sie eine Mietoption aus.
* Wählen Sie Dediziert aus, um sicherzustellen, dass Instanzen, die in dieser VPC gestartet werden, dedizierte Mandanteninstanzen sind, unabhängig vom beim Start angegebenen Tenance-Attribut
* Wählen Sie Standard aus, um sicherzustellen, dass in dieser VPC gestartete Instanzen das beim Start angegebene Mandantenattribut verwenden.
* (Optional) Fügen Sie ein Tag hinzu oder entfernen Sie es.
* Wählen Sie „Erstellen“.

### [VPC Endpoint für SSM hinzufügen] (https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1. Stellen Sie sicher, dass der SSM-Agent auf der Instanz installiert ist.
2. Erstellen Sie ein Instanzprofil für AWS Identity and Access Management (IAM) für Systems Manager. Sie können eine neue Rolle erstellen oder die erforderlichen Berechtigungen zu einer vorhandenen Rolle hinzufügen.
3. Hängen Sie die IAM-Rolle an Ihre private EC2-Instance an.
4. Öffnen Sie die Amazon EC2 EC2-Konsole und wählen Sie dann Ihre Instance aus. Notieren Sie auf der Registerkarte Beschreibung die VPC-ID und die Subnet ID.
5. Erstellen Sie einen VPC-Endpunkt für Systems Manager.
* Wählen Sie für Dienstname com.amazonaws aus. [region] .ssm (z. B. com.amazonaws.us-east-1.ssm). Eine vollständige Liste der Regionencodes finden Sie unter Verfügbare Regionen.
* Wählen Sie für VPC die VPC-ID für Ihre Instance aus.
* Wählen Sie für Subnets eine Subnet ID in Ihrer VPC aus. Wählen Sie für eine hohe Verfügbarkeit mindestens zwei Subnetze aus verschiedenen Availability Zones innerhalb der Region aus.
* Hinweis: Wenn Sie mehr als ein Subnetz in derselben Availability Zone haben, müssen Sie keine VPC endpoints für die zusätzlichen Subnetze erstellen. Alle anderen Subnetze innerhalb derselben Availability Zone können auf die Schnittstelle zugreifen und diese verwenden.
* Wählen Sie für DNS-Namen aktivieren die Option Aktivieren für diesen Endpunkt aus. Weitere Informationen finden Sie unter Private DNS für Interface-Endpoints.
* Wählen Sie für Sicherheitsgruppe eine vorhandene Sicherheitsgruppe aus oder erstellen Sie eine neue. Die Sicherheitsgruppe muss eingehenden HTTPS-Datenverkehr (Port 443) von den Ressourcen in Ihrer VPC zulassen, die mit dem Dienst kommunizieren.
* Wenn Sie eine neue Sicherheitsgruppe erstellt haben, öffnen Sie die VPC-Konsole, wählen Sie Sicherheitsgruppen und wählen Sie dann die neue Sicherheitsgruppe aus. Wählen Sie auf der Registerkarte Eingehende Regeln die Option Eingehende Regeln bearbeiten aus. Fügen Sie eine Regel mit den folgenden Details hinzu und wählen Sie dann Regeln speichern:
* Wählen Sie für Typ HTTPS aus.
* Wählen Sie für Source Ihre VPC CIDR aus. Für eine erweiterte Konfiguration können Sie den CIDR bestimmter Subnetze zulassen, der von Ihren EC2-Instanzen verwendet wird.
* Notieren Sie sich die Sicherheitsgruppen-ID. Sie verwenden diese ID mit den anderen Endpunkten.
6. Erstellen Sie Richtlinien für VPC-Schnittstellen-Endpunkte für AWS Systems Manager.
* Wiederholen Sie Schritt 5 mit der folgenden Änderung: Wählen Sie für Dienstname com.amazonaws aus. [region] .ec2messages.
* Wiederholen Sie Schritt 5 mit der folgenden Änderung: Wählen Sie für Dienstname com.amazonaws aus. [region] .ssmmessages. Sie müssen dies tun, wenn Sie den Session Manager verwenden möchten.

### Erstellen Sie eine Netzwerkzugriffssteuerungsliste für die Forensics VPC
Dieser Prozess wird die VPC isolieren, sodass der Datenverkehr weder in Instanzen eintreten noch verlassen kann, die darin enthalten sind

1. Öffnen Sie die [Amazon VPC-Konsole] (https://console.aws.amazon.com/vpc/)
1. Wählen Sie im Navigationsbereich Sicherheit, Network ACLs aus.
1. Aktivieren Sie das Kontrollkästchen neben der Forensics VPC
1. Wählen Sie oben rechts das Dropdown-Menü aus Aktionen und Eingehende Regeln bearbeiten
1. Ändern Sie Regel 100 von „All Traffic“ in „SSH“
1. Wählen Sie Änderungen speichern
1. Wiederholen Sie diese Schritte für ausgehende Regeln

### Erstelle ein Forensics EC2 Golden AMI
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie „Instance starten“
1. Geben Sie in der Suchleiste `Ubuntu` ein und drücken Sie die Eingabetaste
1. Wählen Sie `Ubuntu Server 18.04 LTS`
1. Wählen Sie einen Instanztyp aus
* Hinweis: Für forensische Zwecke wird empfohlen, eine Instanz vom Typ M5 zu wählen
1. Wählen Sie `Weiter: Instanzdetails konfigurieren`
* Netzwerk: Wählen Sie das Forensik-VPC-Netzwerk
* Öffentliche IP automatisch zuweisen: Deaktivieren
* IAM-Rolle: Wählen Sie eine geeignete Rolle für Forensik-Zwecke
* Kündigungsschutz aktivieren: Stellen Sie sicher, dass dieses Kästchen aktiviert ist
1. Wählen Sie `Weiter: Speicher hinzufügen`
* Wählen Sie „Neues Volumen hinzufügen“ und erstellen Sie ein Volume, das ausreichend groß ist, um die erfassten Forensik-Daten zu verarbeiten. Ein guter Startwert beträgt 100 GB, aber stützen Sie diesen Wert auf Ihre organisatorischen Bedürfnisse und Erfahrungen
1. Wählen Sie `Weiter: Schlagworte hinzufügen`
1. Wählen Sie „Überprüfen und starten“
1. Wählen Sie `Launch`
1. Melden Sie sich entweder über EC2 Session Manager bei den EC2-Instanzen an
1. Installieren Sie Ihre Forensik-Tools auf der Instance. Im Folgenden finden Sie ein paar Beispiele für Open Source Forensik-Tools:
1. Installieren von SANS SIFT Toolkit
* sudo su - ubuntu
* sudo curl -Lo /usr/local/bin/sift https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux
* sudo chmod +x /usr/local/bin/sift
* sudo /usr/local/bin/sift install —mode=server —user=ubuntu
* sudo /usr/local/bin/sift upgrade
1. Google Rapid Response installieren
* sudo apt install mysql-server
* sudo mysql_secure_installation
* create database grr; grant all privileges on grr.* to grr @localhost identified by 'password'; flush privileges; @localhost
* wget https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb
* sudo apt installieren. /grr-server_3.2.4-6_amd64.deb
* sudo systemctl restart grr-server
* sudo systemctl status grr-server
1. Weitere Informationen finden Sie unter [wie man GRR-Clients auf Ubuntu 18.04 installiert und einrichtet] (https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/)
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Zu Instanzen navigieren
1. Klicken Sie mit der rechten Maustaste auf die Instanz, die Sie als Grundlage für Ihr AMI verwenden möchten, und wählen Sie im Kontextmenü Image erstellen.
1. Geben Sie im Dialogfeld Bild erstellen einen eindeutigen Namen und eine Beschreibung ein, und wählen Sie dann Bild erstellen aus.
* Standardmäßig fährt Amazon EC2 die Instance herunter, erstellt Snapshots aller angeschlossenen Volumes, erstellt und registriert das AMI und startet die Instance dann neu.
* Wählen Sie Kein Neustart, wenn Sie nicht möchten, dass Ihre Instance heruntergefahren wird.
* Wenn Sie Kein Neustart wählen, können wir die Integrität des Dateisystems des erstellten Images nicht garantieren.

Es kann einige Minuten dauern, bis das AMI erstellt wurde. Nachdem es erstellt wurde, wird es in der AMIS-Ansicht im AWS Explorer angezeigt. Um diese Ansicht anzuzeigen, doppelklicken Sie im AWS Explorer auf den Knoten Amazon EC2 | AMIs. Um Ihre AMIs anzuzeigen, wählen Sie in der Dropdown-Liste Anzeigen die Option „Owned By Me“ aus. Möglicherweise müssen Sie Aktualisieren wählen, um Ihr AMI zu sehen. Wenn das AMI zum ersten Mal erscheint, befindet es sich möglicherweise in einem ausstehenden Zustand, wechselt jedoch nach einigen Augenblicken in einen verfügbaren Status.

### Schulung
- `Was ist die Erwartung von Wissen für Mitarbeiter mit forensikischer Verantwortung? (Wissen/Fähigkeiten/Fähigkeiten) `
- `Welche Forensik-Schulungen und -verfahren erhält mein Personal? `
- `Welche regulatorischen Anforderungen für die Forensik gelten für mein Unternehmen? `
- `Welche Schulung gibt es für Analysten innerhalb des Unternehmens, um sich mit der AWS API (Befehlszeilenumgebung), S3, RDS und anderen AWS-Services vertraut zu machen? `

**Müssen hier mehr Kontext und Gegenstände hinzufügen, da Forensik eine Fertigkeit ist, nicht nur ein Playbook

## Eskalationsverfahren
- „Ich brauche eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll“
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Erkennung und Analyse
### Mögliche Warnungsquellen
* Abrechnung
* CloudWatch (EC2-Metrik/ Ereignisregel)
* Konfig
* GuardDuty
* Inspektor
* Macie
* Sicherheits-Hub
* Vertrauenswürdiger Berater
* Externe Benachrichtigung

### Datenschutz
Sie sollten **IMMER**:
* Beweise schreibgeschützt machen
* Nachweise sollten **NIE** geändert werden
* Erstellen Sie Kopien von Beweisen und arbeiten Sie NUR von diesen
* **NIE** arbeiten von (direkt Zugriff) Quellbeweisen zur Analyse
* Montieren Sie alle Beweise SCHREIBGESCHÜTZT
* Verfolgen Sie, wer auf WAS und WANN zugegriffen hat
* Machen Sie reichlich Untersuchungsnotizen von:
* Was Sie analysiert haben (welches Datenstück)
* Als du es analysiert hast (Datum/ Uhrzeit)
* Was Sie ausgeführt haben (bestimmte Befehle/Prozedur/Prozess)
* Was du gefunden hast (Ergebnisse)

### Erfassung von Metadaten
Erfassen Sie Informationen im Zusammenhang mit der Instanz, die für die Untersuchung nützlich sein könnten
* Instanztyp/ID
* IP-Adresse (n)
* Sicherheitsgruppe (n)
* VPC/ Subnet ID
* Region
* AMI-ID
* Startzeit

### Speichererfassung
Es wird empfohlen, ein Dienstprogramm eines Drittanbieters zu verwenden, um Speicher von laufenden Instanzen zu erhalten.
Es ist wichtig, Speicher zu erfassen **VOR DEM Systemisolation/Shutdown, um alle verfügbaren flüchtigen (und wertvollen) Daten zu erfassen

### [Erstellen Sie einen Amazon EBS Snapshot] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich unter Elastic Block Store Snapshots aus.
1. Wählen Sie Snapshot erstellen aus.
1. Wählen Sie für Ressourcentyp auswählen die Option Volume.
1. Wählen Sie für Volume das Volume aus.
1. (Optional) Geben Sie eine Beschreibung für den Snapshot ein.
1. (Optional) Wählen Sie Tag hinzufügen, um Ihrem Snapshot Tags hinzuzufügen. Geben Sie für jedes Tag einen Tag-Schlüssel und einen Tag-Wert an.
1. Wählen Sie Snapshot erstellen aus.

### [Einen Amazon EBS Snapshot teilen] (https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
#### Teilen Sie einen unverschlüsselten Snapshot mit der Konsole
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Snapshots aus.
1. Wählen Sie den Snapshot aus und wählen Sie dann Aktionen, Berechtigungen ändern.
1. Machen Sie den Snapshot öffentlich oder teilen Sie ihn wie folgt mit bestimmten AWS-Konten:
* Um den Snapshot öffentlich zu machen, wählen Sie „Öffentlich“.
* Diese Option gilt nicht für verschlüsselte Snapshots oder Snapshots mit einem AWS Marketplace-Produktcode.
* Um den Snapshot mit einem oder mehreren AWS-Konten zu teilen, wählen Sie Privat, geben Sie die AWS-Konto-ID (ohne Bindestriche) in der AWS-Kontonummer ein und wählen Sie Berechtigung hinzufügen. Wiederholen Sie dies für alle zusätzlichen AWS-Konten.
1. Wählen Sie „Speichern“.
#### So verwenden Sie einen unverschlüsselten Snapshot, der privat mit Ihnen geteilt wurde
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Snapshots aus.
1. Wählen Sie den Filter Private Snapshots aus.
1. Suchen Sie den Snapshot nach ID oder Beschreibung. Sie können diesen Snapshot wie jeden anderen verwenden. Sie können beispielsweise ein Volume aus dem Snapshot erstellen oder den Snapshot in eine andere Region kopieren.

#### Teilen Sie einen verschlüsselten Snapshot über die Konsole
1. Öffnen Sie die [AWS KMS KMS-Konsole] (https://console.aws.amazon.com/kms)
1. Um die AWS-Region zu ändern, verwenden Sie die Regionenauswahl in der oberen rechten Ecke der Seite.
1. Wählen Sie im Navigationsbereich Kundenverwaltete Schlüssel aus.
1. Wählen Sie in der Spalte Alias den Alias (Textlink) des vom Kunden verwalteten Schlüssels aus, mit dem Sie den Snapshot verschlüsselt haben. Die wichtigsten Details werden auf einer neuen Seite geöffnet.
1. Im Abschnitt Schlüsselrichtlinie sehen Sie entweder die Richtlinienansicht oder die Standardansicht. In der Richtlinienansicht wird das Schlüsselrichtliniendokument angezeigt. Die Standardansicht zeigt Abschnitte für Schlüsseladministratoren, Schlüssellöschung, Schlüsselverwendung und Andere AWS-Konten an. Die Standardansicht wird angezeigt, ob Sie die Richtlinie in der Konsole erstellt und nicht angepasst haben. Wenn die Standardansicht nicht verfügbar ist, müssen Sie die Richtlinie in der Richtlinienansicht manuell bearbeiten. Weitere Informationen finden Sie unter Anzeigen einer Schlüsselrichtlinie (Konsole) im AWS Key Management Service Developer Guide.
Verwenden Sie je nachdem, auf welche Ansicht Sie zugreifen können, entweder die Richtlinienansicht oder die Standardansicht, um der Richtlinie wie folgt eine oder mehrere AWS-Konto-IDs hinzuzufügen:
* (Standardansicht) Scrollen Sie nach unten zu Andere AWS-Konten. Wählen Sie Andere AWS-Konten hinzufügen und geben Sie die AWS-Konto-ID ein, wie aufgefordert. Um ein weiteres Konto hinzuzufügen, wählen Sie Anderes AWS-Konto hinzufügen und geben Sie die AWS-Konto-ID ein. Wenn Sie alle AWS-Konten hinzugefügt haben, wählen Sie Änderungen speichern aus.
* (Richtlinienansicht) Wählen Sie „Bearbeiten“. Fügen Sie den folgenden Anweisungen eine oder mehrere AWS-Konto-IDs hinzu: „Verwendung des Schlüssels zulassen“ und „Anhängen dauerhafter Ressourcen zulassen“. Wählen Sie Änderungen speichern aus. Im folgenden Beispiel wird die AWS-Konto-ID 444455556666 der Richtlinie hinzugefügt.
```
{
„Sid“: „Verwendung des Schlüssels zulassen“,
„Effekt“: „Zulassen“,
„Principal“: {"AWS“: [
„arn:aws:iam። 111122223333:Benutzer/cmkUser“,
„arn:aws:iam። 444455556666:root“
]},
„Aktion“: [
„kms:Encrypt“,
„km:Entschlüsseln“,
„kms:reencrypt*“,
„kms:GenerateDataKey*“,
„kms:DescribeKey“
],
„Ressource“: „*“
},
{
„Sid“: „Anhängen dauerhafter Ressourcen zulassen“,
„Effekt“: „Zulassen“,
„Principal“: {"AWS“: [
„arn:aws:iam። 111122223333:Benutzer/cmkUser“,
„arn:aws:iam። 444455556666:root“
]},
„Aktion“: [
„KMS:Creategrant“,
„kms:ListGrants“,
„KMS:Revokegrant“
],
„Ressource“: „*“,
„Bedingung“: {"Bool“: {"kms:GrantisForawsResource“: true}}
}
```
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Snapshots aus.
1. Wählen Sie den Snapshot aus und wählen Sie dann Aktionen, Berechtigungen ändern.
1. Geben Sie für jedes AWS-Konto die AWS-Konto-ID unter AWS-Kontonummer ein und wählen Sie Berechtigung hinzufügen. Wenn Sie alle AWS-Konten hinzugefügt haben, wählen Sie Speichern aus.

#### So verwenden Sie einen verschlüsselten Snapshot, der mit Ihnen geteilt wurde
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Snapshots aus.
1. Wählen Sie den Filter Private Snapshots aus. Fügen Sie optional den Filter „Verschlüsselt“ hinzu.
1. Suchen Sie den Snapshot nach ID oder Beschreibung.
1. Wählen Sie den Snapshot aus und wählen Sie Aktionen, Kopieren.
1. (Optional) Wählen Sie eine Zielregion aus.
1. Die Kopie des Snapshots wird durch den im Master Key angezeigten Schlüssel verschlüsselt. Standardmäßig ist der ausgewählte Schlüssel das Standard-CMK Ihres Kontos. Um ein vom Kunden verwaltetes CMK auszuwählen, klicken Sie in das Eingabefeld, um eine Liste der verfügbaren Schlüssel anzuzeigen.
1. Wählen Sie „Kopieren“.

### Erstelle eine Forensik EC2 von Forensics Golden AMI
Erstellen Sie eine neue Forensik-Instanz mit Ihrem zuvor erstellten Forensics Golden AMI

### Verdacht EBS Volume auf Forensics EC2 montieren
1. Öffnen Sie die [Amazon EC2 EC2-Konsole] (https://console.aws.amazon.com/ec2/)
1. Wählen Sie im Navigationsbereich Elastic Block Store, Volumes aus.
1. Wählen Sie ein verfügbares Volume aus und wählen Sie Aktionen, Volume anhängen.
1. Geben Sie beispielsweise den Namen oder die ID der Instanz ein.
1. Wählen Sie die Instance aus der Optionsliste aus (es werden nur Instanzen angezeigt, die sich in derselben Availability Zone wie das Volume befinden).
1. Für Gerät können Sie den vorgeschlagenen Gerätenamen beibehalten oder einen anderen unterstützten Gerätenamen eingeben.
* Weitere Informationen finden Sie unter [Gerätenamen auf Linux-Instanzen] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/device_naming.html).
1. Wählen Sie Anhängen aus.
1. Verbinden Sie sich mit Ihrer Instance und mounten Sie das Volume.
* Weitere Informationen finden Sie unter [Machen Sie ein Amazon EBS EBS-Volume für die Verwendung unter Linux verfügbar] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html).

### Forensik-Prozesse ausführen
* `Nach welchen Forensik-Outputs sucht das Unternehmen? `
* `Was wird in den Abschlussbericht aufgenommen? `
* `An wen geht der Bericht? `
* `Gibt es gesetzliche oder behördliche Anforderungen zur Identifizierung von Informationen, die aus einer forensikischen Untersuchung erforderlich sind? `
* `Gibt es Mitarbeiter im Unternehmen, die ordnungsgemäß zur Forensik geschult/lizenziert/zertifiziert sind? `
* `Gibt es Aufbewahrungsvoraussetzungen für Protokolle, Daten oder Beweise? Betrachten Sie möglicherweise die Gletscher-Tresore, um sie zu unterstützen“
* Ein Beispiel für [Digitale forensische Analyse von Amazon Linux EC2 Instances] (https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235) ist vom SANS-Institut verfügbar.

## Eindämmung
### Betroffene Ressourcen sofort isolieren
**HINWEIS**: Stellen Sie sicher, dass Sie über einen Prozess verfügen, um eine Eskalation und Genehmigung zur Isolierung von Ressourcen anzufordern, um sicherzustellen, dass zuerst eine Analyse der geschäftlichen Auswirkungen darauf durchgeführt wird, wie sich die Isolation auf den aktuellen Betrieb und die Umsatzströme auswirken wird.

### Instanz außer Betrieb genommen
Kündigungsschutz aktivieren
* [Deaktivieren Sie die Beendigung der Instanz (über Console/API/CLI)] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
1. Setzen Sie das Shutdown-Verhalten der Instanz auf Stopp (vs. Beenden)
1. Deaktivieren Sie „DeleteOnTermination“ für alle Instanz-Volumes
* [Entfernen (trennen) von Auto-Scaling group] (https://docs.aws.amazon.com/autoscaling/ec2/userguide/detach-instance-asg.html)
1. Öffnen Sie die Amazon EC2 Auto Scaling Scaling-Konsole unter https://console.aws.amazon.com/ec2autoscaling/
1. Aktivieren Sie das Kontrollkästchen neben Ihrer Auto Scaling Scaling-Gruppe.
1. Im unteren Teil der Seite Auto Scaling Scaling-Gruppen wird ein geteilter Bereich geöffnet, in dem Informationen über die ausgewählte Gruppe angezeigt werden.
1. Wählen Sie auf der Registerkarte Instanzverwaltung unter Instanzen eine Instanz aus und wählen Sie Actions, Trennen.
1. Aktivieren Sie im Dialogfeld Instanz trennen das Kontrollkästchen, um eine Ersatzinstanz zu starten, oder lassen Sie sie deaktiviert, um die gewünschte Kapazität zu verringern. Wählen Sie Instanz trennen.
* [Von Load Balancer (s) abmelden] (https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)
1. Öffnen Sie die Amazon EC2 EC2-Konsole unter https://console.aws.amazon.com/ec2/
1. Wählen Sie im Navigationsbereich unter LOAD BALANCING Load Balancers aus.
1. Wählen Sie Ihren Load Balancer aus.
1. Wählen Sie im unteren Bereich die Registerkarte Instanzen aus.
1. Wählen Sie Instanzen bearbeiten.
1. Wählen Sie die Instanz aus, die Sie bei Ihrem Load Balancer registrieren möchten.
1. Wählen Sie „Speichern“.
1. Erstellen Sie eine *neue* Sicherheitsgruppe, die den gesamten Ein- und Ausstiegsverkehr blockiert. Stellen Sie sicher, dass Sie die Standardregel „Alle zulassen“ für den ausgehenden Datenverkehr entfernen
* Hängen Sie die *neue* Sicherheitsgruppe an die betroffenen Instanz (en) an

## Ausrottung
Es gibt keine Schritte für diesen Prozess, die auf dieses Runbook anwendbar sind.

## Erholung
Nach Abschluss der forensikischen Aktivitäten und nach Erteilung des Abschlussberichts sollten Sie die Forensik EC2-Instanz beenden und den EBS-Snapshot **UNLESS** löschen. Es besteht eine gesetzliche oder behördliche Anforderung, die Daten zu pflegen.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook, um EC2 Forensics durchzuführen
- Als Incident Responder brauche ich eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll
- Als Incident Responder muss ich die Protokollierung in allen Regionen aktiviert haben, die unabhängig von der Nutzungsabsicht aktiviert sind
- Als Incident Responder muss ich sicherstellen, dass nur zugelassene AMIs verwendet werden
- Als Incident Responder muss ich in der Lage sein, Krypto-Mining auf meinen bestehenden EC2-Instanzen zu erkennen
- Als Incident Responder benötige ich eine vorbereitete Methode, um Massenzahlen von Instanzen zu löschen

## Aktuelle Backlog-Artikel