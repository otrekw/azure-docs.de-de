---
title: Serverseitige Verschlüsselung von Azure Managed Disks – PowerShell
description: Azure Storage schützt Ihre Daten, indem der Dienst diese im Ruhezustand verschlüsselt, bevor diese auf Storage-Clustern gespeichert werden. Sie können von Microsoft verwaltete Schlüssel für die Verschlüsselung Ihrer verwalteten Datenträger nutzen, oder Sie können mit vom Kunden verwalteten Schlüsseln die Verschlüsselung mit Ihren eigenen Schlüsseln verwalten.
author: roygara
ms.date: 07/10/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 0bb0fb268d18ddc152dae45014e2154686762976
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259807"
---
# <a name="server-side-encryption-of-azure-disk-storage"></a>Serverseitige Verschlüsselung von Azure Disk Storage

Die serverseitige Verschlüsselung (Server-side Encryption, SSE) schützt Ihre Daten und unterstützt Sie beim Einhalten der Sicherheits- und Complianceanforderungen Ihrer Organisation. Durch SSE werden Ihre auf verwalteten Azure-Datenträgern (Betriebssystemdatenträger und reguläre Datenträger) gespeicherten ruhenden Daten standardmäßig automatisch verschlüsselt, wenn sie in der Cloud gespeichert werden. 

