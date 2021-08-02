---
title: Überwachen von Azure App Configuration
description: Beginnen Sie hier, um zu erfahren, wie Sie App Configuration überwachen können.
services: azure-app-configuration
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 05/05/2021
ms.openlocfilehash: 7edddaac52c569bf29c1be7e173ce966adb0c17b
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111969756"
---
# <a name="monitoring-app-configuration"></a>Überwachen von App Configuration
Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. 

In diesem Artikel werden die von App Configuration generierten Überwachungsdaten beschrieben. Von App Configuration wird [Azure Monitor](../azure-monitor/overview.md) verwendet. Wenn Sie nicht mit den Features von Azure Monitor vertraut sind, die Bestandteil aller Azure-Dienste sind, für die Azure Monitor verwendet wird, lesen Sie den Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).

## <a name="monitoring-overview-page-in-azure-portal"></a>Übersichtsseite zur Überwachung im Azure-Portal
Die Seite **Übersicht** im Azure-Portal enthält eine kurze Ansicht des Ressourceneinsatzes, z. B. die Gesamtzahl der Anforderungen, die Anzahl gedrosselter Anforderungen und die Anforderungsdauer pro Konfigurationsspeicher. Dies sind nützliche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Überwachungsdaten werden automatisch erfasst und sind für die Analyse verfügbar, sobald Sie die Ressource erstellen. Mit einigen Konfigurationsschritten können Sie zusätzliche Typen von Datensammlungen aktivieren.

> [!div class="mx-imgBorder"]
> ![Überwachung auf der Übersichtsseite](./media/monitoring-overview-page.png)

