---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f64892193eb6cfcce8f948b54e5557b5fa3d90ab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80878243"
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

Der aktuelle Erweiterungssatz, der vom Standardbündel installiert wird, ist in der [Datei „extensions.json“](https://github.com/Azure/azure-functions-extension-bundles/blob/dev/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json) aufgelistet.
