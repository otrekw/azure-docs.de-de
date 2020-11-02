---
title: Azure DDoS Protection Standard – Berichte und Datenflussprotokolle
description: Erfahren Sie, wie Sie Berichte und Datenflussprotokolle konfigurieren.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 2c4349662b06ff281ef028c833c6c43dd2707051
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92904866"
---
# <a name="configure-ddos-attack-mitigation-reports-and-flow-logs"></a>Konfigurieren der Berichte und Datenflussprotokolle zur Entschärfung von DDoS-Angriffen 

Die Standardversion von Azure DDoS Protection liefert per Analyse von DDoS-Angriffen ausführliche Erkenntnisse zu Angriffen und ermöglicht eine Visualisierung. Kunden, die ihre virtuellen Netzwerke vor DDoS-Angriffen schützen, verfügen über detaillierte Einblicke in den Datenverkehr von Angriffen und die Aktionen, die zur Eindämmung des Angriffs durchgeführt werden. Hierfür werden Berichte zur Angriffsentschärfung und Protokolle zum Verlauf der Entschärfung genutzt. Über Azure Monitor werden umfangreiche Telemetriedaten bereitgestellt. Diese umfassen u. a. detaillierte Metriken während der Dauer eines DDoS-Angriffs. Warnungen können für beliebige durch den DDoS-Schutz verfügbar gemachte Azure Monitor-Metriken konfiguriert werden. Die Protokollierung kann zudem mit [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection), Splunk (Azure Event Hubs), OMS Log Analytics und Azure Storage für die erweiterte Analyse über die Schnittstelle für die Azure Monitor-Diagnose integriert werden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Anzeigen und Konfigurieren der Berichte zur Entschärfung von DDoS-Angriffen
> * Anzeigen und Konfigurieren der Datenflussprotokolle zur Entschärfung von DDoS-Angriffen

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.
- Um die Schritte in diesem Tutorial auszuführen, müssen Sie zunächst einen [Azure DDoS Standard-Schutzplan](manage-ddos-protection.md) erstellen.

## <a name="view-and-configure-ddos-attack-mitigation-reports"></a>Anzeigen und Konfigurieren der Berichte zur Entschärfung von DDoS-Angriffen

Für Berichte zur Entschärfung von Angriffen werden die NetFlow-Protokolldaten verwendet. Diese Daten werden aggregiert, um ausführliche Informationen zum Angriff auf Ihre Ressource zu liefern. Jedes Mal, wenn eine öffentliche IP-Ressource angegriffen wird, beginnt die Berichterstellung, sobald der Entschärfungsvorgang gestartet wurde. Alle fünf Minuten wird ein inkrementeller Bericht generiert, und für den gesamten Entschärfungszeitraum wird ein Abschlussbericht erstellt. So wird sichergestellt, dass Sie bei einem länger andauernden DDoS-Angriff die aktuellste Momentaufnahme des Berichts zur Entschärfung (alle fünf Minuten) und eine vollständige Zusammenfassung nach Abschluss der Entschärfung anzeigen können. 

