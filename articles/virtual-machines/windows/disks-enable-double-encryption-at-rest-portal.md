---
title: Aktivieren der doppelten Verschlüsselung für ruhende Daten – Azure-Portal – verwaltete Datenträger
description: Hier erfahren Sie, wie Sie die doppelte Verschlüsselung für ruhende Daten auf Ihren verwalteten Datenträgern mithilfe des Azure-Portals aktivieren.
author: roygara
ms.date: 07/23/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: b244ef36e4e1a15327c053402b28ee2272fc71f0
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2020
ms.locfileid: "87135982"
---
# <a name="azure-portal---enable-double-encryption-at-rest-on-your-managed-disks"></a>Azure-Portal: Aktivieren der doppelten Verschlüsselung für ruhende Daten auf Ihren verwalteten Datenträgern

Azure Disk Storage unterstützt die doppelte Verschlüsselung von ruhenden Daten auf verwalteten Datenträgern. Informationen zum Konzept der doppelten Verschlüsselung ruhender Daten sowie weitere Typen der Verschlüsselung von Daten auf verwalteten Datenträgern finden Sie im Abschnitt [Doppelte Verschlüsselung ruhender Daten](disk-encryption.md#double-encryption-at-rest) des Artikels zur Datenträgerverschlüsselung.

[!INCLUDE [virtual-machines-double-encryption-at-rest-portal](../../../includes/virtual-machines-disks-double-encryption-at-rest-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Azure PowerShell: Aktivieren kundenseitig verwalteter Schlüssel mit serverseitiger Verschlüsselung – verwaltete Datenträger](disks-enable-customer-managed-keys-powershell.md)
- [Beispiele für Azure Resource Manager-Vorlagen](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)