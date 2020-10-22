---
title: Debuggen des Renderings
description: Übersicht über die Auswirkungen des serverseitigen Debuggens des Renderings
author: jumeder
ms.author: jumeder
ms.date: 06/15/2020
ms.topic: article
ms.custom: devx-track-csharp
ms.openlocfilehash: 7924409a1760d59f4ecc88a736f7b8238fbd647b
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92205862"
---
# <a name="debug-rendering"></a>Debuggen des Renderings

Die API für das Debuggen des Renderings verfügt über globale Optionen für das serverseitige Rendering, die unterschiedliche Auswirkungen auf das Debuggen haben.

## <a name="available-debug-rendering-effects"></a>Verfügbare Auswirkungen beim Debuggen des Renderings

|Einstellung                          | Wirkung                               |
|---------------------------------|:-------------------------------------|
|Frame counter (Framezähler)                    | Rendert eine Textüberlagerung oben links im Frame. Im Text ist die aktuelle serverseitige Frame-ID enthalten, die während des Renderings fortlaufend inkrementiert wird. |
|Polygon count (Polygonanzahl)                    | Rendert eine Textüberlagerung oben links im Frame. Der Text enthält die derzeit gerenderte Anzahl von Polygonen. Dies ist der gleiche Wert wie bei [Abfragen zur serverseitigen Leistung](performance-queries.md).| 
|Wireframe (Drahtmodell)                        | Wenn diese Option aktiviert ist, wird die gesamte auf dem Server geladene Objektgeometrie im Drahtmodellmodus gerendert. In diesem Modus werden nur die Kanten von Polygonen gerastert. |

Mit dem folgenden Code werden diese Auswirkungen für das Debuggen aktiviert:

```cs
void EnableDebugRenderingEffects(AzureSession session, bool highlight)
{
    DebugRenderingSettings settings = session.Actions.DebugRenderingSettings;

    // Enable frame counter text overlay on the server side rendering
    settings.RenderFrameCount = true;

    // Enable polygon count text overlay on the server side rendering
    settings.RenderPolygonCount = true;

    // Enable wireframe rendering of object geometry on the server
    settings.RenderWireframe = true;
}
```

```cpp
void EnableDebugRenderingEffects(ApiHandle<AzureSession> session, bool highlight)
{
    ApiHandle<DebugRenderingSettings> settings = session->Actions()->GetDebugRenderingSettings();

    // Enable frame counter text overlay on the server side rendering
    settings->SetRenderFrameCount(true);

    // Enable polygon count text overlay on the server side rendering
    settings->SetRenderPolygonCount(true);

    // Enable wireframe rendering of object geometry on the server
    settings->SetRenderWireframe(true);
}
```

![Debuggen des Renderings](./media/debug-rendering.png)

> [!NOTE]
> Alle Auswirkungen beim Debuggen des Renderings sind globale Einstellungen, die für den gesamten Frame gelten.

## <a name="use-cases"></a>Anwendungsfälle

Die API für das Debuggen des Renderings ist für einfache Debugaufgaben gedacht, z. B. die Überprüfung, ob die Dienstverbindung wirklich aktiv ist und richtig ausgeführt wird. Die Optionen für das Rendern von Text wirken sich direkt auf die gestreamten Videoframes aus. Bei ihrer Aktivierung wird überprüft, ob neue Frames korrekt empfangen werden und die Videodecodierung dafür durchgeführt wird.

Die bereitgestellten Auswirkungen ermöglichen aber keine ausführlichen Erkenntnisse zur Integrität des Diensts. Für diesen Anwendungsfall werden die [serverseitigen Leistungsabfragen](performance-queries.md) empfohlen.

## <a name="performance-considerations"></a>Überlegungen zur Leistung

* Die Aktivierung der Textüberlagerungen führt – wenn überhaupt – nur zu einem sehr geringen zusätzlichen Leistungsaufwand.
* Bei der Aktivierung des Drahtmodellmodus kommt es jedoch zu einem nicht trivialen Mehraufwand bei der Leistung, der aber je nach Szene variiert. Bei komplexen Szenen kann es in diesem Modus zu einem Abfall der Bildfrequenz unter den Zielwert von 60 Hz kommen.

## <a name="api-documentation"></a>API-Dokumentation

* [C++ RemoteManager::DebugRenderingSettings()](/cpp/api/remote-rendering/remotemanager#debugrenderingsettings)

## <a name="next-steps"></a>Nächste Schritte

* [Renderingmodi](../../concepts/rendering-modes.md)
* [Serverseitige Leistungsabfragen](performance-queries.md)