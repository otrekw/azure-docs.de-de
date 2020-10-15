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
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: 35d0053d8c45547eaa5e89e6da15fcaf0343d6c5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91617297"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Überprüfen der integrierten Firewall von Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Die [obligatorischen Eingangssicherheitsregeln](connectivity-architecture-overview.md#mandatory-inbound-security-rules) von Azure SQL Managed Instance schreiben vor, dass die Verwaltungsports 9000, 9003, 1438, 1440 und 1452 für **alle Quellen** der Netzwerksicherheitsgruppe (NSG) offen sind, die SQL Managed Instance schützt. Zwar sind diese Ports auf NSG-Ebene offen, auf der Netzwerkebene sind sie jedoch durch die integrierte Firewall geschützt.

## <a name="verify-firewall"></a>Überprüfen der Firewall

Um diese Ports zu überprüfen, verwenden Sie ein beliebiges Überprüfungssicherheitstool zum Testen der Ports. Der folgende Screenshot zeigt die Verwendung eines solchen Tools.

![Überprüfen der integrierten Firewall](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu SQL Managed Instance und zur Konnektivität finden Sie unter [Konnektivitätsarchitektur für Azure SQL Managed Instance](connectivity-architecture-overview.md).
