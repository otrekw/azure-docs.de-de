---
title: Ungefähre Standortbestimmung
description: Erfahren Sie, wann und wie die ungefähre Standortbestimmung ausgeführt wird. Mit der ungefähren Standortbestimmung können Sie Ankerpunkte in Ihrer Nähe suchen.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 01/28/2021
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.custom: devx-track-csharp
ms.openlocfilehash: 3f0b04183c4df469d4f723486103790c4f97671b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104656174"
---
# <a name="coarse-relocalization"></a>Ungefähre Standortbestimmung

Die ungefähre Standortbestimmung ist ein Feature für die umfangreiche Lokalisierung, das eine ungefähre, aber schnelle Antwort auf die folgenden Fragen gibt: 
- *Wo befindet sich zurzeit mein Gerät?* 
- *Welchen Inhalt sollte ich beobachten?* 
 
Die Antwort ist nicht präzise. Es ist in dieser Form: *Sie befinden sich in der Nähe dieser Anker. Versuchen Sie, einen von ihnen zu finden*.

Bei der ungefähren Standortbestimmung werden Anker mit verschiedenen Messungen geräteinterner Sensoren getaggt, die später für schnelle Abfragen dienen. Bei Szenarios im Freien handelt es sich bei den Sensordaten üblicherweise um die GPS-Position des Geräts. Wenn GPS nicht verfügbar oder unzuverlässig ist (beispielsweise in Gebäuden), bestehen die Sensordaten aus den WLAN-Zugangspunkten und Bluetooth-Beacons in Reichweite. Alle gesammelten Sensordaten werden in einen räumlichen Index aufgenommen, mit dem Azure Spatial Anchors schnell ermitteln kann, welche Ankerpunkte sich in der Nähe Ihres Geräts befinden.

## <a name="when-to-use-coarse-relocalization"></a>Anwendungsfälle der ungefähren Standortbestimmung

Wenn Sie mehr als 35 Raumanker in einem Raum größer als ein Tennisplatz planen, empfiehlt sich wahrscheinlich die räumliche Indexierung der ungefähren Standortbestimmung.

Die durch die ungefähre Standortbestimmung ermöglichte schnelle Suche nach Ankern ist so konzipiert, dass sie die Entwicklung von Anwendungen vereinfacht, die von weltweiten Sammlungen von Millionen geografisch verteilten Ankerpunkten unterstützt werden. Die Komplexität der räumlichen Indexierung ist vollkommen verborgen, damit Sie sich voll und ganz auf Ihre Anwendungslogik konzentrieren können. Alle schwierigen Aufgaben werden im Hintergrund durch Azure Spatial Anchors durchgeführt.

## <a name="using-coarse-relocalization"></a>Verwenden der ungefähren Standortbestimmung

Der typische Workflow zum Erstellen und Abfragen von Azure Spatial Anchors mit der ungefähren Standortbestimmung lautet wie folgt:
1.  Erstellen und Konfigurieren eines Sensorfingerabdruckanbieters, um die gewünschten Sensordaten zu erfassen
2.  Starten einer Azure Spatial Anchors-Sitzung und Erstellen der Anker. Da der Sensorfingerabdruck aktiviert ist, werden die Anker räumlich von der ungefähren Standortbestimmung indiziert.
3.  Abfragen der umliegenden Anker mit der ungefähren Standortbestimmung mittels der dedizierten Suchkriterien in der Spatial Anchors-Sitzung

In den folgenden Tutorials finden Sie Informationen zur Einrichtung der ungefähren Standortbestimmung für Ihre Anwendung:
* [Ungefähre Standortbestimmung in Unity](../how-tos/set-up-coarse-reloc-unity.md)
* [Ungefähre Standortbestimmung in Objective-C](../how-tos/set-up-coarse-reloc-objc.md)
* [Ungefähre Standortbestimmung in Swift](../how-tos/set-up-coarse-reloc-swift.md)
* [Ungefähre Standortbestimmung in Java](../how-tos/set-up-coarse-reloc-java.md)
* [Ungefähre Standortbestimmung in C++/NDK](../how-tos/set-up-coarse-reloc-cpp-ndk.md)
* [Ungefähre Standortbestimmung in C++/WinRT](../how-tos/set-up-coarse-reloc-cpp-winrt.md)