## <a name="monitoring-data"></a>Überwachungsdaten 
App Configuration sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die in [Überwachungsdaten von Azure-Ressourcen](../azure-monitor/essentials/monitor-azure-resource.md#monitoring-data) beschrieben werden. Ausführliche Informationen zu den von App Configuration erstellten Metriken und Protokollmetriken finden Sie in der [Referenz für App Configuration-Überwachungsdaten](./monitor-app-configuration-reference.md).

## <a name="collection-and-routing"></a>Sammlung und Routing
Plattformmetriken und das Aktivitätsprotokoll werden automatisch erfasst und gespeichert, können aber mithilfe einer Diagnoseeinstellung an andere Speicherorte geleitet werden.

Ressourcenprotokolle werden erst erfasst und gespeichert, sobald Sie eine Diagnoseeinstellung erstellt und an einen oder mehrere Standorte weitergeleitet haben. Wenn Sie beispielsweise Protokolle und Metriken für einen Konfigurationsspeicher in Azure Monitor nahezu in Echtzeit anzeigen möchten, erfassen Sie die Ressourcenprotokolle in einem Log Analytics-Arbeitsbereich. Wenn Sie noch keinen haben, erstellen Sie einen [Log Analytics-Arbeitsbereich](../azure-monitor/logs/quick-create-workspace.md), und führen Sie die folgenden Schritte aus, um eine Diagnoseeinstellung zu erstellen und zu aktivieren. 

 #### <a name="portal"></a>[Portal](#tab/portal)

1. Melden Sie sich beim Azure-Portal an.

1. Navigieren Sie zu Ihrem App Configuration-Speicher.

1. Wählen Sie im Abschnitt **Überwachung** die Option **Diagnoseeinstellungen** aus, und klicken Sie dann auf **+ Add diagnostic setting** (+ Diagnoseeinstellung hinzufügen). 
    > [!div class="mx-imgBorder"]
    > ![Hinzufügen einer Diagnoseeinstellung](./media/diagnostic-settings-add.png)

1. Geben Sie auf der Seite **Diagnoseeinstellung** einen Namen für Ihre Einstellung ein, und wählen Sie **HttpRequest** und dann das Ziel aus, an das die Protokolle gesendet werden sollen. Wählen Sie **Send to Log Analytics workspace** (An Log Analytics-Arbeitsbereich senden) aus, um sie an einen Log Analytics-Arbeitsbereich zu senden.
 
    > [!div class="mx-imgBorder"]
    > ![Details der Diagnoseeinstellungen](./media/monitoring-diagnostic-settings-details.png)

1. Geben Sie den Namen Ihres **Abonnements** und **Log Analytics-Arbeitsbereichs** ein. 
1. Wählen Sie **Speichern** aus, und überprüfen Sie, ob jetzt auf der Seite der Diagnoseeinstellungen Ihre neue Diagnoseeinstellung aufgeführt ist. 
    

 ### <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)
    
1. Öffnen Sie Azure Cloud Shell, oder falls Sie die Azure-Befehlszeilenschnittstelle lokal installiert haben, öffnen Sie eine Befehlskonsolenanwendung wie Windows PowerShell.

1. Wenn Ihre Identität mehreren Abonnements zugeordnet ist, legen Sie als aktives Abonnement das Abonnement des Speicherkontos fest, für das Sie Protokolle aktivieren möchten.

    ```Azure CLI
    az account set --subscription <your-subscription-id>
    ```

1. Aktivieren Sie Protokolle mit dem Befehl [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az_monitor_diagnostic_settings_create&preserve-view=true).

    ```Azure CLI
    az monitor diagnostic-settings create --name <setting-name> --workspace <log-analytics-workspace-resource-id> --resource <app-configuration-resource-id> --logs '[{"category": <category name>, "enabled": true "retentionPolicy": {"days": <days>, "enabled": <retention-bool}}]'
    ```

 ### <a name="powershell"></a>[PowerShell](#tab/PowerShell)
    
1. Öffnen Sie ein Windows PowerShell-Befehlsfenster, und melden Sie sich dann mit dem Befehl „Connect-AzAccount“ bei Ihrem Azure-Abonnement an. Befolgen Sie dann die Anweisungen auf dem Bildschirm.

    ```PowerShell
    Connect-AzAccount
    ```

1. Legen Sie als aktives Abonnement das Abonnement des App Configuration-Kontos fest, für das Sie die Protokollierung aktivieren möchten.

    ```PowerShell
    Set-AzContext -SubscriptionId <subscription-id>
    ```
    
1. Verwenden Sie das PowerShell-Cmdlet [Set-AzDiagnosticSetting](/previous-versions/azure/mt631625(v=azure.100)?redirectedfrom=MSDN), um Protokolle für einen Log Analytics-Arbeitsbereich zu aktivieren. 

    ```PowerShell
    Set-AzDiagnosticSetting -ResourceId <app-configuration-resource-id> -WorkspaceId <log-analytics-workspace-resource-id> -Enabled $true
    ```
1. Vergewissern Sie sich, dass die Diagnoseeinstellung richtig festgelegt ist und Protokollkategorien aktiviert sind. 

    ```PowerShell
    Get-AzureRmDiagnosticSetting -ResourceId <app-configuration-resource-id> 
    ```
---
Weitere Informationen zum Erstellen einer Diagnoseeinstellung über das Azure-Portal, die Befehlszeilenschnittstelle oder PowerShell finden Sie unter [Erstellen einer Diagnoseeinstellung zum Sammeln von Plattformprotokollen und Metriken in Azure](../azure-monitor/essentials/diagnostic-settings.md). 

Wenn Sie eine Diagnoseeinstellung erstellen, legen Sie fest, welche Kategorien von Protokollen gesammelt werden sollen. Weitere Informationen zu den Kategorien von Protokollen für App Configuration finden Sie unter [Referenz für App Configuration-Überwachungsdaten](./monitor-app-configuration-reference.md#resourcelogs).

## <a name="analyzing-metrics"></a>Analysieren von Metriken

Sie können Metriken für App Configuration mit Metriken aus anderen Azure-Diensten mithilfe des Metrik-Explorers analysieren, indem Sie im Menü **Azure Monitor** die Option **Metriken** öffnen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/essentials/metrics-getting-started.md). Für App Configuration werden die folgenden Metriken gesammelt: 

* Anzahl eingehender HTTP-Anforderungen 
* Dauer eingehender HTTP-Anforderungen 
* Anzahl gedrosselter Anforderungen (Antworten mit HTTP-Statuscode 429)

Navigieren Sie im Portal zum Abschnitt **Metriken**, und wählen Sie die **Metrik-Namespaces** und **Metriken** aus, die Sie analysieren möchten. Dieser Screenshot zeigt die Metrikansicht, wenn Sie die **Anzahl eingehender HTTP-Anforderungen** für den Konfigurationsspeicher auswählen.

> [!div class="mx-imgBorder"]
> ![Verwenden von App Configuration-Metriken](./media/monitoring-analyze-metrics.png)

Eine Liste der Plattformmetriken, die für App Configuration gesammelt werden, finden Sie unter [Referenz zu Metriken für App Configuration-Überwachungsdaten](./monitor-app-configuration-reference.md#metrics). Als Referenz können Sie auch eine Liste mit [allen in Azure Monitor unterstützten Ressourcenmetriken](../azure-monitor/essentials/metrics-supported.md) anzeigen.

## <a name="analyzing-logs"></a>Analysieren von Protokollen
Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz an eindeutigen Eigenschaften hat. Das allgemeine Schema wird in [Azure Monitor-Ressourcenprotokollschema](../azure-monitor/essentials/resource-logs-schema.md#top-level-common-schema) beschrieben. 

Das [Aktivitätsprotokoll](../azure-monitor/essentials/activity-log.md) ist ein Plattformprotokoll in Azure, das einen Einblick in Ereignisse auf Abonnementebene ermöglicht. Sie können es unabhängig anzeigen oder an Azure Monitor-Protokolle weiterleiten, in denen Sie mithilfe von Log Analytics viel komplexere Abfragen durchführen können.  
Eine Liste der Typen von Ressourcenprotokollen, die für App Configuration gesammelt werden, finden Sie unter [Referenz für App Configuration-Überwachungsdaten](./monitor-app-configuration-reference.md#resourcelogs). Eine Liste der Tabellen, die von Azure Monitor-Protokollen verwendet und von Log Analytics abgefragt werden können, finden Sie in der [Referenz für App Configuration-Überwachungsdaten](./monitor-app-configuration-reference.md#azuremonitorlogstables).  

>[!IMPORTANT]
> Wenn Sie im App Configuration-Menü die Option **Protokolle** auswählen, wird Log Analytics geöffnet, und der Abfragebereich ist auf die aktuelle App Configuration-Ressource festgelegt. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. 


Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Konfigurationen oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](/azure/azure-monitor/log-query/scope/).

Navigieren Sie im Portal zum Abschnitt **Protokolle** und dann zum Abfrage-Editor. Wählen Sie auf der linken Seite auf der Registerkarte **Tabellen** die Option **AACHttpRequest** aus, um die Protokolle Ihres Konfigurationsspeichers anzuzeigen. Geben Sie eine Kusto-Abfrage in den Editor ein, und die Ergebnisse werden unten angezeigt.  

> [!div class="mx-imgBorder"]
> ![Schreiben von Kusto-Abfragen in den Protokollen](./media/monitoring-writing-queries.png)

Die folgenden Beispielabfragen können Sie zur Überwachung von App Configuration-Ressourcen verwenden: 



* Auflisten aller HTTP-Anforderungen der letzten drei Tage 
    ```Kusto
       AACHttpRequest
        | where TimeGenerated > ago(3d)
    ```

* Auflisten aller gedrosselten Anforderungen (HTTP-Statuscode 429 für zu viele Anforderungen zurückgegeben) der letzten drei Tage 
    ```Kusto
       AACHttpRequest
        | where TimeGenerated > ago(3d)
        | where StatusCode == "429"
    ```

* Auflisten der Anzahl von Anforderungen, die in den letzten drei Tagen nach IP-Adresse gesendet wurden 
    ```Kusto
       AACHttpRequest
        | where TimeGenerated > ago(3d)
        | summarize requestCount= count() by ClientIPAddress
        | order by requestCount desc 
    ```

* Erstellen eines Kreisdiagramms der Typen von Statuscodes, die in den letzten drei Tagen empfangen wurden
    ```Kusto
       AACHttpRequest
        | where TimeGenerated > ago(3d)
        | summarize requestCount=count() by StatusCode
        | order by requestCount desc 
        | render piechart 
    ```

* Auflisten der Anzahl von Anforderungen, die nach Tag während der letzten 14 Tage gesendet wurden
    ```Kusto
    AACHttpRequest
        | where TimeGenerated > ago(14d)
        | extend Day = startofday(TimeGenerated)
        | summarize requestcount=count() by Day
        | order by Day desc  
    ```

## <a name="alerts"></a>Alerts

Azure Monitor-Warnungen informieren Sie proaktiv, wenn wichtige Bedingungen in Ihren Überwachungsdaten gefunden werden. Sie ermöglichen Ihnen, Probleme in Ihrem System zu identifizieren und zu beheben, bevor Ihre Kunden sie bemerken. Sie können Warnungen für [Metriken](../azure-monitor/alerts/alerts-metric-overview.md), [Protokolle](../azure-monitor/alerts/alerts-unified-log.md) und das [Aktivitätsprotokoll](../azure-monitor/alerts/activity-log-alerts.md) festlegen. Verschiedene Arten von Warnungen haben Vor- und Nachteile.
In der folgenden Tabelle sind allgemeine und empfohlene Warnungsregeln für App Configuration aufgeführt:

| Warnungstyp | Bedingung | BESCHREIBUNG  |
|:---|:---|:---|
|Ratenbegrenzung für HTTP-Anforderungen | Statuscode = 429  | Der Konfigurationsspeicher hat das [stündliche Anforderungskontingent überschritten](./faq.yml#are-there-any-limits-on-the-number-of-requests-made-to-app-configuration). Führen Sie ein Upgrade auf einen Standardspeicher durch, oder befolgen Sie die [Best Practices](./howto-best-practices.md#reduce-requests-made-to-app-configuration), um die Nutzung zu optimieren. |



## <a name="next-steps"></a>Nächste Schritte

* Eine Referenz zu den Metriken, Protokollen und anderen wichtigen Werten, die von App Configuration erstellt werden, finden Sie in der [Referenz für App Configuration-Überwachungsdaten](./monitor-app-configuration-reference.md).

* Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/essentials/monitor-azure-resource.md).