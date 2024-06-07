# Incident Response Playbook: Reaktion auf Sicherheitsereignisse von SageMaker
Dieses Dokument dient nur zu Informationszwecken. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Zeitpunkt der Veröffentlichung dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Kunden sind dafür verantwortlich, die Informationen in diesem Dokument und die Nutzung von AWS-Produkten oder -Services, die jeweils „wie sie sind“ und ohne jegliche ausdrückliche oder stillschweigende Garantie bereitgestellt werden, selbst zu beurteilen. Dieses Dokument stellt keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern dar. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Verträge geregelt, und dieses Dokument ist weder Teil einer Vereinbarung zwischen AWS und seinen Kunden noch ändert es diese.

© 2024 Amazon Web Services, Inc. oder seine Tochtergesellschaften. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.


## Ansprechpartner

Autor: `Name des Autors`\
Genehmiger: `Name des Genehmigers`\
Letztes Genehmigungsdatum:

## Zusammenfassung
Als Teil unseres kontinuierlichen Engagements für Kunden stellt AWS dieses Playbook zur Reaktion auf Sicherheitsvorfälle zur Verfügung, in dem die Schritte beschrieben werden, die zur Untersuchung von Sicherheitsereignissen erforderlich sind, bei denen Amazon SageMaker entweder die Quelle oder das Ziel einer unbefugten Nutzung Ihrer AWS-Konten ist. Der Zweck dieses Dokuments besteht darin, verbindliche Hinweise zu den Maßnahmen zu geben, die zu ergreifen sind, wenn Sie den Verdacht haben, dass ein Sicherheitsvorfall stattgefunden hat.

! [Bild] (sagemaker_images/nist_life_cycle.png)

*Aspekte der AWS-Reaktion auf Vorfall*


### Amazon SageMaker

Amazon SageMaker ist ein vollständig verwalteter Service für maschinelles Lernen (ML). Mit SageMaker können Datenwissenschaftler und Entwickler schnell und zuverlässig ML-Modelle erstellen, trainieren und in einer produktionsbereiten, gehosteten Umgebung einsetzen. Es bietet eine Benutzeroberfläche für die Ausführung von ML-Workflows, sodass SageMaker-ML-Tools in mehreren integrierten Entwicklungsumgebungen (IDEs) verfügbar sind.

Mit SageMaker können Sie Ihre Daten speichern und teilen, ohne Ihre eigenen Server erstellen und verwalten zu müssen. Mit integrierter Unterstützung für Bring-Your-Own-Algorithmen und Frameworks bietet SageMaker flexible, verteilte Trainingsoptionen, die sich an spezifische Arbeitsabläufe anpassen lassen. Weitere Informationen finden Sie im Entwicklerhandbuch für Amazon SageMaker hier: https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html



## Vorbereitung
Bereiten Sie Ihre Umgebung proaktiv vor, indem Sie präventive ([Service Control Policies] (https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)) und detektive Kontrollen implementieren (Konfigurationsregeln finden Sie im Abschnitt Erkennung).


**Präventiv (SCP) **

VPC FlowLogs
* Der folgende SCP verhindert beispielsweise, dass Benutzer Notebooks, Schulungen oder Verarbeitungsaufträge starten, sofern keine VPC angegeben ist.

```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „VPC-Bereitstellung“,
„Effekt“: „Ablehnen“,
„Aktion“: [
„SageMaker: Hyperparameter-Tuning-Job erstellen“,
„SageMaker:CreateModel“,
„SageMaker:Notebook-Instanz erstellen“,
„SageMaker: Verarbeitungsauftrag erstellen“,
„SageMaker: Trainingsjob erstellen“
],
„Ressource“: [
„*“
],
„Zustand“: {
„Null“: {
„SageMaker:vpcSecurityGroupIds“: „wahr“,
„SageMaker:VPCSubnets“: „wahr“
}
}
}
]
}
```


* Jobverschlüsselung erzwingen

