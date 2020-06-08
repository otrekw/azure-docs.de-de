---
title: Archiv der Neuerungen in Azure Security Center
description: Enthält eine Beschreibung der Neuerungen und Änderungen in Azure Security Center, die älter als sechs Monate sind.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2020
ms.author: memildin
ms.openlocfilehash: 253ea748462ec09cd9cf4eab14c05be678cfb82d
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860901"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Archiv der Neuerungen in Azure Security Center

Die Hauptseite mit Versionshinweisen zu [Neuerungen in Azure Security Center](release-notes.md) enthält Aktualisierungen aus den letzten sechs Monaten. Ältere Einträge finden Sie hingegen auf dieser Seite.

Auf dieser Seite finden Sie folgende Informationen:

- Neue Funktionen
- Behebung von Programmfehlern
- Veraltete Funktionen

## <a name="november-2019"></a>November 2019

### <a name="threat-protection-for-azure-key-vault-in-public-preview-in-north-america-regions"></a>Threat Protection für Azure Key Vault jetzt in Nordamerika als öffentliche Vorschauversion verfügbar

Azure Key Vault ist ein zum Schutz von Daten und zur Verbesserung der Leistung von Cloudanwendungen unabdingbarer Dienst, der die Möglichkeit bietet, Schlüssel, Geheimnisse, Kryptografieschlüssel und Richtlinien zentral in der Cloud zu verwalten. Da in Azure Key Vault vertrauliche und geschäftskritische Daten gespeichert werden, ist für die Schlüsseltresore und die darin gespeicherten Daten höchste Sicherheit erforderlich.

Die Unterstützung des Bedrohungsschutzes für Azure Storage in Azure Security Center bietet eine zusätzliche Ebene intelligenter Sicherheitsfunktionen, die ungewöhnliche und möglicherweise schädliche Versuche erkennen, auf Schlüsseltresore zuzugreifen und diese missbräuchlich zu nutzen. Aufgrund dieser neuen Schutzebene können Kunden auch ohne Sicherheitsexpertise oder die Verwaltung von Sicherheitsüberwachungssystemen effektiv auf Bedrohungen ihrer Schlüsseltresore reagieren. Dieses Feature ist in Nordamerika als öffentliche Vorschauversion verfügbar.


### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>Threat Protection für Azure Storage umfasst zuverlässiges Malwarescreening

Threat Protection für Azure Storage bietet mithilfe der Analyse für Hashbewertung und verdächtige Zugriffsmuster von einem aktiven Tor-Exitknoten (anonymisierender Proxy) von Microsoft Threat Intelligence neue Funktionen zum Erkennen von Malwareuploads in Azure Storage. Sie können jetzt mithilfe des Azure Security Center in Speicherkonten entdeckte Malware anzeigen lassen.


### <a name="workflow-automation-with-logic-apps-preview"></a>Workflowautomatisierung mit Azure Logic Apps (Vorschauversion)

Organisationen mit zentral verwalteten Sicherheits- und IT-Abläufen implementieren interne Workflowprozesse, um erforderliche Aktionen innerhalb der Organisation voranzutreiben, wenn Abweichungen in der Umgebung entdeckt werden. In vielen Fällen handelt es sich bei diesen Workflows um wiederholbare Prozesse, und durch die Automatisierung können Prozesse in der Organisation deutlich optimiert werden.

Wir führen heute eine neue Funktion in Security Center ein, die Kunden unter Verwendung von Azure Logic Apps das Erstellen von Automatisierungskonfigurationen und Richtlinien ermöglicht, die basierend auf bestimmten ASC-Ergebnissen wie beispielsweise Empfehlungen oder Warnungen automatisch ausgelöst werden. Azure Logic Apps kann so konfiguriert werden, dass eine beliebige von der großen Logic App-Connectors-Community unterstützte benutzerdefinierte Aktion ausgeführt oder eine der von Security Center bereitgestellten Vorlagen wie das Senden einer E-Mail oder das Öffnen eines ServiceNow™-Tickets verwendet wird.

Weitere Informationen zu den automatischen und manuellen Security Center-Funktionen zum Ausführen von Workflows finden Sie unter [Workflowautomatisierung](workflow-automation.md).

Informationen zum Erstellen von Logic Apps finden Sie unter [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).


### <a name="quick-fix-for-bulk-resources-generally-available"></a>Ressourcenübergreifendes schnelle Problembehebung allgemein verfügbar

