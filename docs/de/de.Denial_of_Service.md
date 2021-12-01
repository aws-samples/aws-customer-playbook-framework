# Incident Response Playbook: Diensteverweigerung /Distributed Denial of Service (DOS/DDoS)
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“\
Genehmiger: `Name des Genehmigers“\
Letztes Datum genehmigt:

## Zusammenfassung
Dieses Playbook beschreibt den Prozess zur Reaktion auf Denial-of-Service/Distributed Denial of Service (DOS/DDoS) -Angriffe auf gehostete AWS.

## Mögliche Indikatoren für Kompromisse
- Eine Internetanwendung ist stark belastet, entweder aufgrund von Beliebtheit oder böswilliger Absicht
- Eine einzelne EC2-Instanz wird zum Hosten einer Anwendung verwendet, und wir löschen/gestalten den Datenverkehr (der Kunde wird über AWS Abuse benachrichtigt)
- Ergebnisse im AWS Shield Dashboard

## Mögliche AWS GuardDuty-Ergebnisse
- Backdoor:EC2/DenialofService.DNS
- Backdoor:EC2/DenialofService.TCP
- Backdoor:EC2/DenialofService.UDP
- Backdoor:EC2/DenialofService.udPontCpports
- Backdoor:EC2/DenialofService.unusualProtocol
- Backdoor:EC2/SpamBot
- Verhalten: EC2/NetworkportUngewöhnlich
- Verhalten: EC2/TrafficVolumeUngewöhnlich

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
1. [**PREPARATION**] Erstellen Sie einen öffentlich exponierten Vermögensbestand
2. [**VORBEREITUNG**] Implementieren Sie Schulungen zur Behebung von DOS/DDoS-Angriffen
3. [**PREPARATION**] Entwickeln Sie eine Kommunikationsstrategie zur Eskalation und Meldung von Ereignissen
4. [**VORBEREITUNG**] Führen Sie Dokumentationsprüfungen durch, um sicherzustellen, dass die Verfahren eingehalten und aktuell bleiben
5. [**ERKENNUNG UND ANALYSE**] AWS Shield implementieren
6. [**ERKENNUNG UND ANALYSE**] Nutzen Sie das Dashboard für globale Bedrohungsumgebung mit AWS Shield
7. [**ERKENNUNG UND ANALYSE**] Erwägen Sie, AWS Shield Advanced zu verwenden
8. [**PREPARATION**] Eskalations- und Benachrichtigungsverfahren implementieren
9. [**ERKENNUNG UND ANALYSE**] Erkennung und Abschwächung durchführen
10. [**ERKENNUNG UND ANALYSE**] Identifizieren Sie Top Contributers
11. [**EINDÄMMUNG**] Eindämmung durchführen
12. [**ERADICATION**] Ausrottung durchführen
13. [**AKTIVITÄTEN NACH VORFALLEN**] Fordern Sie eine Gutschrift in AWS Shield Advanced an
14. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Mitigationstechniken
15. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Reduktion der Angriffsfläche
16. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Betriebstechniken
17. [**VORBEREITUNG**] Zusätzliche vorbeugende Maßnahmen: Allgemeine Sicherheitslage

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Leitfaden zur Behandlung von Computersicherheitsvorfällen] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen

* **Taktiken, Techniken und Verfahren**: Diensteverweigerung/Distributed Denial of Service Attacken
* **Kategorie**: DOS/DDoS
* **Ressource**: Netzwerk
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken, AWS Shield
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

Es folgt den Richtlinien des CIS Amazon Web Services Foundations Benchmark (49 Schecks) und verfügt über mehr als 100 zusätzliche Kontrollen, darunter im Zusammenhang mit DSGVO, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 und anderen.

Dieses Tool bietet eine schnelle Momentaufnahme des aktuellen Sicherheitszustands in einer Kundenumgebung. Darüber hinaus bietet [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) ein automatisiertes Compliance-Scannen und kann [in Prowler integrieren] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Börtlich exponiertes Asset-
Finden Sie Ressourcen, die dem Internet ausgesetzt sind: `. /prowler -g gruppe17`

Sie können Security Hub und Firewall Manager verwenden, um [eine zentrale Überwachung für DDoS-Ereignisse einzurichten und nicht konforme Ressourcen automatisch zu beheben] (https://aws.amazon.com/blogs/security/set-up-centralized-monitoring-for-ddos-events-and-auto-remediate-noncompliant-resources/)

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

### Überprüfung der Dokumentation
1. Machen Sie sich mit der [AWS Shield-Dokumentation] vertraut (https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
1. Wenn Sie abonniert sind, folgen Sie dem [Erste Schritte mit AWS Shield Advanced Guide] (https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html)

## Erkennung
### Cloudwatch-Metriken zum Erkennen von DOS/DDoS-Ereignissen
In der Tabelle Empfohlene Amazon CloudWatch-Metriken werden Beschreibungen von Amazon CloudWatch-Metriken aufgeführt, die häufig zum Erkennen und Reagieren auf DDoS-Angriffe verwendet werden.

* AWS Shield Advanced: DdosDetected
* Zeigt ein DDoS-Ereignis für einen bestimmten Amazon-Ressourcennamen (ARN) an.
* AWS Shield Advanced: DdosatTackBitsPerSecond
* Die Anzahl der Bytes, die während eines DDoS-Ereignisses für einen bestimmten ARN beobachtet wurden. Diese Metrik ist nur für Layer-3/4-DDoS-Ereignisse verfügbar.
* AWS Shield Advanced: DdosatTackPacketsPerSecond
* Die Anzahl der Pakete, die während eines DDoS-Ereignisses für einen bestimmten ARN beobachtet wurden. Diese Metrik ist nur für Layer-3/4-DDoS-Ereignisse verfügbar.
* AWS Shield Advanced: DdosatTackRequestsperSecond
* Die Anzahl der Anfragen, die während eines DDoS-Ereignisses für einen bestimmten ARN beobachtet wurden. Diese Metrik ist nur für Layer-7-DDoS-Ereignisse verfügbar und wird nur für die wichtigsten Layer-7-Ereignisse gemeldet.
* AWS WAF: AllowedRequests
* Die Anzahl der zulässigen Webanfragen.
* AWS WAF: BlockedRequests
* Die Anzahl der blockierten Webanfragen.
* AWS WAF: CountedRequests
* Die Anzahl der gezählten Webanfragen.
* AWS WAF: PassedRequests
* Die Anzahl der bestandenen Anfragen. Dies wird nur für Anfragen verwendet, die eine Regelgruppenauswertung durchlaufen, ohne mit einer der Regelgruppenregeln zu übereinstimmen.
* CloudFront: Anfragen
* Die Anzahl der HTTP/S-Anfragen.
* CloudFront: TotalErrorRate
* Der Prozentsatz aller Anfragen, für die der HTTP-Statuscode 4xx oder 5xx ist.
* Amazon Route 53: HealthCheckStatus
* Der Status des Endpunkts für die Integritätsprüfung.
* ALB: ActiveConnectionCount
* Die Gesamtzahl der gleichzeitigen TCP-Verbindungen, die von Clients zum Load Balancer und vom Load Balancer zu Zielen aktiv sind.
* ALB: konsumedLCUS
* Die Anzahl der Load Balancer-Kapazitätseinheiten (LCU), die von Ihrem Load Balancer verwendet werden.
* ALB: HttpCode_Elb_4xx_Count HttpCode_Elb_5xx_Count
* Die Anzahl der vom Load Balancer generierten HTTP 4xx- oder 5xx-Client-Fehlercodes.
* ALB: NewConnectionCount
* Die Gesamtzahl der neuen TCP-Verbindungen, die von Clients zum Load Balancer und vom Load Balancer zu Zielen hergestellt wurden.
* ALB: ProcessedBytes
* Die Gesamtzahl der vom Load Balancer verarbeiteten Bytes.
* ALB: abgelehnt ConnectionCount
* Die Anzahl der zurückgewiesenen Verbindungen, weil der Load Balancer seine maximale Anzahl von Verbindungen erreicht hat.
* ALB: RequestCount
* Die Anzahl der Anfragen, die bearbeitet wurden.
* ALB: targetConnectionErrorCount
* Die Anzahl der Verbindungen, die zwischen dem Load Balancer und dem Ziel nicht erfolgreich hergestellt wurden.
* ALB: targetResponseTime
* Die Zeit, die in Sekunden verstrichen ist, nachdem die Anforderung den Load Balancer verlassen hat, bis eine Antwort vom Ziel empfangen wurde.
* ALB: UnHealthyHostCount
* Die Anzahl der Ziele, die als ungesund gelten.
* NLB: ActiveFlowCount
* Die Gesamtzahl der gleichzeitigen TCP-Flüsse (oder Verbindungen) von Clients zu Zielen.
* NLB: konsumedLCUS
* Die Anzahl der Load Balancer-Kapazitätseinheiten (LCU), die von Ihrem Load Balancer verwendet werden.
* NLB: NewflowCount
* Die Gesamtzahl der neuen TCP-Flüsse (oder Verbindungen), die im Zeitraum von Clients zu Zielen eingerichtet wurden.
* NLB: ProcessedBytes
* Die Gesamtzahl der vom Load Balancer verarbeiteten Bytes, einschließlich TCP/IP-Header.
* Globaler Beschleuniger NewflowCount
* Die Gesamtzahl der neuen TCP- und UDP-Flüsse (oder Verbindungen), die im Zeitraum von Clients zu Endpunkten hergestellt wurden.
* Globaler Beschleuniger: ProcessedBytesin
* Die Gesamtzahl der eingehenden Bytes, die vom Accelerator verarbeitet werden, einschließlich TCP/IP-Header.
* Auto Scaling: GroupMaxSize
* Die maximale Größe der Auto Scaling-Gruppe.
* Amazon EC2: CpuUUtilization
* Der Prozentsatz der zugewiesenen EC2-Recheneinheiten, die derzeit verwendet werden.
* Amazon EC2: NetworkIn
* Die Anzahl der von der Instanz auf allen Netzwerkschnittstellen empfangenen Bytes.
### AWS-Schild
Die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/shieldv2) oder die API liefern eine Zusammenfassung und Details zu erkannten Angriffen.
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/)
1. Wählen Sie in der AWS Shield-Navigationsleiste Überblick oder Ereignisse

### Dashboard für globale Bedrohungsumgebung
Das Global Threat Environment Dashboard bietet zusammenfassende Informationen über alle DDoS-Angriffe, die von AWS erkannt wurden.
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/)
1. Wählen Sie in der AWS Shield-Navigationsleiste das Dashboard für globale Bedrohungen aus
1. Wählen Sie einen Zeitraum aus.

### AWS Shield Advanced
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/)
1. Wählen Sie in der AWS Shield-Navigationsleiste Überblick oder Ereignisse

Sie haben Zugriff auf eine Reihe von CloudWatch-Metriken, die darauf hinweisen können, dass Ihre Anwendung ins Visier genommen wird.
1. Sie können die DdosDetected-Metrik konfigurieren, um Ihnen mitzuteilen, ob ein Angriff erkannt wurde.
1. Wenn Sie basierend auf dem Angriffsvolume alarmiert werden möchten, können Sie auch die Metriken ddosatTackBitsPerSecond, DdosatTackPacketsPerSecond oder DdosatTackRequestsperSecond-Metriken verwenden.

Eine vollständige Liste der verfügbaren Metriken ist in der Tabelle in der [DDoS-Sichtbarkeit] verfügbar (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)

## Eskalationsverfahren

### AWS-Schild
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

### AWS Shield Advanced
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Haben Sie die AWS-Ressourcen angegeben, die Sie mit Shield Advanced schützen möchten? `
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/)
1. Wählen Sie in der AWS Shield-Navigationsleiste Geschützte Ressourcen
- `Haben Sie das DDoS Response Team (DRT) autorisiert, auf Ihre WAF-Regeln und -Protokolle zuzugreifen? Dies hilft ihnen, Angriffe zu mildern, wenn Sie Hilfe anfordern. `
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/)
1. Wählen Sie in der AWS Shield-Navigationsleiste Überblick
1. Konfigurieren Sie die AWS DRT-Unterstützung `DRT-Zugriff bearbeiten'
- `Benötigen Sie Unterstützung vom AWS DDoS Response Team? `
1. Sie können einen Fall unter AWS Shield im AWS Support Center eröffnen
* Um Unterstützung für das DDoS Response Team (DRT) zu erhalten, wenden Sie sich an das AWS Support Center und wählen Sie die folgenden Optionen aus:
1. Falltyp: Technischer Support
1. Service: Verteilte Diensteverweigerung (DDoS)
1. Kategorie: Inbound to AWS
* Mit dem proaktiven Engagement von AWS Shield Advanced kontaktiert das DRT Sie direkt, wenn der mit Ihrer geschützten Ressource verknüpfte Zustandsprüfung von Amazon Route 53 während eines Ereignisses, das von Shield Advanced erkannt wird, ungesund wird.

## Analyse
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die [AWS WAF & Shield-Konsole] (https://console.aws.amazon.com/wafv2/)
1. Wählen Sie in der AWS Shield-Navigationsleiste Ereignisse

### Erkennung und Abschwächung
Auf der Registerkarte Erkennung und Abschwächung werden Diagramme angezeigt, die den Datenverkehr zeigen, den AWS Shield vor der Meldung dieses Ereignisses ausgewertet hat, und die Auswirkungen einer abgesetzten Minderung. Es bietet auch Metriken für Infrastruktur-Layer-Ereignisse für Distributed Denial of Service (DDoS) für Ressourcen in Ihren eigenen Amazon Virtual Private Cloud VPC- und AWS Global Accelerator-Beschleunigern. Erkennungs- und Minderungsmetriken sind für Amazon CloudFront- oder Amazon Route 53-Ressourcen nicht enthalten.

### Top Contributer
Die Registerkarte „Top-Mitwirkende“ bietet einen Einblick, woher der Datenverkehr während eines erkannten Ereignisses kommt. Sie können die Mitwirkenden mit dem höchsten Volume anzeigen, sortiert nach Aspekten wie Protokoll, Quellport und TCP-Flags. Sie können die Informationen herunterladen und die verwendeten Einheiten und die Anzahl der anzuzeigenden Mitwirkenden anpassen.

## Eindämmung

### [Best Practices für die DDoS-Ausfallsicherheit von AWS] (https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
In diesem Whitepaper bietet AWS Ihnen präskriptive DDoS-Anleitungen zur Verbesserung der Ausfallsicherheit von Anwendungen, die auf AWS ausgeführt werden. Dies beinhaltet eine DDOS-resiliente Referenzarchitektur, die als Leitfaden zum Schutz der Anwendungsverfügbarkeit verwendet werden kann. In diesem Whitepaper werden auch verschiedene Angriffstypen beschrieben, wie Angriffe auf Infrastrukturschicht und Angriffe auf Anwendungsebene. AWS erklärt, welche Best Practices für die Verwaltung jeder Angriffstyp am effektivsten sind Darüber hinaus werden die Dienste und Funktionen, die in eine DDoS-Minderungsstrategie passen, skizziert, und wie jeder zum Schutz Ihrer Anwendungen verwendet werden kann, wird erläutert.
Eindämmungsverfahren gehen von der Verwendung von AWS Web Application Firewalls aus. Wenn eine WAF oder Firewall eines Drittanbieters verwendet wird, passen Sie diese Verfahren hier an Ihren Anwendungsfall an.

### AWS WAF
1. Erstellen Sie Bedingungen in AWS WAF, die dem ungewöhnlichen Verhalten entsprechen.
1. Fügen Sie diese Bedingungen einer oder mehreren AWS WAF-Regeln hinzu.
1. Fügen Sie diese Regeln einer Web-ACL hinzu und konfigurieren Sie die Web-ACL so, dass die Anforderungen gezählt werden, die den Regeln entsprechen.
* **HINWEIS** Sie sollten Ihre Regeln immer zuerst testen, indem Sie zunächst Count statt Block verwenden. Nachdem Sie sich sicher sind, dass die neue Regel die richtigen Anforderungen identifiziert, können Sie Ihre Regel so ändern, dass diese Anfragen blockiert werden.
1. Überwachen Sie diese Zählungen, um festzustellen, ob die Quelle der Anfragen blockiert werden soll. Wenn das Volumen der Anfragen weiterhin ungewöhnlich hoch ist, ändern Sie Ihre Web-ACL, um diese Anfragen zu blockieren.

## Ausrottung
Nicht anwendbar

## Erholung
### Einen Kredit in AWS Shield Advanced beantragen
Wenn Sie AWS Shield Advanced abonniert haben und einen DDoS-Angriff erleben, der die Auslastung einer geschützten Shield Advanced-Ressource erhöht, können Sie eine Gutschrift für Gebühren im Zusammenhang mit der erhöhten Auslastung beantragen, soweit sie von Shield Advanced nicht gemildert wird.

Weitere Informationen finden Sie in AWS-Dokumenten unter [Guthaben in AWS Shield Advanced beantragen] (https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html)

## Vorbeugende Maßnahmen
### Techniken zur Abschwächung
In AWS werden automatisch Funktionen zur DDoS-Minderung bereitgestellt
1. AWS Shield Standard schützt sich vor den häufigsten, häufig auftretenden DDoS-Angriffen auf Netzwerk- und Transportschichten, die auf Ihre Website oder Ihre Anwendungen abzielen. Dies wird für alle AWS-Services und in jeder AWS-Region ohne zusätzliche Kosten angeboten.
1. Sie können Ihre Bereitschaft verbessern, auf DDoS-Angriffe zu reagieren und sie zu mildern, indem Sie AWS Shield Advanced abonnieren. Dieser optionale DDoS-Minderungsdienst hilft Ihnen, eine Anwendung zu schützen, die in jeder AWS-Region gehostet wird. Der Service ist weltweit für Amazon CloudFront und Amazon Route 53 verfügbar. Es ist auch in ausgewählten AWS-Regionen für Classic Load Balancer (CLB), Application Load Balancer (ALB) und Elastic IP Adressen (EIPs) verfügbar. Durch die Verwendung von AWS Shield Advanced mit EIPs können Sie Network Load Balancer (NLBs) oder Amazon EC2-Instanzen schützen.
1. Zu den wichtigsten Überlegungen zur Minderung volumetrischer DDoS-Angriffe gehören die Sicherstellung, dass genügend Transitkapazität und Vielfalt verfügbar sind, und der Schutz Ihrer AWS-Ressourcen wie Amazon EC2-Instances vor Angriffsverkehr
1. Große DDoS-Angriffe können die Kapazität einer einzelnen Amazon EC2-Instance überfordern, so dass das Hinzufügen von Load Balancing Ihre Ausfallsicherheit unterstützen kann.
1. Mit Amazon CloudFront können Sie statische Inhalte zwischenspeichern und von AWS Edge-Standorten aus bereitstellen, was dazu beitragen kann, die Belastung Ihrer Herkunft zu reduzieren.
1. Mithilfe von AWS WAF können Sie Webzugriffskontrolllisten (Web-ACLs) auf Ihren CloudFront-Distributionen oder Application Load Balancers konfigurieren, um Anfragen basierend auf Anforderungssignaturen zu filtern und zu blockieren.
### Reduktion der Angriffsfläche
1. Sie können Sicherheitsgruppen angeben, wenn Sie eine Instance starten, oder Sie können die Instance zu einem späteren Zeitpunkt einer Sicherheitsgruppe zuordnen. Der gesamte Internetverkehr zu einer Sicherheitsgruppe wird implizit verweigert, es sei denn, Sie erstellen eine Zulassungsregel, die den Datenverkehr zulässt.
* Wenn Sie AWS Shield Advanced abonniert haben, können Sie Elastic IPs (EIPs) als Geschützte Ressourcen registrieren.
1. Wenn Sie Amazon CloudFront mit einem Ursprung in Ihrer VPC verwenden, sollten Sie eine AWS Lambda-Funktion verwenden, um Ihre Sicherheitsgruppenregeln automatisch zu aktualisieren, um nur Amazon CloudFront-Datenverkehr zu ermöglichen.
1. Wenn Sie eine API der Öffentlichkeit zugänglich machen müssen, besteht in der Regel die Gefahr, dass das API-Frontend durch einen DDoS-Angriff ins Visier genommen wird. Um das Risiko zu reduzieren, können Sie Amazon API Gateway als Einstieg für Anwendungen verwenden, die auf Amazon EC2, AWS Lambda oder anderswo ausgeführt werden.
* Wenn Sie Amazon CloudFront und AWS WAF mit Amazon API Gateway verwenden, konfigurieren Sie die folgenden Optionen:
1. Konfigurieren Sie das Cache-Verhalten für Ihre Distributionen, um alle Header an den regionalen Endpunkt des API-Gateways weiterzuleiten. Auf diese Weise behandelt CloudFront den Inhalt als dynamisch und überspringt das Zwischenspeichern des Inhalts.
1. Schützen Sie Ihr API-Gateway vor direktem Zugriff, indem Sie die Distribution so konfigurieren, dass sie den x-API-Schlüssel für den ursprünglichen benutzerdefinierten Header enthält, indem Sie den API-Schlüsselwert in API Gateway festlegen.
1. Schützen Sie Ihr Backend vor übermäßigem Datenverkehr, indem Sie Standard- oder Burst-Rate-Limits für jede Methode in Ihren RestAPIs konfigurieren.

### Betriebstechniken
Wenn eine wichtige Betriebsmetrik erheblich vom erwarteten Wert abweicht, versucht ein Angreifer möglicherweise, die Verfügbarkeit Ihrer Anwendung ins Visier zu nehmen. Wenn Sie mit dem normalen Verhalten Ihrer Anwendung vertraut sind, können Sie schneller Maßnahmen ergreifen, wenn Sie eine Anomalie feststellen.
1. Wenn Sie Ihre Anwendung entworfen haben, indem Sie der DDOS-resilient-Referenzarchitektur folgen, werden allgemeine Angriffe auf Infrastrukturschichten blockiert, bevor Sie Ihre Anwendung erreichen.
1. Wenn Sie AWS WAF verwenden, können Sie CloudWatch verwenden, um die Zunahme von Anforderungen, die Sie in WAF festgelegt haben, zu überwachen und zu alarmieren, um zugelassen, gezählt oder blockiert zu werden. Auf diese Weise können Sie eine Benachrichtigung erhalten, wenn der Datenverkehr übersteigt, was Ihre Anwendung verarbeiten kann.

Eine Beschreibung der Amazon CloudWatch-Metriken, die häufig zum Erkennen und Reagieren auf DDoS-Angriffe verwendet werden, finden Sie in der Tabelle in der [DDoS-Sichtbarkeit] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)
### Allgemeine Sicherheitslage
Führen Sie eine [Self-Service-Sicherheitsbewertung] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) gegen die Umgebung durch, um andere Risiken und möglicherweise andere öffentliche Exposition, die in diesem Playbook nicht identifiziert wurden, weiter zu identifizieren.

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder benötige ich einen dokumentierten Eskalationspfad sowohl intern als auch extern zu AWS

## Aktuelle Backlog-Artikel