---
title: Azure IoT Hub-Trigger für Azure Functions
description: Erfahren Sie, wie Sie auf Ereignisse reagieren, die in Azure Functions an einen IoT Hub-Ereignisdatenstrom gesendet werden.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 5c9309834b407ee56d29e38afd965ac947fc8a4f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102612285"
---
# <a name="azure-iot-hub-trigger-for-azure-functions"></a>Azure IoT Hub-Trigger für Azure Functions

Dieser Artikel erläutert das Arbeiten mit Azure Functions-Bindungen für IoT Hub. Die IoT Hub-Unterstützung basiert auf der [Azure Event Hubs-Bindung](functions-bindings-event-hubs.md).

Informationen zu Setup- und Konfigurationsdetails finden Sie in der [Übersicht](functions-bindings-event-iot.md).

> [!IMPORTANT]
> Die folgenden Codebeispiele verwenden zwar die Event Hub-API, aber die Syntax gilt auch für IoT Hub-Funktionen.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs-trigger.md)]

## <a name="hostjson-properties"></a>Eigenschaften von „host.json“

Die Datei [host.json](functions-host-json.md#eventhub) enthält Einstellungen, die das Verhalten des Event Hub-Triggers steuern. Informationen zu verfügbaren Einstellungen finden Sie im Abschnitt [Einstellungen für „host.json“](functions-bindings-event-iot.md#hostjson-settings).

## <a name="next-steps"></a>Nächste Schritte

- [Schreiben von Ereignissen in einen Ereignisdatenstrom (Ausgabebindungen)](./functions-bindings-event-iot-output.md)