```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „Unverschlüsselte Volumes ablehnen“,
„Effekt“: „Ablehnen“,
„Aktion“: [
„SageMaker: Hyperparameter-Tuning-Job erstellen“,
„SageMaker: Trainingsjob erstellen“,
„SageMaker:CreateEndPointConfig“,
„SageMaker:CreateTransformJob“
],
„Ressource“: [
„*“
],
„Zustand“: {
„Null“: {
„SageMaker:VolumeKMSKey“: [
„wahr“
]
}
}
}
]
}
```
* Erzwingen Sie die Verschlüsselung des Datenverkehrs zwischen Containern
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „Unverschlüsselten Verkehr ablehnen“,
„Effekt“: „Ablehnen“,
„Aktion“: [
„SageMaker: Trainingsjob erstellen“,
„SageMaker: Hyperparameter-Tuning-Job erstellen“
],
„Ressource“: [
„*“
],
„Zustand“: {
„Bool“: {
„SageMaker:InterContainerTrafficEncryption“: „falsch“
}
}
}
]
}
```

* Netzwerkisolierung erzwingen
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „DenyNoIsoliert“,
„Effekt“: „Ablehnen“,
„Aktion“: [
„SageMaker: Trainingsjob erstellen“,
„SageMaker: Hyperparameter-Tuning-Job erstellen“,
„SageMaker:Modell erstellen“
],
„Ressource“: „*“,
„Zustand“: {
„Bool“: {
„SageMaker:NetworkIsolation“: „falsch“
}
}
}
]
}
```
* Beschränkung der vorsignierten Notebook-URL auf IP-Adressen
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „URL auf IP beschränken“,
„Effekt“: „Ablehnen“,
„Action“: „SageMaker:CreatePresignedNotebookInstanceUrl“,
„Ressource“: „*“,
„Zustand“: {
„Für alle Werte: keine IP-Adresse“: {
„aws:Quell-IP“: [
„[GEBEN SIE DIE ÖFFENTLICHE IP-ADRESSE EIN]“
]
}
}
}
]
}
```
* Internetzugang deaktivieren
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „Direktes Internet ablehnen“,
„Effekt“: „Ablehnen“,
„Action“: „SageMaker:CreateNotebookInstance“,
„Ressource“: „*“,
„Zustand“: {
„Zeichenfolge entspricht“: {
„SageMaker:Direkter Internetzugang“: [
„Aktiviert“
]
}
}
}
]
}
```

* Deaktivieren Sie den Root-Zugriff in SageMaker-Notebooks
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „SageMaker verweigert Root-Zugriff“,
„Effekt“: „Ablehnen“,
„Aktion“: [
„SageMaker: Notebook-Instanz erstellen“,
„SageMaker:NotebookInstance aktualisieren“
],
„Ressource“: „*“,
„Zustand“: {
„Zeichenfolge entspricht“: {
„SageMaker:RootAccess“: [
„Aktiviert“
]
}
}
}
]
}
```
* Beschränken Sie die Instanztypen, die von Benutzern gestartet werden können
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „SageMakerLimitInstanztypen“,
„Effekt“: „Ablehnen“,
„Action“: „SageMaker:CreateNotebookInstance“,
„Ressource“: „*“,
„Zustand“: {
„ForAnyValue:StringNotLike“: {
„SageMaker:Instanztypen“: [
„[BEISPIELE_INSTANZTYPEN]“,
„ml.c5.xlarge“,
„ml.m5.xlarge“,
„ml.t3.medium“
]
}
}
}
]
}
```

* In ähnlicher Weise finden Sie für Studio die folgende Beispielrichtlinie. Beachten Sie, dass Administratoren die Systeminstanz für die standardmäßigen Jupyter Server-Apps zulassen müssen.
```
{
„Version“: „2012-10-17",
„Aussage“: [
{
„Sid“: „SageMaker hat Instanztypen zugelassen“,
„Effekt“: „Ablehnen“,
„Aktion“: [
„SageMaker:CreateApp“
],
„Ressource“: „*“,
„Zustand“: {
„ForAnyValue:StringNotLike“: {
„SageMaker:Instanztypen“: [
„ml.c5.large“,
„ml.m5.large“,
„ml.t3.medium“,
„System“
]
}
}
}
]
}
```


## Erkennung

### SageMaker-Prüfungen

### AWS Config

AWS Config hat mehrere [verwaltete Regeln zur Evaluierung von SageMaker] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* sagemaker-endpoint-configuration-kms-key-konfiguriert
* Anzahl der Sagemaker-Endpoint-Config-Prod-Instanzen
* Sagemaker-Notebook-Instanz innerhalb von VPC
* Sagemaker-Notebook-Instance-KMS-Schlüssel konfiguriert
VPC
* Sagemaker-Notebook-kein direkter Internetzugang

