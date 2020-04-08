---
title: Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie einen Log Analytics-Arbeitsbereich löschen, wenn Sie einen Arbeitsbereich in einem persönlichen Abonnement erstellt haben oder Ihr Arbeitsbereichsmodell neu strukturieren.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/14/2020
ms.openlocfilehash: 1dceb3db4572ecdaf504745dba1099a5eccead43
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80395783"
---
# <a name="delete-and-recover-azure-log-analytics-workspace"></a>Löschen und Wiederherstellen eines Azure Log Analytics-Arbeitsbereichs

In diesem Artikel werden das Konzept des vorläufigen Löschens eines Azure Log Analytics-Arbeitsbereichs und die Wiederherstellung eines gelöschten Arbeitsbereichs erläutert. 

## <a name="considerations-when-deleting-a-workspace"></a>Überlegungen zum Löschen eines Arbeitsbereichs

Wenn Sie einen Log Analytics-Arbeitsbereich löschen, wird ein vorläufiger Löschvorgang durchgeführt, um die Wiederherstellung des Arbeitsbereichs einschließlich der zugehörigen Daten und verbundenen Agents innerhalb von 14 Tagen zu ermöglichen, unabhängig davon, ob der Löschvorgang versehentlich oder gezielt durchgeführt wurde. Nach Ablauf des Zeitraums für vorläufiges Löschen können die Arbeitsbereichsressource und die zugehörigen Daten nicht mehr wiederhergestellt werden. Die Daten werden in die Warteschlange zum dauerhaften Löschen gestellt und innerhalb von 30 Tagen vollständig gelöscht. Der Arbeitsbereichsname wird „freigegeben“ und kann zum Erstellen eines neuen Arbeitsbereichs verwendet werden.

