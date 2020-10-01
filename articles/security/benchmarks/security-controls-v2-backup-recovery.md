---
title: 'Azure-Sicherheitsvergleichstest V2: Sicherung und Wiederherstellung'
description: 'Azure-Sicherheitsvergleichstest V2: Sicherung und Wiederherstellung'
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: fe6861a3319b9d9c0e6535ee3303c90f0a0f26c8
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059038"
---
# <a name="security-control-backup-and-recovery"></a>Sicherheitskontrolle: Sicherung und Wiederherstellung

Sicherung und Wiederherstellung umfasst Kontrollelemente, die sicherstellen, dass Daten- und Konfigurationssicherungen auf den verschiedenen Dienstebenen durchgeführt, validiert und geschützt werden.

## <a name="br-1-ensure-regular-automated-backups"></a>BR-1: Sicherstellen regelmäßiger automatisierter Sicherungen

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| BR-1 | 10.1 | CP-2, CP4, CP-6, CP-9 |

Stellen Sie sicher, dass Systeme und Daten gesichert werden, um die Geschäftskontinuität nach einem unerwarteten Ereignis aufrechtzuerhalten. Dies sollte sich an den Zielen für das Recovery Point Objective (RPO) und das Recovery Time Objective (RTO) orientieren.

Aktivieren Sie Azure Backup, und konfigurieren Sie die Sicherungsquelle (z. B. virtuelle Azure-Computer, SQL Server, HANA-Datenbanken oder Dateifreigaben) sowie die gewünschte Häufigkeit und Beibehaltungsdauer.  

Für ein höheres Maß an Redundanz können Sie die Option für georedundante Speicherung aktivieren, um Sicherungsdaten in eine sekundäre Region zu replizieren und mittels regionsübergreifender Wiederherstellung wiederherzustellen.

- [Aktivieren von Azure Backup](/azure/backup/)

- [Wie kann ich die regionsübergreifende Wiederherstellung aktivieren?](/azure/backup/backup-azure-arm-restore-vms#cross-region-restore)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Richtlinien und Standards](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Sicherheitsarchitektur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-2-encrypt-backup-data"></a>BR-2: Verschlüsseln von Sicherungsdaten

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| BR-2 | 10.2 | CP-9 |

Stellen Sie sicher, dass Ihre Sicherungen vor Angriffen geschützt sind. Dazu gehört auch die Verschlüsselung der Sicherungen zum Schutz vor dem Verlust der Vertraulichkeit.   

Für die lokale Sicherung mit Azure Backup erfolgt eine Verschlüsselung im Ruhezustand über die bereitgestellte Passphrase. Für regelmäßige Azure-Dienstsicherungen werden die Sicherungsdaten automatisch mit von der Azure-Plattform verwalteten Schlüsseln verschlüsselt. Sie können sich dafür entscheiden, die Sicherung mit einem vom Kunden verwalteten Schlüssel zu verschlüsseln. Stellen Sie in diesem Fall sicher, dass der vom Kunden verwaltete Schlüssel im Schlüsseltresor ebenfalls Teil der Sicherung ist. 

Verwenden Sie rollenbasierte Zugriffssteuerung in Azure Backup, Azure Key Vault oder anderen Ressourcen, um Sicherungen und vom Kunden verwaltete Schlüssel zu schützen. Zusätzlich können Sie erweiterte Sicherheitsfeatures aktivieren, die MFA erfordern, bevor Sicherungen geändert oder gelöscht werden können.

- [Übersicht über Sicherheitsfeatures in Azure Backup](/azure/backup/security-overview)

- [Verschlüsselung von Sicherungsdaten mit von Kunden verwalteten Schlüsseln](/azure/backup/encryption-at-rest-with-cmk) 

- [Sichern von Schlüsseltresorschlüsseln in Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Sicherheitsarchitektur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Vorbereitung auf Vorfälle](/) azure/cloud-adoption-framework/organize/cloud-security-incident-preparation

## <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| BR-3 | 10.3 | CP-4, CP-9 |

Führen Sie regelmäßige Datenwiederherstellungen Ihrer Sicherung aus. Stellen Sie sicher, dass gesicherte kundenseitig verwaltete Schlüssel wiederhergestellt werden können.

- [Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern](/azure/backup/backup-azure-restore-files-from-vm)

- [Wiederherstellen von Key Vault-Schlüsseln in Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Minimieren der Auswirkungen verlorener Schlüssel

| Azure-ID | CIS Controls v7.1 ID(s) | NIST SP800-53 r4 ID(s) |
|--|--|--|--|
| BR-4 | 10,4 | CP-9 |

Stellen Sie sicher, dass Sie über Maßnahmen verfügen, um den Verlust von Schlüsseln zu verhindern bzw. sich von den daraus resultierenden Folgen zu erholen. Aktivieren Sie vorläufiges Löschen und den Löschschutz in Azure Key Vault, um Schlüssel vor versehentlichem oder böswilligem Löschen zu schützen.  

- [Aktivieren des vorläufigen Löschens und des Löschschutzes in Key Vault](/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Verantwortlichkeit**: Kunde

**Sicherheitsbeteiligte der Kunden**:

- [Sicherheitsarchitektur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Datensicherheit](/azure/cloud-adoption-framework/organize/cloud-security-data-security)

