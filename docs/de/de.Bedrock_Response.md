# Sicherheits-Playbook für die Reaktion auf Amazon Bedrock-Sicherheitsereignisse
Dieses Dokument dient nur zu Informationszwecken. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Zeitpunkt der Veröffentlichung dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Kunden sind dafür verantwortlich, die Informationen in diesem Dokument und die Nutzung von AWS-Produkten oder -Services, die jeweils „wie sie sind“ und ohne jegliche ausdrückliche oder stillschweigende Garantie bereitgestellt werden, selbst zu beurteilen. Dieses Dokument stellt keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern dar. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Verträge geregelt, und dieses Dokument ist weder Teil einer Vereinbarung zwischen AWS und seinen Kunden noch ändert es diese.

© 2024 Amazon Web Services, Inc. oder seine Tochtergesellschaften. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## Ansprechpartner

Autor: `Name des Autors`\
Genehmiger: `Name des Genehmigers`\
Letztes Genehmigungsdatum:

## Zusammenfassung

Im Rahmen unseres kontinuierlichen Engagements für Kunden bietet AWS dies
Playbook zur Reaktion auf Sicherheitsvorfälle, in dem die erforderlichen Schritte beschrieben werden
untersuchen Sie Sicherheitsereignisse, bei denen Amazon Bedrock entweder die Quelle oder das Ziel einer unbefugten Nutzung Ihrer AWS-Konten ist. Der Zweck dieses Dokuments besteht darin, verbindliche Hinweise zu den Maßnahmen zu geben, die zu ergreifen sind, wenn Sie den Verdacht haben, dass ein Sicherheitsvorfall stattgefunden hat.

! [Bild] (/images/nist_life_cycle.png)

*Aspekte der AWS-Reaktion auf Vorfall*

## Vorbereitung

Um Maßnahmen zur Reaktion auf Vorfälle schnell und effektiv durchführen zu können, ist es von entscheidender Bedeutung, die Mitarbeiter, Prozesse und Technologien in Ihrem Unternehmen darauf vorzubereiten. Lesen Sie den [*AWS Security Incident Response Guide*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html) und implementieren Sie die erforderlichen Schritte, um sicherzustellen, dass alle drei Domänen darauf vorbereitet sind.

## So wenden Sie sich an den AWS-Support, um Unterstützung zu erhalten

Es ist wichtig, dass Sie AWS benachrichtigen, sobald Sie vermuten, dass Ihre Anmeldeinformationen in Ihrem AWS-Konto oder Ihrer Organisation kompromittiert wurden. Gehen Sie wie folgt vor, um den AWS-Support in Anspruch zu nehmen:

### Einen AWS-Supportfall öffnen

- Melden Sie sich bei Ihrem AWS-Konto an:
- Dies ist das erste AWS-Konto, das von dem Sicherheitsereignis betroffen war, um die Inhaberschaft eines AWS-Kontos zu bestätigen.
- Hinweis: Wenn Sie nicht auf das Konto zugreifen können, verwenden Sie [dieses Formular] (https://support.aws.amazon.com/#/contacts/aws-account-support/), um eine Support-Anfrage einzureichen.
- Wählen Sie „*Services*“, „*Support Center*“, „*Fall erstellen*“ aus.
- Wählen Sie den Problemtyp „*Konto und Abrechnung“ *.
- Wählen Sie den betroffenen Service und die Kategorie aus:
- Beispiel:
- Service: Konto
- Kategorie: Sicherheit
- Wählen Sie einen Schweregrad:
- Enterprise Support- oder On-Ramp-Kunden: *Frage zu kritischem Geschäftsrisiko*.
- Business-Support-Kunden: *Dringende Frage zum Geschäftsrisiko*.
- Beschreiben Sie Ihre Frage oder Ihr Problem:
- Beschreiben Sie ausführlich das aufgetretene Sicherheitsproblem, die betroffenen Ressourcen und die Auswirkungen auf Ihr Unternehmen.
— Zeitpunkt, zu dem das Sicherheitsereignis zum ersten Mal erkannt wurde.
— Zusammenfassung des bisherigen Ablaufs des Ereignisses.
- Artefakte, die du gesammelt hast (Screenshots, Logdateien).
— ARN von IAM-Benutzern oder -Rollen, von denen Sie vermuten, dass sie gefährdet sind.
- Beschreibung der betroffenen Ressourcen und der Auswirkungen auf das Geschäft.
- Grad des Engagements in Ihrem Unternehmen (z. B.: „Dieses Sicherheitsereignis ist für den CEO und den Vorstand sichtbar“).
- Optional: Fügen Sie weitere Ansprechpartner für den Fall hinzu.
- Wählen Sie „*Kontaktieren Sie uns*“.
- Bevorzugte Kontaktsprache (möglicherweise je nach Verfügbarkeit)
- Bevorzugte Kontaktmethode: Internet, Telefon oder Chat (empfohlen)
- Optional: Zusätzliche Ansprechpartner für den Fall
- *Klicken Sie auf „Senden“ *

- **Eskalationen**: Bitte benachrichtigen Sie Ihr AWS-Kundenbetreuungsteam so schnell wie möglich, damit es die erforderlichen Ressourcen einsetzen und bei Bedarf eskalieren kann.

**Hinweis: ** Es ist sehr wichtig, dass Sie überprüfen, ob Ihr „Alternativer Sicherheitskontakt“ für jedes AWS-Konto definiert ist. Weitere Informationen finden Sie unter [diesem
Artikel] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).


