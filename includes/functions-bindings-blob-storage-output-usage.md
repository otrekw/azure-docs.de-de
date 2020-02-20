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
ms.openlocfilehash: 0c0ab0e62a5d951f0bc0e237f44cf55c5b8e16cc
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201967"
---
Sie können sich an die folgenden Typen binden, um Blobs zu schreiben:

* `TextWriter`
* `out string`
* `out Byte[]`
* `CloudBlobStream`
* `Stream`
* `CloudBlobContainer`<sup>1</sup>
* `CloudBlobDirectory`
* `ICloudBlob`<sup>2</sup>
* `CloudBlockBlob`<sup>2</sup>
* `CloudPageBlob`<sup>2</sup>
* `CloudAppendBlob`<sup>2</sup>

<sup>1</sup> Erfordert die in-Bindung `direction` in *function.json* oder `FileAccess.Read` in einer C#-Klassenbibliothek. Allerdings können Sie das Containerobjekt verwenden, das die Runtime für Schreibvorgänge bereitstellt (z.B. zum Hochladen von Blobs in den Container).

<sup>2</sup> Erfordert die inout-Bindung `direction` in *function.json* oder `FileAccess.ReadWrite` in einer C#-Klassenbibliothek.

Wenn Sie versuchen, eine Bindung an einen der Storage SDK-Typen herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x) vorliegt.

In asynchronen Funktionen verwenden Sie den Rückgabewert oder `IAsyncCollector` anstelle eines `out`-Parameters.

Eine Bindung mit `string` oder `Byte[]` wird nur bei kleinen Blobs empfohlen, da der gesamte Blobinhalt in den Arbeitsspeicher geladen wird. Im Allgemeinen ist es günstiger, die Typen `Stream` oder `CloudBlockBlob` zu verwenden. Weitere Informationen finden Sie unter [Nebenläufigkeit und Arbeitsspeichernutzung](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage) weiter oben in diesem Artikel.
