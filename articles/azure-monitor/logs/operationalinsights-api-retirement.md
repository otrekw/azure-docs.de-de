---
title: API-Deaktivierung in Azure Monitor
description: Beschreibt die Deaktivierung älterer Versionen der OperationalInsights-Ressourcenanbieter-API.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/29/2020
ms.openlocfilehash: 7b5d105f6a93102ff1b9142fbdf690c3873d872f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102032023"
---
# <a name="operationalinsights-api-version-retirement"></a>Deaktivierung der OperationalInsights-API-Version
Wenn Microsoft eine API deaktiviert, werden Sie mindestens 12 Monate vorher benachrichtigt, um einen reibungslosen Übergang zu einer neueren/unterstützten Version zu gewährleisten. Wir haben eine neue Version (2020-08-01) für Ressourcenanbieter-APIs vom Typ **OperationalInsights** veröffentlicht. Alle älteren API-Versionen werden am 29. Februar 2024 eingestellt.

Es wird empfohlen, ab sofort die Version 2020-08-01 zu verwenden, um von neuen Funktionen wie [dediziertem Cluster](./logs-dedicated-clusters.md), [kundenseitig verwalteten Schlüsseln](../logs/customer-managed-keys.md), [Private Link](./private-link-security.md) und [Datenexport](./logs-data-export.md) zu profitieren. Außerdem werden neue Features, Funktionen und Optimierungen werden nur der aktuellen API hinzugefügt.

Nach dem 29. Februar 2024 werden ältere API-Versionen als 2020-08-01 nicht mehr von Azure Monitor unterstützt. Wenn Sie kein Upgrade durchführen möchten, werden von älteren Versionen gesendete Anforderungen bis zum 29. Februar 2024 weiterhin vom Azure Monitor-Dienst verarbeitet.

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


### <a name="more-information"></a>Weitere Informationen
Bei Fragen können Sie sich an [unsere Experten in der Tech Community]( https://techcommunity.microsoft.com/t5/azure-monitor/bd-p/AzureMonitor) wenden. Wenn Sie über einen Supportplan verfügen und technische Hilfe benötigen, erstellen Sie eine [Supportanfrage]( https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest): 
1.  Wählen Sie unter *Problemtyp* die Option **Technisch** aus. 
2.  Wählen Sie unter *Abonnement* Ihr Abonnement aus. 
3.  Wählen Sie unter *Dienst* die Option **Meine Dienste** und dann **Log Analytics** aus. 
4.  Geben Sie unter *Zusammenfassung* eine Beschreibung Ihres Problems ein. 
5.  Wählen Sie unter *Problemtyp* die Option **Log Analytics-Arbeitsbereichsverwaltung** aus.  
6.  Wählen Sie unter *Problemuntertyp* die Option **ARM-Vorlagen, PowerShell und CLI** aus. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie in der [Referenz für die OperationalInsights-Arbeitsbereichs-API](/rest/api/loganalytics/workspaces).