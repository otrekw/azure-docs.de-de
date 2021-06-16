---
title: Glossarbegriffe
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Ein Glossar mit Begriffen zur Arbeit mit Azure SQL-Datenbank, Azure SQL Managed Instance und SQL auf Azure-VMs.
services: sql-database
ms.service: sql-database
ms.subservice: service-overview
ms.custom: sqldbrb=4
ms.devlang: ''
ms.topic: reference
author: MashaMSFT
ms.author: mathoma
ms.reviewer: ''
ms.date: 5/18/2021
ms.openlocfilehash: 42e701a53502ce024118686f9835ddfdbc7543b6
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111413509"
---
# <a name="azure-sql-database-glossary-of-terms"></a>Glossar mit Begriffen aus Azure SQL-Datenbank
[!INCLUDE[appliesto-sqldb-sqlmi](./includes/appliesto-sqldb-sqlmi.md)]

## <a name="azure-sql-database"></a>Azure SQL-Datenbank

|Kontext|Begriff|Weitere Informationen|
|:---|:---|:---|
|Azure-Dienst|Azure SQL-Datenbank oder SQL-Datenbank|[Azure SQL-Datenbank](database/sql-database-paas-overview.md)|
|Kaufmodell|DTU-basiertes Kaufmodell|[DTU-basiertes Kaufmodell](database/service-tiers-dtu.md)|
||vCore-basiertes Kaufmodell|[vCore-basiertes Kaufmodell](database/service-tiers-sql-database-vcore.md)|
|Bereitstellungsoption |Einzeldatenbank|[Einzeldatenbanken](database/single-database-overview.md)|
||Pool für elastische Datenbanken|[Pool für elastische Datenbanken](database/elastic-pool-overview.md)|
|Dienstebene|Basic, Standard, Premium, Universell, Hyperscale, Unternehmenskritisch|Informationen zu den Dienstebenen im vCore-Modell finden Sie unter [SQL-Datenbank-Dienstebenen](database/service-tiers-sql-database-vcore.md#service-tiers). Informationen zu Dienstebenen im DTU-Modell finden Sie unter [DTU-Modell](database/service-tiers-dtu.md#compare-the-dtu-based-service-tiers).|
|Computeebene|Serverloses Computing|[Serverloses Computing](database/service-tiers-sql-database-vcore.md#compute-tiers):
||Bereitgestelltes Computing|[Bereitgestelltes Computing](database/service-tiers-sql-database-vcore.md#compute-tiers)
|Computegeneration|Gen5, M-Serie, Fsv2-Serie, DC-Serie|[Hardwaregenerationen](database/service-tiers-sql-database-vcore.md#hardware-generations)
|Serverentität| Server |[Logische SQL Server-Instanzen](database/logical-servers.md)|
|Ressourcentyp|Virtueller Kern|Ein CPU-Kern, der der Computeressource für eine Einzeldatenbank oder einen Pool für elastische Datenbanken bereitgestellt wird. |
||Computegröße und Speichermenge|Die Computegröße ist die maximale Menge an CPU, Arbeitsspeicher und weiteren nicht speicherbezogenen Ressourcen, die für eine Einzeldatenbank oder einen Pool für elastische Datenbanken zur Verfügung steht.  Die Speichergröße ist der maximal verfügbare Speicherplatz für eine Einzeldatenbank oder einen Pool für elastische Datenbanken. Informationen zu den Größenoptionen im V-Kern-Modell finden Sie unter [Einzeldatenbanken im V-Kern-Modell](database/resource-limits-vcore-single-databases.md) und unter [Pools für elastische Datenbanken im V-Kern-Modell](database/resource-limits-vcore-elastic-pools.md).  (../managed-instance/resource-limits.md).  Informationen zu Größenanpassungsoptionen im DTU-Modell finden Sie unter [Einzeldatenbanken im DTU-Modell](database/resource-limits-dtu-single-databases.md) und [Pools für elastische Datenbanken im DTU-Modell](database/resource-limits-dtu-elastic-pools.md).

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

|Kontext|Begriff|Weitere Informationen|
|:---|:---|:---|
|Azure-Dienst|Azure SQL Managed Instance|[SQL Managed Instance](managed-instance/sql-managed-instance-paas-overview.md)|
|Kaufmodell|vCore-basiertes Kaufmodell|[vCore-basiertes Kaufmodell](managed-instance/service-tiers-managed-instance-vcore.md)|
|Bereitstellungsoption |Einzelne Instanz|[Einzelne Instanz](managed-instance/sql-managed-instance-paas-overview.md)|
||Instanzenpool (Vorschau)|[Instanzenpools](managed-instance/instance-pools-overview.md)|
|Dienstebene|Universell, Unternehmenskritisch|[SQL Managed Instance-Dienstebenen](managed-instance/sql-managed-instance-paas-overview.md#service-tiers)|
|Computeebene|Bereitgestelltes Computing|[Bereitgestelltes Computing](managed-instance/service-tiers-managed-instance-vcore.md#compute-tiers)|
|Computegeneration|Gen5|[Hardwaregenerationen](managed-instance/service-tiers-managed-instance-vcore.md#hardware-generations)
|Serverentität|Verwaltete Instanz oder Instanz| Nicht verfügbar, weil SQL Managed Instance selbst als Server fungiert. |
|Ressourcentyp|Virtueller Kern|Ein CPU-Kern, der der Computeressource für SQL Managed Instance bereitgestellt wird.|
||Computegröße und Speichermenge|Die Computegröße ist die maximale Menge an CPU, Arbeitsspeicher und weiteren nicht speicherbezogenen Ressourcen für SQL Managed Instance.  Die Speichergröße ist der maximal verfügbare Speicherplatz, der für SQL Managed Instance verfügbar ist.  Informationen zu Größenoptionen finden Sie unter [SQL Managed Instance](managed-instance/resource-limits.md). |
