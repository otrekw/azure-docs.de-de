---
title: 'HIPAA HITRUST-Blaupausenbeispiel: Steuerungen'
description: Es wird beschrieben, wie Sie die Zuordnung von HIPAA HITRUST-Blaupausenbeispielen steuern. Jede Steuerung wird mindestens einer Azure-Richtlinie zugeordnet, die Sie bei der Bewertung unterstützt.
ms.date: 08/03/2020
ms.topic: sample
ms.openlocfilehash: 10b771e3cfb18a28bd720332a26e13bb1d1f6022
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88209422"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>Steuern der Zuordnung von HIPAA HITRUST-Blaupausenbeispielen

Im folgenden Artikel wird beschrieben, wie das HIPAA HITRUST-Blaupausenbeispiel von Azure Blueprints den HIPAA HITRUST-Steuerungen zugeordnet wird. Weitere Informationen zu den Steuerungen finden Sie unter [HIPAA HITRUST](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

Die folgenden Zuordnungen gelten für die **HIPAA HITRUST**-Steuerungen. Über den rechten Navigationsbereich können Sie direkt zu einer bestimmten Steuerungszuordnung springen. Viele der zugeordneten Steuerungen werden mit einer [Azure Policy](../../../policy/overview.md)-Initiative implementiert. Zum Anzeigen der vollständigen Initiative öffnen Sie **Richtlinie** im Azure-Portal und wählen dann die Seite **Definitionen** aus. Suchen Sie anschließend die integrierte Richtlinieninitiative **\[Vorschauversion\]: Überprüfen von HIPAA HITRUST-Steuerungen**.

> [!IMPORTANT]
> Jede Steuerung unten ist einer oder mehreren [Azure Policy](../../../policy/overview.md)-Definitionen zugeordnet. Diese Richtlinien können Ihnen bei der [Konformitätsbewertung](../../../policy/how-to/get-compliance-data.md) mit der Steuerung helfen. Es gibt jedoch oft keine 1:1- oder vollständige Übereinstimmung zwischen einer Steuerung und einer bzw. mehreren Richtlinien. Daher bezieht sich **Konform** in Azure Policy nur auf die Richtlinien selbst und gewährleistet nicht die vollständige Konformität mit allen Anforderungen einer Steuerung. Außerdem enthält der Kompatibilitätsstandard Steuerungen, die derzeit von keiner Azure Policy-Definition abgedeckt werden. Daher ist die Konformität in Azure Policy nur eine partielle Ansicht Ihres gesamten Konformitätsstatus. Die Zuordnungen zwischen Steuerungen und Azure Policy-Definitionen für dieses Konformitätsblaupausenbeispiel können sich im Laufe der Zeit ändern. Den Änderungsverlaufs finden Sie im [GitHub-Commit-Verlauf](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md).

## <a name="control-against-malicious-code"></a>Nutzen der Steuerung als Schutz vor Schadcode

Diese Blaupause hilft Ihnen bei der Verwaltung des Endpunktschutzes, einschließlich des Schutzes vor schädlichem Code, indem sie [Azure Policy](../../../policy/overview.md)-Definitionen zuweist, die virtuelle Computer in Azure Security Center auf fehlenden Endpunktschutz überprüfen und die Microsoft-Antischadsoftwarelösung auf virtuellen Windows-Computern erzwingen.

- Microsoft Antimalware für Azure sollte für die automatische Aktualisierung von Schutzsignaturen konfiguriert sein.
- Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen
- Die Endpoint Protection-Lösung sollte für VM-Skalierungsgruppen installiert sein.
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.


## <a name="management-of-removable-media"></a>Verwaltung von Wechselmedien

Die Organisation registriert Medien (einschließlich Laptops) vor der Nutzung basierend auf der Datenklassifizierungsebene, legt angemessene Einschränkungen zur Nutzung dieser Medien fest und richtet einen angemessenen Grad an physischem und logischem Schutz (einschließlich Verschlüsselung) für Medien mit relevanten Informationen ein, bis diese korrekt zerstört oder bereinigt wurden.

- Transparent Data Encryption für SQL-Datenbanken sollte aktiviert werden.
- Die Datenträgerverschlüsselung sollte auf virtuelle Computer angewendet werden.
- Nicht angefügte Datenträger müssen verschlüsselt werden
- Verschlüsselung für Data Lake Store-Konten erforderlich
- Die TDE-Schutzvorrichtung in SQL Server sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.
- Die TDE-Schutzvorrichtung verwalteter SQL-Instanzen sollte mit Ihrem eigenen Schlüssel verschlüsselt werden.

## <a name="control-of-operational-software"></a>Steuerung der Betriebssoftware 

Die Organisation identifiziert nicht autorisierte Software im Informationssystem, z. B. auf Servern, Arbeitsstationen und Laptops, nutzt eine Richtlinie vom Typ „Alles zulassen/Durch Ausnahmen verweigern“, um die Ausführung von bekannter nicht autorisierter Software im Informationssystem zu verbieten, und überprüft und aktualisiert die Liste mit nicht autorisierter Software regelmäßig (mindestens einmal pro Jahr).

- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.
- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="change-control-procedures"></a>Ändern von Steuerungsverfahren

Die Integrität aller VM-Images wird jederzeit sichergestellt, indem alle Änderungen, die an VM-Images vorgenommen werden, protokolliert werden und eine entsprechende Warnung ausgelöst wird. Darüber hinaus werden die Ergebnisse einer Änderung oder Verschiebung und die nachfolgende Überprüfung der Imageintegrität für Geschäftsinhaber bzw. Kunden auf elektronischem Wege (z. B. über Portale oder Warnungen) bereitgestellt.

- \[Vorschau\]: Vorschauüberwachungsergebnisse aus Windows-VM-Konfigurationen in „Systemüberwachungsrichtlinien – Detaillierte Nachverfolgung“ anzeigen
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Systemüberwachungsrichtlinien – Detaillierte Nachverfolgung“ anzeigen

## <a name="control-of-technical-vulnerabilities"></a>Steuerung von technischen Sicherheitsrisiken 

Für alle System- und Netzwerkkomponenten ist ein festgeschriebener Konfigurationsstandard vorhanden.

- Für Ihre SQL Server-Instanzen muss eine Sicherheitsrisikobewertung aktiviert sein.
- Bewertung von Sicherheitsrisiken für verwaltete SQL-Instanzen aktivieren
- \[Vorschau\] Sicherheitsrisikobewertung sollte auf VMs aktiviert sein.
- Sicherheitsrisiken sollten durch eine Lösung zur Sicherheitsrisikobewertung beseitigt werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration für Ihre Computer sollten beseitigt werden.
- Sicherheitsrisiken in der Sicherheitskonfiguration von VM-Skalierungsgruppen sollten beseitigt werden.
- Sicherheitsrisiken in Containersicherheitskonfigurationen sollten behoben werden.
- Sicherheitsrisiken in SQL-Datenbanken sollten beseitigt werden.
- \[Vorschau\]: Podsicherheitsrichtlinien sollten in Kubernetes Services definiert sein.

## <a name="segregation-in-networks"></a>Trennung in Netzwerken

Mit den Sicherheitsgateways der Organisation (z. B. Firewalls) werden Sicherheitsrichtlinien erzwungen, und sie sind für die Filterung von Datenverkehr zwischen Domänen und die Blockierung von unberechtigtem Zugriff konfiguriert. Außerdem werden sie genutzt, um die Trennung zwischen internen kabelgebundenen, internen drahtlosen sowie externen Netzwerksegmenten (z. B. dem Internet), z. B. DMZ, zu verwalten und die Zugriffssteuerungsrichtlinien für die einzelnen Domänen zu erzwingen.

- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden
- Virtuelle Computer müssen mit einem genehmigten virtuellen Netzwerk verbunden sein.
- Virtuellen Computern muss eine Netzwerksicherheitsgruppe zugeordnet sein
- Service Bus sollte einen VNET-Dienstendpunkt verwenden.
- App Service sollte einen VNET-Dienstendpunkt verwenden.
- SQL Server sollte einen VNET-Dienstendpunkt verwenden.
- Event Hub sollte einen VNET-Dienstendpunkt verwenden
- Cosmos DB sollte einen VNET-Dienstendpunkt verwenden
- Key Vault sollte einen VNET-Dienstendpunkt verwenden.
- Gatewaysubnetze dürfen nicht mit einer Netzwerksicherheitsgruppe konfiguriert werden
- Speicherkonten sollten einen VNET-Dienstendpunkt verwenden
- \[Vorschau\]: Container Registry sollte einen VNET-Dienstendpunkt verwenden.
- Empfehlungen zur adaptiven Netzwerkhärtung sollten auf virtuelle Computer mit interner Ausrichtung angewendet werden.

## <a name="network-connection-control"></a>Steuerung von Netzwerkverbindungen

Netzwerkdatenverkehr wird gemäß der Zugriffssteuerungsrichtlinie der Organisation per Firewall und andere netzwerkbezogene Einschränkungen für jeden Netzwerkzugriffspunkt oder die verwaltete Schnittstelle des externen Telekommunikationsdiensts gesteuert.

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- API-App muss neueste TLS-Version verwenden
- In Ihrer Web-App sollte die neueste TLS-Version verwendet werden.
- In Ihrer Funktions-App sollte die neueste TLS-Version verwendet werden.
- Zugriff auf Funktions-App nur über HTTPS gestatten
- Zugriff auf Webanwendung nur über HTTPS gestatten
- Auf API-Apps sollte nur über HTTPS zugegriffen werden können
- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein
- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden
- Subnetze sollten einer Netzwerksicherheitsgruppe zugeordnet werden
- Die NSG-Regeln für Webanwendungen in IaaS sollten verstärkt werden.
- NSG-Regeln für VMs mit Internetzugriff sollten verstärken werden.
- Virtuelle Computer müssen mit einem genehmigten virtuellen Netzwerk verbunden sein.
- Virtuellen Computern muss eine Netzwerksicherheitsgruppe zugeordnet sein

## <a name="network-controls"></a>Netzwerksteuerungen

Die Organisation verwendet geschützte und verschlüsselte Kommunikationskanäle, wenn physische Server, Anwendungen oder Daten zu virtualisierten Servern migriert werden.

- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.
- Empfehlungen zur adaptiven Netzwerkhärtung sollten auf virtuelle Computer mit interner Ausrichtung angewendet werden.
- Service Bus sollte einen VNET-Dienstendpunkt verwenden.
- App Service sollte einen VNET-Dienstendpunkt verwenden.
- SQL Server sollte einen VNET-Dienstendpunkt verwenden.
- Event Hub sollte einen VNET-Dienstendpunkt verwenden
- Cosmos DB sollte einen VNET-Dienstendpunkt verwenden
- Key Vault sollte einen VNET-Dienstendpunkt verwenden.
- Nicht eingeschränkten Netzwerkzugriff auf Speicherkonten überwachen
- Speicherkonten sollten einen VNET-Dienstendpunkt verwenden
- \[Vorschau\]: Container Registry sollte einen VNET-Dienstendpunkt verwenden.

## <a name="security-of-network-services"></a>Sicherheit von Netzwerkdiensten

Zulässige Dienste, die von einem Netzwerkdienstanbieter/Manager bereitgestellt werden, werden formal verwaltet und überwacht, um für die Sicherheit der Bereitstellung zu sorgen.

- \[Vorschau\]: Der Datensammlungs-Agent für Netzwerkdatenverkehr muss auf Windows-VMs installiert werden.
- \[Vorschau\]: Der Datensammlungs-Agent für Netzwerkdatenverkehr muss auf Linux-VMs installiert werden. Network Watcher muss aktiviert sein.

## <a name="information-exchange-policies-and-procedures"></a>Richtlinien und Verfahren für den Informationsaustausch

Die Organisation beschränkt die Verwendung von durch die Organisation gesteuerten Wechselmedien durch autorisierte Personen auf externen Informationssystemen.

- Stellen Sie sicher, dass „Clientzertifikate (eingehende Clientzertifikate)“ für die Web-App auf „Ein“ festgelegt ist.
- CORS sollte nicht jeder Ressource Zugriff auf Ihre Webanwendungen erteilen.
- CORS sollte nicht jeder Ressource Zugriff auf Ihre Funktions-App erteilen.
- Nicht jeder Ressource den Zugriff auf Ihre API-APP über CORS gestatten
- Remotedebuggen sollte für Webanwendungen deaktiviert sein.
- Remotedebuggen sollte für Funktions-Apps deaktiviert sein.
- Remotedebuggen sollte für API-Apps deaktiviert sein.

## <a name="on-line-transactions"></a>Onlinetransaktionen

Die Organisation benötigt die Nutzung von Verschlüsselung zwischen den bzw. den Einsatz von elektronischen Signaturen durch die jeweiligen Beteiligten einer Transkation. Die Organisation stellt sicher, dass die Speicherung der Transaktionsdetails außerhalb von öffentlich zugänglichen Umgebungen erfolgt (z. B. auf einer Speicherplattform im Intranet der Organisation) und dass diese Daten nicht auf einem Speichermedium aufbewahrt und verfügbar gemacht werden, das direkt über das Internet zugänglich ist. Bei Verwendung einer vertrauenswürdigen Autorisierungsstelle (z. B. zum Ausstellen und Verwalten von digitalen Signaturen bzw. Zertifikaten) ist die Sicherheit in den gesamten End-to-End-Prozess zur Verwaltung von Zertifikaten bzw. Signaturen integriert und eingebettet.

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- API-App muss neueste TLS-Version verwenden
- In Ihrer Web-App sollte die neueste TLS-Version verwendet werden.
- In Ihrer Funktions-App sollte die neueste TLS-Version verwendet werden.
- Zugriff auf Funktions-App nur über HTTPS gestatten
- Zugriff auf Webanwendung nur über HTTPS gestatten
- Auf API-Apps sollte nur über HTTPS zugegriffen werden können
- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein
- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden
- \[Vorschau\]: Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, die nicht die angegebenen Zertifikate im vertrauenswürdigen Stamm enthalten
- \[Vorschau\]: Überwachungsergebnisse von Windows-VMs anzeigen, die nicht die angegebenen Zertifikate im vertrauenswürdigen Stamm enthalten

## <a name="user-password-management"></a>Verwaltung von Benutzerkennwörtern

Kennwörter werden während der Übertragung und der Speicherung auf allen Systemkomponenten verschlüsselt.

- \[Vorschau\]: VMs mit unsicheren Einstellungen zur Kennwortsicherheit überwachen

## <a name="user-authentication-for-external-connections"></a>Benutzerauthentifizierung für externe Verbindungen

Sichere Authentifizierungsmethoden, z. B. mehrstufig, Radius oder Kerberos (für privilegierten Zugriff) und CHAP (zur Verschlüsselung von Anmeldeinformationen für Einwahlverfahren), werden für alle externen Verbindungen mit dem Organisationsnetzwerk implementiert.

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibberechtigungen für Ihr Abonnement aktiviert werden.
- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein
- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.

## <a name="user-identification-and-authentication"></a>Identifizierung und Authentifizierung von Benutzern

Benutzer, die privilegierte Funktionen übernommen haben (z. B. Systemadministration), nutzen dabei separate Konten. Verfahren für die mehrstufige Authentifizierung werden in Übereinstimmung mit der organisationsweiten Richtlinie genutzt (z. B. für den Remotezugriff auf Netzwerke).

- MFA sollte für Konten mit Besitzerberechtigungen in Ihrem Abonnement aktiviert sein.
- MFA sollte für Konten mit Schreibberechtigungen für Ihr Abonnement aktiviert werden.
- MFA sollte für Ihre Abonnementkonten mit Leseberechtigungen aktiviert sein
- Maximal 3 Besitzer sollten für Ihr Abonnement festgelegt sein.
- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.
- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, bei denen die Gruppe „Administratoren“ beliebige der angegebenen Mitglieder enthält
- Überwachungsergebnisse von Windows-VMs anzeigen, bei denen die Gruppe „Administratoren“ beliebige der angegebenen Mitglieder enthält
- Erforderliche Komponenten zum Überwachen von Windows-VMs bereitstellen, bei denen die Gruppe „Administratoren“ nicht alle der angegebenen Mitglieder enthält
- Überwachungsergebnisse von Windows-VMs anzeigen, bei denen die Gruppe „Administratoren“ nicht alle der angegebenen Mitglieder enthält
- Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, bei denen die Gruppe „Administratoren“ nicht nur die angegebenen Mitglieder enthält
- Überwachungsergebnisse von Windows-VMs anzeigen, bei denen die Gruppe „Administratoren“ nicht nur die angegebenen Mitglieder enthält

## <a name="privilege-management"></a>Berechtigungsverwaltung

Der Zugriff auf Verwaltungsfunktionen oder -konsolen für Systeme, die virtualisierte Systeme hosten, ist für Mitarbeiter gemäß dem Prinzip der geringsten Rechte eingeschränkt und wird anhand von technischen Steuerungen unterstützt.

- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.
- Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.
- Maximal 3 Besitzer sollten für Ihr Abonnement festgelegt sein.
- Ihrem Abonnement sollte mehr als ein Besitzer zugewiesen sein.
- Externe Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Veraltete Konten mit Besitzerberechtigungen sollten aus Ihrem Abonnement entfernt werden.
- Verwendung benutzerdefinierter RBAC-Regeln überwachen
- Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sollte für Kubernetes Service verwendet werden.

## <a name="review-of-user-access-rights"></a>Überprüfung der Zugriffsrechte für Benutzer

Die Organisation führt eine dokumentierte Liste mit Informationsressourcen für autorisierte Benutzer.

- Verwendung benutzerdefinierter RBAC-Regeln überwachen

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Remotediagnose und Schutz von Konfigurationsports

Ports, Dienste und ähnliche Anwendungen, die auf einem Computer oder in Netzwerksystemen installiert und für den Geschäftsablauf nicht unbedingt erforderlich sind, werden deaktiviert oder entfernt.

- Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.
- Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.
- Remotedebuggen sollte für Webanwendungen deaktiviert sein.
- Remotedebuggen sollte für Funktions-Apps deaktiviert sein.
- Remotedebuggen sollte für API-Apps deaktiviert sein.
- Die adaptive Anwendungssteuerung sollte auf virtuellen Computern aktiviert werden.

## <a name="audit-logging"></a>Überwachungsprotokollierung

Es werden Protokolle mit gesendeten und empfangenen Nachrichten geführt. Sie enthalten Informationen wie Datum, Uhrzeit, Ursprung und Ziel der Nachricht, aber nicht ihren Inhalt. Die Überwachung ist immer verfügbar, während das System aktiv ist, und die folgenden wichtigen Ereignisse werden nachverfolgt: Erfolg/Fehler für Datenzugriff, Änderungen an der Sicherheitskonfiguration des Systems, privilegierte Nutzung oder Hilfsprogrammnutzung, ausgelöste Alarme, Aktivierung/Deaktivierung von Schutzsystemen (z. B. A/V und IDS), Aktivierung/Deaktivierung von Identifikations- und Authentifizierungsmechanismen und Erstellung und Löschung von Objekten auf Systemebene.

- In Azure Data Lake Store sollten Diagnoseprotokolle aktiviert sein.
- In Logic Apps müssen Diagnoseprotokolle aktiviert sein 
- In IoT Hub sollten Diagnoseprotokolle aktiviert sein. 
- Diagnoseprotokolle in Batch-Konten sollten aktiviert sein. 
- In Virtual Machine Scale Sets sollten Diagnoseprotokolle aktiviert sein. 
- In Event Hub sollten Diagnoseprotokolle aktiviert sein 
- In den Suchdiensten müssen Diagnoseprotokolle aktiviert sein 
- In App Services sollten Diagnoseprotokolle aktiviert sein. 
- In Data Lake Analytics sollten Diagnoseprotokolle aktiviert sein. 
- Diagnoseprotokolle in Key Vault sollten aktiviert sein. 
- In Service Bus sollten Diagnoseprotokolle aktiviert sein.
- Diagnoseprotokolle in Azure Stream Analytics sollten aktiviert werden.
- Die Überwachung in SQL Server muss aktiviert werden.
- Überwachen der Diagnoseeinstellung
- Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen

## <a name="monitoring-system-use"></a>Verwendung des Überwachungssystems

Automatisierte Systeme, die in der gesamten Umgebung der Organisation bereitgestellt werden, werden zum Überwachen von wichtigen Ereignissen und anomaler Aktivität sowie zum Analysieren von Systemprotokollen verwendet. Die Ergebnisse werden regelmäßig überprüft. Die Überwachung umfasst privilegierte Vorgänge, autorisierte und nicht autorisierte Zugriffsversuche, z. B. versuchte Zugriffe auf deaktivierte Konten, und Systemwarnungen oder -ausfälle.

- Azure Monitor muss Aktivitätsprotokolle aus allen Regionen erfassen
- Der Log Analytics-Agent sollte auf virtuellen Computern installiert werden.
- Der Log Analytics-Agent sollte in Virtual Machine Scale Sets installiert sein.
- \[Vorschau\]: Voraussetzungen für die Überwachung von Windows-VMs bereitstellen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist
- \[Vorschau\]: Überwachungsergebnisse von Windows-VMs anzeigen, auf denen der Log Analytics-Agent nicht wie erwartet verbunden ist
- Das Azure Monitor-Protokollprofil muss Protokolle für die Kategorien „write“, „delete“ und „action“ erfassen
- Für Ihr Abonnement muss die automatische Bereitstellung des Log Analytics-Überwachungs-Agents aktiviert sein

## <a name="segregation-of-duties"></a>Aufgabentrennung

Die Aufgabentrennung wird verwendet, um das Risiko einer nicht autorisierten oder unbeabsichtigten Änderung von Informationen und Systemen einzuschränken. Einzelpersonen können ohne Autorisierung oder Erkennung nicht auf Informationssysteme zugreifen und diese nicht ändern oder nutzen. Der Zugriff für Personen, die für die Verwaltung und den Zugriff auf Steuerungen zuständig sind, ist je nach Rolle und Verantwortungsbereich des jeweiligen Benutzers auf das erforderliche Minimum beschränkt. Diese Personen können nicht auf Überwachungsfunktionen für diese Steuerungen zugreifen.

- Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) sollte für Kubernetes Service verwendet werden.
- Verwendung benutzerdefinierter RBAC-Regeln überwachen
- \[Vorschau\]: Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Zuweisen von Benutzerrechten“ bereitstellen
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Zuweisen von Benutzerrechten“ anzeigen
- \[Vorschau\]: Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Sicherheitsoptionen – Benutzerkontensteuerung“ bereitstellen
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Benutzerkontensteuerung“ anzeigen
- Es dürfen keine benutzerdefinierten Abonnementbesitzerrollen vorhanden sein.