## <a name="sensors-and-platforms"></a>Sensoren und Plattformen

### <a name="platform-availability"></a>Plattformverfügbarkeit

Die folgenden Typen von Sensordaten können an den Ankerdienst gesendet werden:

* GPS-Position: Breitengrad, Längengrad und Höhe
* Signalstärke von WLAN-Zugangspunkten in Reichweite
* Signalstärke von Bluetooth-Beacons in Reichweite

Die folgende Tabelle bietet eine Zusammenfassung der Verfügbarkeit der Sensordaten auf unterstützten Plattformen und ergänzende Informationen, die Sie beachten sollten:

|                 | HoloLens | Android | iOS |
|-----------------|----------|---------|-----|
| **GPS**         | Nein<sup>1</sup>  | Ja<sup>2</sup> | Ja<sup>3</sup> |
| **WLAN**        | Ja<sup>4</sup> | Ja<sup>5</sup> | Nein  |
| **BLE-Beacons** | Ja<sup>6</sup> | Ja<sup>6</sup> | Ja<sup>6</sup>|


<sup>1</sup> Ein externes GPS-Gerät kann mit HoloLens verknüpft werden. Wenden Sie sich an [unseren Support](../spatial-anchor-support.md), wenn Sie HoloLens mit einem GPS-Tracker verwenden möchten.<br/>
<sup>2</sup> Wird von [LocationManager][3]-APIs unterstützt (sowohl GPS als auch NETWORK)<br/>
<sup>3</sup> Wird von [CLLocationManager][4]-APIs unterstützt<br/>
<sup>4</sup> Wird mit einer Häufigkeit von etwa einer Überprüfung alle drei Sekunden unterstützt <br/>
<sup>5</sup> Ab API-Ebene 28 werden WLAN-Überprüfungen auf vier Aufrufe alle zwei Minuten gedrosselt. Ab Android 10 können Sie diese Drosselung im Menü **Entwickleroptionen** deaktivieren. Weitere Informationen finden Sie in der [Android-Dokumentation][5].<br/>
<sup>6</sup> Beschränkt auf [Eddystone][1] und [iBeacon][2]

### <a name="which-sensor-to-enable"></a>Der zu aktivierende Sensor

Die Auswahl des Sensors ist abhängig von der Plattform und der Anwendung, die Sie entwickeln.
Das folgende Diagramm bietet eine Orientierungshilfe zum Bestimmen der Kombination von Sensoren, die je nach Lokalisierungsszenario aktiviert werden können:

![Diagramm, in dem aktivierte Sensoren für verschiedene Szenarien gezeigt werden](media/coarse-relocalization-enabling-sensors.png)

In den folgenden Abschnitten erhalten Sie weitere Einblicke in die Vorteile und Einschränkungen der einzelnen Sensortypen.

### <a name="gps"></a>GPS

GPS ist die Standardoption für Outdoorszenarios.
Wenn Sie GPS in Ihrer Anwendung verwenden, sollten Sie beachten, dass die von der Hardware bereitgestellten Messwerte üblicherweise:

* asynchron sind und eine niedrige Frequenz aufweisen (weniger als 1 Hz).
* unzuverlässig bzw. ungenau sind (durchschnittliche Standardabweichung von 7 m).

Sowohl das Betriebssystem des Geräts als auch Spatial Anchors führen Filterung und Extrapolierung des unformatierten GPS-Signals durch, um diese Probleme zu umgehen. Diese zusätzliche Verarbeitung erfordert Zeit für die Annäherung. Zum Erzielen der besten Ergebnisse sollten Sie Folgendes versuchen:

* Erstellen Sie den Anbieter für den Fingerabdrucksensor in Ihrer Anwendung so früh wie möglich.
* Sorgen Sie dafür, dass der Anbieter für den Fingerabdrucksensor zwischen mehreren Sitzungen beibehalten wird.
* Verwenden Sie den Anbieter für den Fingerabdrucksensor über mehrere Sitzungen hinweg.

