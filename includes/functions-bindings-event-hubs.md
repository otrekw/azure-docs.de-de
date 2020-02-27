---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 47e9e37676a4afa9ec29393bf970c368f3f9e5be
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77586126"
---
## <a name="add-to-your-functions-app"></a>Hinzufügen zu Ihrer Funktions-App

### <a name="functions-2x-and-higher"></a>Functions 2.x und höher

Das Arbeiten mit Triggern und Bindungen erfordert, dass Sie auf das entsprechende Paket verweisen. Das NuGet-Paket wird für .NET-Klassenbibliotheken verwendet, während das Erweiterungspaket für alle anderen Anwendungstypen verwendet wird.

| Sprache                                        | Hinzufügen nach...                                   | Bemerkungen 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installieren des [NuGet-Paket], Version 3.x | |
| C#-Skript, Java, JavaScript, Python, PowerShell | Registrieren des [Erweiterungspaket]          | Die [Azure-Tools-Erweiterung] wird zur Verwendung mit Visual Studio Code empfohlen. |
| C#-Skript (nur online im Azure-Portal)         | Hinzufügen einer Bindung                            | Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen]. |

[NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs
[core tools]: ../articles/azure-functions/functions-run-local.md
[Erweiterungspaket]: ../articles/azure-functions/functions-bindings-register.md#extension-bundles
[Aktualisieren Ihrer Erweiterungen]: ../articles/azure-functions/install-update-binding-extensions-manual.md
[Azure-Tools-Erweiterung]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x-Apps enthalten automatisch einen Verweis auf das NuGet-Paket, Version 2.x, [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs).