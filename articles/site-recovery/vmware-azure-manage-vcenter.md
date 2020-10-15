---
title: Verwalten von VMware vCenter Server in Azure Site Recovery
description: Dieser Artikel beschreibt, wie Sie VMware vCenter für die Notfallwiederherstellung von VMware-VMs in Azure mit Azure Site Recovery hinzufügen und verwalten.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 12/24/2019
ms.author: ramamill
ms.openlocfilehash: 01aef3aca4f6967b1681bff9598c7dd7a24739cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84692518"
---
# <a name="manage-vmware-vcenter-server"></a>Verwalten von VMware vCenter-Servern

In diesem Artikel werden die Verwaltungsaktionen für VMware vCenter Server in [Azure Site Recovery](site-recovery-overview.md) zusammengefasst.

## <a name="verify-prerequisites-for-vcenter-server"></a>Überprüfen der Voraussetzungen für vCenter Server

Die Voraussetzungen für vCenter Server und -VMs während der Notfallwiederherstellung von VMware-VMs in Azure werden in der [Unterstützungsmatrix](vmware-physical-azure-support-matrix.md#replicated-machines) aufgeführt.

## <a name="set-up-an-account-for-automatic-discovery"></a>Einrichten eines Kontos für die automatische Ermittlung

Wenn Sie die Notfallwiederherstellung für lokale VMware-VMs einrichten, benötigt Site Recovery Zugriff auf den vCenter Server-/vSphere-Host. Der Site Recovery-Prozessserver kann dann automatisch VMs ermitteln und bei Bedarf ein Failover durchführen. Der Prozessserver wird standardmäßig auf dem Site Recovery-Konfigurationsserver ausgeführt. Fügen Sie wie folgt ein Konto für den Konfigurationsserver hinzu, um eine Verbindung mit dem vCenter Server-/vSphere-Host herzustellen:

1. Melden Sie sich beim Konfigurationsserver an.
1. Öffnen Sie das Konfigurationsservertool (Datei _cspsconfigtool.exe_) über die Desktopverknüpfung.
1. Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.

   ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)

1. Geben Sie die Kontodetails ein, und klicken Sie auf **OK**, um es hinzuzufügen. Für das Konto sollten die Berechtigungen in der Tabelle der Kontoberechtigungen zusammengefasst sein.

   > [!NOTE]
   > Das Synchronisieren von Kontoinformationen mit Site Recovery dauert ungefähr 15 Minuten.

### <a name="account-permissions"></a>Kontoberechtigungen

|**Aufgabe** | **Konto** | **Berechtigungen** | **Details**|
|--- | --- | --- | ---|
|**Ermittlung/Migration von VMs (ohne Failback)** | Mindestens ein Benutzerkonto mit Lesezugriff | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.|
|**Replikation/Failover** | Mindestens ein Benutzerkonto mit Lesezugriff | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.<br/><br/> Ist für Migrationszwecke geeignet, aber nicht für die vollständige Replikation, Failover oder Failback.|
|**Replikation/Failover/Failback** | Wir empfehlen, dass Sie eine Rolle (AzureSiteRecoveryRole) mit den erforderlichen Berechtigungen erstellen und sie dann einem VMware-Benutzer oder einer VMware-Gruppe zuweisen. | Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=AzureSiteRecoveryRole<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.|

## <a name="add-vmware-server-to-the-vault"></a>Hinzufügen von VMware Server zum Tresor

Wenn Sie die Notfallwiederherstellung für lokale VMware-VMs einrichten, fügen Sie den vCenter Server-/vSphere-Host, auf dem Sie VMs ermitteln, wie folgt dem Site Recovery-Tresor hinzu:

1. Navigieren Sie im Tresor zu **Site Recovery-Infrastruktur** > **Konfigurationsserver**, und öffnen Sie den Konfigurationsserver.
1. Klicken Sie auf der Seite **Details** auf **vCenter**.
1. Geben Sie unter **vCenter-Server hinzufügen** einen Anzeigenamen für den vSphere-Host oder vCenter-Server an.
1. Geben Sie anschließend die IP-Adresse oder den FQDN des Servers an.
1. Belassen Sie den Port als Port 443, es sei denn Ihre VMware-Server sind so konfiguriert, dass sie auf Anforderungen auf einem anderen Port lauschen können.
1. Wählen Sie das Konto aus, unter dem die Verbindung mit dem VMware vCenter- oder vSphere ESXi-Server hergestellt werden soll. Klicken Sie dann auf **OK**.

## <a name="modify-credentials"></a>Ändern von Anmeldeinformationen

Ändern Sie bei Bedarf wie im Folgenden beschrieben die Anmeldeinformationen zum Herstellen der Verbindung mit dem vCenter Server-/vSphere-Host:

1. Melden Sie sich beim Konfigurationsserver an.
1. Öffnen Sie das Konfigurationsservertool (Datei _cspsconfigtool.exe_) über die Desktopverknüpfung.
1. Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.

   ![add-account](./media/vmware-azure-manage-vcenter/addaccount.png)

