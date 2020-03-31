---
title: 'Azure-Sicherheitskontrolle: Datenwiederherstellung'
description: 'Sicherheitskontrolle: Datenwiederherstellung'
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: c585ebd903d4070f6247456e06efffbc6ec45270
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934492"
---
# <a name="security-control-data-recovery"></a>Sicherheitskontrolle: Datenwiederherstellung

Sorgen Sie dafür, dass alle Systemdaten, Konfigurationen und Geheimnisse in regelmäßigen Abständen automatisch gesichert werden.

## <a name="91-ensure-regular-automated-back-ups"></a>9.1: Sicherstellen regelmäßiger automatisierter Sicherungen

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 9.1 | 10.1 | Kunde |

Aktivieren Sie Azure Backup, und konfigurieren Sie die Sicherungsquelle (virtuelle Azure-Computer, SQL Server oder Dateifreigaben) sowie die gewünschte Häufigkeit und Beibehaltungsdauer.

Aktivieren von Azure Backup:

https://docs.microsoft.com/azure/backup/

## <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Durchführen vollständiger Systemsicherungen und Sichern aller von Kunden verwalteten Schlüssel

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 9.2 | 10.2 | Kunde |

Aktivieren Sie Azure Backup und die virtuellen Zielcomputer sowie die gewünschte Häufigkeit und Beibehaltungsdauer. Sichern Sie die von Kunden verwalteten Schlüssel in Azure Key Vault.

Aktivieren von Azure Backup:

https://docs.microsoft.com/azure/backup/

Sichern von Key Vault-Schlüsseln in Azure:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Überprüfen aller Sicherungen einschließlich der von Kunden verwalteten Schlüssel

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 9.3 | 10.3 | Kunde |

Stellen Sie regelmäßig sicher, dass die Inhalte in Azure Backup wiederhergestellt werden können. Testen Sie die Wiederherstellung gegebenenfalls in einem isolierten VLAN. Testen Sie die Wiederherstellung von gesicherten von Kunden verwalteten Schlüsseln.

Wiederherstellen von Dateien aus einer Sicherung auf virtuellen Azure-Computern:

https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm

Wiederherstellen von Key Vault-Schlüsseln in Azure:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

## <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Sicherstellen des Schutzes von Sicherungen und von kundenseitig verwalteten Schlüsseln

| Azure-ID | CIS-IDs | Verantwortlichkeit |
|--|--|--|
| 9,4 | 10,4 | Kunde |

Für die lokale Sicherung erfolgt eine Verschlüsselung im Ruhezustand über die beim Sichern in Azure bereitgestellte Passphrase. Für virtuelle Azure-Computer werden die Daten mit der Speicherdienstverschlüsselung (Storage Service Encryption, SSE) im Ruhezustand verschlüsselt. Sie können vorläufiges Löschen in Key Vault aktivieren, um Schlüssel vor dem versehentlichen oder böswilligen Löschen zu schützen.

Aktivieren von vorläufigem Löschen in Key Vault:

https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der nächsten Sicherheitskontrolle: [Reaktion auf Vorfälle](security-control-incident-response.md)
