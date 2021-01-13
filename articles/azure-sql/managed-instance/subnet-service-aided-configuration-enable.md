---
title: Aktivieren der dienstgestützten Subnetzkonfiguration für Azure SQL Managed Instance
description: Aktivieren der dienstgestützten Subnetzkonfiguration für Azure SQL Managed Instance
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.date: 03/12/2020
ms.openlocfilehash: 67b398194d9094cd99fccaa85ed0df3be362ce2b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91618011"
---
# <a name="enabling-service-aided-subnet-configuration-for-azure-sql-managed-instance"></a>Aktivieren der dienstgestützten Subnetzkonfiguration für Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Die dienstgestützte Subnetzkonfiguration bietet eine automatisierte Netzwerkkonfigurationsverwaltung für Subnetze, in denen verwaltete Instanzen gehostet werden. Mit der dienstgestützten Subnetzkonfiguration behält der Benutzer die vollständige Kontrolle über den Datenzugriff (TDS-Datenverkehrsflüsse), während die verwaltete Instanz den ununterbrochenen Fluss des Verwaltungsdatenverkehrs sicherstellt, um die Vereinbarung zum Servicelevel (Service Level Agreement, SLA) zu erfüllen.

Automatisch konfigurierte Netzwerksicherheitsgruppen und Routingtabellenregeln sind für Kunden sichtbar und mit dem Präfix _Microsoft.Sql-managedInstances_UseOnly__ versehen.

Die dienstgestützte Konfiguration wird automatisch aktiviert, wenn Sie die [Subnetzdelegierung](../../virtual-network/subnet-delegation-overview.md) für den Ressourcenanbieter `Microsoft.Sql/managedInstances` aktivieren.

> [!IMPORTANT] 
> Wenn Sie die Subnetzdelegierung wieder deaktivieren möchten, müssen zuerst alle virtuellen Cluster aus dem Subnetz entfernt werden. Weitere Informationen zum Löschen virtueller Cluster finden Sie in [diesem Artikel](virtual-cluster-delete.md#delete-a-virtual-cluster-from-the-azure-portal).

> [!NOTE] 
> Da die dienstgestützte Subnetzkonfiguration für die Einhaltung der SLA erforderlich ist, können verwaltete Instanzen ab dem 1. Mai 2020 nicht mehr in Subnetzen ohne Delegierung an den Ressourcenanbieter für verwaltete Instanzen bereitgestellt werden. Am 1. Juli 2020 werden alle Subnetze mit verwalteten Instanzen automatisch an den Ressourcenanbieter für verwaltete Instanzen delegiert. 

## <a name="enabling-subnet-delegation-for-new-deployments"></a>Aktivieren der Subnetzdelegierung für neue Bereitstellungen
Wenn Sie eine verwaltete Instanz in einem leeren Subnetz bereitstellen möchten, ist eine automatische Delegierung an den Ressourcenanbieter `Microsoft.Sql/managedInstances` erforderlich, wie in [diesem Artikel](../../virtual-network/manage-subnet-delegation.md) beschrieben. _In dem angegebenen Artikel wird als Beispiel der Ressourcenanbieter `Microsoft.DBforPostgreSQL/serversv2` verwendet. Sie müssen stattdessen den Ressourcenanbieter `Microsoft.Sql/managedInstances` verwenden._

## <a name="enabling-subnet-delegation-for-existing-deployments"></a>Aktivieren der Subnetzdelegierung für bereits vorhandene Bereitstellungen

Wenn Sie die Subnetzdelegierung für eine bereits vorhandene Bereitstellung einer verwalteten Instanz aktivieren möchten, müssen Sie das Subnetz des virtuellen Netzwerks ermitteln, in dem sie sich befindet. 

Diese Information finden Sie auf dem Portalblatt `Overview` Ihrer verwalteten Instanz unter `Virtual network/subnet`.

Alternativ können Sie die folgenden PowerShell-Befehle ausführen. Ersetzen Sie **subscription-id** durch Ihre Abonnement-ID. Ersetzen Sie außerdem **rg-name** durch die Ressourcengruppe für Ihre verwaltete Instanz und **mi-name** durch den Namen Ihrer verwalteten Instanz.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi.SubnetId
```

Nachdem Sie das Subnetz der verwalteten Instanz gefunden haben, müssen Sie es an den Ressourcenanbieter `Microsoft.Sql/managedInstances` delegieren, wie in [diesem Artikel](../../virtual-network/manage-subnet-delegation.md) beschrieben. _In dem angegebenen Artikel wird als Beispiel der Ressourcenanbieter `Microsoft.DBforPostgreSQL/serversv2` verwendet. Sie müssen stattdessen den Ressourcenanbieter `Microsoft.Sql/managedInstances` verwenden._


> [!IMPORTANT]
> Die Aktivierung der dienstgestützten Konfiguration hat kein Failover und keine Unterbrechung der Konnektivität für verwaltete Instanzen zur Folge, die sich bereits im Subnetz befinden.
