---
title: Unterstützungsmatrix für Azure Migrate
description: Enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen für den Azure Migrate-Dienst.
ms.topic: conceptual
ms.date: 01/28/2020
ms.author: raynew
ms.openlocfilehash: 5c29b80f30b024d34ec4e8f65e51b59fc70e8f93
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846563"
---
# <a name="azure-migrate-support-matrix"></a>Unterstützungsmatrix für Azure Migrate

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel enthält eine Zusammenfassung der allgemeinen Supporteinstellungen und Einschränkungen für Azure Migrate-Szenarien und -Bereitstellungen.

## <a name="supported-assessmentmigration-scenarios"></a>Unterstützte Bewertungs-/Migrationsszenarien

Die Tabelle enthält eine Übersicht über die unterstützten Ermittlungs-, Bewertungs- und Migrationsszenarien.

**Bereitstellung** | **Details** 
--- | --- 
**App-spezifische Ermittlung** | Sie können die Apps, Rollen und Features ermitteln, die auf VMware-VMs ausgeführt werden. Diese Funktion ist zurzeit nur auf die Ermittlung beschränkt. Die Bewertung erfolgt zurzeit auf der Computerebene. Es ist noch keine app-, rollen- oder featurespezifische Bewertung verfügbar. 
**Lokale Bewertung** | Bewerten Sie lokale Workloads und Daten, die auf VMware-VMs, Hyper-V-VMs und physischen Servern ausgeführt werden. Verwenden Sie für die Bewertung die Azure Migrate-Serverbewertung und den Microsoft-Datenmigrations-Assistenten (DMA) sowie andere Tools und ISV-Angebote.
**Migration von einem lokalen Standort zu Azure** | Migrieren Sie auf physischen Servern, VMware-VMs, Hyper-V-VMs und cloudbasierten VMs ausgeführte Workloads und Daten zu Azure. Verwenden Sie für die Migration die Azure Migrate-Serverbewertung und Azure Database Migration Service (DMS) sowie andere Tools und ISV-Angebote.


## <a name="supported-tools"></a>Unterstützte Tools

Die Informationen zur Toolunterstützung sind in der Tabelle zusammengefasst.

