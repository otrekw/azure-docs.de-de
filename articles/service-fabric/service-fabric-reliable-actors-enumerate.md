---
title: Aufzählen von Actors in Azure Service Fabric
description: Erfahren Sie anhand von Beispielen mehr über das Aufzählen von Reliable Actors und deren Metadaten in einer Azure Service Fabric-Anwendung.
author: vturecek
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 8e462cc5fa82b8692304f58ef6cf0ea0e2db8725
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86245975"
---
# <a name="enumerate-service-fabric-reliable-actors"></a>Aufzählen von Reliable Actors in Service Fabric
Der Reliable Actors-Dienst ermöglicht Clients das Aufzählen von Metadaten über die vom Dienst gehosteten Actors. Da der Akteurdienst ein partitionierter zustandsbehafteter Dienst ist, erfolgt die Enumeration für die einzelnen Partitionen. Da jede Partition viele Akteure enthalten kann, wird die Enumeration als Satz von Ergebnisseiten zurückgegeben. Die Seiten werden in einer Schleife durchlaufen, bis alle Seiten gelesen wurden. Im folgenden Beispiel wird veranschaulicht, wie eine Liste aller aktiven Akteure in einer Partition eines Actordiensts erstellt wird:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```



## <a name="next-steps"></a>Nächste Schritte
* [Actor-Zustandsverwaltung](service-fabric-reliable-actors-state-management.md)
* [Actor-Lebenszyklus und Garbage Collection](service-fabric-reliable-actors-lifecycle.md)
* [Actors API reference documentation](/previous-versions/azure/dn971626(v=azure.100))
* [.NET-Beispielcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-Beispielcode](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
