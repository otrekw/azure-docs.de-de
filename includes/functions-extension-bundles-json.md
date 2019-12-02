---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: 4ce048cafa4c20e3bbdbd8ecf1669748531ee122
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2019
ms.locfileid: "74129075"
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
| version | Die Version des zu installierenden Pakets. Die Functions Runtime wählt immer die zulässige Höchstversion aus, die durch den Versionsbereich oder das Intervall definiert ist. Mit dem obigen Versionswert sind alle Bündelversionen von 1.0.0 bis kleiner als 2.0.0 zulässig. Weitere Informationen finden Sie im Abschnitt zur [Intervallnotation zum Angeben von Versionsbereichen](/nuget/reference/package-versioning#version-ranges-and-wildcards). |

Bündelversionen werden erhöht, wenn sich Pakete im Bündel ändern. Hauptversionsänderungen erfolgen nur, wenn sich bei Paketen im Bündel die Hauptversion erhöht. Änderungen der Hauptversion im Bündel gehen in der Regel mit einer Änderung der Hauptversion der Functions-Runtime einher.  

Der aktuelle Erweiterungssatz, der vom Standardbündel installiert wird, ist in der [Datei „extensions.json“](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) aufgelistet.
