# Reaktion auf Lösegeldangriffe in AWS

## Hinweise

Dieses Dokument wird nur zu Informationszwecken zur Verfügung gestellt. Es repräsentiert
die aktuellen Produktangebote und Praktiken von Amazon Web Services
(AWS) ab dem Ausstellungsdatum dieses Dokuments, die unterliegen
ohne Vorankündigung ändern. Kunden sind dafür verantwortlich, ihre eigenen zu machen
unabhängige Bewertung der Informationen in diesem Dokument und jede Verwendung
von AWS-Produkten oder -Services, von denen jede „wie besehen“ ohne bereitgestellt wird
Garantie jeglicher Art, ob ausdrücklich oder stillschweigend. Dieses Dokument tut es nicht
Gewährleistungen, Zusicherungen, vertragliche Verpflichtungen zu schaffen,
Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder
Lizenzgeber. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden
werden von AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Bestandteil noch
ändert es, jede Vereinbarung zwischen AWS und seinen Kunden. \
\
© 2024, Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte
reserviert.

## Ransomware

Seit dem ersten aufgezeichneten Ransomware-Angriff im Jahr 1989 namens PC Cyborg,
Ransomware ist zu einer prominenten Monetarisierungsstrategie geworden, die von Kriminellen angewendet wird
Organisationen und Bedrohungsakteure im Internet. Andere Beispiele für
Ransomware gehören:

- CryptoLocker\ [2014\]
- Petya\ [2016\]
- WannaCry\ [2017\]
- NotPetya\ [2017\]
- Ryuk\ [2019\])

Bedrohungsakteure nutzen Probleme und Schwächen im gesamten Kunden
Infrastruktur, Ausnutzung gefährdeter Endpunkte, unsichere Dienste und
Socially Engineering Mitarbeiter.

Ransomware-Angriffe kosten Regierungen, gemeinnützige Organisationen und Unternehmen
Milliarden von Dollar und Unterbrechung des Betriebs. NotPetya erzwungen
Versandriesen Maersk, um 4.000 Server und 45.000 PCs neu zu installieren
\ 300 Mio. USD aufgrund „schwerwiegender Geschäftsunterbrechung“. Der Ransomware-Angriff auf
die Stadt Baltimore kostete über 18 Millionen Dollar, und lokale Regierungen aus
Riviera Beach und Lake City, Florida zahlen Hacker\ 1 Mio. $ zusammen zu
holen Sie seine Systeme und Daten zurück. Das US-amerikanische Federal Bureau of Investigation
(FBI) geht davon aus, dass die Gefahr von Ransomware „gezielter wird,
anspruchsvoll und kostspielig“ in naher Zukunft. Diese Warnungen erreichen
jenseits der US-Grenzen, wobei Europol Ransomware auch als „am meisten“ bezeichnet
weit verbreitete und finanziell schädliche Form des Cyberangriffs.“

## Was sind Lösegeldangriffe?

Lösegeldangriffe sind eine Monetarisierungsstrategie, keine bestimmte Technologie.
Angriffe nutzen oft bestimmte bösartige Software oder Ransomware, die
droht, Opferinformationen zu veröffentlichen oder den Zugriff bis zum Lösegeld zu blockieren
wird bezahlt.

Theoretisch, wenn das Lösegeld innerhalb der vorgesehenen Zeit gezahlt wird, Systeme und
Daten werden entschlüsselt und wieder verfügbar gemacht und normale Operationen
fahren Sie fort. Wenn das Lösegeld jedoch nicht erfüllt ist, riskieren Organisationen
dauerhafte Zerstörung oder öffentlich zugängliche Datenlecks, die von der
Angreifer. Angreifer können den Kunden auch weiter erpressen, um
kundensensible Daten und Systeme an Dritte oder an die Öffentlichkeit.

## Ransomware ist es egal

Viele Lösegeldangriffe sind opportunistischer Natur, was bedeutet, dass Ransomware
infiziert wahllos alle zugänglichen Netzwerke durch Menschen und/oder
maschinelle Vektoren. Sicherheitsteams für Bildungseinrichtungen, staatliche und
lokale Regierungen und Gesundheitsorganisationen verstärken Maßnahmen
um ihre Daten vor einer Zunahme von Ransomware-Angriffen zu schützen. Bedrohung
Akteure verstehen, wie Schwachstellen in Branchen identifiziert werden können. Viele
Bildungs- und Regierungsorganisationen sind anfälliger für Ransomware
zu einer Kombination aus schrumpfenden Budgets, Lücken in Sicherheitsressourcen und
ältere IT-Systeme mit bekannten nicht gepatchten Schwachstellen. In ähnlicher Weise
Lösegeldangriffe können auf Branchen mit Intoleranz für Ausfallzeiten abzielen, wie
Krankenhäuser, in der Hoffnung, die Auszahlungswahrscheinlichkeit zu erhöhen.

## Warum sind Lösegeldangriffe wirksam?

- Das Sicherheitsbewusstsein unter den Mitarbeitern ist gering
- Unternehmen sichern keine Daten oder testen keine vorhandenen Backups
- Angriffe erfordern wenig Geschick und führen zu erheblichen Auszahlungen
- Unternehmen beheben kritische gemeinsame Schwachstellen und Risikopositionen (CVE) nur langsam
- Überlastetes technisches Personal kann nicht alle Sicherheitslücken beheben oder antizipieren
- Bei einem einzigen Angriff werden mehrere Vektoren oder Kanäle verwendet
- Das Diebstahl der Daten (Datenexfiltration, unbefugtes Kopieren) stoppt möglicherweise nicht einen Geschäftsprozess, und Kunden reagieren möglicherweise erst, wenn etwas wie das Verschlüsseln oder Löschen der Informationen geschieht

## Zahlen oder nicht bezahlen?

Unter Cybersicherheitsexperten gibt es aktive Debatten über die
Entscheidung, Lösegeld zu zahlen. Viele Experten, darunter das FBI, beraten
Organisationen, die das Lösegeld nicht zahlen, argumentieren, dass die Zahlung nicht
garantieren, dass gesperrte Systeme oder verschlüsselte Daten zur Verfügung gestellt werden
und wird nur weiterhin schändliche Verhaltensweisen motivieren. Obwohl
System- und Datenzugriff wird nach Zahlung des Lösegeldes nicht garantiert, einige
Organisationen gehen ein kalkuliertes Risiko ein, um zu zahlen, in der Hoffnung, wieder normal zu werden
operationen. Auf diese Weise hoffen sie, potenzielle Nebenkosten zu senken
von Angriffen, einschließlich verlorener Produktivität, verringerten Umsatz im Laufe der Zeit,
Exposition sensibler Daten und Reputationsschäden. Die Ransomware
Bedrohung ist ernst, aber kluge Vorbereitung und anhaltende Wachsamkeit sind
wirksame Konter dagegen. Die volle Rüstung der Datensicherheit umfasst
sowohl menschliche als auch technische Kontrollen, aber es gibt Funktionen des AWS
Cloud, die hilft, Ransomware-Angriffe zu mildern. AWS ist verpflichtet
Bereitstellung von Tools, Best Practices und Services, um bei hohen
Verfügbarkeit, Sicherheit und Ausfallsicherheit, um schlechte Akteure auf der
internet.

## Die Sicherung von Systemen und Daten in AWS ist eine [gemeinsame Verantwortung] (https://aws.amazon.com/compliance/shared-responsibility-model)

Wenn Sie Anwendungen und Infrastruktur in der AWS Cloud bereitstellen, wird AWS
hilft, indem Sie die Sicherheitsaufgaben mit Ihnen teilen. AWS Ingenieure
die zugrunde liegende Cloud-Infrastruktur unter Verwendung sicherer Designprinzipien und
Kunden müssen ihre eigene Sicherheitsarchitektur für Workloads implementieren
auf AWS bereitgestellt. AWS ist verantwortlich für den Schutz der Infrastruktur
das alle in der AWS Cloud angebotenen Dienste ausführt. Diese Infrastruktur
besteht aus Hardware, Software, Netzwerken und Einrichtungen, die
führen Sie AWS Cloud-Dienste aus. Die AWS Cloud-Services, die ein Kunde auswählt
Bestimmen Sie den Umfang der Konfigurationsarbeit, die der Kunde ausführen muss
Teil ihrer Sicherheitsaufgaben. Kunden sind immer
verantwortlich für die Verwaltung und Sicherung ihrer Daten und Klassifizierung
Assets und Verwendung von AWS Identity Access Management (IAM) zur Anwendung des
entsprechende Berechtigungen.

## AWS-Support

Wenn Sie oder Ihre Kunden glauben, dass ein Konto unter einem aktiven Lösegeld steht
Angriff sollte der betroffene Kunde einen Fall aus dem AWS Support erstellen
Zentrieren Sie innerhalb des betroffenen Kontos. Wenn der Root-Benutzer nicht sein kann
zugegriffen oder ist kompromittiert, dann sollte der Kunde ein neues AWS eröffnen
Konto und eröffnen Sie von dort aus ein Support-Ticket. Dieser Prozess ermöglicht es AWS
führen Sie die Identitätsüberprüfung mit dem Kunden durch und beginnen Sie sich mit
interne Ressourcen, um am besten bei der Behebung des Ereignisses zu helfen.

## AWS Security Reference Architecture (AWS SRA)

