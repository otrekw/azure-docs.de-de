---
title: Entdecken von Verwaltungsendpunkten für verwaltete Instanzen
description: Es wird beschrieben, wie Sie die öffentliche IP-Adresse eines Verwaltungsendpunkts für eine verwaltete Azure SQL-Datenbank-Instanz abrufen und den integrierten Firewallschutz überprüfen.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: ac4675be25cb5f2f65cc444e65bd25d65d3c3856
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83770039"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Ermitteln der IP-Adresse des Verwaltungsendpunkts

Der virtuelle Cluster der verwalteten Azure SQL-Datenbank-Instanz enthält einen Verwaltungsendpunkt, der von Microsoft für Verwaltungsvorgänge verwendet wird. Der Verwaltungsendpunkt ist durch eine integrierte Firewall auf Netzwerkebene und eine gegenseitige Zertifikatüberprüfung auf Anwendungsebene geschützt. Sie können die IP-Adresse des Verwaltungsendpunkts ermitteln, aber Sie können nicht auf diesen Endpunkt zugreifen.

Führen Sie einen DNS-Lookup für den vollqualifizierten Domänennamen `mi-name.zone_id.database.windows.net` Ihrer verwalteten Instanz durch, um die Verwaltungs-IP-Adresse zu ermitteln. Dadurch wird ein DNS-Eintrag zurückgegeben, der `trx.region-a.worker.vnet.database.windows.net` ähnelt. Anschließend können Sie einen DNS-Lookup für diesen vollqualifizierten Domänennamen ohne „.vnet“ durchführen. Dadurch wird die Verwaltungs-IP-Adresse zurückgegeben. 

Der folgende PowerShell-Befehl kann all das für Sie tun, wenn Sie \<MI FQDN\> durch den DNS-Eintrag Ihrer verwalteten Instanz (`mi-name.zone_id.database.windows.net`) ersetzen:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Weitere Informationen zu verwalteten Instanzen und zur Konnektivität finden Sie unter [Azure SQL Database Managed Instance Connectivity Architecture](sql-database-managed-instance-connectivity-architecture.md) (Konnektivitätsarchitektur der verwalteten Azure SQL-Datenbank-Instanz).
