---
title: Unterstützte Ressourcen für Metrikwarnungen in Azure Monitor
description: Referenz zu Supportmetriken und -protokollen für Metrikwarnungen in Azure Monitor
author: harelbr
ms.author: harelbr
services: monitoring
ms.topic: conceptual
ms.date: 12/17/2019
ms.subservice: alerts
ms.openlocfilehash: 9585e74170d21ad899224462b6e41e7e9bb75727
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77664813"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Unterstützte Ressourcen für Metrikwarnungen in Azure Monitor

Azure Monitor unterstützt jetzt einen [neuen Metrikwarnungstyp](../../azure-monitor/platform/alerts-overview.md), der erhebliche Vorteile gegenüber älteren [klassischen Metrikwarnungen](../../azure-monitor/platform/alerts-classic.overview.md) aufweist. Metriken stehen für [eine umfangreiche Liste von Azure-Diensten](../../azure-monitor/platform/metrics-supported.md) zur Verfügung. Die neueren Warnungen unterstützen eine (stetig wachsende) Teilmenge der Ressourcentypen. In diesem Artikel wird diese Teilmenge aufgeführt.

Sie können auch neuere Metrikwarnungen für gängige Protokolldaten verwenden, die in einem Log Analytics-Arbeitsbereich gespeichert sind und als Metriken extrahiert wurden. Weitere Informationen finden Sie unter [Metrikwarnungen für Protokolle](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, Befehlszeilenschnittstelle, REST-Unterstützung
Derzeit können Sie neuere Metrikwarnungen nur im Azure-Portal, in der [REST-API](https://docs.microsoft.com/rest/api/monitor/metricalerts/) oder in [Resource Manager-Vorlagen](../../azure-monitor/platform/alerts-metric-create-templates.md) erstellen. Die Konfiguration neuerer Warnungen über PowerShell und die Azure-Befehlszeilenschnittstelle (Azure CLI, Version 2.0 und höher) wird in Kürze unterstützt.

## <a name="metrics-and-dimensions-supported"></a>Unterstützte Metriken und Dimensionen
Neuere Metrikwarnungen unterstützen Warnungen für Metriken mit Dimensionen. Mithilfe von Dimensionen können Sie die Metrik nach der richtigen Ebene filtern. Sie können alle unterstützten Metriken und die dazugehörigen Dimensionen unter [Azure Monitor-Metrik-Explorer](../../azure-monitor/platform/metrics-charts.md) erkunden und visualisieren.

Im Anschluss finden Sie die vollständige Liste der Azure Monitor-Metrikquellen, die von den neueren Warnungen unterstützt werden:

|Ressourcentyp  |Unterstützte Dimensionen |Warnungen mit mehreren Ressourcen| Verfügbare Metriken|
|---------|---------|-----|----------|
|Microsoft.ApiManagement/service | Ja| Nein | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.AppPlatform/Spring |Nein| Ja|
|Microsoft.Automation/automationAccounts | Ja| Nein | [Automation-Konten](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | –| Nein | [Batch-Konten](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis|Ja| Nein |[Azure Cache for Redis](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic|Nein|Ja|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/blobServices|Nein|Ja|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/fileServices|Nein|Ja|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/queueServices|Nein|Ja|
|Microsoft.ClassicStorage/storageAccounts/mmxclassic/tableServices|Nein|Ja| |
|Microsoft.CognitiveServices/accounts| – | Nein | [Cognitive Services](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines |Ja | Ja | [Virtuelle Computer](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets |– | Ja |[Skalierungsgruppen für virtuelle Computer](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Ja| Nein | [Containergruppen](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Ja | Nein | [Verwaltete Cluster](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataBoxEdge/dataBoxEdgeDevices | Ja | Ja | |
|Microsoft.DataFactory/datafactories| Ja| Nein | [Data Factorys V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories |Ja | Nein |[Data Factorys V2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DataShare/accounts |Nein| Ja|
|Microsoft.DBforMySQL/servers |–| Nein |[Datenbank für MySQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers |– | Nein | [Datenbank für PostgreSQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.Devices/IotHubs | – | Nein |[IoT Hub-Metriken](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesiothubs)|
|Microsoft.Devices/provisioningServices| Ja | Nein |[DPS-Metriken](../../azure-monitor/platform/metrics-supported.md#microsoftdevicesprovisioningservices)|
|Microsoft.EventGrid/domains|Nein|Ja| |
|Microsoft.EventGrid/topics |Ja | Nein |[Event Grid-Themen](../../azure-monitor/platform/metrics-supported.md#microsofteventgridtopics)|
|Microsoft.EventHub/clusters |Ja| Nein |[Event Hubs-Cluster](../../azure-monitor/platform/metrics-supported.md#microsofteventhubclusters)|
|Microsoft.EventHub/namespaces |Ja| Nein |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Nein |Nein |[Tresore](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows |– | Nein |[Logik-Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.MachineLearningServices/workspaces|Ja| Nein | [Machine Learning](../../azure-monitor/platform/metrics-supported.md#microsoftmachinelearningservicesworkspaces) |
|Microsoft.Network/applicationGateways|–| Nein |  |
|Microsoft.Network/dnsZones | –| Nein | [DNS-Zonen](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/expressRouteCircuits | – | Nein |[ExpressRoute-Verbindungen](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/loadBalancers (nur für Standard-SKUs)| Ja| Nein | [Lastenausgleichsmodule](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/natGateways|Nein|Ja|
|Microsoft.Network/privateEndpoints|Nein|Ja|
|Microsoft.Network/privateLinkServices|Nein|Ja|
|Microsoft.Network/publicipaddresses |– | Nein |[Öffentliche IP-Adressen](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Network/trafficManagerProfiles | Ja | Nein | [Traffic Manager-Profile](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.OperationalInsights/workspaces| Ja | Nein | [Log Analytics-Arbeitsbereiche](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|
|Microsoft.Relay/namespaces | Ja | Nein | [Relays](../../azure-monitor/platform/metrics-supported.md#microsoftrelaynamespaces)|
|Microsoft.Peering/peeringServices|Nein|Ja|
|Microsoft.PowerBIDedicated/capacities | – | Nein | [Kapazitäten](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices |–|Nein | [Suchdienste](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces |Ja| Nein |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Sql/servers/elasticPools |   Nein | Ja |
|Microsoft.Sql/servers/databases    | Nein | Ja |
|Microsoft.Storage/storageAccounts |Ja | Nein | [Speicherkonten](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services | Ja| Nein | [Blobdienste](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [Dateidienste](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [Warteschlangendienste](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) und [Tabellendienste](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs |–| Nein | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.Microsoft.VMWareCloudSimple/virtualMachines |Ja|Nein |[Virtuelle CloudSimple-Computer](../../azure-monitor/platform/metrics-supported.md#microsoftvmwarecloudsimplevirtualmachines)|
|Microsoft.Web/serverfarms | Ja | Nein | [App Service-Pläne](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)|
|Microsoft.Web/sites | Ja | Nein | [App Services](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) und [Functions](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
|Microsoft.Web/sites/slots | Ja | Nein | [App Service-Slots](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|

## <a name="payload-schema"></a>Nutzlast und Schema

> [!NOTE]
> Für Ihre Webhook-Integrationen können Sie auch das [allgemeine Warnungsschema](https://aka.ms/commonAlertSchemaDocs) verwenden, das den Vorteil einer einzelnen erweiterbaren und einheitlichen Warnungsnutzlast für alle Benachrichtigungsdienste in Azure Monitor bietet. [Hier finden Sie Informationen zu den Definitionen des allgemeinen Warnungsschemas](https://aka.ms/commonAlertSchemaDefinitions).


Bei Verwendung einer ordnungsgemäß konfigurierten [Aktionsgruppe](../../azure-monitor/platform/action-groups.md) enthält der POST-Vorgang für alle neueren Metrikwarnungen die folgende JSON-Nutzlast und das folgende Schema:

```json
{
  "schemaId": "AzureMonitorMetricAlert",
  "data": {
    "version": "2.0",
    "status": "Activated",
    "context": {
      "timestamp": "2018-02-28T10:44:10.1714014Z",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
      "name": "StorageCheck",
      "description": "",
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "severity":"3",
      "condition": {
        "windowSize": "PT5M",
        "allOf": [
          {
            "metricName": "Transactions",
            "metricNamespace":"microsoft.storage/storageAccounts",
            "dimensions": [
              {
                "name": "AccountResourceId",
                "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
              },
              {
                "name": "GeoType",
                "value": "Primary"
              }
            ],
            "operator": "GreaterThan",
            "threshold": "0",
            "timeAggregation": "PT5M",
            "metricValue": 1
          }
        ]
      },
      "subscriptionId": "00000000-0000-0000-0000-000000000000",
      "resourceGroupName": "Contoso",
      "resourceName": "diag500",
      "resourceType": "Microsoft.Storage/storageAccounts",
      "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
      "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
    },
    "properties": {
      "key1": "value1",
      "key2": "value2"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über die [neue Oberfläche „Warnungen“](../../azure-monitor/platform/alerts-overview.md).
* Erfahren Sie mehr über [Protokollwarnungen in Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Erfahren Sie mehr über [Warnungen in Azure](../../azure-monitor/platform/alerts-overview.md).
