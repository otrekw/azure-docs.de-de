---
title: Konfigurieren eines vorhandenen virtuellen Netzwerks
titleSuffix: Azure SQL Managed Instance
description: In diesem Artikel wird beschrieben, wie Sie ein vorhandenes virtuelles Netzwerk und Subnetz konfigurieren, in dem Sie Azure SQL Managed Instance bereitstellen können.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova
ms.date: 03/17/2020
ms.openlocfilehash: 461acc07ee2217a38f7bb59805d4c7e0de4a1e22
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91617654"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-managed-instance"></a>Konfigurieren eines vorhandenen virtuellen Netzwerks für Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL Managed Instance muss in einem [virtuellen Azure-Netzwerk](../../virtual-network/virtual-networks-overview.md) und in dem Subnetz bereitgestellt werden, das speziell für verwaltete Instanzen bestimmt ist. Sie können das vorhandene virtuelle Netzwerk und Subnetz verwenden, wenn diese gemäß den [Anforderungen an virtuelle Netzwerke für SQL Managed Instance](connectivity-architecture-overview.md#network-requirements) konfiguriert sind.

Wenn einer der folgenden Fälle auf Sie zutrifft, können Sie Ihr Netzwerk mithilfe des in diesem Artikel erläuterten Skripts überprüfen und ändern:

- Sie haben ein neues Subnetz, das noch nicht konfiguriert ist.
- Sie sind nicht sicher, ob das Subnetz den [Anforderungen](connectivity-architecture-overview.md#network-requirements) entspricht.
- Sie möchten überprüfen, ob das Subnetz weiterhin die [Netzwerkanforderungen](connectivity-architecture-overview.md#network-requirements) erfüllt, nachdem Sie Änderungen vorgenommen haben.

> [!Note]
> Sie können eine verwaltete Instanz nur in virtuellen Netzwerken erstellen, die mit dem Azure Resource Manager-Bereitstellungsmodell erstellt wurden. Virtuelle Azure-Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden, werden nicht unterstützt. Berechnen Sie die Subnetzgröße anhand der Richtlinien im Artikel [Ermitteln der Größe des Subnetzes für Azure SQL Managed Instance](vnet-subnet-determine-size.md). Die Größe des Subnetzes kann nach der Bereitstellung der darin enthaltenen Ressourcen nicht mehr geändert werden.
>
> Nachdem eine verwaltete Instanz erstellt wurde, wird das Verschieben dieser Instanz oder des virtuellen Netzwerks in eine andere Ressourcengruppe oder ein anderes Abonnement nicht unterstützt.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Überprüfen und Ändern eines vorhandenen virtuellen Netzwerks

Wenn Sie eine verwaltete Instanz in einem vorhandenen Subnetz erstellen möchten, empfehlen wir das folgende PowerShell-Skript zum Vorbereiten des Subnetzes:

```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/delegate-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/delegateSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```

Mit dem Skript wird das Subnetz in drei Schritten vorbereitet:

1. Überprüfen: Das ausgewählte virtuelle Netzwerk und das Subnetz werden im Hinblick auf die Netzwerkanforderungen für SQL Managed Instance überprüft.
2. Bestätigen: Dem Benutzer werden Änderungen angezeigt, die für die Vorbereitung des Subnetzes auf die SQL Managed Instance-Bereitstellung durchgeführt werden müssen. Sie müssen dann diesen Änderungen zustimmen.
3. Vorbereiten: Das virtuelle Netzwerk und das Subnetz werden ordnungsgemäß konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist SQL Managed Instance?](sql-managed-instance-paas-overview.md).
- Ein Tutorial zum Erstellen eines virtuellen Netzwerks und einer verwalteten Instanz sowie zum Wiederherstellen einer Datenbank aus einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Instanz](instance-create-quickstart.md).
- Lesen Sie bei Problemen im Zusammenhang mit dem DNS den Artikel [Konfigurieren eines benutzerdefinierten DNS](custom-dns-configure.md).