Die [Amazon Web Services (AWS) Sicherheitsreferenzarchitektur (AWS)
SRA)] (https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
ist eine ganzheitliche Reihe von Richtlinien für die Bereitstellung der vollständigen Ergänzung von AWS
Sicherheitsdienste in einer Umgebung mit mehreren Konten. Es kann verwendet werden, um zu helfen
entwerfen, implementieren und verwalten Sie AWS-Sicherheitsdienste so, dass sie sich ausrichten
mit bewährten Methoden von AWS.

## US-amerikanische Agentur für Cybersicherheit und Infrastruktur

Der [Ransomware-Leitfaden (Sept.
2020)] (https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf)
bietet Best Practices und Referenzen zur Bewältigung des Risikos von
Ransomware und Unterstützung der koordinierten und effizienten Organisation
Reaktion auf einen Ransomware-Vorfall.

## Die Agentur der Europäischen Union für Cybersicherheit (ENISA)

Die [ENISA-Bedrohungslandschaft 2020 -
Ransomware] (https://www.enisa.europa.eu/publications/ransomware)
skizziert die Ergebnisse zu Ransomware, bietet eine Beschreibung und Analyse
der Domain und listet relevante aktuelle Vorfälle auf. Eine Reihe von vorgeschlagenen
Maßnahmen zur Abschwächung werden bereitgestellt.

## Australisches Cyber-Sicherheitszentrum (ACSC)

Der ACSC bietet mehrere Leitfäden einschließlich Reaktion auf [Ransomware in
Australien] (https://www.cyber.gov.au/sites/default/files/2020-10/Ransomware%20in%20Australia%20%28October%202020%29.pdf), [RANSOMWARE
GREIFT NOTFALLMASSNAHMEN AN
LEITFADEN] (https://www.cyber.gov.au/sites/default/files/2024-07/11515_ACSC_Emergency-Response-Guide_Accessible_08.12.20.pdf),
und [SCHUTZ UND SCHUTZ VON RANSOMWARE-ANGRIFFEN
LEITFADEN] (https://www.cyber.gov.au/sites/default/files/2024-07/11515_ACSC_Prevention-And-Protection-Guide_Accessible_08.12.20.pdf).

## NIST-Cybersicherheits-Framework

Der Leitfaden, NIST Cybersecurity Framework (CSF): Ausrichtung auf den NIST-CSF
in der AWS Cloud wurde entwickelt, um den kommerziellen und öffentlichen Sektor zu unterstützen
Entitäten jeder Größe und in jedem Teil der Welt stimmen mit dem Liquor überein
Verwendung von AWS-Services und -Ressourcen

## Ransomware-Minderung: Top 5 Maßnahmen zum Schutz und zur Vorbereitung der Wiederherstellung

AWS hat einen öffentlichen Sicherheitsblog zur Verfügung gestellt, der [Ransomware-Minderung:
Top 5 Schutzmaßnahmen und Erholungsvorbereitung
Aktionen.] (https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)
Dazu gehören:

- Richten Sie die Möglichkeit ein, Ihre Apps und Daten wiederherzustellen
- Verschlüsseln Sie Ihre Daten
- Wenden Sie kritische Patches an
- Folgen Sie einem Sicherheitsstandard
- Stellen Sie sicher, dass Sie die Antworten überwachen und automatisieren

Sie sollten auch eine starke Authentifizierung für exponierten Remote-Zugriff verwenden
Dienste, insbesondere Remote-Desktop-Protokoll und sichere Shell. [Multifaktor
Authentifizierung kombiniert mit Single
anmelden] (https://docs.aws.amazon.com/singlesignon/latest/userguide/mfa-how-to.html)
ist ein technischer Mechanismus, der den Schutz von Remote-Verbindungen ermöglicht
und Ressourcenanfragen.

## Entwicklung, Sicherheit & Betrieb (DevSecOps)

*Hintergrund*: Der Zweck von DevSecOps ist es, Kunden sicher zu helfen
beschleunigen Feedbackschleifen mit ihren Kunden (d. h. Endbenutzer). Von
Durch die Beschleunigung dieser Feedbackschleifen können Kunden mehr Software liefern
Änderungen der Produktion mit höherer Qualität, Sicherheit und Stabilität. Von
Einbau von Sicherheit in den Entwicklungsprozess, können Kunden verhindern
Bereitstellung von Schwachstellen für Endbenutzer in Produktionsumgebungen.
Laut NIST sind die Kosten für die Behebung eines Sicherheitsfehlers, sobald er
macht es in die Produktion kann bis zu 60-mal teurer sein als während
der Entwicklungszyklus
\ [[Quelle] (https://securityboulevard.com/2020/09/the-importance-of-fixing-and-finding-vulnerabilities-in-development/)\].

Basierend auf Daten des AWS Customer Incident Response Teams
Lösegeldangriffe von Kunden fallen in eine von drei Kategorien: 1/ Ein schlechtes
actor durchsucht Quellcode-Repositorys (z. B. GitHub) nach AWS-Zugriffsschlüsseln.
Mit diesen langfristigen Zugriffsschlüsseln erhalten sie Zugriff auf AWS-Ressourcen in
Konten der Kunden. Risiken steigen basierend auf dem Zugriffsebene
durch die Zugriffsschlüssel zulässig. 2/ Durch das Scannen öffentlicher S3-Buckets und
Objekte, ein schlechter Akteur kann den Zugriff blockieren und S3-Buckets verschlüsseln
Besitzer vom Zugriff auf die Daten. 3/ Ein schlechter Akteur scannt und nutzt das Internet
Sicherheitslücken in Anwendungen (z. B. Code-Injection und Cross-Site
Scripting) auf öffentlichen Ports, um Zugriff auf Kundendaten zu erhalten.

Kunden glauben fälschlicherweise, dass AWS viele dieser Sicherheitsvorkehrungen bietet
automatisch im Rahmen eines AWS-Kontos. Während AWS bietet
Tausende von Sicherheitsfunktionen einschließlich standardmäßig privater Funktionen
(z. B. EC2-Sicherheitsgruppen und S3-Buckets) als Teil des [Shared
Verantwortung
Modell] (https://aws.amazon.com/compliance/shared-responsibility-model/),
Kunden müssen Sicherheitslücken in ihrem
Code und/oder in ihren AWS-Umgebungen. Es gibt AWS-Services und Tools
zur Erkennung und Behebung dieser nicht konformen Ressourcen aber
Kunden müssen Kenntnisse in der Anwendung der Praktiken in einer automatischen
weit über ihre AWS-Ressourcen hinweg.

Eine Bereitstellungspipeline bricht Build-, Test-, Bereitstellungs- und Freigabeaktionen auf
in eine Reihe von Etappen. Jede Phase bietet zunehmendes Vertrauen,
normalerweise auf Kosten der Verlängerung. Frühe Phasen können die meisten Probleme finden
liefert schnelleres Feedback, während spätere Phasen langsamer und mehr liefern
gründliches Sondieren. Bauherren automatisieren viele der Aktionen in diesen
Bereitstellungspipelines; Bereitstellungspipelines sind eine wichtige Architektur
Konstrukt von [Kontinuierlich
Lieferung] (https://aws.amazon.com/devops/continuous-delivery/). Die
Die Aufgabe der Bereitstellungspipeline besteht darin, Änderungen zu erkennen, die dazu führen
Probleme in der Produktion. Dazu können Leistung, Sicherheit oder
Probleme mit der Benutzerfreundlichkeit
\ [[Quelle] (https://martinfowler.com/bliki/DeploymentPipeline.html)\].
Für die Zwecke dieses Dokuments beziehen wir uns auf die Baupraxis
Sicherheitstests und Checks in Bereitstellungspipelines, damit sie verhindern können
und mildern Lösegeldangriffe als Continuous Security.

Die Prinzipien der Verwendung von Continuous Security zur Verhinderung und
mildernde Lösegeldangriffe sind: 1/ Verhindern Sie, Geheimnisse zur Quelle zu begehen
Code-Repositorys, damit schlechte Akteure keine Schlüssel für den Zugriff auf AWS haben
ressourcen. 2/ Verhindern Sie häufige Entwicklerfehler, die das Web machen
anfällige Anwendungen (statisch und Laufzeit). 3/ Stellen Sie sicher, dass es
Backups für kompromittierte AWS-Ressourcen, damit schlechte Akteure nicht
viel zu gewinnen, wenn der Zugriff auf die Ressourcen blockiert wird. 4/ Alle verschlüsseln
relevante Daten, damit ein schlechter Akteur Zugriff auf Daten erhält,
viel weniger durch Erpressung zu gewinnen. 5/ Erkennen Sie, wenn nicht autorisierte Benutzer
greifen Sie auf AWS-Ressourcen zu, damit Minderungen stattfinden können. 6/ Stellen Sie sicher, dass IAM
Zugriffsschlüssel sind mit IAM-Rollen von kurzer Dauer, daher gibt es einen eingeschränkten Zugriff
Dauer auf kompromittierte Ressourcen. 7/ Stellen Sie sicher, dass IAM-Berechtigungen am wenigsten verwenden
Privileg zu verhindern, dass wirklich schlimme Dinge passieren, wenn ein schlechter Schauspieler
erhält Zugriff auf die Schlüssel. 8/ Verhindern oder erkennen Sie Fehlkonfiguration
machen Sie AWS-Ressourcen anfällig.

*Verhindern und erkennen, wenn Geheimnisse in Quellcode-Repos***: **
Von ihren Entwicklerumgebungen (z. B. ihren IDEs) sollten Kunden
Führen Sie Secrets Detection Static Application Security Testing (SAST) -Tools
bevor Sie Code zu einem Quellcode-Repo verpflichten. Darüber hinaus ist der erste
Phase einer automatisierten Bereitstellungspipeline führt eine Secrets-Erkennung SAST
Tool, um zu erkennen, wann Secrets für das Quellcode-Repo gebunden wurden.
Konfigurieren Sie beim Erkennen von Geheimnissen die Pipeline so, dass sie fehlschlägt, benachrichtigen
Entwickler, und stoppen Sie zukünftige Commits, bis Sie einen Fix anwenden. Darüber hinaus
führen Sie die Automatisierung aus, um den Git-Verlauf der Geheimnisse zu bereinigen und das AWS zu deaktivieren
Zugriffsschlüssel. Beispieltools für diese Lösung sind AWS CodePipeline, AWS
CodeBuild, git-secrets und benutzerdefinierter Code zur Behebung von Geheimnissen.

* Verhindern und erkennen Sie häufige Entwicklerfehler, die Anwendungen machen
verwundbar***: ** Aus ihren Entwicklerumgebungen (z. B. ihren IDEs)
Kunden sollten SAST- und Code-Review-Tools ausführen, die das Web erkennen
Sicherheitslücken wie Code-Injection, Cross-Site-Scripting und andere
unsichere Kodierungspraktiken. Darüber hinaus ist die erste Stufe einer automatisierten
Bereitstellungspipeline führt diese Tools aus, um festzustellen, wann Web-Schwachstellen
sind dem Quellcode-Repo verpflichtet. Bei der Entdeckung dieses Webs
Sicherheitslücken, konfigurieren Sie die Pipeline so, dass sie fehlschlägt, Entwickler benachrichtigen und
stoppt zukünftige Commits, bis Sie einen Fix anwenden. Darüber hinaus können Kunden
Konfigurieren der Laufzeiterkennung und Behebung dieser Web-Schwachstellen
als Teil einer Shared Services-Bereitstellungspipeline, die
konfiguriert diese Dienste als Code. Beispieltools für diese Lösung sind
Amazon CodeGuru, AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
SonarQube/CheckMarx und AWS WAF und WAF Security Automations.

*Backups für kompromittierte AWS-Ressourcen: * Erkennen Sie zur Laufzeit nicht markierte
und/oder relevante AWS-Ressourcen, die keine Backups haben. Darüber hinaus
Kunden können die Bereitstellung von AWS-Ressourcen konfigurieren, die
Laufzeiterkennung im Rahmen einer Shared Services-Bereitstellungspipeline, die
stellt diese Dienste bereit und konfiguriert sie als Code. Beispiel-Tools dafür
-Lösung sind AWS Backup, AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Regeln] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(z.B. required-tags, redshift-backup-enabled,
aurora-resources-protected-by-backup-plan,
backup-plan-min-frequency-and-min-retention-check,
backup-recovery-point-manual-deletion-disabled,
db-instance-backup-enabled, dynamodb-in-backup-plan,
ebs-in-backup-plan), Amazon SNS, and -.

*Daten während der Übertragung und in Ruhe verschlüsseln***: ** Von ihrem Entwickler
Umgebungen (z. B. ihre IDEs) sollten Kunden SAST-Tools ausführen, die
erkennen Web-Schwachstellen wie Code-Injection und Cross-Site
Skripten. Darüber hinaus ist die erste Stufe einer automatisierten Bereitstellung
pipeline führt ein iAC Linting Tool aus, um zu erkennen, wann Entwickler definiert haben
unverschlüsselte AWS-Ressourcen als Code und verpflichten den Konfigurationscode
das Quellcode-Repo. Wenn Sie diese unverschlüsselten Ressourcen entdecken,
konfigurieren Sie die Pipeline so, dass sie fehlschlägt, Entwickler benachrichtigt und die Zukunft beendet
verpflichtet sich, bis ein Fix angewendet wird. Darüber hinaus können Kunden die
Bereitstellung von AWS-Ressourcen, die Laufzeiterkennung ermöglichen und
Behebung dieser unverschlüsselten AWS-Ressourcen im Rahmen einer gemeinsam genutzten
Bereitstellungspipeline für Dienste, die diese bereitstellt und konfiguriert
Dienste als Code. Beispieltools für diese Lösung sind AWS
CloudFormation, AWS CodePipeline, AWS CloudFormation Guard, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Regeln] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(z. B. ec2-ebs-encryption-by-default, dynamodb-table-encrypted-kms,
rds-snapshot-encrypted, cloudwatch-log-group-encrypted,
cloud-trail-encryption-enabled,
s3-bucket-server-side-encryption-enabled, api-gw-ssl-enabled,
cloudfront-custom-ssl-certificate), Amazon SNS, AWS KMS und AWS Lambda.

*Erkennen Sie, wenn nicht autorisierte Benutzer auf AWS-Ressourcen zugreifen: * Zur Laufzeit
Erkennen von Datenexfiltration\ "Spikes\“ (z. B. mit CloudWatch-Metriken
speziell
[NetworkOut] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)).
Zum Beispiel ist es möglich, dass ein Angreifer die Datenvernichtung durchführte und
hinterließ einen Lösegeldschein, und in diesen Fällen besteht keine Möglichkeit für Daten
Erholung durch die Zusammenarbeit mit dem böswilligen Akteur. Was ist mehr, Kunden
kann die Bereitstellung von AWS-Ressourcen konfigurieren, die Laufzeit bereitstellen
Erkennung im Rahmen einer Shared Services-Bereitstellungspipeline, die
stellt diese Dienste bereit und konfiguriert sie als Code. Beispiel-Tools dafür
-Lösung sind AWS CloudFormation, AWS CodePipeline, AWS CodeBuild, Amazon
EventBridge, Amazon GuardDuty, Amazon SNS und Amazon CloudWatch
Metriken.

*Stellen Sie sicher, dass die IAM-Zugriffsschlüssel kurzlebig***: ** Führen Sie zur Laufzeit Tools aus, die
erkennt verschiedene Zustände von IAM-Zugriffsschlüsseln und -richtlinien einschließlich Rotation
langlebige Zugangsschlüssel. Kunden können die Bereitstellung von AWS konfigurieren
Ressourcen, die Laufzeiterkennung als Teil eines Shared Services bereitstellen
Bereitstellungspipeline, die diese Dienste als bereitstellt und konfiguriert
code. Beispieltools für diese Lösung sind AWS CloudFormation, AWS
CodePipeline, AWS CodeBuild, Amazon EventBridge, [AWS Config
Regeln] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(z. B. iam-policy-no-statements-with-admin-access,
iam-policy-no-statements-with-full-access, iam-root-access-key-check,
iam-user-no-policies-check, iam-user-unused-credentials-check,
access-keys-rotated) und AWS Lambda.

*Stellen Sie sicher, dass IAM-Berechtigungen das geringste Privileg verwenden***: ** Von Entwicklern
Umgebungen (z. B. ihre IDEs) sollten Kunden SAST-Tools ausführen, die
erkennen Sie übermäßig zulässige Berechtigungsdefinitionen vor dem Festlegen
Code zu einem Quellcode-Repo. Darüber hinaus ist die erste Stufe einer automatisierten
Bereitstellungspipeline führt das SAST-Tool aus, um übermäßig permissive zu erkennen
Richtliniendefinitionen, nachdem der Code für einen Quellcode gebunden wurde
Repo. Wenn Sie übermäßig zulässige Richtliniendefinitionen ermitteln, konfigurieren Sie
die Pipeline zum Scheitern, benachrichtigt Entwickler und stoppt zukünftige Commits bis
einen Fix anwenden. Erkennen Sie darüber hinaus übermäßig zulässige Richtlinien in der
AWS-Konto oder -Konten (d. h. außerhalb von Quellcode-Repos). Im Gegenzug
Kunden können die Bereitstellung von AWS-Ressourcen konfigurieren, die
Laufzeiterkennung im Rahmen einer Shared Services-Bereitstellungspipeline, die
stellt diese Dienste bereit und konfiguriert sie als Code. Beispiel-Tools dafür
-Lösung sind AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
pMapper/AWSPX, AWS IAM Access Analyzer, Amazon EventBridge, AWS Config
Regeln und AWS Lambda.

*Fehlkonfiguration verhindern, die AWS-Ressourcen anfällig machen*: Der
Fehlkonfiguration von oder Nichtaktivierung bestimmter AWS-Ressourcen kann
Kunden, die anfällig für einen Lösegeld-Angriff sind Die bemerkenswertesten Ressourcen sind
diejenigen, die sich auf Netzwerke, Computer, Speicher und Datenbanken beziehen. Was ist
mehr können Kunden die Bereitstellung von AWS-Ressourcen konfigurieren, die
Bereitstellung einer Laufzeiterkennung im Rahmen einer Shared Services-Bereitstellung
Pipeline, die diese Dienste als Code bereitstellt und konfiguriert. Beispiel
Tools für diese Lösung sind AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, AWS Config Rules (z.
s3-account-level-public-access-blocks, s3-bucket-default-lock-enabled,
s3-bucket-level-public-access-prohibited, s3-bucket-logging-enabled,
s3-bucket-policy-not-more-permissive, s3-bucket-public-read-prohibited,
s3-bucket-public-write-prohibited, s3-bucket-replication-enabled,
cloud-trail-log-file-validation-enabled,
ec2-security-group-attached-to-eni, vpc-default-security-group-closed,
und vpc-flow-logs-enabled, guardduty-enabled-centralized), Amazon
Inspector Network Erreichbarkeit, Amazon VPC Reachability Analyzer, Amazon
S3 und AWS Lambda.

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Microsoft Windows

### Identifizieren
- Beurteilen Sie die Sicherheitslage des Kontos, um Sicherheitslücken zu identifizieren und zu beheben
- AWS hat ein neues Open Source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) Tool entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten konto.
- Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen, einschließlich Domänencontroller, Microsoft Windows EC2-Instanzen, Microsoft Windows Windows-Server und Datenbanken sowie jegliche Integration mit externen Identitätsanbietern.
- Führen Sie wiederkehrende Schwachstellenanalysen Ihrer Hosts mithilfe von Dienstprogrammen wie [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/) durch
- Zum Schutz Ihrer Organisation empfiehlt Microsoft, die Informationen in der [**vom Menschen betriebenen Ransomware Mitigation Project Plan**] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) PowerPoint-Präsentation zu verwenden, die [Sichern privilegierter Zugriff] (https://aka.ms/spa).
- Verwenden Sie [CloudWatch-Metriken] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), insbesondere NetworkPacketsOut, um nach „Spikes“ der Datenexfiltration zu suchen. Es ist möglich, dass ein Angreifer die Datenvernichtung durchführte und einen Lösegeldschein hinterlassen hat, und in diesen Fällen gibt es keine Möglichkeit zur Datenrettung durch die Zusammenarbeit mit dem böswilligen Akteur

### Schützen
- Wenden Sie die neuesten Updates auf Ihre Betriebssysteme und Apps an
- Sichern Sie Ihre Dateien mit dem Dateiverlauf, wenn der Hersteller Ihres PCs sie noch nicht eingeschaltet hat. [Erfahren Sie mehr über den Dateiverlauf] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
- Sichern Sie wichtige Dateien regelmäßig. Verwenden Sie die 3-2-1-Regel. Behalten Sie drei Backups Ihrer Daten auf zwei verschiedenen Speichertypen und mindestens ein Backup außerhalb des Standorts auf
- Blockieren Sie bekannte Ransomware-Dateitypen
- [Makros in Office-Dokumenten blockieren] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
- Informieren Sie Ihre Mitarbeiter, damit sie Social Engineering- und Spear-Phishing-Angriffe identifizieren können
- [Befolgen Sie Best Practices zum Sichern Ihrer Active Directory-Dienste] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
- [Befolgen Sie die 10 wichtigsten Gruppenrichtlinieneinstellungen zur Verhinderung von Sicherheitsverletzungen] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
- [Kontrollierten Ordnerzugriff implementieren] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). Es kann verhindern, dass Ransomware Dateien verschlüsselt und die Dateien für Lösegeld hält
- Durchführen von Backups von EC2-Instanzen
- Erwägen Sie, [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) oder [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/) zu verwenden
- Das Microsoft 365 Defender Threat Intelligence Team hat [umfassenden Bericht] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) zur Identifizierung von Aktionen zum Schutz Ihrer Microsoft-Ressourcen vor einem Ransomware-Ereignis bereitgestellt
- Verhärten Sie das internetorientierte Assets und stellen Sie sicher, dass sie über die neuesten Sicherheitsupdates verfügen. Verwenden Sie Bedrohungs- und Schwachstellenmanagement, um diese Assets regelmäßig auf Schwachstellen, Fehlkonfigurationen und verdächtige Aktivitäten zu überprüfen.
- Überwachen Sie auf Brute-Force-Versuche. Übermäßige fehlgeschlagene Authentifizierungsversuche prüfen (Windows Sicherheitsereignis ID 4625)
- Überwachung für das Löschen von Ereignisprotokollen, insbesondere des Sicherheitsereignisprotokolls und der PowerShell Operational-Protokolle. Microsoft Defender ATP löst die Warnung „Event log was cleared“ aus und Windows generiert in diesem Fall eine Ereignis-ID 1102
- Üben Sie das Prinzip der geringsten Privilegien ein und behalten Sie die Hygiene von Anmeldeinformationen bei. Vermeiden Sie die Verwendung von domänenweiten Dienstkonten auf Administratorebene. Erzwingen Sie starke randomisierte Just-in-Time-Administrator-Passwörter für lokale Administratoren. Verwenden Sie Tools wie LAPS
- Sichere Remotedesktop-Gateway mit Lösungen wie Azure Multi-Factor Authentication (MFA). Wenn Sie kein MFA-Gateway haben, aktivieren Sie die Authentifizierung auf Netzwerkebene (NML)
- Schalten Sie AMSI für Office VBA ein, wenn Sie Office 365 haben
- Aktivieren Sie Regeln zur Reduzierung von Angriffsoberflächen, einschließlich Regeln, die den Diebstahl von Anmeldeinformationen, Ransomware-Aktivitäten und die verdächtige Verwendung von PsExec und WMI blockieren. Verwenden Sie Regeln, die erweiterte Makroaktivitäten, die durch bewaffnete Office-Dokumente ausgelöst werden, um bösartige Aktivitäten, ausführbare Inhalte, die Prozesserstellung und die Prozessinjektion, die von Office-Anwendungen initiierte Prozessinjektion zu beheben. Um die Auswirkungen dieser Regeln zu beurteilen, stellen Sie sie im Audit-Modus bereit
- Aktivieren Sie den Cloud-bereitgestellten Schutz und die automatische Beispielübermittlung unter Windows Defender Antivirus. Diese Funktionen nutzen künstliche Intelligenz und maschinelles Lernen, um neue und unbekannte Bedrohungen schnell zu erkennen und zu stoppen
- Aktivieren Sie die Manipulationsschutzfunktionen, um zu verhindern, dass Angreifer Sicherheitsdienste stoppen
- Nutzen Sie die Windows Defender Firewall und Ihr Netzwerksicherheitssystem, um die RPC- und SMB-Kommunikation zwischen Endpunkten wann immer möglich zu verhindern. Dies begrenzt sowohl die seitliche Bewegung als auch andere Angriffsaktivitäten
- Aktivieren und Aktualisieren von Windows Defender Antivirus - kommerzielle, kostenpflichtige Endpoint Detection and Response (EDR) -Lösungen (EDR) werden bevorzugt
- Aktivieren Sie [Controlled Folder Access] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) in Windows 10, um Ihre wichtigen lokalen Ordner vor nicht autorisierten Programmen wie Ransomware oder anderer Malware zu schützen
- Verwenden Sie [Systems Manager und Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/), um zu überprüfen, ob EC2-Instanzen, auf denen Microsoft Windows ausgeführt wird, allgemeine Schwachstellen und Risikopositionen (CVes) enthalten
- Überprüfen Sie Ihre Backups und stellen Sie sicher, dass sich die Infektion nicht in sie ausgebreitet hat

### Erkennen
- Das Microsoft 365 Defender Threat Intelligence Team hat einen [umfassenden Bericht] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) bereitgestellt, in dem Dinge identifiziert werden, nach denen Sie bei mehreren Ransomware-Varianten suchen sollten
- Verwenden Sie vpcFlowLogs, um unangemessenen Datenbankzugriff von externen IP-Adressen zu identifizieren
- Sie können [VPC Flow mit Amazon Detective untersuchen] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) oder [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Reagieren
- Erzwingen Sie NACLs basierend auf Netzwerk-IOCs, um weiteren Datenverkehr zu verhindern
- Isoliere die infizierten Systeme
- Untersuchen Sie Backups auf potenzielle Infektionen
- Entfernen Sie alle kompromittierten Systeme aus dem Netzwerk.
- Befolgen Sie regulatorische Anforderungen oder interne Unternehmensrichtlinien, um festzustellen, ob eine Forensik der EC2-Instanz erforderlich ist
- Wenn Forensik der Instanzen erforderlich ist ODER Daten wiederhergestellt werden müssen, dann [Quarantäne die EC2-Instanz] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)
- [Kompromittierte Domänencontroller-Metadaten aus der Domäne entfernen] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
- Verwenden Sie Flow-Logs, um die Exfiltrations-IP zu ermitteln, suchen Sie nach anderen Instanzen, die mit den IPs kommunizieren, dass Daten zu/von CloudTrail exfiltriert wurden

