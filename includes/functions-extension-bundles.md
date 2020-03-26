---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: d697334fe56fb9133a06cee79067c60bc3a37281
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "68639144"
---
Der einfachste Weg, Bindungserweiterungen zu installieren, ist die Aktivierung von [Erweiterungspaketen](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Wenn Sie Pakete aktivieren, wird automatisch ein vordefinierter Satz von Erweiterungspaketen installiert.

Um Erweiterungspakete zu aktivieren, öffnen Sie die Datei „host.json“, und aktualisieren Sie ihren Inhalt entsprechend dem folgenden Code:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```