---
title: Neuerungen in Azure Migrate
description: Erfahren Sie mehr über die Neuerungen und aktuellen Updates des Azure Migrate-Diensts.
ms.topic: overview
ms.date: 04/19/2020
ms.custom: mvc
ms.openlocfilehash: a46519d434061ebfdedbabe70a3e044cb69ca552
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91322222"
---
# <a name="whats-new-in-azure-migrate"></a>Neuerungen in Azure Migrate

[Azure Migrate](migrate-services-overview.md) unterstützt Sie beim Ermitteln, Bewerten und Migrieren von lokalen Servern, Apps und Daten in die Microsoft Azure Cloud. In diesem Artikel werden die neuen Releases und Features in Azure Migrate zusammengefasst.
## <a name="update-september-2020"></a>Update (September 2020)
- Mit Azure Migrate können Sie jetzt Server in Verfügbarkeitszonen migrieren.
- Mit Azure Migrate können Sie nun UEFI-basierte virtuelle Computer und physische Server zu Azure-VMs der Generation 2 migrieren. 

## <a name="update-august-2020"></a>Update (August 2020)

- Verbessertes Onboardingverfahren, bei dem ein Azure Migrate-Projektschlüssel über das Portal erstellt und zum Abschließen der Applianceregistrierung verwendet wird
- Option zum Herunterladen von OVA-/VHD-Dateien oder der Installationsskripts aus dem Portal, um die VMware- bzw. Hyper-V-Appliances einzurichten
- Aktualisierung des Appliance-Konfigurations-Managers mit erweiterter Benutzeroberfläche
- Unterstützung mehrerer Anmeldeinformationen für die Ermittlung von Hyper-V-VMs
- Verbesserte Such-, Sortier- und Filterfunktionen für hinzugefügte Anmeldeinformationen und Ermittlungsquellen
- Optionen für die Eingabe einzelner und mehrerer Elemente sowie für den Import von CSV-Dateien, damit Benutzer Ermittlungsquellen für Hyper-V-Hosts/-Cluster und physische Server hinzufügen können
- Erweiterte Oberfläche für Fehler mit Statusaktualisierungen für Validierungs- und Ermittlungsvorgänge für jede hinzugefügte Quelle in der Tabelle 

## <a name="update-june-2020"></a>Update (Juni 2020)