## <a name="administrator-and-operator-logs"></a>Administrator- und Bedienerprotokolle

Die Organisation stellt sicher, dass die korrekte Protokollierung aktiviert ist, um Administratoraktivitäten zu überwachen. Die Systemadministrator- und Bedienerprotokolle werden regelmäßig überprüft.

- Für bestimmte administrative Vorgänge muss eine Aktivitätsprotokollwarnung vorliegen

## <a name="identification-of-risks-related-to-external-parties"></a>Identifizierung von Risiken in Bezug auf externe Parteien

Remotezugriffsverbindungen zwischen der Organisation und externen Parteien werden verschlüsselt

- Sichere Übertragung in Speicherkonten sollte aktiviert werden.
- Zugriff auf Funktions-App nur über HTTPS gestatten
- Zugriff auf Webanwendung nur über HTTPS gestatten
- Auf API-Apps sollte nur über HTTPS zugegriffen werden können
- Erzwingen einer SSL-Verbindung muss für MySQL-Datenbankserver aktiviert sein
- Erzwingen einer SSL-Verbindung muss für PostgreSQL-Datenbankserver aktiviert sein
- Nur sichere Verbindungen mit Ihrer Redis Cache-Instanz sollten aktiviert werden

## <a name="business-continuity-and-risk-assessment"></a>Geschäftskontinuität und Risikobewertung

