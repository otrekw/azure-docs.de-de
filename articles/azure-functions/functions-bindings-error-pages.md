---
title: Azure Functions – Anleitung zu Fehlerbehandlung
description: Erfahren Sie, wie Sie Fehler in Azure Functions mit Links zu bestimmten Bindungsfehlern behandeln können.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: befdb4a8cceaef18961c1e9297e23ed5d405ff50
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77586188"
---
# <a name="azure-functions-error-handling"></a>Azure Functions – Fehlerbehandlung

Das Behandeln von Fehlern in Azure Functions ist wichtig, um verlorene Daten und verpasste Ereignisse zu vermeiden sowie um die Integrität Ihrer Anwendung zu überwachen.

Dieser Artikel beschreibt allgemeine Strategien für die Fehlerbehandlung zusammen mit Links zu bindungsspezifischen Fehlern.

## <a name="handling-errors"></a>Behandlung von Fehlern

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Bindungsfehlercodes

Bei der Integration in Azure-Dienste können Fehler auftreten, deren Ursache in den APIs der zugrunde liegenden Dienste liegt. Informationen zu bindungsspezifischen Fehlern finden Sie im Abschnitt **Ausnahmen und Rückgabecodes** der folgenden Artikel:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs-output.md#exceptions-and-return-codes)

+ [IoT Hubs](functions-bindings-event-iot-output.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue-output.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus-output.md#exceptions-and-return-codes)

+ [Tabellenspeicherung](functions-bindings-storage-table.md#exceptions-and-return-codes)
