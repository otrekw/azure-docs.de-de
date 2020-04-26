---
title: Weiterleiten von Azure Automation-Auftragsdaten an Azure Monitor-Protokolle
description: In diesem Artikel wird erläutert, wie Auftragsstatus und Runbookauftrags-Datenströme an Azure Monitor-Protokolle gesendet werden, um zusätzliche Einblicke und Verwaltungsoptionen zu erhalten.
services: automation
ms.subservice: process-automation
ms.date: 02/05/2019
ms.topic: conceptual
ms.openlocfilehash: a9f4e641e60d6cf1c481c445767422e8b4df683b
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457687"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-azure-monitor-logs"></a>Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Azure Monitor-Protokolle

Automation kann Runbookauftragsstatus und Auftragsdatenströme an Ihren Log Analytics-Arbeitsbereich senden. Dieser Prozess beinhaltet nicht die Arbeitsbereichsverknüpfung. Er ist vollkommen unabhängig. Auftragsprotokolle und -streams werden im Azure-Portal oder mit PowerShell für einzelne Aufträge angezeigt, d.h., Sie können einfache Untersuchen durchführen. Mit Azure Monitor-Protokollen können Sie jetzt:

* Gewinnen Sie Erkenntnisse über den Status Ihrer Automation-Aufträge.
* Lösen Sie basierend auf Ihrem Runbookauftragsstatus (beispielsweise „Fehler“ oder „Angehalten“) das Senden einer E-Mail oder einer Warnung aus.
* Schreiben Sie erweiterte Abfragen für Ihre Auftragsdatenströme.
* Korrelieren Sie Aufträge über Automation-Konten hinweg.
* Benutzerdefinierte Ansichten und Suchabfragen zum Visualisieren von Runbookergebnissen, Runbookauftragsstatus und anderer wichtiger Schlüsselindikatoren oder Metriken verwenden

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites-and-deployment-considerations"></a>Voraussetzungen und Überlegungen zur Bereitstellung

Zum Senden Ihrer Automation-Protokolle an Azure Monitor-Protokolle benötigen Sie:

* Das aktuelle Release von [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)
* Einen Log Analytics-Arbeitsbereich Weitere Informationen finden Sie unter [Erste Schritte mit Azure Monitor-Protokolle](../log-analytics/log-analytics-get-started.md).
* Die Ressourcen-ID für Ihr Azure Automation-Konto.

Verwenden Sie den folgenden Befehl, um die Ressourcen-ID für Ihr Azure Automation-Konto zu ermitteln:

