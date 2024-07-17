# Incident Response Playbook: Reagieren auf einfache E-Mail-Service-Ereignisse

## Ansprechpartner

Autor: `Name des Autors“
Genehmiger: `Name des Genehmigers“
Letztes Datum genehmigt:

### Ziele
Konzentrieren Sie sich während der Ausführung des Playbooks auf die gewünschten _***-Ergebnis***_ und machen Sie sich Notizen zur Verbesserung der Funktionen zur Reaktion auf Vorfälle.

#### Bestimmen Sie:
* **Ausgenutzte Schwachstellen**
* **Exploits und Tools beobachtet**
* **Absicht des Schauspieler**
* **Zuordnung des Schauspieler**
* **Schaden der Umwelt und dem Unternehmen verursacht**

#### Wiederherstellen:
* **Zurück zur ursprünglichen und gehärteten Konfiguration**

#### Komponenten der CAF-Sicherheitsperspektive verbessern:
[Sicherheitsperspektive des AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Richtlinie**
* **Detektiv**
* **Verantwortlich**
* **Präventiv**

### Antwortschritte
1. [**VORBEREITUNG**] Verwenden Sie AWS GuardDuty GuardDuty-Erkennungen für IAM
2. [**PREPARATION**] Identifizieren, Dokumentieren und Testen von Eskalationsverfahren
3. [**ERKENNUNG UND ANALYSE**] Erkennung durchführen und analysieren CloudTrail auf nicht erkannte API-Ereignisse
4. [**ERKENNUNG UND ANALYSE**] Erkennung durchführen und analysieren Sie CloudWatch auf nicht erkannte Ereignisse
3. [**EINDÄMMUNG & LÖSCHUNG**] IAM-Benutzerschlüssel löschen oder drehen
4. [**CONTAINMENT & ERADICATION**] Löschen oder Drehen Sie unerkannte Ressourcen
5. [**WIEDERHERSTELLUNG**] Führen Sie gegebenenfalls Wiederherstellungsverfahren aus

***Die Antwortschritte folgen dem Lebenszyklus der Incident Response von [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

### Klassifizierung und Handhabung von Vorfällen
* **Taktiken, Techniken und Verfahren**:
* **Kategorie**:
* **Ressourcen**: SES
* **Indikatoren**:
* **Quellen**:
* **Teams**: Security Operations Center (SOC), Forensische Ermittler, Cloud Engineering

## Prozess zur Handhabung von Vorfällen
### Der Reaktionsprozess auf Vorfälle hat die folgenden Phasen:
* Vorbereitung
* Erkennung & Analyse
* Eindämmung & Ausrottung
* Erholung
* Aktivität nach dem Vorfall

## Zusammenfassung
Dieses Playbook beschreibt den Prozess für Antworten auf Angriffe auf AWS Simple Email Service (SES). In Kombination mit diesem Leitfaden lesen Sie bitte die [FAQs zum Senden des Überprüfungsprozesses von Amazon SES] (https://docs.aws.amazon.com/ses/latest/DeveloperGuide/faqs-enforcement.html), um Antworten auf Durchsetzungsmaßnahmen und Antworten auf nachteilige SES-Nutzung zu erhalten.

Weitere Informationen finden Sie im [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Vorbereitung - Allgemein
* Bewerten Sie Ihre Sicherheitslage, um Sicherheitslücken zu identifizieren und zu beheben
* AWS hat ein neues Open Source [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) Tool entwickelt, das Kunden eine Point-in-Time-Bewertung bietet, um wertvolle Einblicke in die Sicherheitslage ihres AWS zu erhalten konto.
* Pflegen Sie einen vollständigen Asset-Bestand aller Ressourcen, einschließlich Server, Netzwerkgeräte, Netzwerk-/Dateifreigaben und Entwicklermaschinen
* Erwägen Sie, [AWS GuardDuty] (https://aws.amazon.com/guardduty/) zu implementieren, um kontinuierlich auf bösartige Aktivitäten und nicht autorisiertes Verhalten zu überwachen, um Ihre AWS-Konten, Workloads und Daten zu schützen, die in Amazon SES gespeichert sind
* Implementieren Sie [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) einschließlich Ablauf von Konten und obligatorischen Rotationen von Anmeldeinformationen
* **Multi-Faktor-Authentifizierung (MFA) erzwingen**
* Anforderungen an die Passwortkomplexität durchsetzen und Ablaufzeiträume festlegen
* Führen Sie einen [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) aus, um alle Benutzer in Ihrem Konto und den Status ihrer verschiedenen Anmeldeinformationen aufzulisten, einschließlich Passwörter, Zugriffsschlüssel und MFA-Geräte
* Verwenden Sie [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html), um die Ressourcen in Ihrer Organisation und Ihren Konten zu identifizieren, z. B. IAM-Rollen, die mit einer externen Entität geteilt werden. Auf diese Weise können Sie unbeabsichtigten Zugriff auf Ihre Ressourcen und Daten identifizieren, was ein Sicherheitsrisiko darstellt

## Vorbereitung - SES-spezifisch
* Verwenden Sie Sendeautorisierungsrichtlinien, um zu steuern, wer E-Mails senden kann und von wo
* https://docs.aws.amazon.com/ses/latest/dg/sending-authorization.html
* Verwenden Sie Konfigurationssätze, um IP-Adressen und Identitäten zu steuern, die zum Senden von Nachrichten zulässig sind
* https://docs.aws.amazon.com/ses/latest/dg/using-configuration-sets.html
* Verwenden Sie dedizierte IP-Adressen für Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html
* Verwalten Sie Ihre eigenen Listen für Mailing und Abonnements sowie für die E-Mail-Unterdrückung in Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/lists-and-subscriptions.html
* Richten Sie Amazon SES-Ereignisveröffentlichung für Echtzeit-Benachrichtigungen
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-using-event-publishing-setup.html
* Verwenden Sie Identitäts- und Zugriffsmanagement mit den wenigsten Berechtigungen in Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html
* Überprüfen Sie die Best Practices von SES und konzentrieren Sie sich auf Sicherheit und Zugriff
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html (Klassisch)
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/control-user-access.html (Klassisch)
* Richten Sie SPF, DKIM und DMARC für Ihre eigene Domain ein, um Phishing und Spoofing zu verhindern
* https://docs.aws.amazon.com/ses/latest/dg/email-authentication-methods.html

### Mögliche AWS GuardDuty GuardDuty-Erkennungen
Die folgenden Ergebnisse beziehen sich auf IAM-Entitäten und Zugriffsschlüssel und haben immer einen Ressourcentyp von AccessKey. Der Schweregrad und die Details der Ergebnisse unterscheiden sich je nach Befund. Weitere Informationen zu jedem Findetyp finden Sie auf der Webseite [GuardDuty IAM-Findungstypen] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html).

* CredentialAccess:IAMUser/AnomalousBehavior
* DefenseEvasion:IAMUser/AnomalousBehavior
* Discovery:IAMUser/AnomalousBehavior
* Exfiltration:IAMUser/AnomalousBehavior
* Impact:IAMUser/AnomalousBehavior
* InitialAccess:IAMUser/AnomalousBehavior
* PenTest:IAMUser/KaliLinux
* PenTest:IAMUser/ParrotLinux
* PenTest:IAMUser/PentooLinux
* Persistence:IAMUser/AnomalousBehavior
* Policy:IAMUser/RootCredentialUsage
* PrivilegeEscalation:IAMUser/AnomalousBehavior
* Recon:IAMUser/MaliciousIPCaller
* Recon:IAMUser/MaliciousIPCaller.Custom
* Recon:IAMUser/TorIPCaller
* stealth:iamUser/CloudTrailLoggingDisabled
* Stealth:IAMUser/PasswordPolicyChange
* UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B
* UnauthorizedAccess:iamUser/InstanceCredentiAlexFiltration.OutsideAws
* UnauthorizedAccess:IAMUser/MaliciousIPCaller
* UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom
* UnauthorizedAccess:IAMUser/TorIPCaller

## Eskalationsverfahren
- „Ich brauche eine Geschäftsentscheidung darüber, wann die EC2-Forensik durchgeführt werden soll“
- `Wer überwacht die Logs/Warnungen, empfängt sie und reagiert auf jeden? `
- `Wer wird benachrichtigt, wenn eine Warnung entdeckt wird? `
- `Wann werden Öffentlichkeitsarbeit und Recht in den Prozess einbezogen? `
- `Wann würden Sie sich an den AWS Support wenden, um Hilfe zu erhalten? `

## Erkennung und Analyse
### CloudTrail
Amazon SES ist in AWS CloudTrail integriert, einem Service, der eine Aufzeichnung der von einem Benutzer, einer Rolle oder einem AWS-Service in Amazon SES ausgeführten Aktionen bereitstellt. CloudTrail erfasst API-Aufrufe für Amazon SES als Ereignisse. Zu den erfassten Aufrufen gehören Anrufe von der Amazon SES-Konsole und Codeaufrufe an die Amazon SES-API-Vorgänge.

Im Folgenden finden Sie bestimmte CloudTrail `EventName` Ereignisse, um nach Änderungen in Ihrem Konto in Bezug auf SES zu suchen:

* deleteIdentity
* deleteIdentityPolicy
* deletereceiptFilter
* deletereceiptRule
* deletereceiptruleSet
* deleteVerifiedeMailAddress
* GetSendQuota
* PutidentityPolicy
* updateReceiptRule
* VerifyDomaindKIM
* VerifyDomainIdentity
* VerifyeMailAddress
* verifyemailidentity

[Ereignisse mit CloudTrail-Ereignisverlauf anzeigen] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)

### Andere Detektivsteuerungen
* Protokollieren und überwachen Sie SES-Ereignisse (z. B. Senden, Bounces, Beschwerden usw.)
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Überwachen Sie die Kennzahlen der Absender
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Richten Sie geeignete Cloudwatch-Alarme oder ein Dashboard für SES-Metriken ein
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Eindämmung und Ausrottung
* [Löschen oder Drehen von IAM-Benutzerschlüsseln] (https://console.aws.amazon.com/iam/home#users) und [Root-Benutzerschlüssel] (https://console.aws.amazon.com/iam/home#security_credential); Sie können alle Schlüssel in Ihrem Konto drehen, wenn Sie einen bestimmten Schlüssel oder Schlüssel nicht identifizieren können, die freigelegt wurden
* [Unautorisierte IAM-Benutzer löschen] (https://console.aws.amazon.com/iam/home#users.)
* [Unautorisierte Richtlinien löschen] (https://console.aws.amazon.com/iam/home#/policies)
* [Unautorisierte Rollen löschen] (https://console.aws.amazon.com/iam/home#/roles)
* [Temporäre Anmeldeinformationen widerrufen] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Temporäre Anmeldeinformationen können auch durch Löschen des IAM-Benutzers widerrufen werden.
* HINWEIS: Das Löschen von IAM-Benutzern kann sich auf Produktions-Workloads auswirken und sollte mit Vorsicht durchgeführt werden

## Erholung
* Erstellen Sie neue IAM-Benutzer mit Zugriffsrichtlinien mit den geringsten Berechtigungen
* Implementieren Sie Schritte und Ressourcen im Abschnitt `Vorbereitung - SES-Spezifisch`
* Protokollieren und überwachen Sie SES-Ereignisse (z. B. Senden, Bounces, Beschwerden usw.)
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Überwachen Sie die Kennzahlen der Absender
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Richten Sie geeignete Cloudwatch-Alarme oder ein Dashboard für SES-Metriken ein
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Gelernte Lektionen
„Dies ist ein Ort, an dem Sie Elemente hinzufügen können, die für Ihr Unternehmen spezifisch sind, die nicht unbedingt „repariert“ werden müssen, aber wichtig sind, wenn Sie dieses Playbook zusammen mit betrieblichen und geschäftlichen Anforderungen ausführen. `

## Angesprochene Backlog-Elemente

## Aktuelle Backlog-Artikel