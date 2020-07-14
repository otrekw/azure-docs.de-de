---
title: 'Azure Kinect: Indexzuordnung für Körper'
description: Hier finden Sie Informationen darüber, wie Sie eine Indexzuordnung für das Body Tracking in Azure Kinect DK abfragen.
author: qm13
ms.author: quentinm
ms.reviewer: yijwan
ms.prod: kinect-dk
ms.date: 06/26/2019
ms.topic: conceptual
keywords: Kinect, portieren, Körper, Body Tracking, Index, Segmentierung, Zuordnung
ms.openlocfilehash: 8c128e59cef515aafb4f59794f0f644b90fd625d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85276752"
---
# <a name="azure-kinect-body-tracking-index-map"></a>Azure Kinect Body Tracking: Indexzuordnung

Die Indexzuordnung für Körper umfasst die Zuordnung der Instanzsegmentierung für jeden Körper in der Erfassung der Tiefenkamera. Jedes Pixel ist dem entsprechenden Pixel im Tiefen- oder IR-Bild zugeordnet. Der Wert für jedes Pixel repräsentiert den Körper, zu dem das Pixel gehört. Es kann sich entweder um einen Hintergrund (Wert `K4ABT_BODY_INDEX_MAP_BACKGROUND`) oder den Index eines erkannten `k4abt_body_t` handeln.

![Indexzuordnung für Körper: Beispiel](./media/concepts/body-index-map.png)

>[!NOTE]
> Der Körperindex unterscheidet sich von der Körper-ID. Sie können die Körper-ID eines bestimmten Körperindex durch Aufruf von [k4abt_frame_get_body_id()](https://microsoft.github.io/Azure-Kinect-Body-Tracking/release/1.x.x/group__btfunctions_ga1d612404d133a279af847974e9359a92.html#ga1d612404d133a279af847974e9359a92) in der API abfragen.


## <a name="using-body-index-map"></a>Verwenden der Indexzuordnung für Körper

Die Indexzuordnung für Körper wird als `k4a_image_t` gespeichert und weist dieselbe Auflösung auf wie ein Tiefen- oder IR-Bild. Jedes Pixel ist ein 8 Bit langer Wert. Es kann durch Aufruf von `k4abt_frame_get_body_index_map` aus einem `k4abt_frame_t`-Element abgefragt werden. Der Entwickler muss den Arbeitsspeicher für die Körperindexzuordnung durch Aufruf von `k4a_image_release()` freigeben.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen Ihrer ersten Body Tracking-App](build-first-body-app.md)
