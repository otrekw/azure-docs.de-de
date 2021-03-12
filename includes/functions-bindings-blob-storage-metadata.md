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
ms.openlocfilehash: c6b038297945ca900508a822460e1358a2524d23
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102455912"
---
Der Blobtrigger stellt mehrere Metadateneigenschaften bereit. Diese Eigenschaften können als Teil der Bindungsausdrücke in anderen Bindungen oder als Parameter im Code verwendet werden. Diese Werte haben die gleiche Semantik wie der Typ [CloudBlob](/dotnet/api/microsoft.azure.storage.blob.cloudblob).

|Eigenschaft  |Typ  |Beschreibung  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Der Pfad des auslösenden Blobs.|
|`Uri`|`System.Uri`|Der Blob-URI für den primären Speicherort|
|`Properties` |[BlobProperties](/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Die Systemeigenschaften des Blobs |
|`Metadata` |`IDictionary<string,string>`|Die benutzerdefinierten Metadaten für das Blob|

Beispielsweise protokollieren die folgenden C#-Skript- und JavaScript-Beispiele den Pfad zum auslösenden Blob, einschließlich des Containers:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```