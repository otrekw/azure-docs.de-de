---
title: Vergleich zwischen Azure Kinect DK und Kinect für Windows
description: Unterschiede zwischen Azure Kinect DK und Kinect für Windows v2 in Bezug auf Hardware und Software
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: article
keywords: Kinect, Windows, v2, Azure Kinect, Vergleich, SDK, Unterschiede, Hardware, Software
ms.openlocfilehash: 0a8d399370f354524858bdd658ffd65c0494dd4d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87031574"
---
# <a name="azure-kinect-and-kinect-windows-v2-comparison"></a>Vergleich zwischen Azure Kinect und Kinect Windows v2

Die Hardware und die Software Development Kits für Azure Kinect DK und Kinect für Windows v2 unterscheiden sich. Vorhandene Kinect für Windows v2-Anwendungen funktionieren nicht direkt mit Azure Kinect DK und müssen in das neue SDK portiert werden.  

## <a name="hardware"></a>Hardware

Die allgemeinen Unterschiede zwischen dem Azure Kinect Development Kit und Kinect für Windows v2 sind in der folgenden Tabelle aufgeführt.

| Funktion | type | Azure Kinect DK | Kinect für Windows v2 |
| ------- | ---- | --------------- | --------------------- |
| **Audio** | Details  | Array mit 7 kreisförmig angeordneten Mikrofonen | Array mit 4 in Reihe angeordneten Mikrofonen |
| **Bewegungssensor** | Details | 3-Achsen-Beschleunigungssensor und -Gyrometer | 3-Achsen-Beschleunigungsmesser |
| **RGB-Kamera**    | Details | 3840 × 2160 px @30 FPS | 1920 × 1080 px @30 FPS |
| **Tiefenkamera**  | Methode   | Laufzeitverfahren (Time-of-Flight, TOF) | Laufzeitverfahren (Time-of-Flight, TOF) |
|                   | Auflösung | 640 × 576 px @30 FPS | 512 × 424 px @30 FPS |
|                   |            | 512 × 512 px @30 FPS |                       |
|                   |            | 1024 × 1024 px @15 FPS |                       |
| **Konnektivität** | Daten | USB 3.1 Gen 1 mit Typ USB-C  | USB 3.1 Gen 1|
|  | Stromversorgung | Externe PSU oder USB-C | Externe PSU |
|  | Synchronization | RGB und Tiefe intern, extern Gerät-zu-Gerät| Nur RGB und Tiefe intern |
| **Mechanik** | Dimensionen | 103 × 39 × 126 mm | 249 × 66 × 67 mm |
|  | Gewicht | 440 g | 970 g |
| | Montage | Ein ¼-20-UNC-Gewinde, vier interne Schraubpunkte | Ein ¼-20-UNC-Gewinde |

Weitere Details finden Sie im Dokument zur [Azure Kinect DK-Hardware](hardware-specification.md).

## <a name="sensor-access"></a>Sensorzugriff

Die folgende Tabelle zeigt einen detaillierten Vergleich der Zugriffsfunktionen der Gerätesensoren.

| **Funktion**| **Azure Kinect** | **Kinect für Windows** | **Hinweise** |
|---------|---------|------------|---------|
| **Tiefe** | ✔️ | ✔️ |    |   |
| **IR** | ✔️ | ✔️ |  |
| **Farbe** | ✔️ | ✔️ | Das Farbformat unterstützt Unterschiede, Azure Kinect DK unterstützt die folgenden Steuerelemente für die Kamera: Belichtung, Weißabgleich, Helligkeit, Kontrast, Sättigung, Schärfe und Verstärkungsregelung. |
| **Audio** | ✔️ | ✔️ | Der Zugriff auf die Azure Kinect DK-Mikrofone erfolgt über das Speech SDK oder eine native Windows-API. |
| **IMU** | ✔️ |  | Azure Kinect DK verfügt über eine vollständige IMU (Inertial Measurement Unit, inertiale Messeinheit) mit sechs Achsen, Kinect für Windows bietet nur eine Achse. |
| **Kalibrierungsdaten** | ✔️ | ✔️ | Mit OpenCV kompatible Kameramodellkalibrierung. |
| **Synchronisierung von RGB und Tiefe intern** | ✔️ | ✔️ |  |
| **Externe Synchronisierung**| ✔️|  | Azure Kinect DK ermöglicht eine programmierbare Verzögerung für die externe Synchronisierung. |
| **Gemeinsamer Zugriff durch mehrere Clients** | | ✔️ | Das Azure Kinect Sensor SDK nutzt WinUSB/libUSB für den Gerätezugriff. Ein Dienst zur Aktivierung des gemeinsamen Gerätezugriffs durch mehrere Prozesse ist nicht implementiert. |
| **Tool zum Aufzeichnen/Wiedergeben von Streams** | ✔️ | ✔️ | Azure Kinect DK verwendet eine auf Open-Source-Matroska-Containern basierende Implementierung. |

## <a name="features"></a>Features

Das Azure Kinect SDK-Featureset unterscheidet sich wie folgt von Kinect für Windows v2:

| **Kinect v2-Feature** | **Kinect v2-Datentyp** | **Azure Kinect SDK/Dienst** |
|--------|--------|------|
| Zugriff auf Sensordaten |DepthFrame| [Sensor SDK – Abrufen von Bildern](retrieve-images.md) 
| |InfraredFrame | [Sensor SDK – Abrufen von Bildern](retrieve-images.md) 
| | ColorFrame | [Sensor SDK – Abrufen von Bildern](retrieve-images.md) | 
| | AudioBeamFrame |Derzeit nicht unterstützt 
| Body Tracking | BodyFrame | Body Tracking SDK |
| | BodyIndexFrame | Body Tracking SDK  |
| Koordinatenzuordnung|CoordinateMapper| [Sensor SDK – Bildtransformationen](use-image-transformation.md) |
|Face Tracking | FaceFrame | [Cognitive Services: Gesichtserkennung](https://azure.microsoft.com/services/cognitive-services/face/)       |
|    Spracherkennung    |    –                      |    [Cognitive Services: Speech](https://azure.microsoft.com/services/cognitive-services/directory/speech/)     |

## <a name="next-steps"></a>Nächste Schritte

[Kinect für Windows: Seiten für Entwickler](https://developer.microsoft.com/windows/kinect)
