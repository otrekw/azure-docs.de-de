---
title: Nachrichtenübertragungen, Sperren und Abgleich in Azure Service Bus
description: Dieser Artikel bietet eine Übersicht über Nachrichtenübertragungen, Sperren und Abgleichungsvorgänge in Azure Service Bus.
ms.topic: article
ms.date: 04/12/2021
ms.custom: devx-track-csharp
ms.openlocfilehash: 6fbbcbf4a1920ee0e66a956443dcfb8a6a17af43
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107306771"
---
# <a name="message-transfers-locks-and-settlement"></a>Nachrichtenübertragungen, Sperren und Abgleich

Die zentrale Funktion eines Nachrichtenbrokers wie z.B. Service Bus besteht darin, Nachrichten in einer Warteschlange oder einem Thema anzunehmen und zum späteren Abruf verfügbar zu halten. *Senden* ist der allgemein gebräuchliche Begriff für die Übertragung einer Nachricht in den Nachrichtenbroker. *Empfangen* ist der allgemein gebräuchliche Begriff für die Übertragung einer Nachricht an einen abrufenden Client.

Wenn ein Client eine Nachricht sendet, möchte er normalerweise wissen, ob die Nachricht ordnungsgemäß an den Broker übertragen und vom Broker akzeptiert wurde oder ob ein Fehler aufgetreten ist. Diese positive oder negative Bestätigung gleicht die Kenntnis des Clients und des Brokers über den Übertragungsstatus der Nachricht ab. Sie wird daher als *Abgleich* bezeichnet.

Wenn der Broker eine Nachricht an einen Client überträgt, möchten der Broker und der Client ebenso darüber übereinkommen, ob die Nachricht erfolgreich verarbeitet wurde und daher entfernt werden kann, oder ob bei der Übertragung oder Verarbeitung der Nachricht ein Fehler aufgetreten ist und die Nachricht daher möglicherweise erneut übertragen werden muss.

## <a name="settling-send-operations"></a>Abgleichen von Sendevorgängen

Bei Verwendung aller unterstützten Service Bus-API-Clients werden Sendevorgänge in Service Bus immer explizit abgeglichen. Das bedeutet, dass der API-Vorgang auf den Empfang eines Annahmeergebnisses von Service Bus wartet und dann den Sendevorgang abschließt.

Wenn die Nachricht von Service Bus abgelehnt wird, enthält die Ablehnung einen Fehlerindikator sowie Text mit einer Nachverfolgungs-ID (**tracking-id**). Die Ablehnung enthält auch Informationen darüber, ob der Vorgang mit Aussicht auf Erfolg wiederholt werden kann. Im Client werden diese Informationen in eine Ausnahme umgewandelt und für den Aufrufer des Sendevorgangs ausgelöst. Wenn die Nachricht angenommen wurde, wird der Vorgang automatisch abgeschlossen.

Bei Verwendung des AMQP-Protokolls (das exklusive Protokoll für die .NET Standard-, Java-, JavaScript-, Python- und Go-Clients und [eine Option für den .NET Framework-Client](service-bus-amqp-dotnet.md)) werden Nachrichtenübertragungen und Abgleiche asynchron über Pipelines abgewickelt. Es wird empfohlen, die API-Varianten des asynchronen Programmiermodells zu verwenden.

Ein Sender kann mehrere Nachrichten in schneller Folge übertragen, ohne darauf warten zu müssen, dass jede Nachricht bestätigt wird, wie dies andernfalls bei Verwendung des SBMP-Protokolls oder mit HTTP 1.1 der Fall wäre. Diese asynchronen Sendevorgänge werden abgeschlossen, wenn die entsprechenden Nachrichten in partitionierten Entitäten angenommen und gespeichert werden oder wenn sich Sendevorgänge an unterschiedliche Entitäten überlappen. Die Abschlüsse können auch außerhalb der ursprünglichen Sendereihenfolge erfolgen.

Die Strategie zur Verarbeitung des Ergebnisses von Sendevorgängen kann direkte und erhebliche Leistungsauswirkungen auf die Anwendung haben. Die Beispiele in diesem Abschnitt sind in C# geschrieben und gelten für Java-Futures, Java-Monos, JavaScript-Zusagen und entsprechende Konzepte in anderen Programmiersprachen.

