---
title: 'HIPAA HITRUST-Blaupausenbeispiel: Steuerungen'
description: Es wird beschrieben, wie Sie die Zuordnung von HIPAA HITRUST-Blaupausenbeispielen steuern. Jede Steuerung wird mindestens einer Azure-Richtlinie zugeordnet, die Sie bei der Bewertung unterstützt.
ms.date: 11/26/2019
ms.topic: sample
ms.openlocfilehash: c6680055cfe41a2f893d46cba797783913d880ce
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "75470745"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>Steuern der Zuordnung von HIPAA HITRUST-Blaupausenbeispielen

Im folgenden Artikel wird beschrieben, wie das HIPAA HITRUST-Blaupausenbeispiel von Azure Blueprints den HIPAA HITRUST-Steuerungen zugeordnet wird. Weitere Informationen zu den Steuerungen finden Sie unter [HIPAA HITRUST](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

Die folgenden Zuordnungen gelten für die **HIPAA HITRUST**-Steuerungen. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Steuerungszuordnung springen. Viele der zugeordneten Steuerungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Vorschauversion\]: Überprüfen von HIPAA HITRUST-Steuerungen**.

> [!IMPORTANT]
> Jede Steuerung unten ist einer oder mehreren [Azure Policy](../../../policy/overview.md)-Definitionen zugeordnet. Diese Richtlinien können Ihnen bei der [Konformitätsbewertung](../../../policy/how-to/get-compliance-data.md) mit der Steuerung helfen. Es gibt jedoch oft keine 1:1- oder vollständige Übereinstimmung zwischen einer Steuerung und einer bzw. mehreren Richtlinien. Daher bezieht sich **Konform** in Azure Policy nur auf die Richtlinien selbst und gewährleistet nicht die vollständige Konformität mit allen Anforderungen einer Steuerung. Außerdem enthält der Kompatibilitätsstandard Steuerungen, die derzeit von keiner Azure Policy-Definition abgedeckt werden. Daher ist die Konformität in Azure Policy nur eine partielle Ansicht Ihres gesamten Konformitätsstatus. Die Zuordnungen zwischen Steuerungen und Azure Policy-Definitionen für dieses Konformitätsblaupausenbeispiel können sich im Laufe der Zeit ändern. Den Änderungsverlaufs finden Sie im [GitHub-Commit-Verlauf](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md).

## <a name="control-against-malicious-code"></a>Nutzen der Steuerung als Schutz vor Schadcode

Diese Blaupause hilft Ihnen bei der Verwaltung des Endpunktschutzes, einschließlich des Schutzes vor schädlichem Code, indem sie [Azure Policy](../../../policy/overview.md)-Definitionen zuweist, die virtuelle Computer in Azure Security Center auf fehlenden Endpunktschutz überprüfen und die Microsoft-Antischadsoftwarelösung auf virtuellen Windows-Computern erzwingen.

- Bereitstellen der Microsoft IaaS-Antischadsoftware-Standarderweiterung für Windows Server
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein.
- Systemupdates sollten auf Ihren Computern installiert sein.


## <a name="management-of-removable-media"></a>Verwaltung von Wechselmedien

Die Organisation registriert Medien (einschließlich Laptops) vor der Nutzung basierend auf der Datenklassifizierungsebene, legt angemessene Einschränkungen zur Nutzung dieser Medien fest und richtet einen angemessenen Grad an physischem und logischem Schutz (einschließlich Verschlüsselung) für Medien mit relevanten Informationen ein, bis diese korrekt zerstört oder bereinigt wurden.

- Verschlüsselung für Data Lake Store-Konten erforderlich
- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.
- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.


## <a name="information-exchange-policies-and-procedures"></a>Richtlinien und Verfahren für den Informationsaustausch