### Wiederherstellen
- Es wird empfohlen, das Lösegeld nicht zu zahlen
- Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
- Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
- Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
- Erstellen Sie neue EC2-Instanzen aus einem vertrauenswürdigen AMI
- Verwenden Sie Cloudendure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
- Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, dass die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her
- Besuchen Sie < https://www.nomoreransom.org/ >, um festzustellen, ob eine Entschlüsselung für die Malware-Variante verfügbar ist, die Ihre Daten infiziert haben

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Unix/Linux

### Identifizieren
- Bewerten Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
- AWS hat ein neues Open Source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) Tool entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um schnell wertvolle Einblicke in die Sicherheitslage von ihr AWS-Konto.
- Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen einschließlich Server, Netzwerkgeräte, Netzwerk-/Dateifreigaben und Entwicklermaschinen
- Führen Sie wiederkehrende Schwachstellenanalysen Ihrer Hosts mithilfe von Dienstprogrammen wie [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/) durch
- Verwenden Sie [CloudWatch-Metriken] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), insbesondere NetWorkout, um nach „Spikes“ der Datenexfiltration zu suchen. Es ist möglich, dass ein Angreifer die Datenvernichtung durchführte und einen Lösegeldschein hinterlassen hat, und in diesen Fällen gibt es keine Möglichkeit zur Datenrettung durch die Zusammenarbeit mit dem böswilligen Akteur

