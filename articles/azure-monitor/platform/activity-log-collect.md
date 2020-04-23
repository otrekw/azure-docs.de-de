---
title: Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor
description: Erfassen Sie das Azure-Aktivitätsprotokoll in Azure Monitor-Protokollen, und verwenden Sie die Überwachungslösung, um das Azure-Aktivitätsprotokoll in all Ihren Azure-Abonnements zu analysieren und zu durchsuchen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/14/2020
ms.openlocfilehash: 098aeaa06a26c57744402722aa3eacc51ea85fb7
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382871"
---
# <a name="collect-and-analyze-azure-activity-log-in-azure-monitor"></a>Erfassen und Analysieren des Azure-Aktivitätsprotokolls in Azure Monitor
Das [Azure-Aktivitätsprotokoll](platform-logs-overview.md) ist ein [Plattformprotokoll](platform-logs-overview.md), das einen Einblick in Ereignisse auf Abonnementebene bietet, die in Azure aufgetreten sind. Zwar können Sie das Aktivitätsprotokoll im Azure-Portal anzeigen, doch sollten Sie es so konfigurieren, dass es an einen Log Analytics-Arbeitsbereich gesendet wird, um die Verwendung zusätzlicher Features von Azure Monitor zu ermöglichen. In diesem Artikel wird beschrieben, wie Sie diese Konfiguration ausführen und das Aktivitätsprotokoll an Azure Storage und Event Hubs senden.

Das Erfassen des Aktivitätsprotokolls in einem Log Analytics-Arbeitsbereich bietet folgende Vorteile:

- Keine Gebühren für die Erfassung oder Aufbewahrung von Aktivitätsprotokolldaten, die in einem Log Analytics-Arbeitsbereich gespeichert sind
- Korrelieren von Aktivitätsprotokolldaten mit anderen von Azure Monitor gesammelten Überwachungsdaten
- Verwenden von Protokollabfragen zum Ausführen komplexer Analysen und Erhalten tiefer Einblicke in Aktivitätsprotokolleinträge
- Verwenden von Protokollwarnungen mit Aktivitätseinträgen, die eine komplexere Warnungslogik ermöglichen
- Speichern von Aktivitätsprotokolleinträgen für mehr als 90 Tage
- Konsolidieren von Protokolleinträgen mehrerer Azure-Abonnements und -Mandanten an einem einzigen Ort zur gemeinsamen Analyse

> [!IMPORTANT]
> Für eine mandantenübergreifende Erfassung von Protokollen ist [Azure Lighthouse](/azure/lighthouse) erforderlich.

## <a name="collecting-activity-log"></a>Erfassen des Aktivitätsprotokolls
Das Aktivitätsprotokoll wird automatisch für die [Anzeige im Azure-Portal](activity-log-view.md) erfasst. Zur Erfassung in einem Log Analytics-Arbeitsbereich oder zum Senden an Azure Storage oder Event Hubs erstellen Sie eine [Diagnoseeinstellung](diagnostic-settings.md). Dieselbe Methode wird auch von Ressourcenprotokollen verwendet, sodass sie für alle [Plattformprotokolle](platform-logs-overview.md) konsistent ist.  