GPS-Geräte für Verbraucher sind in der Regel ungenau. in einer Studie von [Zandenbergen und Barbeau (2011)][6] wurde eine durchschnittliche Genauigkeit von etwa 7 Metern bei Mobiltelefonen mit GPS (A-GPS) berichtet. Dieser Wert ist zu groß, als dass man ihn ignorieren könnte. Zur Berücksichtigung dieser Messfehler behandelt der Dienst Anker als Wahrscheinlichkeitsverteilungen im GPS-Bereich. Daher bezeichnet ein Anker den Bereich eines Standorts, der höchstwahrscheinlich (mit einer Konfidenz von mehr als 95 %) die tatsächliche, unbekannte GPS-Position enthält.

Die gleiche Logik gilt für Abfragen mit GPS. Das Gerät wird als weiterer räumlicher Konfidenzbereich um die tatsächliche, unbekannte GPS-Position dargestellt. Die Ermittlung von Ankerpunkten in der Nähe bedeutet also, dass Ankerpunkte mit Konfidenzbereichen gefunden werden, die sich wie im folgenden Diagramm gezeigt *nahe genug* am Konfidenzbereich des Geräts befinden:

![Diagramm der Suche nach Ankerkandidaten per GPS](media/coarse-reloc-gps-separation-distance.png)

### <a name="wi-fi"></a>WLAN

In Innenräumen kann das WLAN-Signal eine gute Option für die ungefähre Standortbestimmung von HoloLens- und Android-Geräten sein.
Sein Vorteil ist die potenzielle sofortige Verfügbarkeit von WLAN-Zugriffspunkten (kommt z. B. häufig in Büroräumen oder Einkaufszentren vor), ohne dass ein zusätzliches Setup erforderlich ist.

> [!NOTE]
> iOS bietet keine API zum Lesen der WLAN-Signalstärke, sodass unter diesem Betriebssystem nicht die ungefähre Standortbestimmung per WLAN verwendet werden kann.

Wenn Sie WLAN in Ihrer Anwendung verwenden, sollten Sie beachten, dass die von der Hardware bereitgestellten Messwerte üblicherweise:

* asynchron sind und eine niedrige Frequenz aufweisen (weniger als 0,1 Hz).
* möglicherweise auf Betriebssystemebene gedrosselt werden.
* unzuverlässig bzw. ungenau sind (durchschnittliche Standardabweichung von 3 dBm).

Spatial Anchors versucht während einer Sitzung, eine gefilterte Karte der WLAN-Signalstärke zu erstellen, um diese Probleme zu umgehen. Gehen Sie für optimale Ergebnisse folgendermaßen vor:

* Erstellen Sie die Sitzung, bevor Sie den ersten Anker platzieren.
* Behalten Sie die Sitzung so lange wie möglich bei (d. h., erstellen Sie alle Anker und Abfragen innerhalb einer Sitzung).

### <a name="bluetooth-beacons"></a>Bluetooth-Beacons
<a name="beaconsDetails"></a>

Die umsichtige Bereitstellung von Bluetooth-Beacons ist eine gute Lösung für die ungefähre Standortbestimmung in groß angelegten Innenräumen, in denen kein GPS-Signal empfangen wird oder das Signal ungenau ist. Es ist zudem die einzige Methode für Innenbereiche, die unter allen drei Plattformen unterstützt wird.

Beacons sind meist vielseitige Geräte, bei denen alles, einschließlich UUIDs und MAC-Adressen, konfiguriert werden kann. Azure Spatial Anchors erwartet, dass Beacons durch ihre UUIDs eindeutig identifizierbar sind. Wenn Sie diese Eindeutigkeit nicht sicherstellen, erhalten Sie wahrscheinlich falsche Ergebnisse. Optimale Ergebnisse erzielen Sie wie folgt:

* Weisen Sie Ihren Beacons eindeutige UUIDs zu.
* Stellen Sie die Beacons gleichmäßig im Raum verteilt bereit, sodass von jedem Standpunkt aus mindestens drei Beacons erreichbar sind.
* Übergeben Sie die Liste der eindeutigen Beacon-UUIDs an den Anbieter des Sensorfingerabdrucks.