```powershell-interactive
# Find the ResourceId for the Automation account
Get-AzResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Führen Sie zum Ermitteln der Ressourcen-ID für Ihren Log Analytics-Arbeitsbereich den folgenden PowerShell-Befehl aus:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Sollten Sie über mehrere Automation-Konten (oder über mehrere Arbeitsbereiche) verfügen, suchen Sie in der Ausgabe der vorherigen Befehle den Namen, den Sie konfigurieren müssen, und kopieren Sie den Wert für die Ressourcen-ID.

1. Wählen Sie im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto aus, und wählen Sie dann **Alle Einstellungen** aus. 
2. Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften** aus.  
3. Notieren Sie sich die auf dem Blatt **Eigenschaften** unten angezeigten Eigenschaften.

    ![Automation-Konto – Eigenschaften](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png)erforderlich.

## <a name="azure-monitor-log-records"></a>Protokolldatensätze in Azure Monitor

Die Diagnose von Azure Automation erstellt zwei Typen von Datensätzen in Azure Monitor-Protokollen, gekennzeichnet als `AzureDiagnostics`. Die Tabellen in den nächsten Abschnitten sind Beispiele für Datensätze, die von Azure Automation generiert werden, und für die Datentypen, die in Protokollsuchergebnissen angezeigt werden.

### <a name="job-logs"></a>Auftragsprotokolle

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| TimeGenerated |Ausführungsdatum und -uhrzeit des Runbookauftrags. |
| RunbookName_s |Der Name des Runbooks. |
| Caller_s |Der Aufrufer, der den Vorgang initiiert hat. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| Tenant_g | Die GUID, die den Mandanten für den Aufrufer identifiziert. |
| JobId_g |Die GUID, der den Runbookauftrag identifiziert. |
| ResultType |Der Status des Runbookauftrags. Mögliche Werte:<br>- Neu<br>- Erstellt<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Abgeschlossen |
| Category | Klassifizierung des Datentyps. Für Automation lautet der Wert „JobLogs“. |
| Vorgangsname | Der Typ des in Azure ausgeführten Vorgangs. Für Automation lautet der Wert „Job“. |
| Resource | Den Namen des Automation-Kontos. |
| SourceSystem | Das System, das Azure Monitor-Protokolle verwenden, um die Daten zu erfassen. Für Azure-Diagnose ist der Wert immer „Azure“. |
| ResultDescription |Der Ergebniszustand des Runbookauftrags. Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen |
| CorrelationId |Die Korrelations-GUID des Runbookauftrags. |
| resourceId |Die Ressourcen-ID für das Azure Automation-Konto des Runbooks. |
| SubscriptionId | Die Azure-Abonnement-GUID für das Automation-Konto. |
| ResourceGroup | Der Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | Der Ressourcenanbieter. Der Wert lautet „MICROSOFT.AUTOMATION“. |
| ResourceType | Der Ressourcentyp. Der Wert lautet „AUTOMATIONACCOUNTS“. |

### <a name="job-streams"></a>Auftragsdatenströme
| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| TimeGenerated |Ausführungsdatum und -uhrzeit des Runbookauftrags. |
| RunbookName_s |Der Name des Runbooks. |
| Caller_s |Der Aufrufer, der den Vorgang initiiert hat. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| StreamType_s |Der Typ des Auftragsstreams. Mögliche Werte:<br>\- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich |
| Tenant_g | Die GUID, die den Mandanten für den Aufrufer identifiziert. |
| JobId_g |Die GUID, der den Runbookauftrag identifiziert. |
| ResultType |Der Status des Runbookauftrags. Mögliche Werte:<br>– In Bearbeitung |
| Category | Klassifizierung des Datentyps. Für Automation lautet der Wert „JobStreams“. |
| Vorgangsname | Der Typ des in Azure ausgeführten Vorgangs. Für Automation lautet der Wert „Job“. |
| Resource | Der Namen des Automation-Kontos. |
| SourceSystem | Das System, das Azure Monitor-Protokolle verwenden, um die Daten zu erfassen. Für Azure-Diagnose ist der Wert immer „Azure“. |
| ResultDescription |Die Beschreibung, die den Ausgabedatenstrom des Runbooks enthält. |
| CorrelationId |Die Korrelations-GUID des Runbookauftrags. |
| resourceId |Die Ressourcen-ID für das Azure Automation-Konto des Runbooks. |
| SubscriptionId | Die Azure-Abonnement-GUID für das Automation-Konto. |
| ResourceGroup | Der Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | Der Ressourcenanbieter. Der Wert lautet „MICROSOFT.AUTOMATION“. |
| ResourceType | Der Ressourcentyp. Der Wert lautet „AUTOMATIONACCOUNTS“. |

## <a name="setting-up-integration-with-azure-monitor-logs"></a>Einrichten der Integration in Azure Monitor-Protokolle

1. Starten Sie auf Ihrem Computer die Windows PowerShell über den **Start**bildschirm.
2. Führen Sie die folgenden PowerShell-Befehle aus, und bearbeiten Sie die Werte für `[your resource ID]` und `[resource ID of the log analytics workspace]` mit den Werten aus dem vorherigen Abschnitt.

   ```powershell-interactive
   $workspaceId = "[resource ID of the log analytics workspace]"
   $automationAccountId = "[resource ID of your Automation account]"

   Set-AzDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled 1
   ```

Nach dem Ausführen dieses Skripts kann es eine Stunde dauern, bis in den Azure Monitor-Protokollen Datensätze neu geschriebener `JobLogs` oder `JobStreams` angezeigt werden.

Um die Protokolle anzuzeigen, führen Sie die folgende Abfrage in der Log Analytics-Protokollsuche durch: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Überprüfen der Konfiguration

Vergewissern Sie sich, dass Ihr Automation-Konto Protokolle an Ihren Log Analytics-Arbeitsbereich sendet. Überprüfen Sie hierzu mithilfe des folgenden PowerShell-Befehls, ob die Diagnose für das Automation-Konto ordnungsgemäß konfiguriert ist.

```powershell-interactive
Get-AzDiagnosticSetting -ResourceId $automationAccountId
```

Überprüfen Sie in der Ausgabe Folgendes:

* Unter `Logs` ist der Wert für `Enabled` „True“.
* `WorkspaceId` ist auf den `ResourceId`-Wert für Ihren Log Analytics-Arbeitsbereich festgelegt.

## <a name="viewing-automation-logs-in-azure-monitor-logs"></a>Anzeigen von Automation-Protokollen in Azure Monitor-Protokolle

Nachdem Sie damit begonnen haben, Ihre Automation-Auftragsprotokolle an Azure Monitor-Protokolle zu senden, sehen wir uns nun an, wie Sie diese Protokolle in Azure Monitor-Protokolle verwenden können.

Führen Sie die folgende Abfrage aus, um die Protokolle anzuzeigen: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Senden einer E-Mail, wenn ein Runbookauftrag mit einem Fehler abgebrochen oder angehalten wird

Einer der häufigsten Kundenwünsche ist die Möglichkeit, eine E-Mail oder SMS zu senden, wenn bei einem Runbookauftrag ein Problem auftritt.

Zum Erstellen einer Warnungsregel erstellen Sie zunächst eine Protokollsuche für die Datensätze des Runbookauftrags, die die Warnung auslösen sollen. Klicken Sie auf die Schaltfläche **Warnung**, um die Warnungsregel zu erstellen und zu konfigurieren.

1. Klicken Sie auf der Seite „Übersicht“ für den Log Analytics-Arbeitsbereich auf **Protokolle anzeigen**.
2. Erstellen Sie eine Protokollsuchabfrage für Ihre Warnung, indem Sie folgenden Text in das Abfragefeld eingeben: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")`<br><br>Sie können auch eine Gruppierung nach Runbookname vornehmen, indem Sie Folgendes eingeben: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Wenn Sie Protokolle von mehreren Automation-Konten oder Abonnements in Ihrem Arbeitsbereich eingerichtet haben, können Sie Ihre Warnungen nach Abonnement und Automation-Konto gruppieren. Der Name des Automation-Kontos kann dem `Resource`-Feld in der `JobLogs`-Suche entnommen werden.
3. Klicken Sie oben auf der Seite auf **Neue Warnungsregel**, um den Bildschirm **Regel erstellen** zu öffnen. Weitere Informationen zu den Konfigurationsoptionen für Warnungen finden Sie unter [Protokollwarnungen in Azure Monitor – Warnungen](../azure-monitor/platform/alerts-unified-log.md).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Durchführen einer Suche nach allen Aufträgen, die mit Fehlern abgeschlossen wurden

