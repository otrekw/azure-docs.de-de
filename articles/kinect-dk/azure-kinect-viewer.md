---
title: Azure Kinect Viewer
description: Erfahren Sie, wie Sie im Azure Kinect Viewer alle Datenströme des Geräts visualisieren.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Azure, Kinect, Sensor, Viewer, Visualisierung, Tiefe, RGB, Farbe, IMU, Audiodaten, Mikrofon, Punktwolke
ms.openlocfilehash: 57cf7df831e97da4143a7f196b69a3a10609a017
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276677"
---
# <a name="azure-kinect-viewer"></a>Azure Kinect Viewer

Der Azure Kinect Viewer, im Verzeichnis der installierten Tools zu finden als `k4aviewer.exe` (z. B. `C:\Program Files\Azure Kinect SDK vX.Y.Z\tools\k4aviewer.exe`, wobei `X.Y.Z` die installierte Version des SDK ist), kann zum Visualisieren aller Datenströme des Geräts für Folgendes verwendet werden:

* Überprüfen, ob die Sensoren ordnungsgemäß funktionieren
* Positionieren des Geräts
* Testen der Kameraeinstellungen
* Lesen der Gerätekonfiguration
* Wiedergeben von mit dem [Azure Kinect-Rekorder](azure-kinect-recorder.md) erstellten Aufzeichnungen

Weitere Informationen zum Azure Kinect Viewer erhalten Sie im [Video zur Verwendung von Azure Kinect](https://www.microsoft.com/videoplayer/embed/RE3hNwG).

Der Azure Kinect Viewer ist ein [Open-Source-Tool](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/tools/k4aviewer) und kann als Beispiel für die Verwendung der APIs verwendet werden.

## <a name="use-viewer"></a>Verwenden des Viewers

Der Viewer kann in zwei Modi ausgeführt werden: mit Livedaten des Sensors oder mit aufgezeichneten Daten ([Azure Kinect-Rekorder](azure-kinect-recorder.md)).

### <a name="start-application"></a>Starten der Anwendung

Starten Sie die Anwendung durch Ausführen von `k4aviewer.exe`.

![Überprüfen der Firmwareversion des Geräts mit dem Viewer](./media/how-to-guides/open-viewer.png)

### <a name="use-the-viewer-with-live-data"></a>Verwenden des Viewers mit Livedaten

1. Wählen Sie im Abschnitt **Open Device** (Gerät öffnen) die Seriennummer (**Serial Number**) des zu öffnenden Geräts aus. Wählen Sie dann **Refresh** (Aktualisieren) aus, wenn das Gerät nicht vorhanden ist.
2. Wählen Sie die Schaltfläche **Open Device** (Gerät öffnen) aus.
3. Wählen Sie **Start** (Starten) aus, um das Streamen von Daten mit den Standardeinstellungen zu starten.

### <a name="use-the-viewer-with-recorded-data"></a>Verwenden des Viewers mit aufgezeichneten Daten

Navigieren Sie im Abschnitt **Open Recording** (Aufzeichnung öffnen) zu der aufgezeichneten Datei, und wählen Sie sie aus.

## <a name="check-device-firmware-version"></a>Überprüfen der Firmwareversion des Geräts

Zeigen Sie im Konfigurationsfenster wie in der folgenden Abbildung gezeigt die Firmwareversion des Geräts an.

![Überprüfen der Firmwareversion des Geräts mit dem Viewer](./media/how-to-guides/check-firmware-update.png)

In diesem Fall wird für die Tiefenkamera die Firmwareversion 1.5.63 ausgeführt.

## <a name="depth-camera"></a>Tiefenkamera

Im Viewer der Tiefenkamera werden zwei Fenster angezeigt:

* Im ersten Fenster *Active Brightness* (Aktive Helligkeit) wird ein Graustufenbild mit IR-Helligkeit angezeigt.
* Im zweiten Fenster *Depth* (Tiefe) wird eine farbige Darstellung der Tiefendaten angezeigt.

Zeigen Sie mit dem Mauszeiger auf die Pixel im Fenster „Depth“ (Tiefe), um wie unten gezeigt den Wert des Tiefensensors anzuzeigen.

![Tiefenansicht](./media/how-to-guides/depth-camera.png)

## <a name="rgb-camera"></a>RGB-Kamera

In der folgenden Abbildung ist die Ansicht der Farbkamera dargestellt.

![RGB-Ansicht](./media/how-to-guides/viewer-rgb-camera.png)

Sie können die Einstellungen der RGB-Kamera während des Streamings im Konfigurationsfenster steuern.

![Steuerelemente der RGB-Kamera](./media/how-to-guides/rgb-camera-settings.png)

## <a name="inertial-measurement-unit-imu"></a>Inertiale Messeinheit (Inertial Measurement Unit, IMU)

Das IMU-Fenster enthält zwei Komponenten: einen Beschleunigungsmesser und ein Gyroskop.

In der oberen Hälfte wird der Beschleunigungsmesser mit der linearen Beschleunigung in Meter/Sekunde-Quadrat (m/s<sup>2</sup>) angezeigt.  Sie beinhaltet die Gravitationsbeschleunigung. Wenn das Gerät flach auf einem Tisch liegt, wird für die Z-Achse daher vermutlich etwa –9,8 m/s<sup>2</sup> angezeigt.

In der unteren Hälfte wird das Gyroskop mit der Drehbewegung in Radiant/Sekunde angezeigt.

![Ansicht des Bewegungssensors](./media/how-to-guides/viewer-mu-settings.png)

## <a name="microphone-input"></a>Mikrofoneingabe

In der Mikrofonansicht wird eine Darstellung der über jedes Mikrofon gehörten Töne angezeigt. Wenn kein Ton vorhanden ist, wird der Graph leer angezeigt. Andernfalls wird eine dunkelblaue Wellenform mit überlagerter hellblauer Wellenform angezeigt.

Die dunkelblaue Welle stellt die minimalen und maximalen Werte für das Mikrofon in einem bestimmten Zeitsegment dar. Die hellblaue Welle stellt den quadratischen Mittelwert der Werte für das Mikrofon in dem Zeitsegment dar.

![Ansicht der Mikrofoneingabe](./media/how-to-guides/microphone-data.png)

## <a name="point-cloud-visualization"></a>Visualisierung von Punktwolken

Über die in 3D visualisierte Tiefe können Sie sich anhand von angegebenen Schlüsseln in dem Bild bewegen.

![Tiefenpunktwolke](./media/how-to-guides/depth-point-cloud.png)

## <a name="synchronization-control"></a>Synchronisierungssteuerung

Im Viewer können Sie das Gerät beim Konfigurieren der Synchronisierung von mehreren Geräten im eigenständigen (Standard), Master- oder untergeordneten Modus konfigurieren.
Wenn Sie die Konfiguration ändern oder das Synchronisierungskabel anschließen oder trennen, wählen Sie **Refresh** (Aktualisieren) aus, um die Konfiguration zu aktualisieren.

![Steuerung der externen Synchronisierung](./media/how-to-guides/sync-control.png)

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Anleitung zur Einrichtung der externen Synchronisierung](https://support.microsoft.com/help/4494429/sync-multiple-azure-kinect-dk-devices)
