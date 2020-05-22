---
title: Azure-Funktion als Ereignishandler für Azure Event Grid-Ereignisse
description: Beschreibt, wie Sie Azure-Funktionen als Ereignishandler für Event Grid-Ereignisse verwenden können.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 3ff3c0013cb7a373461b997b9922612763461b8d
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83595649"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Azure-Funktion als Ereignishandler für Event Grid-Ereignisse

Ein Ereignishandler ist der Ort, an den das Ereignis gesendet wird. Der Handler ergreift zur Verarbeitung des Ereignisses eine Maßnahme. Mehrere Azure-Dienste werden automatisch für die Behandlung von Ereignissen konfiguriert. **Azure Functions** ist einer dieser Dienste. 

Verwenden Sie **Azure Functions** in einer serverlosen Architektur, um auf Ereignisse aus Event Grid zu reagieren. Wenn eine Azure-Funktion als Handler eingesetzt wird, verwenden Sie den Event Grid-Trigger anstelle des generischen HTTP-Triggers. Event Grid überprüft Event Grid-Trigger automatisch. Bei generischen HTTP-Triggern müssen Sie die [Überprüfungsantwort](webhook-event-delivery.md) selbst implementieren.

Unter [Event Grid-Trigger für Azure Functions](../azure-functions/functions-bindings-event-grid.md) finden Sie eine Übersicht über die Verwendung des Event Grid-Triggers in Funktionen.

## <a name="tutorials"></a>Tutorials

|Titel  |BESCHREIBUNG  |
|---------|---------|
| [Schnellstart: Verarbeiten von Ereignissen mit Funktionen](custom-event-to-function.md) | Beschreibt das Senden eines benutzerdefinierten Ereignisses an eine Funktion, damit dieses verarbeitet wird |
| [Tutorial: Automatisieren der Größenänderung von hochgeladenen Images per Event Grid](resize-images-on-storage-blob-upload-event.md) | Benutzer laden Bilder über eine Web-App in ein Speicherkonto hoch. Wenn ein Speicherblob erstellt wird, sendet Event Grid ein Ereignis an die Funktions-App, die daraufhin die Größe des hochgeladenen Bilds anpasst. |
| [Tutorial: Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md) | Wenn Event Hubs eine Capture-Datei erstellt, sendet Event Grid ein Ereignis an eine Funktions-App. Die App ruft die Capture-Datei ab und migriert Daten zu einem Data Warehouse. |
| [Tutorial: Beispiele für die Integration von Azure Service Bus in Azure Event Grid](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid sendet Nachrichten von einem Service Bus-Thema an eine Funktions-App und an eine Logik-App. |


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der unterstützten Ereignishandler finden Sie im Artikel zu [Ereignishandlern](event-handlers.md). 
