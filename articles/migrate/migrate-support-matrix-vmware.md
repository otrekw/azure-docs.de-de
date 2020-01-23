---
title: Unterstützung der VMware-Bewertung in Azure Migrate
description: Erfahren Sie etwas über die Unterstützung der VMware-Bewertung in Azure Migrate.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 2a9c5504d99f439723a250b619b9f9b660ea9c59
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029005"
---
# <a name="support-matrix-for-vmware-assessment"></a>Unterstützungsmatrix für die VMware-Bewertung 

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Bewertung virtueller VMware-Computer mithilfe von [Azure Migrate: Serverbewertung](migrate-services-overview.md#azure-migrate-server-migration-tool) aus. Informationen zur Migration von virtuellen VMware-Computern zu Azure finden Sie in der [Unterstützungsmatrix für die Migration](migrate-support-matrix-vmware-migration.md).

## <a name="overview"></a>Übersicht

Zum Bewerten von lokalen Computern für die Migration in Azure entsprechend der Beschreibung in diesem Artikel fügen Sie das Tool „Azure Migrate: Serverbewertung“ einem Azure Migrate-Projekt hinzu. Sie stellen die [Azure Migrate-Appliance](migrate-appliance.md) bereit. Die Appliance ermittelt kontinuierlich lokale Computer und sendet Konfigurations- und Leistungsdaten an Azure. Nach der Computerermittlung ordnen Sie die ermittelten Computer in Gruppen und führen eine Bewertung für die Gruppen aus.


## <a name="limitations"></a>Einschränkungen

**Unterstützung** | **Details**
--- | ---
**Bewertungseinschränkungen**| In einem [Projekt](migrate-support-matrix.md#azure-migrate-projects) können bis zu 35.000 virtuelle VMware-Computer ermittelt und bewertet werden.
**Projekteinschränkungen** | Sie können mehrere Projekte in einem Azure-Abonnement erstellen. Ein Projekt kann im Rahmen der Bewertungseinschränkungen virtuelle VMware-Computer, virtuelle Hyper-V-Computer und physische Server umfassen.
**Ermittlung** | Die Azure Migrate-Appliance kann bis zu 10.000 virtuelle VMware-Computer in einer vCenter Server-Instanz ermitteln.
**Bewertung** | Sie können einer einzelnen Gruppe bis zu 35.000 Computer hinzufügen.<br/><br/> Sie können bis zu 35.000 virtuelle Computer in einem einzelnen Vorgang bewerten.

[Hier erfahren Sie mehr](concepts-assessment-calculation.md) über Bewertungen.


## <a name="application-discovery"></a>Anwendungsermittlung

Neben der Ermittlung von Computern können mithilfe von „Azure Migrate: Serverbewertung“ die auf den Computern ausgeführten Anwendungen, Rollen und Funktionen ermittelt werden. Durch das Ermitteln Ihres App-Bestands können Sie einen maßgeschneiderten Migrationspfad für Ihre lokalen Workloads festlegen und planen. 

**Unterstützung** | **Details**
--- | ---
Ermittlung | Die Ermittlung erfolgt ohne Agent über die Gastanmeldeinformationen der Computer, wobei mithilfe von WMI- und SSH-Aufrufen remote auf die Computer zugegriffen wird.
Unterstützte Computer | Lokale virtuelle VMware-Computer
Betriebssystem des Computers | Alle Windows- und Linux-Versionen
Anmeldeinformationen | Unterstützt derzeit die Verwendung von Anmeldeinformationen für alle Windows-Server sowie einen Satz Anmeldeinformationen für alle Linux-Server.<br/><br/> Sie erstellen ein Gastbenutzerkonto für Windows-VMs und ein normales Benutzerkonto (ohne sudo-Zugriff) für alle Linux-VMs.
Einschränkungen | Pro Appliance können bis zu 10.000 Anwendungen ermittelt werden. 

## <a name="vmware-requirements"></a>VMware-Anforderungen

**VMware** | **Details**
--- | ---
**vCenter Server** | Computer, die ermittelt und bewertet werden sollen, müssen mit vCenter Server Version 5.5, 6.0, 6.5 oder 6.7 verwaltet werden.
**Berechtigungen (Bewertung)** | Schreibgeschütztes vCenter Server-Konto
**Berechtigungen (App-Ermittlung)** | vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für „Virtuelle Computer“ > „Gastvorgänge“
**Berechtigungen (Visualisierung von Abhängigkeiten)** | vCenter Server-Konto mit schreibgeschütztem Zugriff und aktivierten Berechtigungen für **Virtuelle Computer** > **Gastvorgänge**


## <a name="azure-migrate-appliance-requirements"></a>Anforderungen für die Azure Migrate-Appliance

In Azure Migrate erfolgt die Ermittlung und Bewertung über die [Azure Migrate-Appliance](migrate-appliance.md). Die Appliance für VMware wird über eine in vCenter Server importierte OVA-Vorlage bereitgestellt. 

- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---vmware) für VMware.
- Erfahren Sie etwas über die [URLs](migrate-appliance.md#url-access), auf die die Appliance Zugriff benötigt.

## <a name="port-access"></a>Portzugriff

**Device** | **Connection**
--- | ---
Appliance | Eingehende Verbindungen an TCP-Port 3389, um Remotedesktopverbindungen mit der Appliance zu ermöglichen<br/><br/> Eingehende Verbindungen an Port 44368, um über Remotezugriff über die URL ```https://<appliance-ip-or-name>:44368``` auf die Applianceverwaltungs-App zugreifen zu können. <br/><br/>Ausgehende Verbindungen an Port 443, 5671 und 5672, um Ermittlungs- und Leistungsmetadaten an Azure Migrate zu senden
vCenter-Server | Eingehende Verbindungen an TCP-Port 443, damit die Appliance Konfigurations- und Leistungsmetadaten für Bewertungen sammeln kann <br/><br/> Die Appliance stellt standardmäßig über Port 443 eine Verbindung mit vCenter her. Wenn der vCenter-Server an einem anderen Port lauscht, können Sie den Port beim Einrichten der Ermittlung ändern.

## <a name="dependency-visualization"></a>Visualisierung von Abhängigkeiten

Mit der Abhängigkeitsvisualisierung können Sie Abhängigkeiten zwischen Computern visualisieren, die Sie bewerten und migrieren möchten. Sie verwenden die Abhängigkeitszuordnung normalerweise, wenn Sie auf Computer mit höheren Zuverlässigkeitsgraden zugreifen möchten. Für VMware-VMs wird die Abhängigkeitsvisualisierung wie folgt unterstützt:

- **Visualisierung von Abhängigkeiten ohne Agent**: Diese Option befindet sich zurzeit in der Vorschau. Sie erfordert keine Installation von Agents auf Computern.
    - Sie erfasst TCP-Verbindungsdaten von den Computern, für die sie aktiviert wurde. Nachdem die Abhängigkeitsermittlung gestartet wurde, sammelt die Appliance Daten von Computern in einem Abrufintervall von fünf Minuten.
    - Die folgenden Daten werden gesammelt:
        - TCP-Verbindungen
        - Namen von Prozessen mit aktiven Verbindungen
        - Namen der installierten Anwendungen, die die obigen Prozesse ausführen
        - Nein. von Verbindungen, die in jedem Abrufintervall erkannt werden
- **Agent-basierte Visualisierung von Abhängigkeiten:** Zur Verwendung der Agent-basierten Visualisierung von Abhängigkeiten müssen Sie die folgenden Agents auf alle lokalen Computer, die Sie analysieren möchten, herunterladen und dort installieren.
    - Installieren Sie Microsoft Monitoring Agent (MMA) auf jedem Computer. [Weitere Informationen](how-to-create-group-machine-dependencies.md#install-the-mma) zum Installieren des MMA-Agent.
    - Installieren Sie Dependency-Agent auf jedem Computer. [Weitere Informationen](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) zum Installieren des Abhängigkeits-Agent.
    - Falls Sie über Computer ohne Internetverbindung verfügen, ist es außerdem erforderlich, auf diesen das Log Analytics-Gateway herunterzuladen und zu installieren.


## <a name="next-steps"></a>Nächste Schritte

- [Bewährte Methoden für die Erstellung von Bewertungen](best-practices-assessment.md)
- [Vorbereiten der Bewertung von virtuellen VMware-Computern](tutorial-prepare-vmware.md)