- Bewertungen für die Migration von lokalen VMware-VMs zu [Azure VMware Solution (AVS)](https://go.microsoft.com/fwlink/?linkid=2132637) werden jetzt unterstützt. [Weitere Informationen](how-to-create-azure-vmware-solution-assessment.md)
- Unterstützung mehrerer Anmeldeinformationen für die Appliance zur Erkennung physischer Server
- Unterstützung für die Azure-Anmeldung von der Appliance für Mandanten, für die eine Mandanteneinschränkung konfiguriert wurde


## <a name="update-april-2020"></a>Update (April 2020)

Azure Migrate unterstützt Bereitstellungen in Azure Government. 

- Sie können VMware-VMs, Hyper-V-VMs und physische Server ermitteln und bewerten.
- Sie können VMware-VMs, Hyper-V-VMs und physische Server zu Azure migrieren.
- Für VMware können Sie die Migration mit oder ohne Agent verwenden. [Weitere Informationen](server-migrate-overview.md)
- Überprüfen Sie die [unterstützten geografischen Bereiche und Regionen](migrate-support-matrix.md#supported-geographies-azure-government) für Azure Government.
- Die [Agent-basierte Abhängigkeitsanalyse](concepts-dependency-visualization.md#agent-based-analysis) wird in Azure Government nicht unterstützt.
- Funktionen in der Vorschau werden in Azure Government unterstützt, insbesondere die [Abhängigkeitsanalyse ohne Agent](concepts-dependency-visualization.md#agentless-analysis) und die [Anwendungsermittlung](how-to-discover-applications.md).


## <a name="update-march-2020"></a>Update (März 2020)

Es ist jetzt eine skriptbasierte Installation zum Einrichten der [Azure Migrate-Appliance](migrate-appliance.md) verfügbar:

- Die skriptbasierte Installation ist eine Alternative zur .OVA- (VMware) bzw. VHD-Installation (Hyper-V) der Appliance.
- Hierbei wird ein PowerShell-Installationsskript genutzt, um die Appliance für VMware/Hyper-V auf einem vorhandenen Computer mit Windows Server 2016 einzurichten.

## <a name="update-november-2019"></a>Update (November 2019)

Azure Migrate wurden einige neue Features hinzugefügt:

- **Bewertung physischer Server**: Zusätzlich zur bereits vorhandenen Migration physischer Server wird jetzt auch die Bewertung von lokalen physischen Servern unterstützt.
- **Importbasierte Bewertung**. Die Bewertung von Computern, für die Metadaten und Leistungsdaten aus einer CSV-Datei verwendet werden, wird jetzt unterstützt.
- **Anwendungsermittlung**: Azure Migrate unterstützt über die Azure Migrate-Appliance jetzt die Ermittlung von Apps, Rollen und Features auf Anwendungsebene. Dies wird derzeit nur für VMware-VMs unterstützt und ist auf die Ermittlung beschränkt (die Bewertung wird derzeit nicht unterstützt). [Weitere Informationen](how-to-discover-applications.md)
- **Visualisierung von Abhängigkeiten ohne Agent**: Es ist nicht mehr erforderlich, Agents explizit für die Abhängigkeitsvisualisierung zu installieren. Sowohl die Vorgehensweise ohne als auch mit Agent wird jetzt unterstützt.
- **Virtueller Desktop**: Verwenden Sie ISV-Tools, um die lokale Virtual Desktop Infrastructure (VDI) zu bewerten und zu Windows Virtual Desktop in Azure zu migrieren.
- **Web-App**: Der Azure App Service-Migrations-Assistent, der für die Bewertung und Migration von Web-Apps verwendet wird, ist jetzt in Azure Migrate integriert.

Azure Migrate wurden neue Tools zur Bewertung und Migration hinzugefügt:

- **Rackware:** für die Cloudmigration
- **Movere:** für die Bewertung

[Hier](migrate-services-overview.md) finden Sie weitere Informationen zur Verwendung von Tools und ISV-Angeboten für die Bewertung und Migration in Azure Migrate.

## <a name="azure-migrate-current-version"></a>Aktuelle Azure Migrate-Version

Die aktuelle Version von Azure Migrate (veröffentlicht im Juli 2019) enthält eine Reihe neuer Features:

- **Vereinheitlichte Migrationsplattform**: Azure Migrate bietet jetzt ein einziges Portal zum Zentralisieren, Verwalten und Nachverfolgen Ihres Migrationswegs zu Azure mit einem verbesserten Bereitstellungsfluss und einer optimierten Portalbenutzeroberfläche.
- **Bewertungs- und Migrationstools**: Azure Migrate bietet native Tools und lässt sich in andere Azure-Dienste sowie ISV-Tools (Independent Software Vendor) integrieren. [Erfahren Sie mehr](migrate-services-overview.md#isv-integration) über die ISV-Integration.
- **Azure Migrate-Bewertung**: Mit dem Azure Migrate-Serverbewertungstool können Sie lokale VMware-VMs und Hyper-V-VMs für die Migration zu Azure ermitteln und bewerten. Sie können die Migration auch mithilfe anderer Azure-Dienste und ISV-Tools bewerten.
- **Azure Migrate-Migration**: Mit dem Azure Migrate-Servermigrationstool können Sie lokale VMware-VMs, Hyper-V-VMs, physische Server, andere virtualisierte Server und VMs der privaten/öffentlichen Cloud zu Azure migrieren. Außerdem können Sie die Migration zu Azure mithilfe von ISV-Tools ausführen.
- **Azure Migrate-Appliance**: Azure Migrate stellt eine schlanke Appliance für die Ermittlung und Bewertung von lokalen VMware-VMs und Hyper-V-VMs bereit.
    - Diese Appliance wird von der Azure Migrate-Serverbewertung und Azure Migrate-Servermigration für die Migration ohne Agent verwendet.
    - Die Appliance ermittelt kontinuierlich Servermetadaten und Leistungsdaten für die Zwecke der Bewertung und Migration.  
- **VMware-VM-Migration**:  Azure Migrate-Servermigration bietet eine Reihe von Methoden zum Migrieren von lokalen VMware-VMs zu Azure.  Eine Migration ohne Agent mithilfe der Azure Migrate-Appliance und eine agentbasierte Migration, bei der eine Replikationsappliance verwendet und auf jedem virtuellen Computer ein Agent bereitgestellt wird, den Sie migrieren möchten. [Weitere Informationen](server-migrate-overview.md)
 - **Datenbankbewertung und -migration**: Aus Azure Migrate können Sie lokale Datenbanken für die Migration zu Azure mithilfe des Migrations-Assistenten der Azure-Datenbank bewerten. Sie können Datenbanken mit dem Azure Database Migration Service migrieren.
- **Web-App-Migration**: Sie können Web-Apps mit einer öffentlichen Endpunkt-URL mit Azure App Service bewerten. Für die Migration interner .NET-Apps können Sie den Migrations-Assistenten von App Service herunterladen und ausführen.
- **Data Box**: Importieren Sie große Mengen von Offlinedaten in Azure mithilfe von Azure Data Box in Azure Migrate.

## <a name="azure-migrate-previous-version"></a>Vorherige Azure Migrate-Version

Wenn Sie zurzeit die Vorgängerversion von Azure Migrate verwenden (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten Sie ab sofort die aktuelle Version verwenden. In der vorherigen Version können Sie keine neuen Azure Migrate Projekte mehr erstellen oder neue Ermittlungen durchführen. Sie können weiterhin auf vorhandene Projekte zugreifen. Wählen Sie zu diesem Zweck im Azure-Portal die Option **Alle Dienste** aus, und suchen Sie dann nach **Azure Migrate**. In den Azure Migrate-Benachrichtigungen finden Sie einen Link für den Zugriff auf alte Azure Migrate-Projekte.



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den Preisen von Azure Migrate finden Sie [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).
- Sehen Sie sich die [häufig gestellten Fragen](resources-faq.md) zu Azure Migrate an.
- Testen Sie unsere Tutorials, um [VMware-VMs](tutorial-assess-vmware.md) und [Hyper-V-VMs](tutorial-assess-hyper-v.md) zu bewerten.
