---
title: Verwalten von Lesereplikaten – Azure PowerShell – Azure Database for PostgreSQL
description: Erfahren Sie, wie Sie mit PowerShell Lesereplikate in Azure Database for PostgreSQL einrichten und verwalten.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: how-to
ms.date: 06/08/2020
ms.openlocfilehash: 45876c8e176a4f81abfd927b0e2a8df8fc26ac25
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86106853"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-postgresql-using-powershell"></a>Informationen zum Erstellen und Verwalten von Lesereplikaten in Azure Database for PostgreSQL mithilfe von PowerShell

In diesem Artikel erfahren Sie, wie Sie Lesereplikate im Azure Database for PostgreSQL-Dienst über PowerShell erstellen und verwalten. Weitere Informationen zu Lesereplikaten finden Sie in der [Übersicht](concepts-read-replicas.md).

## <a name="azure-powershell"></a>Azure PowerShell

Sie können Lesereplikate mithilfe von PowerShell erstellen und verwalten.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

- Lokale Installation des [Moduls „Az PowerShell“](https://docs.microsoft.com/powershell/azure/install-az-ps) oder von [Azure Cloud Shell](https://shell.azure.com/) im Browser
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Solange nur eine Vorschauversion des PowerShell-Moduls „Az.PostgreSql“ verfügbar ist, müssen Sie es separat über das Az PowerShell-Modul installieren. Verwenden Sie dazu den folgenden Befehl: `Install-Module -Name Az.PostgreSql -AllowPrerelease`.
> Sobald das PowerShell-Modul „Az.PostgreSql“ allgemein verfügbar ist, wird es in die zukünftigen Releases des Az PowerShell-Moduls integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

Wenn Sie PowerShell lieber lokal verwenden möchten, stellen Sie mithilfe des Cmdlets [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) eine Verbindung mit Ihrem Azure-Konto her.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Das Feature für Lesereplikate ist nur für Azure Database for PostgreSQL-Server in den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ verfügbar. Stellen Sie sicher, dass für den Masterserver einer dieser Tarife festgelegt ist.

### <a name="create-a-read-replica"></a>Erstellen eines Lesereplikats

Ein Lesereplikatserver kann mit dem folgenden Befehl erstellt werden:

```azurepowershell-interactive
Get-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup |
  New-AzPostgreSqlServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

Für den Befehl `New-AzPostgreSqlServerReplica` sind folgende Parameter erforderlich:

| Einstellung | Beispielwert | BESCHREIBUNG  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Die Ressourcengruppe, in der der Replikatserver erstellt wird.  |
| Name | mydemoreplicaserver | Der Name des neuen Replikatservers, der erstellt wird. |

Verwenden Sie den **Location**-Parameter, um ein regionsübergreifendes Lesereplikat zu erstellen. Im folgenden Beispiel wird ein Replikat in der Region **USA, Westen** erstellt.

```azurepowershell-interactive
Get-AzPostgreSqlServer -Name mrdemoserver -ResourceGroupName myresourcegroup |
  New-AzMariaDServerReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup -Location westus
```

Weitere Informationen zu den Regionen, in denen Sie ein Replikat erstellen können, finden Sie im [Konzeptartikel zu Lesereplikaten](concepts-read-replicas.md).

Standardmäßig werden Lesereplikate mit der gleichen Serverkonfiguration wie der Master erstellt, es sei denn, der Parameter **Sku** wird angegeben.

> [!NOTE]
> Für die Konfiguration des Replikatservers sollten mindestens die gleichen Werte verwendet werden wie für den Masterserver, damit das Replikat über genügend Kapazität verfügt.

### <a name="list-replicas-for-a-master-server"></a>Auflisten von Replikaten für einen Masterserver

Führen Sie den folgenden Befehl aus, um alle Replikate für einen bestimmten Masterserver anzuzeigen:

```azurepowershell-interactive
Get-AzMariaDReplica -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Für den Befehl `Get-AzMariaDReplica` sind folgende Parameter erforderlich:

| Einstellung | Beispielwert | BESCHREIBUNG  |
| --- | --- | --- |
| ResourceGroupName |  myresourcegroup |  Die Ressourcengruppe, in der der Replikatserver erstellt wird.  |
| ServerName | mydemoserver | Der Name oder die ID des Masterservers. |

### <a name="delete-a-replica-server"></a>Löschen eines Replikatservers

Zum Löschen eines Lesereplikatservers können Sie das Cmdlet `Remove-AzPostgreSqlServer` ausführen.

```azurepowershell-interactive
Remove-AzPostgreSqlServer -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

### <a name="delete-a-master-server"></a>Löschen eines Masterservers

> [!IMPORTANT]
> Wenn Sie einen Masterserver löschen, wird die Replikation auf allen Replikatservern beendet und der Masterserver selbst gelöscht. Replikatserver werden zu eigenständigen Servern, die nun Lese- und Schreibvorgänge unterstützen.

Zum Löschen eines Masterservers können Sie das Cmdlet `Remove-AzPostgreSqlServer` ausführen.

```azurepowershell-interactive
Remove-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Neustarten eines Azure Database for PostgreSQL-Servers mit PowerShell](howto-restart-server-powershell.md)
