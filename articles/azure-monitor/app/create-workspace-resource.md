---
title: Erstellen einer neuen arbeitsbereichsbasierten Application Insights-Ressource in Azure Monitor | Microsoft-Dokumentation
description: Erfahren Sie mehr über die erforderlichen Schritte zum Aktivieren der neuen arbeitsbereichsbasierte Application Insights-Ressourcen in Azure Monitor.
author: mrbullwinkle
ms.author: mbullwin
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: e66ae6aa0b9b7e309fbd6fcc3699cb873a266bbe
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647898"
---
# <a name="workspace-based-application-insights-resources-preview"></a>Arbeitsbereichsbasierte Application Insights-Ressourcen (Vorschau)

Arbeitsbereichsbasierte Ressourcen unterstützen die vollständige Integration zwischen Application Insights und Log Analytics. Sie können jetzt Ihre Application Insights-Telemetriedaten an einen gemeinsamen Log Analytics-Arbeitsbereich senden, über den Sie auf alle Features von Log Analytics zugreifen und gleichzeitig Anwendungs-, Infrastruktur- und Plattformprotokolle an einem einzigen konsolidierten Speicherort sammeln können.

Damit ist auch eine gemeinsame rollenbasierte Zugriffssteuerung über Ihre Ressourcen hinweg möglich, und die Notwendigkeit app- bzw. arbeitsbereichsübergreifender Abfragen entfällt.

