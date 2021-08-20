---
title: Datei einfügen
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/24/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 759ce6ab44e7b50b88abc487cf2ab90041a06259
ms.sourcegitcommit: cd7d099f4a8eedb8d8d2a8cae081b3abd968b827
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2021
ms.locfileid: "112975237"
---
| SSD Premium-Größen | P1 | P2 | P3 | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60 | P70 | P80 |
|-------------------|----|----|----|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Datenträgergröße in GiB | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1\.024 | 2\.048 | 4\.096 | 8\.192 | 16.384 | 32.767 |
| Bereitgestellte IOPS pro Datenträger | 120 | 120 | 120 | 120 | 240 | 500 | 1\.100 | 2\.300 | 5\.000 | 7\.500 | 7\.500 | 16.000 | 18.000 | 20.000 |
| Bereitgestellter Durchsatz pro Datenträger | 25 MB/s | 25 MB/s | 25 MB/s | 25 MB/s | 50 MB/s | 100 MB/s | 125 MB/s | 150 MB/s | 200 MB/s | 250MB/s | 250 MB/s| 500 MB/s | 750 MB/s | 900 MB/s |
| Max. Burst-IOPS pro Datenträger | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 30.000* | 30.000* | 30.000* | 30.000* | 30.000* | 30.000* |
| Max. Burstdurchsatz pro Datenträger | 170 MB/s | 170 MB/s | 170 MB/s | 170 MB/s | 170 MB/s | 170 MB/s | 170 MB/s | 170 MB/s | 1\.000 MB/s* | 1\.000 MB/s* | 1\.000 MB/s* | 1\.000 MB/s* | 1\.000 MB/s* | 1\.000 MB/s* |
| Max. Burstdauer | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | Unbegrenzt* | Unbegrenzt* | Unbegrenzt* | Unbegrenzt* | Unbegrenzt* | Unbegrenzt* |
| Qualifiziert für Reservierung | Nein  | Nein  | Nein  | Nein  | Nein  | Nein  | Nein  | Nein  | Ja, bis zu einem Jahr | Ja, bis zu einem Jahr | Ja, bis zu einem Jahr | Ja, bis zu einem Jahr | Ja, bis zu einem Jahr | Ja, bis zu einem Jahr |

\* Gilt nur für Datenträger mit aktiviertem On-Demand-Bursting.