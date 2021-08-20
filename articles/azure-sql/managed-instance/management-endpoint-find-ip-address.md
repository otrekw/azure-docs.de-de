---
title: Ermitteln der IP-Adresse des Verwaltungsendpunkts
titleSuffix: Azure SQL Managed Instance
description: Erfahren Sie, wie Sie die öffentliche IP-Adresse eines Verwaltungsendpunkts für Azure SQL Managed Instance abrufen und den integrierten Firewallschutz überprüfen.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: deployment-configuration
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: mathoma
ms.date: 12/04/2018
ms.openlocfilehash: 8603f026d396316edf9d83e52ef33973ddc8edd7
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122340278"
---
# <a name="determine-the-management-endpoint-ip-address---azure-sql-managed-instance"></a>Ermitteln der IP-Adresse des Verwaltungsendpunkts: Azure SQL Managed Instance 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Der virtuelle Cluster von Azure SQL Managed Instance enthält einen Verwaltungsendpunkt, der von Azure für Verwaltungsvorgänge verwendet wird. Der Verwaltungsendpunkt ist durch eine integrierte Firewall auf Netzwerkebene und eine gegenseitige Zertifikatüberprüfung auf Anwendungsebene geschützt. Sie können die IP-Adresse des Verwaltungsendpunkts ermitteln, aber Sie können nicht auf diesen Endpunkt zugreifen.

Führen Sie einen [DNS-Lookup](/windows-server/administration/windows-commands/nslookup) auf Ihrem SQL Managed Instance-FQDN `mi-name.zone_id.database.windows.net` aus, um die Verwaltungs-IP-Adresse zu bestimmen. Dadurch wird ein DNS-Eintrag zurückgegeben, der `trx.region-a.worker.vnet.database.windows.net` ähnelt. Anschließend können Sie einen DNS-Lookup für diesen vollqualifizierten Domänennamen ohne „.vnet“ durchführen. Dadurch wird die Verwaltungs-IP-Adresse zurückgegeben. 

Der folgende PowerShell-Code kann all das für Sie tun, wenn Sie \<MI FQDN\> durch den DNS-Eintrag Ihrer SQL Managed Instance ersetzen: `mi-name.zone_id.database.windows.net`:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Weitere Informationen zu SQL Managed Instance und zur Konnektivität finden Sie unter [Konnektivitätsarchitektur für Azure SQL Managed Instance](connectivity-architecture-overview.md).
