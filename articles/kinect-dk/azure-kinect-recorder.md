---
title: Azure Kinect DK-Rekorder
description: Erfahren Sie, wie Sie mithilfe des Azure Kinect-Rekorders Datenströme aus dem Sensor SDK in einer Datei aufzeichnen können.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Aufzeichnen, Wiedergabe, Leser, Matroska, mkv, Streams, Tiefe, RGB, Kamera, Farbe, IMU, Audio
ms.openlocfilehash: 5fb6895d4102956a991c67ffab836a26b7a3abb0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "85276707"
---
# <a name="azure-kinect-dk-recorder"></a>Azure Kinect DK-Rekorder

In diesem Artikel wird beschrieben, wie Sie das Befehlszeilen-Hilfsprogramm `k4arecorder` zum Aufzeichnen von Datenströmen aus dem Sensor SDK in einer Datei verwenden können.

>[!NOTE]
>Der Azure Kinect-Rekorder zeichnet keine Audiodaten auf.

## <a name="recorder-options"></a>Rekorderoptionen

Der `k4arecorder` verfügt über verschiedene Befehlszeilenargumente zum Angeben der Ausgabedatei und der Aufzeichnungsmodi.

Die Aufzeichnungen werden im [Matroska MKV-Format](record-file-format.md) gespeichert. Die Aufzeichnung verwendet mehrere Videospuren für Farbe und Tiefe sowie zusätzliche Informationen wie etwa die Kamerakalibrierung und Metadaten.

```console
k4arecorder [options] output.mkv

 Options:
  -h, --help              Prints this help
  --list                  List the currently connected K4A devices
  --device                Specify the device index to use (default: 0)
  -l, --record-length     Limit the recording to N seconds (default: infinite)
  -c, --color-mode        Set the color sensor mode (default: 1080p), Available options:
                            3072p, 2160p, 1536p, 1440p, 1080p, 720p, 720p_NV12, 720p_YUY2, OFF
  -d, --depth-mode        Set the depth sensor mode (default: NFOV_UNBINNED), Available options:
                            NFOV_2X2BINNED, NFOV_UNBINNED, WFOV_2X2BINNED, WFOV_UNBINNED, PASSIVE_IR, OFF
  --depth-delay           Set the time offset between color and depth frames in microseconds (default: 0)
                            A negative value means depth frames will arrive before color frames.
                            The delay must be less than 1 frame period.
  -r, --rate              Set the camera frame rate in Frames per Second
                            Default is the maximum rate supported by the camera modes.
                            Available options: 30, 15, 5
  --imu                   Set the IMU recording mode (ON, OFF, default: ON)
  --external-sync         Set the external sync mode (Master, Subordinate, Standalone default: Standalone)
  --sync-delay            Set the external sync delay off the master camera in microseconds (default: 0)
                            This setting is only valid if the camera is in Subordinate mode.
  -e, --exposure-control  Set manual exposure value (-11 to 1) for the RGB camera (default: auto exposure)
```

## <a name="record-files"></a>Aufzeichnungsdateien

Beispiel 1: Aufzeichnung im unklassifizierten NFOV-Tiefenmodus (640 x 576), RGB 1080p bei 30 FPS mit IMU.
Drücken Sie die Tastenkombination **STRG-C**, um die Aufzeichnung zu beenden.

```
k4arecorder.exe output.mkv
```

Beispiel 2: Aufzeichnung im nicht klassifizierten WFOV-Modus (1 MP), RGB 3072p bei 15 FPS ohne IMU, 10 Sekunden lang.

```
k4arecorder.exe -d WFOV_UNBINNED -c 3072p -r 15 -l 10 --imu OFF output.mkv
```

Beispiel 3: Aufzeichnung im WFOV-Modus 2 x 2 klassifiziert bei 30 FPS für 5 Sekunden mit Speichern in „output.mkv“.

```
k4arecorder.exe -d WFOV_2X2BINNED -c OFF --imu OFF -l 5 output.mkv
```

>[!TIP]
>Sie können den [Azure Kinect Viewer](azure-kinect-viewer.md) verwenden, um RGB-Kamerasteuerelemente vor der Aufzeichnung zu konfigurieren (beispielsweise um manuellen Weißabgleich festzulegen).

## <a name="verify-recording"></a>Überprüfen der Aufzeichnung

Sie können die MKV-Ausgabedatei im [Azure Kinect Viewer](azure-kinect-viewer.md) öffnen.

Zum Extrahieren von Spuren oder Anzeigen von Dateiinformationen stehen Tools wie `mkvinfo` als Teil des [MKVToolNix](https://mkvtoolnix.download/)-Toolkits zur Verfügung.

## <a name="next-steps"></a>Nächste Schritte

[Verwenden des Rekorders mit extern synchronisierten Einheiten](record-external-synchronized-units.md)