### Schützen
- Wenden Sie die neuesten Updates auf Ihre Betriebssysteme und Apps an
- Sichern Sie wichtige Dateien regelmäßig. Verwenden Sie die 3-2-1-Regel. Behalten Sie drei Backups Ihrer Daten auf zwei verschiedenen Speichertypen und mindestens ein Backup außerhalb des Standorts auf
- Blockieren Sie bekannte Ransomware-Dateitypen
- Erwägen Sie, AWS Config zu verwenden, um Regeln zur Überwachung von Änderungen an AWS-Services zu erstellen AWS Config verfügt über mehrere verwaltete Regeln zur Überwachung von EBS und EC2, darunter:
- [ebs-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-in-backup-plan.html)
- [ebs-optimized-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-optimized-instance.html)
- [ebs-snapshot-public-restorable-check] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-snapshot-public-restorable-check.html)
- [ec2-ebs-encryption-by-default] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-ebs-encryption-by-default.html)
- [ec2-imdsv2-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-imdsv2-check.html)
- [ec2-instance-detailed-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-detailed-monitoring-enabled.html)
- [ec2-instance-managed-by-systems-manager] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-systems-manager.html)
- [ec2-instance-multiple-eni-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-multiple-eni-check.html)
- [ec2-instance-no-public-ip] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-no-public-ip.html)
- [ec2-instance-profile-attached] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-profile-attached.html)
- [ec2-managedinstance-applications-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-blacklisted.html)
- [ec2-managedinstance-applications-required] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-required.html)
- [ec2-managedinstance-association-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-association-compliance-status-check.html)
- [ec2-managedinstance-inventory-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-inventory-blacklisted.html)
- [ec2-managedinstance-patch-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-patch-compliance-status-check.html)
- [ec2-managedinstance-platform-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-platform-check.html)
- [ec2-security-group-attached-to-eni] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html)
- [ec2-stopped-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-stopped-instance.html)
- [ec2-volume-inuse-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-volume-inuse-check.html)
- Informieren Sie Ihre Mitarbeiter, damit sie Social Engineering- und Spear-Phishing-Angriffe identifizieren können
- Lassen Sie Malware-Erkennung und Antivirenprogramme auf allen Systemen aktiviert - kommerzielle, kostenpflichtige Endpoint Detection and Response (EDR) -Lösungen (EDR) werden bevorzugt.
- Ein Beispielleitfaden finden Sie unter [So installieren und verwenden Sie Linux Malware Detect (LMD) mit ClamAV als Antivirus-Engine] (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
- Durchführen von Backups von EC2-Instanzen
- Erwägen Sie, [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) oder [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/) zu verwenden
- Das Microsoft 365 Defender Threat Intelligence Team hat einen [umfassenden Bericht] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) bereitgestellt, in dem Aktionen zum Schutz von Microsoft-Ressourcen vor einem Ransomware-Ereignis identifiziert werden. Viele dieser Empfehlungen können für Unix & Linux angepasst werden, darunter:
- Bestimmen Sie, wo hoch privilegierte Konten sich anmelden und Anmeldeinformationen freigeben.
- Verhärten Sie das internetorientierte Assets und stellen Sie sicher, dass sie über die neuesten Sicherheitsupdates verfügen. Verwenden Sie Bedrohungs- und Schwachstellenmanagement, um diese Assets regelmäßig auf Schwachstellen, Fehlkonfigurationen und verdächtige Aktivitäten zu überprüfen.
- Überwachen Sie auf Brute-Force-Versuche. Übermäßige fehlgeschlagene Authentifizierungsversuche prüfen (/var/log/auth.log oder /var/log/secure) /var/log/auth.log
- Überwachen zum Löschen von Ereignisprotokollen
- Üben Sie das Prinzip der geringsten Privilegien ein und behalten Sie die Hygiene von Anmeldeinformationen bei. Vermeiden Sie die Verwendung von domänenweiten Dienstkonten auf Administratorebene. Erzwingen Sie starke randomisierte Just-in-Time-Administrator-Passwörter für lokale Administratoren.
- Aktivieren Sie die Manipulationsschutzfunktionen, um zu verhindern, dass Angreifer Sicherheitsdienste stoppen
- Verwenden Sie einen Endpoint Security Agent wie [Wazuh] (https://documentation.wazuh.com/current/getting-started/components/index.html)
- Verwenden Sie einen Dateiintegritätsmonitor wie [Tripwire] (https://github.com/Tripwire/tripwire-open-source), um Änderungen an kritischen Dateien zu erkennen
- Verwenden Sie [Systems Manager und Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/), um zu überprüfen, ob EC2-Instanzen allgemeine Schwachstellen und Risikopositionen (CVes) enthalten
- Überprüfen Sie Ihre Backups und stellen Sie sicher, dass sich die Infektion nicht in sie ausgebreitet hat

### Erkennen
- Tabelle 1 von [No Strings on Me: Linux and Ransomware] (https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870) von Richard Horne identifiziert mehrere Indikatoren, die überwacht werden sollen, einschließlich:
- Ein neuer Prozess, der noch nie auf dem Endpunkt gesehen wurde und externe Netzwerkkonnektivitätsanfragen hat
- Versuchte Kommunikation mit DNS-Namen, bei denen Entropie gefunden wird, oder direkt mit bloßen IPs
- Änderung der Verteilung Yum- oder Apt Repositorys
- Erstellung von .sh-Dateien in nicht benutzerbasierten Home-Verzeichnissen
- Aufzählung von freigegebenen Web-, Datenbank- oder Dateispeicher-Verzeichnisbäumen
- Eskalation von Privilegien oder Versuche, Sudo-Zugang zu erhalten
- Externe sowie interne Netzwerkkommunikation
- Dateinamen, die mit Entropie generiert wurden oder die sich in schneller Folge befinden
- Dateien werden mehrfach im selben Verzeichnisbaum umbenannt
- Mit ungeraden Dateierweiterungen erstellte Dateien
- Fokus wird auf Ausreißer und Ereignisse gelegt, die außerhalb des typischen täglichen Betriebs liegen
- Hohe Speicherauslastung für kurze oder anhaltende Zeiträume
- Große Prozess-Tress, bei der der übergeordnete Prozess vor den untergeordneten Prozessen beendet wird
- Änderung der Bootoptionen
- Mehrere Kopien derselben Datei in mehreren Verzeichnissen
- Mehrere Änderungen in einem einzigen Verzeichnis
- Mögliche Aufrufe für Verschlüsselungsbibliotheken
- Mögliche Erstellung und Ausführung von Prozessen innerhalb des Verzeichniss/tmp
- Schnelle und aufeinanderfolgende Verzeichnisaufzählung
- Entfernen von Dateien aus Verzeichnissen mit Platzhaltern oder ohne Bestätigung
- Die Verwendung von chmod mit Wildcards (oder chmod 777)
- Die Verwendung von wget oder curl cmds
- Verwendung des chmod cmd, um ausführbare Dateien in übermäßig zulässige Rechte zu ändern
- Verwendung der Strings cmd, um zu versuchen, die Kommunikation vor oder während der Infektion zu codieren
- Verwenden Sie vpcFlowLogs, um unangemessenen Datenbankzugriff von externen IP-Adressen zu identifizieren
- Sie können [VPC Flow mit Amazon Detective untersuchen] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) oder [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Reagieren
- Untersuchen Sie Backups auf potenzielle Infektionen
- Isoliere die infizierten Systeme
- Entfernen Sie alle kompromittierten Systeme aus dem Netzwerk.
- Befolgen Sie regulatorische Anforderungen oder interne Unternehmensrichtlinien, um festzustellen, ob die Forensik der EC2-Instanz erforderlich ist
- Wenn Forensik der Instanzen erforderlich ist ODER Daten wiederhergestellt werden müssen, dann [Quarantäne die EC2-Instanz] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)

### Wiederherstellen
- Es wird empfohlen, das Lösegeld nicht zu zahlen
- Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
- Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
- Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
- Erstellen Sie neue EC2-Instanzen aus einem vertrauenswürdigen AMI
- Befolgen Sie regulatorische Anforderungen oder interne Unternehmensrichtlinien, um festzustellen, ob eine Forensik der EC2-Instanz erforderlich ist
- Verwenden Sie Cloudendure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
- Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, dass die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her
- Besuchen Sie < https://www.nomoreransom.org/ >, um festzustellen, ob eine Entschlüsselung für die Malware-Variante verfügbar ist, die Ihre Daten infiziert haben

## Amazon Simple Storage Service (S3)

### Identifizieren
- Bewerten Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
- AWS hat ein neues Open Source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) Tool entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten konto.
- Erwägen Sie, [AWS GuardDuty] (https://aws.amazon.com/guardduty/) zu implementieren, um kontinuierlich auf bösartige Aktivitäten und nicht autorisiertes Verhalten zu überwachen, um Ihre in Amazon S3 gespeicherten AWS-Konten, Workloads und Daten zu schützen
- Erwägen Sie die Verwendung von [AWS Config] (https://aws.amazon.com/config/), einem Service, mit dem Sie die Konfigurationen Ihrer AWS-Ressourcen bewerten, prüfen und bewerten können
- Darkbit bietet ein Beispiel für [AWS S3 Data Loss Prevention] (https://darkbit.io/blog/simple-dlp-for-amazon-s3), mit dem das unbefugte Kopieren von Objekten identifiziert werden kann. Andere spezifische Vorgänge können in das Muster auch basierend auf Ihren Geschäftsanwendungsfällen hinzugefügt werden
- Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen einschließlich Server, Netzwerkgeräte, Netzwerk-/Dateifreigaben und Entwicklermaschinen

### Schützen
- Erwägen Sie [Replikation aktivieren] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) - dieselbe Region oder regionsübergreifende Region. Regionsübergreifende Replikation schützt Daten vor Anwendungsfehlern und Bedienfehlern, indem eine zweite Kopie in einer anderen Region verwaltet wird
- Erwägen Sie, AWS Config zu verwenden, um Regeln zur Überwachung von Änderungen an AWS-Services zu erstellen AWS Config verfügt über mehrere verwaltete Regeln zur Überwachung von EBS und EC2, darunter:
- [s3-account-level-public-access-blocks] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks.html)
- [s3-account-level-public-access-blocks-periodic] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks-periodic.html)
- [s3-bucket-blacklisted-actions-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-blacklisted-actions-prohibited.html)
- [s3-bucket-default-lock-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-default-lock-enabled.html)
- [s3-bucket-level-public-access-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)
- [s3-bucket-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-logging-enabled.html)
- [s3-bucket-policy-grantee-check] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-grantee-check.html)
- [s3-bucket-policy-not-more-permissive] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-not-more-permissive.html)
- [s3-bucket-public-read-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-read-prohibited.html)
- [s3-bucket-public-write-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-write-prohibited.html)
- [s3-bucket-replication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-replication-enabled.html)
- [s3-bucket-server-side-encryption-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-server-side-encryption-enabled.html)
- [s3-bucket-ssl-requests-only] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-ssl-requests-only.html)
- [s3-bucket-versioning-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-versioning-enabled.html)
- [s3-default-encryption-kms] (https://docs.aws.amazon.com/config/latest/developerguide/s3-default-encryption-kms.html)
- Erwägen Sie, [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) Schlüssel zu verwenden, um alle Objekte zu verschlüsseln und zu verhindern, dass ein Angreifer seinen Verschlüsselungsschlüssel anwendet
- Erwägen Sie, die [AWS S3 Funktion für öffentlichen Zugriff blockieren] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) zu verwenden, um die unbeabsichtigte Belichtung von Objekten zu mildern
- Erwägen Sie, [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) für Objektsicherungen und Kostenoptimierung zu verwenden
- Erwägen Sie, [S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) zu verwenden, damit Sie Objekte mit einem *write-once-read-viele* (WORM) -Modell speichern können. Object Lock kann dazu beitragen, zu verhindern, dass Objekte für eine bestimmte Zeit oder unbestimmte Zeit gelöscht oder überschrieben werden.
- Im Object Lock-Konformitätsmodus** kann eine Version des geschützten Objekts von keinem Benutzer überschrieben oder gelöscht werden, einschließlich des Root-Benutzers in Ihrem AWS-Konto. Wenn ein Objekt im Compliance-Modus gesperrt ist, kann sein Aufbewahrungsmodus nicht geändert werden, und seine Aufbewahrungsfrist kann nicht verkürzt werden. Der Konformitätsmodus ***stellt sicher, dass eine Objektversion für die Dauer des Aufbewahrungszeitraums nicht überschrieben oder gelöscht werden kann.
- Aktivieren Sie die [CloudTrail-Ereignisprotokollierung für S3-Buckets und Objekte] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) mit sensiblen oder kritischen Informationen. Standardmäßig protokollieren CloudTrail-Trails keine Datenereignisse, Sie können jedoch Trails konfigurieren, um Datenereignisse für von Ihnen angegebene S3-Buckets zu protokollieren oder Datenereignisse für alle Amazon S3 S3-Buckets in Ihrem AWS-Konto zu protokollieren.
- Aktivieren Sie die [CloudTrail-Server-Logging für S3-Buckets] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) und Objekte, die sensible oder kritische Informationen enthalten. Die Serverzugriffsprotokollierung liefert detaillierte Datensätze für die Anfragen, die an einen Bucket gestellt werden
- Aktivieren Sie [S3 Object Versioning] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html), um die Wiederherstellung geänderter Objekte zu ermöglichen
- Um die Anzahl der beibehaltenen Versionen festzulegen, [legen Sie eine Lebenszyklusrichtlinie fest] (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html), um auf nicht aktuelle Versionen anzuwenden
- Aktivieren Sie [S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html), um zu verhindern, dass ein Angreifer die Versionierung und das Überschreiben aller Objekte in einem Bucket deaktiviert
- Durchsetzung der Multi-Faktor-Authentifizierung (MFA)
- Erzwingen Sie die Anforderungen an die Passwortkomplexität und legen
- Implementieren Sie [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) einschließlich Ablauf von Konten und obligatorischen Rotationen von Anmeldeinformationen
- Führen Sie einen [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) aus, um alle Benutzer in Ihrem Konto und den Status ihrer verschiedenen Anmeldeinformationen aufzulisten, einschließlich Passwörter, Zugriffsschlüssel und MFA-Geräte
- Ergreifen Sie Schritte, um [zu verhindern, dass neue Anmeldeinformationen öffentlich veröffentlicht werden] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
- Verwenden Sie [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html), um die Ressourcen in Ihrer Organisation und Ihren Konten zu identifizieren, wie Amazon S3 S3-Buckets oder IAM-Rollen, die mit einer externen Entität geteilt werden. Auf diese Weise können Sie unbeabsichtigten Zugriff auf Ihre Ressourcen und Daten identifizieren, was ein Sicherheitsrisiko darstellt
- Verwenden Sie IAM-Rollen, um Berechtigungen zu verwalten
- Implementieren Sie die geringsten Berechtigungen und lassen Sie keine s3:\ * Berechtigungen zu
- Bucket-Richtlinien verwenden und routinemäßig prüfen
- Machen Sie nur öffentlich, was sein muss, und stellen Sie sicher, dass alle Objekte geschützt sind, indem Sie privat sind
- Obwohl wir bestimmte Lösungen von Drittanbietern nicht empfehlen können, gibt es auch Produkte von unseren Partnern, einschließlich Veritas und CommVault, und anderen Backup-Softwareanbietern, die die native Fähigkeit haben, Objekte in S3 an ihren verwalteten Backup-Speicherorten innerhalb oder außerhalb von S3 zu sichern