Neben dem Erstellen von Warnungen für Fehler können Sie auch ermitteln, wenn ein Runbook-Auftrag einen Fehler ohne Abbruch aufweist. In diesen Fällen erzeugt PowerShell einen Fehlerdatenstrom, die Fehler ohne Abbruch führen jedoch nicht dazu, dass der Auftrag unterbrochen wird oder fehlschlägt.

1. Klicken Sie in Ihrem Log Analytics-Arbeitsbereich auf **Protokolle**.
2. Geben Sie in das Abfragefeld `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` ein.
3. Klicken Sie auf die Schaltfläche **Suchen**.

### <a name="view-job-streams-for-a-job"></a>Anzeigen von Auftragsdatenströmen für einen Auftrag

Wenn Sie einen Auftrag debuggen, sollten Sie ggf. auch einen Blick in die Auftragsdatenströme werfen. Mit der folgenden Abfrage werden alle Datenströme für einen einzelnen Auftrag mit der GUID „2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0“ abgerufen:

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Anzeigen des Auftragsstatusverlaufs

Abschließend sollten Sie Ihren Auftragsverlauf visualisieren. Sie können die folgende Abfrage verwenden, um nach dem Statusverlauf Ihrer Aufträge zu suchen.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`
<br> ![Diagramm zum Auftragsstatusverlauf in Log Analytics](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="removing-diagnostic-settings"></a>Entfernen von Diagnoseeinstellungen

Führen Sie zum Entfernen der Diagnoseeinstellung aus dem Automation-Konto den folgenden Befehl aus:

```powershell-interactive
$automationAccountId = "[resource ID of your Automation account]"

Remove-AzDiagnosticSetting -ResourceId $automationAccountId
```
## <a name="next-steps"></a>Nächste Schritte

* Informationen zur Problembehandlung in Log Analytics finden Sie unter [Verwalten von Nutzung und Kosten mit Azure Monitor-Protokollen](../azure-monitor/platform/manage-cost-storage.md#troubleshooting-why-log-analytics-is-no-longer-collecting-data).
* Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Azure Monitor-Protokolle finden Sie unter [Protokollsuchen in Azure Monitor-Protokolle](../log-analytics/log-analytics-log-searches.md).
* Unter [Runbookausgabe und -meldungen](automation-runbook-output-and-messages.md) erfahren Sie, wie Sie die Ausgabe und Fehlermeldungen von Runbooks erstellen und abrufen.
* Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md).
* Weitere Informationen zu Azure Monitor-Protokolle und Datensammlungsquellen finden Sie unter [Sammeln von Azure Storage-Daten in Azure Monitor-Protokolle – Übersicht](../azure-monitor/platform/collect-azure-metrics-logs.md).