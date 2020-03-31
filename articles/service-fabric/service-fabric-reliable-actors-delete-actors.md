---
title: Löschen von Azure Service Fabric-Actors
description: Hier erfahren Sie, wie Sie Reliable Actors und deren Zustand in einer Azure Service Fabric-Anwendung manuell und vollständig löschen.
author: amanbha
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: amanbha
ms.openlocfilehash: b90c5a10c64e273f1c8f48c7bf5713859796db65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645615"
---
# <a name="delete-reliable-actors-and-their-state"></a>Löschen von Reliable Actors und deren Zustand
Eine Garbage Collection von deaktivierten Actors bereinigt nur das Actor-Objekt, entfernt jedoch keine Daten, die im Zustands-Manager eines Actors gespeichert sind. Wenn ein Actor reaktiviert wird, werden seine Daten durch den Zustands-Manager wieder zur Verfügung gestellt. Wenn Actors Daten im Zustands-Manager speichern und dann deaktiviert, aber nicht wieder aktiviert werden, kann eine Bereinigung ihrer Daten nötig sein.

Der [Actordienst](service-fabric-reliable-actors-platform.md) bietet auch eine Funktion zum Löschen von Actors über einen Remoteanrufer:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Das Löschen eines Actors hat die folgenden Auswirkungen, abhängig davon, ob der Actor derzeit aktiv ist oder nicht:

* **Aktiver Actor**
  * Der Actor wird aus der Liste der aktiven Actors entfernt und deaktiviert.
  * Sein Zustand wird dauerhaft gelöscht.
* **Inaktiver Actor**
  * Sein Zustand wird dauerhaft gelöscht.

Ein Actor kann „delete“ nicht über eine seiner Actormethoden für sich selbst aufrufen, da er nicht gelöscht werden kann, während er in einem Actoraufrufkontext ausgeführt wird, in dem die Laufzeit eine Sperre für den Actoraufruf erwirkt hat, um einen Singlethread-Zugriff zu erzwingen.

Weitere Informationen zu Reliable Actors finden Sie hier:
* [Actor-Timer und -Erinnerungen](service-fabric-reliable-actors-timers-reminders.md)
* [Actor-Ereignisse](service-fabric-reliable-actors-events.md)
* [Actor-Eintrittsinvarianz](service-fabric-reliable-actors-reentrancy.md)
* [Actor-Diagnose und -Leistungsüberwachung](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C#-Beispielcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-Beispielcode](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