**Tool** | **Bewerten** | **Migrieren** 
--- | --- | ---
Azure Migrate-Serverbewertung | Dient zum Bewerten von [VMware-VMs](tutorial-prepare-vmware.md), [Hyper-V-VMs](tutorial-prepare-hyper-v.md) und [physischen Servern](tutorial-prepare-physical.md). |  Nicht verfügbar (N/V)
Azure Migrate-Servermigration | Nicht verfügbar | Dient zum Migrieren von [VMware-VMs](tutorial-migrate-vmware.md), [Hyper-V-VMs](tutorial-migrate-hyper-v.md) und [physischen Servern](tutorial-migrate-physical-virtual-machines.md).
[Carbonite](https://www.carbonite.com/data-protection-resources/resource/Datasheet/carbonite-migrate-for-microsoft-azure) | Nicht verfügbar | Dient zum Migrieren von VMware-VMs, Hyper-V-VMs, physischen Servern und Public Cloud-Workloads. 
[Cloudamize](https://www.cloudamize.com/platform#tab-0)| Dient zum Bewerten von VMware-VMs, Hyper-V-VMs, physischen Servern und Public Cloud-Workloads. | Nicht verfügbar
[Corent Technology](https://go.microsoft.com/fwlink/?linkid=2084928) | Dient zum Bewerten und Migrieren von VMware-VMs, Hyper-V-VMs, physischen Servern und Public Cloud-Workloads. |  Dient zum Migrieren von VMware-VMs, Hyper-V-VMs, physischen Servern und Public Cloud-Workloads.
[Device42](https://go.microsoft.com/fwlink/?linkid=2097158) | Dient zum Bewerten von VMware-VMs, Hyper-V-VMs, physischen Servern und Public Cloud-Workloads.| Nicht verfügbar
[DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) | Dient zum Bewerten von lokalen SQL Server-Datenbanken. | Nicht verfügbar
[DMS](https://docs.microsoft.com/azure/dms/dms-overview) | Nicht verfügbar | Dient zum Migrieren von SQL Server, Oracle, MySQL, PostgreSQL und MongoDB. 
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Dient zum Bewerten der Virtual Desktop Infrastructure (VDI). | Nicht verfügbar
[Movere](https://go.microsoft.com/fwlink/?linkid=2109528) | Dient zum Bewerten von VMWare-VMs, Hyper-V-VMs, Xen-VMs, physischen Computern, Arbeitsstationen (z. B. VDI) und Public Cloud-Workloads. | Nicht verfügbar
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Nicht verfügbar | Dient zum Migrieren von VMWare-VMs, Hyper-V-VMs, Xen-VMs, KVM-VMs, physischen Computern und Public Cloud-Workloads. 
[Turbonomic](https://go.microsoft.com/fwlink/?linkid=2094295)  | Dient zum Bewerten von VMware-VMs, Hyper-V-VMs, physischen Servern und Public Cloud-Workloads. | Nicht verfügbar
[UnifyCloud](https://go.microsoft.com/fwlink/?linkid=2097195) | Dient zum Bewerten von VMware-VMs, Hyper-V-VMs, physischen Servern, Public Cloud-Workloads und SQL Server-Datenbanken. | Nicht verfügbar
[Migrations-Assistent für Web-Apps](https://appmigration.microsoft.com/) | Dient zum Bewerten von Web-Apps. | Dient zum Migrieren von Web-Apps.


## <a name="azure-migrate-projects"></a>Azure Migrate-Projekte

**Unterstützung** | **Details**
--- | ---
Subscription | Ein Abonnement kann mehrere Azure Migrate-Projekte aufweisen.
Azure-Berechtigungen | Sie benötigen Berechtigungen für Mitwirkende oder Eigentümer im Abonnement, um ein Azure Migrate-Projekt zu erstellen.
Virtuelle VMware-Computer  | Bewerten Sie bis zu 35.000 VMware-VMs in einem einzigen Projekt.
Virtuelle Hyper-V-Computer | Bewerten Sie bis zu 35.000 Hyper-V-VMs in einem einzigen Projekt.

Ein Projekt kann im Rahmen der Bewertungseinschränkungen sowohl VMware-VMs als auch Hyper-V-VMs umfassen.

## <a name="azure-permissions"></a>Azure-Berechtigungen

Damit Azure Migrate mit Azure verwendet werden kann, benötigen Sie die folgenden Berechtigungen, um mit der Bewertung und Migration von Computern beginnen zu können.

**Aufgabe** | **Berechtigungen** | **Details**
--- | --- | ---
Erstellen eines Azure Migrate-Projekts | Ihr Azure-Konto benötigt Berechtigungen zum Erstellen eines Projekts. | Einrichtung für [VMware](tutorial-prepare-vmware.md#assign-permissions-to-create-project), [Hyper-V](tutorial-prepare-hyper-v.md#assign-permissions-to-create-project) oder [physische Server](tutorial-prepare-physical.md#assign-permissions-to-create-project)
Registrieren der Azure Migrate-Appliance | Azure Migrate verwendet eine einfache [Azure Migrate-Appliance](migrate-appliance.md), um virtuelle VMware-Computer mit der Azure Migrate-Serverbewertung zu bewerten und die [Migration ohne Agent](server-migrate-overview.md) für die virtuellen VMware-Computer mit der Azure Migrate-Servermigration durchzuführen. Diese Appliance ermittelt VMs und sendet Meta- und Leistungsdaten zu den VMs an Azure Migrate.<br/><br/> Während der Registrierung erstellt Azure Migrate zwei Azure AD-Apps (Active Directory), die die Appliance eindeutig identifizieren. Azure Migrate benötigt Berechtigungen zum Erstellen dieser Apps.<br/><br/> – Die erste App kommuniziert mit Azure Migrate-Dienstendpunkten.<br/><br/> – Die zweite App greift auf eine Azure Key Vault-Instanz zu, die während der Registrierung erstellt wurde, um Azure AD-App-Informationen und Appliancekonfigurationseinstellungen zu speichern. | Einrichtung für [VMware](tutorial-prepare-vmware.md#assign-permissions-to-register-the-appliance), [Hyper-V](tutorial-prepare-hyper-v.md#assign-permissions-to-register-the-appliance) oder [physische Server](tutorial-prepare-physical.md#assign-permissions-to-register-the-appliance)
Erstellen eines Schlüsseltresors für die VMware-Migration ohne Agent | Für die Migration ohne Agent von virtuellen VMware-Computern mithilfe der Azure Migrate-Servermigration erstellt Azure Migrate eine Key Vault-Instanz, um Zugriffsschlüssel für das Replikationsspeicherkonto in Ihrem Abonnement zu verwalten. Für die Tresorerstellung müssen Berechtigungen (Besitzer oder Mitwirkender und Benutzerzugriffsadministrator) für die Ressourcengruppe festgelegt werden, in der sich das Azure Migrate-Projekt befindet. | [Einrichten von Berechtigungen](tutorial-prepare-vmware.md#assign-role-assignment-permissions)

## <a name="supported-geographies"></a>Unterstützte geografischer Regionen

Sie können Azure Migrate-Projekte in verschiedenen geografischen Regionen erstellen. Obwohl Sie Projekte nur in diesen geografischen Regionen erstellen können, haben Sie die Möglichkeit, Computer für andere Zielstandorte zu bewerten und dorthin zu migrieren. Die Projektgeografie wird nur zum Speichern der ermittelten Metadaten verwendet.

**Geografie** | **Speicherort der Metadaten**
--- | ---
Azure Government | US Government, Virginia
Asien-Pazifik | „Asien, Osten“ und „Asien, Südosten“
Australien | „Australien, Osten“ oder „Australien, Südosten“
Brasilien | Brasilien Süd
Canada | „Kanada, Mitte“ oder „Kanada, Osten“
Europa | „Europa, Norden“ oder „Europa, Westen“
Frankreich | Frankreich, Mitte
Indien | „Indien, Mitte“ oder „Indien, Süden“
Japan |  „Japan, Osten“ oder „Japan, Westen“
Korea | „Südkorea, Mitte“ oder „Südkorea, Süden“
United Kingdom | „Vereinigtes Königreich, Süden“ oder „Vereinigtes Königreich, Westen“
USA | „USA, Mitte“ oder „USA, Westen 2“


 > [!NOTE]
 > Der Support für Azure Government ist derzeit nur für die [ältere Version](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) von Azure Migrate verfügbar.



## <a name="vmware-assessment-and-migration"></a>VMware-Bewertung und -Migration

[Überprüfen](migrate-support-matrix-vmware.md) Sie die Azure Migrate-Serverbewertung und Servermigrations-Unterstützungsmatrix für VMware-VMS.

## <a name="hyper-v-assessment-and-migration"></a>Hyper-V-Bewertung und -Migration

[Überprüfen](migrate-support-matrix-hyper-v.md) Sie die Azure Migrate-Serverbewertung und Servermigrations-Unterstützungsmatrix für Hyper-V-VMS.



## <a name="azure-migrate-versions"></a>Azure Migrate-Versionen

Es sind zwei Versionen des Azure Migrate-Diensts verfügbar:

- **Aktuelle Version**: Verwenden Sie diese Version, um neue Azure Migrate-Projekte zu erstellen, lokale Bewertungen durchzuführen und Bewertungen und Migrationen zu orchestrieren. [Weitere Informationen](whats-new.md#release-version-july-2019)
- **Vorherige Version**: Kunden, die die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten ab sofort die aktuelle Version verwenden. In der vorherigen Version können Sie keine neuen Azure Migrate-Projekte erstellen oder neue Ermittlungen durchführen.

## <a name="next-steps"></a>Nächste Schritte

- [Bewerten von virtuellen VMware-Computern](tutorial-assess-vmware.md) für die Migration.
- [Bewerten von virtuellen Hyper-V-Computern](tutorial-assess-hyper-v.md) für die Migration.

