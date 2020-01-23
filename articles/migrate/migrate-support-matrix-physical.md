---
title: Unterstützung der Bewertung physischer Server in Azure Migrate
description: Hier finden Sie Informationen zur Unterstützung der Bewertung physischer Server mit Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 32080605217cde78bd648ca6192f73d1025dea4c
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028762"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Unterstützungsmatrix für die Bewertung physischer Server 

Mit dem [Azure Migrate-Dienst](migrate-overview.md) können Sie Computer bewerten und in die Microsoft Azure-Cloud migrieren. Dieser Artikel enthält eine Übersicht über die Unterstützungseinstellungen und -einschränkungen für die Bewertung und Migration von lokalen physischen Servern.


## <a name="overview"></a>Übersicht

Zum Bewerten von lokalen Computern für die Migration in Azure entsprechend der Beschreibung in diesem Artikel fügen Sie das Tool „Azure Migrate: Server Assessment“ (Azure Migrate: Serverbewertung) einem Azure Migrate-Projekt hinzu. Stellen Sie die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance sucht kontinuierlich nach lokalen Computern und sendet Konfigurations- und Leistungsdaten an Azure. Die ermittelten Computer können Sie in Gruppen zusammenfassen und anschließend eine Bewertung für die Gruppen vornehmen.

## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen**| Sie können bis zu 35.000 physische Server in einem einzelnen [Projekt](migrate-support-matrix.md#azure-migrate-projects) ermitteln und bewerten.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen virtuelle VMware-Computer, virtuelle Hyper-V-Computer und physische Server umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 250 physische Server ermitteln.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einer einzelnen Bewertung bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.




## <a name="physical-server-requirements"></a>Anforderungen für physische Server

| **Unterstützung**                | **Details**               
| :-------------------       | :------------------- |
| **Physische Serverbereitstellung**       | Der physische Server kann eigenständig sein oder in einem Cluster bereitgestellt werden. |
| **Berechtigungen**           | **Windows:** Richten Sie auf allen Windows-Servern, die Sie in die Ermittlung einbeziehen möchten, ein lokales Benutzerkonto ein. Das Benutzerkonto muss den folgenden Gruppen hinzugefügt werden: Remotedesktopbenutzer, Leistungsüberwachungsbenutzer und Leistungsprotokollbenutzer. <br/> **Linux:** Sie benötigen ein root-Konto auf den Linux-Servern, die Sie ermitteln möchten. |
| **Betriebssystem** | Alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)-Betriebssysteme mit Ausnahme der folgenden werden unterstützt:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Die Appliance für physische Server kann auf einem virtuellen Computer oder auf einem physischen Computer ausgeführt werden. Die Einrichtung erfolgt mithilfe eines PowerShell-Skripts aus dem Azure-Portal.

- Informationen zu den Applianceanforderungen für physische Server finden Sie [hier](migrate-appliance.md#appliance---physical).
- Informationen zu URLs, auf die die Appliance Zugriff benötigt, finden Sie [hier](migrate-appliance.md#url-access).

## <a name="port-access"></a>Portzugriff

Die folgende Tabelle fasst die Portanforderungen für die Bewertung zusammen.

**Device** | **Connection**
--- | ---
**Appliance** | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ``` https://<appliance-ip-or-name>:44368 ``` auf die Applianceverwaltungs-App zugreifen zu können.<br/> Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
**Physische Server** | **Windows:** Eingehende Verbindungen auf den Ports 443, den WinRM-Ports 5985 (HTTP) und 5986 (HTTPS) zum Abrufen von Konfigurations- und Leistungsmetadaten von Windows-Servern. <br/> **Linux:**  Eingehende Verbindungen auf dem Port 22 (UDP) zum Abrufen von Konfigurations- und Leistungsmetadaten von Linux-Servern. |


## <a name="next-steps"></a>Nächste Schritte

[Vorbereiten auf die Bewertung und Migration physischer Server zu Azure](tutorial-prepare-physical.md)
