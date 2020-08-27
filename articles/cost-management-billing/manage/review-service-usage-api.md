---
title: Überprüfen der Azure-Dienstressourcennutzung mit der REST-API
description: Erfahren Sie, wie Sie mit Azure-REST-APIs die Azure-Dienstressourcennutzung überprüfen. Erstellen Sie einen Cost Management-Bericht, und filtern Sie nach bestimmten Ressourcentypen.
author: lleonard-msft
ms.service: cost-management-billing
ms.subservice: enterprise
ms.topic: reference
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: a7b06ff41e537513558e7f4dc8e1732966299b01
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88684727"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>Überprüfen der Azure-Ressourcennutzung mithilfe der REST-API

Mithilfe von Azure Cost Management-APIs können Sie die Nutzung Ihrer Azure-Ressourcen überprüfen und verwalten.

In diesem Artikel erfahren Sie, wie Sie einen täglichen Bericht erstellen, der ein Dokument mit durch Trennzeichen getrennten Werten generiert, das Informationen zur stündlichen Nutzung enthält. Außerdem erfahren Sie, wie Sie den Bericht mithilfe von Filtern so anpassen, dass Sie die Nutzung von virtuellen Computern, Datenbanken und markierten Ressourcen in einer Azure-Ressourcengruppe abfragen können.

>[!NOTE]
> Die Cost Management-API befindet sich derzeit in der privaten Vorschau.

## <a name="create-a-basic-cost-management-report"></a>Erstellen eines einfachen Kostenverwaltungsberichts

Definieren Sie mithilfe des Vorgangs `reports` in der Cost Management-API, wie die Kostenberichte generiert und wo sie veröffentlicht werden.

```http
https://management.azure.com/subscriptions/{subscriptionGuid}/providers/Microsoft.CostManagement/reports/{reportName}?api-version=2018-09-01-preview
Content-Type: application/json   
Authorization: Bearer
```

Der Parameter `{subscriptionGuid}` ist erforderlich und sollte eine Abonnement-ID enthalten, die mithilfe der im API-Token bereitgestellten Anmeldeinformationen gelesen werden kann. 

Der Parameter `{reportName}` gibt den Namen des Berichts an. Zum Abrufen der Liste mit Berichtsnamen können Sie den Vorgang „Reports_List“ verwenden: `/subscriptions/{subscriptionId}/providers/Microsoft.CostManagement/reports`. Eine Beispielausgabe können Sie sich auf [GitHub](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/cost-management/resource-manager/Microsoft.CostManagement/preview/2018-08-01-preview/examples/ReportList.json) ansehen.

Die folgenden Header sind erforderlich:

|Anforderungsheader|BESCHREIBUNG|  
|--------------------|-----------------|  
|*Content-Type:*| Erforderlich. Legen Sie diese Option auf `application/json` fest. |  
|*Authorization:*| Erforderlich. Legen Sie diese Option auf ein gültiges `Bearer`-Token fest. |

Konfigurieren Sie die Parameter des Berichts im HTTP-Anforderungstext. Im folgenden Beispiel wird der Bericht so festgelegt, dass er bei Aktivierung an jedem Tag generieren wird, als eine CSV-Datei in einen Azure Storage-Blobcontainer geschrieben wird und Informationen zu stündlichen Kosten für alle Ressourcen in der Ressourcengruppe `westus` enthält.

```json
{
    "properties": {
        "schedule": {
            "status": "Inactive",
            "recurrence": "Daily",
            "recurrencePeriod": {
                "from": "2018-08-21",
                "to": "2019-10-31"
            }
        },
        "deliveryInfo": {
            "destination": {
                "resourceId": "/subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
                "container": "MyReportContainer",
                "rootFolderPath": "MyScheduleTest"
            }
        },
        "format": "Csv",
        "definition": {
            "type": "Usage",
            "timeframe": "MonthToDate",
            "dataSet": {
                "granularity": "Hourly",
                "filter": {
                    "dimensions": {
                        "name": "ResourceLocation",
                        "operator": "In",
                        "values": [
                            "westus"
                        ]
                    }
                }
            }
        }
    }
}
```

Das

## <a name="filtering-reports"></a>Filtern von Berichten

Mithilfe der Abschnitte `filter` und `dimensions` des Anforderungstexts können Sie beim Erstellen eines Berichts den Schwerpunkt auf die Kosten für bestimmte Ressourcentypen legen. Der vorherige Anforderungstext zeigt, wie nach allen Ressourcen in einer Region gefiltert wird.

### <a name="get-all-compute-usage"></a>Abrufen der gesamten Computenutzung

Mithilfe der `ResourceType`-Dimension können Sie einen Bericht zu den Kosten virtueller Azure-Computer in Ihrem Abonnement für alle Regionen erstellen.

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.ClassicCompute/virtualMachines",
                "Microsoft.Compute/virtualMachines"
        ]
    }
}
```

### <a name="get-all-database-usage"></a>Abrufen der gesamten Datenbanknutzung

Mithilfe der `ResourceType`-Dimension können Sie einen Bericht zu den Kosten der Azure SQL-Datenbank in Ihrem Abonnement für alle Regionen erstellen.

```json
"filter": {
    "dimensions": {
        "name": "ResourceType",
        "operator": "In",
        "values": [
                "Microsoft.Sql/servers"
        ]
    }
}
```

### <a name="report-on-specific-instances"></a>Bericht zu bestimmten Instanzen

Mithilfe der `Resource`-Dimension können Sie einen Bericht zu den Kosten für bestimmte Ressourcen erstellen.

```json
"filter": {
    "dimensions": {
        "name": "Resource",
        "operator": "In",
        "values": [
            "subscriptions/{subscriptionGuid}/resourceGroups/{resourceGroup}/providers/Microsoft.ClassicCompute/virtualMachines/{ResourceName}"
        ]
    }
}
```

### <a name="changing-timeframes"></a>Ändern von Zeitrahmen

Legen Sie die `timeframe`-Definition auf `Custom` fest, um einen Zeitrahmen außerhalb der integrierten Optionen für Woche bis Datum und Monat bis Datum festzulegen.

```json
"timeframe": "Custom",
"timePeriod": {
    "from": "2017-12-31T00:00:00.000Z",
    "to": "2018-12-30T00:00:00.000Z"
}
```

## <a name="next-steps"></a>Nächste Schritte
- [Erste Schritte mit der Azure-REST-API](https://docs.microsoft.com/rest/api/azure/)   