1. Geben Sie die Details des neuen Kontos ein, und klicken Sie auf **OK**. Das Konto benötigt die in der Tabelle [Kontoberechtigungen](#account-permissions) aufgeführten Berechtigungen.
1. Navigieren Sie im Tresor zu **Site Recovery-Infrastruktur** > **Konfigurationsserver**, und öffnen Sie den Konfigurationsserver.
1. Klicken Sie unter **Details** auf **Server aktualisieren**.
1. Nachdem der Auftrag zur Serveraktualisierung abgeschlossen ist, wählen Sie vCenter Server aus.
1. Wählen Sie unter **Zusammenfassung** das neu hinzugefügte Konto unter **vCenter Server-/vSphere-Hostkonto** aus, und klicken Sie auf **Speichern**.

   ![modify-account](./media/vmware-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-server"></a>Löschen von vCenter Server

1. Navigieren Sie im Tresor zu **Site Recovery-Infrastruktur** > **Konfigurationsserver**, und öffnen Sie den Konfigurationsserver.
1. Wählen Sie vCenter Server auf der Seite **Details**.
1. Klicken Sie auf die Schaltfläche **Löschen**.

   ![delete-account](./media/vmware-azure-manage-vcenter/delete-vcenter.png)

## <a name="modify-the-ip-address-and-port"></a>Ändern von IP-Adresse und Port

Sie können die IP-Adresse von vCenter Server oder die Ports ändern, die für die Kommunikation zwischen dem Server und Site Recovery verwendet werden. Standardmäßig greift Site Recovery über Port 443 auf Informationen vom vCenter Server-/vSphere-Host zu.

1. Klicken Sie im Tresor unter **Site Recovery-Infrastruktur** > **Konfigurationsserver** auf den Konfigurationsserver, dem vCenter Server hinzugefügt wurde.
1. Klicken Sie unter **vCenter Server** auf die vCenter Server-Instanz, die Sie ändern möchten.
1. Aktualisieren Sie unter **Zusammenfassung** die IP-Adresse und den Port, und speichern Sie die Änderungen.

   ![add_ip_new_vcenter](media/vmware-azure-manage-vcenter/add-ip.png)

1. Damit die Änderungen umgesetzt werden, warten Sie 15 Minuten lang oder [aktualisieren Sie den Konfigurationsserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="migrate-all-vms-to-a-new-server"></a>Migrieren aller VMs zu einem neuen Server

Wenn Sie alle virtuellen Computer für die Verwendung einer neuen vCenter Server-Instanz migrieren möchten, müssen Sie nur die IP-Adresse aktualisieren, die vCenter Server zugewiesen ist. Fügen Sie kein zusätzliches VMware-Konto hinzu, da dies zu doppelten Einträgen führen kann. Aktualisieren Sie die Adresse wie folgt:

1. Klicken Sie im Tresor unter **Site Recovery-Infrastruktur** > **Konfigurationsserver** auf den Konfigurationsserver, dem vCenter Server hinzugefügt wurde.
1. Klicken Sie im Abschnitt **vCenter Server** auf die vCenter Server-Instanz, von der Sie migrieren möchten.
1. Ändern Sie unter **Zusammenfassung** die IP-Adresse in die der neuen vCenter Server-Instanz, und speichern Sie die Änderungen.
1. Sobald die IP-Adresse aktualisiert wurde, empfängt Site Recovery Ermittlungsinformationen zu VMs von der neuen vCenter Server-Instanz. Dies beeinträchtigt nicht die laufenden Replikationsaktivitäten.

## <a name="migrate-a-few-vms-to-a-new-server"></a>Migrieren einiger VMs zu einem neuen Server

Wenn Sie nur einige der replizierten VMs zu einer neuen vCenter Server-Instanz migrieren möchten, gehen Sie folgendermaßen vor:

1. [Fügen](#add-vmware-server-to-the-vault) Sie auf dem Konfigurationsserver die neue vCenter Server-Instanz hinzu.
1. [Deaktivieren Sie die Replikation](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) für VMs, die auf den neuen Server verschoben werden.
1. Migrieren Sie in VMware die virtuellen Computer zur neuen vCenter Server-Instanz.
1. [Aktivieren Sie die Replikation](vmware-azure-tutorial.md#enable-replication) für die migrierten VMs erneut, und wählen Sie die neue vCenter Server-Instanz aus.

## <a name="migrate-most-vms-to-a-new-server"></a>Migrieren der meisten VMs zu einem neuen Server

Wenn die Anzahl der virtuellen Computer, die Sie zu einer neuen vCenter Server-Instanz migrieren möchten, größer ist als die Anzahl der VMs, die auf der ursprünglichen vCenter Server-Instanz verbleiben, gehen Sie folgendermaßen vor:

1. [Ändern Sie die IP-Adresse](#modify-the-ip-address-and-port), die vCenter Server in den Einstellungen auf dem Konfigurationsserver zugewiesen ist, in die Adresse der neuen vCenter Server-Instanz.
1. [Deaktivieren Sie die Replikation](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) für die wenigen, auf dem alten Server verbleibenden VMs erneut.
1. [Fügen](#add-vmware-server-to-the-vault) Sie dem Konfigurationsserver die alte vCenter Server-Instanz und ihre IP-Adresse hinzu.
1. [Aktivieren Sie die Replikation](vmware-azure-tutorial.md#enable-replication) für die auf dem alten Server verbleibenden VMs erneut.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Probleme haben, finden Sie weitere Informationen unter [Problembehandlung von vCenter Server-Ermittlungsfehlern](vmware-azure-troubleshoot-vcenter-discovery-failures.md).