Die Sicherheitsbewertung kann sehr umfangreich sein, wodurch es schwierig wird, Probleme ressourcenübergreifend zu beheben.

Mithilfe der schnellen Problembehebung werden die Vorgänge zum Beheben von fehlerhaften Sicherheitskonfigurationen vereinfacht, und Sie können schnell Empfehlungen ressourcenübergreifend umsetzen und Ihre Sicherheitsbewertung verbessern.

Mit dieser Vorgehensweise können Sie die Ressourcen auswählen, auf die Sie die Wartung anwenden möchten, und dann die Wartungsaktion starten. Die Konfiguration erfolgt automatisch, ohne dass Sie eingreifen müssen.

Die schnelle Problembehebung steht Kunden ab heute über die Seite „Security Center-Empfehlungen“ zur Verfügung.

Im [Referenzhandbuch für Sicherheitsempfehlungen](recommendations-reference.md) erfahren Sie, für welche Empfehlungen die schnelle Problembehebung aktiviert ist.


### <a name="scan-container-images-for-vulnerabilities-preview"></a>Überprüfen von Containerimages auf Sicherheitsrisiken (Vorschauversion)

Azure Security Center kann nun Containerimages in Azure Container Registry auf Sicherheitsrisiken überprüfen.

Bei der Überprüfung von Images wird die Containerimagedatei analysiert und anschließend auf bekannte Sicherheitsrisiken überprüft (unterstützt durch Qualys).

Die Überprüfung selbst wird automatisch ausgelöst, wenn neue Containerimages per Push an Azure Container Registry übertragen werden. Erkannte Sicherheitsrisiken werden als Security Center-Empfehlungen angezeigt und sind zusammen mit Informationen zum Patchen dieser Risiken zur Reduzierung der gebotenen Angriffsfläche in der Azure-Sicherheitsbewertung enthalten.


### <a name="additional-regulatory-compliance-standards-preview"></a>Zusätzliche Standards zur Einhaltung gesetzlicher Bestimmungen (Vorschauversion)

Das Dashboard „Einhaltung gesetzlicher Bestimmungen“ bietet Erkenntnisse über Ihren Konformitätsstatus, die auf Security Center-Bewertungen basieren. Das Dashboard zeigt, inwieweit Ihre Umgebung mit den Steuerelementen und Anforderungen bestimmter gesetzlicher Standards und Branchenbenchmarks übereinstimmt und bietet ausführliche Empfehlungen zum Erfüllen dieser Anforderungen.

Das Dashboard „Einhaltung gesetzlicher Bestimmungen“ verfügt daher über vier integrierte und unterstützte Standards:  Azure CIS 1.1.0, PCI-DSS, ISO 27001 und SOC-TSP. Wir künden hiermit die öffentliche Vorschauversion zusätzlicher unterstützter Standards an: NIST SP 800-53 R4, SWIFT CSP CSCF v2020, Canada Federal PBMM und UK Official zusammen mit UK NHS. Außerdem veröffentlichen wir eine aktualisierte Version von Azure CIS 1.1.0, in der weitere Steuerelemente der optimierten und der Standarderweiterbarkeit verfügbar sind.

[Weitere Informationen über das Anpassen der Standards in Ihrem Dashboard für die Einhaltung gesetzlicher Bestimmungen](update-regulatory-compliance-packages.md)


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>Threat Protection-Unterstützung für Azure Kubernetes Service (AKS) (Vorschauversion)

Kubernetes entwickelt sich in Rekordzeit zum neuen Standard für die Softwarebereitstellung und -verwaltung in der Cloud. Derzeit gibt es noch wenige Benutzer, die umfassende Erfahrungen mit Kubernetes vorweisen können. Viele konzentrieren sich auf die allgemeine Entwicklung und Verwaltung und vernachlässigen dabei den Sicherheitsaspekt. Eine Kubernetes-Umgebung muss sorgfältig konfiguriert werden, damit sie wirklich sicher ist. So wird sichergestellt, dass keine Angriffsfläche für Angriffe auf Container geboten wird. Die Unterstützung für den Containerbereich in Security Center wird um einen der am schnellsten wachsenden Dienst in Azure ausgebaut: Azure Kubernetes Service (AKS).

Die öffentliche Vorschauversion umfasst folgende Funktionen:

