---
title: Azure RabbitMQ-Bindungen für Azure Functions
description: Erfahren Sie, wie Sie Azure RabbitMQ-Trigger und -Bindungen in Azure Functions senden.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/17/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: 2a480f25821f5022295b18ca24abfd2c0fb8a50c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97746523"
---
# <a name="rabbitmq-bindings-for-azure-functions-overview"></a>Übersicht über RabbitMQ-Bindungen für Azure Functions

> [!NOTE]
> RabbitMQ-Bindungen werden nur mit den Plänen **Premium und Dedicated** vollständig unterstützt. Der Verbrauchstarif wird nicht unterstützt.

Die Integration von Azure Functions mit [RabbitMQ](https://www.rabbitmq.com/) erfolgt über [Trigger und Bindungen](./functions-triggers-bindings.md). Die Azure Functions-Erweiterungen für RabbitMQ ermöglichen das Senden und Empfangen von Nachrichten mithilfe der RabbitMQ-API und Functions.

| Aktion | type |
|---------|---------|
| Ausführen einer Funktion, wenn eine RabbitMQ-Nachricht die Warteschlange durchläuft | [Trigger](./functions-bindings-rabbitmq-trigger.md) |
| Senden von RabbitMQ-Nachrichten |[Ausgabebindung](./functions-bindings-rabbitmq-output.md) |

## <a name="add-to-your-functions-app"></a>Hinzufügen zu Ihrer Funktions-App

Wenn Sie mit dieser Erweiterung etwas entwickeln möchten, müssen Sie zunächst einen [RabbitMQ-Endpunkt einrichten](https://github.com/Azure/azure-functions-rabbitmq-extension/wiki/Setting-up-a-RabbitMQ-Endpoint). Weitere Informationen zu RabbitMQ finden Sie auf der [Seite mit den ersten Schritten](https://www.rabbitmq.com/getstarted.html).

### <a name="functions-3x-and-higher"></a>Functions 3.x und höher

Das Arbeiten mit Triggern und Bindungen erfordert, dass Sie auf das entsprechende Paket verweisen. Das NuGet-Paket wird für .NET-Klassenbibliotheken verwendet, während das Erweiterungspaket für alle anderen Anwendungstypen verwendet wird.

| Sprache                                        | Hinzufügen nach...                                   | Bemerkungen
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installieren der Version 4.x des [NuGet-Paket] | |
| C#-Skript, Java, JavaScript, Python, PowerShell | Registrieren des [Erweiterungspaket]          | Die [Erweiterung für Azure-Tools] wird zur Verwendung mit Visual Studio Code empfohlen. |
| C#-Skript (nur online im Azure-Portal)         | Hinzufügen einer Bindung                            | Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen]. |

[NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.RabbitMQ
[core tools]: ./functions-run-local.md
[Erweiterungspaket]: ./functions-bindings-register.md#extension-bundles
[Aktualisieren Ihrer Erweiterungen]: ./functions-bindings-register.md
[Azure-Tools-Erweiterung]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x-and-2x"></a>Functions 1.x und 2.x

Die Erweiterungen für RabbitMQ-Bindungen werden für Functions 1.x und 2.x nicht unterstützt. Verwenden Sie Functions 3.x oder höher.

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion, wenn eine RabbitMQ-Nachricht erstellt wird (Trigger)](./functions-bindings-rabbitmq-trigger.md)
- [Senden von RabbitMQ-Nachrichten mit Azure Functions (Ausgabebindung)](./functions-bindings-rabbitmq-output.md)