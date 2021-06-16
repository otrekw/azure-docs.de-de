---
title: 'Azure Service Fabric: dynamische Knotentags'
description: Azure Service Fabric ermöglicht das dynamische Hinzufügen und Entfernen von Knotentags.
author: yu-supersonic
ms.topic: conceptual
ms.date: 04/05/2021
ms.author: branim
ms.custom: devx-track-csharp
ms.openlocfilehash: b20ab4720f9f172ef9248d3314b25922896eb8bd
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "110796928"
---
# <a name="introduction-to-dynamic-node-tags"></a>Einführung in dynamische Knotentags
Mit Knotentags können Sie dynamisch Tags für Knoten hinzufügen und entfernen, um die Platzierung von Diensten zu beeinflussen. Knotentagging ist äußerst flexibel und ermöglicht Änderungen an der Dienstplatzierung ohne Anwendungs- oder Clusterupgrades. Tags können jederzeit für Knoten hinzugefügt oder entfernt werden, und bei der Erstellung von Diensten können Anforderungen für bestimmte Tags angegeben werden. Die Taganforderungen eines Diensts können auch dynamisch während der Dienstausführung aktualisiert werden.

Knotentagging ist mit [Platzierungseinschränkungen](service-fabric-cluster-resource-manager-configure-services.md) vergleichbar und wird in der Regel verwendet, um zu steuern, auf welchen Knoten ein Dienst ausgeführt wird. Jeder Service Fabric-Dienst kann so konfiguriert werden, dass für die Platzierung oder weitere Ausführung ein Tag erforderlich ist.

Knotentagging wird für alle von Service Fabric gehosteten Diensttypen (Reliable Services, ausführbare Gastanwendungsdateien und Container) unterstützt. Zur Verwendung des Knotentaggings wird mindestens die Version 8.0 der Service Fabric-Runtime benötigt.

Im weiteren Verlauf dieses Artikels werden Verfahren zum Aktivieren oder Deaktivieren des Knotentaggings beschrieben. Außerdem finden Sie hier Beispiele zur Verwendung dieses Features.


## <a name="describing-dynamic-node-tags"></a>Beschreiben dynamischer Knotentags
Von Diensten können die erforderlichen Tags angegeben werden. Es gibt zwei Arten von Tags:
* **Für die Platzierung erforderliche Tags:** Hierbei handelt es sich um eine Gruppe von Tags, die nur für die Dienstplatzierung erforderlich sind. Nach der Platzierung des Replikats können diese Tags entfernt werden, ohne den Dienst zu unterbrechen. Wird eines dieser Tags aus dem Knoten entfernt, funktioniert das Dienstreplikat weiterhin, und der Dienst wird von Service Fabric nicht entfernt.

* **Für die Ausführung erforderliche Tags:** Hierbei handelt es sich um eine Gruppe von Tags, die sowohl für die Platzierung als auch für die Ausführung des Diensts erforderlich sind. Wird eines der für die Ausführung erforderlichen Tags entfernt, wird der Dienst von Service Fabric auf einen anderen Knoten verschoben, der über diese Tags verfügt.

Beispiel: Für die Platzierung erforderliche Tags können verwendet werden, wenn Sie einen Containeraktivatordienst verwenden und dieser Dienst für Ihren Container platziert werden muss. Nach der Aktivierung des Containers wird der Aktivator nicht mehr benötigt, und Sie können das zugehörige Tag entfernen. Der Container soll allerdings weiterhin ausgeführt werden.
Für die Ausführung erforderliche Tags können verwendet werden, wenn Sie über einen Abrechnungsdienst verfügen, der sich möglichst am gleichen Ort befinden sollte wie ein benutzerorientierter Dienst. Wenn der Abrechnungsdienst auf dem Knoten ausfällt, entfernen Sie das zugehörige Tag. Der benutzerorientierte Dienst wird daraufhin auf einen anderen Knoten verschoben, der über den Abrechnungsdienst (sowie über das zugehörige Tag) verfügt.

Ein Tag oder eine Gruppe von Tags kann mithilfe von standardmäßigen Service Fabric-Schnittstellenmechanismen wie C#-APIs, REST-APIs oder PowerShell-Befehlen für einen einzelnen Knoten hinzugefügt, aktualisiert oder entfernt werden.

> [!NOTE]
> Von Service Fabric werden bei Verwendung von Knotentags keine FD/UD-Verteilungen verwaltet. Verwalten Sie Knotentags entsprechend, um FD/UD-Verstöße aufgrund einer fehlerhaften domänenübergreifenden Verteilung zu vermeiden.

