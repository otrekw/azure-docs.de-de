---
title: Kaufmodell für virtuelle Kerne
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Mit dem Kaufmodell für virtuelle Kerne können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis für Azure SQL-Datenbank und Azure SQL Managed Instance optimieren.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service-overview
ms.topic: conceptual
author: dimitri-furman
ms.author: dfurman
ms.reviewer: mathoma
ms.date: 05/18/2021
ROBOTS: NOINDEX
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 6c16e508aa2002aff07df5fc30e0af4c74bdd025
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111413698"
---
# <a name="vcore-model-overview---azure-sql-database-and-azure-sql-managed-instance"></a>Übersicht über das Modell für virtuelle Kerne – Azure SQL-Datenbank und Azure SQL Managed Instance 
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Das Kaufmodell für virtuelle Kerne von Azure SQL-Datenbank und Azure SQL Managed Instance bietet einige Vorteile:

- Kontrolle über die Hardwaregeneration, um besser auf Compute- und Arbeitsspeicheranforderungen der Workload reagieren zu können
- Preisrabatte für [Azure-Hybridvorteil](../azure-hybrid-benefit.md) und [reservierte Instanzen (RI)](reserved-capacity-overview.md)
- Mehr Transparenz bei den Hardwaredetails für das Computing zur besseren Planung von Migrationsvorgängen von lokalen Bereitstellungen
- Im Fall von Azure SQL-Datenbank bietet das Kaufmodell für virtuelle Kerne höhere Compute-, Arbeitsspeicher-, E/A- und Speichergrenzwerte als das DTU-Modell.

Weitere Informationen zur Wahl zwischen dem V-Kern-Kaufmodell (Kaufmodell für virtuelle Kerne) und dem DTU-Kaufmodell finden Sie unter [Entscheidung zwischen dem V-Kern- und DTU-Kaufmodell](purchasing-models.md).

## <a name="service-tiers"></a>Dienstebenen

Die folgenden Artikel enthalten spezifische Informationen zum Kaufmodell für virtuelle Kerne in den einzelnen Produkten.

- Informationen zu Dienstebenen für Azure SQL-Datenbank für das V-Kern-Modell finden Sie unter [Übersicht über das V-Kern-Modell – Azure SQL-Datenbank](service-tiers-sql-database-vcore.md).
- Informationen zu Dienstebenen für Azure SQL Managed Instance für das V-Kern-Modell finden Sie unter [Übersicht über das V-Kern-Modell – Azure SQL Managed Instance](../managed-instance/service-tiers-managed-instance-vcore.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten finden Sie unter: 
- [Erstellen einer SQL-Datenbank-Instanz mithilfe des Azure-Portals](single-database-create-quickstart.md)
- [Erstellen einer Instanz von SQL Managed Instance mithilfe des Azure-Portals](../managed-instance/instance-create-quickstart.md)

- Preisdetails finden Sie unter folgenden Links. 
    - [Preisseite für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/)
    - [Preisseite für Azure SQL Managed Instance Einzelinstanz](https://azure.microsoft.com/pricing/details/azure-sql-managed-instance/single/)
    - [Preisseite für Azure SQL Managed Instance-Pools](https://azure.microsoft.com/pricing/details/azure-sql-managed-instance/pools/)
    
Ausführliche Informationen zu den spezifischen Compute- und Speichergrößen der Dienstebenen „Universell“ und „Unternehmenskritisch“ finden Sie unter:

- [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für Azure SQL-Datenbank](resource-limits-vcore-single-databases.md)
- [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für Azure SQL-Datenbank](resource-limits-vcore-elastic-pools.md)
- [Ressourcenlimits des auf virtuellen Kernen basierenden Kaufmodells für Azure SQL Managed Instance](../managed-instance/resource-limits.md)
