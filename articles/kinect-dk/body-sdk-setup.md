---
title: 'Schnellstart: Einrichten von Azure Kinect Body Tracking'
description: Im Rahmen dieser Schnellstartanleitung richten Sie das Body Tracking SDK für Azure Kinect ein.
author: qm13
ms.author: quentinm
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Kinect, Azure, Sensor, Zugriff, Tiefe, SDK, Körper, Nachverfolgung, gemeinsam, Einrichtung, Cuda, Nvidia
ms.openlocfilehash: 2cf4c1097730f88fc4bd66c28e1bdddd7fea8640
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277800"
---
# <a name="quickstart-set-up-azure-kinect-body-tracking"></a>Schnellstart: Einrichten von Azure Kinect Body Tracking

Diese Schnellstartanleitung führt Sie durch das Einrichten von Body Tracking in Ihrem Azure Kinect DK.

## <a name="system-requirements"></a>Systemanforderungen

Für das Body Tracking SDK muss auf dem Host-PC eine NVIDIA-GPU installiert sein. Die empfohlenen Anforderungen an den Host-PC für Body Tracking sind auf der Seite [Systemanforderungen](system-requirements.md) beschrieben.

## <a name="install-software"></a>Installieren von Software

### <a name="install-the-latest-nvidia-driver"></a>[Installieren des neuesten NVIDIA-Treibers](https://www.nvidia.com/Download/index.aspx?lang=en-us)

Laden Sie den neuesten NVIDIA-Treiber für Ihre Grafikkarte herunter, und installieren Sie ihn. Ältere Treiber sind möglicherweise nicht mit den CUDA-Binarys kompatibel, die mit dem Body Tracking SDK geliefert werden.

### <a name="visual-c-redistributable-for-visual-studio-2015"></a>[Visual C++ Redistributable für Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)

Laden Sie das Visual C++ Redistributable für Visual Studio 2015 herunter, und installieren Sie es. 

## <a name="set-up-hardware"></a>Einrichten der Hardware

### <a name="set-up-azure-kinect-dk"></a>[Schnellstart: Einrichten Ihres Azure Kinect DK-Geräts](set-up-azure-kinect-dk.md)

Starten Sie den [Azure Kinect Viewer](azure-kinect-viewer.md), um zu überprüfen, ob Ihr Azure Kinect DK ordnungsgemäß eingerichtet ist.

## <a name="download-the-body-tracking-sdk"></a>Herunterladen des Body Tracking-SDK
 
1. Wählen Sie den Link zum [Herunterladen des Body Tracking SDK](body-sdk-download.md) aus
2. Installieren Sie das Body Tracking SDK auf Ihrem PC.

## <a name="verify-body-tracking"></a>Überprüfen des Body Tracking

Starten Sie den **Azure Kinect Body Tracking Viewer**, um zu überprüfen, ob das Body Tracking SDK ordnungsgemäß eingerichtet ist. Der Viewer wird mit dem MSI-Installationsprogramm des SDK installiert. Sie finden ihn im Startmenü oder unter `<SDK Installation Path>\tools\k4abt_simple_3d_viewer.exe`.

Wenn Sie nicht über eine ausreichend leistungsstarke GPU verfügen, das Ergebnis aber trotzdem testen möchten, können Sie den **Azure Kinect Body Tracking Viewer** mithilfe des folgenden Befehls an der Befehlszeile starten: `<SDK Installation Path>\tools\k4abt_simple_3d_viewer.exe CPU`

Wenn alles ordnungsgemäß eingerichtet ist, sollte ein Fenster mit einer 3D-Punktwolke und nachverfolgten Körpern angezeigt werden.


![Body Tracking 3D Viewer](./media/quickstarts/samples-simple3dviewer.png)

## <a name="examples"></a>Beispiele

[Hier](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples) finden Sie Beispiele zur Verwendung des Body Tracking SDK.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Erstellen Ihrer ersten Body Tracking-Anwendung](build-first-body-app.md)

