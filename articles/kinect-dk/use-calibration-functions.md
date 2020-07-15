---
title: Verwenden der Kalibrierungsfunktionen für Azure Kinect
description: Erfahren Sie, wie Sie die Kalibrierungsfunktionen für Azure Kinect DK verwenden.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, SDK, Koordinatensystem, Kalibrierung, Funktionen, Kamera, intrinsisch, extrinsisch, Projektion, Umkehrung der Projektion, Transformation, RGB-D, Punktwolke
ms.openlocfilehash: a501765222154bde4ed35e878a9d18042893fca2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276684"
---
# <a name="use-azure-kinect-calibration-functions"></a>Verwenden der Kalibrierungsfunktionen für Azure Kinect

Mit den Kalibrierungsfunktionen können Punkte zwischen den Koordinatensystemen der einzelnen Sensoren im Azure Kinect-Gerät transformiert werden. In Anwendungen, in denen ganze Bilder konvertiert werden müssen, können die mit den [Transformationsfunktionen](use-image-transformation.md) verfügbaren beschleunigten Vorgänge genutzt werden.

## <a name="retrieve-calibration-data"></a>Abrufen von Kalibrierungsdaten

Zum Ausführen von Transformationen zwischen Koordinatensystemen muss die Gerätekalibrierung abgerufen werden. Die Kalibrierungsdaten sind im Datentyp [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html#details) gespeichert. Sie werden über die Funktion [k4a_device_get_calibration()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78) vom Gerät abgerufen. Die Kalibrierungsdaten sind nicht nur für jedes Gerät, sondern auch für den Betriebsmodus der Kameras spezifisch. Daher müssen für [k4a_device_get_calibration()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78) die Parameter `depth_mode` und `color_resolution` eingegeben werden.

### <a name="opencv-compatibility"></a>Kompatibilität mit OpenCV

Die Kalibrierungsparameter sind mit [OpenCV](https://opencv.org/) kompatibel. Weitere Informationen zu den einzelnen Kamerakalibrierungsparametern finden Sie auch in der [Dokumentation zu OpenCV](https://docs.opencv.org/3.2.0/d9/d0c/group__calib3d.html#gga7041b2a9c8f9f8ee93a2796981bc5546a204766e24f2e413e7a7c9f8b9e93f16c). Außerdem veranschaulicht das [OpenCV-Kompatibilitätsbeispiel](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/opencv_compatibility) des SDK die Konvertierung zwischen dem Datentyp [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html#details) und den entsprechenden OpenCV-Datenstrukturen.

## <a name="coordinate-transformation-functions"></a>Transformationsfunktionen für Koordinatensysteme

In der folgenden Abbildung sind die verschiedenen Koordinatensysteme von Azure Kinect sowie die Funktionen für die Konvertierung zwischen den Systemen dargestellt. Zur Vereinfachung der Abbildung sind die 3D-Koordinatensysteme des Gyroskops und des Beschleunigungsmessers nicht dargestellt.

   ![Transformation in Koordinatensystemen](./media/how-to-guides/coordinate-transformation.png)

Hinweis zur Linsenverzerrung: 2D-Koordinaten beziehen sich immer auf das verzerrte Bild im SDK. Das [Beispiel zum Aufheben der Verzerrung](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/undistort) des SDK veranschaulicht die Korrektur verzerrter Bilder. Im Allgemeinen werden 3D-Punkte niemals durch eine Linsenverzerrung beeinträchtigt.

### <a name="convert-between-3d-coordinate-systems"></a>Konvertierung zwischen 3D-Koordinatensystemen

Die Funktion [k4a_calibration_3d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaacd1eab997ef964b5e436afa5295726e.html#gaacd1eab997ef964b5e436afa5295726e) konvertiert einen 3D-Punkt des Quellkoordinatensystems unter Verwendung der extrinsischen Kalibrierung der Kamera in einen 3D-Punkt des Zielkoordinatensystems. Quelle und Ziel können auf eines der vier 3D-Koordinatensysteme festgelegt werden: Farbkamera, Tiefenkamera, Gyroskop oder Beschleunigungsmesser. Wenn Quelle und Ziel identisch sind, wird der unveränderte 3D-Eingabepunkt als Ausgabe zurückgegeben.

### <a name="convert-between-2d-and-3d-coordinate-systems"></a>Konvertierung zwischen 2D- und 3D-Koordinatensystemen

Die Funktion [k4a_calibration_3d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ed8b51d727425caa942aab190fc2ba9.html#ga2ed8b51d727425caa942aab190fc2ba9) konvertiert einen 3D-Punkt des Quellkoordinatensystems in eine 2D-Pixelkoordinate der Zielkamera. Diese Funktion wird häufig als Projektionsfunktion bezeichnet. Die Quelle kann auf ein beliebiges der vier 3D-Koordinatensysteme festgelegt werden, als Ziel muss jedoch die Tiefenkamera oder die Farbkamera festgelegt werden. Wenn sich Quelle und Ziel unterscheiden, wird der 3D-Eingabepunkt unter Verwendung von [k4a_calibration_3d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaacd1eab997ef964b5e436afa5295726e.html#gaacd1eab997ef964b5e436afa5295726e) in das 3D-Koordinatensystem der Zielkamera konvertiert. Wenn der 3D-Punkt im Koordinatensystem der Zielkamera dargestellt wird, werden die entsprechenden 2D-Pixelkoordinaten unter Verwendung der intrinsischen Kalibrierung der Zielkamera berechnet. Wenn ein 3D-Punkt nicht mehr im sichtbaren Bereich der Zielkamera liegt, wird der gültige Wert auf „0“ festgelegt.

Die Funktion [k4a_calibration_2d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga664602bdb48dab38117a6c1d14b880de.html#ga664602bdb48dab38117a6c1d14b880de) konvertiert eine 2D-Pixelkoordinate der Quellkamera in einen 3D-Punkt des Koordinatensystems der Zielkamera. Als Quelle muss die Farbkamera oder die Tiefenkamera festgelegt sein. Das Ziel kann auf ein beliebiges der vier 3D-Koordinatensysteme festgelegt werden. Zusätzlich zur 2D-Pixelkoordinate muss der Tiefenwert des Pixels (in Millimeter) im Bild der Quellkamera für die Funktion eingegeben werden. Eine Möglichkeit zum Ableiten des Tiefenwerts in der Geometrie der Farbkamera ist die Verwendung der Funktion [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514). Die Funktion berechnet unter Verwendung der intrinsischen Kalibrierung der Quellkamera den 3D-Strahl, der vom Fokuspunkt der Quellkamera bis zur angegebenen Pixelkoordinate verläuft. Anschließend wird anhand des Tiefenwerts die genaue Position des 3D-Punkts auf diesem Strahl ermittelt. Dieser Vorgang wird häufig als Projektionsumkehrungsfunktion bezeichnet. Wenn sich Quell- und Zielkamera unterscheiden, transformiert die Funktion den 3D-Punkt über [k4a_calibration_3d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaacd1eab997ef964b5e436afa5295726e.html#gaacd1eab997ef964b5e436afa5295726e) in das Koordinatensystem der Zielkamera. Wenn eine 2D-Pixelkoordinate nicht mehr im sichtbaren Bereich der Quellkamera liegt, wird der gültige Wert auf „0“ festgelegt.

### <a name="converting-between-2d-coordinate-systems"></a>Konvertierung zwischen 2D-Koordinatensystemen

Die Funktion [k4a_calibration_2d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) konvertiert eine 2D-Pixelkoordinate der Quellkamera in eine 2D-Pixelkoordinate der Zielkamera. Quelle und Ziel müssen auf die Farbkamera oder Tiefenkamera festgelegt sein. Für die Funktion muss der Tiefenwert des Pixels (in Millimeter) im Bild der Quellkamera eingegeben werden. Eine Möglichkeit zum Ableiten des Tiefenwerts in der Geometrie der Farbkamera ist die Verwendung der Funktion [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514). Diese Funktion ruft [k4a_calibration_2d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga664602bdb48dab38117a6c1d14b880de.html#ga664602bdb48dab38117a6c1d14b880de) auf, um die Konvertierung in einen 3D-Punkt des Quellkamerasystems durchzuführen. Anschließend wird [k4a_calibration_3d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ed8b51d727425caa942aab190fc2ba9.html#ga2ed8b51d727425caa942aab190fc2ba9) aufgerufen, um die Konvertierung in eine 2D-Pixelkoordinate des Zielkamerabilds durchzuführen. Der gültige Wert wird auf „0“ festgelegt, wenn [k4a_calibration_2d_to_3d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga664602bdb48dab38117a6c1d14b880de.html#ga664602bdb48dab38117a6c1d14b880ded) oder [k4a_calibration_3d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ed8b51d727425caa942aab190fc2ba9.html#ga2ed8b51d727425caa942aab190fc2ba9) ein ungültiges Ergebnis zurückgibt.

## <a name="related-samples"></a>Zugehörige Beispiele

- [OpenCV-Kompatibilitätsbeispiel](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/opencv_compatibility)
- [Beispiel zum Aufheben der Verzerrung](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/undistort)
- [Beispiel für schnelle Punktwolke](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/fastpointcloud)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit Kamerakalibrierungen vertraut gemacht haben, können Sie sich das folgende Thema ansehen:
>[!div class="nextstepaction"]
>[Erfassen der Gerätesynchronisierung](capture-device-synchronization.md)

Außerdem können Sie sich mit folgendem Thema beschäftigen:

[Koordinatensysteme](coordinate-systems.md)
