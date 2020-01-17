---
title: Informationen zu Azure Migrate
description: Erfahren Sie mehr über den Azure Migrate-Dienst.
ms.topic: overview
ms.date: 12/29/2019
ms.custom: mvc
ms.openlocfilehash: c336b2f95a17896edcc7eaad2b2586c6859804f9
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772192"
---
# <a name="about-azure-migrate"></a>Informationen zu Azure Migrate

Dieser Artikel enthält eine kurze Übersicht über den Azure Migrate-Dienst.

Verwenden Sie Azure Migrate für die Migration zu Azure. Azure Migrate bietet mit den folgenden Features einen zentralisierten Hub zur Bewertung und Migration von lokaler Infrastruktur, Anwendungen und Daten zu Azure:

- **Vereinheitlichte Migrationsplattform**: Ein einzelnes Portal zum Starten, Ausführen und Nachverfolgen Ihres Migrationswegs zu Azure.
- **Verfügbare Tools**: Eine Reihe von Tools für Bewertung und Migration. Der Hub umfasst Azure Migrate: Serverbewertung und Azure Migrate: Servermigration. Die Integration in andere Azure-Dienste und andere Tools und Angebote unabhängiger Softwareanbieter (Independent Software Vendor, ISV) ist möglich.
- **Bewertung und Migration**: Im Azure Migrate-Hub können Sie Folgendes bewerten und migrieren:
    - **Server**: Hiermit werden lokale Server bewertet und zu Azure-VMs migriert.
    - **Datenbanken**: Hiermit werden lokale Datenbanken bewertet und zu Azure SQL-Datenbank oder zur verwalteten Azure SQL-Instanz migriert.
    - **Webanwendungen**: Hiermit werden lokale Webanwendungen mit dem Assistenten von Azure App Service bewertet und zu Azure App Service migriert.
    - **Virtuelle Desktops**: Hiermit wird die lokale Virtual Desktop Infrastructure (VDI) in Azure bewertet und zu Windows Virtual Desktop migriert.
    - **Data:** Große Datenmengen werden mit Azure Data Box-Produkten schnell und kostengünstig zu Azure migriert. 


## <a name="integrated-tools"></a>Integrierte Tools

Der Azure Migrate-Hub bietet die folgenden Tools.

