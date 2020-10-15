---
title: Eintrittsinvarianz in Azure Service Fabric-Akteuren
description: 'Einführung in die Eintrittsinvarianz für Service Fabric Reliable Actors: eine Möglichkeit, das Blockieren basierend auf dem Aufrufkontext logisch zu vermeiden.'
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.custom: devx-track-csharp
ms.openlocfilehash: 75512633dae8d9a9f61c37bbc795d8e8d20c4ff1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89007807"
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
* Erfahren Sie mehr zur Eintrittsinvarianz in der [Referenzdokumentation zur Actor-API](/previous-versions/azure/dn971626(v=azure.100))