- **Ermittlung und Sichtbarkeit:** Continuous Discovery für verwaltete AKS-Instanzen in den registrierten Security Center-Abonnements
- **Secure Score-Empfehlungen:** nützliche Hinweise, mit denen Kunden die Best Practices für Sicherheit in AKS im Rahmen des Secure Score eines Kunden einhalten können (z. B. „Die rollenbasierte Zugriffssteuerung sollte verwendet werden, um den Zugriff auf Kubernetes Service-Cluster einzuschränken.“)
- **Bedrohungserkennung:** host- und clusterbasierte Analysen (z. B. „Ein privilegierter Container wurde erkannt.“)


### <a name="virtual-machine-vulnerability-assessment-preview"></a>Sicherheitsrisikobewertung für virtuelle Computer (Vorschauversion)

Auf virtuellen Computern installierte Anwendungen sind oft anfällig für Sicherheitsrisiken, die zu einer Kompromittierung des gesamten virtuellen Computers führen können. Heute geben wir bekannt, dass der Security Center-Standard-Tarif eine kostenlose integrierte Sicherheitsrisikobewertung für virtuelle Computer enthält. Die Sicherheitsrisikobewertung in der öffentlichen Vorschauversion basiert auf Qualys und ermöglicht das kontinuierliche Scannen aller installierten Anwendungen auf einem virtuellen Computer, um anfällige Anwendungen zu finden und die Ergebnisse auf der Benutzeroberfläche im Security Center-Portal anzuzeigen. Das Security Center berücksichtigt dabei alle Bereitstellungsvorgänge, sodass der Benutzer keine weiteren Maßnahmen ergreifen muss. Für die Zukunft planen wir die Bereitstellung von Sicherheitsrisikobewertungsoptionen, um die individuellen Geschäftsanforderungen unserer Kunden zu erfüllen.

[Weitere Informationen über Sicherheitsrisikobewertungen für Ihre virtuellen Azure-Computer](security-center-vulnerability-assessment-recommendations.md)


### <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Advanced Data Security für SQL-Server auf Azure-VMs (Vorschauversion)

Die Unterstützung von Azure Security Center für die Bedrohungsschutz- und Sicherheitsrisikobewertung für SQL-Datenbanken, die auf IaaS-VMs ausgeführt werden, befindet sich jetzt in der Vorschauphase.

[Sicherheitsrisikobewertung](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) ist ein einfach zu konfigurierender Dienst, mit dem potenzielle Schwachstellen in der Datenbank ermittelt, nachverfolgt und behoben werden können. Er bietet Einblicke in Ihren Sicherheitsstatus als Teil der Azure-Sicherheitsbewertung, enthält die Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.

[Advanced Threat Protection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) erkennt Anomalien bei Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hinweisen, auf SQL Server zuzugreifen oder diesen zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und stellt handlungsorientierte Sicherheitswarnungen bei anomalen Datenbankzugriffsmustern bereit. Diese Warnungen enthalten Details zur verdächtigen Aktivität sowie empfohlene Maßnahmen zur Untersuchung und Abwehr der Bedrohung.


### <a name="support-for-custom-policies-preview"></a>Unterstützung für benutzerdefinierte Richtlinien (Vorschauversion)

Azure Security Center unterstützt jetzt benutzerdefinierte Richtlinien (Vorschauphase).

Unsere Kunden haben sich gewünscht, die Abdeckung ihrer aktuellen Sicherheitsbewertungen in Security Center um ihre eigenen Sicherheitsbewertungen zu erweitern, die auf Richtlinien basieren, die sie in Azure Policy erstellen. Da nun benutzerdefinierte Richtlinien unterstützt werden, ist dies jetzt möglich.

Diese neuen Richtlinien sollen Teil der Security Center-Empfehlungen, Sicherheitsbewertung und dem Dashboard für Standards zur Einhaltung gesetzlicher Bestimmungen werden. Durch die Unterstützung benutzerdefinierter Richtlinien können Sie jetzt benutzerdefinierte Initiativen in Azure Policy erstellen, diese als Richtlinien zu Security Center hinzufügen und anschließend als Empfehlungen visualisieren.


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>Erweitertes Azure Security Center-Angebot: Plattform für die Community und Partner

