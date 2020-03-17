---
title: Vorbereiten von Computern für die Migration mit Azure Migrate
description: Es wird beschrieben, wie Sie lokale Computer für die Migration mit Azure Migrate vorbereiten.
ms.topic: tutorial
ms.date: 02/17/2020
ms.custom: MVC
ms.openlocfilehash: eba177a254606bb847e0866ae48281a889c53f9b
ms.sourcegitcommit: 9cbd5b790299f080a64bab332bb031543c2de160
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2020
ms.locfileid: "78927467"
---
# <a name="prepare-on-premises-machines-for-migration-to-azure"></a>Vorbereiten von lokalen Computern für die Migration zu Azure

In diesem Artikel erfahren Sie, wie Sie lokale Computer vorbereiten, bevor Sie sie mithilfe von [Azure Migrate: Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool) zu Azure migrieren.

In diesem Artikel führen Sie folgende Schritte aus:
> [!div class="checklist"]
> * Überprüfen der Einschränkungen bei der Migration
> * Überprüfen der Betriebssystemanforderungen und Supporteinschränkungen
> * Überprüfen des URL- und Portzugriffs für zu migrierende Computer
> * Überprüfen der Änderungen, die Sie vor Beginn der Migration ggf. vornehmen müssen
> * Konfigurieren von Einstellungen, sodass Laufwerkbuchstaben nach der Migration erhalten bleiben
> * Vorbereiten von Computern, sodass Sie nach der Migration eine Verbindung mit den virtuellen Azure-Computern herstellen können

## <a name="verify-migration-limitations"></a>Überprüfen der Einschränkungen bei der Migration

- Mit der Azure Migrate-Servermigration können pro Azure Migrate-Projekt bis zu 35.000 virtuelle VMware-/Hyper-V-Computer bewertet werden. In einem Projekt können virtuelle VMware- und Hyper-V-Computer bis zum jeweiligen Limit miteinander kombiniert werden.
- Sie können bis zu zehn virtuelle Computer gleichzeitig für die Migration auswählen. Verwenden Sie jeweils Gruppen von zehn VMs, falls Sie eine höhere Zahl replizieren möchten.
- Bei der VMware-Migration ohne Agents können Sie bis zu 100 Replikationsvorgänge gleichzeitig ausführen.

## <a name="verify-operating-system-requirements"></a>Überprüfen der Betriebssystemanforderungen

- Überprüfen Sie, ob Ihre [Windows-Betriebssysteme](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) in Azure unterstützt werden.
- Überprüfen Sie, ob Ihre [Linux-Distributionen](../virtual-machines/linux/endorsed-distros.md) in Azure unterstützt werden.

## <a name="see-whats-supported"></a>Überprüfen der Unterstützung

Für VMware-VMs wird bei der Servermigration die [Migration mit oder ohne Agent](server-migrate-overview.md) unterstützt.

- **VMware-VMs:** Überprüfen Sie die [Migrationsanforderungen und -unterstützung](migrate-support-matrix-vmware-migration.md) für virtuelle VMware-Computer.
- **Virtuelle Hyper-V-Computer:** Überprüfen Sie die [Migrationsanforderungen und -unterstützung](migrate-support-matrix-hyper-v-migration.md) in Bezug auf Hyper-V-VMs.
- **Physische Computer:** Überprüfen Sie die [Migrationsanforderungen und -unterstützung](migrate-support-matrix-physical-migration.md) für lokale physische Computer und andere virtualisierte Server. 

## <a name="review-url-and-port-access"></a>Überprüfen des URL- und Portzugriffs

Computer benötigen während der Migration ggf. Internetzugriff.

