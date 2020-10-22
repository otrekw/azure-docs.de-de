---
title: Abfragen von Daten in Azure Monitor mit Azure Data Explorer (Vorschau)
description: Verwenden Sie Azure Data Explorer, um produktübergreifende Abfragen zwischen Azure Data Explorer, Log Analytics-Arbeitsbereichen und klassischen Application Insights-Anwendungen in Azure Monitor auszuführen.
author: orens
ms.author: bwren
ms.reviewer: bwren
ms.subservice: logs
ms.topic: conceptual
ms.date: 10/13/2020
ms.openlocfilehash: 8a503a5456fc28bd1b3ebb69c784fc59b3c6e7df
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92049577"
---
# <a name="query-data-in-azure-monitor-using-azure-data-explorer-preview"></a>Abfragen von Daten in Azure Monitor mit Azure Data Explorer (Vorschau)
Der Azure Data Explorer-Proxycluster ermöglicht Ihnen das Ausführen produktübergreifender Abfragen zwischen Azure Data Explorer, Log Analytics-Arbeitsbereichen und klassischen Application Insights-Anwendungen in Azure Monitor. Sie können Log Analytics-Arbeitsbereiche in Azure Monitor oder klassische Application Insights-Apps als Proxycluster zuordnen. Anschließend können Sie den Proxycluster mit Azure Data Explorer-Tools abfragen und in einer clusterübergreifenden Abfrage auf ihn verweisen. In dem Artikel wird erläutert, wie eine Verbindung mit einem Proxycluster hergestellt wird, der Azure Data Explorer-Webbenutzeroberfläche ein Proxycluster hinzugefügt wird und über Azure Data Explorer Abfragen für Ihre Log Analytics-Arbeitsbereiche oder klassischen Application Insights-Apps ausgeführt werden.

Die folgende Abbildung zeigt den Azure Data Explorer-Proxyflow:

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-monitor-proxy-flow.png" alt-text="Azure Data Explorer-Proxyflow":::


