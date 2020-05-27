---
title: Entitäten
description: Definition von Entitäten im Bereich der Azure Remote Rendering-API
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 7981a28db23ab8c0aed05013dd260ffd97a11c07
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758723"
---
# <a name="entities"></a>Entitäten

Eine *Entität* stellt ein bewegliches Objekt im Raum dar und ist der grundlegende Baustein von remote gerendertem Inhalt.

## <a name="entity-properties"></a>Entitätseigenschaften

Entitäten verfügen über eine Transformation, die durch eine Position, eine Drehung und einen Maßstab definiert ist. Entitäten an sich besitzen keine beobachtbare Funktionalität. Stattdessen wird Verhalten durch Komponenten hinzugefügt, die an Entitäten angefügt werden. Beispielsweise wird durch das Anfügen einer [CutPlaneComponent](../overview/features/cut-planes.md) eine Schnittebene an der Position der Entität erstellt.

Der wichtigste Aspekt der Entität selbst ist die Hierarchie und die sich ergebende hierarchische Transformation. Wenn mehrere Entitäten beispielsweise als untergeordnete Elemente einer freigegebenen übergeordneten Entität angefügt werden, können alle diese Entitäten durch Ändern der Transformation der übergeordneten Entität verschoben, gedreht und gemeinsam skaliert werden.

Eine Entität befindet sich im Besitz ihres übergeordneten Elements. Wenn das übergeordnete Element mit `Entity.Destroy()` zerstört wird, gilt dies daher auch für die untergeordneten Elemente und alle verbundenen [Komponenten](components.md). Daher wird das Entfernen eines Modells aus der Szene erreicht, indem `Destroy` für den Stammknoten eines Modells aufgerufen wird, das von `AzureSession.Actions.LoadModelAsync()` oder dessen SAS-Variante `AzureSession.Actions.LoadModelFromSASAsync()` zurückgegeben wird.

Entitäten werden erstellt, wenn der Server Inhalte lädt oder der Benutzer der Szene ein Objekt hinzufügen möchte. Wenn ein Benutzer beispielsweise eine Schnittebene hinzufügen möchte, um das Innere eines Gittermodells visuell darzustellen, kann der Benutzer eine Entität erstellen, in der die Ebene vorhanden sein sollte, und ihr dann die Schnittebenenkomponente hinzufügen.

## <a name="query-functions"></a>Abfragefunktionen

Es gibt zwei Arten von Abfragefunktionen für Entitäten: synchrone und asynchrone Aufrufe. Synchrone Abfragen können nur für Daten verwendet werden, die auf dem Client vorhanden sind und für die keine größeren Berechnungen durchgeführt werden. Beispiele sind Abfragen für Komponenten, relative Objekttransformationen oder Beziehungen zwischen übergeordneten und untergeordneten Elementen. Asynchrone Abfragen werden für Daten verwendet, die nur auf dem Server vorhanden sind oder zusätzliche Berechnungen erfordern, deren Ausführung auf dem Client zu teuer ist. Beispiele hierfür sind Abfragen für räumliche Begrenzungen oder Metadatenabfragen.

### <a name="querying-components"></a>Abfragen von Komponenten

Um eine Komponente eines bestimmten Typs zu suchen, verwenden Sie `FindComponentOfType`:

```cs
CutPlaneComponent cutplane = (CutPlaneComponent)entity.FindComponentOfType(ObjectType.CutPlaneComponent);

// or alternatively:
CutPlaneComponent cutplane = entity.FindComponentOfType<CutPlaneComponent>();
```

```cpp
ApiHandle<CutPlaneComponent> cutplane = entity->FindComponentOfType(ObjectType::CutPlaneComponent)->as<CutPlaneComponent>();

// or alternatively:
ApiHandle<CutPlaneComponent> cutplane = *entity->FindComponentOfType<CutPlaneComponent>();
```

### <a name="querying-transforms"></a>Abfragen von Transformationen

Transformationsabfragen sind synchrone Aufrufe für das Objekt. Es ist wichtig zu beachten, dass Transformationen, die durch die API abgefragt werden, lokale Raumtransformationen relativ zum übergeordneten Element des Objekts sind. Ausnahmen sind Stammobjekte, für die der lokale Raum und der allgemeine Raum identisch sind.

> [!NOTE]
> Es gibt keine dedizierte API, um die allgemeine Raumtransformation von beliebigen Objekten abzufragen.

```cs
// local space transform of the entity
Double3 translation = entity.Position;
Quaternion rotation = entity.Rotation;
```

```cpp
// local space transform of the entity
Double3 translation = *entity->Position();
Quaternion rotation = *entity->Rotation();
```


### <a name="querying-spatial-bounds"></a>Abfragen räumlicher Begrenzungen

Begrenzungsabfragen sind asynchrone Aufrufe, die für eine vollständige Objekthierarchie ausgeführt werden und eine Entität als Stamm verwenden. Weitere Informationen finden Sie im speziellen Kapitel zu [Objektbegrenzungen](object-bounds.md).

### <a name="querying-metadata"></a>Abfragen von Metadaten

Metadaten sind zusätzliche Daten, die für Objekte gespeichert und vom Server ignoriert werden. Objektmetadaten sind im Wesentlichen ein Satz aus Name-Wert-Paaren, wobei der _Wert_ vom Typ „numeric“ (numerisch), „boolean“ (boolesch) oder „string“ (Zeichenfolge) sein kann. Metadaten können mit dem Modell exportiert werden.

Metadatenabfragen sind asynchrone Aufrufe für eine bestimmte Entität. Die Abfrage gibt nur die Metadaten einer einzelnen Entität zurück, nicht die zusammengeführten Informationen eines Untergraphen.

```cs
MetadataQueryAsync metaDataQuery = entity.QueryMetaDataAsync();
metaDataQuery.Completed += (MetadataQueryAsync query) =>
{
    if (query.IsRanToCompletion)
    {
        ObjectMetaData metaData = query.Result;
        ObjectMetaDataEntry entry = metaData.GetMetadataByName("MyInt64Value");
        System.Int64 intValue = entry.AsInt64;

        // ...
    }
};
```

```cpp
ApiHandle<MetadataQueryAsync> metaDataQuery = *entity->QueryMetaDataAsync();
metaDataQuery->Completed([](const ApiHandle<MetadataQueryAsync>& query)
    {
        if (query->IsRanToCompletion())
        {
            ApiHandle<ObjectMetaData> metaData = *query->Result();
            ApiHandle<ObjectMetaDataEntry> entry = *metaData->GetMetadataByName("MyInt64Value");
            int64_t intValue = *entry->AsInt64();

            // ...
        }
    });
```

Die Abfrage ist selbst dann erfolgreich, wenn das Objekt keine Metadaten enthält.

## <a name="next-steps"></a>Nächste Schritte

* [Komponenten](components.md)
* [Objektbegrenzungen](object-bounds.md)