## Amazon Bedrock

Amazon Bedrock ist durch mehrere Komponenten/Dienste beeinträchtigt:

* Modelle — können entweder Basismodelle von führenden KI-Startups oder kundenspezifische Modelle sein
* Inferenz — der Prozess der Generierung eines Outputs aus einer Eingabe, die für ein Modell bereitgestellt wird
* Knowledge Base — ermöglicht es Ihnen, Datenquellen in einem Informationsspeicher zusammenzustellen
* Agenten — helfen Ihren Endbenutzern, Aktionen auf der Grundlage von Unternehmensdaten und Benutzereingaben durchzuführen

Wenn ein Benutzer mit einem AWS-Konto (entweder autorisiert oder nicht autorisiert) auf Bedrock zugreift, geschieht dies letztendlich entweder über ein Model, eine Knowledge Base oder einen Agenten. Alle ausgegebenen Eingabeaufforderungen und ihre Antworten werden in einem Model Invocation Log ausgegeben, sofern dies zuvor eingerichtet wurde (https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html). Weitere Informationen finden Sie im Benutzerhandbuch für Amazon Bedrock hier: https://docs.aws.amazon.com/bedrock/

Das Bild unten zeigt die Beziehung zwischen Prompts, [Agents] (https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html) und Knowledge Bases und kann eine gute Möglichkeit sein, den Datenfluss zu visualisieren (entnommen aus dem [User Guide] (https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html))

! [Agentenaufbau während Build-Time-API-Vorgängen] (/images/bedrock-01.png)


## Erkennung

### Hinweise und Überlegungen

* Bedrock ist nicht in allen Regionen verfügbar (04.06.2020). Informationen zur aktuellen Verfügbarkeit in der Region finden Sie unter [Link] (https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-regions.html)
* Die Protokollierung von Modellaufrufen ist erforderlich, um die an Modelle gesendeten Aufforderungen und Antworten anzuzeigen. Dies ist eine Einstellung auf Regionsebene und nicht die Standardeinstellung
* Für CloudTrail ist EventSource = bedrock.amazonaws.com
* Die ***invokeModel*** API ist ein**schreibgeschützt** Ereignis

### Namen der Ereignisse

Die in der folgenden Tabelle aufgeführten Ereignisnamen dienen als Kurzreferenz für Ereignistypen, die in der Regel in CloudTrail bei der Untersuchung eines Sicherheitsereignisses, an dem Bedrock beteiligt ist, auftreten. Von den in der Tabelle aufgelisteten Ereignissen sind die häufigsten Ereignisnamen, die bei normaler Modellnutzung protokolliert werden, die folgenden:

* `getFoundationModelAvailability`
* `Modell aufrufen`
* `InvokeModelWithResponseStream`

