---
title: Neues in der Azure Monitor-Dokumentation
description: Wichtige Aktualisierungen für die Azure Monitor-Dokumentation (monatlich aktualisiert).
ms.subservice: ''
ms.topic: overview
author: bwren
ms.author: bwren
ms.date: 03/05/2020
ms.openlocfilehash: c29790035ec4e971957784e826a1e8bd8e0c9329
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "79500502"
---
# <a name="whats-new-in-azure-monitor-documentation"></a>Neues in der Azure Monitor-Dokumentation
Dieser Artikel enthält Listen mit neuen oder signifikant aktualisierten Azure Monitor-Artikeln. Er wird jeweils in der ersten Monatswoche mit Artikelaktualisierungen aus dem Vormonat aktualisiert.

## <a name="february-2020"></a>Februar 2020

### <a name="agents"></a>Agents
Mehrere Updates im Rahmen der Überarbeitung des Inhalts der Diagnoseerweiterung.

- [Übersicht über Azure Monitor-Agents](platform/agents-overview.md): Die Tabellen wurden neu strukturiert, um die individuellen Features der einzelnen Agents deutlicher zu machen.
- [Übersicht zur Azure-Diagnoseerweiterung](platform/diagnostics-extension-overview.md): Vollständig überarbeitet.
- [Sammeln von Daten aus der Azure-Diagnoseerweiterung in Azure Monitor-Protokollen](platform/diagnostics-extension-logs.md): Zur Aktualisierung und Verdeutlichung allgemein überarbeitet.
- [Installieren und Konfigurieren der Microsoft Azure-Diagnoseerweiterung (WAD)](platform/diagnostics-extension-windows-install.md): Neuer Artikel. 
- [Schema der Diagnoseerweiterung für Windows](platform/diagnostics-extension-schema-windows.md): Neu strukturiert.
- [Senden von Daten aus der Windows Azure-Diagnoseerweiterung an Azure Event Hubs](platform/diagnostics-extension-stream-event-hubs.md): Vollständig überarbeitet und aktualisiert.
- [Speichern und Anzeigen von Diagnosedaten in Azure Storage](/azure/cloud-services/diagnostics-extension-to-storage) Vollständig überarbeitet und aktualisiert.
- [Log Analytics-VM-Erweiterung für Windows](../virtual-machines/extensions/oms-windows.md): Verdeutlichung der Beziehung mit dem Log Analytics-Agent.
- [Log Analytics-VM-Erweiterung für Linux](../virtual-machines/extensions/oms-linux.md): Verdeutlichung der Beziehung mit dem Log Analytics-Agent.




### <a name="application-insights"></a>Application Insights
- [Verbindungszeichenfolgen](app/sdk-connection-string.md): Neuer Artikel.

### <a name="insights-and-solutions"></a>Erkenntnisse und Lösungen

#### <a name="azure-monitor-for-containers"></a>Azure Monitor für Container
- [Integrieren von Azure Active Directory in Azure Kubernetes Service](../aks/azure-ad-integration.md): Es wurde ein Hinweis für die Erstellung einer Clientanwendung zur Unterstützung eines RBAC-fähigen Clusters für die Unterstützung von Azure Monitor für Container hinzugefügt.

#### <a name="azure-monitor-for-vms"></a>Azure Monitor für VMs
- [Häufig gestellte Fragen zu Azure Monitor für VMs in der allgemein verfügbaren Version (GA)](insights/vminsights-ga-release-faq.md): Die Informationen zur Speicherung der Leistungsdaten wurden geändert.

#### <a name="office-365"></a>Office 365
- [Office 365-Verwaltungslösung in Azure (Vorschau)](insights/solution-office-365.md): Das Veraltungsdatum wurde aktualisiert.


### <a name="logs"></a>Protokolle
- [Optimieren von Protokollabfragen in Azure Monitor](log-query/query-optimization.md): Neuer Artikel.
- [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](platform/manage-cost-storage.md): Verbesserte Beispielabfragen zum besseren Verständnis Ihrer Nutzung.

### <a name="metrics"></a>metrics
- [Über Diagnoseeinstellungen exportierbare Azure Monitor-Plattformmetriken](platform/metrics-supported-export-diagnostic-settings.md): Ein Abschnitt zum Ändern des Verhaltens für NULL-Werte und Nullwerte wurde hinzugefügt.


### <a name="visualizations"></a>Visualisierungen
Mehrere neue Artikel zur Umstellung vom Ansicht-Designer auf Arbeitsmappen.

