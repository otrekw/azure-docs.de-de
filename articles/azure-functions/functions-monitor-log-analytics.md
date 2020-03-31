---
title: Überwachen von Azure Functions mit Azure Monitor-Protokollen
description: Erfahren Sie, wie Sie Azure Monitor-Protokolle mit Azure Functions zum Überwachen der Funktionsausführung verwenden.
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: cshoe
ms.openlocfilehash: 13c72a1cf8a0dd4a1124e51b9ceee04ae04bf261
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77649873"
---
# <a name="monitoring-azure-functions-with-azure-monitor-logs"></a>Überwachen von Azure Functions mit Azure Monitor-Protokollen

Azure Functions bietet eine Integration in [Azure Monitor-Protokolle](../azure-monitor/platform/data-platform-logs.md) zum Überwachen von Funktionen. In diesem Artikel wird veranschaulicht, wie Sie Azure Functions zum Senden von systemseitig generierten sowie von benutzergenerierten Protokollen an Azure Monitor-Protokolle konfigurieren.

Azure Monitor-Protokolle bieten Ihnen die Möglichkeit, Protokolle aus unterschiedlichen Ressourcen im selben Arbeitsbereich zu konsolidieren, wo Sie mit [Abfragen](../azure-monitor/log-query/log-query-overview.md) analysiert werden können, um gesammelte Daten schnell abzurufen, zu konsolidieren und zu analysieren.  Sie können Abfragen mit [Log Analytics](../azure-monitor/log-query/portals.md) im Azure-Portal erstellen und testen und die Daten dann entweder mit diesen Tools direkt analysieren oder Abfragen zur Verwendung mit [Visualisierungen](../azure-monitor/visualizations.md) oder [Warnungsregeln](../azure-monitor/platform/alerts-overview.md) speichern.

Azure Monitor verwendet eine Version der von Azure Data Explorer verwendeten [Abfragesprache Kusto](/azure/kusto/query/), die für einfache Protokollabfragen geeignet ist, aber auch erweiterte Funktionen enthält, z.B. Aggregationen, Verknüpfungen und intelligente Analysen. Sie können die Abfragesprache schnell erlernen, indem Sie [mehrere Lektionen](../azure-monitor/log-query/get-started-queries.md) nutzen.

> [!NOTE]
> Die Integration in Azure Monitor-Protokolle befindet sich derzeit in der öffentlichen Vorschauphase für Funktions-Apps, die unter den Windows-Hostingplänen „Nutzung“, „Premium“ und „Dedicated“ ausgeführt werden.

## <a name="setting-up"></a>Einrichten

Wählen Sie im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** aus, und klicken Sie dann auf **Diagnoseeinstellung hinzufügen**.

![Hinzufügen einer Diagnoseeinstellung](media/functions-monitor-log-analytics/diagnostic-settings-add.png)

Wählen Sie auf der Seite **Diagnoseeinstellungen** die Option **An Log Analytics senden** und anschließend Ihren Log Analytics-Arbeitsbereich aus. Wählen Sie unter **Protokoll** die Option **FunctionAppLogs** aus. Diese Tabelle enthält die gewünschten Protokolle.

![Hinzufügen einer Diagnoseeinstellung](media/functions-monitor-log-analytics/choose-table.png)

## <a name="user-generated-logs"></a>Benutzergenerierte Protokolle

Um benutzerdefinierte Protokolle zu generieren, können Sie die jeweils spezifische Protokollierungsanweisung, abhängig von Ihrer Sprache, verwenden. Hier finden Sie Beispielcodeausschnitte:


# <a name="c"></a>[C#](#tab/csharp)

```csharp
log.LogInformation("My app logs here.");
```

# <a name="java"></a>[Java](#tab/java)

```java
context.getLogger().info("My app logs here.");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
context.log('My app logs here.');
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Write-Host "My app logs here."
```

# <a name="python"></a>[Python](#tab/python)

```python
logging.info('My app logs here.')
```

---

## <a name="querying-the-logs"></a>Abfragen der Protokolle

Wechseln Sie zum Abfragen der generierten Protokolle zum Log Analytics-Arbeitsbereich, den Sie zum Senden der Funktionsprotokolle konfiguriert haben, und klicken Sie auf **Protokolle**.

![Abfragefenster im Log Analytics-Arbeitsbereich](media/functions-monitor-log-analytics/querying.png)

Azure Functions schreibt alle Protokolle die Tabelle **FunctionAppLogs**. Hier finden Sie ein paar Beispielabfragen.

### <a name="all-logs"></a>Alle Protokolle

```

FunctionAppLogs
| order by TimeGenerated desc

```

### <a name="a-specific-function-logs"></a>Ein bestimmtes Funktionsprotokoll

```

FunctionAppLogs
| where FunctionName == "<Function name>" 

```

### <a name="exceptions"></a>Ausnahmen

```

FunctionAppLogs
| where ExceptionDetails != ""  
| order by TimeGenerated asc

```

## <a name="next-steps"></a>Nächste Schritte

- Lesen der [Übersicht zu Azure Functions](functions-overview.md).
- Weitere Informationen zu [Azure Monitor-Protokolle](../azure-monitor/platform/data-platform-logs.md).
- Erfahren Sie mehr über die [Abfragesprache](../azure-monitor/log-query/get-started-queries.md).
