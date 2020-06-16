---
title: Vorbereiten von Windows Server 2003-Server auf die Migration mit Azure Migrate
description: Hier erfahren Sie, wie Sie Windows Server 2003-Server auf die Migration mit Azure Migrate vorbereiten.
ms.topic: how-to
ms.date: 05/27/2020
ms.openlocfilehash: 33519764b138c7711e6c03a85aa33ec6f936a748
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172125"
---
# <a name="prepare-windows-server-2003-machines-for-migration"></a>Vorbereiten von Windows Server 2003-Computer auf die Migration

In diesem Artikel wird beschrieben, wie Sie Computer mit Windows Server 2003 auf die Migration zu Azure vorbereiten. 

- Sie können die Migration ohne Agent verwenden, um [Hyper-V-VMs](tutorial-migrate-hyper-v.md) und [VMware-VMs](tutorial-migrate-vmware.md) zu Azure zu migrieren.
- Zum Herstellen einer Verbindung mit Azure-VMs nach der Migration muss Hyper-V Integration Services auf der Azure-VM installiert sein. Dies ist auf Windows Server 2003-Computern standardmäßig nicht installiert.
- Es gibt keinen direkten Downloadlink zum Installieren von Hyper-V Integration Services. Daher müssen Sie wie folgt vorgehen:
    - Für Hyper-V-VMs, auf denen die Installation nicht vorhanden ist, extrahieren Sie die Installationsdateien für Integration Services auf einem Computer mit Windows Server 2012 R2/Windows Server 2012 mit der Hyper-V-Rolle. Kopieren Sie dann das Installationsprogramm auf den Windows Server 2003-Computer. Die Installationsdateien sind nicht auf Computern mit Windows Server 2016 verfügbar.
    - Für VMware-VMs erstellen Sie einen Starttask, der Integration Services beim Starten der Azure-VM nach der Migration installiert.


## <a name="install-on-hyper-v-vms"></a>Installation auf Hyper-V-VMs

Überprüfen Sie vor der Migration, ob Hyper-V Integration Services installiert ist, und führen Sie dann bei Bedarf die Installation durch.

1. Führen Sie [diese Anweisungen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#turn-an-integration-service-on-or-off-using-hyper-v-manager) aus, um zu überprüfen, ob die Installation durchgeführt wurde.
2. Wenn keine Installation vorliegt, melden Sie sich mit der Hyper-V-Rolle an einem Windows Server 2012 R2/Windows Server 2012-Computer an.
3. Navigieren Sie zur Installationsdatei unter **C:\Windows\System32\vmguest.iso**, und binden Sie die Datei ein.
2. Kopieren Sie den Installationsordner auf den Windows Server 2003-Computer, und installieren Sie Integration Services.
4. Nach der Installation können Sie die Standardeinstellungen von Integration Services beibehalten. 

## <a name="install-on-vmware-vms"></a>Installation auf VMware-VMs

1. Melden Sie sich mit der Hyper-V-Rolle an einem Windows Server 2012 R2/Windows Server 2012-Computer an.
2. Navigieren Sie zur Installationsdatei unter **C:\Windows\System32\vmguest.iso**, und binden Sie die Datei ein.
3. Kopieren Sie den Installationsordner auf die VMware-VM.
4. Führen Sie ```gpedit.msc``` in der Befehlszeile auf der VM aus.
5. Öffnen Sie **Computerkonfiguration** > **Windows-Einstellungen** > **Skripts (Start/Herunterfahren)** .
6. Geben Sie die Adresse für die Datei „setup.exe“ in **Start** > **Hinzufügen** > **Skriptname** ein.
7. Nach der Migration zu Azure wird das Skript ausgeführt, wenn die Azure-VM zum ersten Mal gestartet wird.
8. Starten Sie die Azure-VM manuell neu. In der Startdiagnose wird ein Popupelement angezeigt, das angibt, dass ein Neustart erforderlich ist.
9. Nachdem das Skript ausgeführt und Hyper-V Integration Services auf der Azure-VM installiert wurde, können Sie das Skript aus dem Start entfernen.
10. Nach der Installation können Sie die Standardeinstellungen von Integration Services beibehalten. 

## <a name="next-steps"></a>Nächste Schritte

- Überprüfen Sie die Migrationsanforderungen für [VMware](migrate-support-matrix-vmware-migration.md)- und [Hyper-V](migrate-support-matrix-hyper-v-migration.md)-VMs.
- Migrieren Sie [VMware](server-migrate-overview.md)- und [Hyper-V](tutorial-migrate-hyper-v.md)-VMs.
