---
title: API-Deaktivierung in Azure Monitor
description: Beschreibt die Deaktivierung älterer Versionen der OperationalInsights-Ressourcenanbieter-API.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/02/2020
ms.openlocfilehash: c9a7ba63246f747a132c315e22452ca9c7144448
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92058181"
---
# <a name="operationalinsights-api-version-retirement"></a>Deaktivierung der OperationalInsights-API-Version
Wenn Microsoft eine API deaktiviert, werden Sie mindestens 12 Monate vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten. Wir haben eine neue Version (2020-08-01) für **OperationalInsights**-Ressourcenanbieter-APIs veröffentlicht. Alle früheren API-Versionen werden am 31. Oktober 2023 deaktiviert. Da neue Features, Funktionen und Optimierungen nur der aktuellen API hinzugefügt werden, sollten Sie so früh wie möglich ein Upgrade auf die neueste API-Version durchführen.

Nach dem 31. Oktober 2023 werden frühere API-Versionen als 2020-08-01 nicht mehr von Azure Monitor unterstützt. Wenn Sie kein Upgrade durchführen möchten, werden von früheren Versionen gesendete Anforderungen bis zum 31. Oktober 2023 weiterhin vom Azure Monitor-Dienst bedient.

## <a name="migration-steps"></a>Schritte bei der Migration
Je nach verwendeter Konfigurationsmethode sollten Sie die neue Version in **REST**-Anforderungen und **Resource Manager-Vorlagen** aktualisieren. Verwenden Sie zum Aktualisieren der API-Version die folgenden Beispiele:

1. REST-API-Anforderungen verwenden die API-Version in der URL der Anforderung. Ersetzen Sie diese Version durch die neueste Version (2020-08-01), wie es im folgenden Beispiel gezeigt wird.

    ```rest
    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.OperationalInsights/workspaces/{workspaceName}?api-version=2020-08-01
    ```

2. Azure Resource Manager-Vorlagen verwenden die API-Version in der **apiVersion**-Eigenschaft der Ressource. Ersetzen Sie diese Version durch die neueste Version (2020-08-01), wie es im folgenden Beispiel gezeigt wird.


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

- Weitere Informationen finden Sie in der [Referenz für die OperationalInsights-Arbeitsbereichs-API](https://docs.microsoft.com/rest/api/loganalytics/workspaces).