Cloud-Dienstanbieter nutzen eine branchenübliche Virtualisierungsplattform und Standardvirtualisierungsformate (z. B. Open Virtualization Format, OVF), um die Interoperabilität sicherzustellen. Darüber hinaus verfügen sie über eine Dokumentation der benutzerdefinierten Änderungen, die an verwendeten Hypervisors vorgenommen werden, sowie aller lösungsspezifischen Virtualisierungshooks, die für die Kundenüberprüfung verfügbar sind.

- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, auf denen die angegebenen Anwendungen nicht installiert sind

## <a name="control-of-operational-software"></a>Steuerung der Betriebssoftware 

Die Organisation identifiziert nicht autorisierte Software im Informationssystem, z. B. auf Servern, Arbeitsstationen und Laptops, nutzt eine Richtlinie vom Typ „Alles zulassen/Durch Ausnahmen verweigern“, um die Ausführung von bekannter nicht autorisierter Software im Informationssystem zu verbieten, und überprüft und aktualisiert die Liste mit nicht autorisierter Software regelmäßig (mindestens einmal pro Jahr).

- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Überwachen“ anzeigen
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Systemüberwachungsrichtlinien – Kontoverwaltung“ anzeigen

## <a name="change-control-procedures"></a>Ändern von Steuerungsverfahren

Die Integrität aller VM-Images wird jederzeit sichergestellt, indem alle Änderungen, die an VM-Images vorgenommen werden, protokolliert werden und eine entsprechende Warnung ausgelöst wird. Darüber hinaus werden die Ergebnisse einer Änderung oder Verschiebung und die nachfolgende Überprüfung der Imageintegrität für Geschäftsinhaber bzw. Kunden auf elektronischem Wege (z. B. über Portale oder Warnungen) bereitgestellt.

- \[Vorschau\]: Vorschauüberwachungsergebnisse aus Windows-VM-Konfigurationen in „Systemüberwachungsrichtlinien – Detaillierte Nachverfolgung“ anzeigen

## <a name="inventory-of-assets"></a>Ressourcenbestand 

Verwaltung eines Bestands von Ressourcen und Diensten

- In den Search-Diensten sollten Diagnoseprotokolle aktiviert sein
- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VM-Konfigurationen in „Sicherheitsoptionen – Microsoft-Netzwerk (Server)“ bereitstellen
- \[Vorschau\]: Anforderungen zum Überwachen von Windows-VM-Konfigurationen in „Administrative Vorlagen – Netzwerk“ bereitstellen

## <a name="control-of-technical-vulnerabilities"></a>Steuerung von technischen Sicherheitsrisiken 

Für alle System- und Netzwerkkomponenten ist ein festgeschriebener Konfigurationsstandard vorhanden.

- Überwachung von virtuellen Computern ohne konfigurierte Notfallwiederherstellung
- Bewertung von Sicherheitsrisiken für verwaltete SQL-Instanzen aktivieren
- Sicherheitsrisiko sollte durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden

## <a name="segregation-in-networks"></a>Trennung in Netzwerken

Mit den Sicherheitsgateways der Organisation (z. B. Firewalls) werden Sicherheitsrichtlinien erzwungen, und sie sind für die Filterung von Datenverkehr zwischen Domänen und die Blockierung von unberechtigtem Zugriff konfiguriert. Außerdem werden sie genutzt, um die Trennung zwischen internen kabelgebundenen, internen drahtlosen sowie externen Netzwerksegmenten (z. B. dem Internet), z. B. DMZ, zu verwalten und die Zugriffssteuerungsrichtlinien für die einzelnen Domänen zu erzwingen.

- Automatische Bereitstellung des Sicherheitsüberwachungs-Agents
- Bereitstellen von Network Watcher beim Erstellen virtueller Netzwerke

## <a name="input-data-validation"></a>Überprüfung von Eingabedaten

Für alle öffentlichen Webanwendungen werden Firewalls auf Anwendungsebene implementiert, um den Datenverkehr zu steuern. Für nicht webbasierte öffentliche Anwendungen hat die Organisation eine netzwerkbasierte Firewall implementiert, die speziell für den Anwendungstyp gilt. Wenn der an die öffentliche Anwendung fließende Datenverkehr verschlüsselt wird, befindet sich das Gerät entweder hinter der Verschlüsselung oder ist in der Lage, den Datenverkehr vor der Analyse zu entschlüsseln.

- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Windows-Firewalleigenschaften“ anzeigen


## <a name="network-connection-control"></a>Steuerung von Netzwerkverbindungen

Netzwerkdatenverkehr wird gemäß der Zugriffssteuerungsrichtlinie der Organisation per Firewall und andere netzwerkbezogene Einschränkungen für jeden Netzwerkzugriffspunkt oder die verwaltete Schnittstelle des externen Telekommunikationsdiensts gesteuert.

- Zugriff über Endpunkte mit Internetzugriff sollte eingeschränkt werden
- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Remotedebuggen für API-Apps sollte deaktiviert werden

## <a name="network-controls"></a>Netzwerksteuerungen

Die Organisation verwendet geschützte und verschlüsselte Kommunikationskanäle, wenn physische Server, Anwendungen oder Daten zu virtualisierten Servern migriert werden.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden
- Die TDE-Schutzvorrichtung in SQL Server sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Netzwerkzugriff“ anzeigen
- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Windows-Firewalleigenschaften“ anzeigen
- Bereitstellen von Diagnoseeinstellungen aus Netzwerksicherheitsgruppen
- Zugriff über Endpunkte mit Internetzugriff sollte eingeschränkt werden

## <a name="sensitive-system-isolation"></a>Isolation von sensiblen Systemen

Freigegebene Systemressourcen (z. B. Register, Hauptspeicher, sekundärer Speicher) werden wieder für das System freigegeben und vor der Offenlegung für andere Systeme/Anwendungen/Benutzer geschützt. Benutzer sind nicht in der Lage, absichtlich oder unabsichtlich auf Restinformationen zuzugreifen.

- VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden

## <a name="security-of-network-services"></a>Sicherheit von Netzwerkdiensten

Zulässige Dienste, die von einem Netzwerkdienstanbieter/Manager bereitgestellt werden, werden formal verwaltet und überwacht, um für die Sicherheit der Bereitstellung zu sorgen.

- VMs sollten zu neuen Azure Resource Manager-Ressourcen migriert werden

## <a name="network-routing-control"></a>Steuerung des Netzwerkroutings

Routingsteuerungen werden über Sicherheitsgateways (z. B. Firewalls) implementiert, die zwischen internen und externen Netzwerken (z. B. dem Internet und Drittanbieter-Netzwerken) eingesetzt werden.

- Adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden

## <a name="information-exchange-policies-and-procedures"></a>Richtlinien und Verfahren für den Informationsaustausch

Die Organisation beschränkt die Verwendung von durch die Organisation gesteuerten Wechselmedien durch autorisierte Personen auf externen Informationssystemen.

- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- Remotedebuggen sollte für Webanwendungen deaktiviert werden
- Auf API-Apps sollte nur über HTTPS zugegriffen werden können

## <a name="electronic-messaging"></a>Elektronische Nachrichten

Die Genehmigungen werden vor der Nutzung von externen öffentlichen Diensten, z. B. Instant Messaging oder Dateifreigabe, eingeholt.

- \[Vorschau\]: Überwachungsergebnisse von Linux-VMs anzeigen, bei denen die Dateiberechtigungen für Kennwörter nicht auf 0644 festgelegt sind

## <a name="on-line-transactions"></a>Onlinetransaktionen