Wählen Sie zum Erstellen einer Diagnoseeinstellung für das Aktivitätsprotokoll in Azure Monitor im Menü **Aktivitätsprotokoll** die Option **Diagnoseeinstellungen** aus. Weitere Informationen zum Erstellen der Einstellung finden Sie unter [Erstellen einer Diagnoseeinstellung zum Erfassen von Plattformprotokollen und Metriken in Azure](diagnostic-settings.md). Eine Beschreibung der Kategorien, die gefiltert werden können, finden Sie unter [Kategorien im Aktivitätsprotokoll](activity-log-view.md#categories-in-the-activity-log). Wenn Sie über Legacyeinstellungen verfügen, stellen Sie sicher, dass Sie diese vor dem Erstellen einer Diagnoseeinstellung deaktivieren. Wenn beide Erfassungsmethoden aktiviert sind, kann dies zu doppelten Daten führen.

![Diagnoseeinstellungen](media/diagnostic-settings-subscription/diagnostic-settings.png)


> [!NOTE]
> Derzeit können Sie eine Diagnoseeinstellung auf Abonnementebene nur über das Azure-Portal und mithilfe einer Resource Manager-Vorlage erstellen. 


## <a name="legacy-settings"></a>Legacyeinstellungen 
Zwar sind Diagnoseeinstellungen die bevorzugte Methode zum Senden des Aktivitätsprotokolls an verschiedene Ziele, doch funktionieren Legacymethoden auch weiterhin, falls Sie diese nicht durch eine Diagnoseeinstellung ersetzen. Diagnoseeinstellungen bieten die folgenden Vorteile gegenüber den Legacymethoden, und es wird empfohlen, die Konfiguration zu aktualisieren:

- Konsistente Methode zum Erfassen aller Plattformprotokolle
- Erfassen des Aktivitätsprotokolls übergreifend für mehrere Abonnements und Mandanten
- Filtern der Erfassung, um nur Protokolle für bestimmte Kategorien zu erfassen
- Sammeln aller Aktivitätsprotokollkategorien. Einige Kategorien werden mit der Legacymethode nicht erfasst.
- Niedrigere Latenz für Protokollerfassung. Die vorherige Methode weist eine Latenz von ungefähr 15 Minuten auf, während sich die Zeit bei Diagnoseeinstellungen nur um etwa eine Minute verlängert.



### <a name="log-profiles"></a>Protokollprofile
Protokollprofile sind die Legacymethode zum Senden des Aktivitätsprotokolls an Azure Storage oder Event Hubs. Verwenden Sie das folgende Verfahren, um weiterhin mit einem Protokollprofil zu arbeiten oder um es in Vorbereitung auf die Migration zu einer Diagnoseeinstellung zu deaktivieren.

1. Wählen Sie im Azure-Portal im Menü **Azure Monitor** die Option **Aktivitätsprotokoll** aus.
3. Klicken Sie auf **Diagnoseeinstellungen**.

   ![Diagnoseeinstellungen](media/diagnostic-settings-subscription/diagnostic-settings.png)

4. Klicken Sie auf das lila Banner, um die Legacybenutzeroberfläche anzuzeigen.

    ![Legacybenutzeroberfläche](media/diagnostic-settings-subscription/legacy-experience.png)

### <a name="log-analytics-workspace"></a>Log Analytics-Arbeitsbereich
Die Legacymethode zum Erfassen des Aktivitätsprotokolls in einem Log Analytics-Arbeitsbereich besteht darin, das Protokoll in der Arbeitsbereichskonfiguration zu verbinden. 

1. Wählen Sie im Azure-Portal im Menü **Log Analytics-Arbeitsbereiche** den Arbeitsbereich zum Erfassen des Aktivitätsprotokolls aus.
1. Wählen Sie im Menü des Arbeitsbereichs im Abschnitt **Arbeitsbereichsdatenquellen** die Option **Azure-Aktivitätsprotokoll** aus.
1. Klicken Sie auf das Abonnement, mit dem Sie eine Verbindung herstellen möchten.

    ![Arbeitsbereiche](media/activity-log-collect/workspaces.png)

1. Klicken Sie auf **Verbinden**, um das Aktivitätsprotokoll im Abonnement mit dem ausgewählten Arbeitsbereich zu verbinden. Wenn das Abonnement bereits mit einem anderen Arbeitsbereich verbunden ist, klicken Sie zuerst auf **Trennen**, um die Verbindung zu trennen.

    ![Verbinden von Arbeitsbereichen](media/activity-log-collect/connect-workspace.png)


Zum Deaktivieren der Einstellung führen Sie dasselbe Verfahren aus, und klicken Sie auf **Trennen**, um das Abonnement aus dem Arbeitsbereich zu entfernen.


## <a name="analyze-activity-log-in-log-analytics-workspace"></a>Analysieren des Aktivitätsprotokolls im Log Analytics-Arbeitsbereich
Wenn Sie ein Aktivitätsprotokoll mit einem Log Analytics-Arbeitsbereich verbinden, werden Einträge in den Arbeitsbereich in eine Tabelle namens *AzureActivity* geschrieben, die Sie mit einer [Protokollabfrage](../log-query/log-query-overview.md) abrufen können. Die Struktur dieser Tabelle ist je nach [Kategorie des Protokolleintrags](activity-log-view.md#categories-in-the-activity-log) verschieden. Eine Beschreibung der einzelnen Kategorien finden Sie unter [Ereignisschema des Azure-Aktivitätsprotokolls](activity-log-schema.md).


### <a name="data-structure-changes"></a>Änderungen der Datenstruktur
Mit Diagnoseeinstellungen werden dieselben Daten wie mit der Legacymethode zum Erfassen des Aktivitätsprotokolls gesammelt, jedoch mit einigen Änderungen an der Struktur der Tabelle *AzureActivity*.

Die Spalten in der folgenden Tabelle sind im aktualisierten Schema veraltet. Sie sind weiterhin in *AzureActivity* vorhanden, verfügen aber über keine Daten. Die Ersatzspalten sind nicht neu, enthalten aber dieselben Daten wie die veralteten Spalte. Sie haben ein anderes Format, sodass Sie ggf. Protokollabfragen ändern müssen, die diese Spalten verwenden. 

| Veraltete Spalte | Ersatzspalte |
|:---|:---|
| ActivityStatus    | ActivityStatusValue    |
| ActivitySubstatus | ActivitySubstatusValue |
| Vorgangsname     | OperationNameValue     |
| ResourceProvider  | ResourceProviderValue  |

> [!IMPORTANT]
> In einigen Fällen werden die Werte in diesen Spalten vollständig in Großbuchstaben angegeben. Wenn Ihre Abfrage diese Spalten umfasst, sollten Sie den [=~](https://docs.microsoft.com/azure/kusto/query/datatypes-string-operators)-Operator verwenden, um einen Vergleich ohne Beachtung der Groß- und Kleinschreibung durchzuführen.

Die folgenden Spalten wurden *AzureActivity* im aktualisierten Schema hinzugefügt:

- Authorization_d
- Claims_d
- Properties_d


## <a name="activity-logs-analytics-monitoring-solution"></a>Überwachungslösung der Aktivitätsprotokollanalyse
Die Azure Log Analytics-Überwachungslösung wird bald veraltet sein und durch eine Arbeitsmappe ersetzt, die das aktualisierte Schema im Log Analytics-Arbeitsbereich verwendet. Sie können die Lösung weiterhin verwenden, wenn sie bereits aktiviert ist, doch ist dies nur möglich, wenn Sie das Aktivitätsprotokoll mithilfe von Legacyeinstellungen erfassen. 



### <a name="use-the-solution"></a>Verwenden der Lösung
Auf Überwachungslösungen wird im Azure-Portal über das Menü **Monitor** zugegriffen. Wählen Sie im Abschnitt **Insights** die Option **Mehr** aus, um die Seite **Übersicht** mit den Lösungskacheln zu öffnen. Die Kachel **Azure-Aktivitätsprotokolle** zeigt die Anzahl der **AzureActivity**-Datensätze in Ihrem Arbeitsbereich.

![Kachel „Azure-Aktivitätsprotokolle“](media/collect-activity-logs/azure-activity-logs-tile.png)


Klicken Sie auf die Kachel **Azure-Aktivitätsprotokolle**, um die Ansicht **Azure-Aktivitätsprotokolle** zu öffnen. Die Ansicht enthält die Visualisierungskomponenten, die in der folgenden Tabelle aufgeführt sind. Für jede Komponente sind bis zu 10 Einträge aufgeführt, die den Kriterien dieser Komponente für den angegebenen Zeitraum entsprechen. Sie können eine Protokollabfrage ausführen, die alle übereinstimmenden Datensätze zurückgibt. Dazu klicken Sie am unteren Rand der Komponente auf **Alle anzeigen**.

![Dashboard „Azure-Aktivitätsprotokolle“](media/collect-activity-logs/activity-log-dash.png)


### <a name="enable-the-solution-for-new-subscriptions"></a>Aktivieren der Lösung für neue Abonnements
In Kürze können Sie die Lösung für die Aktivitätsprotokollanalyse Ihrem Abonnement nicht mehr über das Azure-Portal hinzufügen. Sie können sie mithilfe des folgenden Verfahrens mit einer Resource Manager-Vorlage hinzufügen. 

1. Kopieren Sie den folgenden JSON-Code in eine Datei mit dem Namen *ActivityLogTemplate.json*.

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "defaultValue": "my-workspace",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "east us",
              "west us",
              "australia central",
              "west europe"
            ],
            "defaultValue": "australia central",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        }
      },
        "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "features": {
                    "searchVersion": 2
                }
            }
        },
        {
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "2015-11-01-preview",
            "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
            "location": "[parameters('location')]",
            "dependsOn": [
                "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]"
            ],
            "plan": {
                "name": "[concat('AzureActivity(', parameters('workspaceName'),')')]",
                "promotionCode": "",
                "product": "OMSGallery/AzureActivity",
                "publisher": "Microsoft"
            },
            "properties": {
                "workspaceResourceId": "[resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName'))]",
                "containedResources": [
                    "[concat(resourceId('microsoft.operationalinsights/workspaces', parameters('workspaceName')), '/views/AzureActivity(',parameters('workspaceName'))]"
                ]
            }
        },
        {
          "type": "Microsoft.OperationalInsights/workspaces/datasources",
          "kind": "AzureActivityLog",
          "name": "[concat(parameters('workspaceName'), '/', subscription().subscriptionId)]",
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "dependsOn": [
              "[parameters('WorkspaceName')]"
          ],
          "properties": {
              "linkedResourceId": "[concat(subscription().Id, '/providers/microsoft.insights/eventTypes/management')]"
          }
        }
      ]
    }    
    ```

2. Stellen Sie die Vorlage mithilfe der folgenden PowerShell-Befehle bereit:

    ```PowerShell
    Connect-AzAccount
    Select-AzSubscription <SubscriptionName>
    New-AzResourceGroupDeployment -Name activitysolution -ResourceGroupName <ResourceGroup> -TemplateFile <Path to template file>
    ```


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich ausführlicher über das [Aktivitätsprotokoll](platform-logs-overview.md).
- Informieren Sie sich über die [Azure Monitor-Datenplattform](data-platform.md).
- Verwenden Sie [Protokollabfragen](../log-query/log-query-overview.md) zum Anzeigen ausführlicher Informationen aus dem Aktivitätsprotokoll.
