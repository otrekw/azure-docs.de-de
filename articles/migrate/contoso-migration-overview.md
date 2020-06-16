---
title: Contoso-Migrationsserie | Microsoft-Dokumentation
description: In diesem Artikel werden Links zu Contoso-Beispielszenarien für die Migration zu Azure bereitgestellt.
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: raynew
ms.openlocfilehash: 2cc03462fd72db1a50434d3d6de0851660414cd9
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84195853"
---
# <a name="contoso-migration-series"></a>Contoso-Migrationsserie


In verschiedenen Artikeln wird veranschaulicht, wie die fiktive Organisation Contoso ihre lokale Infrastruktur zur [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/)-Cloud migriert. 

Die bereitgestellten Szenarien veranschaulichen, wie eine Infrastrukturmigration eingerichtet wird und wie verschiedene Arten von Migrationen ausgeführt werden. Die Komplexität der Szenarien erhöht sich hierbei immer mehr. Die Artikel zeigen, wie das Unternehmen Contoso die Migration handhabt, es werden aber auch allgemeine Anweisungen und Hinweise gegeben.

## <a name="migration-articles"></a>Artikel zur Migration

Die Artikel der Reihe werden in der folgenden Tabelle zusammengefasst.  

- Die einzelnen Migrationsszenarien basieren auf geringfügig variierten Geschäftsanforderungen, die die Migrationsstrategie bestimmen.
- Für die einzelnen Bereitstellungsszenarien werden Informationen zu geschäftlichen Faktoren und Zielen, eine vorgeschlagene Architektur, Schritte zum Ausführen der Migration sowie Empfehlungen zur Bereinigung und die nächsten Schritten nach Abschluss der Migration bereitgestellt.


**Artikel** | **Details** 
--- | --- 
[Artikel 1: Übersicht](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Dies ist eine Übersicht über die Artikelreihe, die Migrationsstrategie von Contoso und die in der Reihe verwendeten Beispiel-Apps. 
[Artikel 2: Bereitstellen einer Azure-Infrastruktur](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso bereitet seine lokale Infrastruktur und die Azure-Infrastruktur für die Migration vor. Für alle Artikel der Reihe wird dieselbe Infrastruktur verwendet. 
[Artikel 3: Bewerten der lokalen Ressourcen für die Migration zu Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso führt eine Bewertung seiner lokalen App SmartHotel360 durch, die in VMware ausgeführt wird. Contoso bewertet virtuelle Computer der App mit dem Azure Migrate-Dienst und die SQL Server-Datenbank der App mit dem Datenmigrations-Assistenten.
[Artikel 4: Zuweisen eines neuen Hosts für eine lokale App auf einer Azure-VM und in SQL Managed Instance](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso führt für seine lokale App SmartHotel360 eine Migration per Lift & Shift zu Azure aus. Contoso migriert die Front-End-VM der App mithilfe von [Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-services-overview). Contoso migriert die App-Datenbank mit [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) zu SQL Managed Instance.
[Artikel 5: Zuweisen eines neuen Hosts für eine App auf Azure-VMs](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso migriert die VMs der App SmartHotel360 mithilfe des Azure Migrate-Diensts zu Azure-VMs. 
[Artikel 6: Zuweisen eines neuen Hosts für eine App auf Azure-VMs und in einer SQL Server Always On-Verfügbarkeitsgruppe](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) | Contoso migriert die App SmartHotel360. Contoso verwendet Azure Migrate, um die App-VMs zu migrieren. Der Database Migration Service wird verwendet, um die App-Datenbank zu einem SQL Server-Cluster zu migrieren, der mit einer Always On-Verfügbarkeitsgruppe geschützt ist. 
[Artikel 7: Rehosten einer Linux-App auf Azure-VMs](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso migriert mithilfe von Azure Migrate seine Linux-App osTicket per Lift & Shift zu Azure-VMs.
[Artikel 8: Rehosten einer Linux-App auf Azure-VMs und Azure Database for MySQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso migriert seine Linux-App osTicket mithilfe von Azure Migrate zu Azure-VMs. Die App-Datenbank wird mit Azure Database Migration Service zu Azure-Datenbank für MySQL migriert (umfasst eine alternative Option zur Verwendung von MySQL Workbench).
[Artikel 9: Umgestalten einer App in eine Azure-Web-App und in Azure SQL-Datenbank](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso migriert die SmartHotel360-App zu einer Azure-Web-App und die App-Datenbank mit Azure Database Migration Service zu Azure SQL-Datenbank.
[Artikel 10: Umgestalten einer Windows-App mithilfe von Azure App Services und einer verwalteten SQL-Instanz](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql-managed-instance) | Contoso migriert eine lokale Windows-basierte App in eine Azure-Web-App und die App-Datenbank mit Azure Database Migration Service in eine verwaltete Azure SQL-Instanz.
[Artikel 11: Umgestalten einer Linux-App in eine Azure-Web-App und in Azure Database for MySQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso migriert die Linux-App osTicket mithilfe von Azure Traffic Manager zu einer Azure-Web-App in mehreren Azure-Regionen. Zur Sicherstellung der Continuous Delivery erfolgt eine Integration in GitHub. Contoso migriert die App-Datenbank zu einer Azure Database for MySQL-Instanz. 
[Artikel 12: Umgestalten von Team Foundation Server in Azure DevOps Services](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso migriert die lokale Team Foundation Server-Bereitstellung zu Azure DevOps Services in Azure.
[Artikel 13: Neuerstellen einer App in Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso erstellt die SmartHotel-App mit verschiedenen Azure-Funktionen und -Diensten neu – z. B. Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services und Azure Cosmos DB.
[Artikel 14: Skalieren einer Migration zu Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Nachdem Contoso verschiedene Kombinationen für die Migration getestet hat, bereitet das Unternehmen sich jetzt auf eine vollständige Migration nach Azure in großem Umfang vor.



## <a name="next-steps"></a>Nächste Schritte

- [Informationen zur Cloudmigration](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/)
- Informationen zu Migrationsstrategien für andere Szenarien (Quell-/Zielpaare) finden Sie im [Datenbankmigrationsleitfaden](https://datamigration.microsoft.com/).
