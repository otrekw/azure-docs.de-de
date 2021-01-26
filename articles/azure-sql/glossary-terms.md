---
title: Glossarbegriffe
titleSuffix: Azure SQL
description: Ein Glossar mit Begriffen zur Arbeit mit Azure SQL-Datenbank, Azure SQL Managed Instance und SQL auf Azure-VMs.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=4
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/09/2020
ms.openlocfilehash: 8bd1e312463cf89ae30d54bc4a32a497fc7a9a78
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2021
ms.locfileid: "98249651"
---
# <a name="azure-sql-database-glossary-of-terms"></a>Glossar mit Begriffen aus Azure SQL-Datenbank
[!INCLUDE[appliesto-asf](includes/appliesto-asf.md)]

## <a name="azure-sql-database"></a>Azure SQL-Datenbank

|Kontext|Begriff|Weitere Informationen|
|:---|:---|:---|
|Azure-Dienst|Azure SQL-Datenbank oder SQL-Datenbank|[Azure SQL-Datenbank](database/sql-database-paas-overview.md)|
|Kaufmodell|DTU-basiertes Kaufmodell|[DTU-basiertes Kaufmodell](database/service-tiers-dtu.md)|
||vCore-basiertes Kaufmodell|[vCore-basiertes Kaufmodell](database/service-tiers-vcore.md)|
|Bereitstellungsoption |Einzeldatenbank|[Einzeldatenbanken](database/single-database-overview.md)|
||Pool für elastische Datenbanken|[Pool für elastische Datenbanken](database/elastic-pool-overview.md)|
|Dienstebene|Basic, Standard, Premium, Universell, Hyperscale, Unternehmenskritisch|Informationen zu den Dienstebenen im vCore-Modell finden Sie unter [SQL-Datenbank-Dienstebenen](database/service-tiers-vcore.md#service-tiers). Informationen zu Dienstebenen im DTU-Modell finden Sie unter [DTU-Modell](database/service-tiers-dtu.md#compare-the-dtu-based-service-tiers).|
|Computeebene|Serverloses Computing|[Serverloses Computing](database/service-tiers-vcore.md#compute-tiers):
||Bereitgestelltes Computing|[Bereitgestelltes Computing](database/service-tiers-vcore.md#compute-tiers)
|Computegeneration|Gen5, M-Serie, Fsv2-Serie, DC-Serie|[Hardwaregenerationen](database/service-tiers-vcore.md#hardware-generations)
|Serverentität| Server |[Logische SQL Server-Instanzen](database/logical-servers.md)|
|Ressourcentyp|Virtueller Kern|Ein CPU-Kern, der der Computeressource für eine Einzeldatenbank oder einen Pool für elastische Datenbanken bereitgestellt wird. |
||Computegröße und Speichermenge|Die Computegröße ist die maximale Menge an CPU, Arbeitsspeicher und weiteren nicht speicherbezogenen Ressourcen, die für eine Einzeldatenbank oder einen Pool für elastische Datenbanken zur Verfügung steht.  Die Speichergröße ist der maximal verfügbare Speicherplatz für eine Einzeldatenbank oder einen Pool für elastische Datenbanken. Informationen zu den Größenoptionen im V-Kern-Modell finden Sie unter [Einzeldatenbanken im V-Kern-Modell](database/resource-limits-vcore-single-databases.md) und unter [Pools für elastische Datenbanken im V-Kern-Modell](database/resource-limits-vcore-elastic-pools.md).  (../managed-instance/resource-limits.md).  Informationen zu Größenanpassungsoptionen im DTU-Modell finden Sie unter [Einzeldatenbanken im DTU-Modell](database/resource-limits-dtu-single-databases.md) und [Pools für elastische Datenbanken im DTU-Modell](database/resource-limits-dtu-elastic-pools.md).

## <a name="azure-sql-managed-instance"></a>Verwaltete Azure SQL-Instanz

|Kontext|Begriff|Weitere Informationen|
|:---|:---|:---|
|Azure-Dienst|Azure SQL Managed Instance|[SQL Managed Instance](managed-instance/sql-managed-instance-paas-overview.md)|
|Kaufmodell|vCore-basiertes Kaufmodell|[vCore-basiertes Kaufmodell](database/service-tiers-vcore.md)|
|Bereitstellungsoption |Einzelne Instanz|[Einzelne Instanz](managed-instance/sql-managed-instance-paas-overview.md)|
||Instanzenpool (Vorschau)|[Instanzenpools](managed-instance/instance-pools-overview.md)|
|Dienstebene|Universell, Unternehmenskritisch|[SQL Managed Instance-Dienstebenen](managed-instance/sql-managed-instance-paas-overview.md#service-tiers)|
|Computeebene|Bereitgestelltes Computing|[Bereitgestelltes Computing](database/service-tiers-vcore.md#compute-tiers)|
|Computegeneration|Gen5|[Hardwaregenerationen](database/service-tiers-vcore.md#hardware-generations)
|Serverentität|Verwaltete Instanz oder Instanz| Nicht verfügbar, weil SQL Managed Instance selbst als Server fungiert. |
|Ressourcentyp|Virtueller Kern|Ein CPU-Kern, der der Computeressource für SQL Managed Instance bereitgestellt wird.|
||Computegröße und Speichermenge|Die Computegröße ist die maximale Menge an CPU, Arbeitsspeicher und weiteren nicht speicherbezogenen Ressourcen für SQL Managed Instance.  Die Speichergröße ist der maximal verfügbare Speicherplatz, der für SQL Managed Instance verfügbar ist.  Informationen zu Größenoptionen finden Sie unter [SQL Managed Instance](managed-instance/resource-limits.md). |
