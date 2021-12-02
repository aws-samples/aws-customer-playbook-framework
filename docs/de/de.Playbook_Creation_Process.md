Playbook: Playbook/Runbook-Erstellungsprozess
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

## ANSPRECHPARTNER

Autor: `Name des Autors“\
Genehmiger: `Name des Genehmigers“\
Letztes Datum Genehmigt: 25.03.2021

## NIST 800-61r2 Phase

Vorbereitung

## Zusammenfassung
Dieses Playbook beschreibt den Prozess zum Erstellen eines neuen Playbooks/Runbooks, des Übergangs des Dokuments von DRAFT auf GENEHMIGT und die Re-Validierungsanforderungen mit GitLab.

Für eine erweiterte Playbook-Erstellung lesen Sie bitte den [AWS Incident Response Playbooks Workshop] (https://gitlab.aws.dev/fredski/aws-incident-response-playbooks-workshop/-/blob/main/playbooks/crypto_mining/EC2_crypto_mining.md)

## Anleitung

> Es ist wichtig, unsere Runbooks zu pflegen, um tief zu tauchen und Kunden schnell Ergebnisse zu liefern. Per Matthew Helmke „Runbooks helfen uns als Teil einer breiteren Reihe von Praktiken, die alle darauf abzielen, Zuverlässigkeit zu steigern. Runbooks auf dem neuesten Stand zu halten, ist ein wesentlicher Bestandteil der Standortzuverlässigkeitstechnik.“
(< https://www.gremlin.com/blog/ensuring-runbooks-are-up-to-date/ >)

Unser Team verwaltet mit Github die Dokumentation. Schau dir [Referenzen] an (.. /playbook_creation_Process.md/ #references) um einige abgekürzte Schritte für unseren Prozessablauf zu sehen.

## Neues Playbook/Runbook

### GUI-Verfahren

* Erstellen Sie ein Problem für die Prozessverfolgung
* Erstellen Sie einen neuen Quellzweig, in dem Sie arbeiten können
* Navigiere zum Stamm deines Zweigs
* Wählen Sie die Schaltfläche „Web IDE“
* Um ein Element zu erstellen, markieren Sie den Ordner für den linken Rand und wählen Sie den Dropdown-Pfeil für Optionen
* Wählen Sie „Neue Datei“
* Hinweis: Es kann hilfreich sein, den Inhalt aus einem vorhandenen Playbook zur Formatierung zu kopieren
* Unsere Dokumente verwenden [GitHub Flavored Markdown] (https://github.github.com/gfm/)

* Stellen Sie sicher, dass Sie die Standard-Namenskonvention für unsere Bibliothek befolgen
* Playbooks: `# Playbook: Playbook/Runbook`
* Werkzeugführungen: `# Tooling: ToolName`
* Dies ist das einzige Mal, dass Header 1 (H1) innerhalb des Dokuments verwendet wird

* Erstellen Sie einen Link zum dazugehörigen Problem
* Beispiel: `Associate Issue: (. /ausgaben/1) `

* Sobald Sie Ihr Dokument fertiggestellt haben:
* Fügen Sie in der Ausgabe einen Link zu Ihrem Dokument als Kommentar hinzu
* Fügen Sie das Label „REVIEW“ in das Problem ein
* Veröffentlichen Sie eine Nachricht mit einem Link zu Ihrem Problem in der „Kundenkorrespondenzmethode“ und fordern Sie eine Überprüfung/Kommentar an.

* Damit ein Playbook genehmigt wird, müssen mindestens zwei (2) Teammitglieder ihre Namen und einen Kommentar in das dazugehörige Problem überprüft und hinzugefügt haben

* Aktualisieren und implementieren Sie alle Empfehlungen oder Korrekturen des Dokuments

* Sobald Ihr Dokument überprüft und alle Korrekturen vorgenommen wurden, reichen Sie Ihr Dokument ein, damit der Genehmigungsprozess abgeschlossen werden kann.

* Gehen Sie auf das Problem ein und ändern Sie den Beauftragten zu einem Genehmiger
* Gehen Sie auf das Problem
* Fügen Sie das zur Genehmigung ÜBERMITTELTE Label zu Ihrem Problem hinzu
* Wählen Sie im rechten Rand neben „Beauftragter“ „Bearbeiten“
* Ändern Sie den Beauftragten in „Genehmiger“
* Senden Sie eine Notiz an den „Genehmiger“, indem Sie ihm mitteilen, dass das Dokument zur endgültigen Überprüfung bereit ist
* Fügen Sie nach der Genehmigung einen Link zum Dokument/Playbook in die Readme ein
* Beispiel: `[Playbook-Erstellungsprozess] (. /docs/PlayBook_Creation_Process.md) `
* Senden Sie eine Zusammenführungsanfrage

* Hinweis: Denken Sie daran, dass alle Schritte vor der Zusammenführungsanfrage in Ihrem Arbeitszweig abgeschlossen sein MÜSSEN

* Die SLA für diese Aufgabe beträgt 7 Kalendertage ab dem Zeitpunkt, an dem Sie das Dokument einreichen.
* Wenn innerhalb des SLA-Zeitraums keine Antwort zur Genehmigung eingegangen ist, eskalieren Sie, indem Sie eine Chime-Notiz an „Benutzer A“, „Benutzer B“, „Benutzer C“ senden.

### CLI-Verfahren

TBD

## Playbook/Runbook-Updates

### Vorgehensweise

* Erstellen Sie ein Problem für die Prozessverfolgung
* Erstellen Sie einen neuen Quellzweig, in dem Sie arbeiten können
* Navigiere zum Stamm deines Zweigs
* Wählen Sie die Schaltfläche „Web IDE“
* Öffne ein neues Problem, um den Fortschritt bei deinen Änderungen zu verfolgen
* Alle vorhandenen Labels entfernen
* Fügen Sie das Label DRAFT hinzu
* Erstellen Sie einen Link zum dazugehörigen Problem
* Sie können Ihre Probleme an die Liste anhängen
* Beispiel: (issue1), (issue1), (issue3)

* Sobald Sie Ihr Dokument fertiggestellt haben:
* Fügen Sie in der Ausgabe einen Link zu Ihrem Dokument als Kommentar hinzu
* Fügen Sie das Label „REVIEW“ in das Problem ein
* Veröffentlichen Sie eine Nachricht mit einem Link zu Ihrem Problem im „Kundenkorrespondenzmechanismus“ und fordern Sie eine Überprüfung/Kommentar an.

* Damit ein Playbook genehmigt wird, müssen mindestens zwei (2) Teammitglieder ihre Namen und einen Kommentar in das dazugehörige Problem überprüft und hinzugefügt haben

* Aktualisieren und implementieren Sie alle Empfehlungen oder Korrekturen des Dokuments

* Sobald Ihr Dokument überprüft und alle Korrekturen vorgenommen wurden, reichen Sie Ihr Dokument ein, damit der Genehmigungsprozess abgeschlossen werden kann.

* Gehen Sie auf das Problem ein und ändern Sie den Beauftragten zu einem Genehmiger
* Gehen Sie auf das Problem
* Fügen Sie das zur Genehmigung ÜBERMITTELTE Label zu Ihrem Problem hinzu
* Wählen Sie im rechten Rand neben „Beauftragter“ „Bearbeiten“
* Ändern Sie den Beauftragten in „Genehmiger“
* Senden Sie eine Notiz in Chime an „Genehmiger“, um ihnen mitzuteilen, dass das Dokument zur endgültigen Überprüfung bereit ist
* Fügen Sie nach der Genehmigung einen Link zum Dokument/Playbook in die Readme ein
* Beispiel: `[Playbook-Erstellungsprozess] (. /docs/Playbook_Creation_Process.md) `
* Senden Sie eine Zusammenführungsanfrage

* Hinweis: Denken Sie daran, dass alle Schritte vor der Zusammenführungsanfrage in Ihrem Arbeitszweig abgeschlossen sein MÜSSEN

* Die SLA für diese Aufgabe beträgt 7 Kalendertage ab dem Zeitpunkt, an dem Sie das Dokument einreichen.
* Wenn innerhalb des SLA-Zeitraums keine Antwort zur Genehmigung eingegangen ist, eskalieren Sie, indem Sie eine Notiz an „Benutzer A“, „Benutzer B“, „Benutzer C“ senden

### CLI-Verfahren

TBD

## Genehmigungsprozess

### Task-Besitzer:

Team-Inhaber

### Service Level Agreement

- 7 Kalendertage ab Einreichung des Autors

### Vorgehensweise

* Überprüfen Sie das Dokument und das zugehörige Problem in GitHub

* Löschen Sie REVIEW aus dem Titel des Dokuments

* Lösche die Anweisung THIS IS A DRAFT PLAYBOOK aus dem Dokument

* Unter Kontaktstellen
- Hinzufügen/aktualisieren Sie Ihren Namen zu Genehmiger
- „Letztes Datum genehmigt“ als JJJJJ/MM/TT hinzufügen/aktualisieren, wenn Sie das Dokument genehmigen

* Weisen Sie das Problem dem Anforderer wieder zu
* Fügen Sie dem Problem, dass das Playbook genehmigt wurde, einen Kommentar hinzu und fahren Sie mit einer Zusammenführungsanfrage fort

## Referenzen

### Verwenden von GitHub: Erstellen Sie ein Problem

Wir verwenden Probleme, um laufende und abgeschlossene Arbeiten zu verfolgen. Darüber hinaus unterstützt dies das Team beim Übergang durch unseren Überprüfungs- und Genehmigungsprozess. Dies ermöglicht es den Kommentaren von Gutachtern auch, im Rahmen unseres Prozesses zur Erhebung der Dokumentationsleiste alles mit Repository-Objekten anzusprechen.

* Grafische Benutzeroberfläche: Die GUI ermöglicht es Ihnen, über einen Webbrowser mit GitHub zu interagieren, um mit diesem Repository zu interagieren
1. Navigieren Sie mit Ihrem bevorzugten Webbrowser zu Ihrem GitHub-Repository
1. Öffnen Sie ein Problem, indem Sie im oberen Menü Probleme auswählen
1. Wähle „Neues Problem“
1. Füge einen Titel und eine Beschreibung dessen hinzu, woran du gerade arbeitest
1. Weisen Sie sich selbst zu (dies ermöglicht das Tracking in späteren Schritten)
1. Wählen Sie für Meilenstein „Kundenmeilenstein“
1. Wählen Sie für Labels eine beliebige aus, die für Ihr Problem geeignet sind
* Stellen Sie für neue Dokumente sicher, dass Sie DRAFT auswählen
1. Wählen Sie „Problem einreichen“
1. Auf der neuen Issue-Seite im rechten Rand Sperrproblem > Bearbeiten > Sperren

* Kommandozeilenschnittstelle: Die CLI ermöglicht es Ihnen, mit GitHub aus der Befehlszeile zu interagieren

### Verwenden von GitHub: Erstellen Sie einen Zweig, in dem Sie arbeiten können

Wir arbeiten in einzelnen Branchen, sodass die laufende Arbeit nicht das beeinträchtigt, was andere in Echtzeit tun könnten.

* Grafische Benutzeroberfläche: Die GUI ermöglicht es Ihnen, mit [GitHub von einem Webbrowser aus zu interagieren, um mit diesem Repository zu interagieren] (https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)

* Kommandozeilenschnittstelle: Die CLI ermöglicht es Ihnen, mit GitHub aus der Befehlszeile zu interagieren

### Verwenden von GitHub: Erstellen Sie eine Merge-Anfrage

Zusammenführungsanfragen ermöglichen eine Überprüfung der sekundären Balkenerhöhung vor dem Zusammenführen mit dem übergeordneten Repository.

* [Grafische Benutzeroberfläche] (https://docs.github.com/en/github/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges)

* Kommandozeilenschnittstelle: Die CLI ermöglicht es Ihnen, mit GitHub aus der Befehlszeile zu interagieren

## Backlog-Artikel