| **Namen der Veranstaltungen** | **ReadOnly** | **Beschreibung** |
|: -----------------|: -----------------|: -------------------------------- |
| ***Ereignisse im Zusammenhang mit Modelsen*** | --- | --- |
| `getFoundationModelAvailability` | TRUE | Ruft die Verfügbarkeit eines Foundation-Modells ab |
| `getUseCaseForModelAccess` | TRUE | Ruft einen Anwendungsfall für ein bestimmtes Modell ab |
| `invokeModel` | TRUE | Ruft das angegebene Bedrock-Modell auf, um mithilfe der im Anfragetext bereitgestellten Eingabe eine Inferenz auszuführen |
| `invokeModelWithResponseStream` | TRUE | Ruft das angegebene Bedrock-Modell auf, um die Inferenz unter Verwendung der im Anfragetext bereitgestellten Eingabe mithilfe von Chunking auszuführen |
| `ListCustomModels` | TRUE | Listet benutzerdefinierte Bedrock-Modelle auf, die erstellt wurden |
| `ListFoundationModels` | TRUE | Listet die Foundation-Modelle auf, die verwendet werden können |
| `ListProvisionedModelThroughputs` | TRUE | Listet die bereitgestellten Kapazitäten für Modelle auf |
| --- | --- | --- |
| ***Ereignisse im Zusammenhang mit der Protokollierung von Modellaufrufen*** | --- | --- |
| `DeleteModelInvocationLoggingConfiguration` | FALSE | Löscht eine bestehende Konfiguration für die Aufrufprotokollierung |
| `GetModelInvocationLoggingConfiguration` | TRUE | Ruft die bestehende Konfiguration der Aufrufprotokollierung ab |
| `PutModelInvocationLoggingConfiguration` | FALSE | Erstellt eine bestehende Konfiguration für die Aufrufprotokollierung |
| --- | --- | --- |
| ***Veranstaltungen im Zusammenhang mit Wissensdatenbanken*** | --- | --- |
| `AssociateAgentKnowledgeBase` | FALSE | Diese Aktion protokolliert die Verknüpfung einer Wissensdatenbank während der Agentenerstellung oder nach der Erstellung eines Agenten |
| `CreateDataSource` | FALSE | Erstellt eine Datenquelle |
| `createKnowledgeBase` | FALSE | Erstellt eine Wissensdatenbank |
| `DeleteKnowledgeBase` | FALSE | Löscht eine Wissensdatenbank |
| `GetDataSource` | TRUE | Ruft Informationen über eine Datenquelle ab |
| `getKnowledgeBase` | TRUE | Ruft eine bestehende Wissensdatenbank ab |
| `ListDataSources` | TRUE | Listet verfügbare Datenquellen auf |
| `ListKnowledgeBases` | TRUE | Listet bestehende Wissensdatenbanken auf |
| --- | --- | --- |
| ***Ereignisse im Zusammenhang mit Agenten*** | --- | --- |
| `CreateAgent` | FALSE | Erzeugt einen neuen Agenten und einen Test-Agent-Alias, der auf die Version des DRAFT-Agenten verweist |
| `CreateAgentActionGroup` | FALSE | Erstellt eine Aktionsgruppe für einen Agenten. Eine Aktionsgruppe stellt die Aktionen dar, die ein Agent für den Kunden ausführen kann, indem sie die APIs, die ein Agent aufrufen kann, und die Logik für deren Aufruf definiert.
| `CreateAgentAlias` | FALSE | Erzeugt einen neuen Alias für einen Agenten |
| `DeleteAgent` | FALSE | Löscht einen zuvor erstellten Agenten |
| `getAgent` | TRUE | Ruft einen vorhandenen Agenten ab |
| `getAgentAlias` | TRUE | Ruft einen vorhandenen Alias ab |
| `invokeAgent` | FALSE | Sendet eine Aufforderung, die der Agent verarbeiten und beantworten soll |
| `ListAgentActionGroups` | TRUE | Listet die Aktionsgruppen für einen Agenten und Informationen zu jeder einzelnen auf |
| `ListAgentAliases` | TRUE | Listet die Aliase eines Agenten und Informationen zu jedem einzelnen auf |
| `ListAgentKnowledgeBases` | TRUE | Listet Wissensdatenbanken auf, die einem Agenten zugeordnet sind, sowie Informationen zu jedem einzelnen |
| `ListAgents` | TRUE | Listet bestehende Agenten auf |
| `ListAgentVersions` | TRUE | Listet die Versionen eines Agenten und Informationen zu jeder Version auf |
| `PrepareAgent` | FALSE | Bereitet einen vorhandenen Amazon Bedrock Agent auf den Empfang von Runtime-Anfragen vor |
| --- | --- | --- |
| ***Veranstaltungen im Zusammenhang mit Job Ingestion*** | --- | --- |
| `GetIngestionJob` | TRUE | Ruft Informationen über einen Ingestion-Job ab, bei dem eine Datenquelle zu einer Wissensdatenbank hinzugefügt wird |
| `ListingestionJobs` | TRUE | Listet die Aufnahme-Jobs für eine Datenquelle und Informationen zu jedem von ihnen auf |
| `StartingestionJob` | FALSE | Startet einen Aufnahmejob, bei dem eine Datenquelle zu einer Wissensdatenbank hinzugefügt wird |
| --- | --- | --- |
| ***Veranstaltungen im Zusammenhang mit RAG*** | --- | --- |
| `Retrieve` | TRUE | Ruft aufgenommene Daten aus einer Wissensdatenbank ab (wird als Datenereignis betrachtet und nicht in CloudTrail angezeigt) |
| `RetrieveAndGenerate` | FALSE | Sendet Benutzereingaben zum Abrufen und Generieren (wird als Datenereignisse betrachtet und nicht in CloudTrail angezeigt) |

