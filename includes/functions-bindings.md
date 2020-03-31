---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.custom: fasttrack-edit
ms.openlocfilehash: 819eed1eb4e32469bf0572a6548f5a2a44091ef0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77279619"
---
Die folgende Tabelle zeigt die Bindungen, die in den Hauptversionen der Azure Functions-Runtime unterstützt werden:


| type | 1.x | 2.x und höher<sup>1</sup> | Trigger | Eingabe | Output |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|
| [Event Grid](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| |✔|
| [Event Hubs](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP und Webhooks](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [IoT Hub](../articles/azure-functions/functions-bindings-event-iot.md)             |✔|✔|✔| |✔|
| [Microsoft Graph<br/>Excel-Tabellen](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔| |✔|✔|
| [Microsoft Graph<br/>OneDrive-Dateien](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔| |✔|✔|
| [Microsoft Graph<br/>Outlook-E-Mail](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔| | |✔|
| [Microsoft Graph<br/>Ereignisse](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔|✔|✔|✔|
| [Microsoft Graph<br/>Authentifizierungstoken](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔| |✔| |
| [Mobile Apps](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [Notification Hubs](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [SignalR](../articles/azure-functions/functions-bindings-signalr-service.md)             | |✔| |✔|✔|
| [Tabellenspeicherung](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [Zeitgeber](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |
| [Twilio](../articles/azure-functions/functions-bindings-twilio.md)                       |✔|✔| | |✔|

<sup>1</sup> Ab Version 2.x der Runtime müssen alle Bindungen mit Ausnahme von HTTP und Timer registriert werden. Siehe [Registrieren von Bindungserweiterungen](../articles/azure-functions/functions-bindings-register.md).
