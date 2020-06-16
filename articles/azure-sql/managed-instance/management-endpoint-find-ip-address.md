---
title: Ermitteln der IP-Adresse des Verwaltungsendpunkts
titleSuffix: Azure SQL Managed Instance
description: Erfahren Sie, wie Sie die öffentliche IP-Adresse eines Verwaltungsendpunkts für Azure SQL Managed Instance abrufen und den integrierten Firewallschutz überprüfen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 88965c25702917f17a226cfa51de662703136aae
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "84031161"
---
# <a name="determine-the-management-endpoint-ip-address---azure-sql-managed-instance"></a>Ermitteln der IP-Adresse des Verwaltungsendpunkts: Azure SQL Managed Instance 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Der virtuelle Cluster von Azure SQL Managed Instance enthält einen Verwaltungsendpunkt, der von Azure für Verwaltungsvorgänge verwendet wird. Der Verwaltungsendpunkt ist durch eine integrierte Firewall auf Netzwerkebene und eine gegenseitige Zertifikatüberprüfung auf Anwendungsebene geschützt. Sie können die IP-Adresse des Verwaltungsendpunkts ermitteln, aber Sie können nicht auf diesen Endpunkt zugreifen.

Führen Sie einen [DNS-Lookup](/windows-server/administration/windows-commands/nslookup) auf Ihrem SQL Managed Instance-FQDN `mi-name.zone_id.database.windows.net` aus, um die Verwaltungs-IP-Adresse zu bestimmen. Dadurch wird ein DNS-Eintrag zurückgegeben, der `trx.region-a.worker.vnet.database.windows.net` ähnelt. Anschließend können Sie einen DNS-Lookup für diesen vollqualifizierten Domänennamen ohne „.vnet“ durchführen. Dadurch wird die Verwaltungs-IP-Adresse zurückgegeben. 

Der folgende PowerShell-Befehl kann all das für Sie tun, wenn Sie \<MI FQDN\>durch den DNS-Eintrag Ihrer SQL Managed Instance (`mi-name.zone_id.database.windows.net`) ersetzen:
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Weitere Informationen zu SQL Managed Instance und zur Konnektivität finden Sie unter [Konnektivitätsarchitektur für Azure SQL Managed Instance](connectivity-architecture-overview.md).