### CloudTrail-Ereignisse

Im Falle eines unbefugten Zugriffs auf Ihre Umgebung finden Sie im Folgenden mögliche Szenarien im Zusammenhang mit relevanten SageMaker-API-Aufrufen als Kurzreferenz (bitte beachten Sie, dass dies keine vollständige Liste der SageMaker-API-Aufrufe ist):


**Exfiltration von Daten/Modellen: **
- Kopieren oder Herunterladen sensibler Daten aus SageMaker-Datenspeichern oder Modellartefakten.
- Extrahieren von Modellparametern oder Trainingsdaten, wodurch möglicherweise geistiges Eigentum oder persönliche Informationen offengelegt werden.

<ins>Beispiele für API-Aufrufe:</ins>
- `DescribeModelPackage`, um Informationen über Modellpakete abzurufen.
- `DescribeTrainingJob`, um auf Details von Trainingsjobs und deren Ausgabedaten zuzugreifen.
- `getModelPackageModelMetrics` zum Abrufen von Modellmetriken und potenziell sensiblen Daten.



**Datenvergiftung: **
* Modifizierung oder Vergiftung trainierter Modelle durch Einschleusung bösartiger Daten oder gegnerischer Beispiele
* Bereitstellung kompromittierter Modelle auf SageMaker-Endpunkten, was zu falschen Prognosen oder bösartigen Ergebnissen führt.

<ins>Beispiele für API-Aufrufe:</ins>
- `createModelPackage` oder `updateModelPackage`, um ein kompromittiertes Modellpaket bereitzustellen.
- `createTransformJob`, um einen Transformationsjob mit einem fehlerhaften Modell auszuführen.
- `CreateEndPointConfig` und `CreateEndPoint`, um ein bösartiges Modell auf einem Endpunkt bereitzustellen.
- `CreateTrainingJob` oder `UpdateTrainingJob`, um bösartige Daten in Trainingsjobs einzuschleusen.



**Missbrauch von Ressourcen: **
* Starten nicht autorisierter SageMaker-Notebooks oder -Instances für Cryptomining oder andere bösartige Aktivitäten.
* Verwendung von SageMaker-Ressourcen als Einstiegspunkte oder Dreh- und Angelpunkte für laterale Bewegungen innerhalb der AWS-Umgebung.

<ins>Beispiele für API-Aufrufe:</ins>
- `createNotebookInstance` oder `updateNotebookInstance`, um nicht autorisierte Notebook-Instanzen zu starten.
- `CreateTrainingJob` oder `CreateHyperParameterTuningJob`, um zu viele Trainingsjobs zu initiieren.
- `createEndPoint` oder `updateEndPoint` zum Erstellen oder Ändern von Endpunkten für nicht autorisierte Zwecke.

**Denial of Service (DoS) :**
* Erschöpfung der SageMaker-Ressourcen (z. B. Recheninstanzen, Speicher) durch das Starten übermäßiger Trainingsjobs oder Endpoints.
* Überwältigende SageMaker-APIs oder -Services mit einer hohen Anzahl von Anfragen, was zu Serviceunterbrechungen führt.

<ins>Beispiele für API-Aufrufe:</ins>
- `CreateTrainingJob` oder `CreateHyperParameterTuningJob`, um zahlreiche Trainingsjobs zu starten und Ressourcen zu erschöpfen.
- `createEndPoint` oder `updateEndPoint`, um mehrere Endpunkte zu erstellen und übermäßige Rechenressourcen zu verbrauchen.

**Konfigurationsänderungen: **
* Änderung der Rollen, Richtlinien oder Berechtigungen von SageMaker, um Rechte zu erweitern oder unbefugten Zugriff zu gewähren.
* Änderung der VPC-Konfigurationen, Sicherheitsgruppen oder Netzwerkeinstellungen von SageMaker zur Umgehung der Sicherheitskontrollen.

<ins>Beispiele für API-Aufrufe:</ins>
- `createRole` oder `updateRole`, um die Rollen und Berechtigungen von SageMaker zu ändern.
- `createNotebookInstanceLifecycleConfig` oder `updateNotebookInstanceLifecycleConfig`, um die Instanzkonfigurationen von Notebooks zu ändern.
- `createEndpointConfig` oder `updateEndpointConfig`, um Endpunktkonfigurationen oder Sicherheitseinstellungen zu ändern.

