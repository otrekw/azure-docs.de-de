---
title: Bindungen des SignalR-Diensts für Azure Functions
description: Erfahren Sie, wie Bindungen des SignalR-Diensts in Azure Functions verwendet werden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: 1446808b77e5eea78a9912db4c7a8e2dd783f33a
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92104375"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Bindungen des SignalR-Diensts für Azure Functions

In dieser Gruppe von Artikeln wird erläutert, wie Sie SignalR-Bindungen in Azure Functions verwenden, um mit [Azure SignalR Service](https://azure.microsoft.com/services/signalr-service/) verbundene Clients zu authentifizieren und Nachrichten in Echtzeit an sie zu senden. Azure Functions unterstützt Eingabe- und Ausgabebindungen für den SignalR-Dienst.

| Action | type |
|---------|---------|
| Zurückgeben der Dienstendpunkt-URL und des Zugriffstokens | [Eingabebindung](./functions-bindings-signalr-service-input.md) |
| Senden von SignalR Service-Nachrichten |[Ausgabebindung](./functions-bindings-signalr-service-output.md) |

## <a name="add-to-your-functions-app"></a>Hinzufügen zu Ihrer Funktions-App

### <a name="functions-2x-and-higher"></a>Functions 2.x und höher

Das Arbeiten mit Triggern und Bindungen erfordert, dass Sie auf das entsprechende Paket verweisen. Das NuGet-Paket wird für .NET-Klassenbibliotheken verwendet, während das Erweiterungspaket für alle anderen Anwendungstypen verwendet wird.

| Sprache                                        | Hinzufügen nach...                                   | Bemerkungen 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installieren des [NuGet-Paket], Version 3.x | |
| C#-Skript, Java, JavaScript, Python, PowerShell | Registrieren des [Erweiterungspaket]          | Die [Azure-Tools-Erweiterung] wird zur Verwendung mit Visual Studio Code empfohlen. |
| C#-Skript (nur online im Azure-Portal)         | Hinzufügen einer Bindung                            | Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen]. |

[NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService
[core tools]: ./functions-run-local.md
[Erweiterungspaket]: ./functions-bindings-register.md#extension-bundles
[Aktualisieren Ihrer Erweiterungen]: ./functions-bindings-register.md
[Azure-Tools-Erweiterung]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

Ausführliche Informationen zum Konfigurieren und Verwenden von SignalR Service und Azure Functions finden Sie unter [Azure Functions-Entwicklung und -Konfiguration mit Azure SignalR Service](../azure-signalr/signalr-concept-serverless-development-config.md).

### <a name="annotations-library-java-only"></a>Anmerkungsbibliothek (nur Java)

Um die SignalR Service-Anmerkungen in Java-Funktionen verwenden zu können, müssen Sie in *pom.xml* dem Artefakt *azure-functions-java-library-signalr* (Version 1.0 oder höher) eine Abhängigkeit hinzufügen.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="next-steps"></a>Nächste Schritte

- [Zurückgeben der Dienstendpunkt-URL und des Zugriffstokens (Eingabebindung)](./functions-bindings-signalr-service-input.md)
- [Senden von SignalR Service-Nachrichten (Ausgabebindung)](./functions-bindings-signalr-service-output.md)