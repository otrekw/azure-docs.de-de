---
title: Anzeigen von Azure-Aktivitätsprotokollereignissen in Azure Monitor
description: Zeigen Sie das Azure-Aktivitätsprotokoll in Azure Monitor an, und rufen Sie es mit PowerShell, der CLI und der REST-API ab.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 9df7593a9fd191d3a734fba5e81fb1aecba08345
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79234414"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen

Das [Azure-Aktivitätsprotokoll](platform-logs-overview.md) gewährt Einblick in Ereignisse auf Abonnementebene, die in Azure aufgetreten sind. Dieser Artikel enthält Informationen zu verschiedenen Methoden für das Anzeigen und Abrufen von Aktivitätsprotokollereignissen.

## <a name="azure-portal"></a>Azure-Portal
Zeigen Sie das Aktivitätsprotokoll für alle Ressourcen über das Menü **Monitor** im Azure-Portal an. Das Aktivitätsprotokoll für eine bestimmte Ressource können Sie über die Option **Aktivitätsprotokoll** im Menü dieser Ressource anzeigen.

![Anzeigen des Aktivitätsprotokolls](./media/activity-logs-overview/view-activity-log.png)

Sie können Aktivitätsprotokollereignisse nach folgenden Feldern filtern:

* **Zeitraum**: Die Start- und Endzeit für Ereignisse.
* **Kategorie:** Die Ereigniskategorie wie unter [Kategorien im Aktivitätsprotokoll](activity-log-view.md#categories-in-the-activity-log) beschrieben.
* **Abonnement**: Name von mindestens einem Azure-Abonnement.
* **Ressourcengruppe**: Mindestens eine Ressourcengruppe in den ausgewählten Abonnements.
* **Ressource (Name)** : Der Name einer bestimmten Ressource.
* **Ressourcentyp:** Der Typ der Ressource, z.B. _Microsoft.Compute/virtualmachines_.
* **Vorgangsname**: Der Name eines Azure Resource Manager-Vorgangs, z.B. _Microsoft.SQL/servers/Write_.
* **Schweregrad**: Der Schweregrad des Ereignisses. Verfügbare Werte sind _Information_, _Warnung_, _Fehler_ und _Kritisch_.
* **Ereignis initiiert von:** Der Benutzer, der den Vorgang durchgeführt hat.
* **Suche öffnen**: Öffnet ein Textsuchfeld, mit dem in allen Feldern aller Ereignisse nach dieser Zeichenfolge gesucht wird.

## <a name="categories-in-the-activity-log"></a>Kategorien im Aktivitätsprotokoll
Jedes Ereignis im Aktivitätsprotokoll verfügt über eine bestimmte Kategorie. Die Kategorien sind in der folgenden Tabelle beschrieben. Umfassende Informationen zu den Schemas dieser Kategorien finden Sie unter [Ereignisschema des Azure-Aktivitätsprotokolls](activity-log-schema.md). 

| Category | BESCHREIBUNG |
|:---|:---|
| Administrative | Enthält die Datensätze aller Erstellungs-, Aktualisierungs-, Lösch- und Aktionsvorgänge, die über Resource Manager ausgeführt wurden. Beispiele für Verwaltungsereignisse sind das _Erstellen des virtuellen Computers_ und das _Löschen der Netzwerksicherheitsgruppe_.<br><br>Jede Aktion, die von einem Benutzer oder einer Anwendung mit Resource Manager durchgeführt wird, wird als Vorgang basierend auf einem bestimmten Ressourcentyp modelliert. Wenn der Vorgangstyp _Schreiben_, _Löschen_ oder _Aktion_ lautet, werden die Datensätze zum Start und zum Erfolg oder Fehler dieses Vorgangs in der Kategorie „Verwaltung“ aufgezeichnet. Verwaltungsereignisse umfassen außerdem alle Änderungen an der rollenbasierten Zugriffssteuerung in einem Abonnement. |
| Dienstintegrität | Enthält Datensätze zu allen Incidents im Zusammenhang mit der Dienstintegrität, die in Azure aufgetreten sind. Beispiel für ein Service Health-Ereignis: _Ausfallzeiten bei SQL Azure in der Region „USA, Osten“_ . <br><br>Es gibt sechs Typen von Service Health-Ereignissen: _Aktion erforderlich_, _Unterstützte Wiederherstellung_, _Incident_, _Wartung_, _Informationen_ und _Sicherheit_. Diese Ereignisse werden nur erstellt, wenn Sie über eine Ressource im Abonnement verfügen, die vom Ereignis betroffen wäre.
| Resource Health | Enthält Datensätze zu allen Ereignissen im Zusammenhang mit der Ressourcenintegrität, die für Ihre Azure-Ressourcen aufgetreten sind. Ein Beispiel für ein Resource Health-Ereignis ist _Integritätsstatus des virtuellen Computers ist zu „Nicht verfügbar“ gewechselt_.<br><br>Resource Health-Ereignisse können über einen von vier Integritätsstatus verfügen: _Verfügbar_, _Nicht verfügbar_, _Heruntergestuft_ und _Unbekannt_. Darüber hinaus können Resource Health-Ereignisse kategorisiert werden. Hierbei sind die Kategorien _Von der Plattform initiiert_ und _Vom Benutzer initiiert_ verfügbar. |
| Warnung | Enthält den Datensatz mit den Aktivierungen für Azure-Warnungen. Ein Beispiel für ein Warnungsereignis ist _CPU-Auslastung auf ‚myVM‘ liegt in den letzten 5 Minuten über 80_.|
| Autoscale | Enthält Datensätze zu Ereignissen im Zusammenhang mit der Engine für die Autoskalierung – basierend auf den Einstellungen für die Autoskalierung, die Sie in Ihrem Abonnement definiert haben. Ein Beispiel für ein Ereignis der Autoskalierung ist _Fehler beim automatischen Hochskalieren_. |
| Empfehlung | Enthält Empfehlungsereignisse von Azure Advisor. |
| Sicherheit | Enthält den Datensatz, der von allen Warnungen in Azure Security Center generiert wurde. Ein Beispiel für ein Sicherheitsereignis ist _Verdächtige Datei mit doppelter Erweiterung ausgeführt_. |
| Richtlinie | Enthält Datensätze aller Aktionsvorgänge für Auswirkungen, die von Azure Policy ausgeführt werden. Beispiele für Policy-Ereignisse sind _Überwachen_ und _Ablehnen_. Jede Aktion, die von Policy ausgeführt wird, ist als ein Vorgang für eine Ressource modelliert. |

## <a name="view-change-history"></a>Anzeigen des Änderungsverlaufs

Bei der Überprüfung des Aktivitätsprotokolls kann es hilfreich sein zu wissen, welche Änderungen um die Zeit eines bestimmten Ereignisses stattfanden. Diese Informationen können Sie mit dem **Änderungsverlauf** anzeigen. Wählen Sie aus dem Aktivitätsprotokoll ein Ereignis aus, das Sie eingehender überprüfen möchten. Wählen Sie die Registerkarte **Änderungsverlauf (Vorschau)** aus, um alle Änderungen anzuzeigen, die mit diesem Ereignis verknüpft sind.

![Ändern der Verlaufsliste für ein Ereignis](media/activity-logs-overview/change-history-event.png)

Wenn Änderungen zu dem Ereignis vorhanden sind, sehen Sie eine Liste der Änderungen, aus denen Sie eine Auswahl treffen können. Daraufhin öffnet sich die Seite **Änderungsverlauf (Vorschau)** . Auf dieser Seite sehen Sie die Änderungen an der Ressource. Wie Sie im folgenden Beispiel erkennen können, ist nicht nur zu sehen, dass sich die Größe des virtuellen Computer geändert hat, sondern es wird auch die Größe des virtuellen Computers vor und nach der Änderung angezeigt.

![Seite „Änderungsverlauf“ mit den Unterschieden](media/activity-logs-overview/change-history-event-details.png)

Weitere Informationen zum Änderungsverlauf finden Sie unter [Abrufen von Ressourcenänderungen](../../governance/resource-graph/how-to/get-resource-changes.md).






## <a name="powershell"></a>PowerShell
Verwenden Sie das Cmdlet [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog), um das Aktivitätsprotokoll über PowerShell abzurufen. Im Folgenden sind einige allgemeine Beispiele aufgeführt.

> [!NOTE]
> `Get-AzLog` stellt nur den Verlauf für 15 Tage bereit. Verwenden Sie den Parameter **-MaxEvents**, um die letzten N Ereignisse für mehr als 15 Tage abzufragen. Verwenden Sie für den Zugriff auf Ereignisse, die älter als 15 Tage sind, die REST-API oder das SDK. Wenn Sie **StartTime** nicht angeben, ist der Standardwert **EndTime** minus 1 Stunde. Wenn Sie **EndTime**nicht angeben, ist der Standardwert die aktuelle Zeit. Alle Zeitangaben sind in UTC.


Abrufen von Protokolleinträgen, die nach einem bestimmten Zeitpunkt erstellt wurden:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Abrufen von Protokolleinträgen in einem Datums-/Uhrzeitbereich:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Abrufen von Protokolleinträgen aus einer bestimmten Ressourcengruppe:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Abrufen von Protokolleinträgen von einem bestimmten Ressourcenanbieter in einem Datums-/Uhrzeitbereich:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Abrufen von Protokolleinträgen mit einem bestimmten Aufrufer:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Abrufen der letzten 1000 Ereignisse:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
Verwenden Sie [az monitor activity-log](cli-samples.md#view-activity-log-for-a-subscription), um das Aktivitätsprotokoll über die CLI abzurufen. Im Folgenden sind einige allgemeine Beispiele aufgeführt.


Anzeigen aller verfügbaren Optionen:

```azurecli
az monitor activity-log list -h
```

Abrufen von Protokolleinträgen aus einer bestimmten Ressourcengruppe:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Abrufen von Protokolleinträgen mit einem bestimmten Aufrufer:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Abrufen von Protokollen nach Aufrufer für einen Ressourcentyp innerhalb eines Datumsbereichs:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>REST-API
Verwenden Sie die [Azure Monitor-REST-API](https://docs.microsoft.com/rest/api/monitor/), um das Aktivitätsprotokoll über einen REST-Client abzurufen. Im Folgenden sind einige allgemeine Beispiele aufgeführt.

Abrufen von Aktivitätsprotokollen mit Filter:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Abrufen von Aktivitätsprotokollen mit Filter und Auswahlfunktion:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Abrufen von Aktivitätsprotokollen mit Auswahlfunktion:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Abrufen von Aktivitätsprotokollen ohne Filter oder Auswahlfunktion:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="activity-logs-analytics-monitoring-solution"></a>Überwachungslösung der Aktivitätsprotokollanalyse
Die Azure Log Analytics-Überwachungslösung umfasst mehrere Protokollabfragen und Ansichten zum Analysieren der Aktivitätsprotokolldatensätze in Ihrem Log Analytics-Arbeitsbereich.

### <a name="prerequisites"></a>Voraussetzungen
Sie müssen eine Diagnoseeinstellung erstellen, um das Aktivitätsprotokoll für Ihr Abonnement an einen Log Analytics-Arbeitsbereich zu senden. Informationen dazu finden Sie unter [Erfassen von Protokollen der Azure-Plattform im Log Analytics-Arbeitsbereich in Azure Monitor](resource-logs-collect-workspace.md).

### <a name="install-the-solution"></a>Installieren der Lösung
Verwenden Sie das unter [Installieren einer Überwachungslösung](../insights/solutions.md#install-a-monitoring-solution) beschriebene Verfahren, um die Überwachungslösung der **Aktivitätsprotokollanalyse** zu installieren. Es ist keine zusätzliche Konfiguration erforderlich.

### <a name="use-the-solution"></a>Verwenden der Lösung
Klicken Sie am oberen Rand der Seite **Aktivitätsprotokoll** auf **Protokolle**, um die [Überwachungslösung der Aktivitätsprotokollanalyse](activity-log-collect.md) für das Abonnement zu öffnen. Sie können auch über das Menü **Überwachen** im Azure-Portal auf alle Überwachungslösungen Ihres Abonnements zugreifen. Wählen Sie im Abschnitt **Insights** die Option **Mehr** aus, um die Seite **Übersicht** mit den Lösungskacheln zu öffnen. Die Kachel **Azure-Aktivitätsprotokolle** zeigt die Anzahl der **AzureActivity**-Datensätze in Ihrem Arbeitsbereich.

![Kachel „Azure-Aktivitätsprotokolle“](media/collect-activity-logs/azure-activity-logs-tile.png)


Klicken Sie auf die Kachel **Azure-Aktivitätsprotokolle**, um die Ansicht **Azure-Aktivitätsprotokolle** zu öffnen. Die Ansicht enthält die Visualisierungskomponenten, die in der folgenden Tabelle aufgeführt sind. Für jede Komponente sind bis zu 10 Einträge aufgeführt, die den Kriterien dieser Komponente für den angegebenen Zeitraum entsprechen. Sie können eine Protokollabfrage ausführen, die alle übereinstimmenden Datensätze zurückgibt. Dazu klicken Sie am unteren Rand der Komponente auf **Alle anzeigen**.

![Dashboard „Azure-Aktivitätsprotokolle“](media/collect-activity-logs/activity-log-dash.png)

| Visualisierungskomponente | BESCHREIBUNG |
| --- | --- |
| Azure Activity Log Entries (Azure-Aktivitätsprotokolleinträge) | Zeigt ein Balkendiagramm aller Datensätze des ersten Azure-Aktivitätsprotokolleintrags für den ausgewählten Datumsbereich sowie eine Liste der ersten zehn Aktivitätsaufrufer an. Klicken Sie auf das Balkendiagramm, um eine Protokollsuche für `AzureActivity` durchzuführen. Klicken Sie auf ein Aufruferelement, um eine Protokollsuche durchzuführen, mit der alle Aktivitätsprotokolleinträge für das Element zurückgegeben werden. |
| Activity Logs by Status (Aktivitätsprotokolle nach Status) | Zeigt ein Ringdiagramm für den Status der Azure-Aktivitätsprotokolle für den ausgewählten Datumsbereich sowie eine Liste der ersten zehn Statuseinträge an. Klicken Sie auf das Diagramm, um eine Protokollabfrage für `AzureActivity | summarize AggregatedValue = count() by ActivityStatus` durchzuführen. Klicken Sie auf ein Statuselement, um eine Protokollsuche durchzuführen, mit der alle Aktivitätsprotokolleinträge für das Statuselement zurückgegeben werden. |
| Activity Logs by Resource (Aktivitätsprotokolle nach Ressource) | Zeigt die Gesamtanzahl der Ressourcen mit Aktivitätsprotokollen an und listet die ersten zehn Ressourcen mit der Anzahl der Datensätze für jede Ressource auf. Klicken Sie auf den Gesamtbereich, um eine Protokollsuche für `AzureActivity | summarize AggregatedValue = count() by Resource` durchzuführen, in der alle für die Lösung verfügbaren Azure-Ressourcen angezeigt werden. Klicken Sie auf eine Ressource, um eine Protokollabfrage durchzuführen, mit der alle Aktivitätsdatensätze für die Ressource zurückgegeben werden. |
| Activity Logs by Resource Provider (Aktivitätsprotokolle nach Ressourcenanbieter) | Zeigt die Gesamtanzahl der Ressourcenanbieter an, die Aktivitätsprotokolle generieren, und listet die ersten zehn Anbieter auf. Klicken Sie auf den Gesamtbereich, um eine Protokollabfrage für `AzureActivity | summarize AggregatedValue = count() by ResourceProvider` durchzuführen, in der alle Azure-Ressourcenanbieter angezeigt werden. Klicken Sie auf einen Ressourcenanbieter, um eine Protokollabfrage durchzuführen, mit der alle Aktivitätsdatensätze für den Anbieter zurückgegeben werden. |




## <a name="next-steps"></a>Nächste Schritte

* [Lesen einer Übersicht über Plattformprotokolle](platform-logs-overview.md)
* [Erstellen einer Diagnoseeinstellung zum Senden von Aktivitätsprotokollen an andere Ziele](diagnostic-settings.md)