**Manipulation von Protokollen: **
* Ändern oder Löschen von SageMaker-Protokollen oder Prüfprotokollen, um Spuren zu verwischen und die Untersuchung von Vorfällen zu behindern.
* Einfügen falscher Protokolleinträge, um Sicherheitsanalysten in die Irre zu führen oder böswillige Aktivitäten zu verbergen.

<ins>Beispiele für API-Aufrufe:</ins>
- `PutModelPackageModelMetrics`, um falsche Modellmetriken in Logs einzufügen.
- `StopTrainingJob` oder `StopTransformJob`, um Protokolldaten potenziell zu ändern oder zu löschen.

**Bereitstellung von Schadsoftware: **
* Bereitstellung von Malware oder Backdoors in SageMaker-Notebooks oder -Instances für dauerhaften Zugriff oder Datendiebstahl.
* Nutzung von SageMaker-Ressourcen zur Verbreitung von Malware oder zum Starten von Angriffen auf andere Systeme oder Netzwerke.

<ins>Beispiele für API-Aufrufe:</ins>
- `createNotebookInstance` oder `updateNotebookInstance`, um Notebook-Instanzen mit Malware zu starten.
- `createModelPackage` oder `updateModelPackage` zur Bereitstellung von Modellpaketen, die bösartigen Code enthalten.

**Diebstahl von Zugangsdaten: **
* Diebstahl von AWS-Anmeldeinformationen oder SageMaker-API-Schlüsseln, die in Notebooks oder Instances gespeichert sind.
* Verwendung gestohlener Anmeldeinformationen, um weiteren unbefugten Zugriff auf andere AWS-Ressourcen oder -Services zu erlangen.

<ins>Beispiele für API-Aufrufe:</ins>
- `DescribeNotebookInstance` oder `DescribeTrainingJob`, um möglicherweise auf gespeicherte Anmeldeinformationen oder API-Schlüssel zuzugreifen.
- `getModelPackageModelMetrics` oder `DescribeModelPackage`, um vertrauliche Informationen oder Anmeldeinformationen abzurufen.

**Kryptojacking: **
* Entführung von SageMaker-Rechenressourcen (z. B. Instanzen, Endpunkte) für nicht autorisierte Cryptocurrency-Mining-Aktivitäten.
* Übermäßiger Verbrauch von Rechenressourcen, was möglicherweise zu Betriebsunterbrechungen oder erhöhten Kosten führt.

<ins>Beispiele für API-Aufrufe:</ins>
- `createNotebookInstance` oder `updateNotebookInstance`, um Instanzen für das Cryptocurrency Mining zu starten.
- `CreateTrainingJob` oder `CreateHyperParameterTuningJob`, um rechenintensive Jobs für Mining-Zwecke zu initiieren.


**Hinweis: Es ist wichtig zu beachten, dass diese API-Aufrufe auch für legitime Zwecke verwendet werden können. Im Zusammenhang mit unbefugtem Zugriff können sie jedoch für riskante Aktionen missbraucht werden. Die Implementierung robuster Zugriffskontroll-, Überwachungs- und Prüfmechanismen ist entscheidend, um einen solchen Missbrauch von SageMaker-APIs und -Ressourcen zu erkennen und zu verhindern. **

### SageMaker-Protokolleinträge verstehen

Die folgenden Screenshots dienen dem Incident Responder als visuelle Hilfe bei der Interpretation von Ereignissen, die während einer Untersuchung festgestellt wurden. Jedes Bild unten stellt die ergriffenen Maßnahmen dar, die mit dem protokollierten Ereignisnamen übereinstimmen

---

**Domain erstellen**
<details>
<summary>Bildschirmfoto erweitern</summary>
-
Erstellt eine Domain. Eine Domain besteht aus einem zugehörigen Amazon Elastic File System-Volume, einer Liste autorisierter Benutzer und einer Vielzahl von Sicherheits-, Anwendungs-, Richtlinien- und Amazon Virtual Private Cloud (VPC) -Konfigurationen. Benutzer innerhalb einer Domain können Notebook-Dateien und andere Artefakte miteinander teilen.

