---
title: 'Versionshinweise zu Live Video Analytics in IoT Edge: Azure'
description: Dieses Thema enthält Versionshinweise zu Releases, Verbesserungen, Fehlerbehebungen und bekannten Problemen von Video Analytics in IoT Edge.
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 23c2362d46a0b9bfe2fc2fbe31e00d0a42f80cfa
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055446"
---
# <a name="live-video-analytics-on-iot-edge-release-notes"></a>Versionshinweise zu Live Video Analytics in IoT Edge

>Sie können eine Benachrichtigung erhalten, wann auf dieser Seite Updates vorhanden sind, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Live+Video+Analytics+on+IoT+Edge+release+notes%22&locale=en-us` kopieren und in Ihren RSS-Feedreader einfügen.

Dieser Artikel bietet Folgendes:

* Neueste Versionen
* Bekannte Probleme
* Behebung von Programmfehlern
* Veraltete Funktionen

## <a name="august-19-2020"></a>19. August 2020

Dieses Releasetag für die Aktualisierung des Moduls vom August 2020 lautet:

```
mcr.microsoft.com/media/live-video-analytics:1.0.3
```

> [!NOTE]
> In den Schnellstartanleitungen und Tutorials verwenden die Bereitstellungsmanifeste das Tag 1 (live-video-analytics:1). Daher sollte eine einfache erneute Bereitstellung solcher Manifeste das Modul auf Ihren Edgegeräten aktualisieren.

## <a name="new-features"></a>Neue Funktionen 

* Sie können jetzt mithilfe des gRPC-Frameworks eine hohe Leistung für die Übertragung von Dateninhalten zwischen Live Video Analytics in IoT Edge und Ihrer benutzerdefinierten Erweiterung erzielen. Informationen zum Einstieg finden Sie [hier](analyze-live-video-use-your-grpc-model-quickstart.md).
* Umfassendere regionale Bereitstellung von Live Video Analytics, wobei nur der Clouddienst aktualisiert wurde.  
* Live Video Analytics ist jetzt in 25 weiteren Regionen auf der ganzen Welt verfügbar. Hier finden Sie eine [Liste](https://azure.microsoft.com/global-infrastructure/services/?products=media-services) mit diesen Regionen.  
* Auch die [Einrichtung](https://aka.ms/lva-edge/setup-resources-for-samples) für Schnellstarts wurde mit Unterstützung neuer Regionen aktualisiert.
    * Es gibt keinen Handlungsaufruf für diejenigen, die Ressourcen bereits eingerichtet haben.

## <a name="bug-fixes"></a>Behebung von Programmfehlern 

* Veraltete Azure-Erweiterung im Einrichtungsskript muss nicht mehr verwendet werden.

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
