---
title: Erfassen der Azure Kinect-Gerätesynchronisierung
description: Erfahren Sie, wie Sie Azure Kinect-Aufnahmegeräte mit dem Sensor-SDK für Azure Kinect synchronisieren.
author: xthexder
ms.author: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, SDK, Tiefe, RGB, intern, extern, Synchronisierung, Verkettung, Phasendifferenz
ms.openlocfilehash: ce0c72d3d708d5696a9775b3885f278f0c23cac1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102034307"
---
# <a name="capture-azure-kinect-device-synchronization"></a>Erfassen der Azure Kinect-Gerätesynchronisierung

Mit der Azure Kinect-Hardware lässt sich die Erfassungszeit von Farb- und Tiefenbildern aufeinander abstimmen. Bei der Anpassung von Kameras auf ein und demselben Gerät spricht man von **interner Synchronisierung**. Die Anpassung der Erfassungszeit auf mehreren verbundenen Geräten ist die **externe Synchronisierung**. Das Mikrofonarray arbeitet unabhängig von den Farb- und Tiefenkameras.

## <a name="device-internal-synchronization"></a>Interne Synchronisierung

Die Bilderfassung der einzelnen Kameras wird in der Hardware synchronisiert. Für jedes [k4a_capture_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__capture__t.html)-Element, das Bilder vom Farb- und Tiefensensor enthält, werden die Zeitstempel der Bilder basierend auf dem Betriebsmodus der Hardware aufeinander abgestimmt. Standardmäßig werden die Bilder einer Aufnahme auf den Mittelpunkt der Belichtung abgestimmt. Die relativen Zeitwerte von Tiefen- und Farbaufnahmen können über das Feld `depth_delay_off_color_usec` von [k4a_device_configuration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html) angepasst werden.

## <a name="device-external-synchronization"></a>Externe Synchronisierung

Informationen zum Einrichten der Hardware finden Sie unter [Einrichten der externen Synchronisierung](https://support.microsoft.com/help/4494429/sync-multiple-azure-kinect-dk-devices).

Die Software für die einzelnen verbundenen Geräte muss für den Betrieb im **Master**- oder **untergeordneten** Modus konfiguriert werden. Diese Einstellung wird über das [k4a_device_configuration_t](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__device__configuration__t.html)-Element konfiguriert.

Bei Verwendung der externen Synchronisierung sollten untergeordnete Kameras immer vor der Masterkamera gestartet werden, damit die Zeitstempel ordnungsgemäß aufeinander abgestimmt werden.

### <a name="subordinate-mode"></a>Untergeordneter Modus

```C
k4a_device_configuration_t deviceConfig;
deviceConfig.wired_sync_mode = K4A_WIRED_SYNC_MODE_SUBORDINATE
```

### <a name="master-mode"></a>Master-Modus

```C
k4a_device_configuration_t deviceConfig;
deviceConfig.wired_sync_mode = K4A_WIRED_SYNC_MODE_MASTER;
```

### <a name="retrieving-synchronization-jack-state"></a>Abrufen des Status der Synchronisierungsbuchsen

Verwenden Sie die Funktion [k4a_device_get_sync_jack](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0209ac87bfd055163677321b0304e962.html#ga0209ac87bfd055163677321b0304e962), um den aktuellen Status der Eingangs- und Ausgangsbuchsen für die Synchronisierung programmgesteuert abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie Sie die Gerätesynchronisierung aktivieren und erfassen. Als Nächstes können Sie sich erneut mit folgendem Thema befassen: 

>[!div class="nextstepaction"]
>[Aufnahme- und Wiedergabe-API des Sensor-SDK für Azure Kinect](record-playback-api.md)
