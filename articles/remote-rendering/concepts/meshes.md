---
title: Gittermodelle
description: Definition von Gittermodellen im Bereich von Azure Remote Rendering
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 08d80a5ec2099147c12bcecd3b52d64429837285
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90563963"
---
# <a name="meshes"></a>Gittermodelle

## <a name="mesh-resource"></a>Gittermodellressource

Bei Gittermodellen (Meshes) handelt es sich um eine unveränderliche [gemeinsam genutzte Ressource](../concepts/lifetime.md), die nur per [Modellkonvertierung](../how-tos/conversion/model-conversion.md) erstellt werden kann. Gittermodelle enthalten ein oder mehrere *untergeordnete Gittermodelle* zusammen mit einer Physikdarstellung für [Raycastabfragen](../overview/features/spatial-queries.md). Für jedes untergeordnete Gittermodell wird auf ein [Material](materials.md) verwiesen, das standardmäßig zum Rendern verwendet werden soll. Fügen Sie zum Platzieren eines Gittermodells in einem 3D-Raum einer [Entität](entities.md) ein [MeshComponent](#meshcomponent)-Element hinzu.

### <a name="mesh-resource-properties"></a>Eigenschaften von Gittermodellressourcen

Die Eigenschaften der `Mesh`-Klasse lauten:

* **Materials** (Materialien): Ein Array mit Materialien. Jedes Material wird von einem anderen untergeordneten Gittermodell genutzt. Es ist möglich, dass von mehreren Einträgen im Array auf dasselbe [Material](materials.md) verwiesen wird. Diese Daten können zur Runtime nicht geändert werden.

* **Bounds** (Begrenzungen): Ein an den Achsen ausgerichteter Begrenzungsrahmen (Axis-Aligned Bounding Box, AABB) für die Scheitelpunkte des Gittermodells im lokalen Raum.

## <a name="meshcomponent"></a>MeshComponent

Die `MeshComponent`-Klasse wird verwendet, um eine Instanz einer Gittermodellressource zu platzieren. Jedes „MeshComponent“-Element verweist auf ein Gittermodell. Hiermit kann überschrieben werden, welche Materialien zum Rendern der einzelnen untergeordneten Gittermodelle verwendet werden.

### <a name="meshcomponent-properties"></a>Eigenschaften von „MeshComponent“

* **Mesh** (Gittermodell): Die von dieser Komponente verwendete Gittermodellressource.

* **Materials** (Materialien): Das Array mit den Materialien, die für die Gittermodellkomponente selbst angegeben wurden. Das Array hat immer die gleiche Länge wie das Array *Materials* (Materialien) der Gittermodellressource. Materialien, die nicht durch die Standardeinstellung des Gittermodells überschrieben werden sollen, sind in diesem Array auf *null* festgelegt.

* **UsedMaterials** (Verwendete Materialien): Das Array mit den tatsächlich genutzten Materialien für die einzelnen untergeordneten Gittermodelle. Dies ist für Werte, die nicht „null“ sind, mit den Daten im Array *Materials* (Materialien) identisch. Andernfalls ist hierin der Wert aus dem Array *Materials* (Materialien) der Gittermodellinstanz enthalten.

### <a name="sharing-of-meshes"></a>Gemeinsame Verwendung von Gittermodellen

Eine `Mesh`-Ressource kann von mehreren Instanzen von Gittermodellkomponenten gemeinsam verwendet werden. Darüber hinaus kann die `Mesh`-Ressource, die einer Gittermodellkomponente zugeordnet ist, jederzeit programmgesteuert geändert werden. Im folgenden Code wird das Klonen von Gittermodellen veranschaulicht:

```cs
Entity CloneEntityWithModel(RemoteManager manager, Entity sourceEntity)
{
    MeshComponent meshComp = sourceEntity.FindComponentOfType<MeshComponent>();
    if (meshComp != null)
    {
        Entity newEntity = manager.CreateEntity();
        MeshComponent newMeshComp = manager.CreateComponent(ObjectType.MeshComponent, newEntity) as MeshComponent;
        newMeshComp.Mesh = meshComp.Mesh; // share the mesh
        return newEntity;
    }
    return null;
}
```

```cpp
ApiHandle<Entity> CloneEntityWithModel(ApiHandle<RemoteManager> manager, ApiHandle<Entity> sourceEntity)
{
    if (ApiHandle<MeshComponent> meshComp = sourceEntity->FindComponentOfType<MeshComponent>())
    {
        ApiHandle<Entity> newEntity = *manager->CreateEntity();
        ApiHandle<MeshComponent> newMeshComp = manager->CreateComponent(ObjectType::MeshComponent, newEntity)->as<RemoteRendering::MeshComponent>();
        newMeshComp->SetMesh(meshComp->GetMesh()); // share the mesh
        return newEntity;
    }
    return nullptr;
}
```

## <a name="api-documentation"></a>API-Dokumentation

* [C# Mesh-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.mesh)
* [C# MeshComponent-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.meshcomponent)
* [C++ Mesh-Klasse](https://docs.microsoft.com/cpp/api/remote-rendering/mesh)
* [C++ MeshComponent-Klasse](https://docs.microsoft.com/cpp/api/remote-rendering/meshcomponent)


## <a name="next-steps"></a>Nächste Schritte

* [Materialien](materials.md)
