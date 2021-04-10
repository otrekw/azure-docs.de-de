---
title: Azure Event Hubs-Trigger für Azure Functions
description: Hier erfahren Sie, wie Sie Azure Event Hubs-Trigger in Azure Functions verwenden.
author: craigshoemaker
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: c2b8302e64f7dcc657fd20ed5d918ed6816d750d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102608909"
---
# <a name="azure-event-hubs-trigger-for-azure-functions"></a>Azure Event Hubs-Trigger für Azure Functions

In diesem Artikel erfahren Sie, wie Sie [Azure Event Hubs](../event-hubs/event-hubs-about.md)-Trigger für Azure Functions verwenden. Azure Functions unterstützt Trigger- und [Ausgabebindungen](functions-bindings-event-hubs-output.md) für Event Hubs.

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-event-hubs.md).

[!INCLUDE [functions-bindings-event-hubs-trigger](../../includes/functions-bindings-event-hubs-trigger.md)]

## <a name="hostjson-settings"></a>Einstellungen für „host.json“

Die Datei [host.json](functions-host-json.md#eventhub) enthält Einstellungen, die das Verhalten des Event Hub-Triggers steuern. Informationen zu verfügbaren Einstellungen finden Sie im Abschnitt [Einstellungen für „host.json“](functions-bindings-event-hubs.md#hostjson-settings).

## <a name="next-steps"></a>Nächste Schritte

- [Schreiben von Ereignissen in einen Ereignisdatenstrom (Ausgabebindungen)](./functions-bindings-event-hubs-output.md)