Funksignale, z. B.Bluetooth-Signale, werden von physischen Hindernissen unterbrochen und können andere Funksignale stören. Deshalb kann es schwierig einzuschätzen sein, ob der Raum einheitlich abgedeckt ist. Für eine höhere Kundenfreundlichkeit wird empfohlen, die Abdeckung der Beacons selbst manuell zu testen. Sie können einen Test durchführen, indem Sie mit Kandidatengeräten und einer Anwendung, die die Bluetooth-Signale in Reichweite anzeigt, durch den Raum wandern. Vergewissern Sie sich beim Testen der Signalabdeckung, dass Sie von jeder strategischen Position im Raum mindestens drei Beacons erreichen können. Wenn zu viele Beacons vorhanden sind, können diese sich gegenseitig stören. Viele Beacons führen also nicht notwendigerweise zu einer höheren Genauigkeit der ungefähren Standortbestimmung.

Bluetooth-Beacons bieten in der Regel eine Abdeckung von 80 Metern, wenn der Raum keine Hindernisse enthält.
In einem Raum ohne große Hindernisse können Beacons also alle 40 Meter in einem Raster bereitgestellt werden.

Ein Beacon mit niedrigem Akkustand beeinträchtigt die Ergebnisse. Überwachen Sie die Bereitstellung daher in regelmäßigen Abständen auf niedrige Akkustände oder entladene Akkus.

Azure Spatial Anchors verfolgt nur Bluetooth-Beacons, die in der Liste bekannter Beacon-UUIDs aufgeführt werden. Schädliche Beacons können jedoch so programmiert sein, dass sie ebenfalls zugelassene UUIDs aufweisen, und sich negativ auf die Dienstqualität auswirken. Daher erzielen Sie die besten Ergebnisse in kuratierten Räumen, in denen Sie die Bereitstellung von Beacons kontrollieren können.

### <a name="sensor-accuracy"></a>Sensorgenauigkeit

Sowohl bei der Erstellung von Ankern als auch bei Abfragen hat die Genauigkeit des GPS-Signals einen großen Einfluss auf den Satz der zurückgegebenen Anker. Im Gegensatz dazu berücksichtigen Abfragen, die auf WLAN oder Beacons basieren, alle Anker mit mindestens einem Zugangspunkt oder Beacon, der mit der Abfrage übereinstimmt. In diesem Sinne wird das Ergebnis einer Abfrage, die auf WLAN oder Beacons basiert, größtenteils von der physischen Reichweite der Zugangspunkte oder Beacons und Hindernissen in der Umgebung beeinflusst.
In der folgenden Tabelle wird der erwartete Suchbereich für jeden Sensortyp geschätzt:

| Sensor      | Suchbereichsradios (ungefähr) | Details |
|-------------|:-------:|---------|
| **GPS**         | 20 m bis 30 m | Wird unabhängig von anderen Faktoren durch GPS bestimmt. Die gemeldeten Werte werden für die durchschnittliche GPS-Genauigkeit von Mobiltelefonen mit A-GPS geschätzt: 7 Meter. |
| **WLAN**        | 50 m bis 100 m | Wird von der Reichweite der WLAN-Zugangspunkte bestimmt. Wird von der Frequenz, Signalstärke, physischen Hindernissen, Interferenzen usw. beeinträchtigt. |
| **BLE-Beacons** |  70 m | Wird von der Reichweite des Beacons bestimmt. Wird von der Frequenz, Signalstärke, physischen Hindernissen, Interferenzen usw. beeinträchtigt. |

<!-- Reference links in article -->
[1]: https://developers.google.com/beacons/eddystone
[2]: https://developer.apple.com/ibeacon/
[3]: https://developer.android.com/reference/android/location/LocationManager
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc
[5]: https://developer.android.com/guide/topics/connectivity/wifi-scan
[6]: https://www.cambridge.org/core/journals/journal-of-navigation/article/positional-accuracy-of-assisted-gps-data-from-highsensitivity-gpsenabled-mobile-phones/E1EE20CD1A301C537BEE8EC66766B0A9