### Erkennen

- Überprüfen Sie Ihr CloudTrail-Protokoll auf nicht genehmigte Aktivitäten wie das Erstellen nicht autorisierter IAM-Benutzer, Richtlinien, Rollen oder temporäre Sicherheitsanmeldeinformationen
- Überprüfen Sie Ihr CloudTrail-Protokoll, um Ihr AWS-Konto auf unbefugte AWS-Nutzung wie nicht autorisierte EC2-Instanzen, Lambda-Funktionen oder EC2-Spot-Gebote zu überprüfen. Sie können die Nutzung auch überprüfen, indem Sie sich bei Ihrer AWS Management Console anmelden und jede Serviceseite überprüfen. Die Seite „Rechnungen\“ in der Rechnungskonsole kann auch auf unerwartete Verwendung überprüft werden
- Bitte beachten Sie, dass eine unbefugte Nutzung in jeder Region auftreten kann und dass Ihre Konsole Ihnen möglicherweise nur eine Region gleichzeitig anzeigt. Um zwischen Regionen zu wechseln, können Sie das Dropdown-Menü in der oberen rechten Ecke des Konsolenbildschirms verwenden
- Erpresserbrief, der entweder als Objekt im Bucket oder per E-Mail an den Kunden bereitgestellt wird
- S3-Objekte werden gelöscht oder ganze S3-Buckets werden gelöscht
- Hinweis: Bei Ereignissen zur Datenvernichtung kann ein Lösegeldschein bereitgestellt werden oder nicht. Stellen Sie außerdem sicher, dass Sie CloudWatch-Metriken und CloudTrail S3-Ereignisse überprüfen, um zu überprüfen, ob die Datenexfiltration stattgefunden hat oder nicht, um zwischen einem Lösegeld- oder Datenvernichtungsangriff abzugrenzen
- S3-Objekte werden mit einem Schlüssel von einem Konto verschlüsselt, das nicht dem Kunden gehört

