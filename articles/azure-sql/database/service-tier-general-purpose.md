---
title: Universelle Dienstebene
titleSuffix: Azure SQL Database & Azure SQL Managed Instance
description: Hier erhalten Sie Informationen zur Dienstebene „Universell“ für Azure SQL-Datenbank und Azure SQL Managed Instance.
services: sql-database
ms.service: sql-db-mi
ms.subservice: features
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/07/2019
ms.openlocfilehash: ee218253309995e721c97f4a7f7b4547b32f7c36
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2020
ms.locfileid: "85986640"
---
# <a name="general-purpose-service-tier---azure-sql-database-and-azure-sql-managed-instance"></a>Dienstebene „Universell“ – Azure SQL-Datenbank und Azure SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

> [!NOTE]
> Die Dienstebene „Universell“ im vCore-basierten Kaufmodell wird im DTU-basierten Kaufmodell als Dienstebene „Standard“ bezeichnet. Einen Vergleich zwischen dem vCore-basierten Kaufmodell und dem DTU-basierten Kaufmodell finden Sie unter [Wahl zwischen dem vCore-basierten und dem DTU-basierten Kaufmodell – Azure SQL-Datenbank und SQL Managed Instance](purchasing-models.md).

Azure SQL-Datenbank und Azure SQL Managed Instance basieren auf der Architektur der SQL Server-Datenbank-Engine, die an die Cloudumgebung angepasst ist, um auch bei Infrastrukturfehlern eine Verfügbarkeit von 99,99 % sicherzustellen. 

Es gibt zwei Dienstebenen für Azure SQL-Datenbank und SQL Managed Instance: 

- Universell
- Unternehmenskritisch

Azure SQL-Datenbank verfügt noch über eine dritte Dienstebene, die derzeit nicht für Azure SQL Managed Instance verfügbar ist:

- Hyperscale

Das Architekturmodell für die Dienstebene „Universell“ basiert auf der Trennung von Compute- und Speicherebene. Dieses Architekturmodell beruht auf der Hochverfügbarkeit und Zuverlässigkeit von Azure Blob Storage mit transparenter Replikation von Datenbankdateien und garantiert ohne Datenverlust bei Ausfall der zugrunde liegenden Infrastruktur.

In der folgenden Abbildung sind vier Knoten im Architekturmodell des Typs „Standard“ mit getrennter Compute- und Speicherebene dargestellt.

![Trennung der Compute- und Speicherebene](./media/service-tier-general-purpose/general-purpose-service-tier.png)

Das Architekturmodell für die Dienstebene „Universell“ umfasst zwei Ebenen:

- Eine zustandslose Computeebene, auf der der Prozess `sqlservr.exe` ausgeführt wird und die nur temporäre und zwischengespeicherte Daten (z.B. Plancache, Pufferpool, Spaltenspeicherpool) enthält. Dieser zustandslose Knoten wird von der Azure Service Fabric-Plattform gesteuert, die Prozesse initialisiert, die Integrität des Knotens überprüft und bei Bedarf ein Failover an einen anderen Ort durchführt.
- Eine zustandsbehaftete Datenebene mit Datenbankdateien (MDF- und LDF-Dateien), die in Azure Blob Storage gespeichert sind. Azure Blob Storage garantiert die Vermeidung von Datenverlusten jeglicher Datensätze, die in Datenbankdateien platziert werden. Azure Storage verfügt über integrierte Datenverfügbarkeit und -redundanz, die sicherstellen, dass jeder Eintrag in einer Protokolldatei und jede Seite in einer Datendatei erhalten bleibt, auch wenn der Prozess abstürzt.

Bei einem Upgrade der Datenbank-Engine oder des Betriebssystems, einem Ausfall eines Teils der zugrunde liegenden Infrastruktur oder wenn im `sqlservr.exe`-Prozess ein schwerwiegendes Problem erkannt wird, verschiebt Azure Service Fabric den zustandslosen Prozess auf einen anderen zustandslosen Serverknoten. Es sind mehrere Reserveknoten vorhanden, auf denen im Fall eines Failovers des primären Knotens ein neuer Computedienst ausgeführt werden kann, um die Failoverzeit zu minimieren. Daten auf der Azure Storage-Ebene sind nicht betroffen, und Daten- und Protokolldateien werden an den neu initialisierten Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %, hat jedoch möglicherweise Auswirkungen auf die Leistung bei der Ausführung von hohen Arbeitsauslastungen. Dies ist auf die Übergangszeit zurückzuführen sowie auf die Tatsache, dass der neue Knoten mit „kaltem“ Cache gestartet wird.

## <a name="when-to-choose-this-service-tier"></a>Wann sollte diese Dienstebene gewählt werden?

Die Dienstebene „Universell“ ist eine Standarddienstebene in Azure SQL-Datenbank und Azure SQL Managed Instance, die für die meisten generischen Workloads konzipiert ist. Wenn Sie eine vollständig verwaltete Datenbank-Engine mit einer 99,99%-SLA mit einer Speicherlatenz von 5 bis 10 ms benötigen, was in den meisten Fällen auf SQL Server auf einem virtuellen Azure-Computer (Virtual Machine, VM) zutrifft, ist der Tarif „Universell“ für Sie die richtige Wahl.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Ressourcenmerkmale (Anzahl von Kernen, E/A, Arbeitsspeicher) der Ebene „Universell“ bzw. „Standard“ für [SQL Managed Instance](../managed-instance/resource-limits.md#service-tier-characteristics), für eine Einzeldatenbank im [vCore-basierten Modell](resource-limits-vcore-single-databases.md#general-purpose---provisioned-compute---gen4) oder im [DTU-Modell](resource-limits-dtu-single-databases.md#single-database-storage-sizes-and-compute-sizes) oder für einen Pool für elastische Datenbanken im [vCore-basierten Modell](resource-limits-vcore-elastic-pools.md#general-purpose---provisioned-compute---gen4) und im [DTU-Modell](resource-limits-dtu-elastic-pools.md#standard-elastic-pool-limits) an.
- Informationen zu den Tarifen [Unternehmenskritisch](service-tier-business-critical.md) und [Hyperscale](service-tier-hyperscale.md)
- Informationen zu [Service Fabric](../../service-fabric/service-fabric-overview.md)
- Weitere Optionen zu Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](business-continuity-high-availability-disaster-recover-hadr-overview.md).