### Screenshots zur Nutzung von Bedrock

Die folgenden Screenshots können einem Incident Responder als visuelle Hilfe bei der Interpretation von Ereignissen dienen, die während einer Untersuchung festgestellt wurden. Jedes Bild unten stellt die ausgeführten Aktionen dar, die mit dem protokollierten Ereignisnamen übereinstimmen

---

**Verfügbarkeit des Fundamentmodells abrufen**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Dieses Ereignis wird zuerst protokolliert, wenn Sie die Amazon Bedrock-Hauptseite aufrufen

! [Verfügbarkeit von FoundationModel ermitteln] (/images/bedrock-02.png)

</details>

---

**ListFoundationModels** und **ListCustomModels**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Diese Ereignisse werden protokolliert, wenn Sie die Seiten „Basismodelle“ und „Benutzerdefinierte Modelle“ aufrufen

! [ListFoundationModels und ListCustomModels] (/images/bedrock-03.png)

</details>

---

**Modell aufrufen**
<details>
<summary>Screenshots erweitern</summary>
-
Dieses Ereignis wird beim Modellaufruf protokolliert.

! [Beispiel für einen Modellaufruf] (/images/bedrock-04.png)

Beispiel für einen CloudTrail-Ereignisdatensatz für den Modellaufruf:

! [CloudTrail für invokeModel] (/images/bedrock-05.png)

</details>

---

**invokeModel** - weitere Beispiele
<details>
<summary>Screenshots erweitern</summary>
-
Die folgenden Bilder enthalten weitere Beispiele dafür, wie der Name des `invokeModel`-Ereignisses protokolliert wird.
Das erste Bild zeigt die Unterschiede zwischen den verschiedenen Methoden des Modellaufrufs:
(a) Modellaufruf mithilfe einer Wissensdatenbank
(b) Direkter Modellaufruf
(c) Modellaufruf mit einem Agenten

Beachten Sie, dass der angezeigte Benutzername* je nachdem, wie das Modell aufgerufen wird, unterschiedlich ist

! [Aufruf des Agenten- und Knowledge Base-Modells] (/images/bedrock-06.png)

Dieser Unterschied wird anhand von zwei nebeneinander liegenden Screenshots des `invokeModel`-Ereignisdatensatzes in CloudTrail verdeutlicht.
Das linke Bild = Direkter Modellaufruf
Das rechte Bild = Modellaufruf mithilfe einer Knowledge Base

! [CloudTrail für invokeModel] (/images/bedrock-07.png)


</details>

---

