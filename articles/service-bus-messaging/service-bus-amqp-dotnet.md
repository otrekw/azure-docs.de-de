---
title: Azure Service Bus mit .NET und AMQP 1.0 | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Azure Service Bus über eine .NET-Anwendung mit AMQP (Advanced Message Queuing Protocol) verwenden.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 0d6d7d01a56d2e7068f9c4ccb8ec505914a31ecf
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2021
ms.locfileid: "98233932"
---
# <a name="use-service-bus-from-net-with-amqp-10"></a>Verwenden von Service Bus aus .NET mit AMQP 1.0

Unterstützung für AMQP 1.0 ist im Service Bus-Paket ab Version 2.1 verfügbar. Sie können sicherstellen, dass Sie über die aktuelle Version verfügen, indem Sie die Service Bus-Komponenten von[NuGet][NuGet] herunterladen.

> [!NOTE]
> Sie können das Advance Message Queueing Protocol (AMQP) oder das Service Bus Messaging Protocol (SBMP) mit der .NET-Bibliothek für Service Bus verwenden. AMQP ist das Standardprotokoll, das von der .NET-Bibliothek verwendet wird. Es wird empfohlen, das AMQP-Protokoll (Standardeinstellung) zu verwenden und es nicht außer Kraft zu setzen. 

## <a name="configure-net-applications-to-use-amqp-10"></a>Konfigurieren von .NET-Anwendungen für das Verwenden von AMQP 1.0

Mithilfe des AMQP-Protokolls kommuniziert die .NET-Clientbibliothek von Service Bus standardmäßig mit dem Service Bus-Dienst. AMQP kann auch explizit als Transporttyp angegeben werden, wie im folgenden Abschnitt gezeigt. 

