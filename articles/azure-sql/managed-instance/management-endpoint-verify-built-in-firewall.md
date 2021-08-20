---
title: Überprüfen der Portsicherheit der integrierten Firewall
description: Hier erfahren Sie, wie Sie den integrierten Firewallschutz in Azure SQL Managed Instance überprüfen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: mathoma
ms.date: 12/04/2018
ms.openlocfilehash: 21401f20fe1f71b49861a3ddae8a5a3cdb999408
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122355570"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Überprüfen der integrierten Firewall von Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Die [obligatorischen Eingangssicherheitsregeln](connectivity-architecture-overview.md#mandatory-inbound-security-rules-with-service-aided-subnet-configuration) von Azure SQL Managed Instance schreiben vor, dass die Verwaltungsports 9000, 9003, 1438, 1440 und 1452 für **alle Quellen** der Netzwerksicherheitsgruppe (NSG) offen sind, die SQL Managed Instance schützt. Zwar sind diese Ports auf NSG-Ebene offen, auf der Netzwerkebene sind sie jedoch durch die integrierte Firewall geschützt.

## <a name="verify-firewall"></a>Überprüfen der Firewall

Um diese Ports zu überprüfen, verwenden Sie ein beliebiges Überprüfungssicherheitstool zum Testen der Ports. Der folgende Screenshot zeigt die Verwendung eines solchen Tools.

![Überprüfen der integrierten Firewall](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL Managed Instance und zur Konnektivität finden Sie unter [Konnektivitätsarchitektur für Azure SQL Managed Instance](connectivity-architecture-overview.md).
