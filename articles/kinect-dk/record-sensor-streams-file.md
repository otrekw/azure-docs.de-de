---
title: 'Schnellstart: Aufzeichnen von Azure Kinect-Sensordatenströmen in einer Datei'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Datenströme vom Sensor SDK in einer Datei aufzeichnen.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Azure, Kinect, Aufzeichnen, Wiedergabe, Reader, Matroska, MKV, Datenströme, Tiefe, RGB, Kamera, Farbe, IMU, Audiodaten, Sensor
ms.openlocfilehash: 3dab147b593bf012bd6cd9c95d0195e84a2cbcf1
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "85277793"
---
# <a name="quickstart-record-azure-kinect-sensor-streams-to-a-file"></a>Schnellstart: Aufzeichnen von Azure Kinect-Sensordatenströmen in einer Datei

Diese Schnellstartanleitung enthält Informationen zum Verwenden des [Azure Kinect-Rekorder](azure-kinect-recorder.md)-Tools zum Aufzeichnen von Datenströmen vom Sensor SDK in einer Datei.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für diese Schnellstartanleitung:

- Das Azure Kinect DK ist mit Ihrem PC verbunden und wird ordnungsgemäß mit Strom versorgt.
- Sie haben die [Einrichtung](set-up-azure-kinect-dk.md) der Hardware abgeschlossen.

## <a name="create-recording"></a>Erstellen einer Aufzeichnung

1. Öffnen Sie eine Eingabeaufforderung, und geben Sie den Pfad zum [Azure Kinect-Rekorder](azure-kinect-recorder.md) an, der sich als `k4arecorder.exe` im installierten Toolsverzeichnis befindet. Beispiel: `C:\Program Files\Azure Kinect SDK\tools\k4arecorder.exe`.
2. Zeichnen Sie 5 Sekunden auf.

    `k4arecorder.exe -l 5 %TEMP%\output.mkv`

3. Standardmäßig verwendet der Rekorder den Tiefenmodus „NFOV unklassifiziert“ und gibt ein RGB-Signal mit 1080p bei 30 Frames/s einschließlich IMU-Daten aus. Eine umfassende Übersicht zu den Aufzeichnungsoptionen und Tipps finden Sie in der Dokumentation zum [Azure Kinect-Rekorder](azure-kinect-recorder.md).

## <a name="play-back-recording"></a>Wiedergeben der Aufzeichnung

Sie können den [Azure Kinect Viewer](azure-kinect-viewer.md) verwenden, um eine Aufzeichnung wiederzugeben.

1. Starten Sie [`k4aviewer.exe`](azure-kinect-viewer.md).
2. Klappen Sie die Registerkarte **Aufzeichnung öffnen** auf, und öffnen Sie Ihre Aufzeichnung.

## <a name="next-steps"></a>Nächste Schritte

Da Sie jetzt gelernt haben, wie Sensordatenströme in einer Datei aufgezeichnet werden, ist es an der Zeit zum

> [!div class="nextstepaction"]
> [Erstellen Ihrer ersten Azure Kinect-Anwendung](build-first-app.md)
