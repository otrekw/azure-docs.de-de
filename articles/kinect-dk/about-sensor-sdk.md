---
title: Informationen zum Sensor-SDK für Azure Kinect
description: Übersicht zum Sensor-Software Development Kit (SDK) für Azure Kinect, seinen Features und Tools.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: article
keywords: Azure, Kinect, RGB, IR, Aufzeichnung, Sensor, SDK, Zugriff, Tiefe, Video, Kamera, IMU, Bewegung, Sensor, Audio, Mikrofon, Matroska, Sensor-SDK, Download
ms.openlocfilehash: 17c1b33120eacb5d0c6d3c02e692d1488ef474e6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276731"
---
# <a name="about-azure-kinect-sensor-sdk"></a>Informationen zum Sensor-SDK für Azure Kinect

Dieser Artikel bietet eine Übersicht zum Sensor-Software Development Kit (SDK) für Azure Kinect, seinen Features und Tools.

## <a name="features"></a>Features

Das Sensor SDK von Azure Kinect bietet plattformübergreifenden Low Level-Zugriff für die Azure Kinect-Gerätekonfiguration und Hardware-Sensordatenströme, einschließlich:

- Zugriff auf die Tiefenkamera und Modussteuerung (ein passiver IR-Modus zuzüglich weiterer Tiefenmodi mit breitem und engem Bildwinkel) 
- Zugriff auf die RGB-Kamera und deren Steuerung (z. B. Belichtung und Weißabgleich) 
- Zugriff auf Bewegungssensoren (Gyroskop und Beschleunigungsmesser) 
- Synchronisiertes Streaming von Tiefen- und RGB-Kamera mit konfigurierbarer Verzögerung zwischen den Kameras 
- Externe Steuerung der Gerätesynchronisierung mit konfigurierbarem Verzögerungsoffset zwischen den Geräten 
- Zugriff auf Metadaten von Kameraframes für Bildauflösung, Zeitstempel usw. 
- Zugriff auf Kalibrierungsdaten von Geräten 

## <a name="tools"></a>Tools

- Ein [Azure Kinect-Viewer](azure-kinect-viewer.md) zum Überwachen der Datenströme von Geräten und zum Konfigurieren verschiedener Modi.
- Eine [Azure Kinect-Rekorder-](azure-kinect-recorder.md) und Wiedergabe-Lese-API., die das [Matroska-Containerformat](record-file-format.md) verwendet.
- Ein [Firmwareupdate-Tool](azure-kinect-firmware-tool.md) für Azure Kinect DK.

## <a name="sensor-sdk"></a>Sensor SDK

- [Sensor SDK herunterladen](sensor-sdk-download.md).
- Das Sensor SDK steht als [Open Source auf GitHub](https://github.com/microsoft/Azure-Kinect-Sensor-SDK) zur Verfügung.
- Weitere Informationen zur Verwendung finden Sie in der [Sensor SDK-API-Dokumentation](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/index.html).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich jetzt mit dem Sensor SDK zu Azure Kinect vertraut gemacht haben, können Sie außerdem:
>[!div class="nextstepaction"]
>[Den Sensor SDK-Code herunterladen](sensor-sdk-download.md)

>[!div class="nextstepaction"]
>[Ein Gerät finden und öffnen](find-then-open-device.md)