Die Organisation benötigt die Nutzung von Verschlüsselung zwischen den bzw. den Einsatz von elektronischen Signaturen durch die jeweiligen Beteiligten einer Transkation. Die Organisation stellt sicher, dass die Speicherung der Transaktionsdetails außerhalb von öffentlich zugänglichen Umgebungen erfolgt (z. B. auf einer Speicherplattform im Intranet der Organisation) und dass diese Daten nicht auf einem Speichermedium aufbewahrt und verfügbar gemacht werden, das direkt über das Internet zugänglich ist. Bei Verwendung einer vertrauenswürdigen Autorisierungsstelle (z. B. zum Ausstellen und Verwalten von digitalen Signaturen bzw. Zertifikaten) ist die Sicherheit in den gesamten End-to-End-Prozess zur Verwaltung von Zertifikaten bzw. Signaturen integriert und eingebettet.

- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden
- \[Vorschau\]: Überwachungsergebnisse von Windows-VMs anzeigen, die nicht die angegebenen Zertifikate im vertrauenswürdigen Stamm enthalten

## <a name="password-management"></a>Kennwortverwaltung

Kennwörter werden während der Übertragung und der Speicherung auf allen Systemkomponenten verschlüsselt.

- \[Vorschau\]: Überwachungsergebnisse von Windows-VMs anzeigen, auf denen die Einstellung für die Kennwortkomplexität nicht aktiviert ist

## <a name="user-authentication-for-external-connections"></a>Benutzerauthentifizierung für externe Verbindungen

Sichere Authentifizierungsmethoden, z. B. mehrstufig, Radius oder Kerberos (für privilegierten Zugriff) und CHAP (zur Verschlüsselung von Anmeldeinformationen für Einwahlverfahren), werden für alle externen Verbindungen mit dem Organisationsnetzwerk implementiert.

- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden
- Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden

## <a name="user-identification-and-authentication"></a>Identifizierung und Authentifizierung von Benutzern

Benutzer, die privilegierte Funktionen übernommen haben (z. B. Systemadministration), nutzen dabei separate Konten. Verfahren für die mehrstufige Authentifizierung werden in Übereinstimmung mit der organisationsweiten Richtlinie genutzt (z. B. für den Remotezugriff auf Netzwerke).

- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden
- Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden

## <a name="privilege-management"></a>Berechtigungsverwaltung

Der Zugriff auf Verwaltungsfunktionen oder -konsolen für Systeme, die virtualisierte Systeme hosten, ist für Mitarbeiter gemäß dem Prinzip der geringsten Rechte eingeschränkt und wird anhand von technischen Steuerungen unterstützt.

- Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden
- \[Vorschau\]: Rollenbasierte Zugriffssteuerung (RBAC) für Kubernetes Service verwenden

## <a name="review-of-user-access-rights"></a>Überprüfung der Zugriffsrechte für Benutzer

Die Organisation führt eine dokumentierte Liste mit Informationsressourcen für autorisierte Benutzer.

- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Konten“ anzeigen

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Remotediagnose und Schutz von Konfigurationsports

Ports, Dienste und ähnliche Anwendungen, die auf einem Computer oder in Netzwerksystemen installiert und für den Geschäftsablauf nicht unbedingt erforderlich sind, werden deaktiviert oder entfernt.

- Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden

## <a name="audit-logging"></a>Überwachungsprotokollierung

Es werden Protokolle mit gesendeten und empfangenen Nachrichten geführt. Sie enthalten Informationen wie Datum, Uhrzeit, Ursprung und Ziel der Nachricht, aber nicht ihren Inhalt. Die Überwachung ist immer verfügbar, während das System aktiv ist, und die folgenden wichtigen Ereignisse werden nachverfolgt: Erfolg/Fehler für Datenzugriff, Änderungen an der Sicherheitskonfiguration des Systems, privilegierte Nutzung oder Hilfsprogrammnutzung, ausgelöste Alarme, Aktivierung/Deaktivierung von Schutzsystemen (z. B. A/V und IDS), Aktivierung/Deaktivierung von Identifikations- und Authentifizierungsmechanismen und Erstellung und Löschung von Objekten auf Systemebene.

- In Event Hub sollten Diagnoseprotokolle aktiviert sein
- Systemupdates für VM-Skalierungsgruppen sollten installiert werden.

## <a name="monitoring-system-use"></a>Verwendung des Überwachungssystems