### Reagieren
- Löschen oder drehen Sie [IAM-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); Sie können alle Schlüssel in Ihrem Konto drehen, wenn Sie einen bestimmten Schlüssel oder Schlüssel nicht identifizieren können, die freigegeben wurden
- Löschen Sie nicht autorisierte [IAM-Benutzer] (https://console.aws.amazon.com/iam/home#users.)
- Unautorisierte [Richtlinien] löschen (https://console.aws.amazon.com/iam/home#/policies)
- Löschen Sie nicht autorisierte [Rollen] (https://console.aws.amazon.com/iam/home#/roles)
- Wenn Ihre Anwendung freiliegende Zugriffsschlüssel verwendet, müssen Sie den Schlüssel ersetzen. Um den Schlüssel zu ersetzen, erstellen Sie zuerst einen zweiten Schlüssel (zu diesem Zeitpunkt sind beide Schlüssel aktiv) und ändern Sie dann Ihre Anwendung so, dass sie den neuen Schlüssel verwendet. Deaktivieren (aber nicht löschen) dann freiliegende Keys, indem Sie in der Konsole auf die Option „Inaktiv machen“ klicken. Wenn es Probleme mit Ihrer Anwendung gibt, können Sie exponierte Keys reaktivieren. Wenn Ihre Anwendung mit dem neuen Schlüssel voll funktionsfähig ist, löschen Sie bitte exponierte Schlüssel
- [temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden. *HINWEIS: * Das Löschen von IAM-Benutzern kann sich auf Produktions-Workloads auswirken und sollte mit Vorsicht durchgeführt werden

### Wiederherstellen
- Es wird empfohlen, das Lösegeld nicht zu zahlen
- Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
- Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
- Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
- Implementieren Sie Prozeduren unter Schützen, bevor Sie versuchen, Daten oder Objekte wiederherzustellen
- Entferne [Markierungen löschen] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) für versionierte Objekte
- Gelöschte Buckets neu erstellen
- Wiederherstellen von Objekten aus der Verwendung von [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) Objektsicherungen oder replizierten Regions-Bucket
- *Hinweis: * Es gibt derzeit keine\ "undelete\“ -Fähigkeit für S3, und AWS kann gelöschte Daten nicht wiederherstellen. Im gegenwärtigen Zeitalter der Einhaltung der Datenspeicherung und Vorschriften wie [DSGVO] (https://gdpr-info.eu/) und [CCPA] (https://oag.ca.gov/privacy/ccpa) kann Amazon S3 Kundendaten nicht weiterhin speichern, die explizit aus dem Konto des Kunden gelöscht wurden. Sobald ein Objekt gelöscht wurde, kann es von AWS nicht mehr wiederhergestellt werden, unabhängig davon, wie schnell die unbeabsichtigte Löschung an AWS gemeldet wird

## Relationaler Datenbankdienst (RDS)

### Identifizieren
- Bewerten Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
- AWS hat ein neues Open Source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) Tool entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten konto.
- Erwägen Sie die Verwendung von [AWS Config] (https://aws.amazon.com/config/), einem Service, mit dem Sie die Konfigurationen Ihrer AWS-Ressourcen bewerten, prüfen und bewerten können
- Erwägen Sie, [AWS GuardDuty] (https://aws.amazon.com/guardduty/) zu implementieren, um kontinuierlich auf bösartige Aktivitäten und nicht autorisiertes Verhalten zu überwachen, um Ihre AWS-Konten, Workloads und Daten zu schützen, die in Amazon RDS gespeichert sind
- Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen einschließlich Server, Netzwerkgeräte, Netzwerk-/Dateifreigaben und Entwicklermaschinen

### Schützen
- Weitere Referenzen und Schritte sind unter [Sicherheit in Amazon RDS] verfügbar (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)
- Erwägen Sie, AWS Config zu verwenden, um Regeln zur Überwachung von Änderungen an AWS-Services zu erstellen AWS Config verfügt über mehrere verwaltete Regeln zur Überwachung von RDS, darunter:
- [rds-automatic-minor-version-upgrade-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-automatic-minor-version-upgrade-enabled.html)
- [rds-cluster-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-deletion-protection-enabled.html)
- [rds-cluster-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-iam-authentication-enabled.html)
- [rds-cluster-multi-az-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-multi-az-enabled.html)
- [rds-enhanced-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-enhanced-monitoring-enabled.html)
- [rds-instance-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-deletion-protection-enabled.html)
- [rds-instance-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-iam-authentication-enabled.html)
- [rds-instance-public-access-check] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html)
- [rds-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/rds-in-backup-plan.html)
- [rds-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html)
- [rds-multi-az-support] (https://docs.aws.amazon.com/config/latest/developerguide/rds-multi-az-support.html)
- [rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)
- [rds-snapshot-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshot-encrypted.html)
- [rds-storage-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-storage-encrypted.html)
- Verfügen Sie über eine Host-basierte Intrusion Detection System (HIDS) -Lösung eines Drittanbieters (wie OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/))
- Führen Sie Ihre DB-Instance in einer Virtual Private Cloud (VPC) aus, die auf dem Amazon VPC-Dienst basiert, um die größtmögliche Netzwerkzugriffskontrolle zu gewährleisten. Weitere Informationen zum Erstellen einer DB-Instance in einer VPC finden Sie unter [Amazon Virtual Private Cloud VPCs und Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html).
- Verwenden Sie AWS Identity and Access Management (IAM) -Richtlinien, um Berechtigungen zuzuweisen, die bestimmen, wer Amazon RDS RDS-Ressourcen verwalten kann. Sie können beispielsweise IAM verwenden, um festzustellen, wer DB-Instanzen erstellen, beschreiben, ändern und löschen, Ressourcen kennzeichnen oder Sicherheitsgruppen ändern kann.
- Verwenden Sie die Amazon RDS RDS-Verschlüsselung, um Ihre DB-Instances und Snapshots im Ruhezustand zu sichern. Die Amazon RDS RDS-Verschlüsselung verwendet den Industriestandard AES-256-Verschlüsselungsalgorithmus, um Ihre Daten auf dem Server zu verschlüsseln, der Ihre DB-Instance hostet. Weitere Informationen finden Sie unter [Amazon RDS RDS-Ressourcen verschlüsseln] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html)
- Verwenden Sie Secure Socket Layer (SSL) oder Transport Layer Security (TLS) -Verbindungen mit DB-Instanzen, auf denen die Datenbank-Engines MySQL, MariaDB, PostgreSQL, Oracle oder Microsoft SQL Server ausgeführt werden. Weitere Informationen zur Verwendung von SSL/TLS mit einer DB-Instance finden Sie unter [Verwenden von SSL/TLS zum Verschlüsseln einer Verbindung zu einer DB-Instance] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
- Verwenden Sie Sicherheitsgruppen, um zu steuern, welche IP-Adressen oder Amazon EC2 EC2-Instances eine Verbindung zu Ihren Datenbanken auf einer DB-Instance herstellen können. Wenn Sie zum ersten Mal eine DB-Instance erstellen, verhindert das Sicherheitssystem jeglichen Datenbankzugriff, außer durch Regeln, die von einer zugehörigen Sicherheitsgruppe festgelegt wurden.
- Verwenden Sie Netzwerkverschlüsselung und transparente Datenverschlüsselung mit Oracle DB-Instanzen; weitere Informationen finden Sie unter [Oracle native Netzwerkverschlüsselung] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) und [Oracle Transparent Data Encryption] ( https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html)
- Verwenden Sie Sicherheitsfunktionen Ihrer DB-Engine, um zu steuern, wer sich in einer DB-Instance bei den Datenbanken anmelden kann. Diese Funktionen funktionieren genauso, als ob sich die Datenbank in Ihrem lokalen Netzwerk befand.

### Erkennen
- AWS GuardDuty Machine Learning kann verdächtiges Verhalten erkennen, einschließlich Snapshots des Amazon Relational Database Service (Amazon RDS) im Rahmen von Datenexfiltrationsversuchen. Ein Beispiel finden Sie im AWS Security Blog [Wie Sie Amazon GuardDuty verwenden können, um verdächtige Aktivitäten in Ihrem AWS-Konto zu erkennen] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/)
- Überprüfen Sie das EC2-Betriebssystem- und Anwendungsprotokolle auf unangemessene Anmeldungen, Installation unbekannter Software oder das Vorhandensein nicht erkannter Dateien
- Verwenden Sie vpcFlowLogs, um unangemessenen Datenbankzugriff von externen IP-Adressen zu identifizieren
- Sie können [VPC Flow mit Amazon Detective untersuchen] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) oder [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
- Der [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) ermöglicht es Kunden, Sicherheitsuntersuchungen an AWS-Service-Protokollen durchzuführen, indem sie eine Amazon Athena-Analyseumgebung bereitstellen, die schnell bereitzustellen, einsatzbereit und einfach zu warten ist

### Reagieren
- Löschen oder drehen Sie [IAM-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); Sie können alle Schlüssel in Ihrem Konto drehen, wenn Sie einen bestimmten Schlüssel oder Schlüssel nicht identifizieren können, die freigegeben wurden
- Löschen Sie nicht autorisierte [IAM-Benutzer] (https://console.aws.amazon.com/iam/home#users.)
- Unautorisierte [Richtlinien] löschen (https://console.aws.amazon.com/iam/home#/policies)
- Löschen Sie nicht autorisierte [Rollen] (https://console.aws.amazon.com/iam/home#/roles)
- Löschen Sie nicht erkannte oder nicht autorisierte öffentliche Snapshots oder Datenbanken
- Identifizieren Sie EC2-Instanzen, die zulässigen Zugriff auf die Datenbank (en) hatten, und untersuchen Sie diese ebenfalls
- Wenn Ihre Anwendung freiliegende Zugriffsschlüssel verwendet, müssen Sie den Schlüssel ersetzen. Um den Schlüssel zu ersetzen, erstellen Sie zuerst einen zweiten Schlüssel (zu diesem Zeitpunkt sind beide Schlüssel aktiv) und ändern Sie dann Ihre Anwendung so, dass sie den neuen Schlüssel verwendet. Deaktivieren (aber nicht löschen) dann freiliegende Keys, indem Sie in der Konsole auf die Option „Inaktiv machen“ klicken. Wenn es Probleme mit Ihrer Anwendung gibt, können Sie exponierte Keys reaktivieren. Wenn Ihre Anwendung mit dem neuen Schlüssel voll funktionsfähig ist, löschen Sie bitte exponierte Schlüssel
- [temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden. *HINWEIS: * Das Löschen von IAM-Benutzern kann sich auf Produktions-Workloads auswirken und sollte mit Vorsicht durchgeführt werden

### Wiederherstellen
- Es wird empfohlen, das Lösegeld nicht zu zahlen
- Die Zahlung des Lösegeldes ist ein Glücksspiel, ob der Kriminelle die Transaktion nach Zahlungseingang einhalten wird
- Wenn keine Datensicherungen vorhanden sind, sollten Sie eine Kosten-Nutzen-Analyse durchführen und den Wert der Daten/des Reputationskompromisses gegen die Zahlung an den Angreifer abwägen
- Sie ermöglichen dem Angreifer direkt, seinen Betrieb gegen Ihr Unternehmen oder andere fortzusetzen, wenn Sie das Lösegeld zahlen
- Löschen Sie die kompromittierte Datenbank und erstellen Sie neue RDS-Datenbanken
- Befolgen Sie regulatorische Anforderungen oder interne Unternehmensrichtlinien, um festzustellen, ob eine Forensik der RDS-Datenbank erforderlich ist
- Besuchen Sie < https://www.nomoreransom.org/ >, um festzustellen, ob eine Entschlüsselung für die Malware-Variante verfügbar ist, die Ihre Daten infiziert haben
- Verwenden Sie Cloudendure Disaster Recovery, um den neuesten Wiederherstellungspunkt vor dem Ransomware-Angriff oder Datenbeschädigung auszuwählen, um Ihre Workloads auf AWS wiederherzustellen
- Wenn Sie eine alternative Datensicherungsstrategie verwenden, überprüfen Sie, dass die Backups nicht infiziert wurden, und stellen Sie sie vom letzten geplanten Ereignis vor dem Ransomware-Ereignis wieder her

### Nach einem Vorfall

### Lösegeldangriffe an Behörden melden
Das FBI ermutigt Organisationen, Lösegeldvorfälle an die Strafverfolgungsbehörden zu melden. Das Internet Crime Complaint Center (IC3) akzeptiert Beschwerden im Internet entweder vom tatsächlichen Opfer oder von einem Dritten an den Beschwerdeführer und wird mit ihnen zusammenarbeiten, um die beste Vorgehensweise in Zukunft zu ermitteln. Seien Sie bereit zu teilen:
- Alle relevanten Informationen, die Ihrer Meinung nach notwendig sind, um Ihre Beschwerde zu unterstützen
- E-Mail-Kopfzeile (n)
- Finanztransaktionsinformationen (Kontoinformationen, Transaktionsdatum und -betrag, Empfängerdetails)
- Name, Adresse, Telefon, E-Mail, Website und IP-Adresse des Betreffes
- Spezifische Details darüber, wie Sie schikaniert wurden
- Name, Adresse, Telefon und E-Mail des Opfers

### Durchführen einer Ursachenanalyse
In den meisten Fällen funktionieren Ihre bestehenden Forensik-Tools in der AWS-Umgebung. Forensische Teams profitieren von der automatisierten Bereitstellung von Tools in allen AWS-Regionen und der Möglichkeit, große Datenmengen mit geringer Reibung mit den gleichen robusten, skalierbaren Diensten, auf denen ihre geschäftskritischen Anwendungen aufbauen, schnell zu sammeln.

[Amazon Detective] (https://aws.amazon.com/detective/%20) macht es einfach, die Ursache potenzieller Sicherheitsprobleme oder verdächtiger Aktivitäten zu analysieren, zu untersuchen und schnell zu identifizieren. Amazon Detective sammelt automatisch AWS-Service-Protokolldaten aus Ihren AWS-Ressourcen und erstellt mithilfe von maschinellem Lernen, statistischer Analysen und Graphentheorie einen verknüpften Datensatz, der es Ihnen ermöglicht, schnellere und effizientere Sicherheitsuntersuchungen durchzuführen.

### Sicherheitsprogramm mit gewonnenen Lektionen aktualisieren
Das Dokumentieren und Radfahren von Lektionen, die während Simulationen und Live-Vorfällen in „neue normale“ Prozesse und Verfahren zurückgeführt wurden, ermöglicht es Unternehmen, besser zu verstehen, wie sie verletzt wurden - z. B. wo sie anfällig waren, wo die Automatisierung versagt hat oder wo die Sichtbarkeit fehlte - und die Gelegenheit, ihre allgemeine Sicherheitslage zu stärken.

## Trainiere deine Mitarbeiter
Trainieren Sie Ihre Mitarbeiter durch ein effektives Sicherheitsbewusstseinsprogramm. Dies muss unterhaltsam und einnehmend gemacht werden. Die Konzentration auf die Berichterstattung von Phishing, Benachrichtigungsverfahren und wie man „schlechte“ Dinge meldet und dann dieses Verhalten vergibt, kann sehr effektiv sein.

## Fazit
Lösegeldangriffe entwickeln sich weiter, aber auch Ihr Sicherheitsbewusstsein und Ihre Bereitschaft. Regierungsbehörden, gemeinnützige Organisationen und Unternehmen auf der ganzen Welt vertrauen darauf, dass AWS ihre Infrastruktur mit Strom versorgt und ihre Systeme und Daten sicher hält. Mithilfe der in diesem Playbook geteilten AWS-Services und Best Practices können Sie proaktive Maßnahmen ergreifen, um die Wahrscheinlichkeit und die Auswirkungen von Lösegeld in Ihren AWS-Umgebungen zu verringern.

## Anhang A: Dienste und Tools zur Verhinderung oder Erkennung von Lösegeldangriffen
[Amazon CloudWatch Synthetics] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)
[Amazon CloudWatch] (https://aws.amazon.com/cloudwatch)
[Amazon CodeGuru] (https://aws.amazon.com/codeguru/)
[Amazon EventBridge] (https://aws.amazon.com/eventbridge/)
[Amazon GuardDuty] (https://aws.amazon.com/guardduty/)
[Erreichbarkeit des Amazon Inspector-Netzwerks] (https://docs.aws.amazon.com/inspector/latest/userguide/inspector_network-reachability.html)
[Amazon-Inspektor] (https://aws.amazon.com/inspector/)
[Amazon Macie] (https://aws.amazon.com/macie/)
[Amazon VPC Reachability Analyzer] (https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)
[AWS CDK] (https://aws.amazon.com/cdk/)
[AWS Cloud9] (https://aws.amazon.com/cloud9/)
[AWS CloudFormation Guard] (https://github.com/aws-cloudformation/cloudformation-guard)
[AWS CloudFormation] (https://aws.amazon.com/cloudformation/)
[AWS CodeArtiFact] (https://aws.amazon.com/codeartifact/)
[AWS CodeBuild] (https://aws.amazon.com/codebuild/)
[AWS CodeCommit] (https://aws.amazon.com/codecommit/)
[AWS CodeDeploy] (https://aws.amazon.com/codedeploy/)
[AWS CodePipeline] (https://aws.amazon.com/codepipeline/)
[AWS-Konfigurationsregeln] (https://aws.amazon.com/blogs/aws/aws-config-rules-dynamic-compliance-checking-for-cloud-resources/)
[AWS Control Tower] (https://aws.amazon.com/controltower/)
[AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)
[AWS IAM] (https://aws.amazon.com/iam/)
[AWS KMS] (https://aws.amazon.com/kms/)
[AWS Lambda] (https://aws.amazon.com/lambda/)
[AWS-Organisationen] (https://aws.amazon.com/organizations/)
[AWS Secrets Manager] (https://aws.amazon.com/secrets-manager/)
[AWS-Sicherheits-Hub] (https://aws.amazon.com/security-hub/)
[AWS Step-Funktionen] (https://aws.amazon.com/step-functions/)
[AWS Systems Manager] (https://aws.amazon.com/systems-manager/)
[AWS WAF] (https://aws.amazon.com/waf/)
[AWSPX] (https://github.com/FSecureLABS/awspx)
[CheckMarx] (https://checkmarx.com/)
[EC2 Image Builder] (https://aws.amazon.com/image-builder/)
[ECR-Native Container-Image-Scannen] (https://aws.amazon.com/blogs/containers/amazon-ecr-native-container-image-scanning/)
[git-secrets] (https://github.com/awslabs/git-secrets)
[pMapper] (https://github.com/nccgroup/PMapper)
[Prowler] (https://github.com/toniblyx/prowler)
[SonarQube] (https://www.sonarqube.org/)