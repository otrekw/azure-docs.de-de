---
title: Automatische Speichervergrößerung – Azure PowerShell – Azure Database for PostgreSQL
description: In diesem Artikel wird beschrieben, wie Sie die automatische Speichervergrößerung in Azure Database for PostgreSQL mithilfe von PowerShell aktivieren können.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 06/08/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: a186c6e176cde20474cdf4772b8724ad3f356a3e
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87493617"
---
# <a name="auto-grow-storage-in-azure-database-for-postgresql-server-using-powershell"></a>Automatische Speichervergrößerung für einen Azure Database for PostgreSQL-Server mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie die Speichervergrößerung für einen Azure Database for PostgreSQL-Server konfigurieren können, ohne die Workload zu beeinträchtigen.

Die automatische Speichervergrößerung verhindert, dass Ihr Server [das Speicherlimit erreicht](/azure/postgresql/concepts-pricing-tiers#reaching-the-storage-limit) und dann schreibgeschützt wird. Bei Servern mit maximal 100 GB bereitgestelltem Speicher wird die Größe um 5 GB erhöht, wenn der freie Speicherplatz unter 10 % liegt. Bei Servern mit mehr als 100 GB bereitgestelltem Speicher wird die Größe um 5 % erhöht, wenn der freie Speicherplatz unter 10 GB liegt. Die maximalen Speicherlimits gelten so, wie im Abschnitt „Speicher“ des Artikels [Azure Database for PostgreSQL – Tarife](/azure/postgresql/concepts-pricing-tiers#storage) angegeben.

> [!IMPORTANT]
> Beachten Sie, dass der Speicher nur zentral hochskaliert und nicht herunterskaliert werden kann.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

- Lokale Installation des [Moduls „Az PowerShell“](https://docs.microsoft.com/powershell/azure/install-az-ps) oder von [Azure Cloud Shell](https://shell.azure.com/) im Browser
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Solange nur eine Vorschauversion des PowerShell-Moduls „Az.PostgreSql“ verfügbar ist, müssen Sie es separat über das Az PowerShell-Modul installieren. Verwenden Sie dazu den folgenden Befehl: `Install-Module -Name Az.PostgreSql -AllowPrerelease`.
> Sobald das PowerShell-Modul „Az.PostgreSql“ allgemein verfügbar ist, wird es in die zukünftigen Releases des Az PowerShell-Moduls integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

Wenn Sie PowerShell lieber lokal verwenden möchten, stellen Sie mithilfe des Cmdlets [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) eine Verbindung mit Ihrem Azure-Konto her.

## <a name="enable-postgresql-server-storage-auto-grow"></a>Aktivieren der automatischen Speichervergrößerung für den PostgreSQL-Server

Aktivieren Sie die automatische Speichervergrößerung auf einem vorhandenen Server mit dem folgenden Befehl:

```azurepowershell-interactive
Update-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -StorageAutogrow Enabled
```

Aktivieren Sie die automatische Speichervergrößerung beim Erstellen eines neuen Servers mit dem folgenden Befehl:

```azurepowershell-interactive
$Password = Read-Host -Prompt 'Please enter your password' -AsSecureString
New-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -Sku GP_Gen5_2 -StorageAutogrow Enabled -Location westus -AdministratorUsername myadmin -AdministratorLoginPassword $Password
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Informationen zum Erstellen und Verwalten von Lesereplikaten in Azure Database for PostgreSQL mithilfe von PowerShell](howto-read-replicas-powershell.md).