- [Handbuch für den Übergang vom Azure Monitor-Ansicht-Designer zu Arbeitsmappen](platform/view-designer-conversion-overview.md): Neuer Artikel.
- [Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – Optionen](platform/view-designer-conversion-options.md): Neuer Artikel.
- [Umwandlung von Azure Monitor-Ansichts-Designer-Kacheln](platform/view-designer-conversion-tiles.md): Neuer Artikel.
- [Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – Zusammenfassung und Zugriff](platform/view-designer-conversion-access.md): Neuer Artikel.
- [Umwandlung von Ansichts-Designer in Arbeitsmappen – häufige Aufgaben](platform/view-designer-conversion-tasks.md): Neuer Artikel.
- [Beispiele für die Ansicht-Designer-Umwandlung](platform/view-designer-conversion-examples.md): Neuer Artikel.




## <a name="january-2020"></a>Januar 2020

### <a name="general"></a>Allgemein
- [Was wird von Azure Monitor überwacht?](monitor-reference.md): Neuer Artikel.

### <a name="agents"></a>Agents
- [Sammeln von Protokolldaten mit dem Log Analytics-Agent](platform/log-analytics-agent.md): Die Tabelle mit den Netzwerkfirewallanforderungen wurde aktualisiert.


### <a name="alerts"></a>Alerts
- [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](platform/action-groups.md): Eine nicht mehr benötigte Einstellung für Funktionen der Version 2 wurde entfernt.
- [Erstellen einer Metrikwarnung anhand einer Resource Manager-Vorlage](platform/alerts-metric-create-templates.md): Ein Beispiel für den Parameter *ignoreDataBefore* wurde hinzugefügt.  Einschränkungen für Regeln mit mehreren Kriterien wurden hinzugefügt.
- [Erstellen und Verwalten von Warnungsregeln in Log Analytics mithilfe der REST-API](platform/api-alerts.md): Das JSON-Beispiel wurde korrigiert.


### <a name="application-insights"></a>Application Insights
- [Von Application Insights und Log Analytics verwendete IP-Adressen](app/ip-addresses.md): Dem Abschnitt „Verfügbarkeitstests“ wurde eine Anleitung zum Hinzufügen einer Eingangsportregel hinzugefügt, um Datenverkehr von Application Insights-Verfügbarkeitstests zuzulassen.
- [Behandeln von Problemen mit dem Aktivieren oder Anzeigen von Application Insights Profiler](app/profiler-troubleshooting.md): Die allgemeine Problembehandlung wurde aktualisiert.
- [Erstellen von Stichproben in Application Insights](app/sampling.md): Der Artikel wurde auf der Grundlage von Kundenfeedback aktualisiert und neu strukturiert, um die Lesbarkeit zu verbessern.


### <a name="data-security"></a>Datensicherheit
- [Konfiguration kundenseitig verwalteter Schlüssel in Azure Monitor](platform/customer-managed-keys.md): Neuer Artikel.

### <a name="insights-and-solutions"></a>Erkenntnisse und Lösungen

#### <a name="azure-monitor-for-containers"></a>Azure Monitor für Container
- [Konfigurieren der Datensammlung des Azure Monitor für Container-Agents](insights/container-insights-agent-config.md): In diesem Artikel wurden Details zum Upgraden des Agents auf Azure Red Hat OpenShift sowie zusätzliche Informationen zur Unterscheidung der Methoden zum Upgraden des Agents hinzugefügt.
- [Einrichten von Warnungen für Leistungsprobleme in Azure Monitor für Container](insights/container-insights-alerts.md): In diesem Artikel wurden Informationen überarbeitet und die Schritte aktualisiert, mit denen unter Verwendung von Warnungen für den Arbeitsbereichskontext eine Warnung für im Arbeitsbereich gespeicherte Leistungsdaten erstellt wird.
- [Überwachen der Leistung von Kubernetes-Clustern mit Azure Monitor für Container](insights/container-insights-analyze.md): In diesem Artikel wurde sowohl die Übersicht als auch der Analyseabschnitt im Hinblick auf die Unterstützung von Windows-Kubernetes-Clustern aktualisiert.
- [Konfigurieren von Azure Red Hat OpenShift-Clustern mit Azure Monitor für Container](insights/container-insights-azure-redhat-setup.md): In diesem Artikel wurden Details zum Upgraden des Agents auf Azure Red Hat OpenShift sowie zusätzliche Informationen zur Unterscheidung der Methoden zum Upgraden des Agents hinzugefügt.
- [Konfigurieren von Kubernetes-Hybridclustern mit Azure Monitor für Container](insights/container-insights-hybrid-setup.md): Dieser Artikel wurde mit der zusätzlichen Unterstützung von „secure port: 10250“ für cAdvisor von Kubelet aktualisiert.
- [Verwalten des Agents für Azure Monitor für Container](insights/container-insights-manage-agent.md): In diesem Artikel wurden Details im Zusammenhang mit dem Verhalten und der Konfiguration der Metrikerfassung mit Azure Red Hat OpenShift im Vergleich zu anderen Arten von Kubernetes-Clustern aktualisiert.
- [Konfigurieren des Abrufs von Prometheus-Metriken mit Azure Monitor für Container](insights/container-insights-prometheus-integration.md): In diesem Artikel wurden Details im Zusammenhang mit dem Verhalten und der Konfiguration der Metrikerfassung mit Azure Red Hat OpenShift im Vergleich zu anderen Arten von Kubernetes-Clustern aktualisiert.
- [Gewusst wie: Aktualisieren von Azure Monitor für Container zum Aktivieren von Metriken](insights/container-insights-update-metrics.md): In diesem Artikel wurden Details im Zusammenhang mit dem Verhalten und der Konfiguration der Metrikerfassung mit Azure Red Hat OpenShift im Vergleich zu anderen Arten von Kubernetes-Clustern aktualisiert.


