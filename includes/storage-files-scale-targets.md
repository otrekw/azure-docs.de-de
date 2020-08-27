---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: rogarana
ms.openlocfilehash: 8034a7a520cc50b2e57e0ad26205da2e408296a1
ms.sourcegitcommit: afa1411c3fb2084cccc4262860aab4f0b5c994ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2020
ms.locfileid: "88760586"
---
| Resource | Standarddateifreigaben | Premium-Dateifreigaben |
|----------|---------------|------------------------------------------|
| Mindestgröße einer Dateifreigabe | Keine Mindestgröße, nutzungsbasierte Bezahlung | 100 GiB; bereitgestellt |
| Maximale Größe einer Dateifreigabe | 100 TiB*, 5 TiB | ca. 100 TiB |
| Maximale Größe einer Datei in einer Dateifreigabe | 1 TiB | 4 TiB |
| Maximale Anzahl an Dateien in einer Dateifreigabe | Keine Begrenzung | Keine Begrenzung |
| Maximale Anzahl IOPS pro Freigabe | 10.000 IOPS*, 1.000 IOPS oder 100 Anforderungen in 100 ms | 100.000 IOPS |
| Maximale Anzahl gespeicherter Zugriffsrichtlinien pro Dateifreigabe | 5 | 5 |
| Zieldurchsatz für eine einzelne Dateifreigabe | bis zu 300 MiB/s*, bis zu 60 MiB/s  | Siehe: Ein- und Ausgangswerte für Premium-Dateifreigabe|
| Maximaler Ausgang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 6.204 MiB/s |
| Maximaler Eingang für eine einzelne Dateifreigabe | Siehe: Zieldurchsatz für Standard-Dateifreigabe | Bis zu 4.136 MiB/s |
| Maximale Anzahl geöffneter Handles pro Datei oder Verzeichnis | 2\.000 geöffnete Handles | 2\.000 geöffnete Handles |
| Maximale Anzahl von Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen | 200 Freigabemomentaufnahmen |
| Maximale Objektnamenlänge (Verzeichnisse und Dateien) | 2\.048 Zeichen | 2\.048 Zeichen |
| Maximale Pfadnamenkomponente (im Pfad \A\B\C\D ist jeder Buchstabe eine Komponente) | 255 Zeichen | 255 Zeichen |

\* Der Standardwert für Standarddateifreigaben beträgt 5 TiB. Ausführliche Informationen zum Hochskalieren von Standarddateifreigaben auf 100 TiB finden Sie unter [Aktivieren und Erstellen großer Dateifreigaben](../articles/storage/files/storage-files-how-to-create-large-file-share.md).
