---
title: Verwalten von Streamingendpunkten mit Azure Media Services v3
description: In diesem Artikel erfahren Sie, wie Sie Streamingendpunkte mit Azure Media Services v3 verwalten.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: 8d3835d78ec55f120f6b9439aea9c29434e6bd47
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89289476"
---
# <a name="manage-streaming-endpoints-with--media-services-v3"></a>Verwalten von Streamingendpunkten in Media Services v3

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein **Standard**[-Streamingendpunkt](streaming-endpoint-concept.md) im Zustand **Beendet** hinzugefügt. Um mit dem Streamen Ihrer Inhalte zu beginnen und die [dynamische Paketerstellung](dynamic-packaging-overview.md) und [dynamische Verschlüsselung](content-protection-overview.md) zu nutzen, muss der Streamingendpunkt, von dem Sie Inhalte streamen möchten, den Zustand **Wird ausgeführt** aufweisen.

In diesem Artikel erfahren Sie, wie Sie mithilfe verschiedener Technologien den [Start](/rest/api/media/streamingendpoints/start)-Befehl für Ihren Streamingendpunkt ausführen. 
 
> [!NOTE]
> Abgerechnet werden nur ausgeführte Streamingendpunkte.
    
## <a name="prerequisites"></a>Voraussetzungen

Überprüfung: 

* [Media Services-Konzepte](concepts-overview.md)
* [Streamingendpunktkonzept](streaming-endpoint-concept.md)
* [Dynamische Paketerstellung](dynamic-packaging-overview.md)

## <a name="use-rest"></a>Verwenden von REST

```rest
POST https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/slitestmedia10/streamingEndpoints/myStreamingEndpoint1/start?api-version=2018-07-01
```

Weitere Informationen finden Sie unter 

* Referenzdokumentation zum [Starten eines Streamingendpunkts](/rest/api/media/streamingendpoints/start)
* Das Starten eines Streamingendpunkts ist ein asynchroner Vorgang. 

    Weitere Informationen zum Überwachen von Vorgängen mit langer Ausführungszeit finden Sie unter [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md).
* Diese [Postman-Sammlung](https://github.com/Azure-Samples/media-services-v3-rest-postman/blob/master/Postman/Media%20Services%20v3.postman_collection.json) enthält Beispiele für mehrere REST-Vorgänge, u. a. für das Starten eines Streamingendpunkts.

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals 
 
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Navigieren Sie zu Ihrem Azure Media Services-Konto.
1. Wählen Sie im linken Bereich **Streamingendpunkte** aus.
1. Wählen Sie den Streamingendpunkt aus, den Sie starten möchten, und wählen Sie dann **Starten** aus.

## <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI

```cli
az ams streaming-endpoint start [--account-name]
                                [--ids]
                                [--name]
                                [--no-wait]
                                [--resource-group]
                                [--subscription]
```

Weitere Informationen finden Sie unter [az ams streaming-endpoint start](/cli/azure/ams/streaming-endpoint?view=azure-cli-latest#az-ams-streaming-endpoint-start).

## <a name="use-sdks"></a>Verwenden von SDKs

### <a name="java"></a>Java
    
```java
if (streamingEndpoint != null) {
// Start The Streaming Endpoint if it is not running.
if (streamingEndpoint.resourceState() != StreamingEndpointResourceState.RUNNING) {
    manager.streamingEndpoints().startAsync(config.getResourceGroup(), config.getAccountName(), STREAMING_ENDPOINT_NAME).await();
}
```

Sehen Sie sich das vollständige [Java-Codebeispiel](https://github.com/Azure-Samples/media-services-v3-java/blob/master/DynamicPackagingVODContent/StreamHLSAndDASH/src/main/java/sample/StreamHLSAndDASH.java#L128) an.

### <a name="net"></a>.NET

```csharp
StreamingEndpoint streamingEndpoint = await client.StreamingEndpoints.GetAsync(config.ResourceGroup, config.AccountName, DefaultStreamingEndpointName);

if (streamingEndpoint != null)
{
    if (streamingEndpoint.ResourceState != StreamingEndpointResourceState.Running)
    {
        await client.StreamingEndpoints.StartAsync(config.ResourceGroup, config.AccountName, DefaultStreamingEndpointName);
    }
```

Sehen Sie sich das vollständige [.NET-Codebeispiel](https://github.com/Azure-Samples/media-services-v3-dotnet/blob/master/DynamicPackagingVODContent/StreamHLSAndDASH/Program.cs#L112) an.

---

## <a name="next-steps"></a>Nächste Schritte

* [OpenAPI-Spezifikation von Media Services v3 (Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01)
* [Streamingendpunktvorgänge](/rest/api/media/streamingendpoints)