> [!NOTE]
> Die Abrechnung der Datenerfassung und -aufbewahrung für arbeitsbereichsbasierte Application Insights-Ressourcen erfolgt über den Log Analytics-Arbeitsbereich, in dem sich die Daten befinden. [Erfahren Sie mehr]( https://docs.microsoft.com/azure/azure-monitor/app/pricing#workspace-based-application-insights) über die Abrechnung für arbeitsbereichsbasierte Application Insights-Ressourcen.

Um die neue Funktion zu testen, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und erstellen Sie eine Application Insights-Ressource:

![Arbeitsbereichsbasierte Application Insights-Ressource](./media/create-workspace-resource/create-workspace-based.png)

Wenn Sie noch nicht über einen Log Analytics-Arbeitsbereich verfügen, [sehen Sie sich die Dokumentation zur Erstellung von Log Analytics-Arbeitsbereichen an](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

Für die öffentliche Vorschau **sind arbeitsbereichsbasierte Ressourcen derzeit auf die Regionen „USA, Westen 2“, „USA, Osten“ und „USA, Süden-Mitte“ beschränkt**.

Nachdem Ihre Ressource erstellt wurde, sehen Sie die Informationen zum entsprechenden Arbeitsbereich im Bereich **Übersicht**:

![Name des Arbeitsbereichs](./media/create-workspace-resource/workspace-name.png)

Durch Klicken auf den blauen Linktext gelangen Sie zum zugeordneten Log Analytics-Arbeitsbereich, wo Sie die neue einheitliche Umgebung für Arbeitsbereichsabfragen nutzen können.

> [!NOTE]
> Wir stellen auf der Application Insights-Benutzeroberfläche weiterhin vollständige Abwärtskompatibilität für Ihre klassischen Ressourcenabfragen, Arbeitsmappen und protokollbasierten Warnungen von Application Insights bereit. Um die [neue arbeitsbereichsbasierte Tabellenstruktur und das neue Tabellenschema](apm-tables.md) anzuzeigen und abzufragen, müssen Sie zuerst zu Ihrem Log Analytics-Arbeitsbereich navigieren. Während der Vorschau erhalten Sie durch Auswahl der Option **Protokolle** in den Application Insights-Bereichen Zugriff auf die klassische Application Insights-Benutzeroberfläche für Abfragen.

## <a name="copy-the-connection-string"></a>Verbindungszeichenfolge kopieren

Die [Verbindungszeichenfolge](https://docs.microsoft.com/azure/azure-monitor/app/sdk-connection-string?tabs=net) identifiziert die Ressource, der Sie Ihre Telemetriedaten zuordnen möchten. Hier können Sie die Endpunkte ändern, die Ihre Ressource als Ziel für die Telemetrie verwendet. Sie müssen die Verbindungszeichenfolge kopieren und dem Code Ihrer Anwendung oder einer Umgebungsvariable hinzufügen.

## <a name="monitoring-configuration"></a>Überwachungskonfiguration

Nachdem die arbeitsbereichsbasierte Application Insights-Ressource erstellt wurde, ist die Konfiguration der Überwachung relativ unkompliziert.

### <a name="code-based-application-monitoring"></a>Codebasierte Anwendungsüberwachung

Bei der codebasierten Anwendungsüberwachung installieren Sie einfach das entsprechende Application Insights SDK und verweisen auf den Instrumentierungsschlüssel oder die Verbindungszeichenfolge für Ihre neu erstellte Ressource.  

Detaillierte Informationen zum Einrichten eines Application Insights SDK für die codebasierte Überwachung finden Sie in der jeweiligen sprach- bzw. frameworkspezifischen Dokumentation:

- [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)
- [ASP.NET Core ](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)
- [Hintergrundaufgaben und moderne Konsolenanwendungen (.NET/.NET Core)](https://docs.microsoft.com/azure/azure-monitor/app/worker-service)
- [Klassische Konsolenanwendungen (.NET)](https://docs.microsoft.com/azure/azure-monitor/app/console) 
- [Java](https://docs.microsoft.com/azure/azure-monitor/app/java-get-started?tabs=maven)
- [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript)
- [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs)
- [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python)

### <a name="codeless-monitoring-and-visual-studio-resource-creation"></a>Codelose Überwachung und Erstellung von Visual Studio-Ressourcen

Bei der codelosen Überwachung von Diensten wie Azure Functions und Azure App Services müssen Sie ebenfalls zuerst eine arbeitsbereichsbasierte Application Insights-Ressource erstellen und dann während der Konfigurationsphase der Überwachung auf diese Ressource verweisen.

Diese Dienste bieten zwar die Option, im Rahmen des dienstinternen Ressourcenerstellungsprozesses eine neue Application Insights-Ressource zu erstellen, allerdings sind Ressourcen, die über diese Benutzeroberflächenoptionen erstellt wurden, derzeit auf die klassische Application Insights-Benutzeroberfläche begrenzt.

Dasselbe gilt für die Benutzeroberfläche zur Erstellung von Application Insights-Ressourcen in Visual Studio für ASP.NET und ASP.NET Core. Sie müssen eine vorhandene arbeitsbereichsbasierte Ressource auf der Visual Studio-Benutzeroberfläche zur Aktivierung der Überwachung auswählen. Über die Option zum Erstellen einer neuen Ressource in Visual Studio können Sie nur klassische Application Insights-Ressourcen erstellen.

## <a name="creating-a-resource-automatically"></a>Automatisches Erstellen einer Ressource

### <a name="azure-cli"></a>Azure CLI

Für den Zugriff auf die Azure CLI-Befehle (Vorschauversion) von Application Insights müssen Sie zuerst Folgendes ausführen:

```azurecli
 az extension add -n application-insights
```

Wenn Sie den Befehl `az extension add` nicht ausführen, wird eine Fehlermeldung mit dem folgenden Hinweis angezeigt: `az : ERROR: az monitor: 'app-insights' is not in the 'az monitor' command group. See 'az monitor --help'.`

Jetzt können Sie Folgendes ausführen, um Ihre Application Insights-Ressource zu erstellen:

```azurecli
az monitor app-insights component create --app
                                         --location
                                         --resource-group
                                         [--application-type]
                                         [--ingestion-access {Disabled, Enabled}]
                                         [--kind]
                                         [--only-show-errors]
                                         [--query-access {Disabled, Enabled}]
                                         [--tags]
                                         [--workspace]
```

#### <a name="example"></a>Beispiel

```azurecli
az monitor app-insights component create --app demoApp --location eastus --kind web -g my_resource_group --workspace "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/test1234/providers/microsoft.operationalinsights/workspaces/test1234555"
```

Vollständige Informationen zu diesem Befehl finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest#ext-application-insights-az-monitor-app-insights-component-create).

### <a name="azure-powershell"></a>Azure PowerShell

Der PowerShell-Befehl `New-AzApplicationInsights` unterstützt derzeit nicht die Erstellung einer arbeitsbereichsbasierten Application Insights-Ressource. Um eine arbeitsbereichsbasierte Ressource mit PowerShell zu erstellen, können Sie die folgenden Azure Resource Manager-Vorlagen verwenden und die Ressource über PowerShell bereitstellen.

### <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

#### <a name="template-file"></a>Vorlagendatei

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "string"
        },
        "type": {
            "type": "string"
        },
        "regionId": {
            "type": "string"
        },
        "tagsArray": {
            "type": "object"
        },
        "requestSource": {
            "type": "string"
        },
        "workspaceResourceId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('name')]",
            "type": "microsoft.insights/components",
            "location": "[parameters('regionId')]",
            "tags": "[parameters('tagsArray')]",
            "apiVersion": "2020-02-02-preview",
            "properties": {
                "ApplicationId": "[parameters('name')]",
                "Application_Type": "[parameters('type')]",
                "Flow_Type": "Redfield",
                "Request_Source": "[parameters('requestSource')]",
                "WorkspaceResourceId": "[parameters('workspaceResourceId')]"
            }
        }
    ]
}
```

#### <a name="parameters-file"></a>Parameterdatei

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "type": {
            "value": "web"
        },
        "name": {
            "value": "customresourcename"
        },
        "regionId": {
            "value": "eastus"
        },
        "tagsArray": {
            "value": {}
        },
        "requestSource": {
            "value": "Custom"
        },
        "workspaceResourceId": {
            "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/my_resource_group/providers/microsoft.operationalinsights/workspaces/myworkspacename"
        }
    }
}

```

## <a name="modifying-the-associated-workspace"></a>Ändern des zugeordneten Arbeitsbereichs

Nach dem Erstellen einer arbeitsbereichsbasierten Application Insights-Ressource können Sie den zugeordneten Log Analytics-Arbeitsbereich ändern.

Wählen Sie im Bereich der Application Insights-Ressource die Optionen **Eigenschaften** > **Arbeitsbereich ändern** > **Log Analytics-Arbeitsbereiche**.

## <a name="export-telemetry"></a>Exportieren von Telemetriedaten

Die Legacyfunktion für den fortlaufenden Export wird für arbeitsbereichsbasierte Ressourcen nicht unterstützt. Wählen Sie stattdessen in Ihre Application Insights-Ressource die Optionen **Diagnoseeinstellungen** > **Diagnoseeinstellung hinzufügen**. Sie können alle Tabellen oder eine Teilmenge der Tabellen auswählen, die Sie in einem Speicherkonto archivieren oder an Azure Event Hub streamen möchten.

## <a name="next-steps"></a>Nächste Schritte

* [Untersuchen von Metriken](../../azure-monitor/platform/metrics-charts.md)
* [Schreiben von Analytics-Abfragen](../../azure-monitor/app/analytics.md)

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
