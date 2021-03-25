---
title: 'Azure-Sicherheitsvergleichstest V2: Sicherung und Wiederherstellung'
description: 'Azure-Sicherheitsvergleichstest V2: Sicherung und Wiederherstellung'
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 02/22/2021
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 39466ad621eff1a7d3490c936c90fbff6f63e0fc
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051547"
---
# <a name="security-control-v2-backup-and-recovery"></a>Sicherheitskontrolle V2: Sicherung und Wiederherstellung

Sicherung und Wiederherstellung umfasst Kontrollelemente, die sicherstellen, dass Daten- und Konfigurationssicherungen auf den verschiedenen Dienstebenen durchgeführt, validiert und geschützt werden.

Die entsprechende integrierte Azure Policy finden Sie unter [Details zur integrierten Initiative zur Einhaltung der gesetzlichen Bestimmungen gemäß Azure-Sicherheitsvergleichstest: Sicherung und Wiederherstellung](../../governance/policy/samples/azure-security-benchmark.md#backup-and-recovery).

## <a name="br-1-ensure-regular-automated-backups"></a>BR-1: Sicherstellen regelmäßiger automatisierter Sicherungen

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| BR-1 | 10.1 | CP-2, CP4, CP-6, CP-9 |

Stellen Sie sicher, dass Systeme und Daten gesichert werden, um die Geschäftskontinuität nach einem unerwarteten Ereignis aufrechtzuerhalten. Dies sollte durch Ziele für das Recovery Point Objective (RPO) und das Recovery Time Objective (RTO) definiert werden.

Aktivieren Sie Azure Backup, und konfigurieren Sie die Sicherungsquelle (z. B. virtuelle Azure-Computer, SQL Server, HANA-Datenbanken oder Dateifreigaben) sowie die gewünschte Häufigkeit und Beibehaltungsdauer.

Für ein höheres Maß an Schutz können Sie die Option für georedundante Speicherung aktivieren, um Sicherungsdaten in eine sekundäre Region zu replizieren und mittels regionsübergreifender Wiederherstellung wiederherzustellen.

- [Business Continuity &amp; Disaster Recovery auf Unternehmensebene](/azure/cloud-adoption-framework/ready/enterprise-scale/business-continuity-and-disaster-recovery)

- [Aktivieren von Azure Backup](../../backup/index.yml)

- [Wie kann ich die regionsübergreifende Wiederherstellung aktivieren?](../../backup/backup-azure-arm-restore-vms.md#cross-region-restore)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Richtlinien und Standards](/azure/cloud-adoption-framework/organize/cloud-security-policy-standards)

- [Sicherheitsarchitektur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-2-encrypt-backup-data"></a>BR-2: Verschlüsseln von Sicherungsdaten

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| BR-2 | 10.2 | CP-9 |

Stellen Sie sicher, dass Ihre Sicherungen vor Angriffen geschützt sind. Dazu gehört auch die Verschlüsselung der Sicherungen zum Schutz vor dem Verlust der Vertraulichkeit.

Für lokale Sicherungen mit Azure Backup erfolgt eine Verschlüsselung im Ruhezustand über die bereitgestellte Passphrase. Für regelmäßige Azure-Dienstsicherungen werden die Sicherungsdaten automatisch mit von der Azure-Plattform verwalteten Schlüsseln verschlüsselt. Sie können sich dafür entscheiden, die Sicherungen mit einem vom Kunden verwalteten Schlüssel zu verschlüsseln. Stellen Sie in diesem Fall sicher, dass der vom Kunden verwaltete Schlüssel im Schlüsseltresor ebenfalls Teil der Sicherung ist.

Verwenden Sie rollenbasierte Zugriffssteuerung von Azure in Azure Backup, Azure Key Vault oder anderen Ressourcen, um Sicherungen und kundenseitig verwaltete Schlüssel zu schützen. Zusätzlich können Sie erweiterte Sicherheitsfeatures aktivieren, die MFA erfordern, bevor Sicherungen geändert oder gelöscht werden können.

- [Übersicht über Sicherheitsfeatures in Azure Backup](../../backup/security-overview.md)

- [Verschlüsselung von Sicherungsdaten mit von Kunden verwalteten Schlüsseln](../../backup/encryption-at-rest-with-cmk.md) 

- [Sichern von Schlüsseltresorschlüsseln in Azure](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [Sicherheitsfeatures für den Schutz von Hybridsicherungen vor Angriffen](../../backup/backup-azure-security-feature.md#prevent-attacks)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Sicherheitsarchitektur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Infrastruktur- und Endpunktsicherheit](/azure/cloud-adoption-framework/organize/cloud-security-infrastructure-endpoint)

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

## <a name="br-3-validate-all-backups-including-customer-managed-keys"></a>BR-3: Überprüfen aller Sicherungen einschließlich kundenseitig verwalteter Schlüssel

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| BR-3 | 10.3 | CP-4, CP-9 |

Führen Sie regelmäßige Datenwiederherstellungen Ihrer Sicherung aus. Stellen Sie sicher, dass gesicherte kundenseitig verwaltete Schlüssel wiederhergestellt werden können.

- [Wiederherstellen von Dateien aus einer Sicherung von virtuellen Azure-Computern](../../backup/backup-azure-restore-files-from-vm.md)

- [Wiederherstellen von Key Vault-Schlüsseln in Azure](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Sicherheitscomplianceverwaltung](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="br-4-mitigate-risk-of-lost-keys"></a>BR-4: Minimieren der Auswirkungen verlorener Schlüssel

| Azure-ID | CIS Controls v7.1 ID(s) | ID(s) von NIST SP 800-53 r4 |
|--|--|--|--|
| BR-4 | 10,4 | CP-9 |

Stellen Sie sicher, dass Sie über Maßnahmen verfügen, um den Verlust von Schlüsseln zu verhindern bzw. sich von den daraus resultierenden Folgen zu erholen. Aktivieren Sie vorläufiges Löschen und den Löschschutz in Azure Key Vault, um Schlüssel vor versehentlichem oder böswilligem Löschen zu schützen.

- [Aktivieren des vorläufigen Löschens und des Löschschutzes in Key Vault](../../storage/blobs/soft-delete-blob-overview.md?tabs=azure-portal)

**Verantwortlichkeit**: Kunde

**Sicherheitsverantwortliche beim Kunden** ([weitere Informationen](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Sicherheitsarchitektur](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Vorbereitung auf Vorfälle](/azure/cloud-adoption-framework/organize/cloud-security-incident-preparation)

- [Datensicherheit](/azure/cloud-adoption-framework/organize/cloud-security-data-security)