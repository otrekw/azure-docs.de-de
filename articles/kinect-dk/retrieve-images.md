---
title: Abrufen von Bilddaten für Azure Kinect
description: Erfahren Sie, wie Sie mit dem Kinect Sensor SDK Bilddaten für Azure Kinect abrufen können.
author: cdedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, abrufen, Sensor, Kamera, SDK, Tiefe, RGB, Bilder, Farbe, Aufnahme, Auflösung, Puffer
ms.openlocfilehash: fed5c7340d287d9103ba35f0fd3d80c0fff6e3ec
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87538913"
---
# <a name="retrieve-azure-kinect-image-data"></a>Abrufen von Bilddaten für Azure Kinect

Diese Seite enthält Informationen zum Abrufen von Bildern vom Azure Kinect-Gerät. In diesem Artikel wird beschrieben, wie Sie die zwischen der Farbe und der Tiefe des Geräts koordinierten Bilder aufnehmen und darauf zugreifen. Für den Zugriff auf Bilder müssen Sie zunächst das Gerät öffnen und konfigurieren. Dann können Sie Bilder aufnehmen.
Vor dem Konfigurieren und Aufnehmen eines Bilds müssen Sie [das Gerät suchen und öffnen](find-then-open-device.md).

Informationen finden Sie auch auf GitHub im [Streamingbeispiel für das SDK](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/streaming), das die Verwendung der in diesem Artikel beschriebenen Funktionen veranschaulicht.

Die folgenden Funktionen werden behandelt:

