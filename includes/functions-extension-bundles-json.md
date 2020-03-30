---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f47a543143c949715fe2a49adccf074759a346fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79381838"
---
```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

In `extensionBundle` sind die folgenden Eigenschaften verfügbar:

| Eigenschaft | BESCHREIBUNG |
| -------- | ----------- |
| id | Der Namespace für Microsoft Azure Functions-Erweiterungsbündel. |
| version | Die Version des zu installierenden Pakets. Die Functions Runtime wählt immer die zulässige Höchstversion aus, die durch den Versionsbereich oder das Intervall definiert ist. Mit dem obigen Versionswert sind alle Bündelversionen von 1.0.0 bis kleiner als 2.0.0 zulässig. Weitere Informationen finden Sie im Abschnitt zur [Intervallnotation zum Angeben von Versionsbereichen](/nuget/reference/package-versioning#version-ranges). |

Bündelversionen werden erhöht, wenn sich Pakete im Bündel ändern. Hauptversionsänderungen erfolgen nur, wenn sich bei Paketen im Bündel die Hauptversion erhöht. Änderungen der Hauptversion im Bündel gehen in der Regel mit einer Änderung der Hauptversion der Functions-Runtime einher.  

Der aktuelle Erweiterungssatz, der vom Standardbündel installiert wird, ist in der [Datei „extensions.json“](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) aufgelistet.
