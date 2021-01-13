---
title: Unterstützte VM-SKUs für Azure Policy
description: Artikel, der die unterstützten VM-SKUs (nach Herausgeber, Image-Angebot und Image-SKU) beschreibt, die für die von Azure Backup bereitgestellten integrierten Azure-Richtlinien unterstützt werden
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 4658d1beffe707682e173491edea1eac0db9c811
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82183618"
---
# <a name="supported-vm-skus-for-azure-policy"></a>Unterstützte VM-SKUs für Azure Policy

Azure Backup bietet eine integrierte Richtlinie (unter Verwendung von Azure Policy), die **allen Azure-VMS an einem bestimmten Standort innerhalb eines Abonnements oder einer Ressourcengruppe** zugewiesen werden kann. Wird diese Richtlinie einem bestimmten Bereich zugewiesen, werden alle in diesem Bereich erstellten neuen virtuellen Computer automatisch für die Sicherung in einem **vorhandenen Tresor am gleichen Speicherort und im gleichen Abonnement** konfiguriert. In der folgenden Tabelle sind alle von dieser Richtlinie unterstützten VM-SKUs aufgeführt.

## <a name="supported-vms"></a>Unterstützte VMs*

**Richtlinienname:** Sicherung von VMs eines Standorts in einem vorhandenen zentralen Tresor am selben Standort konfigurieren

Imageherausgeber | Imageangebot | Image-SKU
--- | --- | ---
MicrosoftWindowsServer | Windows Server | Windows Server 2008 R2 SP1 (2008-R2-SP1)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2008 R2 SP (2008-R2-SP1-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2012 Datacenter (2012-Datacenter)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2012 Datacenter (2012-Datacenter-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2012 R2 Datacenter (2012-R2-Datacenter)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2012 R2 Datacenter (2012-R2-Datacenter-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2016 Datacenter (2016-Datacenter)
MicrosoftWindowsServer | Windows Server | Windows Server 2016 Datacenter – Server Core (2016-Datacenter-Server-Core)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2016 Datacenter – Server Core (2016-Datacenter-Server-Core-smalldisk)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2016 Datacenter (2016-Datacenter-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2019 Datacenter Server Core with Containers (2016-Datacenter-with-Containers)
MicrosoftWindowsServer | Windows Server | Windows Server 2016 Remote Desktop Session Host 2016 (2016-Datacenter-with-RDSH)
MicrosoftWindowsServer | Windows Server | Windows Server 2019 Datacenter (2019-Datacenter)
MicrosoftWindowsServer | Windows Server | Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2019 Datacenter Server Core (2019-Datacenter-Core-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2019 Datacenter Server Core with Containers (2019-Datacenter-Core-with-Containers)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2019 Datacenter Server Core with Containers (2019-Datacenter-Core-with-Containers-smalldisk)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2019 Datacenter (2019-Datacenter-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2019 Datacenter with Containers (2019-Datacenter-with-Containers)
MicrosoftWindowsServer | Windows Server | [smalldisk] Windows Server 2019 Datacenter with Containers (2019-Datacenter-with-Containers-smalldisk)
MicrosoftWindowsServer | Windows Server | Windows Server 2019 Datacenter (zh-cn) (2019-Datacenter-zhcn)
MicrosoftWindowsServer | WindowsServerSemiAnnual | Datacenter-Core-1709-smalldisk
MicrosoftWindowsServer | WindowsServerSemiAnnual | Datacenter-Core-1709-with-Containers-smalldisk
MicrosoftWindowsServer | WindowsServerSemiAnnual | Datacenter-Core-1803-with-Containers-smalldisk
MicrosoftWindowsServerHPCPack | WindowsServerHPCPack | Alle Image-SKUs
MicrosoftSQLServer | SQL2016SP1-WS2016 | Alle Image-SKUs
MicrosoftSQLServer | SQL2016-WS2016 | Alle Image-SKUs
MicrosoftSQLServer | SQL2016SP1-WS2016-BYOL | Alle Image-SKUs
MicrosoftSQLServer | SQL2012SP3-WS2012R2 | Alle Image-SKUs
MicrosoftSQLServer | SQL2016-WS2012R2 | Alle Image-SKUs
MicrosoftSQLServer | SQL2014SP2-WS2012R2 | Alle Image-SKUs
MicrosoftSQLServer | SQL2012SP3-WS2012R2-BYOL | Alle Image-SKUs
MicrosoftSQLServer | SQL2014SP1-WS2012R2-BYOL | Alle Image-SKUs
MicrosoftSQLServer | SQL2014SP2-WS2012R2-BYOL | Alle Image-SKUs
MicrosoftSQLServer | SQL2016-WS2012R2-BYOL | Alle Image-SKUs
MicrosoftRServer | MLServer-WS2016 | Alle Image-SKUs
MicrosoftVisualStudio | VisualStudio | Alle Image-SKUs
MicrosoftVisualStudio | Windows | Alle Image-SKUs
MicrosoftDynamicsAX | Dynamics | Pre-Req-AX7-Onebox-U8
microsoft-ads | windows-data-science-vm | Alle Image-SKUs
MicrosoftWindowsDesktop | Windows-10 | Alle Image-SKUs
RedHat | RHEL | 6.7, 6.8, 6.9, 6.10, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7
RedHat | RHEL-SAP-HANA | 6.7, 7.2, 7.3
SUSE | SLES | 12.X
SUSE | SLES-HPC | 12.X
SUSE | SLES-HPC-Priority | 12.X
SUSE | SLES-SAP | 12.X
SUSE | SLES-SAP-BYOS | 12.X
SUSE | SLES-Priority | 12.X
SUSE | SLES-BYOS | 12.X
SUSE | SLES-SAPCAL | 12.X
SUSE | SLES-Standard | 12.X
Canonical | UbuntuServer | 14.04.0-LTS
Canonical | UbuntuServer | 14.04.1-LTS
Canonical | UbuntuServer | 14.04.2-LTS
Canonical | UbuntuServer | 14.04.3-LTS
Canonical | UbuntuServer | 14.04.4-LTS
Canonical | UbuntuServer | 14.04.5-DAILY-LTS
Canonical | UbuntuServer | 14.04.5-LTS
Canonical | UbuntuServer | 16.04-DAILY-LTS
Canonical | UbuntuServer | 16.04-LTS
Canonical | UbuntuServer | 16.04.0-LTS
Canonical | UbuntuServer | 18.04-DAILY-LTS
Canonical | UbuntuServer | 18.04-LTS
Oracle | Oracle-Linux | 6.8, 6.9, 6.10, 7.3, 7.4, 7.5, 7.6
OpenLogic | CentOS | 6.X, 7.X
OpenLogic | CentOS–LVM | 6.X, 7.X
OpenLogic | CentOS–SRIOV | 6.X, 7.X
cloudera | cloudera-centos-os | 7.X