Verwenden Sie Security Center, um Empfehlungen nicht nur von Microsoft, sondern auch aus bestehenden Lösungen von Partnern wie Check Point, Tenable und CyberArk zu erhalten, wobei viele weitere Integrationen vorgesehen sind.  Der einfache Security Center-Onboardingflow ermöglicht das Verbinden Ihrer vorhandenen Lösungen mit Security Center, sodass Sie alle Sicherheitsstatusempfehlungen an einem Ort anzeigen, vereinheitlichte Berichte ausführen und alle Security Center-Funktionen sowohl für integrierte als auch Partnerempfehlungen nutzen können. Sie können Security Center-Empfehlungen auch für Partnerprodukte exportieren.

[Weitere Informationen zur Microsoft Intelligent Security Association](https://www.microsoft.com/security/partnerships/intelligent-security-association)



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>Erweiterte Integration über den Export von Security Center-Empfehlungen und -Warnungen (Vorschauversion)

Für Szenarios auf Unternehmensebene, die auf Security Center basieren, können jetzt Warnungen und Empfehlungen von Security Center auch außerhalb des Azure-Portals oder der Azure-API verarbeitet werden. Sie können sie direkt in einen Event Hub oder in Log Analytics-Arbeitsbereiche exportieren. Hier finden Sie Beispiele für Workflows, die Sie mithilfe dieser neuen Funktionen erstellen können:

- Durch den Export in Log Analytics-Arbeitsbereiche können Sie mithilfe von Power BI benutzerdefinierte Dashboards erstellen.
- Durch den Export in einen Event Hub können Sie Warnungen und Empfehlungen für Security Center in SIEM-Produkte von Drittanbietern, in Echtzeit in Lösungen von Drittanbietern oder in Azure Data Explorer exportieren.


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>Onboarding lokaler Server in Security Center über Windows Admin Center (Vorschauversion)

Windows Admin Center ist ein Verwaltungsportal für Windows-Server, die nicht in Azure bereitgestellt wurden, und bietet diesen mehrere Azure-Verwaltungsfunktionen wie Sicherungen und Systemupdates. Wir haben kürzlich eine Funktion zum Onboarding dieser Nicht-Azure-Server hinzugefügt, damit diese direkt über Windows Admin Center vom Azure Security Center geschützt werden.

Durch diese neue Funktion können Benutzer für einen WAC-Server das Onboarding für Azure Security Center durchführen und die Anzeige dessen Sicherheitswarnungen und -empfehlungen direkt über Windows Admin Center aktivieren.


## <a name="september-2019"></a>September 2019

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>Verbesserte Verwaltung von Regeln mit der adaptiven Anwendungssteuerung

Die Funktion zum Verwalten von Regeln für virtuelle Computer mithilfe der adaptiven Anwendungssteuerung wurde verbessert. Die adaptive Anwendungssteuerung ist ein Feature von Azure Security Center, mit dem Sie steuern können, welche Anwendungen auf Ihren virtuellen Computern ausgeführt werden können. Neben der allgemeinen Verbesserung der Regelverwaltung können Sie dank einer neuen Funktion nun steuern, welche Dateitypen geschützt werden, wenn Sie eine neue Regel hinzufügen.

[Weitere Informationen über Adaptive Anwendungssteuerungen](security-center-adaptive-application.md)


### <a name="control-container-security-recommendation-using-azure-policy"></a>Steuern von Empfehlungen für die Containersicherheit mit Azure Policy

Die Empfehlungen von Azure Security Center zum Korrigieren von Sicherheitsrisiken bei Containern können nun mit Azure Policy aktiviert und deaktiviert werden.

Öffnen Sie zum Anzeigen der von Ihnen aktivierten Sicherheitsrichtlinien in Security Center die Seite „Sicherheitsrichtlinie“.


## <a name="august-2019"></a>August 2019

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>Just-In-Time-Zugriff (JIT) auf virtuelle Computer für Azure Firewall 

Just-In-Time-Zugriff (JIT) auf virtuelle Computer für Azure Firewall ist jetzt allgemein verfügbar. Schützen Sie damit Ihre durch Azure Firewall geschützten Umgebungen zusätzlich zu Ihren durch Netzwerksicherheitsgruppen (NSG) geschützten Umgebungen.

JIT-VM-Zugriff sorgt für ein geringeres Risiko durch volumetrische Netzwerkangriffe, da mit dieser Steuerung nur der wirklich erforderliche Zugriff auf Ihre VMs gewährt wird und dabei Ihre NSG- und Azure Firewall-Regeln angewandt werden.

Beim Aktivieren von JIT für Ihre VMs erstellen Sie eine Richtlinie, die festlegt, welche Ports geschützt werden sollen, wie lange die Ports geöffnet sein sollen und von welchen genehmigten IP-Adressen aus auf diese Ports zugegriffen werden kann. Mit dieser Richtlinie behalten Sie volle Kontrolle darüber, was Ihre Benutzer ausführen dürfen, wenn sie Zugriff anfordern.

Anforderungen werden im Azure-Aktivitätsprotokoll erfasst, sodass Sie den Zugriff ganz einfach überwachen und überprüfen können. Außerdem können Sie auf der JIT-Seite sehr schnell ermitteln, auf welchen vorhandenen VMs JIT aktiviert ist und auf welchen JIT empfohlen wird.

[Weitere Informationen über Azure Firewall](https://docs.microsoft.com/azure/firewall/overview)


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>Ein-Klick-Wartung zur Verbesserung Ihres Sicherheitsstatus (Vorschauversion)

Secure Score ist ein Tool, mit dem Sie den Sicherheitsstatus bewerten können. Es überprüft Ihre Sicherheitsempfehlungen und priorisiert sie für Sie, damit Sie wissen, welche Empfehlungen Sie zuerst durchführen sollten. Dies hilft Ihnen, die schwerwiegendsten Sicherheitsrisiken zu finden, um die Untersuchung priorisieren zu können.

Zur Vereinfachung der Wartung von Fehlkonfigurationen bei der Sicherheit und für eine schnelle Verbesserung Ihrer Sicherheitsbewertung haben wir außerdem eine neue Funktion hinzugefügt, mit der Sie einzelne Empfehlungen mit nur einem Klick für einen ganzen Stapel von Ressourcen ausführen können.

Mit dieser Vorgehensweise können Sie die Ressourcen auswählen, auf die Sie die Wartung anwenden möchten, und dann die Wartungsaktion starten. Die Konfiguration erfolgt automatisch, ohne dass Sie eingreifen müssen.

Im [Referenzhandbuch für Sicherheitsempfehlungen](recommendations-reference.md) erfahren Sie, für welche Empfehlungen die schnelle Problembehebung aktiviert ist.


### <a name="cross-tenant-management"></a>Mandantenübergreifende Verwaltung

Security Center unterstützt jetzt Szenarien mit mandantenübergreifender Verwaltung als Teil von Azure Lighthouse. Dadurch können Sie in Security Center mehrere Mandanten einsehen und ihren Sicherheitsstatus verwalten. 

[Weitere Informationen über mandantenübergreifende Verwaltungsmöglichkeiten](security-center-cross-tenant-management.md)


## <a name="july-2019"></a>Juli 2019

### <a name="updates-to-network-recommendations"></a>Updates für Netzwerkempfehlungen

Azure Security Center (ASC) hat neue Netzwerkempfehlungen veröffentlicht und bestehende verbessert. Damit trägt Security Center noch mehr zum Schutz Ihres Netzwerks und Ihrer Ressourcen bei. 

[Weitere Informationen über Netzwerkempfehlungen](recommendations-reference.md#recs-network)


## <a name="june-2019"></a>Juni 2019

### <a name="adaptive-network-hardening---generally-available"></a>Adaptive Netzwerkhärtung allgemein verfügbar

Eine der größten Angriffsflächen für Workloads, die in der öffentlichen Cloud ausgeführt werden, sind Verbindungen mit und aus dem öffentlichen Internet. Unsere Kunden finden es schwierig zu entscheiden, welche Netzwerksicherheitsgruppenregeln gelten sollten, damit Azure-Workloads auch wirklich nur für die erforderlichen Quellbereiche verfügbar sind. Mit diesem Feature werden der Netzwerkdatenverkehr und Verbindungsmuster von Azure-Workloads von Security Center analysiert und Empfehlungen für Netzwerksicherheitsgruppenregeln für VMs gemacht, die mit dem Internet verbunden sind. Dadurch können Kunden ihre Netzwerkzugriffsrichtlinien besser konfigurieren und die Anfälligkeit für Angriffe minimieren. 

[Weitere Informationen über die adaptive Netzwerkhärtung](security-center-adaptive-network-hardening.md)
