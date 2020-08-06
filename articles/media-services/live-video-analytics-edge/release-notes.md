---
title: 'Versionshinweise zu Live Video Analytics in IoT Edge: Azure'
description: Dieses Thema enthält Versionshinweise zu Releases, Verbesserungen, Fehlerbehebungen und bekannten Problemen von Video Analytics in IoT Edge.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 28260728532d9db52b8d36488c2e456bd11803ea
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091778"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>Versionshinweise zu Live Video Analytics in IoT Edge

>Sie können eine Benachrichtigung erhalten, wann auf dieser Seite Updates vorhanden sind, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us` kopieren und in Ihren RSS-Feedreader einfügen.

Dieser Artikel bietet Folgendes:

* Neueste Versionen
* Bekannte Probleme
* Behebung von Programmfehlern
* Veraltete Funktionen

## <a name="july-13-2020"></a>13. July 2020

Dieses Releasetag für die Aktualisierung des Moduls im Juli 2020 lautet:

```
     mcr.microsoft.com/media/live-video-analytics:1.0.2
```

> [!NOTE]
> In den Schnellstartanleitungen und Tutorials verwenden die Bereitstellungsmanifeste das Tag 1 (live-video-analytics:1). Daher sollte eine einfache erneute Bereitstellung solcher Manifeste das Modul auf Ihren Edgegeräten aktualisieren.

### <a name="new-features"></a>Neue Funktionen
* Sie können jetzt Graphtopologien erstellen, die sowohl einen Knoten der Medienobjektsenke als auch einen Knoten der Dateisenke hinter einem Signalgate-Prozessorknoten aufweisen. Ein Beispiel finden Sie [hier](https://github.com/Azure/live-video-analytics/tree/master/MediaGraph/topologies/evr-motion-assets-files).

### <a name="bug-fixes"></a>Behebung von Programmfehlern
* Verbesserungen bei der Validierung gewünschter Eigenschaften

## <a name="june-1-2020"></a>1\. Juni 2020

Dieses Release ist erste öffentliche Vorschauversion von Live Video Analytics in IoT Edge. Das Releasetag ist

```
     mcr.microsoft.com/media/live-video-analytics:1.0.0
```

### <a name="supported-functionalities"></a>Unterstützte Funktionen
* Analyse von Live-Videostreams mithilfe von KI-Modulen Ihrer Wahl und optionales Aufzeichnen von Videosignalen auf dem Edge-Gerät oder in der Cloud
* Ausführung unter den von IoT Edge [unterstützten](../../iot-edge/support.md) AMD64-Linux-Betriebssystemen
* Bereitstellung und Konfiguration des Moduls über den IoT Hub mithilfe des Azure-Portals oder von Visual Studio Code
* Verwalten von [Graphtopologien und Graphinstanzen](media-graph-concept.md#media-graph-topologies-and-instances) remote oder lokal mithilfe der folgenden Aufrufe direkter Methoden

    *   GraphTopologyGet
    *   GraphTopologySet
    *   GraphTopologyDelete
    *   GraphTopologyList
    *   GraphInstanceGet
    *   GraphInstanceSet
    *   GraphInstanceDelete
    *   GraphInstanceList


## <a name="next-steps"></a>Nächste Schritte

[Übersicht](overview.md)
