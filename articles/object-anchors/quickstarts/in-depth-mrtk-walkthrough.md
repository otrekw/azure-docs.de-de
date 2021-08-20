---
title: 'Schnellstart: Ausführliche exemplarische Vorgehensweise zum MRTK'
description: In dieser Schnellstartanleitung erhalten Sie einen ausführlichen Überblick über die Azure Object Anchors MRTK Unity-Beispielanwendung
author: RamonArguelles
manager: virivera
ms.author: rgarcia
ms.date: 07/12/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 515d44cc7cb40972c67424799163bc57deba9182
ms.sourcegitcommit: b59e0afdd98204d11b7f9b6a3e55f5a85d8afdec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2021
ms.locfileid: "114371577"
---
# <a name="quickstart-in-depth-mrtk-walkthrough"></a>Schnellstart: Ausführliche exemplarische Vorgehensweise zum MRTK

In dieser Anleitung erhalten Sie einen ausführlichen Überblick über die [Azure Object Anchors MRTK Unity-Beispielanwendung](get-started-unity-hololens-mrtk.md). Sie soll Ihnen einen Einblick in den Entwurf des Beispiels bieten. Durch das Lesen dieses Leitfadens können Entwickler ihr Verständnis für die wichtigsten Azure Object Anchors in dem Beispiel beschleunigen.

## <a name="project-layout"></a>Projekt-Layout

Ressourcen, die für das MRTK Azure Object Anchors Unity-Beispiel erstellt wurden, werden in `Assets\MixedReality.AzureObjectAnchors` gespeichert. Folgende Unterordner sind verfügbar:

- **Symbole**
  - Dieser Unterordner enthält einige benutzerdefinierte Symbole, die im gegenüberliegenden Benutzermenü verwendet werden.
- **Materialien**
  - Enthält Shader und Materialien für die Visualisierung der Oberflächenrekonstruktion und einen Shader *Nur für die Tiefe* der in den Tiefenpuffer schreibt, um die Hologramm-Stabilität in der Textumgebung zu unterstützen.
- **Prefabs**
  - Enthält eine wiederverwendbare Unity `GameObjects`. Insbesondere stellt `TrackableObjectPrefab` das Objekt dar, das erstellt wird, wenn Azure Object Anchors ein Objekt erkennt.
- **Profiles**
  - Enthält benutzerdefinierte MRTK-Profile, die die minimal erforderliche Funktionalität von MRTK beschreiben, um die Anwendung zu aktivieren.
- **Szenen**
  - Enthält die `AOASampleTestScene`, die die primäre Szene im Beispiel ist.
- **Skripts**
  - Enthält die Skripts, die für das Beispiel geschrieben wurden.

## <a name="unity-scene"></a>Unity-Szene

**Mixed Reality Play Space** –  Meistens MRTK-Bausteine

- <a href="/windows/mixed-reality/develop/unity/mrtk-getting-started" target="_blank">Einführung in das MRTK für Unity</a>.
- An die Kamera ist eine Benutzeroberfläche angefügt, die den detaillierten Gesamtstatus des Azure Object Anchors observiert (siehe `OverlayDebugText.cs`).

**Objekt Mixed Reality Play Space** – größtenteils in Zusammenhang mit Azure Object Anchors, aber auch mit einigen MRTK-Steuerelementen. Zwei Skripts, `TrackableObjectSearch` und `ObjectTracker`, die an das übergeordnete Element angefügt sind, stellen die primäre Schnittstelle mit Azure Object Anchors dar.

- **Menü**
  - In erster Linie MRTK-Code, aber die Interaktionen der Benutzeroberfläche sind auf die Azure Object Anchors-Funktionalität ausgerichtet.
  - Das angefügte `TrackableObjectMenu`-Skript übernimmt die primäre Aufgabe, die MRTK-Benutzeroberflächenereignisse an die entsprechenden Azure Object Anchors weiterzuleiten.
  - <a href="/windows/mixed-reality/design/hand-menu" target="_blank">MRTK-Handmenü</a>.
