# Sicherheitsleitfaden für kompromittierte AWS-Kontoanmeldedaten

## Einführung

Als Teil unseres kontinuierlichen Engagements für Kunden bietet AWS das
Playbook zur Reaktion auf Sicherheitsvorfälle, das die Schritte beschreibt, die erforderlich sind, um
kompromittierte Anmeldedaten in Ihrem AWS erkennen und darauf reagieren
Konto (e). Der Zweck dieses Dokuments besteht darin, präskriptive
Hinweise zu den Maßnahmen, die zu ergreifen sind, wenn Sie vermuten, dass ein Sicherheitsereignis
hat stattgefunden.

![Image](/images/nist_life_cycle.png)

*Aspekte der AWS-Incident-Response*

## Vorbereitung

Um schnell und effektiv auf Vorfälle reagieren zu können
Aktivitäten, es ist wichtig, die Leute, Prozesse und
Technologie in Ihrem Unternehmen. Überprüfen Sie den [*AWS-Sicherheitsvorfall]
Antwort
Reiseführer*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html),
und die notwendigen Schritte umsetzen, um sicherzustellen, dass alle vorbereitet sind
drei Domains.

## So nehmen Sie den AWS-Support für Unterstützung in Anspruch

Es ist wichtig, dass Sie AWS benachrichtigen, sobald Sie vermuten, dass ein kompromittiertes
Anmeldeinformationen innerhalb Ihres AWS-Kontos oder Ihrer Organisation. Hier sind die
Schritte zur Inanspruchnahme des AWS-Supports:

### Öffnen Sie einen AWS-Supportfall

- Loggen Sie sich in Ihr AWS-Konto ein:

- Dies ist das erste AWS-Konto, das von der Sicherheit betroffen war
Veranstaltung, um die Inhaberschaft eines AWS-Kontos zu bestätigen.

- Hinweis: Wenn Sie nicht auf das Konto zugreifen können, verwenden Sie [
Formular] (https://support.aws.amazon.com/#/contacts/aws-account-support/)
um eine Support-Anfrage einzureichen.

- Wählen Sie „*Services*“, „*Support Center*“, „*Kundenfall erstellen*“.

- Wählen Sie den Problemtyp „*Konto und Abrechnung“ *.

- Wählen Sie den betroffenen Dienst und die Kategorie aus:

- Beispiel:

- Service: Konto

- Kategorie: Sicherheit

- Wählen Sie einen Schweregrad:

- Enterprise Support- oder On-Ramp-Kunden: *Kritisches Geschäftsrisiko
Frage*.

- Business-Support-Kunden: *Dringende Frage zum Geschäftsrisiko*.

- Beschreiben Sie Ihre Frage oder Ihr Problem:

- Geben Sie eine detaillierte Beschreibung des Sicherheitsproblems, das Sie haben
Erleben, die betroffenen Ressourcen und die Auswirkungen auf das Geschäft.

- Zeitpunkt, an dem das Sicherheitsereignis zum ersten Mal erkannt wurde.

- Zusammenfassung des Veranstaltungszeitplans bis zu diesem Zeitpunkt.

- Artefakte, die Sie gesammelt haben (Screenshots, Logdateien).

- ARN von IAM-Benutzern oder -Rollen, von denen Sie vermuten, dass sie gefährdet sind.

- Beschreibung der betroffenen Ressourcen und der Auswirkungen auf das Geschäft.

- Grad des Engagements in Ihrer Organisation (z. B.: „Diese Sicherheit
Die Veranstaltung hat die Sichtbarkeit des CEO und des Vorstands von
Direktoren“).

- Optional: Fügen Sie weitere Ansprechpartner für den Fall hinzu.

- Wählen Sie „*Kontaktieren Sie uns*“.

- Bevorzugte Kontaktsprache (möglicherweise je nach Verfügbarkeit)

- Bevorzugte Kontaktmethode: Internet, Telefon oder Chat (empfohlen)

- Optional: Zusätzliche Ansprechpartner für den Fall

<!-- -->

- *Klicken Sie auf „Abschicken“ *

- **Eskalationen**: Bitte benachrichtigen Sie Ihr AWS-Kundenbetreuungsteam, sobald
möglich, also können sie die notwendigen Ressourcen einsetzen und eskalieren wie
benötigt.

**Hinweis: ** Es ist sehr wichtig, Ihre „alternative Sicherheit“ zu überprüfen
„Kontakt“ ist für jedes AWS-Konto definiert. Für weitere Informationen wenden Sie sich bitte an
zu [dem
Artikel] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).

## Erkennung

Es gibt mehrere Möglichkeiten, kompromittierte Zugangsdaten in Ihrem
AWS-Umgebung. Aufgeführt sind einige Methoden, um mögliche Indikatoren für zu erkennen
Kompromiss (IoC). Eine detaillierte Liste der IoCs finden Sie unter [Anhang]
A.] (#appendix -a-reviewing-logs) **Hinweis**: Dokumentieren Sie die Details von jedem
vermutete IoC zur weiteren Analyse.

1. Bewerten Sie Amazon [GuardDuty]
Ergebnisse] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html).
Die folgenden Ergebnisse beziehen sich auf verdächtige oder anomale Aktivitäten
von IAM-Entitäten. Überprüfen Sie den [Befund]
Einzelheiten] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html),
Und wenn die Aktivität unerwartet ist, könnte das auf eine kompromittierte
Ausweis.

1. CredentialAccess:IAMUser/AnomalousBehavior

2. DefenseEvasion:IAMUser/AnomalousBehavior

3. Discovery:IAMUser/AnomalousBehavior

4. Exfiltration:IAMUser/AnomalousBehavior

5. Impact:IAMUser/AnomalousBehavior

6. InitialAccess:IAMUser/AnomalousBehavior

7. PenTest:IAMUser/KaliLinux

8. PenTest:IAMUser/ParrotLinux

9. PenTest:IAMUser/PentooLinux

10. Persistence:IAMUser/AnomalousBehavior

11. Policy:IAMUser/RootCredentialUsage

12. PrivilegeEscalation:IAMUser/AnomalousBehavior

13. Recon:IAMUser/MaliciousIPCaller

14. Recon:IAMUser/MaliciousIPCaller.Custom

15. Recon:IAMUser/TorIPCaller

16. Stealth:IAMUser/CloudTrailLoggingDisabled

17. Stealth:IAMUser/PasswordPolicyChange

18. UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B

19. UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration

20. UnauthorizedAccess:IAMUser/MaliciousIPCaller

21. UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom

22. UnauthorizedAccess:IAMUser/TorIPCaller

2. Überprüfen Sie die AWS-Abrechnung auf ungewöhnliche Spitzen, was ein Anzeichen dafür sein kann.
und Zugangsdaten kompromittieren, indem Sie die folgenden Schritte befolgen:

1. Melden Sie sich bei der AWS-Managementkonsole an und öffnen Sie die [Abrechnung].
Konsole] (https://console.aws.amazon.com/billing/).

2. Wählen Sie „*Rechnungen“ *, um Details zu Ihren aktuellen Gebühren zu sehen.

3. Wählen Sie „*Zahlungen“ *, um Ihre historischen Zahlungstransaktionen zu sehen.

4. Wählen Sie „*AWS-Kosten- und Nutzungsberichte“ *, um fehlerhafte Berichte zu sehen
senken Sie Ihre Kosten.

3. Laden Sie den Bericht mit den IAM-Anmeldeinformationen herunter, indem Sie zur IAM-Konsole gehen und
Wählen Sie links unter „*Access“ den „*Credential Report“ *
Berichte“ * dann überprüfen Sie Folgendes:

1. Identifizieren Sie ungewöhnliche IAM-Benutzererstellungen anhand des Erstellungsdatums
und die Spalten mit dem zuletzt benutzten/geänderten Passwort.

2. Prüfen Sie, ob irgendwelche IAM-Benutzer zwei oder mehr Zugangsschlüssel haben.

3. Prüfen Sie, ob irgendwelche IAM-Benutzer
*AWS Exposed CredentialPolicy\ _DO\ _NOT\ _REMOVE* angehängt. Falls ja,
seine Zugriffstasten rotieren.

4. Überprüfen Sie die IAM-Rollen innerhalb des AWS-Kontos, um unbekannte Rollen zu identifizieren
Rollen, die erstellt wurden oder auf die zugegriffen wurde

1. Wählen Sie in der AWS-Konsole „*Service*s“, „*IAM*“ und
„*Rollen*“

2. Überprüfen Sie alle unbekannten „*Rollennamen*“.

3. Klicken Sie auf den Rollennamen und überprüfen Sie die aufgelisteten Details:

1. Datum der Erstellung der Rolle

2. ARN

3. Letzte Aktivität

4. Klicken Sie auf „*Permissions*“, um die beigefügten IAM-Richtlinien zu überprüfen.
zu der Rolle.

5. Klicken Sie auf „*Trust Relationships*“, um Entitäten zu sehen, die annehmen können
Die Rolle.

6. Klicken Sie auf „*Access Advisor*“, um zu sehen, welche Dienste es gab
Zugriff über die Rolle und das „*Datum des letzten Zugriffs*“.

5. Entdecken Sie alle unerkannten oder nicht autorisierten Ressourcen in Ihrem AWS
Konto wie:

1. EC2-Instances, indem Sie den folgenden Befehl in der AWS-CLI ausführen
Terminal „*aws aws ec2 Describe-Instanzen describe-instances*“ und validieren Sie die
Ergebnisse. Suchen Sie nach der Erstellungszeit für jede Instanz, die
*--query 'Reservierungen\ [\] .Instanzen\ [\]. {ip: Öffentliche IP-Adresse,
tm: LaunchTime} '--filters 'name=tag:Name, Values=
MyInstanceName' | jq 'sort\ _by (.tm) | reverse |.\ [0\] '* Filter.

2. Lambda funktioniert, indem es den folgenden Befehl in der AWS-CLI ausführt
Terminal „*aws AWS-Lambda-Listenfunktionen list-functions*“ und validieren Sie die letzte
hat die Zeit mit dem Filter „| *grep „LastModified*““ geändert.

