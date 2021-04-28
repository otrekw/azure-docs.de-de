---
title: include file
description: include file
services: service-bus-messaging
author: clemensv
ms.service: service-bus-messaging
ms.topic: include
ms.date: 04/08/2021
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: 08fccf366321b075542f36b86c9bf22d5d877167
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107304415"
---
Die Option AMQP-over-WebSockets-Protokoll wird über TCP-Port 443 wie die HTTP-/REST-API ausgeführt, ist aber ansonsten funktionell identisch mit einfachem AMQP. Diese Option weist eine höhere anfängliche Verbindungslatenz auf, weil zusätzliche Handshakeroundtrips auftreten und der Mehraufwand als Kompromisse bei der gemeinsamen Verwendung des HTTPS-Ports geringfügig größer ist. Wenn dieser Modus ausgewählt ist, ist TCP-Port 443 für die Kommunikation ausreichend. Mit den folgenden Optionen können Sie den AMQP-WebSockets-Modus auswählen. 

| Sprache | Option   |
| -------- | ----- |
| .NET (Azure.Messaging.ServiceBus)    | Erstellen Sie [ServiceBusClient](/dotnet/api/azure.messaging.servicebus.servicebusclient.-ctor) mithilfe eines Konstruktors, der [ServiceBusClientOptions](/dotnet/api/azure.messaging.servicebus.servicebusclientoptions) als Parameter akzeptiert. Legen Sie [ServiceBusClientOptions.TransportType](/dotnet/api/azure.messaging.servicebus.servicebusclientoptions.transporttype) auf [ServiceBusTransportType.AmqpWebSockets](/dotnet/api/azure.messaging.servicebus.servicebustransporttype) fest. |
| .NET (Microsoft.Azure.ServiceBus)    | Verwenden Sie beim Erstellen von Clientobjekten Konstruktoren, die [TransportType](/dotnet/api/microsoft.azure.servicebus.transporttype), [ServiceBusConnection](/dotnet/api/microsoft.azure.servicebus.servicebusconnection) oder [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder) als Parameter verwenden. <p>Legen Sie für das Konstrukt, das `transportType` als Parameter verwendet, den Parameter auf [TransportType.AmqpWebSockets](/dotnet/api/microsoft.azure.servicebus.transporttype) fest.</p> <p>Legen Sie für den Konstruktor, der `ServiceBusConnection` als Parameter verwendet, [ServiceBusConnection.TransportType](/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype) auf [TransportType.AmqpWebSockets](/dotnet/api/microsoft.azure.servicebus.transporttype) fest.</p> <p>Wenn Sie `ServiceBusConnectionStringBuilder` verwenden, nutzen Sie Konstruktoren, mit denen Sie die angeben `transportType` können.</p> |
| Java (com.azure.messaging.servicebus)     | Legen Sie beim Erstellen von Clients [ServiceBusClientBuilder.transportType](/java/api/com.azure.messaging.servicebus.servicebusclientbuilder.transporttype) auf [AmqpTransportType.AMQP.AMQP_WEB_SOCKETS](/java/api/com.azure.core.amqp.amqptransporttype) fest. |
| Java (com.microsoft.azure.servicebus)    | Legen Sie beim Erstellen von Clients `transportType` in [com.microsoft.azure.servicebus.ClientSettings](/java/api/com.microsoft.azure.servicebus.clientsettings.clientsettings#com_microsoft_azure_servicebus_ClientSettings_ClientSettings_com_microsoft_azure_servicebus_security_TokenProvider_com_microsoft_azure_servicebus_primitives_RetryPolicy_java_time_Duration_com_microsoft_azure_servicebus_primitives_TransportType_) auf [com.microsoft.azure.servicebus.primitives.TransportType.AMQP_WEB_SOCKETS](/java/api/com.microsoft.azure.servicebus.primitives.transporttype) fest. |
| JavaScript  | Verwenden Sie beim Erstellen Service Bus-Clientobjekten die Eigenschaft `webSocketOptions` in [ServiceBusClientOptions](/javascript/api/@azure/service-bus/servicebusclientoptions). |
| Python | Legen Sie beim Erstellen von Service Bus-Clients [ServiceBusClient.transport_type](/python/api/azure-servicebus/azure.servicebus.servicebusclient) auf [TransportType.AmqpOverWebSocket](/python/api/azure-servicebus/azure.servicebus.transporttype) fest. |