- [`k4a_device_start_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a)
- [`k4a_device_get_capture()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4dac757a33657f4d3dbf1ae8b21c158a.html#ga4dac757a33657f4d3dbf1ae8b21c158a)
- [`k4a_capture_get_depth_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafa03513da96bf6b8c254fc11a04ee6d6.html#gafa03513da96bf6b8c254fc11a04ee6d6)
- [`k4a_image_get_buffer()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ef070cf4e543cd0f726478af332546e.html#ga2ef070cf4e543cd0f726478af332546e)
- [`k4a_image_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga27c81863b13fafc3934a32935a014e9f.html#ga27c81863b13fafc3934a32935a014e9f)
- [`k4a_capture_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6)
- [`k4a_device_stop_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8)
  
## <a name="configure-and-start-the-device"></a>Konfigurieren und Starten des Geräts

Die beiden Kameras im Kinect-Gerät unterstützen mehrere Modi, Auflösungen und Ausgabeformate. Eine vollständige Liste finden Sie in den [Hardwarespezifikationen](hardware-specification.md) des Azure Kinect Development Kits.

Die Streamingkonfiguration wird mithilfe der Werte in der Struktur [`k4a_device_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) festgelegt.

```C
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps = K4A_FRAMES_PER_SECOND_30;
config.color_format = K4A_IMAGE_FORMAT_COLOR_MJPG;
config.color_resolution = K4A_COLOR_RESOLUTION_2160P;
config.depth_mode = K4A_DEPTH_MODE_NFOV_UNBINNED;

if (K4A_RESULT_SUCCEEDED != k4a_device_start_cameras(device, &config))
{
    printf("Failed to start device\n");
    goto Exit;
}
```

Nachdem die Kameras gestartet wurden, nehmen sie Daten auf, bis [`k4a_device_stop_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8) aufgerufen oder das Gerät beendet wird.

## <a name="stabilization"></a>Stabilisierung

Wenn Sie Geräte mithilfe der Funktion zur Synchronisierung mehrerer Geräte starten, wird dringend empfohlen, dabei eine feste Belichtungseinstellung zu verwenden.
Mit einer manuellen Belichtungseinstellung müssen mit dem Gerät bis zu acht Aufnahmen gemacht werden, bis Bilder und Bildsequenz stabilisiert werden. Mit der automatischen Belichtung müssen bis zu 20 Aufnahmen gemacht werden, bis Bilder und Bildsequenz stabilisiert werden.

## <a name="get-a-capture-from-the-device"></a>Abrufen einer Aufnahme vom Gerät

Bilder werden mit dem Gerät in korrelierter Weise aufgenommen. Jedes aufgenommene Bild enthält ein Tiefenbild, ein IR-Bild, ein Farbbild oder eine Kombination aus Bildern.

Standardmäßig gibt die API eine Aufnahme nur zurück, wenn alle angeforderten Bilder für den Streamingmodus empfangen wurden. Sie können die API so konfigurieren, dass partielle Aufnahmen nur mit Tiefen- oder Farbbildern zurückgegeben werden, sobald diese verfügbar sind. Dazu müssen Sie den Parameter [`synchronized_images_only`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t_a8208974f05d89fc1362c6a0900bdef4d.html#a8208974f05d89fc1362c6a0900bdef4d) von [`k4a_device_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) löschen.

```C
// Capture a depth frame
switch (k4a_device_get_capture(device, &capture, TIMEOUT_IN_MS))
{
case K4A_WAIT_RESULT_SUCCEEDED:
    break;
case K4A_WAIT_RESULT_TIMEOUT:
    printf("Timed out waiting for a capture\n");
    continue;
    break;
case K4A_WAIT_RESULT_FAILED:
    printf("Failed to read a capture\n");
    goto Exit;
}
```

Nachdem die API eine Aufnahme erfolgreich zurückgegeben hat, müssen Sie [`k4a_capture_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6) aufrufen, wenn Sie das Aufnahmeobjekt nicht mehr verwenden.

## <a name="get-an-image-from-the-capture"></a>Abrufen eines Bilds aus der Aufnahme

Zum Abrufen eines aufgenommenen Bilds rufen Sie die entsprechende Funktion für jeden Bildtyp auf. Enthält einen der folgenden Werte:

- [`k4a_capture_get_color_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga683e440b5f22215a2de58d7fa140488c.html#ga683e440b5f22215a2de58d7fa140488c)
- [`k4a_capture_get_depth_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gafa03513da96bf6b8c254fc11a04ee6d6.html#gafa03513da96bf6b8c254fc11a04ee6d6)
- [`k4a_capture_get_ir_image()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga1531c3fa76a7c174b8f2eab24de91794.html#ga1531c3fa76a7c174b8f2eab24de91794)

Sie müssen [`k4a_image_release()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga27c81863b13fafc3934a32935a014e9f.html#ga27c81863b13fafc3934a32935a014e9f) in jedem [`k4a_image_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__image__t.html)-Handle aufrufen, das von diesen Funktionen zurückgegeben wird, wenn Sie das Bild nicht mehr verwenden.

## <a name="access-image-buffers"></a>Zugreifen auf Bildpuffer

[`k4a_image_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__image__t.html) enthält viele Accessorfunktionen zum Abrufen von Eigenschaften des Bilds.

Verwenden Sie [kk4a_image_get_buffer](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga2ef070cf4e543cd0f726478af332546e.html#ga2ef070cf4e543cd0f726478af332546e), um auf den Speicherpuffer des Bilds zuzugreifen.

Im folgenden Beispiel wird der Zugriff auf ein aufgenommenes Tiefenbild veranschaulicht. Das gleiche Prinzip gilt auch für andere Bildtypen. Achten Sie jedoch darauf, dass Sie die Variable „image-type“ durch den entsprechenden Bildtyp ersetzen, z. B. „IR“ oder „color“.

```C
// Access the depth16 image
k4a_image_t image = k4a_capture_get_depth_image(capture);
if (image != NULL)
{
    printf(" | Depth16 res:%4dx%4d stride:%5d\n",
            k4a_image_get_height_pixels(image),
            k4a_image_get_width_pixels(image),
            k4a_image_get_stride_bytes(image));

    // Release the image
    k4a_image_release(image);
}

// Release the capture
k4a_capture_release(capture);
```

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie Sie mit dem Azure Kinect-Gerät die Kamerabilder zwischen Farb- und Tiefenkamera aufnehmen und kombinieren. Sie haben folgende weitere Möglichkeiten:

>[!div class="nextstepaction"]
>[Abrufen von IMU-Stichproben](retrieve-imu-samples.md)

>[!div class="nextstepaction"]
>[Zugreifen auf das Mikrofon](access-mics.md)