> [!NOTE]
> Der Azure Data Explorer-Proxy befindet sich in der Public Preview. [Stellen Sie eine Verbindung mit dem Proxy her](#connect-to-the-proxy), um das Proxyfeature für Ihre Cluster zu aktivieren. 

## <a name="connect-to-the-proxy"></a>Herstellen einer Verbindung mit dem Proxy
Öffnen Sie die [Azure Data Explorer-Webbenutzeroberfläche](https://dataexplorer.azure.com/clusters), um eine Verbindung mit Ihrem Log Analytics-Arbeitsbereich oder Ihrer klassischen Application Insights-App herzustellen. Überprüfen Sie vor dem Herstellen der Verbindung mit dem Log Analytics- oder Application Insights-Cluster, ob im linken Menü der native Azure Data Explorer-Cluster (z. B. der *Hilfecluster*) angezeigt wird.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-data-explorer-web-ui-help-cluster.png" alt-text="Azure Data Explorer-Proxyflow":::

Klicken Sie auf **Add Cluster** (Cluster hinzufügen), und fügen Sie dann die URL des Log Analytics- oder Application Insights-Clusters in einem der folgenden Formate hinzu. 
    
* Für Log Analytics: `https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`
* Für Application Insights: `https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`

Klicken Sie auf **Hinzufügen**, um die Verbindung herzustellen.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-add-cluster.png" alt-text="Azure Data Explorer-Proxyflow":::
 
> [!NOTE]
> Wenn Sie eine Verbindung mit mehreren Proxyclustern hinzufügen, geben Sie jedem Cluster einen anderen Namen. Andernfalls haben alle Cluster im linken Bereich den gleichen Namen.

Nachdem die Verbindung hergestellt wurde, wird Ihr Log Analytics- oder Application Insights-Cluster im linken Bereich mit Ihrem nativen Azure Data Explorer-Cluster angezeigt. 

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-azure-data-explorer-clusters.png" alt-text="Azure Data Explorer-Proxyflow":::
 
> [!NOTE]
> Die Anzahl von Azure Monitor-Arbeitsbereichen, die zugeordnet werden können, ist auf 100 beschränkt.

## <a name="create-queries-using-azure-monitor-data"></a>Erstellen von Abfragen mithilfe von Azure Monitor-Daten

Sie können die Abfragen mithilfe von Clienttools ausführen, die Kusto-Abfragen unterstützen, z. B.: Kusto Explorer, Azure Data Explorer-Webbenutzeroberfläche, Jupyter Kqlmagic, Flow, PowerQuery, PowerShell, Jarvis, Lens und REST-API.

> [!NOTE]
> Das Azure Data Explorer-Proxyfeature wird nur für den Datenabruf verwendet. Weitere Informationen finden Sie unter [Funktionsunterstützbarkeit](#function-supportability).

> [!TIP]
> * Datenbanknamen müssen den gleichen Namen wie die im Proxycluster angegebene Ressource haben. Bei Namen wird die Groß-/Kleinschreibung beachtet.
> * Achten Sie bei clusterübergreifenden Abfragen auf eine korrekte Benennung der Application Insights-Apps und Log Analytics-Arbeitsbereiche.
>     * Wenn Namen Sonderzeichen enthalten, werden diese im Proxyclusternamen durch URL-Codierung ersetzt. 
>     * Wenn Namen Zeichen enthalten, die nicht den [KQL-Regeln für Bezeichnernamen](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/entity-names) entsprechen, werden sie durch einen Bindestrich ( **-** ) ersetzt.

### <a name="direct-query-from-your-log-analytics-or-application-insights-proxy-cluster"></a>Direkte Abfrage über Log Analytics- oder Application Insights-Proxycluster

Führen Sie Abfragen für Ihre Log Analytics- oder Application Insights-Cluster aus. Vergewissern Sie sich, dass Ihr Cluster im linken Bereich ausgewählt ist. 
 
```kusto
Perf | take 10 // Demonstrate query through the proxy on the Log Analaytics workspace
```

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-query-la.png" alt-text="Azure Data Explorer-Proxyflow":::

### <a name="cross-query-of-your-log-analytics-or-application-insights-proxy-cluster-and-the-azure-data-explorer-native-cluster"></a>Übergreifende Abfrage des Log Analytics- oder Application Insights-Proxyclusters und des nativen Azure Data Explorer-Clusters

Wenn Sie über den Proxy clusterübergreifende Abfragen ausführen, überprüfen Sie, ob der native Azure Data Explorer-Cluster im linken Bereich ausgewählt ist. In den folgenden Beispielen wird veranschaulicht, wie Sie Azure Data Explorer-Clustertabellen mithilfe des [union](/azure/data-explorer/kusto/query/unionoperator)-Operators mit einem Log Analytics-Arbeitsbereich kombinieren.

```kusto
union StormEvents, cluster('https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>').Perf
| take 10
```

```kusto
let CL1 = 'https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>';
union <Azure Data Explorer table>, cluster(CL1).database(<workspace-name>).<table name>
```
Wenn Sie anstelle von „union“ den [`join`-Operator](/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer) verwenden, ist möglicherweise ein [Hinweis](/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer#join-hints) erforderlich, um die Abfrage für den nativen Azure Data Explorer-Cluster (und nicht für den Proxy) auszuführen. 

### <a name="join-data-from-an-azure-data-explorer-cluster-in-one-tenant-with-an-azure-monitor-resource-in-another"></a>Verknüpfen von Daten eines Azure Data Explorer-Clusters in einem Mandanten mit einer Azure Monitor-Ressource in einem anderen Mandanten

Mandantenübergreifende Abfragen werden vom Azure Data Explorer-Proxy nicht unterstützt. Sie sind nur bei einem Mandanten angemeldet, um die Abfrage für beide Ressourcen auszuführen.

Wenn sich die Azure Data Explorer-Ressource im Mandanten „A“ und der Log Analytics-Arbeitsbereich im Mandanten „B“ befindet, sollten Sie eine der beiden folgenden Methoden verwenden:

- Mit Azure Data Explorer können Sie Rollen für Prinzipale auf unterschiedlichen Mandanten hinzufügen. Fügen Sie Ihre Benutzer-ID auf Mandant „B“ als autorisierten Benutzer im Azure Data Explorer-Cluster hinzu. Vergewissern Sie sich, dass die Eigenschaft *[ExternalTrustedTenant](https://docs.microsoft.com/powershell/module/az.kusto/update-azkustocluster)* im Azure Data Explorer-Cluster den Mandanten „B“ enthält. Führen Sie die übergreifende Abfrage vollständig auf Mandant „B“ aus.

- Verwenden Sie [Lighthouse](/azure/lighthouse/), um die Azure Monitor-Ressource auf Mandant „A“ zu projizieren.

### <a name="connect-to-azure-data-explorer-clusters-from-different-tenants"></a>Herstellen einer Verbindung mit Azure Data Explorer-Clustern von unterschiedlichen Mandanten

Bei Kusto-Explorer werden Sie automatisch bei dem Mandanten angemeldet, zu dem das Benutzerkonto ursprünglich gehört. Für den Zugriff auf Ressourcen in anderen Mandanten mit demselben Benutzerkonto muss `tenantId` explizit in der Verbindungszeichenfolge angegeben werden: `Data Source=https://ade.applicationinsights.io/subscriptions/SubscriptionId/resourcegroups/ResourceGroupName;Initial Catalog=NetDefaultDB;AAD Federated Security=True;Authority ID=`**TenantId**

## <a name="function-supportability"></a>Funktionsunterstützbarkeit

Der Azure Data Explorer-Proxycluster unterstützt Funktionen sowohl für Log Analytics als auch für Application Insights. Dadurch können clusterübergreifende Abfragen direkt auf eine tabellarische Azure Monitor-Funktion verweisen.

Der Proxy unterstützt die folgenden Befehle:

* `.show functions`
* `.show function {FunctionName}`
* `.show database {DatabaseName} schema as json`

Die folgende Abbildung enthält ein Beispiel für das Abfragen einer tabellarischen Funktion über die Webbenutzeroberfläche von Azure Data Explorer. Führen Sie zur Verwendung der Funktion den Namen im Abfragefenster aus.

:::image type="content" source="media/azure-data-explorer-monitor-proxy/azure-monitor-proxy-function-query.png" alt-text="Azure Data Explorer-Proxyflow":::
 
> [!NOTE]
> Azure Monitor unterstützt nur Tabellenfunktionen, die keine Parameter unterstützen.

## <a name="additional-syntax-examples"></a>Zusätzliche Syntaxbeispiele

Beim Aufrufen der Log Analytics- oder Application Insights-Cluster sind die folgenden Syntaxoptionen verfügbar:

|Syntaxbeschreibung  |Application Insights  |Log Analytics  |
|----------------|---------|---------|
| Datenbank in einem Cluster, der nur die definierte Ressource in diesem Abonnement enthält (**empfohlen für clusterübergreifende Abfragen**) |   cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>').database('<ai-app-name>`) | cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>').database('<workspace-name>`)     |
| Cluster, der alle Apps/Arbeitsbereiche in diesem Abonnement enthält    |     cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>`)    |    cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>`)     |
|Cluster, der alle Apps/Arbeitsbereiche im Abonnement enthält, und diese sind Mitglieder dieser Ressourcengruppe    |   cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |    cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>`)      |
|Cluster, der nur die definierte Ressource in diesem Abonnement enthält      |    cluster(`https://ade.applicationinsights.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.insights/components/<ai-app-name>`)    |  cluster(`https://ade.loganalytics.io/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>`)     |

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den Artikel zur [Datenstruktur von Log Analytics-Arbeitsbereichen und Application Insights](data-platform-logs.md).
- Informieren Sie sich über das [Schreiben von Abfragen für Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/write-queries).