---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4d2c0a02a48c0e04b501f136f66c28b3f532e2a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79486024"
---
Inkrementelle Momentaufnahmen sind Point-in-Time-Sicherungen für verwaltete Datenträger, die bei der Erfassung nur aus allen Änderungen seit der letzten Momentaufnahme bestehen. Wenn Sie versuchen, eine inkrementelle Momentaufnahme herunterzuladen oder anderweitig zu verwenden, wird die vollständige VHD verwendet. Durch diese neue Funktion für Momentaufnahmen verwalteter Datenträger werden diese potenziell kostengünstiger, da Sie nicht mehr mit jeder einzelnen Momentaufnahme den gesamten Datenträger speichern müssen, es sei denn, Sie entscheiden sich dazu. Ebenso wie reguläre Momentaufnahmen können inkrementelle Momentaufnahmen verwendet werden, um entweder einen vollständigen verwalteten Datenträger oder eine reguläre Momentaufnahme zu erstellen.

Es gibt einige Unterschiede zwischen einer inkrementellen Momentaufnahme und einer regulären Momentaufnahme. Inkrementelle Momentaufnahmen verwenden immer Festplattenlaufwerk-Standardspeicher, und zwar unabhängig vom Speichertyp des Datenträgers, wohingegen reguläre Momentaufnahmen SSD Premium verwenden können. Wenn Sie reguläre Momentaufnahmen auf Storage Premium zum Hochskalieren von VM-Bereitstellungen verwenden, empfiehlt es sich, benutzerdefinierte Images im Standardspeicher in [Shared Image Gallery](../articles/virtual-machines/linux/shared-image-galleries.md) zu verwenden. Dies unterstützt Sie dabei, eine umfangreiche Skalierung mit geringeren Kosten zu erzielen. Außerdem bieten inkrementelle Momentaufnahmen möglicherweise bessere Zuverlässigkeit durch [zonenredundanten Speicher](../articles/storage/common/storage-redundancy-zrs.md) (ZRS). Wenn ZRS in der ausgewählten Region verfügbar ist, verwendet eine inkrementelle Momentaufnahme automatisch ZRS. Wenn ZRS nicht in der Region verfügbar ist, wird für die Momentaufnahme standardmäßig [lokal redundanter Speicher](../articles/storage/common/storage-redundancy-lrs.md) (LRS) verwendet. Sie können dieses Verhalten außer Kraft setzen und manuell ein anderes Verhalten auswählen. Dies wird jedoch nicht empfohlen.

Inkrementelle Momentaufnahmen bieten außerdem eine differenzielle Funktion, die nur für verwaltete Datenträger verfügbar ist. Sie ermöglichen es Ihnen, die Änderungen zwischen zwei inkrementellen Momentaufnahmen der gleichen verwalteten Datenträger bis hinunter zur Blockebene abzurufen. Sie können diese Funktion verwenden, um den Datenbedarf beim regionsübergreifenden Kopieren von Momentaufnahmen zu verringern.  Beispielsweise können Sie die erste inkrementelle Momentaufnahme als Basisblob in einer anderen Region herunterladen. Für die nachfolgenden inkrementellen Momentaufnahmen können Sie nur die Änderungen seit der letzten Momentaufnahme in das Basisblob kopieren. Nachdem Sie die Änderungen kopiert haben, können Sie Momentaufnahmen für das Basisblob erstellen, die die Point-in-Time-Sicherung des Datenträgers in einer anderen Region darstellen. Sie können den Datenträger aus dem Basisblob oder aus einer Momentaufnahme für das Basisblob in einer anderen Region wiederherstellen.

:::image type="content" source="media/virtual-machines-disks-incremental-snapshots-description/incremental-snapshot-diagram.png" alt-text="Diagramm von in mehreren Regionen kopierte inkrementellen Momentaufnahmen. Momentaufnahmen führen verschiedene API-Aufrufe durch, bis sie schließlich bei jeder Momentaufnahme Seitenblobs bilden.":::