Wenn die Anwendung eine Burstübertragung von Nachrichten generiert, hier mit einer einfachen Schleife dargestellt, und der Abschluss jedes Sendevorgangs abgewartet werden muss, bevor die nächste Nachricht gesendet werden kann, wird das Senden von 10 Nachrichten bei synchronen wie auch bei asynchronen API-Formen erst nach 10 sequenziellen vollständigen Roundtrips für den Abgleich abgeschlossen.

Bei einer angenommenen Latenzentfernung des TCP-Roundtrips von 70 Millisekunden zwischen einem lokalen Standort und Service Bus und der Annahme von nur 10 ms für Service Bus zum Akzeptieren und Speichern jeder Nachricht dauert die folgende Schleife mindestens acht Sekunden (ohne Berücksichtigung der Übertragungszeit der Nutzdaten oder der Auswirkungen einer potenziellen Überlastung bei der Weiterleitung):

```csharp
for (int i = 0; i < 100; i++)
{
  // creating the message omitted for brevity
  await client.SendAsync(…);
}
```

Wenn die Anwendung die zehn asynchronen Sendevorgänge in direkter Folge startet und gesondert auf den jeweiligen Abschluss wartet, überschneidet sich die Roundtripzeit für diese zehn Sendevorgänge. Die 10 Nachrichten werden in direkter Folge übertragen und teilen möglicherweise sogar TCP-Frames. Die gesamte Übertragungsdauer hängt dann weitgehend von der netzwerkbezogenen Zeit für die Übertragung der Nachrichten an den Broker ab.

Bei den gleichen Annahmen wie für die vorherige Schleife bleibt die gesamte überlappende Ausführungszeit für die folgende Schleife möglicherweise unter einer Sekunde:

```csharp
var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  tasks.Add(client.SendAsync(…));
}
await Task.WhenAll(tasks);
```

Dabei ist unbedingt zu beachten, dass alle asynchronen Programmiermodelle eine Form einer speicherbasierten, verborgenen Arbeitswarteschlange verwenden, die ausstehende Vorgänge enthält. Wenn die Sende-API so weit ist, wird der Sendetask in dieser Arbeitswarteschlange platziert, die Protokollgeste beginnt jedoch erst, wenn der Task ausgeführt wird. Bei Code, der tendenziell eine Burstübertragung von Nachrichten generiert und bei dem Zuverlässigkeit relevant ist, sollte darauf geachtet werden, dass nicht zu viele Nachrichten gleichzeitig aktiv sind, da alle gesendeten Nachrichten Speicher belegen, bis sie tatsächlich übertragen werden.

Semaphore, wie im folgenden Codeausschnitt in C# gezeigt, sind Synchronisierungsobjekte, die eine solche Drosselung auf Anwendungsebene bei Bedarf ermöglichen. Bei dieser Verwendung eines Semaphors können höchstens 10 Nachrichten gleichzeitig aktiv sein. Eine der 10 verfügbaren Semaphorsperren wird vor dem Senden festgelegt und aufgehoben, wenn der Sendevorgang abgeschlossen ist. Der 11. Durchlauf durch die Schleife wartet, bis mindestens einer der vorherigen Sendevorgänge abgeschlossen ist, und macht dann seine Sperre verfügbar:

```csharp
var semaphore = new SemaphoreSlim(10);

var tasks = new List<Task>();
for (int i = 0; i < 100; i++)
{
  await semaphore.WaitAsync();

  tasks.Add(client.SendAsync(…).ContinueWith((t)=>semaphore.Release()));
}
await Task.WhenAll(tasks);
```

Anwendungen sollten **niemals** einen asynchronen „Fire-and-Forget“-Sendevorgang initiieren, ohne das Ergebnis des Vorgangs abzurufen. Andernfalls kann dies dazu führen, dass die interne und nicht sichtbare Taskwarteschlange bis zur Ausschöpfung des Speichers geladen wird und die Anwendung Sendefehler nicht erkennen kann:

```csharp
for (int i = 0; i < 100; i++)
{

  client.SendAsync(message); // DON’T DO THIS
}
```