! [Domain erstellen] (/sagemaker_images/sagemaker-01.png)
</details>

---

**Informationen zur SageMaker-Domain**
<details>
<summary>Bildschirmfoto erweitern</summary>

Die Amazon SageMaker-Domain unterstützt SageMaker-Umgebungen für maschinelles Lernen (ML). Eine SageMaker-Domain besteht aus den folgenden Entitäten: Domain, Benutzerprofil, Shared Space, App


! [Einzelheiten zur Domäne] (/sagemaker_images/sagemaker-02.png)
! [Domänendetails 2] (/sagemaker_images/sagemaker-03.png)

</details>

---


**CloudTrail-Ereignis für CreateDomain**

<details>
<summary>Bildschirmfoto erweitern</summary>

Beachten Sie, dass das Ereignis `createDomain` in Cloudtrail alle folgenden Informationen enthält: VPC, Subnetze, Ausführungsrolle, Apps usw.

! [SageMaker-Domain mit zugehöriger VPC, Ausführungsrolle, Apps usw.] (/sagemaker_images/sagemaker-04.png)


</details>

---

**CloudTrail-Ereignis für CreateEndPoint**

<details>
<summary>Screenshots erweitern</summary>

Beachten Sie, dass das `createEndPoint`-Ereignis für SageMaker in CloudTrail von der Dienstrolle `SageMaker-ExecutionRole` aufgerufen wird

! [Beispiel für einen Endpunkt erstellen] (/sagemaker_images/sagemaker-05.png)

</details>

---



## Analyse

Im Fall eines Vorfalls sollten neben der Untersuchung der Bedrohungsindikatoren, des Bedrohungsakteurs, des Zeitrahmens usw. auch einige weitere Fragen berücksichtigt werden, sobald bestätigt wurde, dass es sich um einen Vorfall handelt, der die Ressourcen von SageMaker betrifft:

1. Auf welche SageMaker-Ressourcen wurde ohne Autorisierung zugegriffen? (z. B. Notebooks, Modelle, Endgeräte, Datenspeicher)
2. Wie wurde der unbefugte Zugriff erlangt? (z. B. kompromittierte Anmeldeinformationen, falsch konfigurierte Berechtigungen, ausgenutzte Sicherheitslücken)
3. Welche Aktionen wurden während des unbefugten Zugriffs auf die betroffenen SageMaker-Ressourcen ausgeführt?
4. Wurden Modelle oder Daten exfiltriert oder manipuliert?
5. Wenn auf Modelle zugegriffen wurde, besteht die Gefahr einer Modellvergiftung oder gegnerischer Angriffe?
6. Wurden während des unbefugten Zugriffs neue Ressourcen (z. B. Notebooks, Endgeräte) erstellt oder geändert?
7. Wurden während des unbefugten Zugriffs SageMaker-APIs oder -SDKs verwendet, und welche Aktionen wurden über sie ausgeführt?
8. Wurden SageMaker-Protokolle oder Prüfprotokolle geändert oder gelöscht, um Spuren zu verwischen?
9. Wurden während des Vorfalls Rollen oder IAM-Richtlinien von SageMaker geändert oder missbraucht?
10. Wurden SageMaker VPC-Konfigurationen oder Netzwerkeinstellungen geändert?
11. Wurden SageMaker-Notebooks oder -Instances als Eintritts- oder Drehpunkte für weiteren unbefugten Zugriff verwendet?
12. Wurden SageMaker-Ressourcen verwendet, um Angriffe oder böswillige Aktivitäten gegen andere AWS-Ressourcen oder externe Systeme zu starten?
13. Welche potenziellen Auswirkungen hat der unbefugte Zugriff auf die Vertraulichkeit, Integrität und Verfügbarkeit der SageMaker-Ressourcen und der zugehörigen Daten?
14. Wie können die betroffenen SageMaker-Ressourcen sicher isoliert, gesichert und möglicherweise wiederhergestellt oder neu aufgebaut werden?
15. Welche speziellen bewährten Sicherheitsmethoden oder -konfigurationen von SageMaker wurden nicht befolgt, was zu unberechtigtem Zugriff führte?

## Eindämmung und Beseitigung

