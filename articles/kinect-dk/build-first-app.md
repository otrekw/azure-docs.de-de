---
title: 'Schnellstart: Erstellen Ihrer ersten Azure Kinect-Anwendung'
description: In dieser Schnellstartanleitung wird der Azure Kinect DK-Benutzer durch die Erstellung einer neuen Anleitung geführt.
author: tesych
ms.author: tesych
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: quickstart
keywords: Kinect, Azure, Sensor, SDK, Mikrofon, Mikrofonzugriff, Mikrofondaten
ms.openlocfilehash: 3632145b3f3b63023e0c66e3cf99903231802edf
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "85277789"
---
# <a name="quickstart-build-your-first-azure-kinect-application"></a>Schnellstart: Erstellen Ihrer ersten Azure Kinect-Anwendung

Machen Sie Ihre ersten Schritte mit dem Azure Kinect DK? Mit dieser Schnellstartanleitung geht Ihr Gerät gut gerüstet an den Start!

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Die folgenden Funktionen werden behandelt:

- [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc)
- [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113)
- [`k4a_device_get_serialnum()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga798489af207ff1c99f2285ff6b08bc22.html#ga798489af207ff1c99f2285ff6b08bc22)
- [`k4a_device_start_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaad7a85e1e5471810262442fc4a8e217a.html#gaad7a85e1e5471810262442fc4a8e217a)
- [`k4a_device_stop_cameras()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga4fa0e0a011a7105309ad97f081a5d6b8.html#ga4fa0e0a011a7105309ad97f081a5d6b8)
- [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423)

## <a name="prerequisites"></a>Voraussetzungen

1. [Einrichten des Azure Kinect DK-Geräts](set-up-azure-kinect-dk.md).
2. [Herunterladen](sensor-sdk-download.md) und installieren des Azure Kinect Sensor SDK.

## <a name="headers"></a>Header

Sie benötigen lediglich einen Header, nämlich `k4a.h`. Vergewissern Sie sich, dass der Compiler Ihrer Wahl mit den lib- und include-Ordnern des SDK eingerichtet ist. Ferner müssen die Dateien `k4a.lib` und `k4a.dll` eingebunden sein. Informationen dazu finden Sie unter [Hinzufügen der Azure Kinect-Bibliothek zu Ihrem Visual Studio-Projekt](add-library-to-project.md).

```C
#include <k4a/k4a.h>
```

## <a name="finding-an-azure-kinect-dk-device"></a>Auffinden eines Azure Kinect DK-Geräts

Es können mehrere Azure Kinect DK-Geräte mit Ihrem Computer verbunden sein. Zunächst ermitteln wir mithilfe der [`k4a_device_get_installed_count()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaf7d19df0f73f8e4dfaa21e1b4b719ecc.html#gaf7d19df0f73f8e4dfaa21e1b4b719ecc)-Funktion die Anzahl der verbundenen Geräte bzw. ob überhaupt verbundene Geräte vorhanden sind. Diese Funktion sollte sofort und ohne weitere Einrichtung funktionieren.

```C
uint32_t count = k4a_device_get_installed_count();
```

Nachdem Sie festgestellt haben, dass ein Gerät mit dem Computer verbunden ist, können Sie es mithilfe von [`k4a_device_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga3d4eb5dfbf4d576d4978b66ea419f113.html#ga3d4eb5dfbf4d576d4978b66ea419f113) öffnen. Sie können den Index des Geräts angeben, das Sie öffnen möchten, oder für das erste einfach `K4A_DEVICE_DEFAULT` verwenden.

```C
// Open the first plugged in Kinect device
k4a_device_t device = NULL;
k4a_device_open(K4A_DEVICE_DEFAULT, &device);
```
Für die meisten Dinge in der Azure Kinect-Bibliothek gilt, wenn Sie etwas öffnen, sollten Sie es nach Gebrauch auch wieder schließen! Denken Sie daran, beim Herunterfahren [`k4a_device_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga7a3931d9a690b3971caaac83b43f9423.html#ga7a3931d9a690b3971caaac83b43f9423) aufzurufen.

```C
k4a_device_close(device);
```

Sobald das Gerät geöffnet ist, können wir einen Test vornehmen, um sicherzustellen, dass es funktioniert. Lesen wir also einmal die Seriennummer des Geräts aus!

```C
// Get the size of the serial number
size_t serial_size = 0;
k4a_device_get_serialnum(device, NULL, &serial_size);

// Allocate memory for the serial, then acquire it
char *serial = (char*)(malloc(serial_size));
k4a_device_get_serialnum(device, serial, &serial_size);
printf("Opened device: %s\n", serial);
free(serial);
```

## <a name="starting-the-cameras"></a>Starten der Kameras

Nachdem Sie das Gerät geöffnet haben, müssen Sie die Kamera mit einem [`k4a_device_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html)-Objekt konfigurieren. Die Kamerakonfiguration weist eine Reihe verschiedener Optionen auf. Wählen Sie die Einstellungen, die optimal zu Ihrem eigenen Szenario passen.

```C
// Configure a stream of 4096x3072 BRGA color data at 15 frames per second
k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
config.camera_fps       = K4A_FRAMES_PER_SECOND_15;
config.color_format     = K4A_IMAGE_FORMAT_COLOR_BGRA32;
config.color_resolution = K4A_COLOR_RESOLUTION_3072P;

// Start the camera with the given configuration
k4a_device_start_cameras(device, &config);

// ...Camera capture and application specific code would go here...

// Shut down the camera when finished with application logic
k4a_device_stop_cameras(device);
```

## <a name="error-handling"></a>Fehlerbehandlung

Aus Gründen der Übersichtlichkeit und Deutlichkeit wird die Fehlerbehandlung in einigen Inlinebeispielen nicht gezeigt. Fehlerbehandlung ist aber immer wichtig! Viele Funktionen geben einen allgemeinen Erfolgs-/Fehlertyp [`k4a_result_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga4b419a99aa2220b076a4520dc2afd1e5.html#ga4b419a99aa2220b076a4520dc2afd1e5) oder eine spezifischere Variante mit Detailinformationen zurück, wie etwa [`k4a_wait_result_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___enumerations_ga44c7c0c1cfba7c879e9e2da1a869e4ee.html#ga44c7c0c1cfba7c879e9e2da1a869e4ee). Schlagen Sie für jede Funktion in der Dokumentation oder in IntelliSense nach, um zu sehen, mit welchen Fehlermeldungen Sie rechnen müssen!

Sie können die Makros [`K4A_SUCCEEDED`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___macros_ga8e5b48150bc243c6052793bd830c2fcd.html#ga8e5b48150bc243c6052793bd830c2fcd) und [`K4A_FAILED`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___macros_ga7c2e32349135d008b6f836c571d434b4.html#ga7c2e32349135d008b6f836c571d434b4) verwenden, um das Ergebnis einer Funktion zu überprüfen. Anstatt ein Azure Kinect DK-Gerät einfach zu öffnen, könnten wir den Funktionsaufruf etwa in folgender Weise schützen:

```C
// Open the first plugged in Kinect device
k4a_device_t device = NULL;
if ( K4A_FAILED( k4a_device_open(K4A_DEVICE_DEFAULT, &device) ) )
{
    printf("Failed to open k4a device!\n");
    return;
}
```

## <a name="full-source"></a>Vollständiger Quellcode

```C
#pragma comment(lib, "k4a.lib")
#include <k4a/k4a.h>

#include <stdio.h>
#include <stdlib.h>

int main()
{
    uint32_t count = k4a_device_get_installed_count();
    if (count == 0)
    {
        printf("No k4a devices attached!\n");
        return 1;
    }

    // Open the first plugged in Kinect device
    k4a_device_t device = NULL;
    if (K4A_FAILED(k4a_device_open(K4A_DEVICE_DEFAULT, &device)))
    {
        printf("Failed to open k4a device!\n");
        return 1;
    }

    // Get the size of the serial number
    size_t serial_size = 0;
    k4a_device_get_serialnum(device, NULL, &serial_size);

    // Allocate memory for the serial, then acquire it
    char *serial = (char*)(malloc(serial_size));
    k4a_device_get_serialnum(device, serial, &serial_size);
    printf("Opened device: %s\n", serial);
    free(serial);

    // Configure a stream of 4096x3072 BRGA color data at 15 frames per second
    k4a_device_configuration_t config = K4A_DEVICE_CONFIG_INIT_DISABLE_ALL;
    config.camera_fps       = K4A_FRAMES_PER_SECOND_15;
    config.color_format     = K4A_IMAGE_FORMAT_COLOR_BGRA32;
    config.color_resolution = K4A_COLOR_RESOLUTION_3072P;

    // Start the camera with the given configuration
    if (K4A_FAILED(k4a_device_start_cameras(device, &config)))
    {
        printf("Failed to start cameras!\n");
        k4a_device_close(device);
        return 1;
    }

    // Camera capture and application specific code would go here

    // Shut down the camera when finished with application logic
    k4a_device_stop_cameras(device);
    k4a_device_close(device);

    return 0;
}

```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie ein Azure Kinect DK-Gerät mithilfe von Sensor SDK öffnen
> [!div class="nextstepaction"]
>[Suchen und Öffnen eines Geräts](find-then-open-device.md)
