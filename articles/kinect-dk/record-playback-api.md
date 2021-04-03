---
title: Azure Kinect-Wiedergabe-API
description: Erfahren Sie, wie Sie das Sensor-SDK für Azure Kinect verwenden, um eine Aufzeichnungsdatei mithilfe der Wiedergabe-API zu öffnen.
author: xthexder
ms.author: jawirth
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, Azure, Sensor, SDK, Tiefe, RGB, Aufzeichnung, Wiedergabe, Matroska, MKV
ms.openlocfilehash: fe403f314c1df415537d090433f34627eb1249e7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "85276659"
---
# <a name="the-azure-kinect-playback-api"></a>Die Azure Kinect-Wiedergabe-API

Das Sensor-SDK beinhaltet eine API, um Gerätedaten in einer Matroska-Datei (.mkv) zu erfassen. Im Matroska-Containerformat werden Videospuren, IME-Beispiele und die Gerätekalibrierung gespeichert. Aufzeichnungen können über das bereitgestellte Befehlszeilen-Hilfsprogramm [k4arecorder](record-sensor-streams-file.md) generiert werden. Über die Aufnahme-API können diese Aufzeichnungen zudem angepasst und direkt erfasst werden.

Weitere Informationen zur Aufnahme-API finden Sie unter [`k4a_record_create()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gae14f4181e9688e710d1c80b215413831.html#gae14f4181e9688e710d1c80b215413831).

Weitere Informationen zu den Spezifikationen des Matroska-Dateiformats finden Sie unter [Format der Aufzeichnungsdatei](record-file-format.md).

## <a name="use-the-playback-api"></a>Verwenden der Wiedergabe-API

Aufzeichnungsdateien können mit der Wiedergabe-API geöffnet werden. Die Wiedergabe-API bietet Zugriff auf Sensordaten im gleichen Format wie die übrigen Elemente des Sensor-SDKs.

### <a name="open-a-record-file"></a>Öffnen einer Aufzeichnungsdatei

Im folgenden Beispiel wird unter Verwendung von [`k4a_playback_open()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gacb254ac941b2ab3c202ca68f4537f368.html#gacb254ac941b2ab3c202ca68f4537f368) eine Aufzeichnungsdatei geöffnet. Dann wird die Aufzeichnungslänge gedruckt und die Datei mit [`k4a_playback_close()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga76f415f2076f1c8c544e094a649306ff.html#ga76f415f2076f1c8c544e094a649306ff) geschlossen.

```C
k4a_playback_t playback_handle = NULL;
if (k4a_playback_open("recording.mkv", &playback_handle) != K4A_RESULT_SUCCEEDED)
{
    printf("Failed to open recording\n");
    return 1;
}

uint64_t recording_length = k4a_playback_get_last_timestamp_usec(playback_handle);
printf("Recording is %lld seconds long\n", recording_length / 1000000);

k4a_playback_close(playback_handle);
```

### <a name="read-captures"></a>Lesen von Aufnahmen

Nachdem die Datei geöffnet wurde, können wir mit dem Lesen von Aufnahmen in der Aufzeichnung beginnen. Im nächsten Beispiel werden die einzelnen Aufnahmen in der Datei gelesen.

```C
k4a_capture_t capture = NULL;
k4a_stream_result_t result = K4A_STREAM_RESULT_SUCCEEDED;
while (result == K4A_STREAM_RESULT_SUCCEEDED)
{
    result = k4a_playback_get_next_capture(playback_handle, &capture);
    if (result == K4A_STREAM_RESULT_SUCCEEDED)
    {
        // Process capture here
        k4a_capture_release(capture);
    }
    else if (result == K4A_STREAM_RESULT_EOF)
    {
        // End of file reached
        break;
    }
}
if (result == K4A_STREAM_RESULT_FAILED)
{
    printf("Failed to read entire recording\n");
    return 1;
}
```

### <a name="seek-within-a-recording"></a>Suchen innerhalb einer Aufzeichnung

Sobald wir das Ende der Datei erreicht haben, kann es sinnvoll oder erforderlich sein, die Datei noch einmal zu lesen. Dazu können wir die Datei mit [`k4a_playback_get_previous_capture()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga54732e3aa0717e1ca4eb76ee385e878c.html#ga54732e3aa0717e1ca4eb76ee385e878c) rückwärts lesen. Dieser Vorgang kann abhängig von der Länge der Aufzeichnung jedoch sehr langsam sein.
Stattdessen können wir mit der Funktion [`k4a_playback_seek_timestamp()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaea748994a121543bd77f90417cf428f6.html#gaea748994a121543bd77f90417cf428f6) zu einer bestimmten Stelle innerhalb der Datei wechseln.

In diesem Beispiel geben wir Zeitstempel in Mikrosekunden an, um zu verschiedenen Punkten in der Datei zu gehen.

```C
// Seek to the beginning of the file
if (k4a_playback_seek_timestamp(playback_handle, 0, K4A_PLAYBACK_SEEK_BEGIN) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}

// Seek to the end of the file
if (k4a_playback_seek_timestamp(playback_handle, 0, K4A_PLAYBACK_SEEK_END) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}

// Seek to 10 seconds from the start
if (k4a_playback_seek_timestamp(playback_handle, 10 * 1000000, K4A_PLAYBACK_SEEK_BEGIN) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}

// Seek to 10 seconds from the end
if (k4a_playback_seek_timestamp(playback_handle, -10 * 1000000, K4A_PLAYBACK_SEEK_END) != K4A_RESULT_SUCCEEDED)
{
    return 1;
}
```

### <a name="read-tag-information"></a>Lesen von Taginformationen

Aufzeichnungen können zudem über verschiedene Metadaten verfügen (z. B. die Seriennummer des Geräts oder Firmwareversionen). Diese Metadaten werden in Aufzeichnungstags gespeichert, auf die über die Funktion [`k4a_playback_get_tag()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga320f966fc89b4ba0d758f787f70d5143.html#ga320f966fc89b4ba0d758f787f70d5143) zugegriffen werden kann.

```C
// Print the serial number of the device used to record
char serial_number[256];
size_t serial_number_size = 256;
k4a_buffer_result_t buffer_result = k4a_playback_get_tag(playback_handle, "K4A_DEVICE_SERIAL_NUMBER", &serial_number, &serial_number_size);
if (buffer_result == K4A_BUFFER_RESULT_SUCCEEDED)
{
    printf("Device serial number: %s\n", serial_number);
}
else if (buffer_result == K4A_BUFFER_RESULT_TOO_SMALL)
{
    printf("Device serial number too long.\n");
}
else
{
    printf("Tag does not exist. Device serial number was not recorded.\n");
}
```

### <a name="record-tag-list"></a>Liste der Aufzeichnungstags

In der nachfolgenden Liste sind alle Standardtags aufgeführt, die in einer Aufzeichnungsdatei enthalten sein können: Viele dieser Werte sind als Teil der [`k4a_record_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__record__configuration__t.html)-Struktur verfügbar und können mit der Funktion [`k4a_playback_get_record_configuration()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_gaab54a85c1f1e98d170d009042b449255.html#gaab54a85c1f1e98d170d009042b449255) gelesen werden.

Wenn ein Tag nicht vorhanden ist, wird vom Standardwert ausgegangen.

| Tag-Name                     | Standardwert      | Feld [`k4a_record_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__record__configuration__t.html) | Notizen     |
|------------------------------|--------------------|--------------------------------------|----------------------------------------------------------------------------------------------------------------|
| `K4A_COLOR_MODE`             | „OFF“              | `color_format` / `color_resolution`  | Mögliche Werte: „OFF“, „MJPG_1080P“, „NV12_720P“, „YUY2_720P“ usw.                                      |
| `K4A_DEPTH_MODE`             | „OFF“              | `depth_mode` / `depth_track_enabled` | Mögliche Werte: „OFF“, „NFOV_UNBINNED“, „PASSIVE_IR“ usw.                                                |
| `K4A_IR_MODE`                | „OFF“              | `depth_mode` / `ir_track_enabled`    | Mögliche Werte: „OFF“, „ACTIVE“, „PASSIVE“                                                                    |
| `K4A_IMU_MODE`               | „OFF“              | `imu_track_enabled`                  | Mögliche Werte: „ON“, „OFF“                                                                                   |
| `K4A_CALIBRATION_FILE`       | „calibration.json“ | –                                  | Informationen finden Sie unter [`k4a_device_get_raw_calibration()`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga8c4e46642cee3115aeb0b33e2b43b24f.html#ga8c4e46642cee3115aeb0b33e2b43b24f). |
| `K4A_DEPTH_DELAY_NS`         | "0"                | `depth_delay_off_color_usec`         | Der Wert wird in Nanosekunden gespeichert. Die API gibt Werte in Mikrosekunden an.                                                        |
| `K4A_WIRED_SYNC_MODE`        | „STANDALONE“       | `wired_sync_mode`                    | Mögliche Werte: „STANDALONE“, „MASTER“, „SUBORDINATE“                                                         |
| `K4A_SUBORDINATE_DELAY_NS`   | "0"                | `subordinate_delay_off_master_usec`  | Der Wert wird in Nanosekunden gespeichert. Die API gibt Werte in Mikrosekunden an.                                                        |
| `K4A_COLOR_FIRMWARE_VERSION` | ""                 | –                                  | Firmwareversion der Farbeinheit, z. B. „1.x.xx“                                                            |
| `K4A_DEPTH_FIRMWARE_VERSION` | ""                 | –                                  | Firmwareversion der Tiefeneinheit, z. B. „1.x.xx“                                                            |
| `K4A_DEVICE_SERIAL_NUMBER`   | ""                 | –                                  | Aufzeichnung der Seriennummer des Geräts                                                                                 |
| `K4A_START_OFFSET_NS`        | "0"                | `start_timestamp_offset_usec`        | Weitere Informationen finden Sie unter [Synchronisieren von Zeitstempeln](record-playback-api.md#timestamp-synchronization) weiter unten.                       |
| `K4A_COLOR_TRACK`            | Keine               | –                                  | Weitere Informationen finden Sie unter [Format der Aufzeichnungsdatei – Identifizieren von Spuren](record-file-format.md#identifying-tracks).                     |
| `K4A_DEPTH_TRACK`            | Keine               | –                                  | Weitere Informationen finden Sie unter [Format der Aufzeichnungsdatei – Identifizieren von Spuren](record-file-format.md#identifying-tracks).                     |
| `K4A_IR_TRACK`               | Keine               | –                                  | Weitere Informationen finden Sie unter [Format der Aufzeichnungsdatei – Identifizieren von Spuren](record-file-format.md#identifying-tracks).                     |
| `K4A_IMU_TRACK`              | Keine               | –                                  | Weitere Informationen finden Sie unter [Format der Aufzeichnungsdatei – Identifizieren von Spuren](record-file-format.md#identifying-tracks).                     |

## <a name="timestamp-synchronization"></a>Synchronisieren von Zeitstempeln

Beim Matroska-Format ist es erforderlich, dass Aufzeichnungen mit dem Zeitstempel 0 beginnen. Bei der [externen Synchronisierung von Kameras](record-external-synchronized-units.md) kann der Zeitstempel der einzelnen Geräte einen anderen Wert als 0 aufweisen.

Um die ursprünglichen Zeitstempel der Geräte zwischen Aufzeichnung und Wiedergabe beizubehalten, wird in der Datei ein Versatz zum Anwenden der Zeitstempel gespeichert.

Das Tag `K4A_START_OFFSET_NS` wird zum Angeben eines Zeitstempelversatzes verwendet, um Dateien nach der Aufzeichnung erneut synchronisieren zu können. Dieser Zeitstempelversatz kann zu jedem Zeitstempel innerhalb der Datei hinzugefügt werden, um die ursprünglichen Zeitstempel der Geräte wiederherzustellen.

Der Startversatz ist auch in der [`k4a_record_configuration_t`](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/structk4a__record__configuration__t.html)-Struktur verfügbar.
