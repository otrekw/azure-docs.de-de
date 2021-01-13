---
title: 'Azure Event Hubs: .NET-Ausnahmen'
description: Dieser Artikel enthält eine Liste mit .NET-Messagingausnahmen für Azure Event Hubs sowie vorgeschlagene Aktionen.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 09/23/2020
ms.author: shvija
ms.openlocfilehash: 6a1d7c969d31033ae9d00d212cc8f1a45abbeda1
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91339552"
---
# <a name="eventhubsexception---net"></a>EventHubsException: .NET
Eine Ausnahme vom Typ **EventHubsException** wird ausgelöst, wenn ein Problem durch einen Event Hubs-spezifischen Vorgang verursacht wurde. Dies schließt sowohl dienstinterne als auch clientspezifische Fehler ein. 

## <a name="exception-information"></a>Ausnahmeinformationen
Die Ausnahme enthält die folgenden Kontextinformationen, damit Sie den Kontext des Fehlers sowie den relativen Schweregrad nachvollziehen können. 

- **IsTransient**: Gibt an, ob die Ausnahme als wiederherstellbar betrachtet wird. Wenn sie als vorübergehend betrachtet wurde, wurde die entsprechende Wiederholungsrichtlinie bereits angewendet, und erneute Versuche waren nicht erfolgreich.
- **Grund:** Gibt eine Reihe bekannter Gründe für den Fehler an, die zur Kategorisierung und Klärung der Grundursache beitragen. Diese Gründe dienen dazu, die Anwendung von Ausnahmefilterung und anderer Logik zu ermöglichen, wenn die Untersuchung des Texts einer Ausnahmemeldung nicht ideal wäre. Zu wichtigen Fehlerursachen zählen unter anderem folgende:
    - **Client geschlossen:** Tritt auf, wenn ein Event Hub-Client bereits geschlossen oder verworfen wurde. Vergewissern Sie sich anhand des Anwendungscodes, dass Objekte aus der Event Hubs-Clientbibliothek im vorgesehenen Bereich erstellt und geschlossen werden.
    - **Timeout des Diensts:** Gibt an, dass der Event Hubs-Dienst nicht innerhalb des erwarteten Zeitraums auf einen Vorgang reagiert hat. Dieses Problem ist möglicherweise auf ein vorübergehendes Netzwerk- oder Dienstproblem zurückzuführen. Es ist nicht klar, ob die Anforderung durch den Event Hubs-Dienst erfolgreich abgeschlossen wurde. Überprüfen Sie nach Möglichkeit den aktuellen Zustand, und wiederholen Sie den Vorgang bei Bedarf.
    - **Kontingent überschritten:** Gibt an, dass für eine einzelne Consumergruppe zu viele aktive Lesevorgänge vorhanden sind. Dieser Grenzwert hängt von der Ebene des Event Hubs-Namespace ab. Unter Umständen ist eine Umstellung auf eine höhere Ebene erforderlich. Alternativ können Sie zusätzliche Consumergruppen erstellen und sicherstellen, dass die Anzahl von Consumerclient-Lesevorgängen bei keiner Gruppe über den Grenzwert hinausgehen. Weitere Informationen finden Sie unter [Kontingente und Grenzwerte in Azure Event Hubs](event-hubs-quotas.md).
    - **Nachrichtengröße überschritten:** Bei Ereignisdaten gilt eine maximal zulässige Größe (sowohl für ein einzelnes Ereignis als auch für einen Ereignisbatch). Sie beinhaltet die Daten des Ereignisses sowie alle zugehörigen Metadaten und zusätzlichen Systemdaten. Verringern Sie zur Behebung dieses Fehlers die Anzahl von Ereignissen, die in einem Batch gesendet werden, oder die Größe der in der Nachricht enthaltenen Daten. Größenbeschränkungen können sich ggf. ändern. Einzelheiten finden Sie unter [Kontingente und Grenzwerte in Azure Event Hubs](event-hubs-quotas.md).
    - **Verbindung mit Consumer getrennt:** Vom Event Hub-Dienst wurde die Verbindung zwischen einem Consumerclient und der Event Hub-Instanz getrennt. Das passiert in der Regel, wenn ein Consumer mit einer höheren Besitzerebene eine Kombination aus Partition und Consumergruppe für sich beansprucht.
    - **Ressource nicht gefunden:** Vom Event Hubs-Dienst wurde eine Ressource (Event Hub, Consumergruppe, Partition oder Ähnliches) nicht gefunden. Möglicherweise wurde sie gelöscht, oder es liegt ein Problem mit dem Event Hubs-Dienst vor.

## <a name="handling-exceptions"></a>Behandeln von Ausnahmen
Sie können auf verschiedene Arten auf eine bestimmte Fehlerursache für die Ausnahme **EventHubException** reagieren. Eine Möglichkeit besteht darin, im Rahmen des Catch-Blocks eine Ausnahmefilterklausel anzuwenden.

```csharp
try
{
    // Read events using the consumer client
}
catch (EventHubsException ex) where 
    (ex.Reason == EventHubsException.FailureReason.ConsumerDisconnected)
{
    // Take action based on a consumer being disconnected
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Ausnahmen sind im [Legacy-Artikel](event-hubs-messaging-exceptions.md) dokumentiert. Einige davon gelten nur für die ältere .NET-Clientbibliothek von Event Hubs.