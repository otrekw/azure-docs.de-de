---
title: Tiefenkamera in Azure Kinect DK
description: Informationen zu den Funktionsprinzipien und wichtigsten Funktionen der Tiefenkamera in Azure Kinect DK.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, SDK, Tiefenkamera, ToF, Prinzipien, Leistung, Invalidierung
ms.openlocfilehash: 22f04b983ed7c6a2ab19a5c1c709621655ee31c0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "85276701"
---
# <a name="azure-kinect-dk-depth-camera"></a>Tiefenkamera in Azure Kinect DK

Auf dieser Seite wird die Verwendung der Tiefenkamera in Azure Kinect DK beschrieben. Die Tiefenkamera ist die zweite der beiden Kameras. Wie in den vorherigen Abschnitten beschrieben, ist die andere Kamera die RGB-Kamera.  

## <a name="operating-principles"></a>Funktionsprinzipien

Die Tiefenkamera in Azure Kinect DK implementiert das Laufzeitverfahren (Time of Flight, ToF) mit CW-Amplitudenmodulation (AMCW). Die Kamera leuchtet die Szene mit moduliertem Licht im Nahinfrarotspektrum (NIR) aus. Anschließend wird eine indirekte Messung der Zeit aufgezeichnet, in der das Licht die Strecke von der Kamera zur Szene und zurück durchquert.

Diese Messungen werden verarbeitet, um eine Tiefenzuordnung zu generieren. Eine Tiefenzuordnung ist eine Gruppe von Z-Koordinatenwerten für jedes Pixel des Bilds gemessen in Millimetereinheiten.

Zusammen mit der Tiefenzuordnung wird auch eine sogenannte saubere IR-Messung erstellt. Der Wert der Pixel in der sauberen IR-Messung ist proportional zu der von der Szene zurückgegebenen Lichtmenge. Das Bild sieht ähnlich wie ein normales IR-Bild aus. In der folgenden Abbildung sind ein Beispiel für eine Tiefenzuordnung (links) und ein entsprechendes sauberes IR-Bild (rechts) dargestellt.

![Tiefenzuordnung und IR-Bild](./media/concepts/depth-camera-depth-ir.png)

## <a name="key-features"></a>Wichtige Features

Technische Merkmale der Tiefenkamera:

- 1-Megapixel-ToF-Bildchip mit fortschrittlicher Pixeltechnologie, die höhere Modulationsfrequenzen und Tiefengenauigkeit ermöglicht
- Zwei NIR-Laserdioden für Tiefenmodi für enges und breites Sichtfeld
- Kleinstes ToF-Pixel der Welt, 3,5 μm × 3,5 μm
- Automatische Verstärkungsauswahl pro Pixel für einen großen dynamischen Bereich, sodass nahe und ferne Objekte sauber erfasst werden können
- Globaler Verschluss für verbesserte Leistung bei Sonnenlicht
- Mehrstufige Tiefenberechnungsmethode, die robuste Genauigkeit auch bei Schwankungen von Chip, Laser und Stromversorgung ermöglicht
- Geringe systematische und zufällige Fehler

![Tiefenmodul](./media/concepts/depth-camera-depth-module.jpg)

Die Tiefenkamera überträgt modulierte IR-Rohdatenbilder an den Host-PC. Auf dem PC wird das Rohdatensignal über die Software des GPU-beschleunigten Tiefenmoduls in Tiefenzuordnungen konvertiert. Die Tiefenkamera unterstützt mehrere Modi. Die Modi für **enges Sichtfeld** sind ideal für Szenen mit kleineren Ausdehnungen in der X- und Y-Dimension, aber größeren Ausdehnungen in der Z-Dimension. Bei Szenen mit größeren X- und Y-Ausdehnungen und kleineren Z-Bereichen sind die Modi für **breites Sichtfeld** besser geeignet.

Die Tiefenkamera unterstützt **2 × 2-Binning-Modi**, sodass der Z-Bereich im Vergleich zu den entsprechenden **Modi ohne Binning** erweitert wird. Das Binning erfolgt auf Kosten der Verringerung der Bildauflösung. Alle Modi können mit bis zu 30 Bildern pro Sekunde (BpS) ausgeführt werden, mit Ausnahme des 1-Megapixel-Modus (MP) mit einer maximalen Bildfrequenz von 15 BpS. Die Tiefenkamera umfasst außerdem einen **passiven IR-Modus**. In diesem Modus sind die Beleuchtungseinheiten in der Kamera nicht aktiv, und es wird nur die Umgebungsbeleuchtung berücksichtigt.

## <a name="camera-performance"></a>Kameraleistung

Die Leistung der Kamera wird anhand von systematischen und zufälligen Fehlern gemessen.

### <a name="systematic-error"></a>Systematischer Fehler

Ein systematischer Fehler ist definiert als die Differenz zwischen der gemessenen Tiefe nach der Rauschunterdrückung und der korrekten Tiefe (Ground Truth). Der zeitliche Mittelwert wird über viele Einzelbilder einer statischen Szene berechnet, um Tiefenrauschen so weit wie möglich zu unterdrücken. Genauer gesagt ist der systematische Fehler wie folgt definiert:

![Systematischer Fehler bei der Tiefe](./media/concepts/depth-camera-systematic-error.png)

*d <sub>t</sub>* gibt die gemessene Tiefe zum Zeitpunkt *t* an, *N* entspricht der Anzahl der Bilder, die für die Mittelwertbildung verwendet werden, und *d <sub>gt</sub>* gibt den Ground Truth-Wert der Tiefe an.

