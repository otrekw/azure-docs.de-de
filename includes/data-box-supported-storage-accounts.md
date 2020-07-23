---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 06/08/2020
ms.author: alkohli
ms.openlocfilehash: da36e2bbf358a1c61d2b9b3f7ede592ac88fd427
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2020
ms.locfileid: "86200284"
---
Im Folgenden finden Sie eine Liste der unterstützten Speicherkonten und Speichertypen für das Data Box-Gerät. Eine vollständige Liste aller verschiedenen Typen von Speicherkonten und ihren vollständigen Funktionen finden Sie unter [Speicherkontotypen](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

Die folgende Tabelle zeigt die unterstützten Speicherkonten für Importaufträge.

| **Speicherkonto/unterstützte Speichertypen** | **Blockblob** |**Seitenblob*** |**Azure Files** |**Hinweise**|
| --- | --- | -- | -- | -- |
| Klassisch Standard | J | J | J |
| Universell V1 Standard  | J | J | J | Es werden sowohl „heiße“ als auch „kalte“ Blobs unterstützt.|
| Universell V1 Premium  |  | J| | |
| Universell V2 Standard  | J | J | J | Es werden sowohl „heiße“ als auch „kalte“ Blobs unterstützt.|
| Universell V2 Premium  |  |J | | |
| Blobspeicher Standard |J | | |Es werden sowohl „heiße“ als auch „kalte“ Blobs unterstützt. |

\* *: In Seitenblobs hochgeladene Daten müssen einem ganzzahligen Vielfachen von 512 Byte entsprechen, z. B. VHDs.*

Die folgende Tabelle zeigt die unterstützten Speicherkonten für Exportaufträge.

| **Speicherkonto/unterstützte Speichertypen** | **Blockblob** |**Seitenblob*** |**Azure Files** |**Unterstützte Zugriffsebenen**|
| --- | --- | -- | -- | -- |
| Klassisch Standard | J | J | J | |
| Universell V1 Standard  | J | J | J | Heiße Ebene, kalte Ebene|
| Universell V1 Premium  |  | J| | |
| Universell V2 Standard  | J | J | J | Heiße Ebene, kalte Ebene|
| Universell V2 Premium  |  |J | | |
| Blobspeicher Standard |J | | |Heiße Ebene, kalte Ebene |
| Block Blob Storage Premium |J | | |Heiße Ebene, kalte Ebene |
| Page Blob Storage Premium | |J | | |

> [!IMPORTANT]
> - Data Box unterstützt Warteschlangen, Tabellen, Datenträger und Azure Data Lake Gen 2 nicht als Speichertypen für universelle Konten.
> - Data Box unterstützt keine Anfügeblobs für Blob Storage- und Block Blob Storage-Konten.
> - Data Box unterstützt keine File Storage Premium-Konten.
> - In Seitenblobs hochgeladene Daten müssen einem ganzzahligen Vielfachen von 512 Byte entsprechen, z. B. VHDs.
> - Es können maximal 80 TB exportiert werden.
> - Dateiverlauf und Blobmomentaufnahmen werden nicht exportiert.