6. Überprüfen Sie alle Sicherheitsbenachrichtigungen von AWS über Ihr Konto von
Anmeldung bei [AWS Support]
Center] (https://support.console.aws.amazon.com/support/home#/) dann
die Nachricht (en) lesen und beantworten.

7. Überprüfen Sie die Ergebnisse, indem Sie diesen Schritten folgen:

1. Öffnen Sie die [IAM-Konsole] (https://console.aws.amazon.com/iam/).

2. Wählen Sie „*Access Analyzer“ * aus der linken Spalte unter Access
berichtet.

3. Unter „*Aktive Ergebnisse“ * Ergebnisse überprüfen, um Ressourcen zu identifizieren
in Ihrer Organisation, wie S3-Buckets, IAM-Rollen oder Lambda
Funktionen, die mit externen Entitäten geteilt werden.

## Analyse

Sobald Sie verdächtige Ressourcen oder Aktivitäten identifiziert haben, die
einen Kompromiss angeben, weitere Analysen in Ihrem SIEM durchführen oder protokollieren
Analysetools. AWS bietet verschiedene Tools für Sicherheitsdienste, um zu helfen
analysiert Sicherheitsereignisse. Einige dieser Tools beinhalten:

1. **Amazon GuardDuty** — Amazon GuardDuty ist eine Bedrohungserkennung.
Service, der kontinuierlich nach böswilligen Aktivitäten sucht und
Unautorisiertes Verhalten zum Schutz Ihrer AWS-Konten, Amazon Elastic
Compute Cloud (EC 2) -Workloads, Container-Anwendungen, Amazon Aurora
Datenbanken und Daten, die im Amazon Simple Storage Service (S3) gespeichert sind.

2. **AWS Security Hub** — AWS-Sicherheitszentrum ist eine Cloud-Sicherheitslösung
Management-Service (CSPM), der automatisierte, kontinuierliche
Best-Practice-Sicherheitsverfahren werden mit Ihren AWS-Ressourcen verglichen, um Ihnen zu helfen
identifiziert Fehlkonfigurationen und fasst Ihre Sicherheitswarnungen zusammen
(d. h. Ergebnisse) in einem standardisierten Format, so dass Sie es einfacher haben
bereichern, untersuchen und korrigieren.

3. **Amazon Detective** — Amazon Detective macht es einfacher, es zu analysieren,
untersuchen und schnell die Ursache des Potenzials identifizieren
Sicherheitsprobleme oder verdächtige Aktivitäten.

Sie können auch Ihren AWS CloudTrail CloudTrail-Ereignisverlauf durchsuchen, indem Sie
diese Schritte, um Beweise zu analysieren und zu sammeln:

1. Analysieren Sie die AWS CloudTrail CloudTrail-Logs für Folgendes:

1. Irgendwelche ungewöhnlichen Aktivitäten im Zusammenhang mit Logins:

1. Gehen Sie zum [Cloud Trail]
Dashboard] (https://console.aws.amazon.com/cloudtrail).

2. Wählen Sie auf der linken Seite Verlauf der Veranstaltung aus.

3. Ändern Sie im Drop-down-Menü Read-Only in Name des Ereignisses.

4. Überprüfen Sie die verfügbaren Ereignisse auf verdächtige Aktivitäten von
über das Suchfeld nach den folgenden Begriffen suchen:
„*ConsoleLogin*“, „als Umerole übernehmen*“, „*GetFederationToken*“,
„*GetCredentialReport*“, „*CredentialReport generieren*“. Außerdem
es ist wichtig zu beachten, dass „*userIdentity*“ erscheinen sollte
als „*type*“: „*Root*“ für den Root-Benutzer oder „*type*“:
„*IAMUser*“ für alle lokalen IAM-Benutzer auf dem Konto.

<!-- -->

1. Suchen Sie nach allen IAM-Zugangsschlüsseln, IDs und Benutzernamen, die verwendet wurden, um ein
verdächtige Amazon EC2 EC2-Instanz:

1. Öffnen Sie die AWS CloudTrail CloudTrail-Konsole und wählen Sie „*Event“
Geschichte“ * aus dem Navigationsbereich.

2. Wählen Sie das Drop-down-Menü „*Attribute nachschlagen“ * aus, dann
wählen Sie „*Name der Ressource“ *.

3. Fügen Sie in das Feld Ressourcennamen eingeben die EC2-Instanz-ID ein,
und drücken Sie dann auf Ihrem Gerät die Eingabetaste.

4. Erweitern Sie den Eventnamen für*RunInstances*.

5. Kopieren Sie den AWS-Zugriffsschlüssel und notieren Sie sich den Benutzernamen.

2. Überprüfen Sie den AWS CloudTrail CloudTrail-Ereignisverlauf auf Aktivitäten von
kompromittierter Zugangsschlüssel:

1. Öffnen Sie die CloudTrail-Konsole und wählen Sie „*Event“
> Geschichte“ * aus dem Navigationsbereich.

2. Wählen Sie das Drop-down-Menü „*Attribute nachschlagen“ * aus, dann
> wählen Sie „*AWS-Zugriffsschlüssel“ *.

3. Geben Sie im Feld „*Geben Sie einen AWS-Zugangsschlüssel ein“ * den
> kompromittierte IAM-Zugangsschlüssel-ID.

4. Erweitern Sie den Eventnamen für den API-Aufruf *RunInstances*.

3. Wenn Sie über einen externen Identitätsanbieter auf AWS zugreifen, seien Sie
Ich bin sicher, die Zugriffsprotokolle zu überprüfen und die Anweisungen des Anbieters zu befolgen.
über die Reaktion auf ein Ereignis und die Sicherung Ihrer Umgebung.

<!-- -->

1. Analysieren Sie die kompromittierten Anmeldeinformationen von ICH BIN Access Advisor, auf die zuletzt zugegriffen wurde
Informationen, um zu sehen, auf welchen Dienst es zuletzt zugegriffen hat, indem Sie
diese Schritte:

1. Gehen Sie zur [IAM-Konsole] (https://console.aws.amazon.com/iam).

2. Gehen Sie zu Benutzer oder Rollen, klicken Sie auf den Namen des Kompromittierten
Principal, wählen Sie den Tab „*Access Advisor“ * und schauen Sie sich an, welcher
Ressourcen, auf die es zuletzt zugegriffen hat.

## Eindämmung

Nach der Analyse und dem Sammeln weiterer Informationen über das Kompromittierte
Zugangsdaten und alle anderen betroffenen Ressourcen, es ist Zeit zu kontrollieren und
unterdrücken Sie das Sicherheitsereignis.

1. Deaktivieren Sie die IAM-Benutzer, erstellen Sie einen Backup-IAM-Zugriffsschlüssel und dann
deaktivieren Sie den kompromittierten Zugangsschlüssel, indem Sie diesen Schritten folgen:

1. Öffnen Sie die [IAM-Konsole] (https://console.aws.amazon.com/iam/) und
fügen Sie die IAM-Zugriffsschlüssel-ID in die Such-IAM-Leiste ein.

2. Wählen Sie den Benutzernamen und dann „*Sicherheit“
Reiter „Anmeldeinformationen*“.

3. Wählen Sie im Feld Passwort für die Konsole „*Verwalten*“.

4. Wählen Sie unter Konsolenzugriff „*Deaktivieren*“ und dann Anwenden.

5. Für den kompromittierten IAM-Zugangsschlüssel wählen Sie „*Make inactive*“.

2. Wechseln Sie die Zugangsschlüssel, indem Sie diesen Schritten folgen:

1. Erstellen Sie zunächst einen zweiten Schlüssel, indem Sie zum [ICH BIN] gehen.
Konsole] (https://console.aws.amazon.com/iam/).

2. Wählen Sie im Navigationsbereich „*Benutzer“ *.

3. Wählen Sie den Namen des beabsichtigten Benutzers und wählen Sie dann
die Registerkarte „*Sicherheitsanmeldedaten“ *.

4. Wählen Sie im Abschnitt „*Zugangsschlüssel“ * „*Zugangsschlüssel erstellen“ *. Am
die Seite „*Zugriff auf wichtige Best Practices* *und Alternativen“ *,
wählen Sie „*Andere“ *, dann wählen Sie „*Weiter“ . *

5. Dann ändern Sie Ihre Anwendung so, dass sie den neuen Schlüssel verwendet.

6. Den ersten Schlüssel deaktivieren (aber nicht löschen).

7. Wenn es Probleme mit Ihrer Bewerbung gibt, reaktivieren Sie die
vorübergehend eingeben. Wenn Ihre Anwendung voll funktionsfähig ist und
Der erste Schlüssel ist deaktiviert, erst dann ist es sicher, den zu löschen
erster Schlüssel. Stellen Sie sicher, dass Sie alle gelöschten Zugriffe aufzeichnen.
Schlüssel, um weiter in den AWS CloudTrail CloudTrail-Protokollen nach ihnen zu suchen.

3. Widerrufen Sie die aktiven Sitzungen der IAM-Rolle (n), indem Sie die folgenden Schritte befolgen:

1. Öffnen Sie die [IAM-Konsole] (https://console.aws.amazon.com/iam/) und
Gehen Sie zu Rolle und klicken Sie auf die IAM-Rolle, die Sie provozieren möchten, aktiv
Sitzungen für.

2. Klicken Sie auf den IAM-Rollennamen und gehen Sie zum Tab „*Sessions widerrufen“ *.

3. Klicken Sie auf die Schaltfläche „*Aktive Sitzungen widerrufen*“ und bestätigen Sie den Schritt.

4. Isolieren Sie die betroffenen Ressourcen, indem Sie diese Schritte befolgen:

1. Gehen Sie für Amazon EC2 EC2-Instances zur EC2-Instance-Konsole, überprüfen Sie
das Feld neben der EC2-Instance, die Sie isolieren möchten, klicken Sie auf
*Aktionen*, klicken Sie auf *Sicherheit*, dann klicken Sie auf *Sicherheit ändern
Gruppen*. Trennen Sie alle aktuellen Sicherheitsgruppen und fügen Sie eine hinzu
isolierte Sicherheitsgruppe, die eingehende und ausgehende Nachrichten blockiert
Kommunikation mit der EC 2.

2. Verwenden Sie für Amazon S3 S3-Buckets [Bucket
Richtlinien] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)
um verdächtige IP-Adressen am Zugriff auf die S3 zu hindern
Eimer.

5. Identity Center-Sitzungen widerrufen:

Bei Identity Center gibt es zwei Sitzungen, über die man sich Sorgen machen muss.
Das sind die Access-Portal-Sitzung und Rollen-/Anwendungssitzungen:

1. Auf die Portalsitzung zugreifen:

1. Deaktivieren Sie den Benutzer in Identity Center:

1. Navigieren Sie zur Identity Center-Konsole und wählen Sie „Benutzer“.

2. Wählen Sie den Benutzernamen des Benutzers, der deaktiviert ist

3. Klicken Sie im Feld Allgemeine Informationen für den Benutzer auf „Deaktivieren“.
> Benutzerzugriff'

2. Widerrufen Sie alle aktiven Sitzungen:

1. Wählen Sie auf der Benutzerseite in Identity Center „Aktiv“
> Registerkarte „Sitzungen“

2. Wählen Sie eine der aufgelisteten Sitzungen aus und klicken Sie dann auf „Sitzung löschen“.

2. Rollensitzungen widerrufen:

1. Identifizieren Sie den oder die Berechtigungssätze, die vom Benutzer verwendet werden.

1. Klicken Sie in der Identity Center-Konsole auf Berechtigungssätze

2. Wählen Sie den Namen des Berechtigungssatzes.

3. Scrollen Sie nach unten zu „Inline Policy“ und klicken Sie auf die Schaltfläche Bearbeiten

4. Fügen Sie die folgende Richtlinie hinzu:

{

„Version“: „17.10.2012“,

„Aussage“:\ [

{

„Effekt“: „Verweigern“,

„Aktion“: „\ *“,

„Ressource“: „\ *“,

„Zustand“: {

„Zeichenfolge ist gleich“: {

„IdentityStore:UserID“: „Beispiel“

},

„DateLessThan“: {

„AWS:TokenIssueTime“: „2023-09-26T 15:00:00.000 Z“

}

}

}

\]

}

Aktualisieren Sie für diese Richtlinie „Beispiel“ auf die Identity Center-Benutzer-ID des Benutzers.
Die Benutzer-ID finden Sie im Feld „Allgemeine Informationen“ des Benutzers
Benutzerseite. Der Wert für aws:TokenIssueTime sollte der Zeit in entsprechen
für die Sie diese Richtlinie anwenden.

1. Bewerbungssitzungen

Anwendungssitzungen werden erstellt, wenn ein Benutzer auf einen Drittanbieter zugreift
Anwendung oder AWS-Service direkt vom Access-Portal aus.

Um Bewerbungssitzungen zu widerrufen, überprüfen Sie die Dokumentation der
Anwendung, auf die zugegriffen wurde.

1. Kompromittiertes Identitätszentrum

Wenn Ihr Identitätsanbieter kompromittiert ist, sollten Sie den Zugriff auf blockieren
Identity Center von einem kompromittierten Identitätsanbieter (d. h. extern
SAML-basierter Identity Provider (oder Active Directory) durch Änderung der
Identitätsquelle in das „Identitätsquellenverzeichnis“.

Um Ihre Identitätsquelle zu ändern:

6.1 Navigieren Sie zur Identity Center-Konsole

6.2 Wählen Sie „Einstellungen“

6.3 Klicken Sie auf der Einstellungsseite auf den Tab „Identitätsquelle“

6.4 Klicken Sie auf die Schaltfläche „Aktionen“ und wählen Sie dann „Identität ändern“
Quelle'

6.5 Wählen Sie „Identity Center-Verzeichnis“ unter „Identitätsquelle wählen“
Seite

6.6 Klicken Sie auf die Schaltfläche „Weiter“

6.7 Lesen Sie die Informationen im Feld „Überprüfen und bestätigen“,

6.8 Geben Sie „AKZEPTIEREN“ in das entsprechende Feld ein und klicken Sie auf „Ändern“
Schaltfläche „Identitätsquelle“

Bitte beachten Sie, wenn Sie über einen externen Identitätsanbieter auf AWS zugreifen,
Achten Sie darauf, die Zugriffsprotokolle zu überprüfen und die Anweisungen des Anbieters zu befolgen
auf ein Ereignis reagieren und Ihre Umgebung sichern.

Außerdem, wenn Sie von Active Directory zum Identity Center wechseln
Verzeichnis, alle Benutzer und Gruppen werden gelöscht. Berechtigungssätze werden
nicht gelöscht werden, aber Zuweisungen von Berechtigungssätzen werden gelöscht. Wenn Sie
wechseln von einem externen SAML-basierten Identitätsanbieter, alle Benutzer
und Gruppen werden weiterhin in Identity Center angezeigt, genauso wie die
Zuweisungen von Berechtigungssätzen

## Ausrottung

Sobald Sie mit der Eindämmung des Sicherheitsereignisses fertig sind, ist es Zeit zu arbeiten.
über die Beseitigung und Beseitigung der Ursache des Sicherheitsereignisses.

1. Entfernen Sie alle Ressourcen, die durch die kompromittierten Schlüssel erstellt wurden, die
in der „Analysephase Schritt 1“ entdeckt. Überprüfen Sie alle AWS-Regionen, sogar
Regionen, in denen Sie keine AWS-Ressourcen einsetzen. Wenn Sie müssen
Halten Sie eine Ressource für Nachforschungen bereit, erwägen Sie, sie zu sichern.

2. Suchen Sie nach und
[entfernen] (https://repost.aws/knowledge-center/terminate-resources-account-closure)
alle unbekannten Dienste, die in Ihrem Konto laufen. Zahlen
besondere Aufmerksamkeit gilt den folgenden Ressourcen:

1. EC2-Instances und AMIs, einschließlich Instances in den gestoppten
Bundesstaat.

2. EBS-Volumes und Snapshots.

3. Funktionen und Ebenen von AWS Lambda.

3. Entfernen Sie unnötige Berechtigungen aus der Protokollierung von S3-Buckets oder Log
Aggregationsressourcen, die in der „Erkennungsphase, Schritt 6“ identifiziert wurden
könnte verwendet werden, um einer Entdeckung zu entgehen. Auf diese Weise können Sie unbeabsichtigte Personen identifizieren.
Zugriff auf Ihre Ressourcen und Daten.

4. Entfernen Sie alle offengelegten Daten, die nicht für den Betrieb benötigt werden.

5. Führen Sie Scans nach Sicherheitslücken auf öffentlich zugänglichen Räumen durch
Ressourcen. Sie können Tools wie [Amazon] verwenden.
Inspektor] (https://docs.aws.amazon.com/inspector/latest/user/scanning-resources.html)
<sup>um das Betriebssystem und die Softwareanwendung von Drittanbietern zu scannen für</sup>
Sicherheitslücken.

## Erholung

Sobald Sie mit der Beseitigung der Ursachen von Sicherheitsereignissen fertig sind. Es ist Zeit
um die betroffenen Ressourcen wieder in einen bekannten Zustand zu versetzen.

1. Stellen Sie notwendige Daten aus bekannten sauberen Backups wieder her, die älter sind als
Veranstaltung:

1. [Wiederherstellung von einem Amazon EBS-Snapshot oder einem
AMI] (https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/restore.html).

2. [Aus einer Datenbank wiederherstellen
Schnappschuss] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) Amazon
RDS.

3. [Frühere Version wiederherstellen
Versionen] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RestoringPreviousVersions.html) Amazon
S3-Objektversionen.

2. Falls nötig, Systeme von Grund auf neu aufbauen, einschließlich erneuter Bereitstellung
aus vertrauenswürdiger Quelle mit Automatisierung, manchmal in einem neuen AWS-Konto.

3. Wenn der Zugriff auf die Multi-Faktor-Authentifizierung „MFA“ verloren geht und Sie nicht
Lassen Sie bitte ein anderes MFA-Gerät auf Ihrem Root-Konto registrieren
folgen Sie diesen Schritten:

1. Melden Sie sich beim [AWS-Management] an.
[Konsole] (https://console.aws.amazon.com/) als Kontoinhaber
indem Sie Root-Benutzer wählen und die E-Mail-Adresse Ihres AWS-Kontos eingeben
Adresse. Geben Sie auf der nächsten Seite Ihr Passwort ein.

2. Wählen Sie auf der Seite Zusätzliche Überprüfung erforderlich eine MFA aus.
Methode, mit der Sie sich authentifizieren und Weiter wählen.

3. Je nachdem, welche Art von MFA Sie verwenden, sollten Sie eine sehen
andere Seite, aber die Option „*Troubleshooting MFA“ * funktioniert
Das Gleiche. Auf der Seite „*Zusätzliche Überprüfung erforderlich“ *
oder Seite „*Multi-Faktor-Authentifizierung“ *, wählen Sie „*Problembehandlung
MFA“ *.

4. Falls erforderlich, geben Sie Ihr Passwort erneut ein und wählen Sie*Anmelden*.

5. Auf der Seite „*Problembehandlung mit Ihrem Authentifizierungsgerät“ *, in
das „*Melden Sie sich mit alternativen Faktoren an
Abschnitt Authentifizierung“ *, wählen Sie „*Melden Sie sich mit einer Alternative an
Faktoren“ *.

6. Auf dem „*Melden Sie sich mit alternativen Faktoren an“ von
Authentifizierung“ * Seite, authentifizieren Sie Ihr Konto, indem Sie verifizieren
die E-Mail-Adresse, wählen Sie „*Bestätigungs-E-Mail senden“ *

7. Suchen Sie in der E-Mail, die mit Ihrem AWS-Konto verknüpft ist, nach einem
Nachricht von Amazon Web Services
(recover-mfa-no-reply@verify.signin.aws). Folgen Sie den Anweisungen
in der E-Mail.

> Wenn Sie die E-Mail nicht in Ihrem Konto sehen, überprüfen Sie Ihren Spam-Ordner oder
> kehren Sie zu Ihrem Browser zurück und wählen Sie „*E-Mail erneut senden*“.

1. Nachdem Sie Ihre E-Mail-Adresse bestätigt haben, können Sie mit der Authentifizierung fortfahren.
Ihr Konto. Um Ihre primäre Kontakttelefonnummer zu überprüfen,
wählen Sie Rufen Sie mich jetzt an.

2. Nehmen Sie den Anruf von AWS entgegen und geben Sie, wenn Sie dazu aufgefordert werden, die sechsstellige Zahl ein
Nummer von der AWS-Website auf Ihrer Telefontastatur.

> Wenn Sie keinen Anruf von AWS erhalten, wählen Sie Anmelden, um sich bei der
> Nochmals Konsole und neu anfangen. Oder siehe [Verloren oder unbrauchbar] Multi-Factor
> Authentifizierung (MFA)
> Gerät] (https://support.aws.amazon.com/#/contacts/aws-mfa-support) zu
> kontaktieren Sie den Support für Hilfe.

1. Nachdem Sie Ihre Telefonnummer verifiziert haben, können Sie sich bei Ihrem Konto anmelden
> indem Sie Auf der Konsole anmelden wählen.

2. Der nächste Schritt hängt von der Art der MFA ab, die Sie verwenden:

1. Für ein virtuelles MFA-Gerät entfernen Sie das Konto von Ihrem Gerät.
> Dann gehen Sie zur [AWS-Sicherheit]
> Referenzen] (https://console.aws.amazon.com/iam/home? #security_credential) Seite
> und löschen Sie die alte MFA-Entität für virtuelle Geräte, bevor Sie sie erstellen
> ein neuer.

2. Einen FIDO-Sicherheitsschlüssel finden Sie unter [AWS-Sicherheit]
> Referenzen] (https://console.aws.amazon.com/iam/home? #security_credential) Seite
> und deaktivieren Sie den alten FIDO-Sicherheitsschlüssel, bevor Sie einen neuen aktivieren
> eins.

3. Für ein Hardware-TOTP-Token wenden Sie sich an den Drittanbieter für
> Hilfe bei der Reparatur oder beim Austausch des Geräts. Sie können weiter unterschreiben
> bei der Verwendung alternativer Authentifizierungsfaktoren, bis Sie
> erhalten Sie Ihr neues Gerät. Nachdem Sie die neue Hardware-MFA haben
> Gerät, gehen Sie zur [AWS-Sicherheit]
> Referenzen] (https://console.aws.amazon.com/iam/home? #security_credential) Seite
> und löschen Sie die alte MFA-Hardware-Geräteeinheit vor Ihnen
> erstellen Sie ein neues.

<!-- -->

1. Bestätigen Sie, dass die IAM-Principals über den entsprechenden Zugriff und die entsprechenden Berechtigungen verfügen
nach dem Sicherheitsereignis.

2. Behebung von Sicherheitslücken und Installation der erforderlichen Patches mit [AWS]
SSM-Patch
Manager] (https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html)
oder jedes andere <sup>Drittanbieter-Tool</sup>, das Sie verwenden.

3. Ersetzen Sie alle kompromittierten/beschädigten Dateien durch saubere Versionen von
Backup.

## Aktivität nach dem Vorfall

Nachdem Sie sich erfolgreich von dem Sicherheitsvorfall erholt haben,
sollte die in [*AWS Security Incident] beschriebenen Übungen durchführen
Leitfaden zur Reaktion nach einem Vorfall
Aktivität*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/establish-framework-for-learning.html).
Das wird Ihnen helfen, die Lehren aus dem Vorfall zu dokumentieren, Maßnahme
und verbessern Sie die Effektivität Ihrer Fähigkeiten zur Reaktion auf Vorfälle,
und implementieren zusätzliche Kontrollen, um einen ähnlichen Vorfall zu verhindern
wiederkehrend.

## Fazit

In diesem Playbook haben wir die ersten Schritte beschrieben, die zu ergreifen sind, wenn ein
Ein Ereignis, das die Sicherheit Ihrer Anmeldeinformationen gefährdet hat, tritt in Ihren AWS-Konten auf.
Dazu gehören die Inanspruchnahme AWS AWS-Supports, die Erkennung eines Kompromisses, die Analyse von
Ereignisse in Ihrem Konto (en), die das Sicherheitsereignis beinhalten,
Beseitigung der Bedrohung, Wiederherstellung Ihres Kontos (s) auf „zweifelsfrei funktionierend“
Betriebszustand und Aktivitäten nach dem Vorfall, einschließlich Unterricht
gelernt. Als nächsten Schritt lesen Sie bitte die folgenden AWS-Ressourcen, um
helfen Sie dabei, Ihre Fähigkeiten zur Reaktion auf Vorfälle zu verbessern:

1. [Reaktion auf AWS-Sicherheitsvorfälle
Leitfaden] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.html)

2. [AWS Customer Playbook Framework für kompromittiertes ICH BIN]
Referenzen] (https://github.com/aws-samples/aws-customer-playbook-framework/blob/main/docs/Compromised_IAM_Credentials.md)

3. [Bewährte AWS-Sicherheitsmethoden in
ICH BIN] (https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

4. [AWS Well Architected Framework — Sicherheit
[Säule] (https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

## Anhang A — Protokolle überprüfen

**CloudTrail-Protokollstruktur**

Wenn Sie CloudTrail-Logs durchsuchen, ist es wichtig zu verstehen
die verschiedenen Felder, die in einem CloudTrail-Ereignisdatensatz enthalten sind. Für ein
Vollständige Liste, bitte beziehen Sie sich auf den [<u>offiziellen] Cloud Trail
</u>Dokumentation] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feld</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Uhrzeit der Veranstaltung</td>
<td>Datum und Uhrzeit der Bearbeitung der Anfrage, koordiniert
Weltzeit (UTC).</td>
</tr>
<tr class="even">
<td>Name der Veranstaltung</td>
<td>Die angeforderte Aktion, eine der Aktionen in der API für
dieser Service</td>
</tr>
<tr class="odd">
<td>Quelle des Ereignisses</td>
<td>Der Dienst, an den die Anfrage gestellt wurde. Dieser Name ist normalerweise ein
Kurzform des Servicenamens ohne Leerzeichen plus .amazonaws.com.</td>
</tr>
<tr class="even">
<td>Benutzeridentität</td>
<td>Informationen über die IAM-Identität, die eine Anfrage gestellt hat. Für mehr
Informationen, siehe <a
<u>href=“ https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html „> Cloud Trail
Element „Benutzeridentität</u></a></td>“.
</tr>
<tr class="odd">
<td>Ressourcen</td>
<td><p>Eine Liste der Ressourcen, auf die bei der Veranstaltung zugegriffen wurde. Das Feld kann enthalten
die folgenden Informationen:</p>
<ul>
<li><p>Ressourcen-ARNs</p></li>
<li><p>Konto-ID des Ressourcenbesitzers</p></li>
<li><p>Kennung des Ressourcentyps im Format:
AWS: :aws-service-name: :datentypname</p></li>
</ul></td>
</tr>
<tr class="even">
<td>AWS-Region</td>
<td>Die AWS-Region, in der die Anfrage gestellt wurde, z. B. us-east-2</td>
</tr>
<tr class="odd">
<td>Quell-IP-Adresse</td>
<td>Die IP-Adresse, von der aus die Anfrage gestellt wurde. Für Aktionen, die
stammen von der Servicekonsole, die angegebene Adresse ist für
zugrunde liegende Kundenressource, nicht der Konsolen-Webserver.</td>
</tr>
</tbody>
</table>

**Veranstaltungen**

Es gibt eine Reihe von Aktionen, die Bedrohungsakteure danach durchführen werden.
kompromittierende Kontoanmeldedaten. Es ist zwar unpraktisch, alle aufzulisten
Mögliche Aktion, hier sind einige Muster, nach denen Sie während Ihres
Untersuchung.

**Hinweis: ** Die unten aufgeführten API-Aktionen deuten nicht unbedingt auf eine Sicherheit hin
Ein Vorfall ist eingetreten. Evaluieren Sie alle protokollierten Aktivitäten innerhalb des Kontextes
Ihrer Untersuchung.

**Änderungen an der SAML-/OIDC-Identitätsanbieter-Konfiguration**

Bedrohungsakteure können SAML/OIDC-Identitätsanbieter erstellen oder modifizieren
Konfigurationen zur Vermeidung von Entdeckungen und zur Aufrechterhaltung der Persistenz innerhalb Ihres
Cloud-Umgebung.

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API-Aktion</strong></th>
<th><strong>Beschreibung</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Ich bin: SAML-Anbieter erstellen</td>
<td>Erstellt eine IAM-Ressource, die einen Identity Provider (IdP) beschreibt
das unterstützt SAML 2.0</td>
</tr>
<tr class="even">
<td>Ich bin: SAML-Anbieter löschen</td>
<td>Löscht eine SAML-Provider-Ressource in ICH BIN.</td>
</tr>
<tr class="odd">
<td>Ich bin: SAML-Anbieter aktualisieren</td>
<td>Aktualisiert das Metadaten-Dokument für einen bestehenden SAML-Anbieter</td>
</tr>
<tr class="even">
<td>Ich bin: Create OpenID Connect Provider</td>
<td>Erstellt eine IAM-Entität, um einen Identity Provider (IdP) zu beschreiben, der
unterstützt OpenID Connect (OIDC)</td>.
</tr>
<tr class="odd">
<td>Ich bin: Kunden-ID zu OpenID Connect Provider hinzufügen</td>
<td>Fügt der Kundenliste eine neue Kunden-ID (auch bekannt als Audience) hinzu
IDs sind bereits für das angegebene ICH BIN OpenID Connect (OIDC) registriert
Anbieter-Ressource</td>
</tr>
<tr class="even">
<td>Ich bin: DeleteOpenIDConnect-Anbieter</td>
<td>Löscht ein OpenID Connect Identity Provider (IdP) -Ressourcenobjekt in
ICH BIN</td>
</tr>
<tr class="odd">
<td>Ich bin: Client-ID vom OpenID Connect-Anbieter entfernen</td>
<td>Entfernt die angegebene Kunden-ID (auch bekannt als Zielgruppe) aus dem
Liste der Client-IDs, die für das angegebene ICH BIN OpenID Connect registriert sind
(OIDC) Provider-Ressourcenobjekt</td>
</tr>
<tr class="even">
<td>Ich bin: Update OpenID Connect Provider Thumbprint</td>
<td>Ersetzt die bestehende Liste von Fingerabdrücken von Serverzertifikaten
verknüpft mit einem OpenID Connect (OIDC) Provider-Ressourcenobjekt mit einem
neue Liste von Fingerabdrücken</td>
</tr>
</tbody>
</table>

**Änderungen an der IAM-Konfiguration**

Bedrohungsakteure können IAM-Prinzipale, Anmeldeinformationen erstellen oder ändern oder
Berechtigungen, um eine Entdeckung zu verhindern oder die Persistenz in Ihrer Cloud aufrechtzuerhalten
Umwelt.

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>API-Aktion</th>
<th>Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Ich bin: Passwort ändern</td>
<td>Ändert das Passwort des IAM-Benutzers, der das anruft
</td>Betrieb
</tr>
<tr class="even">
<td>Ich bin: Create User</td>
<td>Erstellt einen neuen IAM-Benutzer für Ihr AWS-Konto</td>
</tr>
<tr class="odd">
<td>Ich bin: CreateRole</td>
<td>Erstellt eine neue Rolle für Ihr AWS-Konto</td>
</tr>
<tr class="even">
<td>Ich bin: Gruppe erstellen</td>
<td>Erstellt eine neue Gruppe</td>
</tr>
<tr class="odd">
<td>Ich bin: Benutzerrichtlinie anhängen</td>
<td>Hängt die angegebene verwaltete Richtlinie an den angegebenen Benutzer an</td>
</tr>
<tr class="even">
<td>Ich bin: Attach Role Policy</td>
<td>Hängt die angegebene verwaltete Richtlinie an die angegebene IAM-Rolle an</td>
</tr>
<tr class="odd">
<td>Ich bin: Gruppenrichtlinie anhängen</td>
<td>Hängt die angegebene verwaltete Richtlinie an das angegebene ICH BIN an.
</td>Gruppe
</tr>
<tr class="even">
<td>Ich bin: List Access Keys</td>
<td>Gibt Informationen über die Zugangsschlüssel-IDs zurück, die mit dem verknüpft sind
spezifizierter IAM-Benutzer</td>
</tr>
<tr class="odd">
<td>Ich bin: Policy-Version erstellen</td>
<td>Erstellt eine neue Version der angegebenen verwalteten Richtlinie.</td>
</tr>
<tr class="even">
<td>Ich bin: Login-Profil aktualisieren</td>
<td>Ändert das Passwort für den angegebenen IAM-Benutzer</td>
</tr>
<tr class="odd">
<td>Ich bin: Create Access Key</td>
<td>Erstellt einen neuen geheimen AWS-Zugriffsschlüssel und den entsprechenden AWS-Zugriffsschlüssel
ID für den angegebenen Benutzer</td>
</tr>
<tr class="even">
<td>Ich bin: Zugangsschlüssel aktualisieren</td>
<td>Ändert den Status des angegebenen Zugriffsschlüssels von Aktiv zu
Inaktiv oder umgekehrt.</td>
</tr>
<tr class="odd">
<td>Ich bin: MFA-Gerät deaktivieren</td>
<td>Deaktiviert das angegebene MFA-Gerät und entfernt es aus der Zuordnung
mit dem Benutzernamen, für den es ursprünglich aktiviert wurde</td>
</tr>
</tbody>
</table>

**Änderungen der Protokollierungs- und Überwachungskonfigurationen**

Bedrohungsakteure können die Ressourcenüberwachung deaktivieren oder Protokolle löschen, um zu verhindern
Erkennung oder Verhinderung von Ermittlungen bei Vorfällen.

<table>
<colgroup>
<col style="width: 46%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header">
<th>CloudTrail: Trail löschen</th>
<th>Löscht einen Trail — deaktiviert die Übertragung von CloudTrail-Ereignissen an einen
Amazon S3 S3-Bucket, CloudWatch Logs oder Amazon EventBridge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>CloudTrail: Stoppen Sie die Protokollierung</td>
<td>Unterbricht die Aufzeichnung von AWS-API-Aufrufen und die Bereitstellung von Protokolldateien für
der angegebene Weg</td>
</tr>
<tr class="even">
<td>CloudTrail: Trail aktualisieren</td>
<td>Aktualisiert die Einstellungen, die die Lieferung von Protokolldateien spezifizieren</td>
</tr>
<tr class="odd">
<td>CloudTrail: Event-Datenspeicher löschen</td>
<td>Löscht ein <a
<u>href=“ https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store.html „> Cloud Trail
Lake Event Date Store</u></a></td>
</tr>
<tr class="even">
<td>Wachdienst: Detektor löschen</td>
<td>Löscht einen Amazon Guard Duty GuardDuty-Detektor und deaktiviert GuardDuty in einem
bestimmte Region</td>
</tr>
<tr class="odd">
<td>Wachdienst: Verlagsziel löschen</td>
<td>Löscht das Veröffentlichungsziel, in das die Ergebnisse exportiert werden
</td>zu
</tr>
<tr class="even">
<td>Access Analyzer: Analyzer löschen</td>
<td>Löscht den angegebenen Analyzer und deaktiviert den ICH BIN Access Analyzer
Service für diese Region.</td>
</tr>
<tr class="odd">
<td>Config: Config-Regel löschen</td>
<td>Löscht die angegebene AWS-Config-Regel und ihre gesamte Auswertung
</td>Ergebnisse
</tr>
<tr class="even">
<td>Config: Configuration Recorder löschen</td>
<td>Löscht den AWS-Konfiguration Config-Konfigurationsrekorder</td>
</tr>
<tr class="odd">
<td>Config: Delevery Channel</td>
<td>Löscht den AWS-Konfiguration Config-Lieferkanal</td>
</tr>
<tr class="even">
<td>Config: Configuration Recorder beenden</td>
<td>Stoppt die Aufzeichnung von Konfigurationen und Konfigurationsänderungen für
angegebene Aufnahmegruppe</td>
</tr>
</tbody>
</table>

**Änderungen an der S3-Bucket-Konfiguration**

Bedrohungsakteure können S3-Bucket-Konfigurationen ändern oder löschen, um
Daten exfiltrieren.

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>S3: Buckets auflisten</th>
<th>Gibt eine Liste aller Buckets zurück, die dem authentifizierten Absender von gehören
Die Anfrage</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>S3: Bucket erstellen</td>
<td>Erstellt einen neuen Bucket</td>
</tr>
<tr class="even">
<td>S3: Bucket Public Access Block löschen</td>
<td>Entfernt die PublicAccessBlock-Konfiguration für einen Amazon S3
</td>Eimer
</tr>
<tr class="odd">
<td>S3: Putbucket-Richtlinie</td>
<td>Fügt eine Richtlinie für einen Bucket hinzu oder aktualisiert sie</td>
</tr>
<tr class="even">
<td>S3: Bucket-Richtlinie löschen</td>
<td>Löscht die Richtlinie aus dem Bucket</td>
</tr>
<tr class="odd">
<td>S3: Objekt ACL setzen</td>
<td>Legt die Zugriffssteuerungslisten (ACL) -Berechtigungen für ein Objekt in einem
Amazon S3 S3-Bucket</td>
</tr>
<tr class="even">
<td>S3: Objekt ACL löschen</td>
<td>Löschen Sie die Zugriffskontrollliste (ACL) eines Objekts</td>
</tr>
<tr class="odd">
<td>S3: Bucket Cors</td>
<td>Legt die CORS-Konfiguration für Ihren Bucket fest</td>
</tr>
<tr class="even">
<td>S3: Bucket Cors löschen</td>
<td>Löscht die CORS-Konfigurationsinformationen für den Bucket</td>
</tr>
<tr class="odd">
<td>S3: Putbucket-Verschlüsselung</td>
<td>Konfigurieren Sie die Standardverschlüsselung und Amazon S3 Bucket Keys für ein
vorhandener Bucket</td>
</tr>
<tr class="even">
<td>S3: Bucket-Verschlüsselung löschen</td>
<td>Setzt die Standardverschlüsselung für den Bucket auf serverseitig zurück
Verschlüsselung mit verwalteten Amazon S3 S3-Schlüsseln (SSE-S3</td>)
</tr>
</tbody>
</table>

**Andere Aktionen**

Hier sind einige andere Aktionen, die Sie während Ihres
Ermittlung

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>KMS: DisableKey</th>
<th>Setzt den Status eines KMS-Schlüssels auf deaktiviert. Diese Änderung ist vorübergehend
verhindert die Verwendung des KMS-Schlüssels für kryptografische Operationen</th>.
</tr>
</thead>
<tbody>
<tr class="odd">
<td>KMS: Löschen des Schlüssels planen</td>
<td>Plant das Löschen eines KMS-Schlüssels.</td>
</tr>
<tr class="even">
<td>KMS: PutKey-Richtlinie</td>
<td>Hängt eine Schlüsselrichtlinie an den angegebenen KMS-Schlüssel an. Die wichtigsten Richtlinien sind
primärer Weg, den Zugriff auf KMS-Schlüssel zu kontrollieren.</td>
</tr>
<tr class="odd">
<td>KMS: Schlüssel erstellen</td>
<td>Erstellt einen eindeutigen, vom Kunden verwalteten KMS-Schlüssel in Ihrem AWS-Konto und
Region.</td>
</tr>
<tr class="even">
<td>EC2: Instanzen beschreiben</td>
<td>Beschreibt EC2-Instances in Ihrem Konto.</td>
</tr>
<tr class="odd">
<td>EC2: Instanzen ausführen</td>
<td>Startet EC2-Instances in Ihrem Konto.</td>
</tr>
<tr class="even">
<td>EC2: VPC erstellen</td>
<td>Erstellt eine VPC in Ihrem Konto.</td>
</tr>
<tr class="odd">
<td>EC2: Beschreiben Sie VPCs</td>
<td>Beschreibt VPCs in Ihrem Konto.</td>
</tr>
<tr class="even">
<td>EC2: Sicherheitsgruppe erstellen</td>
<td>Erstellt eine Sicherheitsgruppe. Eine Sicherheitsgruppe agiert als virtuelle
Firewall für Ihre Instanz zur Kontrolle von eingehendem und ausgehendem Verkehr.</td>
</tr>
<tr class="odd">
<td>EC2: Sicherheitsgruppe löschen</td>
<td>Löscht eine Sicherheitsgruppe.</td>
</tr>
<tr class="even">
<td>RDS: DB-Instance erstellt</td>
<td>Erstellt eine RDS-Datenbankinstanz.</td>
</tr>
<tr class="odd">
<td>RDS: DB-Instance gelöscht</td>
<td>Löscht eine RDS-Datenbankinstanz.</td>
</tr>
</tbody>
</table>