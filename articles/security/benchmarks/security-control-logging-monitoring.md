---
title: 'Azure-Sicherheitskontrolle: Protokollierung und Überwachung'
description: Azure-Sicherheitskontrolle – Protokollierung und Überwachung
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 82114164d70eae71678e70ff2bdb7ea44a54d4cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87076299"
---
# <a name="security-control-logging-and-monitoring"></a>Sicherheitskontrolle: Protokollierung und Überwachung

Die Sicherheitsprotokollierung und -überwachung konzentriert sich auf Aktivitäten im Zusammenhang mit dem Aktivieren, Abrufen und Speichern von Überwachungsprotokollen für Azure-Dienste.

## <a name="21-use-approved-time-synchronization-sources"></a>2.1: Verwenden von genehmigten Zeitsynchronisierungsquellen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.1 | 6.1 | Microsoft |

Microsoft verwaltet Zeitquellen für Azure-Ressourcen. Sie haben jedoch die Möglichkeit, die Zeitsynchronisierungseinstellungen für Ihre Computeressourcen zu verwalten.

- [Konfigurieren der Zeitsynchronisierung für Azure Windows-Computeressourcen](https://docs.microsoft.com/azure/virtual-machines/windows/time-sync)

- [Konfigurieren der Zeitsynchronisierung für Azure Linux-Computeressourcen](https://docs.microsoft.com/azure/virtual-machines/linux/time-sync)

## <a name="22-configure-central-security-log-management"></a>2.2: Konfigurieren der zentralen Sicherheitsprotokollverwaltung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.2 | 6.5, 6.6 | Kunde |

Erfassen von Protokollen über Azure Monitor zum Aggregieren von Sicherheitsdaten, die von Endpunktgeräten, Netzwerkressourcen und anderen Sicherheitssystemen generiert werden. Verwenden Sie Log Analytics-Arbeitsbereiche in Azure Monitor, um Analysen abzufragen und auszuführen, und verwenden Sie Azure Storage-Konten für die langfristige Speicherung/Archivierung.

Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren. 

- [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Sammeln von internen Azure Virtual Machine-Hostprotokollen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Erste Schritte mit Azure Monitor und der Integration einer SIEM-Drittanbieterlösung](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

## <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Aktivieren der Überwachungsprotokollierung für Azure-Ressourcen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.3 | 6.2, 6.3 | Kunde |

Aktivieren der Diagnoseeinstellungen auf Azure-Ressourcen für den Zugriff auf Überwachungs-, Sicherheits- und Diagnoseprotokolle. Aktivitätsprotokolle, die automatisch verfügbar sind, enthalten Ereignisquelle, Datum, Benutzer, Zeitstempel, Quelladressen, Zieladressen und andere nützliche Elemente.

- [Sammeln von Plattformprotokollen und -metriken mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings)

- [Grundlegendes zur Protokollierung und zu verschiedenen Protokolltypen in Azure](https://docs.microsoft.com/azure/azure-monitor/platform/platform-logs-overview)

## <a name="24-collect-security-logs-from-operating-systems"></a>2.4: Erfassen von Sicherheitsprotokollen von Betriebssystemen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.4 | 6.2, 6.3 | Kunde |

Wenn sich die Computeressource im Besitz von Microsoft befindet, ist Microsoft für die Überwachung verantwortlich. Wenn die Computeressource Ihrer Organisation gehört, liegt es in Ihrer Verantwortung, sie zu überwachen. Sie können Azure Security Center zum Überwachen des Betriebssystems verwenden. Von Azure Security Center vom Betriebssystem erfasste Daten sind z. B. Betriebssystemtyp und -version, Betriebssystem (Windows-Ereignisprotokolle), ausgeführte Prozesse, Computername, IP-Adressen und angemeldeter Benutzer. Darüber hinaus erfasst der Log Analytics Agent Absturzabbilddateien.

- [Sammeln von internen Azure Virtual Machine-Hostprotokollen mit Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-azurevm)

- [Grundlegendes zur Datensammlung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection)

## <a name="25-configure-security-log-storage-retention"></a>2.5: Konfigurieren der Sicherheitsprotokoll-Aufbewahrungsdauer im Speicher

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.5 | 6.4 | Kunde |

Legen Sie in Azure Monitor den Aufbewahrungszeitraum Ihres Log Analytics-Arbeitsbereichs gemäß den Compliancevorschriften Ihrer Organisation fest. Verwenden Sie Azure Storage-Konten für die langfristige/Archivierungsspeicherung.

- [Ändern des Datenaufbewahrungszeitraums in Protokollanalyse](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Konfigurieren der Aufbewahrungsrichtlinie für Azure Storage-Kontoprotokolle](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging)

## <a name="26-monitor-and-review-logs"></a>2.6: Überwachen und Überprüfen von Protokollen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.6 | 6.7 | Kunde |

Analysieren und überwachen Sie Protokolle auf anormales Verhalten, und prüfen Sie die Ergebnisse regelmäßig. Verwenden Sie den Log Analytics-Arbeitsbereich von Azure Monitor, um Protokolle zu überprüfen und Abfragen nach Protokolldaten auszuführen.

Alternativ dazu können Sie auch Daten in Azure Sentinel oder einer Drittanbieter-SIEM-Lösung aktivieren und integrieren. 

- [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Grundlegendes zum Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

- [Ausführen benutzerdefinierter Abfragen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

## <a name="27-enable-alerts-for-anomalous-activities"></a>2.7: Aktivieren von Warnungen bei anomalen Aktivitäten

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.7 | 6,8 | Kunde |

Verwenden Sie Azure Security Center mit einem Log Analytics-Arbeitsbereich für die Überwachung und Warnung bei anomalen Aktivitäten, die in Sicherheitsprotokollen und Ereignissen gefunden werden.

Alternativ dazu können Sie auch Daten in Azure Sentinel aktivieren und integrieren.

- [Durchführen des Onboardings für Azure Sentinel](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

- [Verwalten von Warnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

- [Warnungen bei Log Analytics-Protokolldaten](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-response)

## <a name="28-centralize-anti-malware-logging"></a>2.8: Zentralisieren der Antischadsoftwareprotokollierung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.8 | 8.6 | Kunde |

Aktivieren Sie die Antischadsoftware-Ereigniserfassung für Azure Virtual Machines und Cloud Services.

- [Konfigurieren von Microsoft Antimalware für Virtual Machines](/powershell/module/servicemanagement/azure.service/set-azurevmmicrosoftantimalwareextension?view=azuresmps-4.0.0)

- [Konfigurieren von Microsoft Antimalware für Cloud Services](/powershell/module/servicemanagement/azure.service/set-azureserviceantimalwareextension?view=azuresmps-4.0.0)

- [Grundlegendes zu Microsoft Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)

## <a name="29-enable-dns-query-logging"></a>2.9: Aktivieren der DNS-Abfrageprotokollierung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2,9 | 8,7 | Kunde |

Implementieren Sie eine Drittanbieterlösung aus Azure Marketplace für die DNS-Protokollierungslösung entsprechend den Anforderungen Ihrer Organisation.  

## <a name="210-enable-command-line-audit-logging"></a>2.10: Aktivieren der Befehlszeilen-Überwachungsprotokollierung

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 2.10 | 8,8 | Kunde |

Verwenden Sie Microsoft Monitoring Agent auf allen unterstützten Azure Windows Virtual Machines, um das Prozesserstellungsereignis und das Feld „CommandLine“ zu protokollieren.   Bei unterstützten virtuellen Azure Linux-Computern können Sie die Konsolenprotokollierung pro Knoten manuell konfigurieren und die Daten mithilfe von Syslog speichern.  Verwenden Sie außerdem den Log Analytics-Arbeitsbereich von Azure Monitor, um Protokolle zu überprüfen und Abfragen für protokollierte Daten von Azure Virtual Machines auszuführen. 

- [Datensammlung in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection#data-collection-tier)

- [Ausführen benutzerdefinierter Abfragen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

- [Syslog-Datenquellen in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-syslog)


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Identität und Zugriffssteuerung](security-control-identity-access-control.md)