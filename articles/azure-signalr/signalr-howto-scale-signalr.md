---
title: Skalieren einer Azure SignalR Service-Instanz
description: Erfahren Sie, wie Sie eine Azure SignalR Service-Instanz über das Azure-Portal oder die Azure CLI skalieren, um die Kapazität zu erhöhen oder zu verringern.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 9/9/2020
ms.author: zhshang
ms.custom: devx-track-azurecli
ms.openlocfilehash: bb01f2f96e5db4c94e759b114818360e6084255f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89595766"
---
# <a name="how-to-scale-an-azure-signalr-service-instance"></a>Skalieren einer Azure SignalR Service-Instanz
In diesem Artikel erfahren Sie, wie Sie Ihre Azure SignalR Service-Instanz skalieren. Es gibt zwei Szenarios für die Skalierung: Hochskalieren und Aufskalieren.

* [Hochskalieren](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Erhalten Sie weitere Einheiten, Verbindungen, Nachrichten und mehr. Sie können hochskalieren, indem Sie den Tarif von Free in Standard ändern.
* [Aufskalieren:](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling) Erhöhen Sie die Anzahl von SignalR-Einheiten. Sie können auf bis zu 100 Einheiten aufskalieren. Für die Skalierung stehen eingeschränkte Einheitenoptionen zur Verfügung: 1, 2, 5, 10, 20, 50 und 100 Einheiten für eine einzelne SignalR Service-Instanz.

Es dauert einige Minuten, bis die Skalierungseinstellungen angewendet werden. In seltenen Fällen kann dies etwa 30 Minuten dauern. Dazu müssen Sie weder Ihren Code ändern noch Ihre Serveranwendung neu bereitstellen.

Informationen zur Preisgestaltung und zu den Funktionen der einzelnen SignalR Service-Instanzen finden Sie unter [Azure SignalR Service – Preise ](https://azure.microsoft.com/pricing/details/signalr-service/).  

> [!NOTE]
> Wenn Sie den Tarif von SignalR Service von **Free** in **Standard** oder umgekehrt ändern, ändert sich die öffentliche IP-Adresse des Diensts. Es dauert in der Regel 30-60 Minuten, bis die Änderung an die DNS-Server im gesamten Internet weitergegeben wurde. Ihr Dienst ist möglicherweise nicht erreichbar, bis der DNS-Server aktualisiert wurde. Im Allgemeinen wird nicht empfohlen, Ihren Tarif häufig zu ändern.


## <a name="scale-on-azure-portal"></a>Skalieren im Azure-Portal

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com) in Ihrem Browser.

2. Wählen Sie auf der SignalR Service-Seite im Menü auf der linken Seite **Skalieren** aus.
   
3. Wählen Sie Ihren Tarif aus, und klicken Sie auf **Auswählen**. Legen Sie die Anzahl von Einheiten für den Tarif **Standard** fest.
   
    ![Skalieren im Portal](./media/signalr-howto-scale/signalr-howto-scale.png)

4. Klicken Sie auf **Speichern**.

## <a name="scale-using-azure-cli"></a>Skalieren über die Azure-Befehlszeilenschnittstelle

Dieses Skript erstellt eine neue SignalR Service-Ressource im Tarif **Free** sowie eine neue Ressourcengruppe, und skaliert sie auf den Tarif **Standard** hoch. 

```azurecli-interactive
#!/bin/bash

# Generate a unique suffix for the service name
let randomNum=$RANDOM*$RANDOM

# Generate a unique service and group name with the suffix
SignalRName=SignalRTestSvc$randomNum
#resource name must be lowercase
mySignalRSvcName=${SignalRName,,}
myResourceGroupName=$SignalRName"Group"

# Create resource group 
az group create --name $myResourceGroupName --location eastus

# Create the Azure SignalR Service resource
az signalr create \
  --name $mySignalRSvcName \
  --resource-group $myResourceGroupName \
  --sku Free_F1 \
  --service-mode Default

# Scale up to Standard Tier, and scale out to 50 units
az signalr update \
  --name $mySignalRSvcName \
  --resource-group $myResourceGroupName \
  --sku Standard_S1 \
  --unit-count 50
```

Notieren Sie den tatsächlichen Namen, der für die neue Ressourcengruppe generiert wurde. Dieser Ressourcengruppenname wird verwendet, wenn Sie alle Gruppenressourcen löschen möchten.

[!INCLUDE [cli-script-clean-up](../../includes/cli-script-clean-up.md)]

## <a name="compare-pricing-tiers"></a>Vergleichen von Tarifen

Ausführliche Informationen, z. B. enthaltene Meldungen und Verbindungen für jeden Tarif, finden Sie unter [Azure SignalR Service – Preise](https://azure.microsoft.com/pricing/details/signalr-service/).

Eine Tabelle mit Dienstgrenzwerten, Kontingenten und Einschränkungen in jedem Tarif finden Sie unter [Grenzwerte für Azure SignalR Service](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-signalr-service-limits).

## <a name="next-steps"></a>Nächste Schritte

In dieser Anleitung haben Sie erfahren, wie Sie eine einzelne SignalR Service-Instanzen skalieren.

Mehrere Endpunkte werden auch für die Skalierung, das Sharding und regionsübergreifende Szenarien unterstützt.

> [!div class="nextstepaction"]
> [Skalieren von SignalR Service mit mehreren Instanzen](./signalr-howto-scale-multi-instances.md)
