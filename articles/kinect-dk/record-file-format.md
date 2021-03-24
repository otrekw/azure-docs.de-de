---
title: Verwenden des Dateiformats des Sensor-SDK für Azure Kinect zum Aufzeichnen
description: Informationen zur Verwendung des Dateiformats für Aufzeichnungen des Sensor-SDK für Azure Kinect.
author: xthexder
ms.author: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: reference
keywords: Kinect, Azure, Sensor, SDK, Tiefe, RGB, Aufzeichnung, Wiedergabe, Matroska, MKV
ms.openlocfilehash: f4fa14b0841cb76b2ba191310ecbca312d29f805
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97654590"
---
# <a name="use-azure-kinect-sensor-sdk-to-record-file-format"></a>Verwenden des Dateiformats des Sensor-SDK für Azure Kinect zum Aufzeichnen

Zum Aufzeichnen von Sensordaten wird das Matroska-Containerformat (.mkv) verwendet, mit dem mehrere Tracks mithilfe einer breiten Palette von Codecs gespeichert werden können. Die Aufzeichnungsdatei enthält Tracks zum Speichern von Farbe, Tiefe, IR-Bildern und IMU.

Allgemeine Informationen zum MKV-Containerformat finden Sie auf der [Matroska-Website](https://www.matroska.org/index.html).

| Trackname | Codec-Format                          |
|------------|---------------------------------------|
| COLOR      | Vom Modus abhängig (MJPEG, NV12 oder YUY2) |
| DEPTH      | b16g (16-Bit-Graustufen, Big-Endian)   |
| IR         | b16g (16-Bit-Graustufen, Big-Endian)   |
| IME        | Benutzerdefinierte Struktur, siehe [IMU-Stichprobenstruktur](record-file-format.md#imu-sample-structure) weiter unten. |

## <a name="using-third-party-tools"></a>Verwenden von Drittanbietertools

Tools wie `ffmpeg` oder der Befehl `mkvinfo` aus dem Toolkit [MKVToolNix](https://mkvtoolnix.download/) können verwendet werden, um Informationen aus Aufzeichnungsdateien anzuzeigen und zu extrahieren.

Mit dem folgenden Befehl wird beispielsweise der Tiefentrack als Sequenz von 16-Bit-PNGs in denselben Ordner extrahiert:

```
ffmpeg -i output.mkv -map 0:1 -vsync 0 depth%04d.png
```

Mit dem Parameter `-map 0:1` wird der Trackindex 1 extrahiert, der sich bei den meisten Aufzeichnungen auf die Tiefe bezieht. Wenn die Aufzeichnung keinen Farbtrack enthält, wird `-map 0:0` verwendet.

Mit dem Parameter `-vsync 0` wird erzwungen, dass ffmpeg Frames unverändert extrahiert, anstatt zu versuchen, eine Bildrate von 30 BpS, 15 BpS oder 5 BpS abzugleichen.

## <a name="imu-sample-structure"></a>IMU-Stichprobenstruktur

Wenn IMU-Daten ohne Verwendung der Wiedergabe-API aus der Datei extrahiert werden, weisen die Daten das Binärformat auf.
Die Struktur der IMU-Daten ist unten dargestellt. Alle Felder weisen das Little-Endian-Format auf.

| Feld                        | type     |
|------------------------------|----------|
| Accelerometer Timestamp (Beschleunigungsmesser-Zeitstempel) (µs) | uint64   |
| Accelerometer Data (Beschleunigungsmesserdaten) (x, y, z) | float[3] |
| Gyroscope Timestamp (Gyroskopzeitstempel) (µs)     | uint64   |
| Gyroscope Data (Gyroskopdaten) (x, y, z)     | float[3] |

## <a name="identifying-tracks"></a>Identifizieren von Tracks

Möglicherweise muss ermittelt werden, welche Tracks Daten zu Farbe, Tiefe, IR usw. enthalten. Die Ermittlung der Tracks ist erforderlich, wenn zum Lesen einer Matroska-Datei Drittanbietertools verwendet werden.
Tracknummern unterscheiden sich je nach Kameramodus und Gruppe der aktivierten Tracks. Zum Ermitteln der Bedeutung der einzelnen Tracks werden Tags verwendet.

Die weiter unten aufgeführten Tags sind jeweils an ein spezifisches Matroska-Element angefügt und können verwendet werden, um nach dem entsprechenden Track oder der entsprechenden Anlage zu suchen.

Diese Tags können mit Tools wie `ffmpeg` oder `mkvinfo` angezeigt werden.
Die vollständige Liste der Tags ist auf der Seite [Aufzeichnen und Wiedergeben](record-playback-api.md) aufgeführt.

| Tag-Name             | Tagziel             | Tagwert             |
|----------------------|------------------------|-----------------------|
| K4A_COLOR_TRACK      | Farbtrack            | Matroska-Track-UID    |
| K4A_DEPTH_TRACK      | Tiefentrack            | Matroska-Track-UID    |
| K4A_IR_TRACK         | IR-Track               | Matroska-Track-UID    |
| K4A_IMU_TRACK        | IMU-Track              | Matroska-Track-UID    |
| K4A_CALIBRATION_FILE | Kalibrierungsanlage | Dateiname der Anlage   |

## <a name="next-steps"></a>Nächste Schritte

[Aufzeichnen und Wiedergeben](record-playback-api.md)
