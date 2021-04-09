---
title: Übersicht über das Runtime-SDK
description: Machen Sie sich mit dem Runtime-SDK für Object Anchors vertraut.
author: craigktreasure
manager: vriveras
services: azure-object-anchors
ms.author: crtreasu
ms.date: 03/02/2021
ms.topic: conceptual
ms.service: azure-object-anchors
ms.openlocfilehash: 551374824610c0257aaf52c45768d31849026524
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105047540"
---
# <a name="runtime-sdk-overview"></a>Übersicht über das Runtime-SDK

Dieser Abschnitt enthält eine allgemeine Übersicht über das Runtime-SDK für Object Anchors, das zur Objekterkennung mithilfe eines Object Anchors-Modells verwendet wird. Sie erhalten einen Einblick in die Darstellung eines Objekts und die Verwendung der verschiedenen Komponenten.

Die im Folgenden beschriebenen Typen finden Sie im Namespace **Microsoft.MixedReality.ObjectAnchors**.

## <a name="types"></a>Typen

### <a name="objectmodel"></a>ObjectModel

Eine [ObjectModel](/dotnet/api/microsoft.azure.objectanchors.objectmodel)-Klasse stellt die Geometrie eines physischen Objekts dar und codiert die erforderlichen Parameter für die Erkennung und die Schätzung der räumlichen Lage. Sie muss mithilfe des [Object Anchors-Diensts](../quickstarts/get-started-model-conversion.md) erstellt werden. Die generierte Modelldatei kann dann von einer Anwendung mithilfe der Object Anchors-API geladen, und das im Modell eingebettete Gittermodell kann zur Visualisierung abgefragt werden.

### <a name="objectsearcharea"></a>ObjectSearchArea

Eine [ObjectSearchArea](/dotnet/api/microsoft.azure.objectanchors.objectsearcharea)-Klasse gibt den Bereich an, in dem nach einem oder mehreren Objekten gesucht werden soll. Sie wird von der Knoten-ID eines Raumdiagramms und den räumlichen Begrenzungen in dem Koordinatensystem definiert, für das diese ID steht. Das Runtime-SDK für Object Anchors unterstützt vier Begrenzungsarten: **Sichtfeld**, **Begrenzungsrahmen**, **Kugel** und **Standort**.

### <a name="objectquery"></a>ObjectQuery

Eine [ObjectQuery](/dotnet/api/microsoft.azure.objectanchors.objectquery)-Klasse teilt einem **Objektobserver** mit, wie die Objekte eines bestimmten Modells gefunden werden können. Sie stellt die folgenden anpassbaren Parameter bereit, deren Parameter von einem Objektmodell abgerufen werden können.

#### <a name="minsurfacecoverage"></a>MinSurfaceCoverage

Die Eigenschaft [MinSurfaceCoverage](/dotnet/api/microsoft.azure.objectanchors.objectquery.minsurfacecoverage) gibt den Wert an, bei dem eine Instanz als erkannt gelten soll.

Für jeden Objektkandidaten berechnet ein **Observer** das Verhältnis der überlappenden Oberflächen zwischen dem transformierten Objektmodell und der Szene. Dieser Kandidat wird dann nur an die Anmeldung weitergegeben, wenn das Deckungsverhältnis den festgelegten Schwellenwert übersteigt.

#### <a name="isexpectedtobestandingongroundplane"></a>IsExpectedToBeStandingOnGroundPlane

Die Eigenschaft [IsExpectedToBeStandingOnGroundPlane](/dotnet/api/microsoft.azure.objectanchors.objectquery.isexpectedtobestandingongroundplane) gibt an, ob erwartet wird, dass das Zielobjekt auf der Bodenfläche steht.

Die Bodenfläche ist die niedrigste horizontale Fläche im Suchbereich. Sie stellt eine gute Einschränkung für mögliche Objektlagen dar. Wenn Sie dieses Flag aktivieren, wird der **Observer** angewiesen, die räumliche Lage in einem eingeschränkten Bereich zu schätzen. Dadurch kann die Genauigkeit verbessert werden. Dieser Parameter wird ignoriert, wenn das Modell nicht auf der Bodenfläche stehen soll.

#### <a name="expectedmaxverticalorientationindegrees"></a>ExpectedMaxVerticalOrientationInDegrees

Die Eigenschaft [ExpectedMaxVerticalOrientationInDegrees](/dotnet/api/microsoft.azure.objectanchors.objectquery.expectedmaxverticalorientationindegrees) gibt den erwarteten maximalen Winkel in Grad an, der zwischen der Aufwärtsrichtung einer Objektinstanz und der Schwerkraft erwartet wird.

