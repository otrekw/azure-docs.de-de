---
title: include file
description: include file
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 3788fdb954917f28f64a0dfe035bed4ded0932d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "69641874"
---
Der Blobtrigger stellt mehrere Metadateneigenschaften bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Diese Werte haben die gleiche Semantik wie der Typ [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet).

|Eigenschaft  |type  |BESCHREIBUNG  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Der Pfad des auslösenden Blobs.|
|`Uri`|`System.Uri`|Der Blob-URI für den primären Speicherort|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Die Systemeigenschaften des Blobs |
|`Metadata` |`IDictionary<string,string>`|Die benutzerdefinierten Metadaten für das Blob|

Beispielsweise protokollieren die folgenden C#-Skript- und JavaScript-Beispiele den Pfad zum auslösenden Blob, einschließlich des Containers:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```
