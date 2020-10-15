---
title: Azure Service Bus-Bindungen für Azure Functions
description: Erfahren Sie, wie Azure Service Bus-Trigger und -Bindungen in Azure Functions verwendet werden.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: 5e15dfec049197fa056cbd55fd839b3eb93be77c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90530368"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Service Bus-Bindungen für Azure Functions

Azure Functions wird über [Trigger und Bindungen](./functions-triggers-bindings.md) in [Azure Service Bus](https://azure.microsoft.com/services/service-bus) integriert. Durch die Integration mit Service Bus können Sie Funktionen erstellen, die auf Warteschlangen- oder Themennachrichten reagieren und diese senden.

| Aktion | type |
|---------|---------|
| Ausführen einer Funktion, wenn eine Warteschlangen- oder Themennachricht von Service Bus empfangen wird | [Trigger](./functions-bindings-service-bus-trigger.md) |
| Senden von Azure Service Bus-Nachrichten |[Ausgabebindung](./functions-bindings-service-bus-output.md) |

## <a name="add-to-your-functions-app"></a>Hinzufügen zu Ihrer Funktions-App

> [!NOTE]
> Von der Service Bus-Bindung wird die Authentifizierung mithilfe einer verwaltete Identität derzeit nicht unterstützt. Verwenden Sie stattdessen eine [Service Bus-Zugriffssteuerung mit Shared Access Signatures](../service-bus-messaging/service-bus-authentication-and-authorization.md#shared-access-signature).

### <a name="functions-2x-and-higher"></a>Functions 2.x und höher

Das Arbeiten mit Triggern und Bindungen erfordert, dass Sie auf das entsprechende Paket verweisen. Das NuGet-Paket wird für .NET-Klassenbibliotheken verwendet, während das Erweiterungspaket für alle anderen Anwendungstypen verwendet wird.

| Sprache                                        | Hinzufügen nach...                                   | Bemerkungen 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installieren der Version 4.x des [NuGet-Paket] | |
| C#-Skript, Java, JavaScript, Python, PowerShell | Registrieren des [Erweiterungspaket]          | Die [Azure-Tools-Erweiterung] wird zur Verwendung mit Visual Studio Code empfohlen. |
| C#-Skript (nur online im Azure-Portal)         | Hinzufügen einer Bindung                            | Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen]. |

[NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus/
[core tools]: ./functions-run-local.md
[Erweiterungspaket]: ./functions-bindings-register.md#extension-bundles
[Aktualisieren Ihrer Erweiterungen]: ./install-update-binding-extensions-manual.md
[Azure-Tools-Erweiterung]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x-Apps enthalten automatisch einen Verweis auf das NuGet-Paket, Version 2.x, [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs).

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion, wenn eine Warteschlangen- oder Themennachricht von Service Bus empfangen wird (Trigger)](./functions-bindings-service-bus-trigger.md)
- [Senden von Azure Service Bus-Nachrichten mit Azure Functions (Ausgabebindung)](./functions-bindings-service-bus-output.md)
