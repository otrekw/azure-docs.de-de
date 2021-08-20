---
title: Versionshinweise zu Azure Video Analyzer – Azure
description: Dieser Artikel enthält Versionshinweise zu Releases, Verbesserungen, Fehlerbehebungen und bekannten Problemen von Azure Video Analyzer.
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: f7189e6bda42ad77cb4bb6f426bf73209f46c363
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2021
ms.locfileid: "114601926"
---
# <a name="azure-video-analyzer-release-notes"></a>Versionshinweise zu Azure Video Analyzer

>Sie können eine Benachrichtigung erhalten, wann auf dieser Seite Updates vorhanden sind, indem Sie die URL `https://docs.microsoft.com/api/search/rss?search=%22Azure+Video+Analyzer+on+IoT+Edge+release+notes%22&locale=en-us` kopieren und in Ihren RSS-Feedreader einfügen.

Dieser Artikel bietet Folgendes:

* Neueste Versionen
* Bekannte Probleme
* Behebung von Programmfehlern
* Veraltete Funktionen

<hr width=100%>

## <a name="june-3-2021"></a>3\. Juni 2021

Das Releasetag für die Aktualisierung des Moduls im Juli 2021 lautet:

```
     mcr.microsoft.com/media/video-analyzer:1.0.1
```
> [!NOTE]
> In den Schnellstarts und Tutorials verwenden die Bereitstellungsmanifeste das Tag „1“ (video-analyzer:1). Daher sollte eine einfache Neubereitstellung solcher Manifeste das Modul auf Ihren Edgegeräten aktualisieren, wenn neuere Tags veröffentlicht werden.

### <a name="module-updates"></a>Modulupdates
* Unterstützt Unicode-Zeichen in den Anmeldeinformationen zum Herstellen einer Verbindung mit einer RTSP-Kamera
* Updates zum Aktivieren detaillierter Protokolle im Debugmodus

<hr width=100%>

## <a name="may-25-2021"></a>25. Mai 2021

Dieses Release ist die erste öffentliche Vorschauversion von Azure Video Analyzer. Das Releasetag ist

```
mcr.microsoft.com/media/video-analyzer:1.0.0
```

> [!NOTE]
> In den Schnellstarts und Tutorials verwenden die Bereitstellungsmanifeste das Tag „1“ (video-analyzer:1). Daher sollte eine einfache Neubereitstellung solcher Manifeste das Modul auf Ihren Edgegeräten aktualisieren, wenn neuere Tags veröffentlicht werden.

### <a name="supported-functionalities"></a>Unterstützte Funktionen

* Analyse von Live-Videostreams mithilfe von KI-Modulen Ihrer Wahl und optionales Aufzeichnen von Videosignalen auf dem Edge-Gerät oder in der Cloud
* Aufzeichnen von Videos zusammen mit Rückschlussmetadaten in der Cloud
* Auslösen von Ereignissen mit Ihrem eigenen Objekterkennungsmodell, wenn Objekte eine Grenze überschreiten
* Nachverfolgen von Objekten, die von Ihrem eigenen Erkennungsmodell erkannt wurden 
* Verwenden von Video Analyzer-Playerwidgets (Webkomponenten), um aufgezeichnete Video- und Rückschlussmetadaten wiederzugeben
* Bereitstellung und Konfiguration des Moduls über den IoT Hub mithilfe des Azure-Portals oder von Visual Studio Code
* Remote- oder Lokalverwaltung von [Pipelinetopologien](pipeline.md#pipeline-topologies) mithilfe [direkter Methodenaufrufe](direct-methods.md)

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](overview.md)