1. Wählen Sie oben im Portal auf der linken Seite **Alle Dienste** aus.
2. Geben Sie *Monitor* in das Feld **Filter** ein. Wenn **Monitor** in den Ergebnissen angezeigt wird, wählen Sie diese Angabe aus.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Diagnoseeinstellungen** aus.
4. Wählen Sie das **Abonnement** und die **Ressourcengruppe** aus, die die öffentliche zu protokollierende IP-Adresse enthält.
5. Wählen Sie **Öffentliche IP-Adresse** für **Ressourcentyp** aus, wählen Sie dann die jeweilige öffentliche IP-Adresse aus, für die Metriken protokolliert werden sollen.
6. Wählen Sie **Turn on diagnostics to collect the DDoSMitigationReports log** (Diagnose zum Erfassen des DDoSMitigationReports-Protokolls aktivieren) und dann so viele der folgenden Optionen aus, wie Sie benötigen:

    - **In einem Speicherkonto archivieren:** Daten werden in einem Azure Storage-Konto gespeichert. Weitere Informationen zu dieser Option finden Sie unter [Archivieren von Ressourcenprotokollen](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **An einen Event Hub streamen:** Erlaubt einem Protokollempfänger das Erfassen von Protokollen mithilfe eines Azure-Event Hubs. Event Hubs ermöglichen die Integration in Splunk oder andere SIEM-Systeme. Weitere Informationen zu dieser Option finden Sie unter [Streamen von Ressourcenprotokollen an Event Hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **An Log Analytics senden:** Schreibt Protokolle in den Azure Monitor-Dienst. Weitere Informationen zu dieser Option finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Azure Monitor-Protokollen](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Sowohl der inkrementelle Bericht als auch der Abschlussbericht zur Entschärfung enthält Felder zu den folgenden Bereichen:
- Angriffsvektoren
- Statistiken zum Datenverkehr
- Grund für verworfene Pakete
- Beteiligte Protokolle
- Top 10 der Länder/Regionen
- Top 10 der Quell-ASNs

## <a name="view-and-configure-ddos-attack-mitigation-flow-logs"></a>Anzeigen und Konfigurieren der Datenflussprotokolle zur Entschärfung von DDoS-Angriffen
Mit Datenflussprotokollen zur Entschärfung von Angriffen können Sie den verworfenen Datenverkehr, weitergeleiteten Datenverkehr und andere interessante Datenpunkte bei einem aktiven DDoS-Angriff nahezu in Echtzeit überprüfen. Sie können den konstanten Datenstrom dieser Daten in Azure Sentinel oder Ihren SIEM-Drittanbietersystemen per Event Hub erfassen, um eine Überwachung nahezu in Echtzeit zu ermöglichen, gegebenenfalls Aktionen durchzuführen und Ihre erforderlichen Abwehrmaßnahmen zu ermitteln.

1. Wählen Sie oben im Portal auf der linken Seite **Alle Dienste** aus.
2. Geben Sie *Monitor* in das Feld **Filter** ein. Wenn **Monitor** in den Ergebnissen angezeigt wird, wählen Sie diese Angabe aus.
3. Wählen Sie unter **EINSTELLUNGEN** die Option **Diagnoseeinstellungen** aus.
4. Wählen Sie das **Abonnement** und die **Ressourcengruppe** aus, die die öffentliche zu protokollierende IP-Adresse enthält.
5. Wählen Sie **Öffentliche IP-Adresse** für **Ressourcentyp** aus, wählen Sie dann die jeweilige öffentliche IP-Adresse aus, für die Metriken protokolliert werden sollen.
6. Wählen Sie **Turn on diagnostics to collect the DDoSMitigationFlowLogs log** (Diagnose zum Erfassen des DDoSMitigationFlowLogs-Protokolls aktivieren) und dann so viele der folgenden Optionen aus, wie Sie benötigen:

    - **In einem Speicherkonto archivieren:** Daten werden in einem Azure Storage-Konto gespeichert. Weitere Informationen zu dieser Option finden Sie unter [Archivieren von Ressourcenprotokollen](../azure-monitor/platform/archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **An einen Event Hub streamen:** Erlaubt einem Protokollempfänger das Erfassen von Protokollen mithilfe eines Azure-Event Hubs. Event Hubs ermöglichen die Integration in Splunk oder andere SIEM-Systeme. Weitere Informationen zu dieser Option finden Sie unter [Streamen von Ressourcenprotokollen an Event Hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **An Log Analytics senden:** Schreibt Protokolle in den Azure Monitor-Dienst. Weitere Informationen zu dieser Option finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Azure Monitor-Protokollen](../azure-monitor/platform/collect-azure-metrics-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="azure-sentinel-data-connector"></a>Azure Sentinel-Datenconnector

Sie können eine Verbindung mit Azure Sentinel herstellen, Ihre Daten in Arbeitsmappen anzeigen und analysieren, benutzerdefinierte Warnungen erstellen und diese in Untersuchungsprozesse integrieren. Informationen zum Herstellen einer Verbindung mit Azure Sentinel finden Sie unter [Herstellen einer Verbindung mit Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-ddos-protection). 

![Azure Sentinel-DDoS-Connector](./media/ddos-attack-telemetry/azure-sentinel-ddos.png)

### <a name="azure-ddos-protection-workbook"></a>Azure DDoS Protection-Arbeitsmappe

Zum Anzeigen der Verlaufsprotokolldaten im Azure Analytics-Dashboard können Sie das Beispieldashboard von https://github.com/Azure/Azure-Network-Security/tree/master/Azure%20DDoS%20Protection/Azure%20DDoS%20Protection%20Workbook importieren.

Flussprotokolle enthalten die folgenden Felder: 
- Quell-IP
- Ziel-IP
- Quellport 
- Zielport 
- Protokolltyp 
- Bei der Entschärfung durchgeführte Aktion

![DDoS Protection-Arbeitsmappe](./media/ddos-attack-telemetry/ddos-attack-analytics-workbook.png)

Die Angriffsanalyse funktioniert nur, wenn DDoS Protection Standard im virtuellen Netzwerk der öffentlichen IP-Adresse aktiviert ist. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

- Anzeigen und Konfigurieren der Berichte zur Entschärfung von DDoS-Angriffen
- Anzeigen und Konfigurieren der Datenflussprotokolle zur Entschärfung von DDoS-Angriffen

Informationen zum Testen und Simulieren eines DDoS-Angriffs finden Sie in der Anleitung zum Durchführen von Simulationstests:

> [!div class="nextstepaction"]
> [Durchführen von Simulationstests](test-through-simulations.md)

