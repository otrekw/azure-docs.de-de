---
title: Azure IoT Hub-Bindungen für Azure Functions
description: Erfahren Sie, wie IoT Hub-Trigger und -Bindungen in Azure Functions verwendet werden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 1c25543b16c3486a8f6a445427346382faaaa09a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "77586127"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>Azure IoT Hub-Bindungen für Azure Functions

Diese Gruppe von Artikeln erläutert das Arbeiten mit Azure Functions-Bindungen für IoT Hub. Die IoT Hub-Unterstützung basiert auf der [Azure Event Hubs-Bindung](functions-bindings-event-hubs.md).

> [!IMPORTANT]
> Die folgenden Codebeispiele verwenden zwar die Event Hub-API, aber die Syntax gilt auch für IoT Hub-Funktionen.

| Aktion | type |
|--------|------|
| Reagieren auf Ereignisse, die an einen IoT Hub-Ereignisdatenstrom gesendet werden. | [Trigger](./functions-bindings-event-iot-trigger.md) |
| Schreiben von Ereignissen in einen IoT-Ereignisdatenstrom | [Ausgabebindung](./functions-bindings-event-iot-output.md) |

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Reagieren auf Ereignisse, die an einen Event Hub-Ereignisdatenstrom gesendet werden (Trigger)](./functions-bindings-event-iot-trigger.md)
- [Schreiben von Ereignissen in einen Ereignisdatenstrom (Ausgabebindungen)](./functions-bindings-event-iot-output.md)
