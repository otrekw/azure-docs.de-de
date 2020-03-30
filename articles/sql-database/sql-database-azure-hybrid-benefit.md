---
title: Azure-Hybridvorteil
description: Verwenden Sie vorhandene SQL Server-Lizenzen für SQL-Datenbank-Rabatte.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/13/2019
ms.openlocfilehash: d1a59e7ad86191bcc30b7d898d00f327c20fbc5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945619"
---
# <a name="azure-hybrid-benefit"></a>Azure-Hybridvorteil

Bei der bereitgestellten Computeebene des V-Kern-basierten Kaufmodells können Sie Ihre vorhandenen Lizenzen gegen Rabattpreise für SQL-Datenbank tauschen, indem Sie den [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) verwenden. Mit diesem Azure-Vorteil können Sie bei Azure SQL-Datenbank mindestens 30 Prozent sparen, indem Sie Ihre lokalen SQL Server-Lizenzen mit Software Assurance verwenden. Verwenden Sie den Einsparungsrechner für den Azure-Hybridvorteil unter dem oben angegebenen Link, um die genauen Werte zu ermitteln. 

> [!NOTE]
> Um zu Azure-Hybridvorteil zu wechseln, ist keine Downtime erforderlich.

![Preise](./media/sql-database-service-tiers/pricing.png)

## <a name="choose-a-license-model"></a>Auswählen eines Lizenzmodells

Mit dem Azure-Hybridvorteil können Sie wahlweise nur die zugrunde liegende Azure-Infrastruktur bezahlen und Ihre vorhandene SQL Server-Lizenz für die eigentliche SQL-Datenbank-Engine verwenden (Grundpreis für Computeleistung), oder Sie bezahlen für die zugrunde liegende Infrastruktur sowie für die SQL Server-Lizenz (Preismodell mit enthaltener Lizenz).

Sie können Ihr Lizenzierungsmodell im Azure-Portal oder mithilfe einer der folgenden APIs auswählen oder ändern:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Festlegen oder Aktualisieren des Lizenztyps mithilfe von PowerShell:

- [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase)
- [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase)
- [New-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance)
- [Set-AzSqlInstance](/powershell/module/az.sql/set-azsqlinstance)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Festlegen oder Aktualisieren des Lizenztyps mithilfe der Azure-Befehlszeilenschnittstelle:

- [az sql db create](/cli/azure/sql/db#az-sql-db-create)
- [az sql db update](/cli/azure/sql/db#az-sql-db-update)
- [az sql mi create](/cli/azure/sql/mi#az-sql-mi-create)
- [az sql mi update](/cli/azure/sql/mi#az-sql-mi-update)

# <a name="rest-api"></a>[REST-API](#tab/rest)

Festlegen oder Aktualisieren des Lizenztyps mithilfe der REST-API:

- [Datenbanken – Erstellen oder Aktualisieren](/rest/api/sql/databases/createorupdate)
- [Datenbanken – Aktualisieren](/rest/api/sql/databases/update)
- [Verwaltete Instanzen – Erstellen oder Aktualisieren](/rest/api/sql/managedinstances/createorupdate)
- [Verwaltete Instanzen – Aktualisieren](/rest/api/sql/managedinstances/update)

* * *

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Auswahl zwischen den Bereitstellungsoptionen für Azure SQL-Datenbank finden Sie unter [Auswählen der richtigen Bereitstellungsoption in Azure SQL](sql-database-paas-vs-sql-server-iaas.md).
- Einen Vergleich der Funktionen von Azure SQL-Datenbank finden Sie unter [Azure SQL-Datenbank – Funktionen](sql-database-features.md).
