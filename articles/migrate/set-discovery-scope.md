---
title: Festlegen des Bereichs für die Ermittlung von VMware-VMs mit Azure Migrate
description: In diesem Artikel wird beschrieben, wie Sie den Suchbereich für die Bewertung und Migration von VMware-VMs mithilfe von Azure Migrate festlegen.
ms.topic: how-to
ms.date: 03/23/2020
ms.openlocfilehash: 29b3077ead168cef2790468d6ac62d1c59c24c11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337352"
---
# <a name="set-discovery-scope-for-vmware-vms"></a>Festlegen des Suchbereichs für VMware-VMs

In diesem Artikel wird beschrieben, wie Sie den Suchbereich für VMware-VMs festlegen, die von der [Azure Migrate-Appliance](migrate-appliance-architecture.md) ermittelt werden.

Die Azure Migrate-Appliance ermittelt in folgenden Situationen lokale VMware-VMs: 

- Bei Verwendung des Tools [Azure Migrate:Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool) zur Bewertung von VMware-VMs für die Migration zu Azure.
- Bei Verwendung des Tools [Azure Migrate:Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool) für die [Migration ohne Agent](server-migrate-overview.md) von VMware-VMs zu Azure.

## <a name="set-discovery-scope"></a>Festlegen des Suchbereichs


Nachdem Sie die Azure Migrate-Appliance für die Bewertung oder Migration von VMware-VMs eingerichtet haben, beginnt die Appliance mit der Ermittlung von VMs. Außerdem sendet die Appliance VM-Metadaten an Azure. Bevor Sie die Appliance mit vCenter Server verbinden, um die Ermittlung durchzuführen, können Sie den Suchbereich festlegen und auf bestimmte Rechenzentren oder Cluster, einen Clusterordner, Hosts, einen Hostordner oder einzelne VMs in vCenter Server beschränken.

Um den Bereich festzulegen, weisen Sie dem Konto, über das Azure Migrate auf vCenter Server zugreift, Berechtigungen zu.

## <a name="create-a-vcenter-user-account"></a>Erstellen eines vCenter-Benutzerkontos

Wenn Sie noch kein vCenter-Benutzerkonto eingerichtet haben, das von der Azure Migrate-Appliance für die Ermittlung, Bewertung und Migration von VMware-VMs verwendet wird, richten Sie zunächst ein solches Konto ein.

1.    Melden Sie sich als vCenter Server-Administrator beim vSphere-Webclient an.
2.    Wählen Sie **Administration** (Verwaltung)  > **SSO users and Groups** (SSO-Benutzer und -Gruppen) und anschließend die Registerkarte **Users** (Benutzer) aus.
3.    Wählen Sie das Symbol für **Neuer Benutzer** aus.
4.    Geben Sie die neuen Benutzerinformationen ein, und wählen Sie **OK**.

Die Kontoberechtigungen hängen davon ab, was Sie bereitstellen.

**Feature** | **Kontoberechtigungen**
--- | ---
[Bewerten](tutorial-assess-vmware.md)| Das Konto benötigt reinen Lesezugriff.
[Erkunden von Apps/Rollen/Features](how-to-discover-applications.md) | Das Konto benötigt reinen Lesezugriff. Dabei müssen Berechtigungen für „Virtual Machines“ (Virtuelle Maschinen) > „Guest Operations“ (Gastvorgänge) aktiviert sein.
[Analysieren von Abhängigkeiten (ohne Agent)](how-to-create-group-machine-dependencies-agentless.md) | Das Konto benötigt reinen Lesezugriff. Dabei müssen Berechtigungen für „Virtual Machines“ (Virtuelle Maschinen) > „Guest Operations“ (Gastvorgänge) aktiviert sein.
[Migrieren (ohne Agents)](tutorial-migrate-vmware.md) | Sie benötigen eine Rolle, der die erforderlichen Berechtigungen zugewiesen sind.<br/><br/> Um eine Rolle zu erstellen, melden Sie sich als vCenter Server-Administrator beim vSphere Web Client an. Wählen Sie die vCenter Server-Instanz und anschließend **Create role** (Rolle erstellen) aus. Geben Sie einen Rollennamen an (z. B. <em>Azure_Migrate</em>), und weisen Sie der Rolle die [erforderlichen Berechtigungen](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) zu.