- **Azure Migrate-Appliance**: Überprüfen Sie die [URLs](migrate-appliance.md#url-access) und [Ports](migrate-support-matrix-vmware-migration.md#agentless-ports), die von der Azure Migrate-Appliance für den Zugriff während der Migration ohne Agent benötigt werden.
- **Agent-basierte Migration virtueller VMware-Computer:** Überprüfen Sie die [URLs](migrate-replication-appliance.md#url-access) und [Ports](migrate-replication-appliance.md#port-access), die die Replikationsappliance bei der Agent-basierten Migration virtueller VMware-Computer verwendet. 
- **Hyper-V-Hosts:** Überprüfen Sie die [URLs und Ports](migrate-support-matrix-hyper-v-migration.md#hyper-v-hosts), auf die Hyper-V-Hosts bei der Migration Zugriff haben müssen. 
- **Physische Server**: Überprüfen Sie die [URLs](migrate-replication-appliance.md#url-access) und [Ports](migrate-replication-appliance.md#port-access), die die Replikationsappliance bei der Migration physischer Server verwendet.

## <a name="verify-required-changes-before-migrating"></a>Überprüfen der erforderlichen Änderungen vor der Migration

Bei manchen virtuellen Computern sind möglicherweise Änderungen erforderlich, damit sie in Azure ausgeführt werden können. Für virtuelle Computer unter den folgenden Betriebssystemen werden diese Änderungen automatisch von Azure Migrate vorgenommen:

- Red Hat Enterprise Linux 7.0+, 6.5+
- CentOS 7.0+, 6.5+
- SUSE Linux Enterprise Server 12 SP1+
- Ubuntu 18.04LTS, 16.04LTS, 14.04LTS
- Debian 8, 7

Bei anderen Betriebssystemen müssen die Computer vor der Migration manuell vorbereitet werden. 

### <a name="prepare-windows-machines"></a>Vorbereiten von Windows-Computern

Falls Sie einen Windows-Computer migrieren, nehmen Sie vor der Migration die folgenden Änderungen vor. Wenn Sie den virtuellen Computer migrieren, bevor Sie die Änderungen vorgenommen haben, wird der virtuelle Computer in Azure unter Umständen nicht gestartet.

1. [Aktivieren Sie die serielle Azure-Konsole](../virtual-machines/troubleshooting/serial-console-windows.md) für den virtuellen Azure-Computer. Die Aktivierung der Konsole vereinfacht die Problembehandlung. Sie müssen den virtuellen Computer nicht neu starten. Der virtuelle Azure-Computer wird unter Verwendung des Datenträgerimages gestartet. Der Start mit dem Datenträgerimage entspricht einem Neustart für den neuen virtuellen Computer. 
2. Bei der Migration von Computern unter Windows Server 2003 müssen die Dienste für die Hyper-V-Gastintegration unter dem Betriebssystem des virtuellen Computers installiert werden. [Weitere Informationen](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services)

### <a name="prepare-linux-machines"></a>Vorbereiten von Linux-Computern

1. Installieren Sie Linux Integration Services für Hyper-V. In den meisten neuen Versionen von Linux-Distributionen ist Linux Integration Services für Hyper-V bereits standardmäßig enthalten.
2. Erstellen Sie das Linux-Initialisierungsimage neu, damit es die erforderlichen Hyper-V-Treiber enthält. Durch die Neuerstellung des Initialisierungsimages wird sichergestellt, dass der virtuelle Computer in Azure gestartet wird. (Dies ist nur bei einigen Distributionen erforderlich.)
3. [Aktivieren Sie die Protokollierung der seriellen Azure-Konsole.](../virtual-machines/troubleshooting/serial-console-linux.md) Die Aktivierung der Konsolenprotokollierung vereinfacht die Problembehandlung. Sie müssen den virtuellen Computer nicht neu starten. Der virtuelle Azure-Computer wird unter Verwendung des Datenträgerimages gestartet. Der Start mit dem Datenträgerimage entspricht einem Neustart für den neuen virtuellen Computer.
4. Aktualisieren Sie die Gerätezuordnungsdatei mit den Zuordnungen von Gerätenamen und Volumes so, dass persistente Gerätebezeichner verwendet werden.
5. Aktualisieren Sie die Einträge in der FSTAB-Datei so, dass persistente Volumebezeichner verwendet werden.
6. Entfernen Sie alle udev-Regeln, die Schnittstellennamen basierend auf der MAC-Adresse usw. reservieren.
7. Aktualisieren Sie die Netzwerkschnittstellen so, dass IP-Adressen per DHCP empfangen werden.

Machen Sie sich ausführlicher mit den [Schritten zum Ausführen eines virtuellen Linux-Computers in Azure](../virtual-machines/linux/create-upload-generic.md) vertraut, und sehen Sie sich die Anleitungen für einige gängige Linux-Distributionen an.

## <a name="preserve-drive-letters-after-migration"></a>Beibehalten von Laufwerkbuchstaben nach der Migration

Wenn Sie einen lokalen Computer zu Microsoft Azure migrieren, ändern sich unter Umständen die Laufwerkbuchstaben von zusätzlichen Datenträgern. 

Standardmäßig wird virtuellen Azure-Computern das Laufwerk D als temporärer Speicher zugewiesen. Diese Laufwerkzuweisung führt dazu, dass alle anderen angefügten Zuweisungen von Speicherlaufwerken um einen Buchstaben inkrementiert werden. Wenn für Ihre lokale Installation beispielsweise ein Datenträger genutzt wird, dem der Laufwerkbuchstabe D für Anwendungsinstallationen zugewiesen ist, wird die Zuweisung für dieses Laufwerk auf Laufwerk E inkrementiert, nachdem Sie die VM zu Azure migriert haben. Legen Sie die SAN-Richtlinie (Storage Area Network) auf **OnlineAll** fest, um diese automatische Zuweisung zu verhindern und sicherzustellen, dass Azure dem temporären Volume den nächsten freien Laufwerkbuchstaben zuweist:

1. Öffnen Sie auf dem lokalen Computer (nicht auf dem Hostserver) eine Eingabeaufforderung mit erhöhten Rechten.
2. Geben Sie **diskpart** ein.
3. Geben Sie **SAN** ein. Wenn der Laufwerkbuchstabe des Gastbetriebssystems nicht beibehalten wird, wird **Offline – Alle** oder **Offline – Freigegeben** zurückgegeben.
4. Geben Sie an der Eingabeaufforderung **DISKPART** Folgendes ein: **SAN Policy=OnlineAll**. Durch diese Einstellung wird sichergestellt, dass Datenträger online geschaltet werden und dass Sie von beiden Datenträgern lesen sowie auf beide Datenträger schreiben können.
5. Während der Testmigration können Sie überprüfen, ob die Laufwerkbuchstaben beibehalten werden.

## <a name="check-azure-vm-requirements"></a>Überprüfen der Anforderungen von Azure-VMs

Für lokale Computer, die Sie in Azure replizieren, müssen die Azure-VM-Anforderungen für Betriebssystem und Architektur, Datenträger, Netzwerkeinstellungen und die VM-Benennung erfüllt sein. Überprüfen Sie vor der Migration die Anforderungen für [virtuelle VMware-Computer und physische Server](migrate-support-matrix-vmware-migration.md#azure-vm-requirements) sowie für [virtuelle Hyper-V-Computer](migrate-support-matrix-hyper-v-migration.md#azure-vm-requirements).

## <a name="prepare-to-connect-after-migration"></a>Vorbereiten der Verbindung nach der Migration

Azure-VMs werden während der Migration zu Azure erstellt. Nach der Migration müssen Sie eine Verbindung mit den neuen virtuellen Azure-Computern herstellen können. Für eine erfolgreiche Verbindungsherstellung sind mehrere Schritte erforderlich.

### <a name="prepare-to-connect-to-azure-windows-vms"></a>Vorbereiten der Verbindungsherstellung mit virtuellen Windows-Computern in Azure

Vorgehensweise für lokale Windows-Computer:

1. Konfigurieren Sie die Windows-Einstellungen. Hierzu gehört auch das Entfernen aller statischen beständigen Routen oder des WinHTTP-Proxys.
2. Vergewissern Sie sich, dass die [erforderlichen Dienste](../virtual-machines/windows/prepare-for-upload-vhd-image.md#check-the-windows-services) ausgeführt werden.
3. Aktivieren Sie den Remotedesktop (RDP), um Remoteverbindungen mit dem lokalen Computer zuzulassen. Informationen zum Aktivieren von RDP mithilfe von PowerShell finden Sie [hier](../virtual-machines/windows/prepare-for-upload-vhd-image.md#update-remote-desktop-registry-settings).
4. Wenn Sie nach der Migration über das Internet auf eine Azure-VM zugreifen möchten, lassen Sie in der Windows-Firewall auf dem lokalen Computer TCP und UDP im öffentlichen Profil zu, und legen Sie RDP als zulässige App für alle Profile fest.
5. Wenn Sie nach der Migration auf eine Azure-VM über ein Site-to-Site-VPN zugreifen möchten, lassen Sie in der Windows-Firewall auf dem lokalen Computer RDP für das Domänenprofil und private Profile zu. Informationen zum Zulassen von RDP-Datenverkehr finden Sie [hier](../virtual-machines/windows/prepare-for-upload-vhd-image.md#configure-windows-firewall-rules). 
6. Vergewissern Sie sich, dass auf dem lokalen virtuellen Computer keine ausstehenden Windows-Updates vorhanden sind, wenn Sie die Migration durchführen. Sollte dies nämlich der Fall sein, werden nach der Migration auf dem virtuellen Azure-Computer ggf. Updates installiert, und Sie können sich dann erst bei der VM anmelden, nachdem die Updates abgeschlossen sind.

### <a name="prepare-to-connect-with-linux-azure-vms"></a>Vorbereiten der Verbindung mit Linux-Azure-VMs

Vorgehensweise für lokale Linux-Computer:

1. Stellen Sie sicher, dass der Secure Shell-Dienst so festgelegt ist, dass er beim Systemstart automatisch gestartet wird.
2. Überprüfen Sie, ob die Firewallregeln eine SSH-Verbindung zulassen.

### <a name="configure-azure-vms-after-migration"></a>Vorbereiten virtueller Azure-Computer nach der Migration

Führen Sie nach der Migration auf den erstellten virtuellen Azure-Computern die folgenden Schritte aus:

1. Um aus dem Internet auf die VM zugreifen zu können, müssen Sie ihr eine öffentliche IP-Adresse zuweisen. Die öffentliche IP-Adresse für den virtuellen Azure-Computer muss sich von der öffentlichen IP-Adresse Ihres lokalen Computers unterscheiden. [Weitere Informationen](../virtual-network/virtual-network-public-ip-address.md)
2. Überprüfen Sie, ob die Regeln der Netzwerksicherheitsgruppen (NSG) auf dem virtuellen Computer eingehende Verbindungen am RDP- oder SSH-Port zulassen.
3. Überprüfen Sie die [Startdiagnose](../virtual-machines/troubleshooting/boot-diagnostics.md#enable-boot-diagnostics-on-existing-virtual-machine), um den virtuellen Computer anzuzeigen.

> [!NOTE]
> Der Azure Bastion-Dienst bietet privaten RDP- und SSH-Zugriff auf Azure-VMs. Sehen Sie sich die [weiteren Informationen](../bastion/bastion-overview.md) zu diesem Dienst an.

## <a name="next-steps"></a>Nächste Schritte

Entscheiden Sie, welche Methode Sie für die [Migration virtueller VMware-Computer](server-migrate-overview.md) zu Azure verwenden möchten, oder beginnen Sie mit der Migration von [virtuellen Hyper-V-Computern](tutorial-migrate-hyper-v.md) oder [physischen Servern oder virtualisierten oder cloudbasierten virtuellen Computern](tutorial-migrate-physical-virtual-machines.md).
