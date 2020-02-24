---
title: HTTP-Trigger und -Bindungen in Azure Functions
description: Erfahren Sie, wie HTTP-Trigger und -Bindungen in Azure Functions verwendet werden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.openlocfilehash: 29b5e9c7673b4a730a41bf7cf2b1c4a2a86209ed
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462104"
---
# <a name="azure-functions-http-triggers-and-bindings-overview"></a>Übersicht über HTTP-Trigger und -Bindungen in Azure Functions

Azure Functions kann mittels HTTP-Anforderungen aufgerufen werden, um serverlose APIs zu erstellen und auf [Webhooks](https://en.wikipedia.org/wiki/Webhook) zu antworten.

| Aktion | type |
|---------|---------|
| Ausführen einer Funktion aus einer HTTP-Anforderung | [Trigger](./functions-bindings-http-webhook-trigger.md) |
| Zurückgeben einer HTTP-Antwort von einer Funktion |[Ausgabebindung](./functions-bindings-http-webhook-output.md) |

Der Code in diesem Artikel nutzt standardmäßig .NET Core-Syntax, die in Functions ab Version 2.x verwendet wird. Informationen zur 1.x-Syntax finden Sie in den [1.x-Functions-Vorlagen](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="add-to-your-functions-app"></a>Hinzufügen zu Ihrer Funktions-App

### <a name="functions-2x-and-higher"></a>Functions 2.x und höher

Das Arbeiten mit Triggern und Bindungen erfordert, dass Sie auf das entsprechende Paket verweisen. Das NuGet-Paket wird für .NET-Klassenbibliotheken verwendet, während das Erweiterungspaket für alle anderen Anwendungstypen verwendet wird.

| Sprache                                        | Hinzufügen nach...                                   | Bemerkungen 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installieren des [NuGet-Paket], Version 3.x | |
| C#-Skript, Java, JavaScript, Python, PowerShell | Registrieren des [Erweiterungspaket]          | Die [Erweiterung für Azure-Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) wird zur Verwendung mit Visual Studio Code empfohlen. |
| C#-Skript (nur online im Azure-Portal)         | Hinzufügen einer Bindung                            | Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen]. |

[core tools]: ./functions-run-local.md
[Erweiterungspaket]: ./functions-bindings-register.md#extension-bundles
[NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http
[Aktualisieren Ihrer Erweiterungen]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x-Apps enthalten automatisch einen Verweis auf das NuGet-Paket, Version 2.x, [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs).

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion aus einer HTTP-Anforderung](./functions-bindings-http-webhook-trigger.md)
- [Zurückgeben einer HTTP-Antwort von einer Funktion](./functions-bindings-http-webhook-output.md)
