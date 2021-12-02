# Incident Response Playbook: Lösegeld-Antwort für S3
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnisse und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Ausgenutzte Schwachstellen**
* **Exploits und Tools beobachtet**
* **Absichten des Schauspieler**
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
1. [**PREPARATION**] Verwenden Sie AWS Config, um die Konformität der Konfiguration anzuzeigen
2. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
3. [**ERKENNUNG UND ANALYSE**] Erkennung durchführen und analysieren CloudTrail auf unerkannte API
4. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsverfahren aus

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Bild] (/images/nist_life_cycle.png) ***

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Ransom & Data Destruction
* **Kategorie**: Lösegeldangriff
* **Ressource**: S3
* **Indikatoren**: Cyber Threat Intelligence, Hinweis Dritter, Cloudwatch-Metriken
* **Protokollquellen**: S3-Serverprotokolle, S3 Access Logs, CloudTrail, CloudWatch, AWS Config
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Handhabung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

## Zusammenfassung
Dieses Playbook beschreibt den Prozess für Reaktionen auf Ransom Angriffe auf AWS Simple Storage Service (S3).

Weitere Informationen finden Sie im [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Vorbereitung
* Bewerten Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
* AWS hat ein neues Open Source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) Tool entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten konto.
* Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen einschließlich Server, Netzwerkgeräte, Netzwerk-/Dateifreigaben und Entwicklermaschinen
* Erwägen Sie die Verwendung von [AWS Config] (https://aws.amazon.com/config/), einem Service, mit dem Sie die Konfigurationen Ihrer AWS-Ressourcen bewerten, prüfen und bewerten können
* Erwägen Sie, [AWS GuardDuty] (https://aws.amazon.com/guardduty/) zu implementieren, um kontinuierlich auf bösartige Aktivitäten und nicht autorisiertes Verhalten zu überwachen, um Ihre in Amazon S3 gespeicherten AWS-Konten, Workloads und Daten zu schützen
* Darkbit bietet ein Beispiel für [AWS S3 Data Loss Prevention] (https://darkbit.io/blog/simple-dlp-for-amazon-s3), mit dem das unbefugte Kopieren von Objekten identifiziert werden kann. Andere spezifische Vorgänge können in das Muster auch basierend auf Ihren Geschäftsanwendungsfällen hinzugefügt werden
* Verwenden Sie IAM-Rollen, um Berechtigungen zu verwalten
* Implementieren Sie die geringsten Berechtigungen und lassen Sie keine s3: * Berechtigungen zu
* Implementieren Sie [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) einschließlich Ablauf von Konten und obligatorischen Rotationen von Anmeldeinformationen
* Multi-Faktor-Authentifizierung (MFA) erzwingen
* Anforderungen an die Passwortkomplexität durchsetzen und Ablaufzeiträume festlegen
* Führen Sie einen [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) aus, um alle Benutzer in Ihrem Konto und den Status ihrer verschiedenen Anmeldeinformationen aufzulisten, einschließlich Passwörter, Zugriffsschlüssel und MFA-Geräte
* Verwenden Sie [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html), um die Ressourcen in Ihrer Organisation und Ihren Konten zu identifizieren, wie Amazon S3 S3-Buckets oder IAM-Rollen, die mit einer externen Entität geteilt werden. Auf diese Weise können Sie unbeabsichtigten Zugriff auf Ihre Ressourcen und Daten identifizieren, was ein Sicherheitsrisiko darstellt
* [S3 Object Versioning aktivieren] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html), um die Wiederherstellung geänderter Objekte zu ermöglichen
* Um die Anzahl der beibehaltenen Versionen festzulegen, legen Sie eine Lebenszyklusrichtlinie (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) fest, die auf nicht aktuelle Versionen angewendet werden soll
* [Aktivieren Sie S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html), um zu verhindern, dass ein Angreifer die Versionierung und das Überschreiben aller Objekte in einem Bucket deaktiviert
* Erwägen Sie, [S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) zu verwenden, damit Sie Objekte mit einem Write-Once-Read-Many (WORM) -Modell speichern können. Object Lock kann dazu beitragen, zu verhindern, dass Objekte für eine bestimmte Zeit oder unbestimmte Zeit gelöscht oder überschrieben werden.
* Im Object Lock-Compliance-Modus* kann * eine Version des geschützten Objekts von keinem Benutzer überschrieben oder gelöscht werden, einschließlich des Root-Benutzers in Ihrem AWS-Konto. Wenn ein Objekt im Compliance-Modus gesperrt ist, kann sein Aufbewahrungsmodus nicht geändert werden, und seine Aufbewahrungsfrist kann nicht verkürzt werden. Der Konformitätsmodus *stellt sicher, dass eine Objektversion für die Dauer des Aufbewahrungszeitraums nicht überschrieben oder gelöscht werden kann.
* Erwägen Sie, [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) für Objektsicherungen und Kostenoptimierung zu verwenden
* Bucket-Richtlinien verwenden und routinemäßig prüfen
* Machen Sie nur öffentlich, was sein muss, und stellen Sie sicher, dass alle Objekte geschützt sind, indem sie privat sind
* Erwägen Sie, [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) Schlüssel zu verwenden, um alle Objekte zu verschlüsseln und zu verhindern, dass ein Angreifer seinen Verschlüsselungsschlüssel anwendet
* Erwägen Sie, die [AWS S3 Funktion für öffentlichen Zugriff blockieren] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) zu verwenden, um die unbeabsichtigte Belichtung von Objekten zu verringern
* Aktivieren Sie die [CloudTrail-Ereignisprotokollierung für S3-Buckets und Objekte] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) mit sensiblen oder kritischen Informationen. Standardmäßig protokollieren CloudTrail-Trails keine Datenereignisse, Sie können jedoch Trails konfigurieren, um Datenereignisse für von Ihnen angegebene S3-Buckets zu protokollieren oder Datenereignisse für alle Amazon S3 S3-Buckets in Ihrem AWS-Konto zu protokollieren.
* Aktivieren Sie die [CloudTrail-Server-Logging für S3-Buckets] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) und Objekte, die sensible oder kritische Informationen enthalten. Die Serverzugriffsprotokollierung liefert detaillierte Datensätze für die Anfragen, die an einen Bucket gestellt werden
* Bei geschäftskritischen Datenuploads sollten Sie [Replikation aktivieren] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) in Betracht ziehen - dieselbe Region oder regionsübergreifende Region. Regionsübergreifende Replikation schützt Daten vor Anwendungsfehlern und Bedienfehlern, indem eine zweite Kopie in einer anderen Region verwaltet wird
* Ergreifen Sie Schritte, um [zu verhindern, dass neue Anmeldeinformationen öffentlich veröffentlicht werden] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
* Obwohl wir bestimmte Lösungen von Drittanbietern nicht empfehlen können, gibt es auch Produkte von unseren Partnern, einschließlich Veritas und CommVault, und anderen Backup-Softwareanbietern, die die native Fähigkeit haben, Objekte in S3 an ihren verwalteten Backup-Speicherorten innerhalb oder außerhalb von S3 zu sichern

### Verwenden Sie AWS Config, um die Konformität der Konfiguration anzuzeigen:
1. Melden Sie sich bei der AWS Management Console an und öffnen Sie die AWS Config-Konsole unter https://console.aws.amazon.com/config/
1. Stellen Sie im Menü der AWS Management Console sicher, dass die Regionsauswahl auf eine Region festgelegt ist, die AWS Config-Regeln unterstützt. Eine Liste der unterstützten Regionen finden Sie unter AWS Config-Regionen und Endpoints in der allgemeinen Referenz von Amazon Web Services
1. Wählen Sie im Navigationsbereich Ressourcen aus. Auf der Seite Ressourcenbestand können Sie nach Ressourcenkategorie, Ressourcentyp und Konformitätsstatus filtern. Wählen Sie gegebenenfalls Gelöschte Ressourcen einbeziehen aus. In der Tabelle werden die Ressourcenkennung für den Ressourcentyp und den Ressourcenkonformitätsstatus für diese Ressource angezeigt. Die Ressourcenkennung könnte eine Ressourcen-ID oder ein Ressourcenname sein
1. Wählen Sie eine Ressource aus der Spalte „Ressourcenkennung“
1. Wählen Sie die Schaltfläche „Ressourcen-Zeitleiste“. Sie können nach Konfigurationsereignissen, Compliance-Ereignissen oder CloudTrail-Ereignissen filtern
1. Konzentrieren Sie sich insbesondere auf die folgenden Ereignisse:
* s3-account-level-public-access-blocks
* s3-account-level-public-access-blocks-periodic
* s3-bucket-blacklisted-actions-prohibited
* s3-bucket-default-lock-enabled
* s3-bucket-level-public-access-prohibited
* s3-bucket-logging-enabled
* s3-bucket-policy-grantee-check
* s3-bucket-policy-not-more-permissive
* s3-bucket-public-read-prohibited
* s3-bucket-public-write-prohibited
* s3-bucket-replication-enabled
* s3-bucket-server-side-encryption-enabled
* s3-bucket-ssl-requests-only
* s3-bucket-versioning-enabled
* s3-default-encryption-kms

