# Playbook zur Reaktion auf Vorfälle: Reaktion auf Amazon Q-Sicherheitsereignisse

Dieses Dokument dient nur zu Informationszwecken. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Zeitpunkt der Veröffentlichung dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Kunden sind dafür verantwortlich, die Informationen in diesem Dokument und die Nutzung von AWS-Produkten oder -Services, die jeweils „wie sie sind“ und ohne jegliche ausdrückliche oder stillschweigende Garantie bereitgestellt werden, selbst zu beurteilen. Dieses Dokument stellt keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern dar. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Verträge geregelt, und dieses Dokument ist weder Teil einer Vereinbarung zwischen AWS und seinen Kunden noch ändert es diese.

© 2024 Amazon Web Services, Inc. oder seine Tochtergesellschaften. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors`\
Genehmiger: `Name des Genehmigers`\
Letztes Genehmigungsdatum:

## Zusammenfassung
In diesem Playbook werden Beispielprozesse und Abfragen zur Untersuchung von Sicherheitsereignissen im Zusammenhang mit Amazon Q beschrieben.

## Vorbereitung
Dieses Playbook bezieht sich auf [Prowler] (https://github.com/toniblyx/prowler) und ist, soweit möglich, in dieses integriert. Dabei handelt es sich um ein Befehlszeilentool, das Sie bei der AWS-Sicherheitsbewertung, Prüfung, Abhärtung und Reaktion auf Vorfälle unterstützt.

### Ziele
Konzentrieren Sie sich bei der Umsetzung des Playbooks auf die _***gewünschten Ergebnisse***_ und machen Sie sich Notizen zur Verbesserung der Fähigkeiten zur Reaktion auf Vorfälle.

#### Ermitteln Sie:
* **Ausgenutzte Sicherheitslücken**
* **Exploits und Tools beobachtet**
* **Absicht des Schauspielers**
* **Zuschreibung des Schauspielers**
* **Umwelt- und Unternehmensschädigungen**

#### Wiederherstellen:
* **Kehren Sie zur ursprünglichen und gehärteten Konfiguration zurück**

#### Verbessern Sie die Komponenten von CAF Security Perspective:
[Sicherheitsperspektive des AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Richtlinie**
* **Detektiv**
* **Reaktionsfähig**
* **Präventiv**

! [Bild] (/images/aws_caf.png)
* * *

### Klassifizierung und Behandlung von Vorfällen
* **Taktiken, Techniken und Verfahren**: Amazon Q
* **Kategorie**: Protokollanalyse
* **Ressource**: Amazon Q
* **Indikatoren**: Informationen zu Cyberbedrohungen, Hinweis Dritter
* **Protokollquellen**: CloudTrail
* **Teams**: Security Operations Center (SOC), forensische Ermittler, Cloud-Engineering

## Prozess zur Bearbeitung von Vorfällen
### Der Prozess zur Reaktion auf Vorfälle umfasst die folgenden Phasen:
* Vorbereitung
* Erkennung und Analyse
* Eindämmung und Ausrottung
* Erholung
* Aktivitäten nach dem Vorfall

### Schritte zur Reaktion
1. [**VORBEREITUNG**]
2. [**VORBEREITUNG**]
3. [**VORBEREITUNG**]
4. [**ERKENNUNG UND ANALYSE**] Erkennung durchführen und CloudTrail auf unbekannte API-Ereignisse analysieren
5. [**ERKENNUNG UND ANALYSE**] Erkennung durchführen und CloudWatch auf unerkannte Ereignisse analysieren
6. [**CONTAINMENT & ERADICATION**] IAM-Benutzerschlüssel löschen oder rotieren
7. [**EINDÄMMUNG UND AUSROTTUNG**] Unbekannte Ressourcen löschen oder rotieren

## Vorbereitung
* Beurteilen Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
* AWS hat ein neues Open-Source-Tool Self-Service Security Assessment entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in den Sicherheitsstatus ihres AWS-Kontos zu gewinnen.
* Führen Sie ein vollständiges Inventar aller Ressourcen, einschließlich Servern, Netzwerkgeräten, Netzwerk-/Dateifreigaben und Entwicklercomputern
* Erwägen Sie die Implementierung von AWS GuardDuty zur kontinuierlichen Überwachung auf böswillige Aktivitäten und unbefugtes Verhalten zum Schutz Ihrer AWS-Konten, Workloads und in Amazon SES gespeicherten Daten
* Implementierung von CIS AWS Foundations, einschließlich Ablauf von Konten und obligatorischer Rotation von Anmeldeinformationen
* Erzwingen Sie die Multi-Faktor-Authentifizierung (MFA)
* Setzen Sie die Anforderungen an die Passwortkomplexität durch und legen Sie Ablaufzeiträume fest
* Führen Sie einen IAM Credential Report aus, um alle Benutzer in Ihrem Konto und den Status ihrer verschiedenen Anmeldeinformationen, einschließlich Kennwörtern, Zugriffsschlüsseln und MFA-Geräten, aufzulisten
* Verwenden Sie AWS IAM Access Analyzer, um die Ressourcen in Ihrer Organisation und Konten zu identifizieren, z. B. IAM-Rollen, die mit einer externen Entität gemeinsam genutzt werden. Auf diese Weise können Sie einen unbeabsichtigten Zugriff auf Ihre Ressourcen und Daten identifizieren, der ein Sicherheitsrisiko darstellt
* Erwägen Sie die Implementierung von AWS GuardDuty zur kontinuierlichen Überwachung auf böswillige Aktivitäten und unbefugtes Verhalten, um Ihre AWS-Konten und Workloads zu schützen.

## Eskalationsverfahren
- `Ich benötige eine Geschäftsentscheidung darüber, wann EC2-Forensik durchgeführt werden soll. `
- `Wer überwacht die Protokolle/Warnmeldungen, empfängt sie und reagiert darauf? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Presse- und Rechtsabteilung in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS-Support wenden, um Hilfe zu erhalten? `

## Amazon Q-Modell

Amazon Q besteht aus mehreren Komponenten/Diensten:

* Chat — Amazon Q beantwortet Fragen zu AWS in natürlicher Sprache in englischer Sprache, einschließlich Fragen zur AWS-Serviceauswahl, zur Nutzung der AWS-Befehlszeilenschnittstelle (AWS CLI), zur Dokumentation und zu bewährten Methoden. Amazon Q antwortet mit Informationszusammenfassungen oder schrittweisen Anleitungen und enthält Links zu seinen Informationsquellen.
* Speicher — Amazon Q verwendet den Kontext Ihrer Konversation als Grundlage für zukünftige Antworten für die Dauer Ihrer Konversation.
* Codeverbesserungen und Ratschläge — In IDEs kann Amazon Q Fragen zur Softwareentwicklung beantworten, Ihren Code verbessern und neuen Code generieren.
* Fehlerbehebung und Support — Amazon Q kann Ihnen helfen, Fehler in der AWS-Managementkonsole zu verstehen, und bietet Zugang zu Live-AWS-Supportagenten, die sich mit Ihren AWS-Fragen und -Problemen befassen.
* Kundenfeedback — Amazon Q verwendet die Informationen und das Feedback, das Sie über Feedback-Formulare einreichen, um Support zu bieten oder technische Probleme zu beheben.

## Erkennung und Analyse

### Hinweise und Überlegungen

* Q ist nicht in allen Regionen verfügbar (2024-05-22). Informationen zur aktuellen Verfügbarkeit in der Region finden Sie in [dem Entwicklerhandbuch] (https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/regions.html)
* Model Invocation Logging ist derzeit kein Feature von Q. Es ist geplant, es in Zukunft hinzuzufügen. Dies ist eine Einstellung auf Regionsebene und nicht die Standardeinstellung
* Endpunkte:
* Q Business (Entwickler): qbusiness.amazonaws.com
* Q Chat: q.amazonaws.com

### Namen der Ereignisse

Die in der Tabelle unten aufgeführten Ereignisnamen dienen als Kurzreferenz für Ereignistypen, die in der Regel in CloudTrail bei der Untersuchung eines Sicherheitsereignisses auftreten, an dem Q beteiligt ist. Von den in der Tabelle aufgeführten Ereignisnamen sind die häufigsten, die bei normaler Modellnutzung protokolliert werden:

* `GetConversation`
* `Konversation starten`
* `Index abrufen`
* `ListRetriever`
* `Anwendungen abrufen`

| **Namen der Veranstaltungen** | **Beschreibung** |
|: -----------------|: ------------------------------------------------- |
| ***Allgemeine Genehmigungen*** | --- |
| `getConversation` | Erteilt die Erlaubnis, einzelne Nachrichten zu erhalten, die mit einer bestimmten Konversation mit Amazon Q verknüpft sind |
| `getTroubleshootingResults` | Erteilt die Erlaubnis, Ergebnisse der Fehlerbehebung mit Amazon Q abzurufen |
| `sendMessage` | Erteilt die Erlaubnis, eine Nachricht an Amazon Q zu senden |
| `StartConversation` | Erteilt die Erlaubnis, eine Konversation mit Amazon Q zu beginnen |
| `StartTroubleshootingAnalysis` | StartTroubleshootingAnalysisErteilt die Erlaubnis, eine Fehlerbehebungsanalyse mit Amazon Q zu starten
| `StartTroubleshootingResolutionExplanation` | Erteilt die Erlaubnis, eine Erklärung zur Problembehebung mit Amazon Q zu starten |
| --- | --- |
| ***Anwendung erstellen*** | -------- |
| `CreateApplication` | Erteilt die Erlaubnis, eine Anwendung zu erstellen |
| `DeleteApplication` | Erteilt die Erlaubnis, eine Anwendung zu löschen |
| `getApplication` | Erteilt die Erlaubnis, eine Anwendung abzurufen |
| `ListApplications` | Erteilt die Erlaubnis, die Anwendungen aufzulisten|
| `UpdateApplication` | Erteilt die Erlaubnis, eine Anwendung zu aktualisieren |
| --- | --- |
| ***Einen Index erstellen*** | -------- |
| `createIndex` | Erteilt die Erlaubnis, einen Index für eine bestimmte Anwendung zu erstellen |
| `deleteIndex` | Erteilt die Erlaubnis, einen Index zu löschen |
| `getIndex` | Erteilt die Erlaubnis, einen Index abzurufen |
| `listIndices` | Erteilt die Erlaubnis, die Indizes einer Anwendung aufzulisten |
| `updateIndex` | Erteilt die Erlaubnis, einen Index zu aktualisieren |
| --- | --- |
| ***Einen Retriever erstellen*** | -------- |
| `createRetriever` | Erteilt die Erlaubnis, einen Retriever für eine bestimmte Anwendung zu erstellen |
| `DeleteRetriever` | Erteilt die Erlaubnis, einen Retriever zu löschen |
| `getRetriever` | Erteilt die Erlaubnis, einen Retriever zu bekommen |
| `ListRetriever` | Erteilt die Erlaubnis, die Retriever einer Anwendung aufzulisten |
| `UpdateRetriever` | Erteilt die Erlaubnis, einen Retriever zu aktualisieren |
| --- | --- |
| ***Datenquellen verbinden*** | -------- |
| `CreateDataSource` | Erteilt die Erlaubnis, eine Datenquelle für eine bestimmte Anwendung und einen Index zu erstellen |
| `DeleteDataSource` | Erteilt die Erlaubnis, eine DataSource zu löschen |
| `getDataSource` | Erteilt die Erlaubnis, eine Datenquelle abzurufen |
| `ListDataSources` | Erteilt die Erlaubnis, die Datenquellen einer Anwendung und eines Indexes aufzulisten |
| `UpdateDataSource` | Erteilt die Erlaubnis, eine DataSource zu aktualisieren |
| `StartDataSourceSyncJobs` | Erteilt die Erlaubnis, den Datenquellen-Synchronisierungsjob zu starten |
| `StopDataSourceSyncJobs` | Erteilt die Erlaubnis, den Datenquellen-Synchronisierungsjob zu beenden |
| `ListDataSourceSyncJobs` | Erteilt die Erlaubnis, den Verlauf der Datenquellen-Synchronisierungsaufträge abzurufen |
| --- | --- |
| ***Dokumente hochladen*** | -------- |
| `BatchPutDocument` | Erteilt die Erlaubnis zum Batch-Put-Dokument |
| `BatchDeleteDocument` | Erteilt die Erlaubnis, ein Dokument stapelweise zu löschen |
| --- | --- |
| ***Chat- und Konversationsmanagement*** | -------- |
| `Chat` | Erteilt die Erlaubnis, mit einer Anwendung zu chatten|
| `ChatSync` | Erteilt die Erlaubnis, mit einer Anwendung synchron zu chatten |
| `DeleteConversation` | Erteilt die Erlaubnis, eine Konversation zu löschen |
| `ListConversations` | Erteilt die Erlaubnis, alle Konversationen für eine Anwendung aufzulisten |
| `listMessages` | Erteilt die Erlaubnis, alle Nachrichten aufzulisten |
| --- | --- |
| ***Benutzer- und Gruppenverwaltung*** | -------- |
| `createUser` | Erteilt die Erlaubnis, einen Benutzer zu erstellen |
| `DeleteUser` | Erteilt die Erlaubnis, einen Benutzer zu löschen |
| `getUser` | Erteilt die Erlaubnis, einen Benutzer zu finden |
| `updateUser` | Erteilt die Erlaubnis, einen Benutzer zu aktualisieren |
| `PutGroup` | Erteilt die Erlaubnis, eine Gruppe von Benutzern zu platzieren |
| `DeleteGroup` | Erteilt die Erlaubnis, eine Gruppe zu löschen |
| `getGroup` | Erteilt die Erlaubnis zum Abrufen einer Gruppe |
| `ListGroups` | Erteilt die Erlaubnis, Gruppen aufzulisten |
| --- | --- |
| ***Plugins*** | -------- |
| `CreatePlugin` | Erteilt die Erlaubnis, ein Plugin für eine bestimmte Anwendung zu erstellen |
| `DeletePlugin` | Erteilt die Erlaubnis, ein Plugin zu löschen |
| `getPlugin` | Erteilt die Erlaubnis, ein Plugin zu bekommen |
| `UpdatePlugin` | Erteilt die Erlaubnis, ein Plugin zu aktualisieren |
| --- | --- |
| ***Admin-Kontrollen und Leitplanken *** | -------- |
| `UpdateChatControlsConfiguration` | Erteilt die Erlaubnis, die Konfiguration der Chat-Steuerelemente für eine Anwendung zu aktualisieren |
| `DeleteChatControlsConfiguration` | Erteilt die Erlaubnis, die Konfiguration der Chat-Steuerelemente für eine Anwendung zu löschen |
| `getChatControlsConfiguration` | Erteilt die Erlaubnis, die Konfiguration der Chat-Steuerelemente für eine Anwendung abzurufen |

### Ereignisse auf Datenebene in CloudTrail

Standardmäßig protokolliert CloudTrail keine Datenereignisse. Im Folgenden werden die Amazon Q API-Operationen gezeigt, die in CloudTrail als Datenereignisse protokolliert wurden. In der Spalte Datenereignistyp (Konsole) wird die entsprechende Auswahl in der CloudTrail-Konsole angezeigt. In der Spalte Amazon Q-Ressourcentypen wird der Wert resources.type angezeigt, den Sie angeben würden, um Datenereignisse für die Ressource zu protokollieren. Weitere Informationen finden Sie im [Q-Benutzerhandbuch] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/logging-using-cloudtrail.html).

**Amazon Q Geschäftsanwendungen**: AWS: :QBusiness: :Anwendung
* Datenquellen-Synchronisierungsaufträge auflisten
* Starten Sie den DataSourceSyncJob
* Stoppt den DataSourceSyncJob
* BatchPut-Dokument
* Dokument stapelweise löschen
* Feedback geben
* ChatSync
* Konversation löschen
* Konversationen auflisten
* Nachrichten auflisten
* Gruppen auflisten
* Gruppe löschen
* Gruppe abrufen
* Gruppe setzen
* Benutzer erstellen
* Benutzer löschen
* Benutzer abrufen
* Benutzer aktualisieren
* Dokumente auflisten

**Amazon Q Business-Datenressource**: AWS: :QBusiness: :DataSource
* DataSourceSync-Jobs auflisten
* Starten Sie den DataSourceSyncJob
* Stoppt den DataSourceSyncJob

**Amazon Q-Geschäftsindex**: AWS: :QBusiness: :Index
* Gruppe löschen
* Gruppe abrufen
* Gruppe setzen
* Gruppen auflisten
* Dokumente auflisten
* BatchPut-Dokument
* Dokument stapelweise löschen

### Analyse

Im Falle eines Vorfalls sind neben der Untersuchung der Indikatoren für Gefährdung, Bedrohungsakteur, Zeitrahmen usw. auch einige weitere Fragen zu berücksichtigen, sobald bestätigt wurde, dass es sich um einen Vorfall im Zusammenhang mit den Ressourcen von Amazon Q handelt:

1. Welche Ressourcen wurden erstellt und gelöscht?
2. Welche PlugIns wurden verwendet?
3. Auf welche Anwendungen hatte Q während des Sicherheitsvorfalls Zugriff?
4. Wurde von Q auf diese Anwendungen zugegriffen?
5. Wurde irgendeine Art von Datei auf Q hochgeladen?
6. Hat Q Zugriff auf eine IDE-Umgebung?

## Eindämmung und Ausrottung

* [IAM-Benutzerschlüssel löschen oder rotieren] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); möglicherweise möchten Sie alle Schlüssel in Ihrem Konto rotieren, wenn Sie einen oder mehrere Schlüssel nicht identifizieren können, die offengelegt wurden
* [Nicht autorisierte IAM-Benutzer löschen] (https://console.aws.amazon.com/iam/home#users.)
* [Nicht autorisierte Richtlinien löschen] (https://console.aws.amazon.com/iam/home#/policies)
* [Nicht autorisierte Rollen löschen] (https://console.aws.amazon.com/iam/home#/roles)
* [Temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden.
* HINWEIS: Das Löschen von IAM-Benutzern kann sich auf die Produktionsauslastung auswirken und sollte mit Vorsicht erfolgen
* [SMTP-Anmeldeinformationen wechseln] (https://aws.amazon.com/premiumsupport/knowledge-center/ses-create-smtp-credentials/)
* [So löschen Sie die Amazon Q-Anwendung] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-app-actions.html)
* [So löschen Sie Amazon Q Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-native-retriever-actions.html)
* [So löschen Sie Amazon Q Kenra Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-kendra-retriever-actions.html)
* [So löschen Sie hochgeladene Dokumente] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/delete-doc-upload.html)
* [So löschen Sie die Amazon Q-Datenquelle] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-datasource-actions.html)
* [So löschen Sie Amazon Web Experience] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-exp-actions.html)

## Wiederherstellung

* Neue IAM-Benutzer mit Richtlinien für den Zugriff mit den geringsten Rechten erstellen


## Datenschutz und IAM-Richtlinien

### Datenschutz:

* Geben Sie NIEMALS vertrauliche oder sensible Informationen wie die E-Mail-Adressen Ihrer Kunden in Tags oder frei formatierte Textfelder wie ein Namensfeld ein. Dies gilt auch, wenn Sie mit Amazon Q oder anderen AWS-Services über die Konsole, API, AWS-CLI oder AWS-SDKs arbeiten. Alle Daten, die Sie in Tags oder freie Textfelder für Namen eingeben, können für Abrechnungs- oder Diagnoseprotokolle verwendet werden.
* Amazon Q speichert Ihre Fragen, deren Antworten und zusätzlichen Kontext, wie Konsolenmetadaten und Code, in Ihrer IDE, um Antworten auf Ihre Fragen zu generieren.
* Amazon Q, Daten werden in eine US-Region gesendet und dort gespeichert. Daten, die während der Konversationen mit Amazon Q gesammelt wurden, werden in der Region USA Ost (Nord-Virginia) gespeichert. Daten, die bei der Behebung von Konsolenfehlern gesammelt wurden, werden in der Region USA West (Oregon) gespeichert.

### IAM-Berechtigungen:

* Die verwaltete Richtlinie `AmazonQFullAccess` bietet vollen Zugriff, um Interaktionen mit Amazon Q zu ermöglichen. Alle Administratoren und Hauptbenutzer haben standardmäßig Zugriff, aber für andere Benutzer und Rollen muss der Kunde die verwaltete AWS-Q-Richtlinie beifügen. Berechtigungen können auf der Grundlage von Amazon Q-Aktionen eingeschränkt werden.
* Um Amazon Q-Fragen in der AWS-Konsole zu stellen, benötigt eine IAM-Identität Berechtigungen für die folgenden Aktionen:
* StartConversation [Beispiel: „Q:StartConversation"]
* Nachricht senden

* Um Konsolenfehler mit Amazon Q zu beheben, benötigt eine IAM-Identität Berechtigungen für die folgenden Aktionen:
* Starten Sie die Analyse zur Fehlerbehebung
* Ergebnisse zur Fehlerbehebung abrufen
* Starten Sie die Analyse zur Fehlerbehebung

* Wenn eine dieser Aktionen in einer beigefügten Richtlinie nicht ausdrücklich zulässig ist, wird beim Versuch, Amazon Q zu verwenden, ein IAM-Berechtigungsfehler zurückgegeben.


## Gelernte Lektionen
`An dieser Stelle können Sie spezifische Punkte für Ihr Unternehmen hinzufügen, die nicht unbedingt „repariert“ werden müssen, aber wichtig zu wissen sind, wenn Sie dieses Playbook zusammen mit den betrieblichen und geschäftlichen Anforderungen umsetzen möchten. `

## Behobene Backlog-Einträge

## Aktuelle Backlog-Einträge