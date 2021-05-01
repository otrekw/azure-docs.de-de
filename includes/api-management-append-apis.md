---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 04/16/2021
ms.author: vlvinogr
ms.openlocfilehash: 329ea156b296810395eb7b8e8310bed5ee0ee4c9
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2021
ms.locfileid: "107601927"
---
## <a name="append-other-apis"></a>Anfügen weiterer APIs

Sie können eine API aus APIs zusammensetzen, die von verschiedenen Diensten verfügbar gemacht werden, u. a.:
* OpenAPI-Spezifikation
* SOAP-API
* API-Apps-Feature von Azure App Service
* Azure-Funktionen-App
* Azure Logic Apps
* Azure Service Fabric

Führen Sie zum Anfügen einer anderen API an eine vorhandene API die folgenden Schritte aus. 

>[!NOTE] 
> Wenn Sie eine andere API importieren, werden die Vorgänge an Ihre aktuelle API angefügt.

1. Navigieren Sie im Azure-Portal zu Ihrer Azure API Management-Instanz.

    :::image type="content" source="./media/api-management-append-apis/service-page.png" alt-text="Navigieren zur Azure API Management-Instanz":::

1. Wählen Sie im Menü links **APIs** aus.

    :::image type="content" source="./media/api-management-append-apis/api-select.png" alt-text="Auswählen von „APIs“":::

1. Klicken Sie neben der API, die Sie an eine andere API anfügen möchten, auf **...**.
1. Wählen Sie im Dropdownmenü **Importieren** aus.

    :::image type="content" source="./media/api-management-append-apis/append-01.png" alt-text="Auswählen von „Importieren“":::

1. Wählen Sie einen Dienst aus, aus dem eine API importiert werden soll.

    :::image type="content" source="./media/api-management-append-apis/select-to-import.png" alt-text="Auswählen des Diensts":::