Mit einem AMQP-Client auf niedriger Ebene akzeptiert Service Bus auch „vorab abgeglichene“ Übertragungen. Eine vorab abgeglichene Übertragung ist ein Fire-and-Forget-Vorgang, bei dem das Ergebnis (in beiden Fällen) nicht zurück an den Client gesendet wird und die Nachricht nach dem Senden als abgeglichen gilt. Das fehlende Feedback an den Client bedeutet auch, dass keine verwertbaren Daten für die Diagnose verfügbar sind. Das heißt, dass bei diesem Modus keine Unterstützung über den Azure-Support möglich ist.

## <a name="settling-receive-operations"></a>Abgleichen von Empfangsvorgängen

Für Empfangsvorgänge ermöglichen die Service Bus-API-Clients zwei verschiedene explizite Modi: *Receive-and-Delete* und *Peek-Lock*.

### <a name="receiveanddelete"></a>ReceiveAndDelete

Im Modus **ReceiveAndDelete** wird der Broker angewiesen, alle an den empfangenden Client gesendeten Nachrichten nach dem Senden als abgeglichen einzustufen. Das bedeutet, dass eine Nachricht als verarbeitet gilt, sobald der Broker sie übertragen hat. Wenn bei der Nachrichtenübertragung ein Fehler auftritt, geht die Nachricht verloren.

Der Vorteil dieses Modus besteht darin, dass der Empfänger keine weitere Aktion für die Nachricht ausführen muss und auch nicht dadurch verlangsamt wird, dass er auf das Ergebnis des Abgleichs warten muss. Wenn die in den einzelnen Nachrichten enthaltenen Daten einen geringen Wert haben und/oder nur für einen sehr kurzen Zeitraum aussagekräftig sind, ist dieser Modus sinnvoll.

### <a name="peeklock"></a>PeekLock