#### <a name="azure-monitor-for-vms"></a>Azure Monitor für VMs
- [Häufig gestellte Fragen zu Azure Monitor für VMs in der allgemein verfügbaren Version (GA)](insights/vminsights-ga-release-faq.md): In diesem Artikel wurden Informationen zum Upgraden des Arbeitsbereichs und der Agents auf die neue Version hinzugefügt.

#### <a name="office-365"></a>Office 365
- [Office 365-Verwaltungslösung in Azure (Vorschau)](insights/solution-office-365.md): In diesem Artikel wurden Details und häufig gestellte Fragen im Zusammenhang mit der Migration zur Office 365-Lösung in Azure Sentinel hinzugefügt. Der Onboardingabschnitt wurde entfernt.



### <a name="logs"></a>Protokolle
- [Verwalten des Zugriffs auf Protokolldaten und Arbeitsbereiche in Azure Monitor](platform/manage-access.md): In diesem Artikel wurden die Informationen zu „NotActions“ aktualisiert.
- [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](platform/manage-cost-storage.md): In diesem Artikel wurde die Erläuterung der Berechnung des Datenvolumens im Abschnitt „Preismodell“ verbessert.
- [Verwalten von Log Analytics-Arbeitsbereichen mithilfe von Azure Resource Manager-Vorlagen](platform/template-workspace-configuration.md): In diesem Artikel wurde die Vorlage mit neuen Tarifen aktualisiert.


### <a name="platform-logs"></a>Plattformprotokolle
- [Aktualisierung für das Erfassen und Exportieren des Azure-Aktivitätsprotokolls](platform/diagnostic-settings-legacy.md): In diesem Artikel wurden weitere Informationen zu geänderten Eigenschaften hinzugefügt.
- [Exportieren des Aktivitätsprotokolls in den Speicher oder in Azure Event Hubs](platform/activity-log-export.md): Dieser Artikel wurde mit Benutzeroberflächenänderungen aktualisiert. 





## <a name="december-2019"></a>Dezember 2019

### <a name="agents"></a>Agents
- [Verbinden von Linux-Computern mit Azure Monitor](platform/agent-linux.md): Neuer Artikel.

### <a name="alerts"></a>Alerts
- [Erstellen einer Metrikwarnung anhand einer Resource Manager-Vorlage](platform/alerts-metric-create-templates.md): Diesem Artikel wurde ein Beispiel eine benutzerdefinierte Metrik hinzugefügt.
- [Metrikwarnungen mit dynamischen Schwellenwerten in Azure Monitor](platform/alerts-dynamic-thresholds.md): Diesem Artikel wurde ein Abschnitt zum Interpretieren dynamischer Schwellenwertdiagramme hinzugefügt.
- [Überblick über Warnungen in Microsoft Azure](platform/alerts-overview.md): In diesem Artikel wurde die Resource Graph-Abfrage aktualisiert.
- [Unterstützte Ressourcen für Metrikwarnungen in Azure Monitor](platform/alerts-metric-near-real-time.md): In diesem Artikel wurden die unterstützten Metriken und Dimensionen aktualisiert.
- [Wechseln der API-Einstellung für Protokollwarnungen](platform/alerts-log-api-switch.md): Diesem Artikel wurde ein Hinweis zum geänderten Warnungsnamen hinzugefügt.
- [Informationen zur Funktionsweise von Metrikwarnungen in Azure Monitor](platform/alerts-metric-overview.md): Diesem Artikel wurden unterstützte Ressourcentypen für die bedarfsorientierte Überwachung hinzugefügt.