Automatisierte Systeme, die in der gesamten Umgebung der Organisation bereitgestellt werden, werden zum Überwachen von wichtigen Ereignissen und anomaler Aktivität sowie zum Analysieren von Systemprotokollen verwendet. Die Ergebnisse werden regelmäßig überprüft. Die Überwachung umfasst privilegierte Vorgänge, autorisierte und nicht autorisierte Zugriffsversuche, z. B. versuchte Zugriffe auf deaktivierte Konten, und Systemwarnungen oder -ausfälle.

- In VM-Skalierungsgruppen sollten Diagnoseprotokolle aktiviert sein

## <a name="segregation-of-duties"></a>Aufgabentrennung

Die Aufgabentrennung wird verwendet, um das Risiko einer nicht autorisierten oder unbeabsichtigten Änderung von Informationen und Systemen einzuschränken. Einzelpersonen können ohne Autorisierung oder Erkennung nicht auf Informationssysteme zugreifen und diese nicht ändern oder nutzen. Der Zugriff für Personen, die für die Verwaltung und den Zugriff auf Steuerungen zuständig sind, ist je nach Rolle und Verantwortungsbereich des jeweiligen Benutzers auf das erforderliche Minimum beschränkt. Diese Personen können nicht auf Überwachungsfunktionen für diese Steuerungen zugreifen.

- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden

## <a name="administrator-and-operator-logs"></a>Administrator- und Bedienerprotokolle

Die Organisation stellt sicher, dass die korrekte Protokollierung aktiviert ist, um Administratoraktivitäten zu überwachen. Die Systemadministrator- und Bedienerprotokolle werden regelmäßig überprüft.

- MFA sollte für Konten mit Schreibrechten für Ihr Abonnement aktiviert werden

## <a name="identification-of-risks-related-to-external-parties"></a>Identifizierung von Risiken in Bezug auf externe Parteien

Remotezugriffsverbindungen zwischen der Organisation und externen Parteien werden verschlüsselt

- Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden

## <a name="business-continuity-and-risk-assessment"></a>Geschäftskontinuität und Risikobewertung

Die Organisation identifiziert ihre kritischen Geschäftsprozesse und integriert die Verwaltungsanforderungen für die Informationssicherheit in Bezug auf die Geschäftskontinuität in andere Kontinuitätsanforderungen. Hierbei geht es beispielsweise um Aspekte wie operativer Betrieb, Personalwesen, Material, Transport und Einrichtungen.

- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Wiederherstellungskonsole“ anzeigen

> [!NOTE]
> Die Verfügbarkeit spezifischer Azure Policy-Definitionen kann in Azure Government und anderen nationalen Clouds variieren. 

## <a name="next-steps"></a>Nächste Schritte

Sie haben sich die Informationen zur Steuerungszuordnung anhand des HIPAA HITRUST-Blaupausenbeispiels durchgelesen. Lesen Sie als Nächstes die folgenden Artikel, um sich mit der Übersicht und der Bereitstellung dieses Beispiels vertraut zu machen:

> [!div class="next step action"]
> [HIPAA HITRUST-Blaupause: Übersicht](./control-mapping.md)
> [HIPAA HITRUST-Blaupause: Bereitstellungsschritte](./deploy.md)

Weitere Artikel zu Blaupausen und ihrer Nutzung:

- Erfahren Sie mehr über den [Lebenszyklus von Blaupausen](../../concepts/lifecycle.md).
- Machen Sie sich mit der Verwendung [statischer und dynamischer Parameter](../../concepts/parameters.md) vertraut.
- Erfahren Sie, wie Sie die [Abfolge von Blaupausen](../../concepts/sequencing-order.md) anpassen können.
- Erfahren Sie, wie Sie [Ressourcen in Blaupausen sperren](../../concepts/resource-locking.md) können.
- Lernen Sie, wie Sie [vorhandene Zuweisungen aktualisieren](../../how-to/update-existing-assignments.md).
