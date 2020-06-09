---
title: Azure Service Bus-Nachrichtensitzungen | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Nachrichtensitzungen zum Ermöglichen der gemeinsamen und geordneten Verarbeitung unbegrenzter Sequenzen verwandter Nachrichten verwendet werden.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/20/2020
ms.author: aschhab
ms.openlocfilehash: 9cedf3678fc73b004c142380b4ba69c10ca72ebf
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726994"
---
# <a name="message-sessions"></a>Nachrichtensitzungen
Microsoft Azure Service Bus-Sitzungen ermöglichen die gemeinsame und geordnete Verarbeitung unbegrenzter Sequenzen zusammengehöriger Nachrichten. Sitzungen können mit **FIFO**-Mustern (First in, First Out) und **Anforderung/Antwort**-Mustern verwendet werden. In diesem Artikel wird gezeigt, wie Sie diese Muster mithilfe von Sitzungen implementieren, wenn Sie Service Bus verwenden. 

> [!NOTE]
> Der Basic-Tarif von Service Bus unterstützt keine Sitzungen. Die Standard- und Premium-Tarife unterstützen Sitzungen. Informationen zu den Unterschieden zwischen diesen Tarifen finden Sie unter [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="first-in-first-out-fifo-pattern"></a>FIFO-Muster (First in, First Out)
Um eine FIFO-Garantie in Service Bus umzusetzen, müssen Sie Sitzungen verwenden. Service Bus macht keine Vorgaben zur Art der Beziehung zwischen den Nachrichten und definiert auch kein bestimmtes Modell zur Bestimmung, wo eine Nachrichtensequenz beginnt oder endet.

Jeder Absender kann eine Sitzung erstellen, wenn er Nachrichten an ein Thema oder eine Warteschlange sendet, indem er die Eigenschaft [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) auf einen von der Anwendung definierten Bezeichner festlegt, der für die Sitzung eindeutig ist. Auf der AMQP 1.0-Protokollebene ist dieser Wert der *group-id*-Eigenschaft zugeordnet.

Bei sitzungsabhängigen Warteschlangen oder Abonnements entstehen Sitzungen, wenn mindestens eine Nachricht mit der [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) der Sitzung vorhanden ist. Sobald eine Sitzung vorhanden ist, gibt es keine definierte Uhrzeit oder API für den Zeitpunkt, an dem die Sitzung abläuft oder erlischt. Theoretisch kann eine Nachricht für eine Sitzung heute und die nächste Nachricht in einem Jahr empfangen werden. Wenn die **SessionId** übereinstimmt, ist die Sitzung aus Service Bus-Sicht identisch.

Typischerweise verfügt eine Anwendung jedoch über klare Informationen dazu, wo eine Reihe zusammengehöriger Nachrichten beginnt und endet. Service Bus legt allerdings keine spezifischen Regeln fest.

Ein Beispiel dafür, wie eine Sequenz für die Übertragung einer Datei beschrieben werden kann, ist das Festlegen der **Label**-Eigenschaft für die erste Nachricht auf **start**, für die Zwischennachrichten auf **content** und für die letzte Nachricht auf **end**. Die relative Position der content-Nachrichten kann als *SequenceNumber*-Delta der aktuellen Nachricht von der *SequenceNumber* der **Start**-Nachricht berechnet werden.

Das Sitzungsfeature in Service Bus ermöglicht einen bestimmten Empfangsvorgang in Form von [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) in den C#- und Java-APIs. Sie aktivieren das Feature, indem Sie die [requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values)-Eigenschaft über Azure Resource Manager für die Warteschlange oder das Abonnement festlegen oder das Flag im Portal festlegen. Dies ist erforderlich, bevor Sie versuchen, die entsprechenden API-Vorgänge zu verwenden.

Im Portal legen Sie das Flag mit dem folgenden Kontrollkästchen fest:

![][2]

> [!NOTE]
> Wenn Sitzungen für eine Warteschlange oder ein Abonnement aktiviert sind, können die Clientanwendungen ***keine*** regulären Nachrichten mehr senden/empfangen. Alle Nachrichten müssen im Rahmen einer Sitzung (durch Festlegen der Sitzungs-ID) gesendet und durch Empfangen der Sitzung empfangen werden.

Die APIs für Sitzungen sind für Warteschlangen- und Abonnementclients vorhanden. Es gibt ein verbindliches Modell, das steuert, wann Sitzungen und Nachrichten empfangen werden, und ein auf einem Handler basierendes Modell, ähnlich *OnMessage*, das die Komplexität der Verwaltung der Empfangsschleife verbirgt.

### <a name="session-features"></a>Sitzungsfeatures

Sitzungen ermöglichen das gleichzeitige Demultiplexing von verschachtelten Nachrichtenströmen unter Beibehaltung und Gewährleistung der geordneten Zustellung.

![][1]

Ein [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession)-Empfänger wird vom Client erstellt, der eine Sitzung annimmt. In C# ruft der Client [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) oder [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) auf. Im reaktiven Rückrufmodell registriert er einen Sitzungshandler.

Wenn das Objekt [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) akzeptiert wird und solange es von einem Client gehalten wird, hält dieser Client eine exklusive Sperre für alle Nachrichten mit der [SessionId](/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId), die in der Warteschlange oder im Abonnement vorhanden sind, sowie für alle Nachrichten mit der **SessionId**, die noch während der laufenden Sitzung eingehen.

Die Sperre wird aufgehoben, wenn **Close** oder **CloseAsync** aufgerufen werden oder wenn die Sperre abläuft, weil die Anwendung den Vorgang zum Schließen nicht ausführen kann. Die Sitzungssperre sollte wie eine exklusive Sperre für eine Datei behandelt werden, d. h. die Anwendung muss die Sitzung schließen, sobald sie diese nicht mehr benötigt und/oder keine weiteren Nachrichten erwartet.

Wenn mehrere gleichzeitige Empfänger auf die Warteschlange zugreifen, werden die zu einer bestimmten Sitzung gehörenden Nachrichten an den spezifischen Empfänger gesendet, der gerade die Sperre für diese Sitzung hält. Mit diesem Vorgang erfolgt für einen geschachtelten Nachrichtendatenstrom, der sich in einer Warteschlange oder einem Abonnement befindet, ein ordnungsgemäßes Demultiplexing an verschiedene Empfänger. Diese Empfänger können sich auch auf verschiedenen Clientcomputern befinden, da die Sperrverwaltung in Service Bus auf Dienstseite erfolgt.

Die vorherige Abbildung zeigt drei gleichzeitige Sitzungsempfänger. Eine Sitzung mit `SessionId` = 4 verfügt über keinen aktiven, besitzenden Client, was bedeutet, dass von dieser bestimmten Sitzung keine Nachrichten übermittelt werden. Eine Sitzung fungiert in vielerlei Hinsicht wie eine untergeordnete Warteschlange.

Die vom Sitzungsempfänger gehaltene Sitzungssperre ist ein Schirm für die Nachrichtensperren, die vom *peek-lock*-Abstimmungsmodus verwendet werden. Nur ein Empfänger kann eine Sperre für eine Sitzung haben. Ein Empfänger kann viele übermittelte Nachrichten haben, aber die Nachrichten werden in Reihenfolge empfangen. Das Abbrechen einer Nachricht bewirkt, dass dieselbe Nachricht beim nächsten Empfangsvorgang erneut abgearbeitet wird.

### <a name="message-session-state"></a>Nachrichtensitzungszustand

Wenn Workflows in umfangreichen, hochverfügbaren Cloudsystemen verarbeitet werden, muss der zu einer bestimmten Sitzung gehörende Workflowhandler in der Lage sein, unerwartete Fehler zu beheben. Er kann teilweise abgeschlossene Aufgaben in einem anderen Prozess oder auf einem anderen Computer, auf dem die Aufgabe begonnen hat, fortsetzen.

Der Sitzungszustand ermöglicht eine von der Anwendung definierte Anmerkung einer Nachrichtensitzung innerhalb des Brokers, sodass der aufgezeichnete Verarbeitungszustand in Bezug auf diese Sitzung sofort verfügbar wird, sobald die Sitzung von einem neuen Prozessor übernommen wird.

Aus Service Bus-Sicht ist der Nachrichtensitzungszustand ein undurchsichtiges binäres Objekt, das Daten von der Größe einer Nachricht aufnehmen kann, die bei Service Bus Standard 256 KB und bei Service Bus Premium 1 MB beträgt. Der Verarbeitungszustand relativ zu einer Sitzung kann innerhalb des Sitzungszustands gespeichert werden, oder der Sitzungszustand kann auf einen Speicherort oder Datenbankdatensatz verweisen, der solche Informationen enthält.

Die APIs zum Verwalten des Sitzungszustands, [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) und [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), befinden sich in den C#- und Java-APIs im [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession)-Objekt. Eine Sitzung, für die zuvor kein Sitzungszustand festgelegt wurde, gibt für **GetState** einen **null**-Verweis zurück. Das Löschen des zuvor festgelegten Sitzungszustands erfolgt mit [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Der Sitzungszustand bleibt erhalten, bis er gelöscht wird (Rückgabe: **NULL**), auch wenn alle Nachrichten in einer Sitzung verarbeitet werden.

Alle vorhandenen Sitzungen in einer Warteschlange oder einem Abonnement können in der Java-API mit der **SessionBrowser**-Methode und im .NET Framework-Client mit [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) für [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) und [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) aufgelistet werden.

Der Sitzungszustand, der in einer Warteschlange oder in einem Abonnement gespeichert ist, wird auf das Speicherkontingent dieser Entität angerechnet. Wenn die Anwendung mit einer Sitzung fertig ist, empfiehlt es sich daher für die Anwendung, ihren beibehaltenen Zustand zu bereinigen, um externe Verwaltungskosten zu vermeiden.

### <a name="impact-of-delivery-count"></a>Auswirkungen der Übermittlungsanzahl

Die Definition der Übermittlungsanzahl pro Nachricht im Kontext von Sitzungen weicht geringfügig von der Definition bei fehlenden Sitzungen ab. Hier ist eine Tabelle mit einer Zusammenfassung der Umstände, unter denen die Übermittlungsanzahl erhöht wird.

| Szenario | Übermittlungsanzahl der Nachricht inkrementiert? |
|----------|---------------------------------------------|
| Die Sitzung wird akzeptiert, aber die Sitzungssperre läuft ab (aufgrund eines Timeouts). | Ja |
| Die Sitzung wird akzeptiert, die Nachrichten innerhalb der Sitzung werden nicht abgeschlossen (selbst wenn sie gesperrt sind), und die Sitzung wird geschlossen. | Nein |
| Die Sitzung wird akzeptiert, Nachrichten werden abgeschlossen, und die Sitzung wird explizit geschlossen. | Nicht zutreffend (Standardflow. Hier werden die Nachrichten aus der Sitzung entfernt.) |

## <a name="request-response-pattern"></a>Anforderung/Antwort-Muster
Das [Anforderung/Antwort-Muster](https://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) ist ein etabliertes Integrationsmuster, das es der sendenden Anwendung ermöglicht, eine Anforderung zu senden, und dem Empfänger die Möglichkeit bietet, eine Antwort ordnungsgemäß an die Absenderanwendung zurückzusenden. Dieses Muster benötigt in der Regel eine kurzlebige Warteschlange oder ein kurzlebiges Thema, an die bzw. das die Anwendung Antworten senden soll. In diesem Szenario stellen Sitzungen eine einfache alternative Lösung mit vergleichbarer Semantik dar. 

Mehrere Anwendungen können ihre Anforderungen an eine einzelne Anforderungswarteschlange senden, wobei ein spezifischer Headerparameter zur eindeutigen Identifizierung der Absenderanwendung festgelegt wird. Die Empfängeranwendung kann die in der Warteschlange eingehenden Anforderungen verarbeiten und Antworten in der sitzungsfähigen Warteschlange senden. Dabei wird die Sitzungs-ID auf den eindeutigen Bezeichner festgelegt, den der Absender in der Anforderungsnachricht gesendet hatte. Die Anwendung, die die Anforderung gesendet hat, kann dann Nachrichten mit der bestimmten Sitzungs-ID empfangen und die Antworten ordnungsgemäß verarbeiten.

> [!NOTE]
> Die Anwendung, die die ersten Anforderungen sendet, muss die Sitzungs-ID kennen und `SessionClient.AcceptMessageSession(SessionID)` verwenden, um die Sitzung zu sperren, in der sie die Antwort erwartet. Es ist eine gute Idee, eine GUID zu verwenden, die die Instanz der Anwendung als Sitzungs-ID eindeutig identifiziert. Es darf kein Sitzungshandler oder `AcceptMessageSession(timeout)` in der Warteschlange vorhanden sein, um sicherzustellen, dass die Antworten für die Sperrung und Verarbeitung durch bestimmte Empfänger verfügbar sind.

## <a name="next-steps"></a>Nächste Schritte

- Ein Beispiel, das den .NET Framework-Client verwendet, um für Sitzungen aktivierte Nachrichten zu verarbeiten, finden Sie unter [Microsoft.Azure.ServiceBus-Beispiele](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/Sessions) oder [Microsoft.ServiceBus.Messaging-Beispiele](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions). 

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Themen:

* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png
