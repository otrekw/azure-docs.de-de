---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 8a8619da831dfa5b240bd93d3a046c49cc30affa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "76901413"
---
| Resource | Standarddateifreigaben | Premium-Dateifreigaben |
|----------|---------------|------------------------------------------|
| Mindestgröße einer Dateifreigabe | Keine Mindestgröße, nutzungsbasierte Bezahlung | 100 GiB; bereitgestellt |
| Maximale Größe einer Dateifreigabe | 100 TiB*, 5 TiB | ca. 100 TiB |
| Maximale Größe einer Datei in einer Dateifreigabe | 1 TiB | 1 TiB |
| Maximale Anzahl an Dateien in einer Dateifreigabe | Keine Begrenzung | Keine Begrenzung |
| Maximale Anzahl IOPS pro Freigabe | 10.000 IOPS*, 1.000 IOPS | 100.000 IOPS |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Dateifreigabe | 5 | 5 |
| Zieldurchsatz für eine einzelne Dateifreigabe | bis zu 300 MiB/s*, bis zu 60 MiB/s  | Siehe: Ein- und Ausgangswerte für Premium-Dateifreigabe|
| Maximaler Ausgang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 6.204 MiB/s |
| Maximaler Eingang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 4.136 MiB/s |
| Maximale Anzahl geöffneter Handles pro Datei | 2\.000 geöffnete Handles | 2\.000 geöffnete Handles |
| Maximale Anzahl von Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen |
| Maximale Objektnamenlänge (Verzeichnisse und Dateien) | 2\.048 Zeichen | 2\.048 Zeichen |
| Maximale Pfadnamenkomponente (im Pfad \A\B\C\D ist jeder Buchstabe eine Komponente) | 255 Zeichen | 255 Zeichen |

\* In den meisten Regionen verfügbar. Weitere Informationen zu den verfügbaren Regionen finden Sie unter [Regionale Verfügbarkeit](../articles/storage/files/storage-files-planning.md#regional-availability).
