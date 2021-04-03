---
title: Suchen und Öffnen des Azure Kinect-Geräts
description: Erfahren Sie, wie Sie mit dem Azure Kinect Sensor SDK ein Azure Kinect-Gerät finden und öffnen.
author: cedmonds
ms.author: cedmonds
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, Tiefe, RGB, Gerät, suchen, öffnen
ms.openlocfilehash: 67fc93b924d5d663bb43098969c54d1975bd5895
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "85276665"
---
# <a name="find-then-open-the-azure-kinect-device"></a>Suchen und Öffnen des Azure Kinect-Geräts

In diesem Artikel wird beschrieben, wie Sie Ihr Azure Kinect DK finden und danach öffnen. Es wird auch erläutert, was in einem Szenario zu tun ist, in dem mehrere Geräte mit Ihrem Computer verbunden sind.

Informationen finden Sie auch auf GitHub im [Beispiel für die SDK-Enumerierung](https://github.com/microsoft/Azure-Kinect-Sensor-SDK/tree/develop/examples/enumerate), das die Verwendung der Funktionen in diesem Artikel veranschaulicht.

Die folgenden Funktionen werden behandelt:
 * [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc)
 * [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113)
 * [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22)
 * [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423)

## <a name="discover-the-number-of-connected-devices"></a>Ermitteln der Anzahl von verbundenen Geräten

Rufen Sie zunächst mit [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc) die Anzahl der aktuell verbundenen Azure Kinect-Geräte ab.

```C
uint32_t device_count = k4a_device_get_installed_count();

printf("Found %d connected devices:\n", device_count);
```

## <a name="open-a-device"></a>Öffnen eines Geräts

Um Informationen über ein Gerät abzurufen oder Daten von diesem Gerät zu lesen, müssen Sie zuerst mit [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) ein Handle für das Gerät öffnen.

```C
k4a_device_t device = NULL;

for (uint8_t deviceIndex = 0; deviceIndex < device_count; deviceIndex++)
{
    if (K4A_RESULT_SUCCEEDED != k4a_device_open(deviceIndex, &device))
    {
        printf("%d: Failed to open device\n", deviceIndex);
        continue;
    }

    ...

    k4a_device_close(device);
}
```

Der `index`-Parameter von [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) gibt an, welches Gerät geöffnet werden soll, wenn mehrere verbunden sind. Wenn voraussichtlich nur ein Gerät verbunden ist, können Sie `K4A_DEVICE_DEFAULT` oder 0 als Argument übergeben, um das erste Gerät anzugeben.

Wann immer Sie ein Gerät öffnen, müssen Sie [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423) aufrufen, wenn die Verwendung des Handles beendet ist. Andere Handles können erst dann auf demselben Gerät geöffnet werden, wenn dieses Handle geschlossen ist.

## <a name="identify-a-specific-device"></a>Identifizieren eines bestimmten Geräts

Die Reihenfolge, in der Geräte nach dem Index aufgelistet werden, ändert sich nur, wenn Geräte angefügt oder getrennt werden. Um ein physisches Gerät zu identifizieren, sollten Sie die Seriennummer des Geräts verwenden.

Zum Auslesen der Seriennummer vom Gerät verwenden Sie die Funktion [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22), nachdem Sie ein Handle geöffnet haben.

Dieses Beispiel veranschaulicht, wie Sie die richtige Menge an Arbeitsspeicher zuordnen, um die Seriennummer zu speichern.

```C
char *serial_number = NULL;
size_t serial_number_length = 0;

if (K4A_BUFFER_RESULT_TOO_SMALL != k4a_device_get_serialnum(device, NULL, &serial_number_length))
{
    printf("%d: Failed to get serial number length\n", deviceIndex);
    k4a_device_close(device);
    device = NULL;
    continue;
}

serial_number = malloc(serial_number_length);
if (serial_number == NULL)
{
    printf("%d: Failed to allocate memory for serial number (%zu bytes)\n", deviceIndex, serial_number_length);
    k4a_device_close(device);
    device = NULL;
    continue;
}

if (K4A_BUFFER_RESULT_SUCCEEDED != k4a_device_get_serialnum(device, serial_number, &serial_number_length))
{
    printf("%d: Failed to get serial number\n", deviceIndex);
    free(serial_number);
    serial_number = NULL;
    k4a_device_close(device);
    device = NULL;
    continue;
}

printf("%d: Device \"%s\"\n", deviceIndex, serial_number);
```

## <a name="open-the-default-device"></a>Öffnen des Standardgeräts

In den meisten Anwendungen ist wahrscheinlich nur ein einziges Azure Kinect DK-Gerät an einen Computer angefügt. Wenn Sie nur mit diesem einen erwarteten Gerät eine Verbindung herstellen müssen, können Sie [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) mit `K4A_DEVICE_DEFAULT` als `index` aufrufen, um das erste Gerät zu öffnen.

```C
k4a_device_t device = NULL;
uint32_t device_count = k4a_device_get_installed_count();

if (device_count != 1)
{
    printf("Unexpected number of devices found (%d)\n", device_count);
    goto Exit;
}

if (K4A_RESULT_SUCCEEDED != k4a_device_open(K4A_DEVICE_DEFAULT, &device))
{
    printf("Failed to open device\n");
    goto Exit;
}
```

## <a name="next-steps"></a>Nächste Schritte

>[!div class="nextstepaction"]
>[Abrufen von Bildern](retrieve-images.md)

>[!div class="nextstepaction"]
>[Abrufen von IMU-Beispielen](retrieve-imu-samples.md)