Bei der Spezifikation der systematischen Fehler der Tiefenkamera wird die Mehrweginterferenz nicht berücksichtigt. Mehrweginterferenz tritt auf, wenn ein Sensorpixel Licht integriert, das von mehreren Objekten reflektiert wird. Die Mehrweginterferenz wird in der Tiefenkamera durch höhere Modulationsfrequenzen sowie die Tiefeninvalidierung (weiter unten beschrieben) teilweise gemindert.

### <a name="random-error"></a>Zufälliger Fehler

Angenommen, es werden 100 Bilder eines Objekts aufgenommen, ohne die Kamera zu bewegen. Die Tiefe des Objekts weicht in jedem der 100 Bilder geringfügig ab. Dieser Unterschied wird durch Schrotrauschen verursacht. Schrotrauschen gibt die Anzahl der auf den Sensor auftreffenden Photonen an, die mit der Zeit um einen Zufallsfaktor variiert. Dieser zufällige Fehler in einer statischen Szene wird als Standardabweichung der Tiefe im zeitlichen Verlauf definiert und wie folgt berechnet:

![Zufälliger Fehler bei der Tiefe](./media/concepts/depth-camera-random-error.png)

*N* gibt die Anzahl der Tiefenmessungen an, *d <sub>t</sub>* stellt die Tiefenmessung zum Zeitpunkt *t* dar, und *d* gibt den berechneten Mittelwert für alle Tiefenmessungen *d <sub>t</sub>* an.

## <a name="invalidation"></a>Invalidierung

In bestimmten Fällen generiert die Tiefenkamera möglicherweise keine richtigen Werte für einige Pixel. In diesen Fällen werden die Tiefenpixel ungültig gemacht. Ungültige Pixel werden durch den Tiefenwert „0“ angegeben. Mögliche Gründe dafür, dass die Tiefenkamera keine korrekten Werte generiert:

- Außerhalb der aktiven IR-Beleuchtungsmaske
- Gesättigtes IR-Signal
- Niedriges IR-Signal
- Filterausreißer
- Mehrweg-Interferenz

### <a name="illumination-mask"></a>Beleuchtungsmaske

Pixel werden ungültig gemacht, wenn sie sich außerhalb der aktiven IR-Beleuchtungsmaske befinden. Es wird davon abgeraten, das Signal dieser Pixel zur Berechnung der Tiefe zu verwenden. In der folgenden Abbildung ist ein Beispiel für die Invalidierung in Bezug auf die Beleuchtungsmaske dargestellt. Die ungültig gemachten Pixel sind die schwarzen Pixel außerhalb des Kreises in den Modi für breites Sichtfeld (links) und außerhalb des Sechsecks in den Modi für enges Sichtfeld (rechts).

![Invalidierung aufgrund von Pixeln außerhalb der Beleuchtungsmaske](./media/concepts/depth-camera-invalidation-illumination-mask.png)

### <a name="signal-strength"></a>Signalstärke

Pixel werden ungültig gemacht, wenn sie ein gesättigtes IR-Signal enthalten. Wenn Pixel gesättigt sind, gehen die Phaseninformationen verloren. In der folgenden Abbildung ist ein Beispiel für die Invalidierung aufgrund eines gesättigten IR-Signals dargestellt. Der Pfeil im Tiefenbild und im IR-Bild zeigt jeweils auf die Beispielpixel.

![Invalidierung aufgrund von Sättigung](./media/concepts/depth-camera-invalidation-saturation.png)

Die Invalidierung kann auch erfolgen, wenn das IR-Signal nicht stark genug ist, um Tiefe zu generieren. In der folgenden Abbildung ist ein Beispiel für die Invalidierung aufgrund eines niedrigen IR-Signals dargestellt. Die Pfeile im Tiefenbild und im IR-Bild zeigen jeweils auf die Beispielpixel.

![Invalidierung aufgrund eines niedrigen IR-Signals](./media/concepts/depth-camera-invalidation-low-signal.png)

### <a name="ambiguous-depth"></a>Mehrdeutige Tiefe

Pixel können auch ungültig gemacht werden, wenn Signale von mehreren Objekten in der Szene empfangen werden. Dieser Fall tritt häufig bei Ecken auf.  Aufgrund der Szenengeometrie wird das IR-Licht der Kamera der einen Wand an die andere Wand reflektiert. Das reflektierte Licht führt zur Mehrdeutigkeit in der gemessenen Tiefe des Pixels. Filter im Tiefenalgorithmus erkennen diese mehrdeutigen Signale und machen die Pixel ungültig.

In den nachfolgenden Abbildungen sind Beispiele für Invalidierung aufgrund von Mehrweginterferenz dargestellt. Sie können auch feststellen, dass eine Fläche, die für eine Kameraansicht ungültig gemacht wurde (Aufsicht), in einer anderen Kameraansicht wieder angezeigt werden kann (Untersicht). In der Abbildung unten wird veranschaulicht, dass die für eine Perspektive ungültig gemachten Flächen in einer anderen Perspektive sichtbar sein können.

![Invalidierung aufgrund von Mehrweginterferenz – Ecken](./media/concepts/depth-camera-invalidation-multipath.png)

Ein weiterer häufiger Fall von Mehrweginterferenz sind Pixel, die das gemischte Signal von Vordergrund und Hintergrund enthalten (z. B. Bereiche an Objektkanten). Im Zeitraffer sind möglicherweise zusätzliche ungültig gemachte Pixel an Kanten zu erkennen. Die zusätzlichen ungültig gemachten Pixel sind auf das Belichtungsintervall bei der Erfassung der Tiefenrohdaten zurückzuführen.

![Invalidierung aufgrund von Mehrweginterferenz – Kanten](./media/concepts/depth-camera-invalidation-edge.png)

## <a name="next-steps"></a>Nächste Schritte

[Koordinatensysteme](coordinate-systems.md)