> [!NOTE]
> Wenn Sie das vorläufige Löschen außer Kraft setzen und den Arbeitsbereich dauerhaft löschen möchten, führen Sie die Schritte unter [Dauerhaftes Löschen eines Arbeitsbereichs](#permanent-workspace-delete) aus.

Gehen Sie beim Löschen eines Arbeitsbereichs vorsichtig vor, da er unter Umständen wichtige Daten und Konfigurationen enthält, deren Löschung sich negativ auf den Dienstvorgang auswirken kann. Überprüfen Sie die Agents, Lösungen und anderen Azure-Dienste und Quellen, deren Daten in Log Analytics gespeichert werden, z. B.:

* Verwaltungslösungen
* Azure-Automatisierung
* Auf virtuellen Windows- und Linux-Computern ausgeführte Agents
* Auf Windows- und Linux-Computern in Ihrer Umgebung ausgeführte Agents
* System Center Operations Manager

Beim vorläufigen Löschen wird die Arbeitsbereichsressource gelöscht und die Berechtigung aller zugeordneten Benutzer aufgehoben. Wenn Benutzer anderen Arbeitsbereichen zugeordnet sind, können sie Log Analytics mit diesen Arbeitsbereichen weiter nutzen.

## <a name="soft-delete-behavior"></a>Verhalten des vorläufigen Löschens

Mit dem Löschvorgang des Arbeitsbereichs wird die Resource Manager-Ressource des Arbeitsbereichs gelöscht, die zugehörige Konfiguration und die zugehörigen Daten werden jedoch 14 Tage lang beibehalten, obwohl der Arbeitsbereich dem Anschein nach gelöscht ist. Alle für die Berichterstellung im Arbeitsbereich konfigurierten Agents und System Center Operations Manager-Verwaltungsgruppen werden während des Zeitraums des vorläufigen Löschens in einem verwaisten Status beibehalten. Der Dienst umfasst darüber hinaus einen Mechanismus zur Wiederherstellung des gelöschten Arbeitsbereichs einschließlich der zugehörigen Daten und verbundenen Ressourcen, bei dem der Löschvorgang im Wesentlichen rückgängig gemacht wird.

> [!NOTE] 
> Installierte Lösungen und verknüpfte Dienste wie Ihr Azure Automation-Konto werden zum Zeitpunkt der Löschung dauerhaft aus dem Arbeitsbereich entfernt und können nicht wiederhergestellt werden. Diese sollten nach dem Wiederherstellungsvorgang neu konfiguriert werden, um den vorherigen konfigurierten Zustand des Arbeitsbereichs wiederherzustellen.

Sie können einen Arbeitsbereich mithilfe von [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/remove-azurermoperationalinsightsworkspace?view=azurermps-6.13.0), über die [REST-API](https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) oder im [Azure-Portal](https://portal.azure.com) löschen.

### <a name="azure-portal"></a>Azure-Portal

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich an. 
2. Wählen Sie im Azure-Portal **Alle Dienste** aus. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics-Arbeitsbereiche** aus.
3. Wählen Sie in der Liste der Log Analytics-Arbeitsbereiche einen Arbeitsbereich aus, und klicken Sie dann oben im mittleren Bereich auf **Löschen**.
   ![Löschoption im Bereich mit den Arbeitsbereichseigenschaften](media/delete-workspace/log-analytics-delete-workspace.png)
4. Wenn das Fenster mit einer Meldung angezeigt wird, in der Sie zum Bestätigen der Löschung des Arbeitsbereichs aufgefordert werden, klicken Sie auf **Ja**.
   ![Bestätigen des Löschvorgangs für den Arbeitsbereich](media/delete-workspace/log-analytics-delete-workspace-confirm.png)

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Remove-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

### <a name="troubleshooting"></a>Problembehandlung

Zum Löschen eines Log Analytics-Arbeitsbereichs sind Berechtigungen vom Typ „Log Analytics-Mitwirkender“ erforderlich.<br>
Wenn Sie beim Erstellen eines Arbeitsbereichs die Fehlermeldung *Dieser Arbeitsbereichsname wird bereits verwendet* erhalten, kann dies folgende Gründe haben:
* Der Name des Arbeitsbereichs ist nicht verfügbar und wird bereits von jemandem in Ihrer Organisation oder von einem anderen Kunden verwendet.
* Der Arbeitsbereich wurde innerhalb der letzten 14 Tage gelöscht, und der Name wurde für den Zeitraum der vorläufigen Löschung reserviert. Zum Überschreiben der vorübergehenden Löschung, dem sofortigen Löschen des Arbeitsbereichs und dem Erstellen eines neuen, gleichnamigen Arbeitsbereichs, gehen Sie folgendermaßen vor, um den Arbeitsbereich zunächst wiederherzustellen und dann dauerhaft zu löschen:<br>
   1. [Stellen Sie Ihren Arbeitsbereich wieder her.](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#recover-workspace)
   2. [Löschen Sie Ihren Arbeitsbereich dauerhaft.](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace#permanent-workspace-delete)
   3. Erstellen Sie einen neuen Arbeitsbereich mit demselben Arbeitsbereichnamen.


## <a name="permanent-workspace-delete"></a>Dauerhaftes Löschen eines Arbeitsbereichs
Die Methode des vorläufigen Löschens ist in einigen Szenarien möglicherweise nicht geeignet, in denen Sie eine Bereitstellung mit denselben Einstellungen und demselben Arbeitsbereichsnamen wiederholen müssen (z.B. bei Entwicklung und Test). In solchen Fällen können Sie den Arbeitsbereich dauerhaft löschen und den Zeitraum der vorläufigen Löschung überschreiben. Bei der dauerhaften Löschung des Arbeitsbereichs wird der Arbeitsbereichsname freigegeben, und Sie können einen neuen Arbeitsbereich mit demselben Namen erstellen.


> [!IMPORTANT]
> Verwenden Sie dauerhafte Löschungen von Arbeitsbereichen mit Vorsicht, da dieser Vorgang nicht rückgängig gemacht werden kann. Sie können dann weder den Arbeitsbereich noch dessen Daten wiederherstellen.

Das dauerhafte Löschen eines Arbeitsbereichs kann derzeit über die REST-API vorgenommen werden.

> [!NOTE]
> Jede API-Anforderung muss ein Bearer-Autorisierungstoken im Anforderungsheader enthalten.
>
> Sie können das Token mit folgenden Aktionen abrufen:
> - [App-Registrierungen](https://docs.microsoft.com/graph/auth/auth-concepts#access-tokens)
> - Navigieren Sie im Browser über die Entwicklerkonsole (F12) zum Azure-Portal. Suchen Sie in einer der **batch?** -Instanzen nach der Authentifizierungszeichenfolge unter **Anforderungsheader**. Diese entspricht dem Muster *authorization: Bearer <token>* . Kopieren Sie die Zeichenfolge, und fügen Sie sie Ihrem API-Aufruf hinzu, wie es in den Beispielen gezeigt wird.
> - Navigieren Sie zur Azure-REST-Dokumentationswebsite. Wählen Sie für eine beliebige API die Option **Ausprobieren** aus, kopieren Sie das Bearertoken, und fügen Sie es Ihrem API-Aufruf hinzu.
Verwenden Sie zum dauerhaften Löschen Ihres Arbeitsbereichs den REST-API-Aufruf [Workspaces - Delete]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/delete) mit einem Erzwingungstag:
>
> ```rst
> DELETE https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/workspaces/<workspace-name>?api-version=2015-11-01-preview&force=true
> Authorization: Bearer eyJ0eXAiOiJKV1Qi….
> ```
Wobei „eyJ0eXAiOiJKV1Qi…“ das vollständige Autorisierungstoken darstellt.

## <a name="recover-workspace"></a>Wiederherstellen eines Arbeitsbereichs

Wenn Sie über Berechtigungen vom Typ „Mitwirkender“ für das Abonnement und die Ressourcengruppe verfügen, denen der Arbeitsbereich vor dem vorläufigen Löschvorgang zugeordnet war, können Sie den Arbeitsbereich einschließlich der zugehörigen Daten, der Konfiguration und der verbundenen Agents während des Zeitraums des vorläufigen Löschens wiederherstellen. Nach dem Zeitraum des vorläufigen Löschens kann der Arbeitsbereich nicht mehr wiederhergestellt werden und wird zum dauerhaften Löschen zugewiesen. Die Namen von gelöschten Arbeitsbereichen werden während des Zeitraums des vorläufigen Löschens beibehalten und können bei dem Versuch, einen neuen Arbeitsbereich zu erstellen, nicht verwendet werden.  

Sie können einen Arbeitsbereich wiederherstellen, indem Sie ihn mithilfe einer der folgenden Methoden zum Erstellen von Arbeitsbereichen neu erstellen: [PowerShell](https://docs.microsoft.com/powershell/module/az.operationalinsights/New-AzOperationalInsightsWorkspace) oder [REST-API]( https://docs.microsoft.com/rest/api/loganalytics/workspaces/createorupdate), sofern die folgenden Eigenschaften mit den Details des gelöschten Arbeitsbereichs gefüllt sind:

* Abonnement-ID
* Ressourcengruppenname
* Arbeitsbereichname
* Region

### <a name="powershell"></a>PowerShell
```PowerShell
PS C:\>Select-AzSubscription "subscription-name-the-workspace-was-in"
PS C:\>New-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name-the-workspace-was-in" -Name "deleted-workspace-name" -Location "region-name-the-workspace-was-in"
```

Der Arbeitsbereich und alle zugehörigen Daten sind nach dem Wiederherstellungsvorgang wieder vorhanden. Lösungen und verknüpfte Dienste wurden dauerhaft aus dem Arbeitsbereich entfernt, als dieser gelöscht wurde, und sollten daher neu konfiguriert werden, um den zuvor konfigurierten Zustand des Arbeitsbereichs wiederherzustellen. Einige Daten können nach der Wiederherstellung des Arbeitsbereichs möglicherweise nicht abgefragt werden, ehe die zugehörigen Lösungen neu installiert und deren Schemas dem Arbeitsbereich hinzugefügt wurden.

> [!NOTE]
> * Die Wiederherstellung eines Arbeitsbereichs wird im [Azure-Portal](https://portal.azure.com) nicht unterstützt. 
> * Beim erneuten Erstellen eines Arbeitsbereichs während des Zeitraums des vorläufigen Löschens wird darauf hingewiesen, dass der Name des Arbeitsbereichs bereits verwendet wird. 
> 