### <a name="application-insights"></a>Application Insights
- [Application Insights für Workerdienstanwendungen (Anwendungen ohne HTTP)](app/worker-service.md): In diesem Artikel wurde dem C#-Code ein Standardprotokolliergrad hinzugefügt. Außerdem wurde die Paketreferenzversion aktualisiert.
- [Konfigurieren des Application Insights-SDK mit "ApplicationInsights.config" oder XML](app/configuration-with-applicationinsights-config.md): In diesem Artikel wurde der Beispielcode aktualisiert.
- [Verwalten von Application Insights-Ressourcen mithilfe von PowerShell](app/powershell.md): In diesem Artikel wurde die Resource Manager-Vorlage aktualisiert.
- [Application Insights-NuGet-Pakete](app/nuget.md): In diesem Artikel wurden die Paketversionen aktualisiert.
- [Erstellen einer Application Insights-Ressource](app/create-new-resource.md): Diesem Artikel wurde ein Hinweis zur Verwendung eines global eindeutigen Namens hinzugefügt.
- [Live Metrics Stream: Überwachung und Diagnose mit einer Latenzzeit von 1 Sekunde](app/live-stream.md): In diesem Artikel wurde die Versionsanforderung für das ASP.NET Core SDK aktualisiert.
- [Einführung in EventCounters](app/eventcounters.md): In diesem Artikel wurden Kategorie und Tabelle auf „customMetrics“ aktualisiert.
- [Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights](app/java-trace-logs.md): Diesem Artikel wurde eine Konfiguration für den Protokollierungsschwellenwert des Java-Agents hinzugefügt.
- [Von Application Insights und Log Analytics verwendete IP-Adressen](app/ip-addresses.md): In diesem Artikel wurden die IP-Adressen für Live Metrics Stream aktualisiert.
- [Überwachen der Leistung von Azure App Service](app/azure-web-apps.md): In diesem Artikel wurde die Unterstützung von ASP.NET Core 3.0 hinzugefügt. 
- [Einrichten von Azure Monitor für Ihre Python-Anwendung (Vorschau)](app/opencensus-python.md): In diesem Artikel wurde dem Azure Monitor-Schema eine Verdeutlichung der OpenCensus-Python-Schemazuordnung hinzugefügt.
- [SDK-Versionshinweise – Application Insights](app/release-notes.md): Diesem Artikel wurden Hinweise für ältere Releases hinzugefügt.
- [Telemetriekanäle in Application Insights](app/telemetry-channels.md): In diesem Artikel wurde die Dauer für verworfene Daten bei einem längeren Verbindungsausfall aktualisiert.
- [Erstellen von Stichproben in Application Insights](app/sampling.md): In diesem Artikel wurde der Codeausschnitt für das benutzerdefinierte TelemetryInitializer-Element korrigiert.
- [Anleitung zur Problembehandlung sowie Fragen und Antworten zu Application Insights für Java](app/java-troubleshoot.md): In diesem Artikel wurde die Aussage entfernt, dass die Abhängigkeitssammlung in JDK 9 nicht unterstützt wird.

### <a name="insights-and-solutions"></a>Erkenntnisse und Lösungen
- [Häufig gestellte Fragen zu Azure Monitor](insights/container-insights-faq.md): Diesem Artikel wurde eine Frage zu den Feldern „Image“ und „Name“ hinzugefügt.
- [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](insights/azure-sql.md): In diesem Artikel wurde die Unterstützung für die verwaltete Instanz für Datenbankwartevorgänge aktualisiert.
- [Konfigurieren der Datensammlung des Azure Monitor für Container-Agents](insights/container-insights-agent-config.md): Diesem Artikel wurde eine Einstellung für „enrich_container_logs“ hinzugefügt.
- [Konfigurieren von Kubernetes-Hybridclustern mit Azure Monitor für Container](insights/container-insights-hybrid-setup.md): Diesem Artikel wurde ein Problembehandlungsabschnitt hinzugefügt.
- [Überwachen des Active Directory-Replikationsstatus mit Azure Monitor](insights/ad-replication-status.md): In diesem Artikel wurde die .NET Framework-Voraussetzung aktualisiert.
- [Netzwerkleistungsmonitor-Lösung in Azure](insights/network-performance-monitor.md): Diesem Artikel wurden unterstützte Regionen hinzugefügt.
- [Optimieren Ihrer Active Directory-Umgebung mit der Active Directory-Integritätsüberprüfung-Lösung in Azure Monitor](insights/ad-assessment.md): In diesem Artikel wurde die .NET Framework-Voraussetzung aktualisiert.
- [Optimieren der SQL-Umgebung mit der SQL Server-Integritätsüberprüfung-Lösung in Azure Monitor](insights/sql-assessment.md): In diesem Artikel wurde die .NET Framework-Voraussetzung aktualisiert.
- [Optimieren Ihrer Umgebung mit der Lösung zur System Center Operations Manager-Integritätsüberprüfung (Vorschauversion)](insights/scom-assessment.md): In diesem Artikel wurde die .NET Framework-Voraussetzung aktualisiert.
- [Verbinden von ITSM-Produkten/-Diensten mit dem ITSM-Connector](platform/itsmc-connections.md): In diesem Artikel wurde der Voraussetzung im Zusammenhang mit Client-ID und Clientgeheimnis New York hinzugefügt.

