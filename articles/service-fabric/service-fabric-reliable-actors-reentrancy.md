---
title: Eintrittsinvarianz in Azure Service Fabric-Akteuren
description: 'Einführung in die Eintrittsinvarianz für Service Fabric Reliable Actors: eine Möglichkeit, das Blockieren basierend auf dem Aufrufkontext logisch zu vermeiden.'
ms.topic: conceptual
ms.date: 11/02/2017
ms.custom: devx-track-csharp
ms.openlocfilehash: 90a8e9146d18d628fbf124307f1b79b912c9cef9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98789379"
---
# <a name="reliable-actors-reentrancy"></a>Reliable Actors-Eintrittsinvarianz
Die Reliable Actors-Laufzeit erlaubt bei logischen Aufrufen standardmäßig kontextbasierte Eintrittsinvarianz. Dadurch können Actors eintrittsinvariant sein, wenn sie sich in der selben Aufrufkontextkette befinden. Beispiel: Actor A sendet eine Nachricht an Actor B und dieser wiederum sendet eine Nachricht an Actor C. Wenn Actor C Actor A aufruft, wird im Rahmen der Nachrichtenverarbeitung zugelassen, dass die Nachricht eintrittsinvariant ist. Alle weiteren Nachrichten, die zu unterschiedlichen Aufrufkontexten gehören, werden auf Actor A blockiert, bis dieser die Verarbeitung abgeschlossen hat.

Für die in der Enumeration `ActorReentrancyMode` definierte Actor-Eintrittsinvarianz sind zwei Optionen verfügbar:

* `LogicalCallContext` (Standardverhalten)
* `Disallowed` : Deaktiviert die Eintrittsinvarianz.

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Die Eintrittsinvarianz kann während der Registrierung in den `ActorService`-Einstellungen konfiguriert werden. Die Einstellung gilt für alle im Actor-Dienst erstellten Actor-Instanzen.

Das folgende Beispiel zeigt einen Actor-Dienst, der den Eintrittsinvarianz-Modus auf `ActorReentrancyMode.Disallowed`festlegt. Wenn in diesem Fall ein Actor eine eintrittsinvariante Nachricht an einen anderen Actor sendet, wird eine Ausnahme vom Typ `FabricException` ausgelöst.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Nächste Schritte
* Die [Referenzdokumentation zur Actor-API](/dotnet/api/microsoft.servicefabric.actors) bietet weitere Informationen zur Eintrittsinvarianz.
