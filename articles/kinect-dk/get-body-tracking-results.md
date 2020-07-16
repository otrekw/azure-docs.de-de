---
title: 'Azure Kinect: Abrufen von Bodytrackingergebnissen'
description: In diesem Artikel erfahren Sie, wie Sie mit dem Bodytracking-SDK für Azure Kinect Bodytrackingergebnisse abrufen.
author: qm13
ms.prod: kinect-dk
ms.author: quentinm
ms.reviewer: yijwan
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, SDK, Körper, Tracking, gemeinsam
ms.openlocfilehash: 1b62022242144d5db51455a32ac04b67c3e5dd7a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276756"
---
# <a name="get-body-tracking-results"></a>Abrufen der Body Tracking-Ergebnisse

Das Bodytracking-SDK verarbeitet Aufnahmen von Azure Kinect DK mithilfe eines Bodytrackerobjekts und generiert Bodytrackingergebnisse. Außerdem wird mithilfe des SDK der globale Status des Trackers, der Verarbeitungswarteschlangen und der Ausgabewarteschlange verwaltet. Die Verwendung des Bodytrackers umfasst drei Schritte:

- Erstellen eines Trackers
- Erfassen von Tiefen- und Infrarotbildern mithilfe von Azure Kinect DK
- Einreihen der Aufzeichnungen in eine Warteschlange und Ausgeben der Ergebnisse

## <a name="create-a-tracker"></a>Erstellen eines Trackers


Der erste Schritt bei der Verwendung des Bodytrackers besteht darin, einen Tracker zu erstellen. Dazu muss die Struktur [k4a_calibration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__calibration__t.html) für die Sensorkalibrierung übergeben werden. Die Sensorkalibrierung kann mithilfe der Funktion [k4a_device_get_calibration()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4e43940d8d8db48da266c7a7842c8d78.html#ga4e43940d8d8db48da266c7a7842c8d78) des Azure Kinect Sensor SDK abgefragt werden.

```C
k4a_calibration_t sensor_calibration;
if (K4A_RESULT_SUCCEEDED != k4a_device_get_calibration(device, device_config.depth_mode, K4A_COLOR_RESOLUTION_OFF, &sensor_calibration))
{
    printf("Get depth camera calibration failed!\n");
    return 0;
}

k4abt_tracker_t tracker = NULL;
k4abt_tracker_configuration_t tracker_config = K4ABT_TRACKER_CONFIG_DEFAULT;
if (K4A_RESULT_SUCCEEDED != k4abt_tracker_create(&sensor_calibration, tracker_config, &tracker))
{
    printf("Body tracker initialization failed!\n");
    return 0;
}
```

## <a name="capture-depth-and-ir-images"></a>Aufnehmen von Tiefen- und Infrarotbildern

Wie Bilder mithilfe von Azure Kinect DK aufgenommen werden, wird auf der Seite [Abrufen von Bildern](retrieve-images.md) behandelt.

>[!NOTE]
> Für eine optimale Leistung und Genauigkeit werden die Modi `K4A_DEPTH_MODE_NFOV_UNBINNED` oder `K4A_DEPTH_MODE_WFOV_2X2BINNED` empfohlen. Verwenden Sie nicht die Modi `K4A_DEPTH_MODE_OFF` oder `K4A_DEPTH_MODE_PASSIVE_IR`.

Die unterstützten Azure Kinect DK-Modi werden in der [Hardwarespezifikation](hardware-specification.md) und der Enumeration [k4a_depth_mode_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga3507ee60c1ffe1909096e2080dd2a05d.html#ga3507ee60c1ffe1909096e2080dd2a05d) von Azure Kinect DK beschrieben.

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

## <a name="enqueue-the-capture-and-pop-the-results"></a>Einstellen der Aufzeichnung in die Warteschlange und Abrufen der Ergebnisse

Die Nachverfolgung verwaltet intern eine Eingabewarteschlange und eine Ausgabewarteschlange, um die asynchrone Verarbeitung der Azure Kinect DK-Aufzeichnungen effizienter durchzuführen. Mit der Funktion [k4abt_tracker_enqueue_capture()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga093becd9bb4a63f5f4d56f58097a7b1e.html#ga093becd9bb4a63f5f4d56f58097a7b1e) fügen Sie der Eingabewarteschlange eine neue Aufzeichnung hinzu. Mit der Funktion [k4abt_tracker_pop_result()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gaaf446fb1579cbbe0b6af824ee0a7458b.html#gaaf446fb1579cbbe0b6af824ee0a7458b) können Sie ein Ergebnis aus der Ausgabewarteschlange ausgeben. Die Verwendung des Timeoutwerts hängt von der Anwendung ab und steuert die Wartezeit beim Queuing.

### <a name="real-time-processing"></a>Echtzeitverarbeitung
Verwenden Sie dieses Muster für Singlethreadanwendungen, die Echtzeitergebnisse benötigen und verworfene Frames aufnehmen können. `simple_3d_viewer` in den [Azure Kinect-Beispielen auf GitHub](https://github.com/microsoft/Azure-Kinect-Samples) ist ein Beispiel für die Echtzeitverarbeitung.

```C
k4a_wait_result_t queue_capture_result = k4abt_tracker_enqueue_capture(tracker, sensor_capture, 0);
k4a_capture_release(sensor_capture); // Remember to release the sensor capture once you finish using it
if (queue_capture_result == K4A_WAIT_RESULT_FAILED)
{
    printf("Error! Adding capture to tracker process queue failed!\n");
    break;
}

k4abt_frame_t body_frame = NULL;
k4a_wait_result_t pop_frame_result = k4abt_tracker_pop_result(tracker, &body_frame, 0);
if (pop_frame_result == K4A_WAIT_RESULT_SUCCEEDED)
{
    // Successfully popped the body tracking result. Start your processing
    ...

    k4abt_frame_release(body_frame); // Remember to release the body frame once you finish using it
}
```

### <a name="synchronous-processing"></a>Synchrone Verarbeitung
Verwenden Sie dieses Muster für Anwendungen, die keine Echtzeitergebnisse benötigen oder keine verworfenen Frames aufnehmen können.

Der Verarbeitungsdurchsatz ist unter Umständen eingeschränkt.

`simple_sample.exe` in den [Azure Kinect-Beispielen auf GitHub](https://github.com/microsoft/Azure-Kinect-Samples) ist ein Beispiel für die synchrone Verarbeitung.

```C
k4a_wait_result_t queue_capture_result = k4abt_tracker_enqueue_capture(tracker, sensor_capture, K4A_WAIT_INFINITE);
k4a_capture_release(sensor_capture); // Remember to release the sensor capture once you finish using it
if (queue_capture_result != K4A_WAIT_RESULT_SUCCEEDED)
{
    // It should never hit timeout or error when K4A_WAIT_INFINITE is set.
    printf("Error! Adding capture to tracker process queue failed!\n");
    break;
}

k4abt_frame_t body_frame = NULL;
k4a_wait_result_t pop_frame_result = k4abt_tracker_pop_result(tracker, &body_frame, K4A_WAIT_INFINITE);
if (pop_frame_result != K4A_WAIT_RESULT_SUCCEEDED)
{
    // It should never hit timeout or error when K4A_WAIT_INFINITE is set.
    printf("Error! Popping body tracking result failed!\n");
    break;
}
// Successfully popped the body tracking result. Start your processing
...

k4abt_frame_release(body_frame); // Remember to release the body frame once you finish using it
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Zugreifen auf Daten in einem Bodyframe](access-data-body-frame.md)