**Tool** | **Bewerten/Migrieren** | **Details**
--- | --- | ---
**Azure Migrate: Serverbewertung** | Bewerten von Servern | Lokale VMware-VMs, Hyper-V-VMs und physische Server werden ermittelt und bewertet, um festzustellen, ob sie für die Migration zu Azure bereit sind.
**Azure Migrate: Servermigration** | Migrieren von Servern | VMware-VMs, Hyper-V-VMs, physische Server sowie andere virtualisierte Computer und VMs der öffentlichen Cloud werden zu Azure migriert. 
**Datenmigrations-Assistent (DMA)** | Lokale SQL Server-Datenbanken werden für die Migration zu Azure SQL-Datenbank, in die verwaltete Azure SQL-Instanz oder zu Azure-VMs, auf denen SQL Server ausgeführt wird, bewertet. | Der DMA stellt Informationen zu möglichen Blockierungsproblemen bei der Migration zur Verfügung. Er identifiziert nicht unterstützte Features sowie neue Features, von denen Sie nach der Migration profitieren können, und hilft Ihnen bei der Identifizierung des richtigen Pfads für die Datenbankmigration. [Weitere Informationen](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017)
**Database Migration Service (DMS)** | Lokale Datenbanken werden zu Azure-VMs migriert, auf denen SQL, Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen ausgeführt werden. | [Informieren Sie sich](https://docs.microsoft.com/azure/dms/dms-overview) über DMS.
**Movere** | Bewerten von Servern | Weitere Informationen zu Movere finden Sie [hier](#movere).
**Migrations-Assistent für Web-Apps** | Lokale Web-Apps werden bewertet und zu Azure migriert. |  Verwenden Sie den Migrations-Assistenten von Azure App Service, um lokale Websites für die Migration zu Azure App Service zu bewerten.<br/><br/> Migrieren Sie .NET- und PHP-Web-Apps mithilfe des Migrations-Assistenten von Azure App Service zu Azure. [Weitere Informationen](https://appmigration.microsoft.com/) zum Assistenten.
**Azure Data Box** | Offlinedatenmigration. | Verschieben Sie große Datenmengen mit Azure Data Box-Produkten offline nach Azure. [Weitere Informationen](https://docs.microsoft.com/azure/databox/)

## <a name="isv-integration"></a>ISV-Integration

Azure Migrate kann in eine Reihe von ISV-Angeboten integriert werden. 

**ISV** | **Feature**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Migrieren von Servern
[Cloudamize](https://www.cloudamize.com/platform) | Bewerten von Servern
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Bewerten und Migrieren von Servern
[Device42](https://docs.device42.com/) | Bewerten von Servern
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | Bewerten der VDI
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Migrieren von Servern
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Bewerten von Servern
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Bewerten von Servern und Datenbanken


## <a name="azure-migrate-server-assessment-tool"></a>Von der Azure Serverbewertungstool

Azure Migrate: Das Serverbewertungstool ermittelt und bewertet lokale VMware-VMs, Hyper-V-VMs und physische Server für die Migration zu Azure. Sie hilft Ihnen, Folgendes zu identifizieren:

- **Azure-Bereitschaft:** Bewertet, ob lokale Computer bereit für die Migration zu Azure sind.
- **Azure-Dimensionierung:** Die geschätzte Größe von Azure-VMs nach der Migration.
- **Azure-Kostenschätzung:** Geschätzte Kosten für die Ausführung lokaler Server in Azure.
- **Abhängigkeitsvisualisierung:** Wenn Sie die Serverbewertung mit Abhängigkeitsvisualisierung verwenden, können Sie effektiv serverübergreifende Abhängigkeiten und optimale Möglichkeiten zum Verschieben abhängiger Server zu Azure ermitteln.

Die Serverbewertung verwendet eine schlanke Appliance, die Sie lokal bereitstellen und bei der Serverbewertung registrieren.

- Die Appliance wird auf einem physischen Server oder virtuellen Computer ausgeführt und kann problemlos mithilfe einer heruntergeladenen Vorlage installiert werden.
- Die Appliance ermittelt lokale Computer und sendet kontinuierlich Computermetadaten und Leistungsdaten an Azure Migrate.
- Die Applianceermittlung erfolgt ohne Agent. Auf ermittelten Computern muss nichts installiert werden.
- Nach der Ermittlung erfassen Sie ermittelte Computer in Gruppen und bewerten Gruppen für die Migration.


## <a name="azure-migrate-server-migration-tool"></a>Von der Azure Tool für die Servermigration

Azure Migrate: Mit dem Servermigrationstool können Sie lokale VMware-VMs, Hyper-V-VMs, physische Server, andere virtualisierte Computer und VMs der öffentlichen Cloud zu Azure migrieren. Computer können nach der Bewertung oder ohne Bewertung migriert werden.


## <a name="select-a-tool"></a>Auswählen eines Tools

Wählen Sie im Azure Migrate-Hub das Tool aus, das Sie für die Bewertung verwenden möchten, und fügen Sie es einem Azure Migrate-Projekt hinzu. Falls Sie ein ISV-Tool oder Movere hinzufügen, gehen Sie wie folgt vor:

- Beziehen Sie zunächst eine Lizenz, oder registrieren Sie sich für eine kostenlose Testversion (gemäß der Toolanweisungen). Die Lizenzierung von Tools wird durch das ISV-Tool bestimmt. 
- In jedem Tool ist eine Option zum Herstellen einer Verbindung mit Azure Migrate vorhanden. Befolgen Sie die Anweisungen, um die Verbindung herzustellen.
- Verfolgen Sie Ihren Migrationsweg aus dem Azure Migrate-Projekt über alle Azure- und ISV-Tools nach.


## <a name="movere"></a>Movere

Bei Movere handelt es sich um eine SaaS-Plattform, die durch die präzise Darstellung vollständiger IT-Umgebungen innerhalb eines Tages zur Verbesserung der Business Intelligence beiträgt. Organisationen wachsen, verändern sich und werden digital optimiert. Mit dieser Lösung können Unternehmen darauf vertrauen, dass sie über die nötige Transparenz und Kontrolle für Ihre Umgebungen verfügen – unabhängig von Plattform, Anwendung oder Geografie. Movere wurde von Microsoft [übernommen](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) und wird nicht mehr als eigenständiges Angebot verkauft.  Movere steht im Rahmen der Microsoft-Programme für Lösungsbewertungen und Wirtschaftlichkeit in der Cloud zur Verfügung. Weitere Informationen zu Movere finden Sie [hier](https://www.movere.io). Sollten Sie Fragen haben, senden Sie diese entweder an movereq@microsoft.com, oder wenden Sie sich an Ihren Microsoft-Vertreter.

Informieren Sie sich auch über Azure Migrate, unseren integrierten Migrationsdienst. Azure Migrate bietet einen zentralen Hub, um die Migration zur Cloud zu vereinfachen. Der Hub zeichnet sich durch umfassende Unterstützung verschiedenster Workloads aus. Hierzu zählen unter anderem physische und virtuelle Server sowie Datenbanken und Anwendungen. Die End-to-End-Transparenz ermöglicht eine mühelose Nachverfolgung des Status während der Ermittlung, Bewertung und Migration. Dank der integrierten Azure- und Partner-ISV-Tools bietet Azure Migrate auch ein breites Spektrum an Features wie etwa die Ermittlung virtueller und physischer Server, eine leistungsbasierte Größenanpassung, eine Kostenplanung, importbasierte Bewertungen sowie eine Anwendungsabhängigkeitsanalyse ohne Agent. Sollten Sie professionelle Hilfe bei Ihren ersten Schritten benötigen: Microsoft verfügt über kompetente [Azure Expert Managed Service Provider](https://azure.microsoft.com/partners), die Sie auf Ihrem Weg begleiten. Besuchen Sie die [Azure Migrate-Website](https://azure.microsoft.com/services/azure-migrate/). 
 

## <a name="azure-migrate-versions"></a>Azure Migrate-Versionen

Es sind zwei Versionen des Azure Migrate-Diensts verfügbar:

- **Aktuelle Version**: Verwenden Sie diese Version, um Azure Migrate-Projekte zu erstellen, lokale Computer zu ermitteln und Bewertungen und Migrationen zu orchestrieren. [Erfahren Sie mehr](whats-new.md) über die Neuerungen in dieser Version.
- **Vorherige Version**: Wenn Sie die Vorgängerversion von Azure Migrate verwendet haben (es wird nur die Bewertung von lokalen VMware-VMs unterstützt), sollten Sie ab sofort die aktuelle Version verwenden. Azure Migrate-Projekte können nicht mehr mit der vorherigen Version erstellt werden, und es wird empfohlen, keine neuen Ermittlungen auszuführen. Wenn Sie auf bereits vorhandene Projekte zugreifen möchten, suchen Sie im Azure-Portal nach **Azure Migrate**, und wählen Sie die entsprechende Option aus. Auf dem Dashboard **Azure Migrate** finden Sie eine Benachrichtigung und einen Link für den Zugriff auf alte Azure Migrate-Projekte.



## <a name="next-steps"></a>Nächste Schritte

- Probieren Sie unsere Tutorials zum Bewerten von [VMware-VMs](tutorial-prepare-vmware.md), [Hyper-V-VMs](tutorial-prepare-hyper-v.md) und [physischen Servern](tutorial-prepare-physical.md) aus.
- Sehen Sie sich die [häufig gestellten Fragen](resources-faq.md) zu Azure Migrate an.