Wenn Ressourcen von einem nicht autorisierten Benutzer erstellt wurden oder eine Ressource nicht zur Erstellung autorisiert wurde, folgen Sie den folgenden Anweisungen zum Löschen/Ändern der erstellten Ressourcen oder Berechtigungen:

- [So löschen Sie eine SageMaker-Domain (Konsole)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio)
- [So löschen Sie eine SageMaker-Domain (CLI)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-cli)
- [So löschen Sie einen SageMaker-Endpunkt] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint)
- [So löschen Sie eine SageMaker-Endpunktkonfiguration] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint-config)
- [So löschen Sie ein SageMaker-Modell] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-model)
- [Root-Zugriff aus SageMaker-Notebooks entfernen] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html) (Gehen Sie zur gewünschten Notebook-Instanz/Instanz stoppen/Klicken Sie anschließend auf Bearbeiten/Wählen Sie unter Berechtigungen die Option Notebook-Instanz deaktivieren/aktualisieren/Instanz ausführen)


## Behobene Backlog-Elemente
- Als Incident Responder muss ich in der Lage sein, alle kritischen SageMaker-Ereignisse zu überwachen
- Als Incident Responder benötige ich ein Playbook zur skalierten Abfrage von SageMaker Cloudtrail-Ereignissen

## Aktuelle Backlog-Einträge

## Anhang — Bewährte Methoden

**Dringend empfohlen**

— [In einer isolierten VPC bereitstellen] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Verwenden Sie den VPC-Endpunkt, um auf Ressourcen zuzugreifen
- [Sagemaker-Notebooks sollten den Zugriff auf Verbindungen innerhalb der VPC einschränken] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Verwenden Sie Sicherheitsgruppen und NACLs, um den Datenverkehr in und aus Ihrer Umgebung zu kontrollieren
- [Verschlüsselung des Datenverkehrs zwischen Containern für Trainingsaufgaben mit mehreren Recheninstanzen] (https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)
- [Verschlüsselung im Ruhezustand mit KMS aktivieren] (https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest.html)
- [Deaktivieren Sie den Root-Zugriff auf Notebooks, falls er nicht benötigt wird] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)
— [Bewährte Methoden für die Lebenszykluskonfiguration] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html)
— Erstellen Sie eine Zulassungsliste mit Paketen, die das Team verwenden kann, um das Risiko der Ausführung von bösartigem Code zu verringern
- [Geringste Rechte bei Verwendung von IAM-Rollen und ressourcenbasierten Richtlinien (z. B. Bucket-Richtlinien für den Zugriff auf S3-Bucket-Daten), Nutzung von ML Governance] (https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html)
- [Verwenden Sie das IAM Identity Center] (https://docs.aws.amazon.com/sagemaker/latest/dg/domain-user-profile-add-remove.html)
- [Anmeldeinformationen in Secrets Manager speichern und rotieren] (https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating-sagemaker.html)
- [Überwachen Sie die Eingabe und Ausgabe von Modellen mit SageMaker Model Monitor] (https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html)
— [Aktivieren Sie die CloudTrail S3-Datenereignisprotokollierung für die Prüfung von S3-Daten und Modellartefakten] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)
— [Aktivieren Sie SageMaker-Experimente und nutzen Sie die Versionskontrolle für Modellartefakte] (https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)
- [Aktivieren Sie VPC Flow Logs, um den Netzwerkverkehr in Ihrer VPC zu überwachen] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [Verwenden Sie CodeArtifact, um benötigte Bibliotheken/Pakete aus dem Internet herunterzuladen] (https://aws.amazon.com/blogs/machine-learning/secure-aws-codeartifact-access-for-isolated-amazon-sagemaker-notebook-instances/)
- [CloudWatch kann auch zur Überwachung von SageMaker verwendet werden] (https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)

**Ermutigt**

— [Verwenden Sie Amazon Macie, um sensible S3-Daten zu schützen] (https://docs.aws.amazon.com/macie/latest/user/data-classification.html)
— [Verwenden Sie Service Catalog, um vorab geprüfte SageMaker-Ressourcen wie Notebooks zu verkaufen, um die Instance-Größe einzuschränken und die Verwendung sicherer Konfigurationen durchzusetzen] (https://aws.amazon.com/blogs/machine-learning/automate-a-centralized-deployment-of-amazon-sagemaker-studio-with-aws-service-catalog/)