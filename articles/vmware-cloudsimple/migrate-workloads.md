---
title: 'Azure VMware Solutions (AVS): Migrieren von Workload-VMs zur privaten AVS-Cloud'
description: In diesem Artikel wird beschrieben, wie Sie virtuelle Computer aus einer lokalen vCenter-Instanz zu einer vCenter-Instanz in einer privaten AVS-Cloud migrieren.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: af02bd947c73b670306704a10a09ca981b34c9a9
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019994"
---
# <a name="migrate-workload-vms-from-on-premises-vcenter-to-avs-private-cloud-vcenter-environment"></a>Migrieren von Workload-VMs aus einer lokalen vCenter-Instanz zur vCenter-Umgebung in einer privaten AVS-Cloud

Zum Migrieren von VMs aus einem lokalen Rechenzentrum zu Ihrer privaten AVS-Cloud stehen verschiedene Optionen zur Verfügung. Die private AVS-Cloud bietet nativen Zugriff auf VMware vCenter, und für die Workloadmigration können von VMware unterstützte Tools verwendet werden. In diesem Artikel werden einige der vCenter-Migrationsoptionen beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

Für die Migration von VMs und Daten aus Ihrem lokalen Rechenzentrum ist eine Netzwerkverbindung zwischen dem Rechenzentrum und Ihrer privaten AVS-Cloudumgebung erforderlich. Verwenden Sie eine der folgenden Methoden, um die Netzwerkverbindung herzustellen:

* [Site-to-Site-VPN-Verbindung](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) zwischen Ihrer lokalen Umgebung und Ihrer privaten AVS-Cloud.
* ExpressRoute Global Reach-Verbindung zwischen Ihrer lokalen ExpressRoute-Leitung und einer AVS-ExpressRoute-Leitung.

Der Netzwerkpfad von Ihrer lokalen vCenter-Umgebung zu Ihrer privaten AVS-Cloud muss verfügbar sein, um VMs mithilfe von vMotion zu migrieren. Das vMotion-Netzwerk in Ihrem lokalen vCenter muss über Routingfähigkeiten verfügen. Vergewissern Sie sich, dass Ihre Firewall den gesamten vMotion-Datenverkehr zwischen Ihrer lokalen vCenter-Instanz und der Instanz in Ihrer privaten AVS-Cloud zulässt. (In der privaten AVS-Cloud ist das Routing im vMotion-Netzwerk standardmäßig konfiguriert.)

## <a name="migrate-isos-and-templates"></a>Migrieren von ISO-Dateien und Vorlagen

Zum Erstellen neuer virtueller Computer in Ihrer privaten AVS-Cloud dienen ISO-Dateien und VM-Vorlagen. Verwenden Sie die folgende Methode, um die ISO-Dateien und Vorlagen in die vCenter-Instanz in Ihrer privaten AVS-Cloud hochzuladen und verfügbar zu machen.

1. Laden Sie die ISO-Datei auf der vCenter-Benutzeroberfläche in die vCenter-Instanz Ihrer privaten AVS-Cloud hoch.
2. [Veröffentlichen Sie eine Inhaltsbibliothek](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A0F1C13-7336-45CE-B211-610D39A6E1F4.html) in der vCenter-Instanz Ihrer privaten AVS-Cloud:

    1. Veröffentlichen Sie Ihre lokale Inhaltsbibliothek.
    2. Erstellen Sie eine neue Inhaltsbibliothek in der vCenter-Instanz Ihrer privaten AVS-Cloud.
    3. Abonnieren Sie die veröffentlichte lokale Inhaltsbibliothek.
    4. Synchronisieren Sie die Inhaltsbibliothek, damit auf die abonnierten Inhalte zugegriffen werden kann.

## <a name="migrate-vms-using-powercli"></a>Migrieren von VMs mithilfe von PowerCLI

Um VMs aus der lokalen vCenter-Instanz zur vCenter-Instanz der privaten AVS-Cloud zu migrieren, verwenden Sie VMware PowerCLI oder das vCenter-übergreifende Hilfsprogramm für die Workloadmigration (Workload Migration Utility), das in VMware Labs verfügbar ist. Das folgende Beispielskript zeigt die PowerCLI-Migrationsbefehle.

```
$sourceVC = Connect-VIServer -Server <source-vCenter name> -User <source-vCenter user name> -Password <source-vCenter user password>
$targetVC = Connect-VIServer -Server <target-vCenter name> -User <target-vCenter user name> -Password <target-vCenter user password>
$vmhost = <name of ESXi host on destination>
$vm = Get-VM -Server $sourceVC <name of VM>
Move-VM -VM $vm -VMotionPriority High -Destination (Get-VMhost -Server $targetVC -Name $vmhost) -Datastore (Get-Datastore -Server $targetVC -Name <name of tgt vc datastore>)
```

> [!NOTE]
> Um die Namen des vCenter-Zielservers und der ESXi-Zielhosts zu verwenden, konfigurieren Sie die DNS-Weiterleitung von Ihrer lokalen Umgebung zu Ihrer privaten AVS-Cloud.

## <a name="migrate-vms-using-nsx-layer-2-vpn"></a>Migrieren von VMs mit einem NSX-Layer-2-VPN

Diese Option ermöglicht eine Livemigration von Workloads von Ihrer lokalen VMware-Umgebung zur privaten AVS-Cloud in Azure. Mit diesem Layer-2-Stretchingnetzwerk ist das Subnetz der lokalen Umgebung in der privaten AVS-Cloud verfügbar. Nach der Migration müssen den VMs keine neuen IP-Adressen zugewiesen werden.

Unter [Migrieren von Workloads unter Verwendung von Layer-2-Stretchingnetzwerken](migration-layer-2-vpn.md) wird beschrieben, wie Sie mit einem Layer-2-VPN das Stretching eines Layer-2-Netzwerks von Ihrer lokalen Umgebung auf Ihre private AVS-Cloud durchführen.

## <a name="migrate-vms-using-backup-and-disaster-recovery-tools"></a>Migrieren von VMs mithilfe von Tools für die Sicherung und Notfallwiederherstellung

Die Migration von VMs zu einer privaten AVS-Cloud kann mithilfe von Sicherungs-/Wiederherstellungstools sowie Notfallwiederherstellungstools durchgeführt werden. Verwenden Sie die private AVS-Cloud als Ziel für die Wiederherstellung aus Sicherungen, die mithilfe eines Drittanbietertools erstellt wurden. Die private AVS-Cloud kann auch als Ziel für die Notfallwiederherstellung über VMware SRM oder ein Drittanbietertool verwendet werden.

Weitere Informationen zu diesen Tools finden Sie in den folgenden Themen:

* [Sichern von Workload-VMs in einer privaten AVS-Cloud mithilfe von Veeam B&R](backup-workloads-veeam.md)
* [Einrichten der privaten AVS-Cloud als Notfallwiederherstellungsstandort für lokale VMware-Workloads](disaster-recovery-zerto.md)
