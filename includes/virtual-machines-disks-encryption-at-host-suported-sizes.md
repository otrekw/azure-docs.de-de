---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e5a811620de8336abd3e0df6d72db761ce18b2b6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86230985"
---
Alle VM-Größen der neuesten Generation unterstützen die Verschlüsselung auf dem Host:

|type  |Nicht unterstützt  |Unterstützt  |
|---------|---------|---------|
|Allgemeiner Zweck     | Dv3, Dav4, Dv2, Av2        | B, DSv2, Dsv3, DC, DCv2, Dasv4        |
|Computeoptimiert     |         | Fsv2        |
|Arbeitsspeicheroptimiert     | Ev3, Eav4        | DSv2, Esv3, M, Mv2, Easv4        |
|Speicheroptimiert     |         | Ls, Lsv2 (NVMe-Datenträger werden nicht verschlüsselt)        |
|GPU     | NC, NV        | NCv2, NCv3, ND, NVv3, NVv4, NDv2 (Vorschau)        |
|High Performance Computing     | H        | HB, HC, HBv2        |
|Vorherige Generationen     | F, A, D, L, G        | DS, GS, Fs, NVv2        |

Ein Upgrade der VM-Größe führt zu einem Validierungsprozess, um zu überprüfen, ob die neue VM-Größe das EncryptionAtHost-Feature unterstützt.
