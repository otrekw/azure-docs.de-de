---
title: API-Deaktivierung in Azure Monitor
description: Beschreibt die Deaktivierung älterer Versionen der OperationalInsights-Ressourcenanbieter-API.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: 7ddfb3221c73c740a339dd8d9d4f60cca46c6425
ms.sourcegitcommit: daab0491bbc05c43035a3693a96a451845ff193b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2020
ms.locfileid: "93026767"
---
# <a name="operationalinsights-api-version-retirement"></a>Deaktivierung der OperationalInsights-API-Version
Wenn Microsoft eine API deaktiviert, werden Sie mindestens 12 Monate vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten. Wir haben eine neue Version (2020-08-01) für Ressourcenanbieter-APIs vom Typ **OperationalInsights** veröffentlicht. Alle älteren API-Versionen werden am 29. Februar 2024 eingestellt.

Es wird empfohlen, ab sofort die Version 2020-08-01 zu verwenden, um von neuen Funktionen wie [dediziertem Cluster](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters), [kundenseitig verwalteten Schlüsseln](https://docs.microsoft.com/azure/azure-monitor/platform/customer-managed-keys), [Private Link](https://docs.microsoft.com/azure/azure-monitor/platform/private-link-security) und [Datenexport](https://docs.microsoft.com/azure/azure-monitor/platform/logs-data-export) zu profitieren. Außerdem werden neue Features, Funktionen und Optimierungen werden nur der aktuellen API hinzugefügt.

Nach dem 29. Februar 2024 werden ältere API-Versionen als 2020-08-01 nicht mehr von Azure Monitor unterstützt. Wenn Sie kein Upgrade durchführen möchten, werden von älteren Versionen gesendete Anforderungen bis zum 29. Februar 2024 weiterhin vom Azure Monitor-Dienst verarbeitet.

## <a name="migration-steps"></a>Schritte bei der Migration
Je nach verwendeter Konfigurationsmethode sollten Sie die neue Version in **REST** -Anforderungen und **Resource Manager-Vorlagen** aktualisieren. Verwenden Sie zum Aktualisieren der API-Version die folgenden Beispiele:

1. REST-API-Anforderungen verwenden die API-Version in der URL der Anforderung. Ersetzen Sie diese Version durch die neueste Version (2020-08-01), wie es im folgenden Beispiel gezeigt wird.

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure Resource Manager-Vorlagen verwenden die API-Version in der **apiVersion** -Eigenschaft der Ressource. Ersetzen Sie diese Version durch die neueste Version (2020-08-01), wie es im folgenden Beispiel gezeigt wird.


    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2019-08-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string",
                "metadata": {
                "description": "Name of the workspace."
                }
            },
            "resources": [
            {
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('workspaceName')]",
                "apiVersion": "2020-08-01",
                "location": "westus",
                "properties": {
                    "sku": {
                        "name": "pergb2018"
                    },
                    "retentionInDays": 30,
                    "features": {
                        "searchVersion": 1,
                        "legacy": 0,
                        "enableLogAccessUsingOnlyResourcePermissions": true
                    }
                }
            }
        ]
    }
    }
    ```


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der [Referenz für die OperationalInsights-Arbeitsbereichs-API](/rest/api/loganalytics/workspaces).
