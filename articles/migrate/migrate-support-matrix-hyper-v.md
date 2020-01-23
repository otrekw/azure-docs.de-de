---
title: Unterstützung der Hyper-V-Bewertung in Azure Migrate
description: Erfahren Sie etwas über die Unterstützung der Hyper-V-Bewertung mit Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 20bdbb16d2f0610f6519424141b09190eae3cc42
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028790"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Unterstützungsmatrix für die Hyper-V-Bewertung

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Bewertung virtueller Hyper-V-Computer mithilfe von [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool). Informationen zur Migration von virtuellen Hyper-V-Computern zu Azure finden Sie unter [Unterstützungsmatrix für die Hyper-V-Migration](migrate-support-matrix-hyper-v-migration.md).

## <a name="overview"></a>Übersicht

Zum Bewerten von lokalen Computern für die Migration in Azure entsprechend der Beschreibung in diesem Artikel fügen Sie das Tool „Azure Migrate: Serverbewertung“ einem Azure Migrate-Projekt hinzu. Sie stellen die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance ermittelt kontinuierlich lokale Computer und sendet Konfigurations- und Leistungsdaten an Azure. Nach der Computerermittlung ordnen Sie die ermittelten Computer in Gruppen und führen eine Bewertung für die Gruppen aus.


## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen**| In einem [Projekt](migrate-support-matrix.md#azure-migrate-projects) können bis zu 35.000 virtuelle Hyper-V-Computer ermittelt und bewertet werden.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen virtuelle VMware-Computer, virtuelle Hyper-V-Computer und physische Server umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 5.000 virtuelle Hyper-V-Computer ermitteln.<br/><br/> Die Appliance kann Verbindungen mit bis zu 300 Hyper-V-Hosts herstellen.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einem einzelnen Vorgang bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.



## <a name="hyper-v-host-requirements"></a>Anforderungen für Hyper-V-Hosts

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Hostbereitstellung**       | Der Hyper-V-Host kann eigenständig oder in einem Cluster bereitgestellt werden. |
| **Berechtigungen**           | Sie benötigen Administratorrechte auf dem Hyper-V-Host. <br/> Falls Sie keine Administratorberechtigungen zuweisen möchten, können Sie alternativ ein lokales Benutzerkonto oder ein Domänenbenutzerkonto erstellen und den Benutzer den folgenden Gruppen zuweisen: „Remoteverwaltungsbenutzer“, „Hyper-V-Administratoren“ und „Systemmonitorbenutzer“. |
| **Betriebssystem des Hosts** | Windows Server 2019, Windows Server 2016 oder Windows Server 2012 R2.<br/> Sie können keine VMs auf Hyper-V-Hosts mit Windows Server 2012 bewerten. |
| **PowerShell-Remoting**   | Muss auf jedem Host aktiviert sein |
| **Hyper-V-Replikat**       | Wenn Sie Hyper-V Replikate verwenden (oder mehrere VMs mit den gleichen VM-Bezeichnern vorliegen) und sowohl die ursprünglichen als auch die replizierten VMs mit Azure Migrate ermitteln, ist die von Azure Migrate generierte Bewertung möglicherweise nicht korrekt. |


## <a name="hyper-v-vm-requirements"></a>Anforderungen für virtuelle Hyper-V-Computer

| **Unterstützung**                  | **Details**               
| :----------------------------- | :------------------- |
| **Betriebssystem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)-Betriebssysteme, die von Azure unterstützt werden |
| **Integrationsdienste**       | [Hyper-V Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) muss auf den von Ihnen bewerteten VMs ausgeführt werden, um Betriebssysteminformationen zu erfassen. |


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Die Appliance für Hyper-V wird auf einem virtuellen Hyper-V-Computer ausgeführt und mithilfe einer über das Azure-Portal heruntergeladenen komprimierten Hyper-V-VHD bereitgestellt. 

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---hyper-v) für Hyper-V.
- Erfahren Sie etwas über die [URLs](migrate-appliance.md#url-access), auf die die Appliance Zugriff benötigt.

## <a name="port-access"></a>Portzugriff

Die folgende Tabelle fasst die Portanforderungen für die Bewertung zusammen.

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ``` https://<appliance-ip-or-name>:44368 ``` auf die Applianceverwaltungs-App zugreifen zu können.<br/> Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**Hyper-V-Host/-Cluster** | Eingehende Verbindungen an den WinRM-Ports 5985 (HTTP) und 5986 (HTTPS) zum Abrufen von Konfigurations- und Leistungsmetadaten der Hyper-V-VMs mithilfe einer CIM-Sitzung (Common Information Model)



## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten der Bewertung von virtuellen Hyper-V-Computern](tutorial-prepare-hyper-v.md)
