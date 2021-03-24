---
title: Zugreifen auf Azure Kinect DK-Daten in einem Körperframe
description: Erfahren Sie mehr über die Daten in einem Körperframe und die Funktionen für den Zugriff auf diese Daten in Azure Kinect DK.
author: qm13
ms.author: quentinm
ms.reviewer: yijwan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: how-to
keywords: Körper, Frame, Azure, Kinect, Körper, Tracking, Tipps
ms.openlocfilehash: be44f59cb84e99129bf526575293eee69ca64598
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "85276760"
---
# <a name="access-data-in-body-frame"></a>Zugreifen auf Daten im Body-Frame

In diesem Artikel werden die in einem Körperframe enthaltenen Daten und die Funktionen für den Zugriff auf diese Daten behandelt.

Die folgenden Funktionen werden behandelt:

- [k4abt_frame_get_body_id()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92)
- [k4abt_frame_get_body_index_map()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga0e4f2d0d7e330d444de7070fb1fee4f6.html#ga0e4f2d0d7e330d444de7070fb1fee4f6)
- [k4abt_frame_get_body_skeleton()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gac7032ab06268253538556750775064fb.html#gac7032ab06268253538556750775064fb)
- [k4abt_frame_get_capture()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gad9eef11f6496bbfe997536c374217d9a.html#gad9eef11f6496bbfe997536c374217d9a)
- [k4abt_frame_get_num_bodies()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga29ab088b1a0d1a246bdb5542e21aa3c3.html#ga29ab088b1a0d1a246bdb5542e21aa3c3)
- [k4abt_frame_get_device_timestamp_usec()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga04be7b814b40296cd6b97044ed7283e4.html#ga04be7b814b40296cd6b97044ed7283e4)

## <a name="key-components-of-a-body-frame"></a>Hauptkomponenten eines Körperframes

Jeder Körperframe enthält eine Sammlung von Körperstrukturen, eine 2D-Körperindexkarte und die Eingabeaufzeichnung, die dieses Ergebnis generiert hat.

![Komponenten eines Körperframes](./media/how-to-guides/body-frame.png)

## <a name="access-the-collection-of-body-structs"></a>Zugreifen auf die Sammlung von Körperstrukturen

In einer einzelnen Aufzeichnung werden möglicherweise mehrere Körper erkannt. Sie können die Anzahl von Körpern abfragen, indem Sie die Funktion [k4abt_frame_get_num_bodies()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga29ab088b1a0d1a246bdb5542e21aa3c3.html#ga29ab088b1a0d1a246bdb5542e21aa3c3) aufrufen.

```C
size_t num_bodies = k4abt_frame_get_num_bodies(body_frame);
```

Sie verwenden die Funktionen [k4abt_frame_get_body_id()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92) und [k4abt_frame_get_body_skeleton()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gac7032ab06268253538556750775064fb.html#gac7032ab06268253538556750775064fb), um jeden Körperindex zu durchlaufen, um die jeweilige Körper-ID und Informationen zur Position/Ausrichtung der Gelenke zu erhalten.

```C
for (size_t i = 0; i < num_bodies; i++)
{
    k4abt_skeleton_t skeleton;
    k4abt_frame_get_body_skeleton(body_frame, i, &skeleton);
    uint32_t id = k4abt_frame_get_body_id(body_frame, i);
}
```

## <a name="access-the-body-index-map"></a>Zugreifen auf die Körperindexkarte

Sie verwenden die Funktion [k4abt_frame_get_body_index_map()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga0e4f2d0d7e330d444de7070fb1fee4f6.html#ga0e4f2d0d7e330d444de7070fb1fee4f6), um auf die Körperindexkarte zuzugreifen. Ausführliche Erläuterungen zur Körperindexkarte finden Sie unter [Azure Kinect-Körpertracking-Indexkarte](body-index-map.md). Stellen Sie sicher, dass Sie die Körperindexkarte freigegeben, wenn sie nicht mehr benötigt wird.

```C
k4a_image_t body_index_map = k4abt_frame_get_body_index_map(body_frame);
...  // Do your work with the body index map
k4a_image_release(body_index_map);
```

## <a name="access-the-input-capture"></a>Zugreifen auf die Eingabeaufzeichnung

Der Körper-Tracker ist eine asynchrone API. Die ursprüngliche Aufzeichnung wurde möglicherweise bereits freigegeben, wenn das Ergebnis per Pop entfernt wird. Verwenden Sie die Funktion [k4abt_frame_get_capture()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_gad9eef11f6496bbfe997536c374217d9a.html#gad9eef11f6496bbfe997536c374217d9a), um die Eingabeaufzeichnung abzufragen, mit der dieses Körpertrackingergebnis generiert wurde. Die Verweisanzahl für k4a_capture_t erhöht sich bei jedem Aufruf dieser Funktion. Verwenden Sie die Funktion [k4a_capture_release()](https://microsoft.github.io/Azure-Kinect-Sensor-SDK/master/group___functions_ga0ed6f74ec403c3eac1b8ef3afb32cee6.html#ga0ed6f74ec403c3eac1b8ef3afb32cee6), wenn die Aufzeichnung nicht mehr benötigt wird.

```C
k4a_capture_t input_capture = k4abt_frame_get_capture(body_frame);
... // Do your work with the input capture
k4a_capture_release(input_capture);
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
>[Azure Kinect-Körpertrackings-SDK](https://microsoft.github.io/Azure-Kinect-Body-Tracking/)
