---
title: Azure Kinect DK-Koordinatensysteme
description: Beschreibung der Azure Kinect DK-Koordinatensysteme, die Azure DK-Sensoren zugeordnet sind
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, SDK, Tiefenkamera, TOF, Prinzipien, Leistung, Außerkraftsetzung
ms.openlocfilehash: 4bb1c3b79862b918870cff786042d9b4c66270d1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276636"
---
# <a name="azure-kinect-dk-coordinate-systems"></a>Azure Kinect DK-Koordinatensysteme

In diesem Artikel werden die Konventionen beschrieben, die für 2D- und 3D-Koordinatensysteme verwendet werden.  Den Geräten der einzelnen Sensoren und den [Kalibrierungsfunktionen](use-calibration-functions.md), die für die Transformation von Punkten verwendet werden können, sind separate Koordinatensysteme zugeordnet. Mit den [Transformationsfunktionen](use-image-transformation.md) können Bilder vollständig zwischen Koordinatensystemen transformiert werden.  

## <a name="2d-coordinate-systems"></a>2D-Koordinatensysteme

 Sowohl Tiefen- als auch Farbkameras sind unabhängige 2D-Koordinatensysteme zugeordnet. [x,y]-Koordinaten werden in Pixeleinheiten angegeben, wobei die *x*-Werte im Bereich 0 bis width-1 und die *y*-Werte im Bereich 0 bis height-1 liegen können. Die Breite und die Höhe hängen vom ausgewählten Modus ab, in dem die Tiefen- und Farbkameras betrieben werden. Die Pixelkoordinate `[0,0]` entspricht dem Pixel links oben im Bild. Bei Pixelkoordinaten kann es sich um fraktionelle Koordinaten handeln, die Subpixelkoordinaten darzustellen.

Das 2D-Koordinatensystem ist 0-zentriert, d. h., die Subpixelkoordinate `[0.0, 0.0]` stellt den Mittelpunkt und `[0.5,0.5]` die Ecke rechts unten des Pixels dar (siehe Abbildung unten).

   ![2D-Koordinatensystem](./media/concepts/concepts-coordinate-systems/coordinate-systems-sdk-2d-system.png)

## <a name="3d-coordinate-systems"></a>3D-Koordinatensysteme

Jede Kamera, der Beschleunigungsmesser und das Gyroskop sind einem unabhängigen 3D-Koordinatenraumsystem zugeordnet.

Punkte innerhalb des 3D-Koordinatensystems werden als metrische [X,Y,Z]-Koordinatentripel dargestellt, wobei als Einheit Millimeter verwendet wird.

### <a name="depth-and-color-camera"></a>Tiefen- und Farbkamera

Der Ursprung `[0,0,0]` befindet sich am Fokuspunkt der Kamera. Das Koordinatensystem ist so ausgerichtet, dass die positive X-Achse nach rechts, die positive Y-Achse nach unten und die positive Z-Achse nach vorne zeigt.

Die Tiefenkamera ist im Verhältnis zur Farbkamera 6 Grad nach unten geneigt, wie nachfolgend gezeigt. 

Von der Tiefenkamera werden zwei Beleuchtungseinheiten verwendet. Die in NFOV-Modi (Narrow Field-of-View, schmales Blickfeld) verwendete Beleuchtungseinheit ist am Gehäuse der Tiefenkamera ausgerichtet, sodass sie nicht geneigt ist. Die in WFOV-Modi (Wide Field-of-View, breites Blickfeld) verwendete Beleuchtungseinheit ist im Verhältnis zur Tiefenkamera um weitere 1,3 Grad nach unten geneigt.

![Konventionen für 3D-Koordinaten](./media/concepts/concepts-coordinate-systems/coordinate-systems-camera-features.png)

### <a name="gyroscope-and-accelerometer"></a>Gyroskop und Beschleunigungsmesser

Der Ursprung des Gyroskops `[0,0,0]` ist mit dem Ursprung der Tiefenkamera identisch. Der Ursprung des Beschleunigungsmessers stimmt mit der physischen Position überein. Die Koordinatensysteme des Beschleunigungsmessers und des Gyroskops sind rechtshändige Koordinatensysteme. Die positive X-Achse des Koordinatensystems zeigt nach hinten, die positive Y-Achse nach links und die positive Z-Achse nach unten (siehe Abbildung unten).

![IME-Koordinatensystem](./media/concepts/concepts-coordinate-systems/coordinate-systems-gyroscope.png)

## <a name="next-steps"></a>Nächste Schritte

[Grundlegendes zum Sensor-SDK für Azure Kinect](about-sensor-sdk.md)