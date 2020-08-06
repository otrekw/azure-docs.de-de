---
title: Verwalten von Lesereplikaten – Azure PowerShell – Azure Database for MySQL
description: Erfahren Sie, wie Sie mit PowerShell Lesereplikate in Azure Database for MySQL einrichten und verwalten.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 6/10/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: e99d378b49596ac208f7235a4d1d8b2b1c8c5310
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498785"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-powershell"></a>Informationen zum Erstellen und Verwalten von Lesereplikaten in Azure Database for MySQL mithilfe von PowerShell

In diesem Artikel erfahren Sie, wie Sie Lesereplikate im Azure Database for MySQL-Dienst über PowerShell erstellen und verwalten. Weitere Informationen zu Lesereplikaten finden Sie in der [Übersicht](concepts-read-replicas.md).

## <a name="azure-powershell"></a>Azure PowerShell

Sie können Lesereplikate mithilfe von PowerShell erstellen und verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

- Lokale Installation des [Moduls „Az PowerShell“](/powershell/azure/install-az-ps) oder von [Azure Cloud Shell](https://shell.azure.com/) im Browser
- Ein [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Solange nur eine Vorschauversion des PowerShell-Moduls „Az.MySql“ verfügbar ist, müssen Sie es separat über das AZ PowerShell-Modul installieren. Verwenden Sie dazu den folgenden Befehl: `Install-Module -Name Az.MySql -AllowPrerelease`.
> Sobald das PowerShell-Modul „Az.MySql“ allgemein verfügbar ist, wird es in die zukünftigen Releases des Az PowerShell-Moduls integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

Wenn Sie PowerShell lieber lokal verwenden möchten, stellen Sie mithilfe des Cmdlets [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) eine Verbindung mit Ihrem Azure-Konto her.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Das Feature für Lesereplikate ist nur für Azure Database for MySQL-Server in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ verfügbar. Stellen Sie sicher, dass für den Masterserver einer dieser Tarife festgelegt ist.

### <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats

> [!IMPORTANT]
> Wenn Sie ein Replikat für einen Master erstellen, der keine vorhandenen Replikate hat, startet der Master zunächst neu, um sich auf die Replikation vorzubereiten. Beachten Sie dies, und führen Sie diese Vorgänge nicht zu Spitzenzeiten durch.

Ein Lesereplikatserver kann mit dem folgenden Befehl erstellt werden:

```azurepowershell-interactive
Get-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup |
  New-AzMySqlServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

Für den Befehl `New-AzMySqlServerReplica` sind folgende Parameter erforderlich:

| Einstellung | Beispielwert | BESCHREIBUNG  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Die Ressourcengruppe, in der der Replikatserver erstellt wird.  |
| Name | mydemoreplicaserver | Der Name des neuen Replikatservers, der erstellt wird. |

Verwenden Sie den **Location**-Parameter, um ein regionsübergreifendes Lesereplikat zu erstellen. Im folgenden Beispiel wird ein Replikat in der Region **USA, Westen** erstellt.

```azurepowershell-interactive
Get-AzMySqlServer -Name mrdemoserver -ResourceGroupName myresourcegroup |
  New-AzMySqlServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup -Location westus
```

Weitere Informationen zu den Regionen, in denen Sie ein Replikat erstellen können, finden Sie im [Konzeptartikel zu Lesereplikaten](concepts-read-replicas.md).

Standardmäßig werden Lesereplikate mit der gleichen Serverkonfiguration wie der Master erstellt, es sei denn, der Parameter **Sku** wird angegeben.

> [!NOTE]
> Für die Konfiguration des Replikatservers sollten mindestens die gleichen Werte verwendet werden wie für den Masterserver, damit das Replikat über genügend Kapazität verfügt.

### <a name="list-replicas-for-a-master-server"></a>Auflisten von Replikaten für einen Masterserver

Führen Sie den folgenden Befehl aus, um alle Replikate für einen bestimmten Masterserver anzuzeigen:

```azurepowershell-interactive
Get-AzMySqlReplica -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Für den Befehl `Get-AzMySqlReplica` sind folgende Parameter erforderlich:

| Einstellung | Beispielwert | BESCHREIBUNG  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Die Ressourcengruppe, in der der Replikatserver erstellt wird.  |
| ServerName | mydemoserver | Der Name oder die ID des Masterservers. |

### <a name="delete-a-replica-server"></a>Löschen eines Replikatservers

Zum Löschen eines Lesereplikatservers können Sie das Cmdlet `Remove-AzMySqlServer` ausführen.

```azurepowershell-interactive
Remove-AzMySqlServer -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

### <a name="delete-a-master-server"></a>Löschen eines Masterservers

> [!IMPORTANT]
> Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Replikatservern beendet und der Masterserver selbst gelöscht. Replikatserver werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

Zum Löschen eines Masterservers können Sie das Cmdlet `Remove-AzMySqlServer` ausführen.

```azurepowershell-interactive
Remove-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Neustarten eines Azure Database for MySQL-Servers mithilfe von Azure PowerShell](howto-restart-server-powershell.md)
