---
title: Erstellen und Verwalten von Azure-Diensten mithilfe der Azure CLI
description: Verwenden der Azure-Befehlszeilenschnittstelle zum Erstellen und Verwalten von Azure-Diensten für Azure Backup.
ms.topic: conceptual
ms.date: 05/21/2021
ms.openlocfilehash: 3581be20082d1c579caad4cf82ce6b2408f01a92
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482655"
---
# <a name="create-and-manage-azure-backup-services-using-azure-command-line-interface-azure-cli"></a>Erstellen und Verwalten Azure Backup-Diensten mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI)

Die Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI) setzt sich aus Befehlen zum Erstellen und Verwalten von Azure-Ressourcen zusammen. Die Azure-Befehlszeilenschnittstelle unterstützt Sie bei der Automatisierung und bei der Arbeit mit Azure Backup.

Dieser Artikel enthält Informationen zu den Diensten, die die Azure-Befehlszeilenschnittstelle für Azure Backup unterstützen, sowie die entsprechenden Links zu den Artikeln.

## <a name="document-references-for-cli-supported-azure-services"></a>Dokumentreferenzen zu von der CLI unterstützten Azure-Diensten

In der folgenden Tabelle sind die Azure CLI-Dokumentreferenzen aufgeführt, die für unterstützte Azure-Dienste verfügbar sind.

Azure-Dienste | CLI-Dokumentverweise
-------------------------- | ---------------------------------
Azure-Tresor | [Löschen eines Recovery Services-Tresors](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-using-cli)
Virtueller Azure-Computer (VM) | <li>[Sichern eines virtuellen Azure-Computers](quick-backup-vm-cli.md)</li><li>[Wiederherstellen einer Azure-VM](tutorial-restore-disk.md)</li><li>[Wiederherstellen von Dateien aus Azure-VM-Sicherungen](tutorial-restore-files.md)</li><li>[Aktualisieren der vorhandenen VM-Sicherungsrichtlinie](modify-vm-policy-cli.md)</li><li>[Sichern und Wiederherstellen selektiver Datenträger für virtuelle Azure-Computer](selective-disk-backup-restore.md#using-azure-cli)</li>
Azure-Dateifreigabe | <li>[Sichern von Azure-Dateifreigaben](backup-afs-cli.md)</li><li>[Wiederherstellen von Azure-Dateifreigaben](restore-afs-cli.md)</li><li>[Verwalten der Sicherungen von Azure-Dateifreigaben](manage-afs-backup-cli.md)</li>
SAP HANA | <li>[Sichern von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer](tutorial-sap-hana-backup-cli.md)</li><li>[Wiederherstellen von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer](tutorial-sap-hana-restore-cli.md)</li><li>[Verwalten von SAP HANA-Datenbanken auf einem virtuellen Azure-Computer](tutorial-sap-hana-manage-cli.md)</li>

