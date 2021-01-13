---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: 9805f53d5901226fc9e32b24a323256cd1da6844
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88655057"
---
Hier sind die Größen der Azure-Objekte, die geschrieben werden können. Stellen Sie sicher, dass alle Dateien, die hochgeladen werden, diesen Einschränkungen entsprechen.

| Azure-Objekttyp | Standardlimit                                             |
|-------------------|-----------------------------------------------------------|
| Blockblob        | Ca. 4,75 TiB                                                 |
| Seitenblob         | 8 TiB <br> Jede Datei, die im Seitenblobformat hochgeladen wird, muss einem ganzzahligen Vielfachen von 512 Bytes entsprechen. Andernfalls tritt beim Hochladen ein Fehler auf. <br> VHD- und VHDX-Dateien entsprechen einem ganzzahligen Vielfachen von 512 Bytes. |
| Azure Files        | 1 TiB                                                      |
| Verwaltete Datenträger     | 4 TiB <br> Weitere Informationen zu den Größen und Grenzwerten finden Sie unter: <li>[Skalierbarkeitsziele bei SSD Standard](../articles/virtual-machines/disks-types.md#standard-ssd)</li><li>[Skalierbarkeitsziele bei SSD Premium](../articles/virtual-machines/disks-types.md#standard-hdd)</li><li>[Skalierbarkeitsziele bei HDD Standard](../articles/virtual-machines/disks-types.md#premium-ssd)</li><li>[Managed Disks – Preise und Abrechnung](../articles/virtual-machines/disks-types.md#billing)</li>  