In der aktuellen Version gibt es ein paar API-Features, die bei Verwendung von AMQP nicht unterstützt werden. Diese nicht unterstützten Features finden Sie im Abschnitt [Verhaltensunterschiede](#behavioral-differences). Darüber hinaus haben einige der erweiterten Konfigurationseinstellungen eine unterschiedliche Bedeutung, wenn AMQP zum Einsatz kommt.

### <a name="configuration-using-appconfig"></a>Konfiguration mithilfe von "App.config"

Es ist empfehlenswert, für Anwendungen die Konfigurationsdatei „App.config“ zu verwenden, um die Einstellungen zu speichern. Bei Service Bus-Anwendungen können Sie die Datei „App.config“ verwenden, um die Service Bus-Verbindungszeichenfolge zu speichern. Es folgt ein Beispiel der Datei "App.config":

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
    </appSettings>
</configuration>
```

Beim Wert der Einstellung `Microsoft.ServiceBus.ConnectionString` handelt es sich um die Service Bus-Verbindungszeichenfolge, die zum Konfigurieren der Verbindung mit Service Bus verwendet wird. Das Format sieht folgendermaßen aus:

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

`namespace` und `SAS key` werden aus dem [Azure-Portal][Azure portal] abgerufen, wenn Sie einen Service Bus-Namespace erstellen. Weitere Informationen finden Sie unter [Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals][Create a Service Bus namespace using the Azure portal].

Wenn Sie AMQP verwenden, fügen Sie `;TransportType=Amqp` an die Verbindungszeichenfolge an. Diese Notation weist die Clientbibliothek an, die Verbindung mit Service Bus über AMQP 1.0 herzustellen.

### <a name="amqp-over-websockets"></a>AMQP über WebSockets
Wenn Sie AMQP über WebSockets verwenden möchten, legen Sie `TransportType` in der Verbindungszeichenfolge auf `AmqpWebSockets` fest. Beispiel: `Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=AmqpWebSockets`. 

Wenn Sie die .NET-Bibliothek „Microsoft.Azure.ServiceBus“ verwenden, legen Sie [ServiceBusConnection.TransportType](/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype) in der [TransportType-Enumeration](/dotnet/api/microsoft.azure.servicebus.transporttype) auf AmqpWebSockets fest.

Wenn Sie die .NET-Bibliothek „Azure.Messaging.ServiceBus“ verwenden, legen Sie [ServiceBusClient.TransportType](/dotnet/api/azure.messaging.servicebus.servicebusclient.transporttype) in der [ServiceBusTransportType-Enumeration](/dotnet/api/azure.messaging.servicebus.servicebustransporttype) auf AmqpWebSockets fest.


## <a name="message-serialization"></a>Nachrichtenserialisierung

Bei Verwendung des Standardprotokolls sieht das Standardserialisierungsverhalten der .NET-Clientbibliothek die Verwendung des [DataContractSerializer][DataContractSerializer]-Typs zum Serialisieren einer [BrokeredMessage][BrokeredMessage]-Instanz für die Übertragung zwischen der Clientbibliothek und dem Service Bus-Dienst vor. Bei Verwendung des AMQP-Transportmodus verwendet die Clientbibliothek das AMQP-Typsystem für die Serialisierung der [im Broker gespeicherten Nachricht][BrokeredMessage] in eine AMQP-Nachricht. Diese Serialisierung ermöglicht, dass die Nachricht von einer empfangenden Anwendung empfangen und interpretiert wird, die möglicherweise auf einer anderen Plattform ausgeführt wird, z. B. einer Java-Anwendung, die die JMS-API für den Zugriff auf Service Bus verwendet.

Wenn Sie eine [BrokeredMessage][BrokeredMessage]-Instanz erstellen, können Sie ein .NET-Objekt als Parameter für den Konstruktor bereitstellen, der als Text der Nachricht dient. Für Objekte, die primitiven AMQP-Typen zugeordnet werden können, wird der Text in AMQP-Datentypen serialisiert. Wenn das Objekt nicht direkt einem primitiven AMQP-Typ, d.h. einem von der Anwendung definierten benutzerdefinierten Typ, zugeordnet werden kann, wird das Objekt mithilfe von [DataContractSerializer][DataContractSerializer] serialisiert, und die serialisierten Bytes werden in einer AMQP-Datennachricht gesendet.

Um die Interoperabilität mit Nicht-.NET-Clients zu erleichtern, verwenden Sie nur .NET-Typen, die für den Text der Nachricht direkt in AMQP-Typen serialisiert werden können. Die folgende Tabelle enthält diese Typen und die entsprechende Zuordnung zum AMQP-Typsystem.

| .NET Body-Objekttyp | Zugeordneter AMQP-Typ | AMQP Body-Abschnittstyp |
| --- | --- | --- |
| bool |boolean |AMQP Value |
| byte |ubyte |AMQP Value |
| ushort |ushort |AMQP Value |
| uint |uint |AMQP Value |
| ulong |ulong |AMQP Value |
| sbyte |byte |AMQP Value |
| short |short |AMQP Value |
| INT |INT |AMQP Value |
| long |long |AMQP Value |
| float |float |AMQP Value |
| double |double |AMQP Value |
| Decimal |decimal128 |AMQP Value |
| char |char |AMQP Value |
| Datetime |timestamp |AMQP Value |
| Guid |uuid |AMQP Value |
| byte[] |BINARY |AMQP Value |
| Zeichenfolge |Zeichenfolge |AMQP Value |
| System.Collections.IList |list |AMQP Value: Diese Auflistung kann nur Elemente enthalten, die in dieser Tabelle definiert sind. |
| System.Array |array |AMQP Value: Diese Auflistung kann nur Elemente enthalten, die in dieser Tabelle definiert sind. |
| System.Collections.IDictionary |map |AMQP Value: Diese Auflistung kann nur Elemente enthalten, die in dieser Tabelle definiert sind. Hinweis: nur "String"-Schlüssel werden unterstützt. |
| Uri |Beschriebene Zeichenfolge (siehe die folgende Tabelle) |AMQP Value |
| DateTimeOffset |Lange Beschreibung (siehe die folgende Tabelle) |AMQP Value |
| TimeSpan |Lange Beschreibung (siehe die folgende Tabelle) |AMQP Value |
| STREAM |BINARY |AMQP Data (können mehrere sein). Die "Data"-Abschnitte enthalten die rohen Bytes, die aus dem "Stream"-Objekt gelesen. |
| Other Object |BINARY |AMQP Data (können mehrere sein). Enthält die serialisierten Binärdaten des Objekts, das "DataContractSerializer" oder einen von der Anwendung bereitgestellten Serialisierer verwendet. |

| .NET-Typ | Zugeordneter beschriebener AMQP-Typ | Notizen |
| --- | --- | --- |
| Uri |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| DateTimeOffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>` |TimeSpan.Ticks |

## <a name="behavioral-differences"></a>Verhaltensunterschiede

Es gibt bei Verwenden von AMQP im Vergleich zum Standardprotokoll einige kleine Unterschiede beim Verhalten der .NET-API von Service Bus:

* Die [OperationTimeout][OperationTimeout]-Eigenschaft wird ignoriert.
* `MessageReceiver.Receive(TimeSpan.Zero)` wird als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))` implementiert.
* Das Abschließen von Nachrichten durch Sperrtoken kann nur von den Nachrichtenempfängern durchgeführt werden, die die Nachrichten ursprünglich erhalten haben.

## <a name="control-amqp-protocol-settings"></a>Steuern von AMQP-Protokolleinstellungen

Die [.NET-APIs](/dotnet/api/) stellen mehrere Einstellungen zum Steuern des Verhaltens des AMQP-Protokolls zur Verfügung:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)** : Steuert die anfänglichen Rechte, die auf einen Link angewendet werden. Die Standardeinstellung ist 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)** : Steuert die maximale AMQP-Framegröße, die während der Verhandlung zum Herstellen der Verbindung angeboten wird. Der Standardwert ist 65.536 Bytes.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)** : Wenn eine Übertragung in Batches durchgeführt werden kann, bestimmt dieser Wert die maximale Verzögerung bei der Verteilung von Sendungen. Wird von Absendern/Empfängern standardmäßig vererbt. Einzelne Sender/Empfänger können die Standardeinstellung (20 Millisekunden) überschreiben.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)** : Steuert, ob AMQP-Verbindungen über eine TLS-Verbindung hergestellt werden. Der Standardwert ist **true**.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Nutzen Sie die folgenden Links:

* [Übersicht über Service Bus AMQP]
* [AMQP 1.0 – Protokollleitfaden]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: /dotnet/api/system.runtime.serialization.datacontractserializer?view=netcore-3.1
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azureservicebus-4.0.0
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[NuGet]: https://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Übersicht über Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0 – Protokollleitfaden]: service-bus-amqp-protocol-guide.md