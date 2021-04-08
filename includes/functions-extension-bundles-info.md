---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 1fc37c6f93fba34944caa7a91c2a89ce5dcdc398
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "78201021"
---
::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
> [!TIP]
> Beim Starten führt der Host den Download und die Installation der [Storage-Bindungserweiterung](../articles/azure-functions/functions-bindings-storage-queue.md#functions-2x-and-higher) und anderer Microsoft-Bindungserweiterungen durch. Diese Installation erfolgt, weil Bindungserweiterungen in der Datei *host.json* mit den folgenden Eigenschaften standardmäßig aktiviert sind:
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> Falls bei Ihnen Fehler in Bezug auf Bindungserweiterungen auftreten, sollten Sie sich vergewissern, dass die obigen Eigenschaften in *host.json* enthalten sind.
::: zone-end  