Dieser Parameter stellt eine weitere Einschränkung bei der Aufwärtsrichtung einer geschätzten räumlichen Lage dar. Wenn ein Objekt aufrecht steht, kann dieser Parameter beispielsweise 0 sein. Object Anchors ist nicht darauf ausgelegt, Objekte zu erkennen, die von diesem Modell abweichen. Wenn das Modell aufrecht ist, wird eine seitlich liegende Instanz nicht erkannt. Für das seitlich liegende Layout müsste ein neues Modell verwendet werden. Die gleiche Regel gilt für die Artikulation.

#### <a name="maxscalechange"></a>MaxScaleChange

Die Eigenschaft [MaxScaleChange](/dotnet/api/microsoft.azure.objectanchors.objectquery.maxscalechange) gibt die maximale Änderung am Objektmaßstab (zwischen 0 und 1) in Bezug auf die räumliche Zuordnung an. Der geschätzte Maßstab wird auf transformierte Objekteckpunkte angewendet, die auf den Nullpunkt zentriert und auf die Achsen ausgerichtet sind. Geschätzte Maßstäbe entsprechen möglicherweise nicht dem tatsächlichen Maßstab zwischen einem CAD-Modell und dem physischen Äquivalent, sondern liefern der App einige Werte, mit der ein Objektmodell in der Nähe der räumlichen Zuordnung für das physische Modell gerendert werden kann.

#### <a name="searchareas"></a>SearchAreas

Die Eigenschaft [SearchAreas](/dotnet/api/microsoft.azure.objectanchors.objectquery.searchareas) gibt ein Array räumlicher Begrenzungen an, in denen Objekte gefunden werden können.

Der **Observer** sucht nach Objekten in der Schnittmenge aller Suchbereiche, die in einer Abfrage angegeben werden. In diesem Release wird nur das Objekt mit der höchsten Zuverlässigkeit zurückgegeben, um die Latenz niedrig zu halten.

### <a name="objectinstance"></a>ObjectInstance

Eine [ObjectInstance](/dotnet/api/microsoft.azure.objectanchors.objectinstance)-Klasse stellt eine hypothetische Position dar, in der sich eine Instanz eines bestimmten Modells im HoloLens-Koordinatensystem befinden könnte. Jede Instanz enthält die Eigenschaft `SurfaceCoverage`, um anzugeben, wie gut die räumliche Lage geschätzt wurde.

Eine Instanz wird durch einen Aufruf der Methode `ObjectObserver.DetectAsync` erstellt und dann automatisch im Hintergrund aktualisiert, solange sie aktiv ist. Eine Anwendung kann dem Ereignis für die Statusveränderung in Instanzen lauschen oder den Nachverfolgungsmodus ändern, um Updates anzuhalten oder fortzusetzen. Wenn keine Nachverfolgung möglich ist, wird eine Instanz automatisch aus dem **Observer** entfernt.

### <a name="objectobserver"></a>ObjectObserver

Eine [ObjectObserver](/dotnet/api/microsoft.azure.objectanchors.objectobserver)-Klasse lädt Objektmodelle, erkennt ihre Instanzen und meldet die räumliche Lage jeder Instanz im HoloLens-Koordinatensystem nach den sechs Freiheitsgraden (6-DoF, Six Degrees of Freedom).

Obwohl alle Objektmodelle und Instanzen aus einem **Observer** generiert werden, ist deren Lebensdauer voneinander unabhängig. Eine Anwendung kann den Observer löschen und das Objektmodell oder die Instanz weiterhin verwenden.

### <a name="objectdiagnosticssession"></a>ObjectDiagnosticsSession

Die [ObjectDiagnosticSession](/dotnet/api/microsoft.azure.objectanchors.diagnostics.objectdiagnosticssession)-Klasse zeichnet Diagnosedaten auf und schreibt diese in ein Archiv.

Ein Diagnosearchiv enthält die Punktwolke für die Szene, den Observerstatus und Informationen zu den Modellen. Diese Informationen sind nützlich, um mögliche Runtimeprobleme zu identifizieren. Weitere Informationen finden Sie in den [häufig gestellten Fragen](../faq.md).

## <a name="runtime-sdk-usage-and-details"></a>Verwendung und Details zum Runtime-SDK

In diesem Abschnitt wird die Verwendung des Runtime-SDK grundlegend erläutert. Die Informationen sollten ausreichen, damit Sie die Beispielanwendungen durchgehen und die ganzheitliche Verwendung von Object Anchors nachvollziehen können.

### <a name="initialization"></a>Initialisierung

Anwendungen müssen zunächst die API `ObjectObserver.IsSupported()` aufrufen, um zu bestimmen, ob Object Anchors auf dem Gerät unterstützt wird. Wenn die API `ObjectObserver.IsSupported()` den Wert `false` zurückgibt, sollten Sie überprüfen, ob die Option **spatialPerception** in der Anwendung aktiviert ist und/oder ein Upgrade auf das neueste HoloLens-Betriebssystem durchführen.

