---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: d697334fe56fb9133a06cee79067c60bc3a37281
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "96010478"
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