# Rationalisierung von Warnmeldungen für Sicherheitsvorfälle
Dieses Dokument wird nur zu Informationszwecken bereitgestellt. Es stellt die aktuellen Produktangebote und -praktiken von Amazon Web Services (AWS) zum Ausstellungsdatum dieses Dokuments dar, die sich ohne vorherige Ankündigung ändern können. Die Kunden sind dafür verantwortlich, ihre eigene unabhängige Bewertung der Informationen in diesem Dokument und jede Nutzung von AWS-Produkten oder -Dienstleistungen durchzuführen, von denen jede „wie besehen“ ohne ausdrückliche oder stillschweigende Gewährleistung jeglicher Art bereitgestellt wird. Dieses Dokument enthält keine Garantien, Zusicherungen, vertraglichen Verpflichtungen, Bedingungen oder Zusicherungen von AWS, seinen verbundenen Unternehmen, Lieferanten oder Lizenzgebern. Die Verantwortlichkeiten und Verbindlichkeiten von AWS gegenüber seinen Kunden werden durch AWS-Vereinbarungen kontrolliert, und dieses Dokument ist weder Teil noch ändert es eine Vereinbarung zwischen AWS und seinen Kunden.

© 2021 Amazon Web Services, Inc. oder seine verbundenen Unternehmen. Alle Rechte vorbehalten. Dieses Werk ist unter einer Creative Commons Attribution 4.0 International License lizenziert.

Dieser AWS-Inhalt wird gemäß den Bedingungen der AWS-Kundenvereinbarung bereitgestellt, die unter http://aws.amazon.com/agreement verfügbar ist, oder einer anderen schriftlichen Vereinbarung zwischen dem Kunden und entweder Amazon Web Services, Inc. oder Amazon Web Services EMEA SARL oder beiden.

> „Wenn Sie Ihre Büroklammern und Diamanten mit gleicher Kraft schützen, werden Sie bald mehr Büroklammern und weniger Diamanten haben.“, wird dem ehemaligen US-Außenminister Dean Rusk zugeschrieben

Warnungen werden durch die Bedrohungsmodellierung einer Workload während der Entwicklung von Sicherheitskontrollen bestimmt. Die Verwendung von Warnungen wird durch Responsive Kontrollen definiert, ihre Definition beruht jedoch auf der gesamten Sicherheitsperspektive: Richtlinie, Präventiv, Detektiv und Responsive.

## Definitionen

### Modellierung von Bedrohungen:

* **Workload**: was Sie schützen möchten
* **Bedrohlich**: was du fürchtest, passiert
* **Auswirkung**: wie das Geschäft betroffen ist, wenn Bedrohung auf Workload trifft
* **Schwachstellkeit**: was kann die Bedrohung erleichtern
* **Minderung**: Welche Kontrollen gibt es, um Schwachstellen auszugleichen
* **Wahrscheinlichkeit**: wie wahrscheinlich ist es, dass die Bedrohung durch geltende Eindämmungen eintritt

**Die Priorisierung von Bedrohungen ist ein Faktor für Auswirkungen und Wahrscheinlichkeit. **

### Sicherheitskontrollen:

* **Richtlinien** Kontrollen legen die Governance-, Risiko- und Compliance-Modelle fest, in denen die Umgebung arbeiten wird.
* **Präventive**-Steuerelemente schützen Ihre Workloads und mindern Bedrohungen und Schwachstellen.
* **Detektive**-Steuerelemente bieten vollständige Transparenz und Transparenz über den Betrieb Ihrer Bereitstellungen in AWS.
* **Verantwortliche** Kontrollen fördern die Behebung potenzieller Abweichungen von Ihren Sicherheitsbaselines.


! [Bild] (/images/image-caf-sec.png)

## Um die Ermüdung von Warnungen und eine bessere Handhabung von Sicherheitsereignissen zu minimieren, sollte der Kontext zur Bedrohungsmodellierung Folgendes berücksichtigen:

* Relevanz der Arbeitslast (*) für das Unternehmen.
* Datenklassifizierung (*) jeder Workload-Komponente.
* Durchgängig verwendete Methodik wie STRIDE (Spoofing, Manipulation, Offenlegung von Informationen, Ablehnung, Diensteverweigerung und Erhöhung des Privilegs)
* Cloud-Sicherheitsbereitschaft und Reife.
* Fähigkeit zur Reaktion auf Vorfälle und Bedrohungsjagd
* Funktionen zur automatischen Behebung.
* Reife der Automatisierung von Vorfällen.


(*) ***Relevanz der Arbeitslast und Datenklassifizierung*** werden durch die Risikorahmeninitiative des Unternehmens definiert. Beispiele:

### Relevanz der Arbeitslast:

**Hoch**: großer Geldverlust- und Imagewahrnehmungsschaden, langfristige Auswirkungen auf das Unternehmen, geringer Erholungserfolg
**Mittel**: nachhaltiger monetärer Verlust und Imagewahrnehmungsschaden, kurzfristige geschäftliche Auswirkungen, hoher Erholungserfolg
**Niedrig**: kein messbarer monetärer Verlust und Imagewahrnehmungsschaden, keine geschäftlichen Auswirkungen, Erholung ist nicht anwendbar

### Datenklassifizierung:

**Geheim**: großer Geldverlust- und Imagewahrnehmungsschaden, langfristige Auswirkungen auf das Unternehmen, geringer Erholungserfolg
**Vertraulich**: nachhaltiger monetärer Verlust und Imagewahrnehmungsschaden, kurzfristige geschäftliche Auswirkungen, hoher Erholungserfolg
**Nicht klassifiziert**: kein messbarer monetärer Verlust und Bildwahrnehmungsschaden, keine geschäftlichen Auswirkungen, Erholung ist nicht anwendbar


## Ziel der Warnpriorisierung ist es, sie an die entsprechende Warteschlange zu senden:

* Warnungs-Triage-Warteschlange
* Warteschlange für Bedrohungen
* Warteschlange archivieren


Der Prozess, um zu definieren, wo die Warnung enden wird, hängt von allen zuvor aufgezählten Faktoren ab. Eine grundlegende Entscheidung für die Warteschlange lautet wie folgt:

### Warnungs-Triage-Warteschlange:

1. Spezifisches Risikorahmenmandat, einschließlich, aber nicht beschränkt auf Branchenregulierung, gesetzliche Compliance, Geschäftsanforderungen. In diesem Szenario hat die Arbeitslast eine Relevanz von **hoch** oder Daten werden als **geheim** klassifiziert.
2. Spezifische Anforderungen für das Bedrohungsmodell, um den formalen Reaktionsprozess für
3. Die automatische Behebung ist fehlgeschlagen für Warnungen, bei denen die Arbeitslast eine Relevanz von **mittel** oder **hoch** hat oder die Datenklassifizierung **geheim** oder **vertraulich** ist.

### **Warteschlange für die Bedrohungsjagd**:

1. Die automatische Behebung erfolgreich für Arbeitslast mit **mittel** oder **hoch** Relevanz oder Datenklassifizierung ist **geheim** oder **vertraulich**.
2. Die automatische Behebung für Arbeitslast mit **niedriger** Relevanz fehlgeschlagen oder die Datenklassifizierung ist **nicht klassifiziert**.

### Archiv-Warteschlange:

1. Die automatische Behebung für Workload mit **niedriger** Relevanz war erfolgreich oder die Datenklassifizierung ist **nicht klassifiziert**.