```cs
if (!ObjectObserver.IsSupported())
{
    // Handle the error
}

// This call should grant the access we need.
var status = await ObjectObserver.RequestAccessAsync();
if(status != ObjectObserverStatus.Allowed)
{
    // Handle the error
}
```

Als Nächstes erstellt die Anwendung einen Objektobserver und lädt die vom [Object Anchors-Modellkonvertierungsdienst](../quickstarts/get-started-model-conversion.md) generierten erforderlichen Modelle.

```cs
var observer = new ObjectObserver();

byte[] modelAsBytes; // Load a model into a byte array. Model could be a file, an embedded resource, or a network stream.
var model = await observer.LoadObjectModelAsync(modelAsBytes);

// Note that after a model is loaded, its vertices and normals are transformed into a centered coordinate system for the 
// ease of computing the object pose. The rigid transform can be retrieved through the `OriginToCenterTransform` property.
```

Die Anwendung erstellt eine Abfrage, um Instanzen dieses Modells in einem Bereich zu erkennen.

```cs
var coordinateSystem = default(SpatialGraphCoordinateSystem);
var searchAreaAsBox = ObjectSearchArea.FromOrientedBox(coordinateSystem, default(SpatialOrientedBox));

// Optionally change the parameters, otherwise use the default values embedded in the model.
var query = new ObjectQuery(model);
query.MinSurfaceCoverage = 0.2f;
query.ExpectedMaxVerticalOrientationInDegrees = 1.5f;
query.MaxScaleChange = 0.1f;
query.SearchAreas.Add(searchAreaAsBox);

// Detection could take long, run in a background thread.
var detectedObjects = await observer.DetectAsync(query);
```

Standardmäßig wird jede erkannte Instanz vom **Observer** automatisch nachverfolgt. Bei Bedarf können Sie diese Instanzen bearbeiten, indem Sie den Nachverfolgungsmodus ändern oder dem Ereignis für die Statusveränderung lauschen.

```cs
foreach (var instance in detectedObjects)
{
    // Supported modes:
    // "LowLatencyCoarsePosition" - consumes less CPU cycles thus fast to update the state.
    // "HighLatencyAccuratePosition" - (not yet implemented) consumes more CPU cycles thus potentially taking longer time to update the state.
    // "Paused" - stops to update the state until mode changed to low or high.
    instance.Mode = ObjectInstanceTrackingMode.LowLatencyCoarsePosition;

    // Listen to state changed event on this instance.
    instance.Changed += InstanceChangedHandler;

    // Optionally dispose an instance if not interested in it.
    //instance.Dispose();
}
```

Im Ereignis für die Statusveränderung können Sie den aktuellen Status abfragen oder Instanzen löschen, wenn die Nachverfolgung nicht mehr möglich ist.

```cs
var InstanceChangedHandler = new Windows.Foundation.TypedEventHandler<ObjectInstance, ObjectInstanceChangedEventArgs>((sender, args) =>
{
    // Try to query the current instance state.
    var state = sender.TryGetCurrentState();

    if (state.HasValue)
    {
        // Process latest state via state.Value.
        // An object pose includes scale, rotation and translation, applied in the same order
        // to the object model in the centered coordinate system.
    }
    else
    {
        // This object instance is lost for tracking, and will never be recovered.
        // The caller can detach the Changed event handler from this instance and dispose it.
        sender.Dispose();
    }
});
```

Außerdem kann eine Anwendung optional eine oder mehrere Diagnosesitzungen für das Offlinedebuggen aufzeichnen.

```cs
string diagnosticsFolderPath = Windows.Storage.ApplicationData.Current.TemporaryFolder.Path;
const uint maxSessionSizeInMegaBytes = uint.MaxValue;

// Recording starts on the creation of a diagnostics session.
var diagnostics = new ObjectDiagnosticsSession(observer, maxSessionSizeInMegaBytes);

// Wait for the observer to do a job.

// Application can report some **pseudo ground-truth** pose for an instance acquired from other means.
diagnostics.ReportActualInstanceLocation(instance, coordinateSystem, Vector3.Zero, Quaternion.Identity);

// Close a session and write the diagnostics into an archive at specified location.
await diagnostics.CloseAsync(System.IO.Path.Combine(diagnosticsFolderPath, "diagnostics.zip"));
```

Schließlich geben Sie die Ressourcen frei, indem alle Objekte gelöscht werden.

```cs
foreach(var instance in activeInstances)
{
    instance.Changed -= InstanceChangedHandler;
    instance.Dispose();
}

model.Dispose();
observer.Dispose();
```