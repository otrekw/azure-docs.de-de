---
title: Abrufen von IMU-Stichproben für Azure Kinect
description: Erfahren Sie, wie Sie mit dem Azure Kinect SDK IMU-Stichproben für Azure Kinect abrufen können.
author: qm13
ms.author: cedmonds
ms.date: 06/26/2019
ms.prod: kinect-dk
ms.topic: conceptual
keywords: Kinect, Azure, konfigurieren, Tiefe, Farbe, RBG, Kamera, Sensor, SDK, IMU, Bewegungssensor, Bewegung, Gyroskop, Gyrometer, Beschleunigungsmesser, BpS
ms.openlocfilehash: 649dd5b9de62d43d59e74d53adff1ec7de8dfd32
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85276689"
---
# <a name="retrieve-azure-kinect-imu-samples"></a>Abrufen von IMU-Stichproben für Azure Kinect

Das Azure Kinect-Gerät bietet Zugriff auf IMUs (Inertial Motion Units, inertiale Bewegungseinheiten), einschließlich Beschleunigungsmesser und Gyroskop. Für den Zugriff auf IMU-Stichproben müssen Sie zunächst Ihr Gerät öffnen und konfigurieren und dann IMU-Daten aufnehmen. Weitere Informationen finden Sie unter [Suchen und Öffnen des Geräts](find-then-open-device.md).

IMU-Stichproben werden mit einer deutlich höheren Frequenz als Bilder generiert. Stichproben werden mit einer niedrigeren Rate als bei ihrer Entnahme an den Host gesendet. Beim Warten auf eine IMU-Stichprobe werden häufig mehrere Stichproben gleichzeitig verfügbar.

Ausführliche Informationen zur IMU-Berichterstellungsrate finden Sie in der [Hardwarespezifikation](hardware-specification.md) für Azure Kinect DK.

## <a name="configure-and-start-cameras"></a>Konfigurieren und Starten von Kameras

> [!NOTE]
> IMU-Sensoren können nur verwendet werden, wenn die Farb- oder Tiefenkameras ausgeführt werden. IMU-Sensoren können nicht allein verwendet werden.

Verwenden Sie [k4a_device_start_cameras()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a), um die Kameras zu starten.

```C
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps = K4A_FRAMES_PER_SECOND_30;
config.color_format = K4A_IMAGE_FORMAT_COLOR_MJPG;
config.color_resolution = K4A_COLOR_RESOLUTION_2160P;

if (K4A_RESULT_SUCCEEDED != k4a_device_start_cameras(device, &config))
{
    printf("Failed to start cameras\n");
    goto Exit;
}

if (K4A_RESULT_SUCCEEDED != k4a_device_start_imu(device))
{
    printf("Failed to start imu\n");
    goto Exit;
}
```

## <a name="access-imu-samples"></a>Zugreifen auf IMU-Stichproben

 Jeder [k4a_imu_sample_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__imu__sample__t.html#details)-Datensatz beinhaltet einen Messwert für den Beschleunigungsmesser und das Gyroskop, die fast gleichzeitig aufgenommen werden.

Sie können die IMU-Stichproben in dem Thread, mit dem Sie Bilder aufnehmen, oder in separaten Threads abrufen.

Um IMU-Stichproben abzurufen, sobald sie verfügbar sind, können Sie [`k4a_device_get_imu_sample()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8e5913b3bb94a453c7143bbd6e399a0e.html#ga8e5913b3bb94a453c7143bbd6e399a0e) in einem eigenen Thread aufrufen. Die API verfügt auch über genügend interne Warteschlangen, damit Sie erst nach der Rückgabe jeder Bildaufnahme nach Stichproben suchen können.

Da es einige interne Warteschlangen für IMU-Stichproben gibt, können Sie sich an folgender Vorgehensweise orientieren, ohne Daten zu löschen:

1. Warten Sie auf eine Aufnahme mit einer beliebigen Bildrate.
2. Verarbeiten Sie die Aufnahme.
3. Rufen Sie alle IMU-Stichproben in der Warteschlange ab.
4. Warten Sie wieder auf die nächste Aufnahme.

Um alle derzeit in die Warteschlange eingereihten IMU-Stichproben abzurufen, können Sie [`k4a_device_get_imu_sample()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8e5913b3bb94a453c7143bbd6e399a0e.html#ga8e5913b3bb94a453c7143bbd6e399a0e) mit dem Wert „0“ für `timeout_in_ms` in einer Schleife aufrufen, bis die Funktion `K4A_WAIT_RESULT_TIMEOUT` zurückgibt. `K4A_WAIT_RESULT_TIMEOUT` gibt an, dass sich keine Stichproben in der Warteschlange befinden und dass keine im angegebenen Timeout eingegangen sind.

## <a name="usage-example"></a>Verwendungsbeispiel

```C
k4a_imu_sample_t imu_sample;

// Capture a imu sample
switch (k4a_device_get_imu_sample(device, &imu_sample, TIMEOUT_IN_MS))
{
case K4A_WAIT_RESULT_SUCCEEDED:
    break;
case K4A_WAIT_RESULT_TIMEOUT:
    printf("Timed out waiting for a imu sample\n");
    continue;
    break;
case K4A_WAIT_RESULT_FAILED:
    printf("Failed to read a imu sample\n");
    goto Exit;
}

// Access the accelerometer readings
if (imu_sample != NULL)
{
    printf(" | Accelerometer temperature:%.2f x:%.4f y:%.4f z: %.4f\n",
            imu_sample.temperature,
            imu_sample.acc_sample.xyz.x,
            imu_sample.acc_sample.xyz.y,
            imu_sample.acc_sample.xyz.z);
}

```

## <a name="next-steps"></a>Nächste Schritte

Sie haben sich mit IMU-Stichproben vertraut gemacht. Daher können Sie sich nun folgendem Thema widmen:
>[!div class="nextstepaction"]
>[Zugreifen auf Mikrofoneingangsdaten](access-mics.md)