## Eskalationsverfahren
- „Ich brauche eine Geschäftsentscheidung darüber, wann S3-Forensik durchgeführt werden soll“
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Erkennung und Analyse
* S3-Objekte werden gelöscht oder ganze S3-Buckets werden gelöscht
* Hinweis: Bei Ereignissen zur Datenvernichtung kann ein Lösegeldschein bereitgestellt werden oder nicht. Stellen Sie außerdem sicher, dass Sie CloudWatch-Metriken und CloudTrail S3-Ereignisse überprüfen, um zu überprüfen, ob die Datenexfiltration stattgefunden hat oder nicht, um zwischen einem Lösegeld- oder Datenvernichtungsangriff abzugrenzen
* S3-Objekte werden mit einem Schlüssel von einem Konto verschlüsselt, das nicht dem Kunden gehört
* Erpresserbrief, der entweder als Objekt im Bucket oder per E-Mail an den Kunden bereitgestellt wird
* Überprüfen Sie Ihr CloudTrail-Protokoll auf nicht genehmigte Aktivitäten wie das Erstellen nicht autorisierter IAM-Benutzer, Richtlinien, Rollen oder temporäre Sicherheitsanmeldeinformationen
* Überprüfen Sie CloudTrail auf nicht erkannte API-Aufrufe. Suchen Sie insbesondere nach folgenden Ereignissen:
* DeleteBucket
* DeleteBucketCors
* DeleteBucketEncryption
* DeleteBucketLifecycle
* DeleteBucketPolicy
* DeleteBucketReplication
* DeleteBucketTagging
* DeleteBucketPublicAccessBlock
* Wenn S3-Serverzugriffsprotokolle aktiviert sind, suchen Sie nach hohen, sequentiellen `REST.COPY.OBJECT_GET` von derselben Remote-IP und demselben Requester.
* Überprüfen Sie Ihr CloudTrail-Protokoll, um Ihr AWS-Konto auf unbefugte AWS-Nutzung wie nicht autorisierte EC2-Instanzen, Lambda-Funktionen oder EC2-Spot-Gebote zu überprüfen. Sie können die Nutzung auch überprüfen, indem Sie sich bei Ihrer AWS Management Console anmelden und jede Serviceseite überprüfen. Die Seite „Rechnungen“ in der Rechnungskonsole kann auch auf unerwartete Nutzung überprüft werden
* Bitte beachten Sie, dass eine unbefugte Nutzung in jeder Region auftreten kann und dass Ihre Konsole Ihnen möglicherweise nur eine Region gleichzeitig anzeigt. Um zwischen Regionen zu wechseln, können Sie das Dropdown-Menü in der oberen rechten Ecke des Konsolenbildschirms verwenden

## Erholung
* Es wird empfohlen, das Lösegeld nicht zu zahlen
* Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
* Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
* Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
* Besuchen Sie https://www.nomoreransom.org/, um festzustellen, ob eine Entschlüsselung für die Malware-Variante verfügbar ist, die Ihre Daten infiziert haben
* [IAM-Benutzerschlüssel löschen oder drehen] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); Sie können alle Schlüssel in Ihrem Konto drehen, wenn Sie einen bestimmten Schlüssel oder Schlüssel nicht identifizieren können, die freigegeben wurden
* [Unautorisierte IAM-Benutzer löschen] (https://console.aws.amazon.com/iam/home#users.)
* [Unautorisierte Richtlinien löschen] (https://console.aws.amazon.com/iam/home#/policies)
* [Unautorisierte Rollen löschen] (https://console.aws.amazon.com/iam/home#/roles)
* [temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden. HINWEIS: Das Löschen von IAM-Benutzern kann sich auf Produktions-Workloads auswirken und sollte mit Vorsicht durchgeführt werden* Verwenden Sie CloudEndure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
* Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, ob die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her
* Implementieren Sie Prozeduren unter Schützen, bevor Sie versuchen, Daten oder Objekte wiederherzustellen
* [Löschmarkierungen entfernen] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) für versionierte Objekte
* Gelöschte Buckets neu erstellen
* [Objekte aus der Verwendung von S3 Intelligent Tiering wiederherstellen] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) Objektsicherungen oder replizierten Regions-Bucket
* Hinweis: Es gibt derzeit keine „Undelete“ -Fähigkeit für S3, und AWS kann gelöschte Daten nicht wiederherstellen. Im gegenwärtigen Zeitalter der Einhaltung der Datenspeicherung und Vorschriften wie DSGVO (https://gdpr-info.eu/) und CCPA (https://oag.ca.gov/privacy/ccpa) kann Amazon S3 Kundendaten, die explizit aus dem Konto des Kunden gelöscht wurden, nicht weiter speichern. Sobald ein Objekt gelöscht wurde, kann es von AWS nicht mehr wiederhergestellt werden, unabhängig davon, wie schnell die unbeabsichtigte Löschung an AWS gemeldet wird

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente
- Als Incident Responder brauche ich ein Runbook, um S3 Forensics durchzuführen
- Als Incident Responder brauche ich eine Geschäftsentscheidung darüber, wann S3-Forensik durchgeführt werden soll
- Als Incident Responder muss ich die Protokollierung in allen Regionen aktiviert haben, die unabhängig von der Nutzungsabsicht aktiviert sind

## Aktuelle Backlog-Artikel