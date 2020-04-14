---
title: Erstellen von Integrationsdienstumgebungen (Integration Service Environments, ISEs) mit der Logic Apps-REST-API
description: Hier erfahren Sie, wie Sie mithilfe der Logic Apps-REST-API eine Integrationsdienstumgebung (Integration Service Environment, ISE) erstellen, um über Azure Logic Apps auf virtuelle Azure-Netzwerke (VNETs) zugreifen zu können.
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 0670331d2338b4b6419ffbff1452b5fbac91029f
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478830"
---
# <a name="create-an-integration-service-environment-ise-by-using-the-logic-apps-rest-api"></a>Erstellen einer Integrationsdienstumgebung (Integration Service Environment, ISE) mithilfe der Logic Apps-REST-API

In diesem Artikel erfahren Sie, wie Sie für Szenarien, in denen Ihre Logik-Apps und Integrationskonten Zugriff auf ein [virtuelles Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) benötigen, mithilfe der Logic Apps-REST-API eine [*Integrationsdienstumgebung* (Integration Service Environment, ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) erstellen. Eine Integrationsdienstumgebung ist eine isolierte Umgebung, die dedizierten Speicher und andere Ressourcen verwendet, die vom „globalen“ mehrinstanzfähigen Logic Apps-Dienst getrennt bleiben. Diese Trennung trägt auch dazu bei, jegliche Auswirkungen anderer Azure-Mandanten auf die Leistung Ihrer Apps zu verringern. Eine ISE stellt Ihnen außerdem Ihre eigenen statischen IP-Adressen bereit. Diese IP-Adressen sind gesondert von den statischen IP-Adressen, die von den Logik-Apps im öffentlichen, mehrinstanzenfähigen Dienst gemeinsam verwendet werden.

Sie können eine ISE auch erstellen, indem Sie das [Beispiel für die Azure Resource Manager-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment) oder das [Azure-Portal](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) verwenden.

> [!IMPORTANT]
> Für Logik-Apps, integrierte Trigger, integrierte Aktionen und Connectors, die in Ihrer ISE ausgeführt werden, gilt ein anderer als der nutzungsbasierte Tarif. Weitere Informationen zur Preisgestaltung und Abrechnung für ISEs finden Sie unter [Feststehendes Preismodell](../logic-apps/logic-apps-pricing.md#fixed-pricing). Eine Preisübersicht finden Sie unter [Logic Apps – Preise](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Voraussetzungen

* Die gleichen [Voraussetzungen](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#prerequisites) und [Anforderungen zum Aktivieren des Zugriffs für die ISE](../logic-apps/connect-virtual-network-vnet-isolated-environment.md#enable-access) wie bei der ISE-Erstellung über das Azure-Portal

* Ein Tool, mit dem Sie Ihre ISE durch Aufrufen der Logic Apps-REST-API mit einer HTTPS PUT-Anforderung erstellen können. Beispielsweise können Sie [Postman](https://www.getpostman.com/downloads/) verwenden oder eine Logik-App erstellen, die diese Aufgabe ausführt.

## <a name="send-the-request"></a>Senden der Anforderung

Um Ihre ISE durch Aufrufen der Logic Apps-REST-API zu erstellen, führen Sie diese HTTPS PUT-Anforderung aus:

`PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/integrationServiceEnvironments/{integrationServiceEnvironmentName}?api-version=2019-05-01`

> [!IMPORTANT]
> Version 2019-05-01 der Logic Apps-REST-API erfordert, dass Sie Ihre eigene HTTP PUT-Anforderung für ISE-Connectors ausführen.

Der Bereitstellungsvorgang dauert in der Regel maximal zwei Stunden. Gelegentlich kann die Bereitstellung bis zu vier Stunden dauern. Wählen Sie zum Überprüfen des Bereitstellungsstatus im [Azure-Portal](https://portal.azure.com) auf Ihrer Azure-Symbolleiste das Benachrichtigungssymbol aus, um den Benachrichtigungsbereich zu öffnen.

> [!NOTE]
> Wenn die Bereitstellung fehlschlägt oder Sie Ihre ISE löschen, kann es bis zu einer Stunde dauern, bis Azure Ihre Subnetze freigibt. Daher müssen Sie möglicherweise warten, bevor Sie diese Subnetze in einer anderen ISE wiederverwenden können.
>
> Wenn Sie Ihr virtuelles Netzwerk löschen, dauert es in der Regel bis zu zwei Stunden, bis Azure Ihre Subnetze freigibt, dieser Vorgang kann aber auch länger dauern. 
> Stellen Sie beim Löschen virtueller Netzwerke sicher, dass keine Ressourcen mehr verbunden sind. 
> Beachten Sie hierzu die Anleitung [Löschen eines virtuellen Netzwerks](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

## <a name="request-header"></a>Anforderungsheader

Schließen Sie die folgenden Eigenschaften in den Anforderungsheader ein:

* `Content-type`: Legen Sie diesen Eigenschaftswert auf `application/json` fest.

* `Authorization`: Legen Sie diesen Eigenschaftswert auf das Bearertoken für den Kunden fest, der Zugriff auf das gewünschte Azure-Abonnement bzw. die Ressourcengruppe hat.

### <a name="request-body-syntax"></a>Syntax des Anforderungstexts

Hier ist die Syntax des Anforderungstexts, in der die Eigenschaften für das Erstellen der ISE beschrieben werden:

```json
{
   "id": "/subscriptions/{Azure-subscription-ID/resourceGroups/{Azure-resource-group}/providers/Microsoft.Logic/integrationServiceEnvironments/{ISE-name}",
   "name": "{ISE-name}",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "{Azure-region}",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-1}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-2}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-3}",
            },
            {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Network/virtualNetworks/{virtual-network-name}/subnets/{subnet-4}",
            }
         ]
      }
   }
}
```

### <a name="request-body-example"></a>Beispiel für Anforderungstext

In diesem Beispielanforderungstext werden die Beispielwerte gezeigt:

```json
{
   "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Logic/integrationServiceEnvironments/Fabrikam-ISE",
   "name": "Fabrikam-ISE",
   "type": "Microsoft.Logic/integrationServiceEnvironments",
   "location": "WestUS2",
   "sku": {
      "name": "Premium",
      "capacity": 1
   },
   "properties": {
      "networkConfiguration": {
         "accessEndpoint": {
            // Your ISE can use the "External" or "Internal" endpoint. This example uses "External".
            "type": "External"
         },
         "subnets": [
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-1",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-2",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-3",
            },
            {
               "id": "/subscriptions/********************/resourceGroups/Fabrikam-RG/providers/Microsoft.Network/virtualNetworks/Fabrikam-VNET/subnets/subnet-4",
            }
         ]
      }
   }
}
```

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen von Ressourcen zu Ihrer Integrationsdienstumgebung (Integration Service Environment, ISE) in Azure Logic Apps](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [Verwalten von Integrationsdienstumgebungen](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)