## <a name="enabling-dynamic-node-tags"></a>Aktivieren dynamischer Knotentags
Damit dieses Feature funktioniert, müssen Sie die Konfiguration „NodeTaggingEnabled“ im Abschnitt „PlacementAndLoadBalancing“ des Clustermanifests mithilfe von XML oder JSON aktivieren:

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="NodeTaggingEnabled" Value="true" />
</Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "NodeTaggingEnabled",
          "value": "true"
      }
    ]
  }
]
```

## <a name="setting-dynamic-node-tags"></a>Festlegen dynamischer Knotentags

### <a name="using-powershell"></a>PowerShell

Hinzufügen von Knotentags zum Knoten:

```posh
Add-ServiceFabricNodeTags -NodeName "DB.1" -NodeTags @("SampleTag1", "SampleTag2")
```
Durch diesen Befehl werden dem Knoten „DB.1“ die Tags „SampleTag1“ und „SampleTag2“ hinzugefügt.

Entfernen von Knotentags vom Knoten:

```posh
Remove-ServiceFabricNodeTags -NodeName "DB.1" -NodeTags @("SampleTag1", "SampleTag2")
```
Durch diesen Befehl werden die Tags „SampleTag1“ und „SampleTag2“ vom Knoten „DB.1“ entfernt.

### <a name="using-c-apis"></a>Verwenden von C#-APIs

Hinzufügen von Knotentags zum Knoten:

```csharp
FabricClient fabricClient = new FabricClient();
List<string> nodeTagsList = new List<string>();
nodeTagsList.Add("SampleTag1");
nodeTagsList.Add("SampleTag2");
await fabricClient.ClusterManager.AddNodeTagsAsync("DB.1", nodeTagsList);
```

Entfernen von Knotentags vom Knoten:

```csharp
FabricClient fabricClient = new FabricClient();
List<string> nodeTagsList = new List<string>();
nodeTagsList.Add("SampleTag1");
nodeTagsList.Add("SampleTag2");
await fabricClient.ClusterManager.RemoveNodeTagsAsync("DB.1", nodeTagsList);
```

## <a name="setting-required-tags-for-services"></a>Festlegen der erforderlichen Tags für Dienste

### <a name="using-powershell"></a>PowerShell

Erstellen eines neuen Diensts:

```posh
New-ServiceFabricService -ApplicationName fabric:/HelloWorld -ServiceName fabric:/HelloWorld/svc1 -ServiceTypeName HelloWorldStateful -Stateful -PartitionSchemeSingleton -TargetReplicaSetSize 5 -MinReplicaSetSize 3 -TagsRequiredToRun @("SampleTag1") - TagsRequiredToPlace @("SampleTag2")
```
Durch diesen Befehl wird ein Dienst erstellt, für den „SampleTag2“ auf einem Knoten vorhanden sein muss, damit der Dienst dort platziert wird, und „SampleTag1“ vorhanden sein muss, damit der Dienst auf diesem Knoten weiter ausgeführt wird.

Aktualisieren eines vorhandenen Diensts:

```posh
Update-ServiceFabricService -Stateful -ServiceName fabric:/myapp/test -TagsRequiredToRun @("SampleTag1") -TagsRequiredToPlace @("SampleTag2")
```
Durch diesen Befehl wird ein Dienst aktualisiert, für den „SampleTag2“ auf einem Knoten vorhanden sein muss, damit der Dienst dort platziert wird, und „SampleTag1“ vorhanden sein muss, damit der Dienst auf diesem Knoten weiter ausgeführt wird.

### <a name="using-c-apis"></a>Verwenden von C#-APIs

Erstellen eines neuen Diensts:

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
ServiceTags serviceTags = new ServiceTags();
serviceTags.TagsRequiredToPlace.Add("SampleTag1");
serviceTags.TagsRequiredToRun.Add("SampleTag2");
serviceDescription.ServiceTags = serviceTags;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Aktualisieren eines vorhandenen Diensts:

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceUpdateDescription serviceUpdate = new StatefulServiceUpdateDescription();
ServiceTags serviceTags = new ServiceTags();
serviceTags.TagsRequiredToPlace.Add("SampleTag1");
serviceTags.TagsRequiredToRun.Add("SampleTag2");
serviceUpdate.ServiceTags = serviceTags;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), serviceUpdate);
```

Alle diese Befehle können ebenso für zustandslose Dienste verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Platzierungseinschränkungen finden Sie [hier](service-fabric-cluster-resource-manager-configure-services.md).