- **WorkspaceBoundingBox**
  - Enthält MRTK-Skripts, die dem Steuern eines Begrenzungsfelds zugeordnet sind.
  - Es ist auch ein untergeordnetes Objekt `ModelVis` enthalten, das zum Visualisieren des Azure Object Anchors-Modells verwendet wird, bevor eine Erkennung aufgetreten ist, um die Ausrichtung bei komplizierten Erkennungen zu unterstützen.

## <a name="menu-walkthrough"></a>Exemplarische Vorgehensweise im Menü

Bevor wir uns mit den Skripts befassen, lassen Sie uns zunächst die Menüelemente durchgehen. Auf diese Weise können wir darauf verweisen, wie diese Menüelemente mit den Skripts interagieren.

:::image type="content" source="./media/mrtk-menus.png" alt-text="MRTK-Menüs":::

Die unteren und rechten Untermenüs werden nicht automatisch angezeigt, sondern mit `Search Area Settings` bzw. `Tracker Settings` umschaltet.

### <a name="main-menu"></a>Hauptmenü

- **Suche starten**
  - Es beginnt die Suche nach Objekten im angegebenen Suchbereich.
- **Umschalten der räumlichen Zuordnung**
  - Zyklen zwischen der Anzeige der räumlichen Zuordnung während der Suche, zeigt immer die räumlichen Zuordnung und niemals räumlichen Zuordnung.
- **Tracker Einstellungen/Suchbereich-Einstellungen**
  - Umschalten in die entsprechenden Untermenüs.
- **Ablaufverfolgung starten/Ablaufverfolgung beenden**
  - Beginnt oder beendet die Diagnoseablaufverfolgung.
- **Hochladen Ablaufverfolgung**
  - Lädt die Diagnoseablaufverfolgung zur Debuganalyse an Microsoft hoch.

### <a name="tracker-settings"></a>Nachverfolgungseinstellungen

- **Hohe Genauigkeit**
  - Wenn diese Option aktiviert ist, wird `ObjectInstanceTrackingMode` auf `HighLatencyAccuratePosition` festgelegt.
  - Wenn diese Option deaktiviert ist, wird `ObjectInstanceTrackingMode` auf `LowLatencyCoarsePosition` festgelegt.
- **Gelockerte Vertikale Ausrichtung**
  - Wenn diese Option aktiviert ist, wird `AllowedVerticalOrientationInDegrees` auf 10 Grad festgelegt. Dieses Funktion ermöglicht die Erkennung von Objekten, die sich auf Rampen befinden.
  - Wenn diese Option deaktiviert ist, wird `AllowedVerticalOrientationInDegrees` auf 0 Grad festgelegt.
- **Zulassen von Skalierungsänderung**
  - Wenn diese Option aktiviert ist, wird `MaxScaleChange` auf 0,1 festgelegt. Dieses Funktion ermöglicht Azure Object Anchors, die Skalierung des Objekts basierend auf den Anpassungen der HoloLens-Nachverfolgung anzupassen.
  - Wenn diese Option deaktiviert ist, wird `MaxScaleChange` auf 0 festgelegt.
- **Schieberegler "Abdeckungsverhältnis"**
  - Passt das Verhältnis der Abdeckung an, das für die Objekterkennung erforderlich ist, um eine Übereinstimmung zu berücksichtigen. Niedrigere Werte erhöhen die Erkennungshäufigkeit. Diese Funktion kann für schwer zu erkennende Objekte wünschenswert sein, kann aber auch zu einer erhöhten Erkennung falsch-positiver Objekte führen.

### <a name="search-area-settings"></a>Suchbereichseinstellungen

- **Sperrsuchbereich**
  - Wenn diese Option aktiviert ist, wird verhindert, dass der Benutzer den Suchbereich ändert.
- **Suchbereich automatisch anpassen**
  - Wenn diese Option aktiviert ist, können Skripts den Suchbereich verschieben, um den Erkennungsprozess zu verfeinern.
- **Zyklusnetz**
  - Durchläuft die Gitternetze für `.ou` Objekte, die erkannt werden können, und auch ohne Gitternetz.

## <a name="scripts"></a>Skripts

- **AutonomousSearchArea.cs**
  - Dieses Skript ist an `WorkspaceBoundingBox` angefügt. Das Skript versucht, zu skalieren und `WorkspaceBoundingBox` automatisch zu platzieren. Es wird aktiviert, wenn `Auto-Adjust Search Area` aktiviert ist.
- **ObjectAnchorsSubscription.cs**
  - Dieses Skript umschließt die Informationen, die zum Hochladen von Diagnosedaten erforderlich sind.
- **ObjectTracker.cs**
  - Dieses Skript ist eine Brücke zwischen Unity und den Erkennungsaspekten des Azure Object Anchors SDK.
- **ObjectTrackerDiagnostics.cs**
  - Dieses Skript verwaltet das Diagnosefeature des Azure Object Anchors SDK.
- **OverlayDebugText.cs**
  - Dieses Skript ist an die Hauptkamera angefügt. Es ist dafür verantwortlich, den Gesamtzustand des Beispiels anzuzeigen und dem Benutzer die Azure Object Anchors anzuzeigen.
- **PositionDebugInfo.cs**
  - Dieses Skript erzwingt lediglich, dass dem Benutzer der an ein erkanntes Objekt angefügte Debugtext angezeigt wird.
- **SearchAreaController.cs**
  - Dieses Skript verwaltet den Zustand des `WorkspaceBoundingBox`, das verwendet wird, um anzugeben, wo Azure Object Anchors nach Objekten suchen sollen.
- **SearchAreaModelVisualization.cs**
  - Dieses Skript aktiviert die `Cycle Mesh` Funktionalität unter `Search Area Settings`.
- **SpatialMappingController.cs**
  - Dieses Skript verwaltet, wann die räumliche Zuordnung basierend auf der Interaktion mit `Toggle Spatial Mapping` unter `Main Menu` aktiviert werden soll.
- **TextToSpeech.cs**
  - Dieses Skript verwendet einen Text und setzt ihn in eine Sprache um.
- **TrackableObjectData.cs**
  - Dieses Skript stellt die Daten für Objekte dar, die nachverfolgt werden können.
- **TrackableObjectDataLoader.cs**
  - Mit diesem Skript werden die `.ou` Dateien geladen und in `TrackableObjectData` Elemente umgearbeitet.
- **TrackableObjectMenu.cs**
  - Die meisten Benutzerinteraktionen fließen von der Benutzeroberfläche zu diesem Skript und dann zum entsprechenden Skript. Beispielsweise, wenn `TrackableObjectMenu` über `ToggleSpatialMapping` verfügt, das an `SpatialMappingController` geroutet wird.
- **TrackableObjectSearch.cs**
  - Dieses Skript führt eine geringe Verwaltung des Suchbereichs durch. Insbesondere die Interaktion, bei der der Benutzer in den Raum tippt und das Suchfeld vor dem Benutzer platziert wird. Außerdem wird das Innere des Suchfelds während der Suche oder während der Erkennung eines Objekts nicht gerendert, um zu verhindern, dass das vollständige Rendering das Objekt einblendet.
- **TrackedObject.cs**
  - Ein Primäres Skript auf der `TrackableObjectPrefab`. Es behält den Visualisierungszustand eines Objekts bei, das von der Azure Object Anchors erkannt wurde.
- **TrackedObjectData.cs**
  - Dieses Skript enthält Informationen zu einem Objekt, das von Azure Object Anchors derzeit nachverfolgt wird.

## <a name="prefabs"></a>Prefabs

- **TrackableObjectPrefab**
  - Das Prefab, das erstellt wird, wenn ein Objekt von AzureObjectAnchors erkannt wird. Ein untergeordnetes Objekt dieses Prefabs, `LogicalCenter`, stellt die Mitte des Objekts dar und ist ein geeigneter Startort, wenn versucht wird, untergeordneten Inhalt an erkannte Objekte anfügen. Der im Beispiel angezeigte Informationstext wird an diesen logischen Mittelpunkt angefügt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte: SDK-Übersicht](../concepts/sdk-overview.md)

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen](../faq.md)

> [!div class="nextstepaction"]
> [Konvertierungs-SDK](/dotnet/api/overview/azure/mixedreality.objectanchors.conversion-readme-pre)
