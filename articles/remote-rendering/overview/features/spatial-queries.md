---
title: Räumliche Abfragen
description: Ausführen von räumlichen Abfragen in einer Szene
author: jakrams
ms.author: jakras
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: 3f808d45197f7d9ee23d3f809a2ab0452e92c20e
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021295"
---
# <a name="spatial-queries"></a>Räumliche Abfragen

Räumliche Abfragen sind Vorgänge, in denen Sie den Remote Rendering-Dienst fragen, welche Objekte sich in einem Bereich befinden. Häufig werden räumliche Abfragen verwendet, um Interaktionen zu implementieren, z. B. um herauszufinden, auf welches Objekt ein Benutzer verweist.

Alle räumlichen Abfragen werden auf dem Server ausgewertet. Demnach handelt es sich um asynchrone Vorgänge, und die Ergebnisse treffen mit einer Verzögerung ein, die von der Netzwerklatenz abhängt. Da jede räumliche Abfrage Netzwerkdatenverkehr generiert, sollten Sie darauf achten, nicht zu viele gleichzeitig auszugeben.

## <a name="collision-meshes"></a>Kollisionsgittermodelle

Räumliche Abfragen werden durch die [Havok](https://www.havok.com/products/havok-physics)-Physik-Engine umgesetzt und erfordern ein vorhandenes dediziertes Kollisionsgittermodell. Standardmäßig werden bei der [Modellkonvertierung](../../how-tos/conversion/model-conversion.md) Kollisionsgittermodelle generiert. Wenn Sie bei einem komplexen Modell keine räumlichen Abfragen benötigen, empfiehlt es sich, die Generierung von Kollisionsgittermodellen in den [Konvertierungsoptionen](../../how-tos/conversion/configure-model-conversion.md) zu deaktivieren, da dies mehrere Auswirkungen hat:

* Die [Modellkonvertierung](../../how-tos/conversion/model-conversion.md) dauert erheblich länger.
* Die Dateigröße der konvertierten Modelle ist deutlich größer und wirkt sich auf die Downloadgeschwindigkeit aus.
* Die Ladezeiten zur Laufzeit sind länger.
* Der CPU-Speicherverbrauch zur Laufzeit ist höher.
* Für jede Modellinstanz gibt es einen geringfügigen Laufzeitaufwand.

## <a name="ray-casts"></a>Raycasts

Bei einem *Raycast* handelt es sich um eine räumliche Abfrage, bei der die Runtime überprüft, welche Objekte von einem Strahl geschnitten werden, der an einer angegebenen Position beginnt und in eine bestimmte Richtung weist. Als Optimierung wird auch eine maximale Strahlendistanz angegeben, damit nicht nach Objekten gesucht wird, die zu weit entfernt sind.

```cs
async void CastRay(AzureSession session)
{
    // trace a line from the origin into the +z direction, over 10 units of distance.
    RayCast rayCast = new RayCast(new Double3(0, 0, 0), new Double3(0, 0, 1), 10);

    // only return the closest hit
    rayCast.HitCollection = HitCollectionPolicy.ClosestHit;

    RayCastHit[] hits = await session.Actions.RayCastQueryAsync(rayCast).AsTask();

    if (hits.Length > 0)
    {
        var hitObject = hits[0].HitObject;
        var hitPosition = hits[0].HitPosition;
        var hitNormal = hits[0].HitNormal;

        // do something with the hit information
    }
}
```

```cpp
void CastRay(ApiHandle<AzureSession> session)
{
    // trace a line from the origin into the +z direction, over 10 units of distance.
    RayCast rayCast;
    rayCast.StartPos = { 0, 0, 0 };
    rayCast.EndPos = { 0, 0, 1 };
    rayCast.MaxHits = 10;

    // only return the closest hit
    rayCast.HitCollection = HitCollectionPolicy::ClosestHit;

    ApiHandle<RaycastQueryAsync> castQuery = *session->Actions()->RayCastQueryAsync(rayCast);

    castQuery->Completed([](const ApiHandle<RaycastQueryAsync>& async)
    {
        std::vector<RayCastHit> hits = *async->Result();

        if (hits.size() > 0)
        {
            auto hitObject = hits[0].HitObject;
            auto hitPosition = hits[0].HitPosition;
            auto hitNormal = hits[0].HitNormal;

            // do something with the hit information
        }
    });

}
```


Es gibt drei Treffererfassungsmodi:

* **`Closest`:** In diesem Modus wird nur der nächste Treffer gemeldet.
* **`Any`:** Diesen Modus sollten Sie bevorzugen, wenn Sie nur wissen möchten, *ob* ein Strahl etwas treffen würde, aber das genaue getroffene Objekt nicht relevant ist. Diese Abfrage kann erheblich einfacher ausgewertet werden, hat aber auch nur wenige Anwendungsfälle.
* **`All`:** In diesem Modus werden alle Treffer entlang des Strahls gemeldet, sortiert nach Entfernung. Verwenden Sie diesen Modus nur, wenn Sie wirklich mehr als den ersten Treffer benötigen. Begrenzen Sie die Anzahl der gemeldeten Treffer mit der Option `MaxHits`.

Mit der Komponente [HierarchicalStateOverrideComponent](override-hierarchical-state.md) können Sie Objekte selektiv aus der Berücksichtigung in Raycasts ausschließen.

<!--
The CollisionMask allows the query to consider or ignore some objects based on their collision layer. If an object has layer L, it will be hit only if the mask has bit L set.
It is useful in case you want to ignore objects, for instance when setting an object transparent, and trying to select another object behind it.
TODO : Add an API to make that possible.
-->

### <a name="hit-result"></a>Trefferergebnis

Das Ergebnis einer Raycastabfrage ist ein Array von Treffern. Das Array ist leer, wenn kein Objekt gefunden wurde.

Ein Treffer weist folgende Eigenschaften auf:

* **`HitEntity`:** Die getroffene [Entität](../../concepts/entities.md).
* **`SubPartId`:** Das getroffene *Untergittermodell* in einer [MeshComponent](../../concepts/meshes.md). Kann für eine Indizierung in `MeshComponent.UsedMaterials` und zum Suchen des [Materials](../../concepts/materials.md) an diesem Punkt verwendet werden.
* **`HitPosition`:** Die Raumposition, an der der Strahl das Objekt schneidet.
* **`HitNormal`:** Die Raumoberflächen-Normale des Gittermodells an der Schnittposition.
* **`DistanceToHit`:** Der Abstand von der Anfangsposition des Strahls zum Treffer.

## <a name="next-steps"></a>Nächste Schritte

* [Objektbegrenzungen](../../concepts/object-bounds.md)
* [Außerkraftsetzen hierarchischer Zustände](override-hierarchical-state.md)
