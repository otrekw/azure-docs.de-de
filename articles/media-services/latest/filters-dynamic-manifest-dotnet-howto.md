---
title: Erstellen von Filtern mit dem Azure Media Services v3 .NET SDK
description: In diesem Thema wird erläutert, wie Sie Filter erstellen, mit denen Ihre Kunden bestimmte Abschnitte eines Streams streamen können. Das Media Services v3 .NET SDK erstellt dynamische Manifeste, um dieses selektive Streaming zu erreichen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 11c65498d5a31c2e2ee997bdaf18037b1f0f9060
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102455211"
---
# <a name="create-filters-with-media-services-net-sdk"></a>Erstellen von Filtern mit dem Media Services .NET SDK

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bei der Inhaltsbereitstellung für Ihre Kunden (Streaming von Liveereignissen oder Video on Demand) benötigen Ihre Kunden möglicherweise mehr Flexibilität als in der Manifestdatei für das Standardmedienobjekt beschrieben. Azure Media Services ermöglicht es Ihnen, Kontofilter und Medienobjektfilter für Ihre Inhalte zu definieren.

Eine ausführliche Beschreibung dieses Features und der Szenarien, in denen es verwendet wird, finden Sie unter [Dynamische Manifeste](filters-dynamic-manifest-overview.md) und [Filter](filters-concept.md).

In diesem Thema wird beschrieben, wie Sie mit dem Media Services .NET SDK einen Filter für ein Video on Demand-Medienobjekt sowie [Kontofilter](/dotnet/api/microsoft.azure.management.media.models.accountfilter) und [Medienobjektfilter](/dotnet/api/microsoft.azure.management.media.models.assetfilter) erstellen. 

> [!NOTE]
> Lesen Sie die Informationen zu [presentationTimeRange](filters-concept.md#presentationtimerange).

## <a name="prerequisites"></a>Voraussetzungen 

- Lesen Sie [Filter und dynamische Manifeste](filters-dynamic-manifest-overview.md).
- [Erstellen Sie ein Media Services-Konto.](./create-account-howto.md) Merken Sie sich den Namen der Ressourcengruppe und den Namen des Media Services-Kontos. 
- Beschaffen Sie die Informationen, die für den [Zugriff auf APIs](./access-api-howto.md) benötigt werden.
- Lesen Sie [Hochladen, Codieren und Streamen mit Azure Media Services](stream-files-tutorial-with-api.md), um sich über den [Einstieg in die Verwendung des .NET SDK](stream-files-tutorial-with-api.md#start-using-media-services-apis-with-net-sdk) zu informieren.

## <a name="define-a-filter"></a>Definieren eines Filters  

In .NET konfigurieren Sie die Spurauswahl mit den Klassen [FilterTrackSelection](/dotnet/api/microsoft.azure.management.media.models.filtertrackselection) und [FilterTrackPropertyCondition](/dotnet/api/microsoft.azure.management.media.models.filtertrackpropertycondition). 

Der folgende Code definiert einen Filter, der alle EC-3-Audiotitel und alle Videotitel mit Bitraten im Bereich von 0–1000000 einbezieht.

```csharp
var audioConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Type, "Audio", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.FourCC, "EC-3", FilterTrackPropertyCompareOperation.Equal)
};

var videoConditions = new List<FilterTrackPropertyCondition>()
{
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Type, "Video", FilterTrackPropertyCompareOperation.Equal),
    new FilterTrackPropertyCondition(FilterTrackPropertyType.Bitrate, "0-1000000", FilterTrackPropertyCompareOperation.Equal)
};

List<FilterTrackSelection> includedTracks = new List<FilterTrackSelection>()
{
    new FilterTrackSelection(audioConditions),
    new FilterTrackSelection(videoConditions)
};
```

## <a name="create-account-filters"></a>Erstellen von Kontofiltern

Der folgende Code zeigt, wie Sie .NET zum Erstellen eines Kontofilters verwenden, der die [oben definierte](#define-a-filter) Spurauswahl einschließt. 

```csharp
AccountFilter accountFilterParams = new AccountFilter(tracks: includedTracks);
client.AccountFilters.CreateOrUpdate(config.ResourceGroup, config.AccountName, "accountFilterName1", accountFilter);
```

## <a name="create-asset-filters"></a>Erstellen von Medienobjektfiltern

Der folgende Code zeigt, wie Sie .NET zum Erstellen eines Medienobjektfilters verwenden, der die [oben definierte](#define-a-filter) Spurauswahl einschließt. 

```csharp
AssetFilter assetFilterParams = new AssetFilter(tracks: includedTracks);
client.AssetFilters.CreateOrUpdate(config.ResourceGroup, config.AccountName, encodedOutputAsset.Name, "assetFilterName1", assetFilterParams);
```

## <a name="associate-filters-with-streaming-locator"></a>Zuordnen von Filtern mit Streaminglocator

Sie können eine Liste von Medienobjekt- oder Kontenfiltern angeben, die für Ihren Streaminglocator gelten würden. Der [dynamische Paketerstellungs-Manager (Streamingendpunkt)](dynamic-packaging-overview.md) wendet diese Liste der Filter zusammen mit den Filtern an, die Ihr Client in der URL angibt. Diese Kombination generiert ein [dynamisches Manifest](filters-dynamic-manifest-overview.md), das auf Filtern in den URL und Filtern basiert, die Sie im Streaminglocator angeben. Es wird empfohlen, dieses Feature zu verwenden, wenn Sie Filter anwenden, aber nicht die Filternamen in der URL verfügbar machen möchten.

Der folgende C#-Code zeigt, wie ein Streaminglocator erstellt und `StreamingLocator.Filters` angegeben wird. Dies ist eine optionale Eigenschaft, die ein `IList<string>`-Objekt mit Filternamen annimmt.

```csharp
IList<string> filters = new List<string>();
filters.Add("filterName");

StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly,
        Filters = filters
    });
```
      
## <a name="stream-using-filters"></a>Streamen unter Verwendung von Filtern

Sobald Sie Filter definiert haben, können Ihre Kunden diese in der Streaming-URL verwenden. Die Filter können auf Streaminprotokolle mit adaptiver Bitrate angewendet werden: Apple HTTP Live Streaming (HLS), MPEG-DASH und Smooth Streaming.

Die folgende Tabelle zeigt einige Beispiele für URLs mit Filtern:

|Protocol|Beispiel|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="next-steps"></a>Nächste Schritte

[Streamen von Videos](stream-files-tutorial-with-api.md) 
