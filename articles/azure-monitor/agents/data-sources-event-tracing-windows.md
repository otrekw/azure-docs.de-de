---
title: Sammeln von ETW-Ereignissen (Ereignisablaufverfolgung für Windows) zur Analyse in Azure Monitor-Protokollen
description: Erfahren Sie, wie ETW-Ereignisse (Ereignisablaufverfolgung für Windows) zur Analyse in Azure Monitor-Protokollen gesammelt werden.
services: azure-monitor
ms.topic: conceptual
ms.author: jamesfit
author: jimmyfit
ms.date: 01/29/2021
ms.openlocfilehash: 096d16bb2c7249ec99f37fbee9d8ffc8f0e45db2
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050765"
---
# <a name="collecting-event-tracing-for-windows-etw-events-for-analysis-azure-monitor-logs"></a>Sammeln von ETW-Ereignissen (Ereignisablaufverfolgung für Windows) zur Analyse in Azure Monitor-Protokollen

Die *Ereignisablaufverfolgung für Windows (Event Tracing for Windows, ETW)* bietet einen Mechanismus für die Instrumentierung von Benutzermodusanwendungen und Kernelmodustreibern. Mithilfe des Log Analytics-Agents werden [Windows-Ereignisse gesammelt](./data-sources-windows-events.md), die in die [ETW-Kanäle](/windows/win32/wes/eventmanifestschema-channeltype-complextype) für Verwaltung und Betrieb geschrieben werden. Manchmal ist es jedoch erforderlich, andere Ereignisse zu erfassen und zu analysieren, z. B. Ereignisse, die in den Analysekanal geschrieben werden.  

## <a name="event-flow"></a>Ereignisfluss

Um erfolgreich [manifestbasierte ETW-Ereignisse](/windows/win32/etw/about-event-tracing#types-of-providers) für die Analyse in Azure Monitor-Protokollen zu sammeln, müssen Sie die [Azure-Diagnoseerweiterung](./diagnostics-extension-overview.md) für Windows (WAD) verwenden. In diesem Szenario fungiert die Diagnoseerweiterung als der ETW-Consumer und schreibt Ereignisse in Azure Storage (Tabellen) als Zwischenspeicher. Hier werden diese in einer Tabelle mit dem Namen **WADETWEventTable** gespeichert. Log Analytics sammelt dann die Tabellendaten aus Azure Storage und stellt Sie als Tabelle mit dem Namen **ETWEvent** dar.

![Ereignisfluss](./media/data-sources-event-tracing-windows/event-flow.png)

## <a name="configuring-etw-log-collection"></a>Konfigurieren der ETW-Protokollsammlung

### <a name="step-1-locate-the-correct-etw-provider"></a>Schritt 1: Suchen des richtigen ETW-Anbieters

Verwenden Sie einen der folgenden Befehle, um die ETW-Anbieter in einem Windows-Quellsystem aufzulisten.

Befehlszeile:

```
logman query providers
```

PowerShell:
```
Get-NetEventProvider -ShowInstalled | Select-Object Name, Guid
```
Optional können Sie diese PowerShell-Ausgabe zur leichteren Navigation an Out-Gridview übergeben.

Notieren Sie sich den Namen des ETW-Anbieters und die GUID, die dem in der Ereignisanzeige dargestellten Analyse- oder Debugprotokoll bzw. dem Modul entspricht, für das Sie Ereignisdaten sammeln möchten.

### <a name="step-2-diagnostics-extension"></a>Schritt 2: Diagnoseerweiterung

Stellen Sie sicher, dass die *Windows-Diagnoseerweiterung* auf allen Quellsystemen [installiert](./diagnostics-extension-windows-install.md#install-with-azure-portal) ist.

### <a name="step-3-configure-etw-log-collection"></a>Schritt 3: Konfigurieren der ETW-Protokollsammlung

1. Navigieren Sie zum Blatt **Diagnoseeinstellungen** des virtuellen Computers.

2. Wählen Sie die Registerkarte **Protokolle** aus.

3. Scrollen Sie nach unten, und aktivieren Sie die Option **Ereignisablaufverfolgung für Windows-Ereignisse (ETW)** . ![Screenshot der Diagnoseeinstellungen](./media/data-sources-event-tracing-windows/enable-event-tracing-windows-collection.png)

4. Legen Sie die Anbieter-GUID oder die Anbieterklasse entsprechend dem Anbieter fest, für den Sie die Sammlung konfigurieren.

5. Legen Sie die [**Protokollebene**](/windows/win32/etw/configuring-and-starting-an-event-tracing-session) nach Bedarf fest.

6. Klicken Sie auf die Auslassungspunkte neben dem angegebenen Anbieter, und klicken Sie dann auf **Konfigurieren**.

7. Vergewissern Sie sich, dass **Standardmäßige Zieltabelle** auf **etweventtable** festgelegt ist.

8. Legen Sie bei Bedarf einen [**Schlüsselwortfilter**](/windows/win32/wes/defining-keywords-used-to-classify-types-of-events) fest.

9. Speichern Sie die Anbieter- und Protokolleinstellungen.

Sobald passende Ereignisse generiert werden, sollten die ETW-Ereignisse in der Tabelle **WADetweventtable** in Azure Storage angezeigt werden. Dies können Sie mit Azure Storage-Explorer kontrollieren.

### <a name="step-4-configure-log-analytics-storage-account-collection"></a>Schritt 4: Konfigurieren einer Sammlung aus Log Analytics-Speicherkonten

Folgen Sie [diesen Anweisungen](https://docs.microsoft.com/azure/azure-monitor/essentials/diagnostics-extension-logs#collect-logs-from-azure-storage), um die Protokolle aus Azure Storage zu sammeln. Nach der Konfiguration sollten die ETW-Ereignisdaten in Log Analytics unter der Tabelle **ETWEvent** angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
- Verwenden Sie [benutzerdefinierte Felder](../logs/custom-fields.md) zum Strukturieren Ihrer ETW-Ereignisse.
- Erfahren Sie mehr über [Protokollabfragen](../logs/log-query-overview.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.