Die Organisation identifiziert ihre kritischen Geschäftsprozesse und integriert die Verwaltungsanforderungen für die Informationssicherheit in Bezug auf die Geschäftskontinuität in andere Kontinuitätsanforderungen. Hierbei geht es beispielsweise um Aspekte wie operativer Betrieb, Personalwesen, Material, Transport und Einrichtungen.

- Überwachen von virtuellen Computern ohne konfigurierte Notfallwiederherstellung
- Key Vault-Objekte müssen wiederherstellbar sein
- \[Vorschau\]: Voraussetzungen für die Überwachung von Windows-VM-Konfigurationen in „Sicherheitsoptionen – Wiederherstellungskonsole“ bereitstellen
- \[Vorschau\]: Überwachungsergebnisse aus Windows-VM-Konfigurationen in „Sicherheitsoptionen – Wiederherstellungskonsole“ anzeigen

## <a name="back-up"></a>Sichern

Mit dieser Blaupause werden Azure Policy-Definitionen zugewiesen, mit denen die Systemsicherungsinformationen der Organisation am alternativen Speicherstandort elektronisch überwacht werden. Erwägen Sie für den physischen Versand der Speichermetadaten die Verwendung von Azure Data Box.

- Langfristige georedundante Sicherung muss für Azure SQL-Datenbank aktiviert sein
- Georedundante Sicherung muss für Azure Database for MySQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for PostgreSQL aktiviert sein
- Georedundante Sicherung muss für Azure Database for MariaDB aktiviert sein
- Azure Backup muss für Virtual Machines aktiviert sein.

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