**invokeModel** oder **invokeModelWithResponseStream** — Eingabeaufforderungen und Antwort
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Während die Ereignisse `invokeModel` und `invokeModelWithResponseStream` in CloudTrail angezeigt werden, zeigen Modellaufrufprotokolle (sofern konfiguriert) auch die Aufforderung und Antwort an, die jedem `invokeModel`-Ereignis entsprechen. Das Bild unten stammt aus Modellaufrufprotokollen, die an S3 gesendet wurden. Diese enthalten die verwendete Eingabeaufforderung sowie die Ausgabe oder die Antwort des Modells (eine Athena-DDL ist unten verfügbar):

! [Beispiel für einen S3-Modellaufruf] (/images/bedrock-09.png)

</details>

---

**Konfiguration der Modellaufruf-Protokollierung eingeben**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Dieses Ereignis wird protokolliert, wenn die Protokollierung von Modellaufrufen in einem AWS-Konto konfiguriert ist.

! [Konfiguration der Modellaufrufprotokollierung] (/images/bedrock-10.png)

</details>

---

**Wissensdatenbank erstellen**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Der folgende CloudTrail-Ereignisdatensatz wird protokolliert, wenn eine Wissensdatenbank erstellt wird. Der Ereignisdatensatz enthält die Identität, die die Anfrage gestellt hat, und den Namen der Knowledge Base. Auf den Namen der Wissensdatenbank kann in nachfolgenden Protokollen verwiesen werden, in denen Modellaufrufe mithilfe von `invokeModel` erfasst werden

! [Erstellung einer Wissensdatenbank] (/images/bedrock-11.png)

</details>

---

**Agent erstellen**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Das Bild unten zeigt die Erstellung eines Agenten mithilfe der Konsole. Der resultierende Ereignisname, der protokolliert wird, lautet `createAgent`

! [Agentenerstellung] (/images/bedrock-12.png)

</details>

---

**Retrieve** und **RetrieveAndGenerate**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Ein Beispiel dafür, wie die Namen der Ereignisse `Retrieve` und `RetrieveAndGenerate` beim Testen einer Wissensdatenbank in der Abbildung unten dargestellt werden. Beachten Sie, dass `Retrieve` nur das Abrufen von Daten aus der Wissensdatenbank testet und `RetrieveAndGenerate` das Abrufen von Daten aus der Wissensdatenbank und das Generieren einer Antwort testet:

! [Abrufen und RetrieveAndGenerate] (/images/bedrock-13.png)

</details>

---

### Athena DDL

Verwenden Sie den folgenden DDL-Code, um eine Tabelle in Athena aus den in einem Amazon S3 S3-Bucket gespeicherten Modellaufrufprotokollen zu erstellen. Stellen Sie sicher, dass der richtige S3-URI für die Bucket-Position am Ende des Codeausschnitts verwendet wird:

```
EXTERNE TABELLE ERSTELLEN bedrock_model_invocation_metadata_unfiltered (
SchemaType-Zeichenfolge,
SchemaVersion-Zeichenfolge,
Zeitstempel-Zeichenfolge,
AccountID-Zeichenfolge,
<arn: string>Identitätsstruktur,
Zeichenfolge für die Region,
RequestID-Zeichenfolge,
Operationszeichenfolge,
ModelID-Zeichenfolge,
ErrorCode-Zeichenfolge,
Eingabestruktur <inputBodyJson: Zeichenfolge,
inputBodys3Path: Zeichenfolge,
inputContentType: Zeichenfolge,
Anzahl der Eingaben: int>,
Ausgabestruktur <outputBodyJson: Zeichenfolge,
outputBodys3Path: Zeichenfolge,
OutputContentType: Zeichenfolge,
OutputTokenCount: int,
OutputImageBucketedStepSize: int,
OutputImageHeight: int,
OutputImageWidth: int
>
)
ZEILENFORMAT SERDE 'org.openx.data.jsonSerde.jsonSerde'
<insert_prefix>STANDORT 's3<insert_S3_bucket>:////'
```

## Behobene Backlog-Einträge
- Als Incident Responder muss ich in der Lage sein, alle kritischen Bedrock-Ereignisse zu überwachen
- Als Incident Responder benötige ich ein Playbook zur Abfrage von Bedrock Cloudtrail-Ereignissen in großem Maßstab

## Aktuelle Backlog-Einträge