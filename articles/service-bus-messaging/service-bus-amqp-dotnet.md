---
title: Verwenden der .NET Framework-Legacybibliothek „WindowsAzure.ServiceBus“ mit AMQP 1.0 | Microsoft-Dokumentation
description: In diesem Artikel wird die Verwendung der .NET Framework-Legacybibliothek „WindowsAzure.ServiceBus“ mit AMQP (Advanced Messaging Queuing Protocol) beschrieben.
ms.topic: article
ms.date: 04/30/2021
ms.openlocfilehash: 160da6a770e58e9a76e966f018d25dc9bc8a8c76
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108770063"
---
# <a name="use-legacy-windowsazureservicebus-net-framework-library-with-amqp-10"></a>Verwenden der .NET Framework-Legacybibliothek „WindowsAzure.ServiceBus“ mit AMQP 1.0

> [!NOTE]
> Dieser Artikel richtet sich an vorhandene Benutzer des WindowsAzure.ServiceBus-Pakets, die zur Verwendung von AMQP innerhalb desselben Pakets wechseln möchten. Dieses Paket wird zwar weiterhin kritische Fehlerbehebungen erhalten, es wird jedoch dringend empfohlen, ein Upgrade auf das neue [Azure.Messaging.ServiceBus-Paket](https://www.nuget.org/packages/Azure.Messaging.ServiceBus) durchzuführen, das ab November 2020 verfügbar ist und AMQP standardmäßig unterstützt.

Das WindowsAzure.ServiceBus-Paket kommuniziert standardmäßig mit dem Service Bus-Dienst mithilfe eines dedizierten SOAP-basierten Protokolls namens Service Bus Messaging Protocol (SBMP). In Version 2.1 wurde die Unterstützung für AMQP 1.0 hinzugefügt, das anstelle des Standardprotokolls verwendet werden sollte.

Wenn Sie anstatt des Standardprotokolls AMQP 1.0 verwenden möchten, ist eine explizite Konfiguration der Service Bus-Verbindungszeichenfolge oder der Clientkonstruktoren über die Option [TransportType](/dotnet/api/microsoft.servicebus.messaging.transporttype) erforderlich. Abgesehen von dieser Änderung bleibt der Anwendungscode bei Verwendung von AMQP 1.0 unverändert.

Einige wenige API-Features werden bei Verwendung von AMQP nicht unterstützt. Diese nicht unterstützten Features finden Sie im Abschnitt [Verhaltensunterschiede](#behavioral-differences). Darüber hinaus haben einige der erweiterten Konfigurationseinstellungen eine unterschiedliche Bedeutung, wenn AMQP zum Einsatz kommt.

## <a name="configure-connection-string-to-use-amqp-10"></a>Konfigurieren einer Verbindungszeichenfolge zur Verwendung von AMQP 1.0

Fügen Sie die Verbindungszeichenfolge mit `;TransportType=Amqp` an, um den Client anzuweisen, mithilfe von AMQP 1.0 eine Verbindung mit der Service Bus-Instanz herzustellen.
Beispiel: 

`Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp`

`namespace` und `SAS key` werden aus dem [Azure-Portal][Azure portal] abgerufen, wenn Sie einen Service Bus-Namespace erstellen. Weitere Informationen finden Sie unter [Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals][Create a Service Bus namespace using the Azure portal].

### <a name="amqp-over-websockets"></a>AMQP über WebSockets
Wenn Sie AMQP über WebSockets verwenden möchten, legen Sie `TransportType` in der Verbindungszeichenfolge auf `AmqpWebSockets` fest. Beispiel: `Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=AmqpWebSockets`. 

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

Bei der Verwendung von AMQP gibt es im Vergleich zum Standardprotokoll einige kleine Unterschiede im Verhalten der WindowsAzure.ServiceBus-API:

* Die [OperationTimeout][OperationTimeout]-Eigenschaft wird ignoriert.
* `MessageReceiver.Receive(TimeSpan.Zero)` wird als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))` implementiert.
* Das Abschließen von Nachrichten durch Sperrtoken kann nur von den Nachrichtenempfängern durchgeführt werden, die die Nachrichten ursprünglich erhalten haben.

## <a name="control-amqp-protocol-settings"></a>Steuern von AMQP-Protokolleinstellungen

Die [.NET-APIs](/dotnet/api/) stellen mehrere Einstellungen zum Steuern des Verhaltens des AMQP-Protokolls zur Verfügung:

* **[MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver.prefetchcount#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount)** : Steuert die anfänglichen Rechte, die auf einen Link angewendet werden. Die Standardeinstellung ist 0.
* **[MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.maxframesize#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_MaxFrameSize)** : Steuert die maximale AMQP-Framegröße, die während der Verhandlung zum Herstellen der Verbindung angeboten wird. Der Standardwert ist 65.536 Bytes.
* **[MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.batchflushinterval#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_BatchFlushInterval)** : Wenn eine Übertragung in Batches durchgeführt werden kann, bestimmt dieser Wert die maximale Verzögerung bei der Verteilung von Sendungen. Wird von Absendern/Empfängern standardmäßig vererbt. Einzelne Sender/Empfänger können die Standardeinstellung (20 Millisekunden) überschreiben.
* **[MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity](/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.usesslstreamsecurity#Microsoft_ServiceBus_Messaging_Amqp_AmqpTransportSettings_UseSslStreamSecurity)** : Steuert, ob AMQP-Verbindungen über eine TLS-Verbindung hergestellt werden. Der Standardwert ist **true**.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Nutzen Sie die folgenden Links:

* [Übersicht über Service Bus AMQP]
* [AMQP 1.0 – Protokollleitfaden]

[Create a Service Bus namespace using the Azure portal]: service-bus-create-namespace-portal.md
[DataContractSerializer]: /dotnet/api/system.runtime.serialization.datacontractserializer
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory.acceptmessagesession#Microsoft_ServiceBus_Messaging_MessagingFactory_AcceptMessageSession
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[Azure portal]: https://portal.azure.com
[Übersicht über Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0 – Protokollleitfaden]: service-bus-amqp-protocol-guide.md
