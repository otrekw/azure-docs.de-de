---
title: Kontingente und Einschränkungen von Live Video Analytics in IoT Edge | Azure
description: In diesem Artikel werden die Kontingente und Einschränkungen von Live Video Analytics in IoT Edge beschrieben.
ms.topic: conceptual
ms.date: 05/22/2020
ms.openlocfilehash: 68c7b91bb1051348b5a8e52f841d443894f0a632
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97400523"
---
# <a name="quotas-and-limitations"></a>Kontingente und Einschränkungen

In diesem Artikel werden die Kontingente und Einschränkungen im Modul Live Video Analytics in IoT Edge aufgelistet.

## <a name="maximum-period-of-disconnected-use"></a>Maximaler Verwendungszeitraum bei getrennter Verbindung

Das Edgemodul kann bei einem zeitlich begrenzten Verlust der Internetverbindung fortgesetzt werden. Wenn das Modul länger als 36 Stunden getrennt bleibt, deaktiviert es alle ausgeführten Graphinstanzen. Alle weiteren direkten Methodenaufrufe werden blockiert.

Wenn Sie das Edgemodul wieder in den Betriebsstatus versetzen möchten, müssen Sie die Internetverbindung wiederherstellen, damit das Modul erfolgreich mit dem Azure Media Services-Konto kommunizieren kann.

## <a name="maximum-number-of-graph-instances"></a>Maximale Anzahl von Diagramminstanzen

Pro Modul werden maximal 1.000 Graphinstanzen unterstützt (erstellt mit GraphInstanceSet).

## <a name="maximum-number-of-graph-topologies"></a>Maximale Anzahl von Diagrammtopologien

Pro Modul werden maximal 50 Graphtopologien unterstützt (erstellt mit GraphTopologySet).

## <a name="limitations-on-graph-topologies-at-preview"></a>Einschränkungen bei Diagrammtopologien in der Vorschauversion

Für die Vorschauversion gelten Einschränkungen für die unterschiedlichen Knoten, die in einer Mediendiagrammtopologie miteinander verbunden werden können.

* RTSP-Quelle
   * Pro Diagrammtopologie ist nur eine RTSP-Quelle zulässig.
* Verarbeitungsknoten für die Bewegungserkennung
   * Muss direkt hinter dem RTSP-Quellknoten angeordnet sein.
   * Darf nicht hinter einem HTTP- oder gRPC-Erweiterungsprozessor angeordnet sein.
* Signalgateprozessor
   * Muss direkt hinter dem RTSP-Quellknoten angeordnet sein.
* Medienobjektsenke 
   * Muss direkt hinter dem RTSP-Quellknoten oder Signalgateprozessor angeordnet sein.
* Dateisenke
   * Muss direkt hinter dem Signalgateprozessor angeordnet sein.
   * Darf nicht direkt hinter einem HTTP- oder gRPC-Erweiterungs- oder -Bewegungserkennungsprozessor angeordnet sein.
* IoT Hub-Senke
   * Darf nicht direkt hinter einer IoT Hub-Quelle angeordnet sein.

## <a name="limitations-on-media-service-operations-at-preview"></a>Einschränkungen bei Media Service-Vorgängen in der Vorschauversion

Während der Vorschauphase unterstützt Live Video Analytics in IoT Edge Folgendes nicht:

* Die Möglichkeit, das Media Services-Konto ohne Unterbrechung von einem Abonnement zu einem anderen zu migrieren
* Die Möglichkeit, mehrere Speicherkonten mit dem Media Services-Konto zu verwenden
* Die Möglichkeit, die Informationen zum Dienstprinzipal in den gewünschten Eigenschaften des Moduls dynamisch zu ändern, ohne dass ein Neustart erforderlich ist

Es können nur IP-Kameras verwendet werden, die das RTSP-Protokoll unterstützen. IP-Kameras, die RTSP unterstützen, finden Sie auf der [Seite mit den ONVIF-konformen Produkten](https://www.onvif.org/conformant-products). Suchen Sie nach Geräten, die mit den Profilen G, S oder T konform sind.

Außerdem sollten diese Kameras für die Verwendung von H.264-Video und AAC-Audio konfiguriert werden. Andere Codecs werden zurzeit nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](overview.md)
