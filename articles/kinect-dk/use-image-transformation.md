---
title: Verwenden von Bildtransformationen des Azure Kinect Sensor SDK
description: Erfahren Sie, wie Sie die Funktionen des Azure Kinect Sensor SDK zur Bildtransformation verwenden.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.topic: conceptual
ms.date: 06/26/2019
keywords: Kinect, Azure, Sensor, SDK, Koordinatensystem, Kalibrierung, Projektion, Umkehrung der Projektion, Transformation, RGB-D, Punktwolke
ms.openlocfilehash: df7f2aa13c0e9c0241494e96e720b30f3ff1d8a5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276648"
---
# <a name="use-azure-kinect-sensor-sdk-image-transformations"></a>Verwenden von Bildtransformationen des Azure Kinect Sensor SDK

Führen Sie die spezifischen Funktionen aus, um Bilder zwischen koordinierten Kamerasystemen in Azure Kinect DK zu verwenden und zu transformieren.

## <a name="k4a_transformation-functions"></a>k4a_transformation-Funktionen

 Alle Funktionen mit dem Präfix *k4a_transformation* werden auf ganze Bilder angewandt. Diese Funktionen erfordern das Transformationshandle [k4a_transformation_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__transformation__t.html), das über [k4a_transformation_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga853a1a5b6d521bbdf523a69e890c4f10.html#ga853a1a5b6d521bbdf523a69e890c4f10) abgerufen wird, und ihre Zuordnung wird über [k4a_transformation_destroy()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7d3ecaae66f26c1a89da9042b1bc6d44.html#ga7d3ecaae66f26c1a89da9042b1bc6d44) aufgehoben. Informationen finden Sie auch auf GitHub im [Transformation-Beispiel](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/transformation) für das SDK, das die Verwendung der drei in diesem Thema beschriebenen Funktionen veranschaulicht.

Die folgenden Funktionen werden behandelt:

* [`k4a_transformation_depth_image_to_color_camera()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514)
* [`k4a_transformation_depth_image_to_color_camera_custom()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gac00dd00e7612a86382e3d0a130f276bb.html#gac00dd00e7612a86382e3d0a130f276bb)
* [`k4a_transformation_color_image_to_depth_camera()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf3a941f07bb0185cd7a72699a648fc29.html#gaf3a941f07bb0185cd7a72699a648fc29)
* [`k4a_transformation_depth_image_to_point_cloud()`](
https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70)

### <a name="k4a_transformation_depth_image_to_color_camera"></a>k4a_transformation_depth_image_to_color_camera

#### <a name="overview"></a>Übersicht

 Die Funktion [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) transformiert die Tiefenzuordnung vom Standpunkt der Tiefenkamera in den Standpunkt der Farbkamera. Diese Funktion ist so konzipiert, dass sogenannte RGB-D-Bilder generiert werden, wobei D einen zusätzlichen Bildkanal darstellt, der den Tiefenwert aufzeichnet. Wie in der folgenden Abbildung dargestellt, sehen das Farbbild und die Ausgabe von [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) so aus, als ob sie vom gleichen Standpunkt aus aufgenommen wurden, d. h. vom Standpunkt der Farbkamera.

   ![Bildtransformation](./media/how-to-guides/image-transformation.png)

#### <a name="implementation"></a>Implementierung

 Diese Transformationsfunktion ist komplexer als der einfache Aufruf von [k4a_calibration_2d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) für jedes Pixel. Die Funktion verzerrt ein Dreiecksgitter aus der Geometrie der Tiefenkamera in die Geometrie der Farbkamera. Das Dreiecksgitter wird verwendet, um zu vermeiden, dass Lücken im transformierten Tiefenbild generiert werden. Ein Z-Puffer stellt sicher, dass Verdeckungen ordnungsgemäß verarbeitet werden. Für diese Funktion ist standardmäßig die GPU-Beschleunigung aktiviert.

#### <a name="parameters"></a>Parameter

 Eingabeparameter sind das Transformationshandle und ein Tiefenbild. Die Auflösung des Tiefenbilds muss mit dem Wert für ```depth_mode``` übereinstimmen, der bei der Erstellung des Transformationshandles angegeben wird. Wenn das Transformationshandle z. B. im Modus ```K4A_DEPTH_MODE_WFOV_UNBINNED``` mit 1.024 × 1.024 erstellt wurde, muss die Auflösung des Tiefenbilds 1.024 × 1.024 Pixel betragen. Bei der Ausgabe handelt es sich um ein transformiertes Tiefenbild, das vom Benutzer durch Aufrufen von [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef) zugeordnet werden muss. Die Auflösung des transformierten Tiefenbilds muss mit dem Wert für ```color_resolution``` übereinstimmen, der bei der Erstellung des Transformationshandles angegeben wird. Wenn die Farbauflösung beispielsweise auf `K4A_COLOR_RESOLUTION_1080P` festgelegt wurde, muss die Auflösung des Ausgabebilds 1.920 × 1.080 Pixel betragen. Die Größe des Ausgabebilds wird auf `width * sizeof(uint16_t)` festgelegt, da im Bild 16-Bit-Tiefenwerte gespeichert werden.

### <a name="k4a_transformation_depth_image_to_color_camera_custom"></a>k4a_transformation_depth_image_to_color_camera_custom

#### <a name="overview"></a>Übersicht

 Die Funktion [k4a_transformation_depth_image_to_color_camera_custom()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gac00dd00e7612a86382e3d0a130f276bb.html#gac00dd00e7612a86382e3d0a130f276bb) transformiert die Tiefenzuordnung und ein benutzerdefiniertes Bild vom Standpunkt der Tiefenkamera in den Standpunkt der Farbkamera. Als Erweiterung von [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) ist diese Funktion so konzipiert, dass zusätzlich zum transformierten Tiefenbild ein entsprechendes benutzerdefiniertes Bild generiert wird, für das jedes Pixel mit den entsprechenden Pixelkoordinaten der Farbkamera übereinstimmt.

#### <a name="implementation"></a>Implementierung

 Diese Transformationsfunktion generiert das transformierte Tiefenbild auf die gleiche Weise wie [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514). Zum Transformieren des benutzerdefinierten Bilds umfasst diese Funktion Optionen zur Verwendung der linearen Interpolation oder der Interpolation der Pixelwiederholung. Durch Verwendung der linearen Interpolation können neue Werte im transformierten benutzerdefinierten Bild erstellt werden. Die Verwendung der Interpolation der Pixelwiederholung verhindert, dass Werte, die im ursprünglichen Bild nicht vorhanden sind, in das Ausgabebild aufgenommen werden, führt jedoch zu einem weniger geglätteten Bild. Das benutzerdefinierte Bild muss in einem einzelnen Kanal mit 8 Bit oder 16 Bit erstellt werden. Für diese Funktion ist standardmäßig die GPU-Beschleunigung aktiviert.

#### <a name="parameters"></a>Parameter

 Eingabeparameter sind das Transformationshandle, ein Tiefenbild, ein benutzerdefiniertes Bild und der Interpolationstyp. Die Auflösung des Tiefenbilds und des benutzerdefinierten Bilds muss mit dem Wert für `depth_mode` übereinstimmen, der bei der Erstellung des Transformationshandles angegeben wird. Wenn das Transformationshandle z. B. im Modus `K4A_DEPTH_MODE_WFOV_UNBINNED` mit 1.024 × 1.024 erstellt wurde, muss die Auflösung des Tiefenbilds und des benutzerdefinierten Bilds 1.024 × 1.024 Pixel betragen. Der Wert für `interpolation_type` muss `K4A_TRANSFORMATION_INTERPOLATION_TYPE_LINEAR` oder `K4A_TRANSFORMATION_INTERPOLATION_TYPE_NEAREST` sein. Bei der Ausgabe handelt es sich um ein transformiertes Tiefenbild und ein transformiertes benutzerdefiniertes Bild, die vom Benutzer durch Aufrufen von [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef) zugeordnet werden müssen. Die Auflösung des transformierten Tiefenbilds und des transformierten benutzerdefinierten Bilds muss mit dem Wert für `color_resolution` übereinstimmen, der bei der Erstellung des Transformationshandles angegeben wird. Wenn die Farbauflösung beispielsweise auf `K4A_COLOR_RESOLUTION_1080P` festgelegt wurde, muss die Auflösung des Ausgabebilds 1.920 × 1.080 Pixel betragen. Die Größe des Ausgabetiefenbilds wird auf `width * sizeof(uint16_t)` festgelegt, da im Bild 16-Bit-Tiefenwerte gespeichert werden. Das benutzerdefinierte Eingabebild und das transformierte benutzerdefinierte Bild müssen das Format `K4A_IMAGE_FORMAT_CUSTOM8` oder `K4A_IMAGE_FORMAT_CUSTOM16` aufweisen. Die zugehörige Bildgröße muss entsprechend festgelegt werden. 

### <a name="k4a_transformation_color_image_to_depth_camera"></a>k4a_transformation_color_image_to_depth_camera

#### <a name="overview"></a>Übersicht

 Die Funktion [k4a_transformation_color_image_to_depth_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf3a941f07bb0185cd7a72699a648fc29.html#gaf3a941f07bb0185cd7a72699a648fc29) transformiert das Farbbild vom Standpunkt der Farbkamera in den Standpunkt der Tiefenkamera (siehe Abbildung oben). Mit dieser Funktion können RGB-D-Bilder generiert werden.

#### <a name="implementation"></a>Implementierung

 Die Funktion verwendet für jedes Pixel der Tiefenzuordnung den Tiefenwert des Pixels, um die entsprechende Subpixelkoordinate im Farbbild zu berechnen. Anschließend wird nach dem Farbwert an dieser Koordinate im Farbbild gesucht. Im Farbbild wird die bilineare Interpolation ausgeführt, um den Farbwert mit Subpixelgenauigkeit abzurufen. Ein Pixel ohne zugeordnete Tiefenmessung wird dem BGRA-Wert `[0,0,0,0]` im Ausgabebild zugewiesen. Für diese Funktion ist standardmäßig die GPU-Beschleunigung aktiviert. Da bei dieser Methode Lücken im transformierten Farbbild generiert und Verdeckungen nicht verarbeitet werden, empfiehlt es sich, stattdessen die Funktion [k4a_transformation_depth_image_to_color_camera()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafacffb5f781a9c2df30d4a16241cd514.html#gafacffb5f781a9c2df30d4a16241cd514) zu verwenden.

#### <a name="parameters"></a>Parameter

Eingabeparameter sind das Transformationshandle, ein Tiefenbild und ein Farbbild. Die Auflösung des Tiefenbilds und des Farbbilds muss mit den Werten für „depth_mode“ und „color_resolution“ übereinstimmen, die bei der Erstellung des Transformationshandles angegeben wurden. Bei der Ausgabe handelt es sich um ein transformiertes Farbbild, das vom Benutzer durch Aufrufen von [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef) zugeordnet werden muss. Die Auflösung des transformierten Farbbilds muss mit dem Wert für „depth_resolution“ übereinstimmen, der bei der Erstellung des Transformationshandles angegeben wurde. Im Ausgabebild werden vier 8-Bit-Werte gespeichert, die den BGRA-Wert für jedes Pixel darstellen. Daher ist ```width * 4 * sizeof(uint8_t)``` als Größe des Bilds festgelegt. Die Datenreihenfolge liegt als Pixelinterleave vor, d. h. Blauwert – Pixel 0, Grünwert – Pixel 0, Rotwert – Pixel 0, Alphawert – Pixel 0, Blauwert – Pixel 1 usw.

### <a name="k4a_transformation_depth_image_to_point_cloud"></a>k4a_transformation_depth_image_to_point_cloud

#### <a name="overview"></a>Übersicht

Mit der Funktion [k4a_transformation_depth_image_to_point_cloud()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70) wird eine mit einer Kamera erstellte 2D-Tiefenzuordnung in eine 3D-Punktwolke im Koordinatensystem der Kamera umgewandelt. Bei der Kamera kann es sich somit um die Tiefenkamera oder die Farbkamera handeln.

#### <a name="implementation"></a>Implementierung

 Diese Funktion liefert gleichwertige Ergebnisse wie die Ausführung von [k4a_calibration_2d_to_2d()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3b6bf6dedbfe67468e2f895dcce68ed4.html#ga3b6bf6dedbfe67468e2f895dcce68ed4) für jedes Pixel, ist aber rechnerisch effizienter. Beim Aufrufen von [k4a_transformation_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga853a1a5b6d521bbdf523a69e890c4f10.html#ga853a1a5b6d521bbdf523a69e890c4f10) wird eine sogenannte XY-Nachschlagetabelle vorausberechnet, in der X- und Y-Skalierungsfaktoren für jedes Bildpixel gespeichert sind. Beim Aufrufen von [k4a_transformation_depth_image_to_point_cloud()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7385eb4beb9d8892e8a88cf4feb3be70.html#ga7385eb4beb9d8892e8a88cf4feb3be70) wird die 3D-X-Koordinate eines Pixels durch Multiplikation des X-Skalierungsfaktors des Pixels mit der Z-Koordinate des Pixels berechnet. Analog dazu wird die 3D-Y-Koordinate durch Multiplikation mit dem Y-Skalierungsfaktor berechnet. Das [fastpointcloud-Beispiel](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/fastpointcloud) des SDK veranschaulicht, wie die XY-Tabelle berechnet wird. Benutzer können den Beispielcode verwenden, um ihre eigene Version dieser Funktion zu implementieren, z. B. um die GPU-Pipeline zu beschleunigen.

#### <a name="parameters"></a>Parameter

 Eingabeparameter sind das Transformationshandle, ein Kameraspezifizierer und ein Tiefenbild. Wenn der Kameraspezifizierer auf die Tiefenkamera festgelegt ist, muss die Auflösung des Tiefenbilds mit dem Wert für „depth_mode“ übereinstimmen, der bei der Erstellung des Transformationshandles angegeben wird. Wenn der Spezifizierer dagegen auf die Farbkamera festgelegt ist, muss die Auflösung mit der Auflösung des ausgewählten Werts für „color_resolution“ übereinstimmen. Bei dem Ausgabeparameter handelt es sich um ein XYZ-Bild, das vom Benutzer durch Aufrufen von [k4a_image_create()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga859554581bb97a620ff8e92a893e71ef.html#ga859554581bb97a620ff8e92a893e71ef) zugeordnet werden muss. Die Auflösung des XYZ-Bilds muss mit der Auflösung der Eingabetiefenzuordnung übereinstimmen. Für jedes Pixel werden drei signierte 16-Bit-Koordinatenwerte in Millimeter gespeichert. Die Größe des XYZ-Bilds wird daher auf `width * 3 * sizeof(int16_t)` festgelegt. Die Datenreihenfolge liegt als Pixelinterleave vor, d. h. X-Koordinate – Pixel 0, Y-Koordinate – Pixel 0, Z-Koordinate – Pixel 0, X-Koordinate – Pixel 1 usw. Wenn ein Pixel nicht in 3D konvertiert werden kann, weist die Funktion dem Pixel die Werte `[0,0,0]` zu.

## <a name="samples"></a>Beispiele

[Transformation-Beispiel](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/transformation)

## <a name="next-steps"></a>Nächste Schritte

Sie haben sich mit der Verwendung der Funktionen des Azure Kinect Sensor SDK zur Bildtransformation vertraut gemacht. Daher können Sie sich nun folgendem Thema widmen:

>[!div class="nextstepaction"]
>[Kalibrierungsfunktionen des Azure Kinect Sensor SDK](use-calibration-functions.md)

Außerdem können Sie sich mit folgendem Thema beschäftigen:

[Koordinatensysteme](coordinate-systems.md)
