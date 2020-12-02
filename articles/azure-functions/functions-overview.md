---
title: Übersicht zu Azure Functions
description: Es wird beschrieben, wie Ihnen Azure Functions beim Entwickeln von skalierbaren serverlosen Apps als Hilfe dient.
author: craigshoemaker
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 11/20/2020
ms.author: cshoe
ms.custom: contperfq2
ms.openlocfilehash: 514f2e9a82a50f95f9c054c6a54e7b5af3c0af15
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "96167776"
---
# <a name="introduction-to-azure-functions"></a>Einführung in Azure Functions

Systeme werden häufig entwickelt, um auf eine Reihe von kritischen Ereignissen zu reagieren. Unabhängig davon, ob Sie eine Web-API entwickeln, auf Datenbankänderungen reagieren, IoT-Datenströme verarbeiten oder auch Nachrichtenwarteschlangen verwalten, gilt Folgendes: Für jede Anwendung muss es eine Möglichkeit zum Ausführen von Code geben, wenn diese Ereignisse eintreten.

Zur Erfüllung dieser Anforderung verfügt Azure Functions über zwei Optionen für bedarfsgesteuertes Computing (Compute On-Demand).

Erstens können Sie mit Azure Functions die Logik Ihres Systems in schnell verfügbaren Codeblöcken implementieren. Diese Codeblöcke werden als „Funktionen“ bezeichnet. Die verschiedenen Funktionen können jederzeit ausgeführt werden, wenn Sie auf kritische Ereignisse reagieren müssen.

Zweitens stellt Azure Functions bei einem Anstieg der Anforderungen so viele Ressourcen und Funktionsinstanzen bereit, wie dies zur Erfüllung der Nachfrage erforderlich ist – aber jeweils nur so lange, wie die Nachfrage besteht. Wenn die Anzahl von Anforderungen abnimmt, wird die Bereitstellung aller zusätzlichen Ressourcen und Anwendungsinstanzen automatisch rückgängig gemacht.

Woher kommen all diese Computeressourcen? Von Azure Functions werden [so viele bzw. wenige Computeressourcen wie nötig bereitgestellt](./functions-scale.md), um die Nachfrage Ihrer Anwendung zu erfüllen.

Die bedarfsgesteuerte Bereitstellung von Computeressourcen ist die Grundlage des [serverlosen Computings](https://azure.microsoft.com/solutions/serverless/) in Azure Functions.

## <a name="scenarios"></a>Szenarien

In vielen Fällen wird eine Funktion [in ein Array mit Clouddiensten integriert](./functions-triggers-bindings.md), um Implementierungen mit großem Funktionsumfang zu erzielen.

Unten sind einige häufige Szenarien für Azure Functions aufgeführt (_keine umfassende Liste_).

| Zweck | Aktion |
| --- | --- |
| **Entwickeln einer Web-API** | Implementieren eines Endpunkts für Ihre Webanwendungen mit dem [HTTP-Trigger](./functions-bindings-http-webhook.md) |
| **Verarbeiten von Dateiuploads** | Ausführen von Code, wenn eine Datei im [Blobspeicher](./functions-bindings-storage-blob.md) hochgeladen oder geändert wird |
| **Entwickeln eines serverlosen Workflows** | Verketten einer Reihe von Funktionen mit [Durable Functions](./durable/durable-functions-overview.md) |
| **Reagieren auf Datenbankänderungen** | Ausführen von benutzerdefinierter Logik beim Erstellen oder Aktualisieren eines Dokuments in [Cosmos DB](./functions-bindings-cosmosdb-v2.md) |
| **Ausführen von geplanten Aufgaben** | Ausführen von Code zu [festen Zeiten](./functions-bindings-timer.md) |
| **Erstellen zuverlässiger Systeme für Nachrichtenwarteschlangen** | Verarbeiten von Nachrichtenwarteschlangen mit [Queue Storage](./functions-bindings-storage-queue.md), [Service Bus](./functions-bindings-service-bus.md) oder [Event Hubs](./functions-bindings-event-hubs.md) |
| **Analysieren von IoT-Datenströmen** | Sammeln und Verarbeiten der [Daten von IoT-Geräten](./functions-bindings-event-iot.md) |
| **Verarbeiten von Daten in Echtzeit** | Verwenden von [Functions und Signal R](./functions-bindings-signalr-service.md) zum schnellen Reagieren auf Datenvorgänge |

Beim Entwickeln Ihrer Funktionen stehen Ihnen die folgenden Optionen und Ressourcen zur Verfügung:

- **Nutzung der bevorzugten Sprache**: Schreiben Sie Funktionen in [C#, Java, JavaScript, PowerShell oder Python](./supported-languages.md), oder verwenden Sie einen [benutzerdefinierten Handler](./functions-custom-handlers.md), um nahezu alle anderen Sprachen zu verwenden.

- **Automatisierung der Bereitstellung**: Von einem toolbasierten Ansatz bis zur Verwendung von externen Pipelines sind [viele verschiedene Bereitstellungsoptionen](./functions-deployment-technologies.md) verfügbar.

- **Problembehandlung für eine Funktion**: Verwenden Sie [Überwachungstools](./functions-monitoring.md) und [Teststrategien](./functions-test-a-function.md), um Einblicke in Ihre Apps zu erhalten.

- **Flexible Preisoptionen**: Beim [Verbrauchstarif](./pricing.md) zahlen Sie nur, während Ihre Funktionen ausgeführt werden, und die Tarife [Premium](./pricing.md) und [App Service](./pricing.md) verfügen über Features für besondere Anforderungen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit Lektionen, Beispielen und interaktiven Tutorials](./functions-get-started.md)