### <a name="logs"></a>Protokolle
- [Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs](platform/delete-workspace.md): Diesem Artikel wurde die PowerShell-Methode hinzugefügt.
- [Entwerfen Ihrer Azure Monitor-Protokollbereitstellung](platform/design-logs-deployment.md): In diesem Artikel wurde die Erfassungsrate für einen Arbeitsbereich erhöht.

### <a name="metrics"></a>metrics
- [Über Diagnoseeinstellungen exportierbare Azure Monitor-Plattformmetriken](platform/metrics-supported-export-diagnostic-settings.md): Neuer Artikel.

### <a name="platform-logs"></a>Plattformprotokolle
Im Zuge der Neustrukturierung von Inhalten für Plattformprotokolle wurden mehrere Artikel aktualisiert. Grundlage dieser Aktualisierung ist ein neues Feature zum Konfigurieren des Aktivitätsprotokolls mit Diagnoseeinstellungen.

- [Archivieren von Azure-Ressourcenprotokollen in einem Speicherkonto](platform/resource-logs-collect-storage.md)
- [Ereignisschema des Azure-Aktivitätsprotokolls](platform/activity-log-schema.md)
- [Azure Monitor-Diensteinschränkungen](service-limits.md)
- [Erfassen und Analysieren von Azure-Aktivitätsprotokollen im Log Analytics-Arbeitsbereich in Azure Monitor](platform/activity-log-collect.md)
- [Aktualisierung für das Erfassen und Exportieren des Azure-Aktivitätsprotokolls](platform/diagnostic-settings-legacy.md)
- [Erfassen von Azure-Aktivitätsprotokollen für Azure Active Directory-Mandanten (Legacy) in Azure Monitor](platform/activity-log-collect-tenants.md)
- [Erfassen von Protokollen der Azure-Plattform im Log Analytics-Arbeitsbereich in Azure Monitor](platform/resource-logs-collect-workspace.md)
- [Erstellen von Diagnoseeinstellungen in Azure mithilfe einer Resource Manager-Vorlage](platform/diagnostic-settings-template.md)
- [Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure](platform/diagnostic-settings.md)
- [Exportieren des Aktivitätsprotokolls in den Speicher oder in Azure Event Hubs](platform/activity-log-export.md)
- [Übersicht über Protokolle der Azure-Plattform](platform/platform-logs-overview.md)
- [Streamen von Azure-Überwachungsdaten an einen Event Hub](platform/stream-monitoring-data-event-hubs.md)
- [Streamen von Protokollen der Azure-Plattform an Azure Event Hubs](platform/resource-logs-stream-event-hubs.md)

### <a name="quickstarts-and-tutorials"></a>Schnellstartanleitungen und Tutorials

- [Tutorial: Erstellen eines Metrikdiagramms in Azure Monitor](learn/tutorial-metrics-explorer.md): Neuer Artikel.
- [Tutorial: Sammeln und Analysieren von Ressourcenprotokollen von einer Azure-Ressource](learn/tutorial-resource-logs.md): Neuer Artikel.
- [Schnellstart: Überwachen einer Azure-Ressource mit Azure Monitor](learn/quick-monitor-azure-resource.md): Neuer Artikel.
   
## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Mitwirkung an der Azure Monitor-Dokumentation finden Sie bei Interesse im [Leitfaden für Mitwirkende an der Microsoft-Dokumentation](https://docs.microsoft.com/contribute/).