---
title: include file
description: include file
services: service-bus-messaging
author: clemensv
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/24/2020
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: ca483d0b71bde945a7e46da785dd6a76b3a8f177
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98693397"
---
Die Option AMQP-over-WebSockets-Protokoll wird über TCP-Port 443 wie die HTTP-/REST-API ausgeführt, ist aber ansonsten funktionell identisch mit einfachem AMQP. Diese Option weist eine etwas höhere anfängliche Verbindungslatenz auf, weil zusätzliche Handshakeroundtrips auftreten und der Mehraufwand als Kompromisse bei der gemeinsamen Verwendung des HTTPS-Ports geringfügig größer ist. Wenn dieser Modus ausgewählt ist, ist TCP-Port 443 für die Kommunikation ausreichend. Mit den folgenden Optionen können Sie den einfachen AMQP- oder den AMQP-WebSockets-Modus auswählen:

| Sprache | Option   |
| -------- | ----- |
| .NET     | [ServiceBusConnection.TransportType](/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype)-Eigenschaft mit [TransportType.Amqp](/dotnet/api/microsoft.azure.servicebus.transporttype) oder [TransportType.AmqpWebSockets](/dotnet/api/microsoft.azure.servicebus.transporttype) |
| Java     | [com.microsoft.azure.servicebus.ClientSettings](/java/api/com.microsoft.azure.servicebus.clientsettings.clientsettings) mit [com.microsoft.azure.servicebus.primitives.TransportType.AMQP](/java/api/com.microsoft.azure.servicebus.primitives.transporttype) oder [com.microsoft.azure.servicebus.primitives.TransportType.AMQP_WEB_SOCKETS](/java/api/com.microsoft.azure.servicebus.primitives.transporttype) |
| Node  | [ServiceBusClientOptions](/javascript/api/@azure/service-bus/servicebusclientoptions) verfügt über ein `webSocket`-Konstruktorargument. |
| Python | [ServiceBusClient.transport_type](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.ServiceBusClient) mit [TransportType.Amqp](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) oder [TransportType.AmqpOverWebSocket](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) |