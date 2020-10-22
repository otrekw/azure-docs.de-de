---
title: Abrufen des Typs und des SKU-Namens eines Speicherkontos mit .NET
titleSuffix: Azure Storage
description: Hier erfahren Sie, wie Sie den Typ und den SKU-Namen eines Azure Storage-Kontos mit der .NET-Clientbibliothek abrufen.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/06/2019
ms.service: storage
ms.subservice: common
ms.topic: how-to
ms.custom: devx-track-csharp
ms.openlocfilehash: 17f18f84ac1c1738f8c248bb0071c748e15dacf3
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92090929"
---
# <a name="get-storage-account-type-and-sku-name-with-net"></a>Abrufen des Typs und des SKU-Namens eines Speicherkontos mit .NET

In diesem Artikel wird gezeigt, wie Sie mithilfe der [Azure Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage) den Azure Storage-Kontotyp und -SKU-Namen für einen Blob abrufen.

Kontoinformationen sind für Dienstversionen verfügbar, die mit „2018-03-28“ beginnen.

## <a name="about-account-type-and-sku-name"></a>Abrufen von Kontotyp und SKU-Name

**Kontotyp:** Gültige Kontotypen sind `BlobStorage`, `BlockBlobStorage`, `FileStorage`, `Storage` und `StorageV2`. In der [Übersicht über Azure Storage-Konten](storage-account-overview.md) finden Sie weitere Informationen, u. a. Beschreibungen der verschiedenen Speicherkonten.

**SKU-Name:** Zu den gültigen SKU-Namen zählen `Premium_LRS`, `Premium_ZRS`, `Standard_GRS`, `Standard_GZRS`, `Standard_LRS`, `Standard_RAGRS`, `Standard_RAGZRS` und `Standard_ZRS`. Bei SKU-Namen muss die Groß-/Kleinschreibung beachtet werden, und es handelt sich dabei um Zeichenfolgenfelder in der [SkuName-Klasse](/dotnet/api/microsoft.azure.management.storage.models.skuname).

## <a name="retrieve-account-information"></a>Abrufen von Kontoinformationen

Rufen Sie zum Abrufen des Speicherkontotyps und -SKU-Namens eines Blobs die Methode [GetAccountProperties](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountproperties) oder [GetAccountPropertiesAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getaccountpropertiesasync) auf.

Im folgenden Codebeispiel werden die schreibgeschützten Kontoeigenschaften abgerufen und angezeigt.

```csharp
private static async Task GetAccountInfoAsync(CloudBlob blob)
{
    try
    {
        // Get the blob's storage account properties.
        AccountProperties acctProps = await blob.GetAccountPropertiesAsync();

        // Display the properties.
        Console.WriteLine("Account properties");
        Console.WriteLine("  AccountKind: {0}", acctProps.AccountKind);
        Console.WriteLine("      SkuName: {0}", acctProps.SkuName);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über andere Vorgänge, die Sie über das [Azure-Portal](https://portal.azure.com) und die Azure-Rest-API für ein Speicherkonto ausführen können.

- [Abrufen von Kontoinformationen (REST)](/rest/api/storageservices/get-account-information)