## <a name="assign-permissions-on-vcenter-objects"></a>Zuweisen von Berechtigungen für vCenter-Objekte

Sie können Berechtigungen für Bestandsobjekte über eine der beiden folgenden Methoden zuweisen:

- Weisen Sie dem von Ihnen verwendeten Konto für alle übergeordneten Objekte, die VMs hosten, die Sie ermitteln/migrieren möchten, eine Rolle mit den erforderlichen Berechtigungen zu.
- Alternativ können Sie die Rolle und das Benutzerkonto auf Rechenzentrumsebene zuweisen und diese an die untergeordneten Objekte weitergeben. Weisen Sie dem Konto dann eine Rolle vom Typ **Kein Zugriff** für jedes Objekt zu, das Sie nicht ermitteln bzw. migrieren möchten. Diese Vorgehensweise wird jedoch nicht empfohlen. Sie ist mühsam und birgt das Risiko einer versehentlichen Preisgabe von Zugangsdaten, da jedem neuen untergeordneten Objekt automatisch die vom übergeordneten Objekt vererbten Zugangsrechte erteilt werden.

Gehen Sie folgendermaßen vor, um dem von Ihnen verwendeten Konto für alle relevanten Objekte eine Rolle zuzuweisen:

- **Für die Bewertung**: Wenden Sie für die VM-Bewertung die Rolle **Read-only** (Nur Lesen) auf das vCenter-Benutzerkonto für alle übergeordneten Objekte an, die VMs hosten, die Sie ermitteln möchten. Übergeordnete Objekte sind: Hosts, Hostordner, Cluster und Clusterordner innerhalb der Hierarchie bis hin zur Rechenzentrumsebene. Geben Sie diese Berechtigungen an die untergeordneten Objekte in der Hierarchie weiter.

    ![Zuweisen von Berechtigungen](./media/tutorial-assess-vmware/assign-perms.png)

- **Migration ohne Agent**: Wenden Sie für die Migration ohne Agent eine benutzerdefinierte Rolle mit den [erforderlichen Berechtigungen](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) auf das Benutzerkonto an, und zwar für alle übergeordneten Objekte, die VMs hosten, die Sie ermitteln möchten. Sie können der Rolle den Namen <em>Azure_Migrate</em> zuweisen.

### <a name="scope-support"></a>Bereichsunterstützung

Das Serverbewertungstool kann aktuell keine VMs ermitteln, wenn dem vCenter-Konto Zugriff auf vCenter-VM-Ordnerebene gewährt wurde. Host- und Clusterordner werden unterstützt.

Wenn Ihre Ermittlung auf VM-Ordnern basieren soll, führen Sie die folgenden Schritte aus, um sicherzustellen, dass das vCenter-Konto auf VM-Ebene über reinen Lesezugriff verfügt.

1. Weisen Sie reine Leseberechtigungen für alle VMs in den VM-Ordnern zu, auf denen die Ermittlung basieren soll.
2. Erteilen Sie reinen Lesezugriff für alle übergeordneten Objekte, die VMs hosten.
    - Es werden alle übergeordneten Objekte (Hosts, Hostordner, Cluster, Clusterordner) in der Hierarchie bis hin zur Rechenzentrumsebene einbezogen.
    - Die Berechtigungen müssen nicht an alle untergeordneten Objekte weitergegeben werden.
3. Verwenden Sie die Anmeldeinformationen für die Ermittlung, indem Sie das Rechenzentrum als **Sammlungsbereich** auswählen. Durch die Einrichtung der rollenbasierten Zugriffssteuerung wird sichergestellt, dass das entsprechende vCenter-Benutzerkonto nur auf mandantenspezifische VMs Zugriff hat.


## <a name="next-steps"></a>Nächste Schritte

[Richten Sie die Appliance ein](how-to-set-up-appliance-vmware.md), und [starten Sie die kontinuierliche Ermittlung](how-to-set-up-appliance-vmware.md#start-continuous-discovery-by-providing-vcenter-server-and-vm-credential).