Im Modus **PeekLock** wird der Broker informiert, dass der empfangende Client empfangene Nachrichten explizit abgleichen möchte. Die Nachricht wird für den Empfänger zur Verarbeitung verfügbar gemacht und gleichzeitig im Dienst exklusiv gesperrt, sodass andere konkurrierende Empfänger sie nicht sehen können. Die Dauer der Sperre wird anfänglich auf Warteschlangen- oder Abonnementebene definiert und kann von dem Client, der die Sperre besitzt, erweitert werden. Ausführliche Informationen zum Verlängern von Sperren finden Sie in diesem Artikel im Abschnitt [Verlängern von Sperren](#renew-locks). 

Wenn eine Nachricht gesperrt ist, können andere Clients, die Nachrichten von derselben Warteschlange oder demselben Abonnement empfangen, Sperren annehmen und die nächsten verfügbaren Nachrichten abrufen, die nicht aktiv gesperrt sind. Wenn die Sperre für eine Nachricht explizit aufgehoben wird oder abläuft, wird die Nachricht zur erneuten Übertragung wieder ganz oder nahe am Anfang in der Abrufreihenfolge angezeigt.

Wenn die Nachricht wiederholt von Empfängern freigegeben wird oder diese die Sperre nach einer bestimmten Anzahl von Übertragungen ([Anzahl maximaler Zustellungen](service-bus-dead-letter-queues.md#maximum-delivery-count)) ablaufen lassen, wird die Nachricht automatisch aus der Warteschlange oder dem Abonnement entfernt und in der zugeordneten Warteschlange für unzustellbare Nachrichten platziert.

Der empfangende Client initiiert den Abgleich einer empfangenen Nachricht mit einer positiven Bestätigung, wenn er die API `Complete` für die Nachricht aufruft. Für den Broker bedeutet das, dass die Nachricht erfolgreich verarbeitet wurde. Die Nachricht wird dann aus der Warteschlange oder dem Abonnement entfernt. Der Broker antwortet auf die Abgleichabsicht des Empfängers mit einer Antwort, die angibt, ob der Abgleich ausgeführt werden kann.

Wenn der empfangende Client eine Nachricht nicht verarbeiten kann, die Nachricht jedoch erneut übertragen werden soll, kann er explizit anfordern, dass die Nachricht freigegeben und die Sperre direkt aufgehoben werden soll, indem er die API `Abandon` für die Nachricht aufruft. Alternativ kann der Client keine Aktion ausführen und die Sperre ablaufen lassen.

Wenn ein empfangender Client eine Nachricht nicht verarbeiten kann und weiß, dass die erneute Übertragung der Nachricht und die Wiederholung des Vorgangs nicht hilft, kann er die Nachricht ablehnen. Die Nachricht wird daraufhin durch Aufrufen der API `DeadLetter` für die Nachricht in die Warteschlange für unzustellbare Nachrichten verschoben. Dies ermöglicht auch die Festlegung einer benutzerdefinierten Eigenschaft mit einem Ursachencode, der mit der Nachricht aus der Warteschlange für unzustellbare Nachrichten abgerufen werden kann.

Ein Sonderfall des Abgleichs ist die Verzögerung, die in einem [separaten Artikel](message-deferral.md) beschrieben wird.

Bei den Vorgängen `Complete`, `Deadletter` und `RenewLock` können aufgrund von Netzwerkproblemen Fehler auftreten, wenn die festgelegte Sperre abgelaufen ist oder andere dienstseitige Bedingungen den Abgleich verhindern. In einem der letzteren Fälle sendet der Dienst eine negative Bestätigung, die in den API-Clients als Ausnahme angezeigt wird. Wenn die Ursache eine unterbrochene Netzwerkverbindung ist, wird die Sperre gelöscht, da Service Bus die Wiederherstellung von vorhandenen AMQP-Links für eine andere Verbindung nicht unterstützt.

Ist `Complete` nicht erfolgreich (in der Regel ganz am Ende der Nachrichtenverarbeitung, in einigen Fällen aber auch nach mehreren Minuten der Verarbeitung), kann die empfangende Anwendung entscheiden, ob sie den Status der Arbeit beibehält und die Nachricht ignoriert, wenn sie ein zweites Mal übertragen wird, oder ob sie das Arbeitsergebnis verwirft und den Vorgang bei der erneuten Übertragung der Nachricht wiederholt.

Das typische Verfahren zum Identifizieren von doppelten Nachrichtenübertragungen besteht in der Überprüfung der Nachrichten-ID (message-id), die vom Sender auf einen eindeutigen Wert festgelegt werden kann und sollte, eventuell verbunden mit einem Bezeichner vom sendenden Prozess. Ein Auftragsplaner wird wahrscheinlich die message-id auf den Bezeichner des Auftrags, der einem Worker zugewiesen werden soll, mit dem gegebenen Worker festlegen, und der Worker wird das zweite Vorkommen der Auftragszuweisung ignorieren, sofern der Auftrag bereits abgeschlossen ist.

> [!IMPORTANT]
> Beachten Sie unbedingt, dass die Sperre, die PeekLock für die Nachricht abruft, flüchtig ist und unter den folgenden Bedingungen verloren gehen kann:
>   * Dienstupdate
>   * Betriebssystemupdate
>   * Ändern von Eigenschaften für die Entität (Warteschlange, Thema, Abonnement) während der Sperre.
>
> Wenn die Sperre verloren geht, generiert Azure Service Bus eine LockLostException, die im Clientanwendungscode angezeigt wird. In diesem Fall sollte die standardmäßige Wiederholungslogik des Clients automatisch gestartet werden und den Vorgang wiederholen.

## <a name="renew-locks"></a>Verlängern von Sperren
Der Standardwert für die Sperrdauer beträgt **30 Sekunden**. Sie können einen anderen Wert für die Sperrdauer auf Warteschlangen- oder Abonnementebene angeben. Der Client, der die Sperre besitzt, kann die Nachrichtensperre mithilfe von Methoden für das Empfängerobjekt verlängern. Stattdessen können Sie das Feature für die automatische Verlängerung von Sperren verwenden und dort den Zeitraum angeben, für den die Sperre verlängert werden soll. 

## <a name="next-steps"></a>Nächste Schritte
- Ein Sonderfall des Abgleichs ist die Verzögerung. Ausführliche Informationen finden Sie unter [Nachrichtenverzögerung](message-deferral.md). 
- Informationen zu unzustellbaren Nachrichten finden Sie in der [Übersicht über Service Bus-Warteschlangen für unzustellbare Nachrichten](service-bus-dead-letter-queues.md).
- Weitere allgemeine Informationen zu Service Bus-Nachrichten finden Sie unter [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md).