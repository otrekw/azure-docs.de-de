---
title: HTTP-Trigger und -Bindungen in Azure Functions
description: Erfahren Sie, wie HTTP-Trigger und -Bindungen in Azure Functions verwendet werden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.openlocfilehash: 5236a3be9f8908d886274764dfc0c0da5b3565bc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92104477"
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
[Aktualisieren Ihrer Erweiterungen]: ./functions-bindings-register.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x-Apps enthalten automatisch einen Verweis auf das NuGet-Paket, Version 2.x, [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs).

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion aus einer HTTP-Anforderung](./functions-bindings-http-webhook-trigger.md)
- [Zurückgeben einer HTTP-Antwort von einer Funktion](./functions-bindings-http-webhook-output.md)