Daten in verwalteten Azure-Datenträgern werden transparent mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) – einer der stärksten verfügbaren Blockverschlüsselungen – FIPS 140-2-konform ver- und entschlüsselt. Weitere Informationen zu den kryptografischen Modulen, die verwalteten Azure-Datenträgern zugrunde liegen, finden Sie unter [Kryptografie-API: Die nächste Generation](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

Die serverseitige Verschlüsselung wirkt sich nicht auf die Leistung verwalteter Datenträger aus, und es fallen keine zusätzlichen Kosten an. 

> [!NOTE]
> Temporäre Datenträger sind keine verwalteten Datenträger und werden nicht von SSE verschlüsselt, es sei denn, Sie aktivieren die Verschlüsselung auf dem Host.

## <a name="about-encryption-key-management"></a>Informationen zur Verwaltung von Verschlüsselungsschlüsseln

Sie können von der Plattform verwaltete Schlüssel für die Verschlüsselung der verwalteten Datenträger verwenden oder die Verschlüsselung mit eigenen Schlüsseln verwalten. Wenn Sie die Verschlüsselung mit eigenen Schlüsseln verwalten möchten, können Sie einen *vom Kunden verwalteten Schlüssel* angeben, der zum Verschlüsseln und Entschlüsseln aller Daten in verwalteten Datenträgern verwendet werden soll. 

In den folgenden Abschnitten werden die einzelnen Optionen für die Schlüsselverwaltung ausführlicher beschrieben.

### <a name="platform-managed-keys"></a>Von der Plattform verwaltete Schlüssel

Verwaltete Datenträger verwenden standardmäßig von der Plattform verwaltete Verschlüsselungsschlüssel. Seit 10. Juni 2017 werden alle neuen verwalteten Datenträger, Momentaufnahmen, Images und neuen Daten, die auf vorhandene verwaltete Datenträger geschrieben wurden, im Ruhezustand automatisch mit von der Plattform verwalteten Schlüsseln verschlüsselt.

### <a name="customer-managed-keys"></a>Vom Kunden verwaltete Schlüssel

[!INCLUDE [virtual-machines-managed-disks-description-customer-managed-keys](../../../includes/virtual-machines-managed-disks-description-customer-managed-keys.md)]

#### <a name="restrictions"></a>Beschränkungen

Vorerst gelten für vom Kunden verwaltete Schlüssel die folgenden Einschränkungen:

- Wenn dieses Feature für Ihren Datenträger aktiviert ist, können Sie es nicht deaktivieren.
    Bei Bedarf müssen Sie [alle Daten auf einen anderen verwalteten Datenträger ohne kundenseitig verwaltete Schlüssel kopieren](disks-upload-vhd-to-managed-disk-powershell.md#copy-a-managed-disk).
[!INCLUDE [virtual-machines-managed-disks-customer-managed-keys-restrictions](../../../includes/virtual-machines-managed-disks-customer-managed-keys-restrictions.md)]

## <a name="encryption-at-host---end-to-end-encryption-for-your-vm-data"></a>Verschlüsselung auf dem Host: End-to-End-Verschlüsselung für Ihre VM-Daten

Die End-to-End-Verschlüsselung beginnt mit dem VM-Host, dem Azure-Server, dem Ihre VM zugeordnet ist. Daten auf Ihren temporären Datenträgern sowie für den Cache von Betriebssystemdatenträgern/regulären Datenträgern werden auf diesem VM-Host gespeichert. Wenn Sie die End-to-End-Verschlüsselung aktivieren, werden alle diese Daten sowohl im Ruhezustand verschlüsselt als auch bei der Übertragung an den Speicherdienst, wo sie gespeichert werden. Bei der End-to-End-Verschlüsselung wird weder die CPU Ihres virtuellen Computers beansprucht noch die Leistung Ihres virtuellen Computers beeinträchtigt. 

Temporäre Datenträger werden im Ruhezustand mit plattformseitig verwalteten Schlüsseln verschlüsselt, wenn Sie die End-to-End-Verschlüsselung aktivieren. Der Cache von Betriebssystemdatenträgern und regulären Datenträgern wird im Ruhezustand entweder mit kundenseitig verwalteten Schlüsseln oder mit plattformseitig verwalteten Schlüsseln verschlüsselt. Dies hängt vom Verschlüsselungstyp ab. Wenn ein Datenträger beispielsweise mit kundenseitig verwalteten Schlüsseln verschlüsselt ist, wird der Cache für den Datenträger ebenfalls mit kundenseitig verwalteten Schlüsseln verschlüsselt. Ist ein Datenträger dagegen mit plattformseitig verwalteten Schlüsseln verschlüsselt, wird auch der Cache für den Datenträger mit plattformseitig verwalteten Schlüsseln verschlüsselt.

### <a name="restrictions"></a>Beschränkungen

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

#### <a name="supported-regions"></a>Unterstützte Regionen

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

#### <a name="supported-vm-sizes"></a>Unterstützte VM-Größen

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="double-encryption-at-rest"></a>Doppelte Verschlüsselung im Ruhezustand

Kunden mit besonders hohen Sicherheitsanforderungen, die befürchten, dass ein einzelner Verschlüsselungsalgorithmus, eine einzelne Verschlüsselungsimplementierung oder ein einzelner Verschlüsselungsschlüssel kompromittiert werden könnte, haben nun die Möglichkeit, eine zusätzliche Verschlüsselungsebene mit einem anderen Verschlüsselungsalgorithmus/-modus auf der Infrastrukturebene zu nutzen (unter Verwendung von plattformseitig verwalteten Schlüsseln). Diese neue Ebene kann auf Datenträger, Momentaufnahmen und Images angewendet werden, um sie im Ruhezustand mit doppelter Verschlüsselung zu verschlüsseln.

### <a name="supported-regions"></a>Unterstützte Regionen

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](../../../includes/virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Vergleich: Serverseitige Verschlüsselung und Azure-Datenträgerverschlüsselung

[Azure Disk Encryption](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) nutzt das [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)-Feature von Windows zum Verschlüsseln verwalteter Datenträger mit vom Kunden verwalteten Schlüsseln innerhalb der Gast-VM.  Die serverseitige Verschlüsselung mit vom Kunden verwalteten Schlüsseln bewirkt eine ADE-Verbesserung, indem Sie beliebige Betriebssystemtypen und Images für Ihre VMs verwenden können, indem Daten im Speicherdienst verschlüsselt werden.

> [!IMPORTANT]
> Von Kunden verwaltete Schlüssel basieren auf verwalteten Identitäten für Azure-Ressourcen, einem Feature von Azure Active Directory (Azure AD). Wenn Sie vom Kunden verwaltete Schlüssel konfigurieren, wird Ihren Ressourcen im Hintergrund automatisch eine verwaltete Identität zugewiesen. Wenn Sie anschließend das Abonnement, die Ressourcengruppe oder den verwalteten Datenträger von einem Azure AD-Verzeichnis in ein anderes Verzeichnis verschieben, wird die den verwalteten Datenträgern zugeordnete verwaltete Identität nicht an den neuen Mandanten übertragen, sodass vom Kunden verwaltete Schlüssel möglicherweise nicht mehr funktionieren. Weitere Informationen finden Sie unter [Übertragen eines Abonnements zwischen Azure AD-Verzeichnissen](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).


## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der End-to-End-Verschlüsselung mit Verschlüsselung auf dem Host – PowerShell](disks-enable-host-based-encryption-powershell.md)
- [Aktivieren der doppelten Verschlüsselung für ruhende Daten auf Ihren verwalteten Datenträgern – PowerShell](disks-enable-double-encryption-at-rest-powershell.md)
- [Aktivieren kundenseitig verwalteter Schlüssel auf Ihren verwalteten Datenträgern – PowerShell](disks-enable-customer-managed-keys-powershell.md)
- [Aktivieren kundenseitig verwalteter Schlüssel auf Ihren verwalteten Datenträgern – Portal](disks-enable-customer-managed-keys-portal.md)
- [Untersuchen der Azure Resource Manager-Vorlagen zum Erstellen verschlüsselter Datenträger mit vom Kunden verwalteten Schlüsseln](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md)
