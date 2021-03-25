---
title: Neustarten von Servern – Azure PowerShell – Azure Database for PostgreSQL
description: In diesem Artikel wird beschrieben, wie Sie einen Azure Database for PostgreSQL-Server mit PowerShell neu starten.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 06/08/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 2d37de4a1861cb78b4a76c8ca7bc8c3643245b32
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97706965"
---
# <a name="restart-azure-database-for-postgresql-server-using-powershell"></a>Neustarten eines Azure Database for PostgreSQL-Servers mit PowerShell

In diesem Thema wird erläutert, wie Sie einen Azure Database for PostgreSQL-Server neu starten können. Möglicherweise müssen Sie Ihren Server zu Wartungszwecken neu starten. Während des Vorgangs kommt es dann zu einem kurzen Ausfall.

Der Neustart des Servers wird blockiert, wenn der Dienst ausgelastet ist. Beispielsweise kann der Dienst einen zuvor angeforderten Vorgang (z. B. das Skalieren von virtuellen Kernen) verarbeiten.

> [!NOTE] 
> Die für einen Neustart benötigte Zeit hängt von dem PostgreSQL-Wiederherstellungsprozess ab. Um die Neustartzeit zu verkürzen, empfehlen wir, die Aktivitäten auf dem Server vor dem Neustart auf ein Minimum zu beschränken. Sie sollten zudem die Prüfpunkthäufigkeit erhöhen. Darüber hinaus können Sie prüfpunktbezogene Parameterwerte optimieren, einschließlich `max_wal_size`. Außerdem wird empfohlen, vor dem Neustart des Servers den Befehl `CHECKPOINT` auszuführen.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

- Lokale Installation des [Moduls „Az PowerShell“](/powershell/azure/install-az-ps) oder von [Azure Cloud Shell](https://shell.azure.com/) im Browser
- Einen [Azure-Datenbank für PostgreSQL-Server](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Solange nur eine Vorschauversion des PowerShell-Moduls „Az.PostgreSql“ verfügbar ist, müssen Sie es separat über das Az PowerShell-Modul installieren. Verwenden Sie dazu den folgenden Befehl: `Install-Module -Name Az.PostgreSql -AllowPrerelease`.
> Sobald das PowerShell-Modul „Az.PostgreSql“ allgemein verfügbar ist, wird es in die zukünftigen Releases des Az PowerShell-Moduls integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

Wenn Sie PowerShell lieber lokal verwenden möchten, stellen Sie mithilfe des Cmdlets [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) eine Verbindung mit Ihrem Azure-Konto her.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="restart-the-server"></a>Neustarten des Servers

Starten Sie den Server mit dem folgenden Befehl neu:

```azurepowershell-interactive
Restart-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen eines Azure Database for PostgreSQL-Servers mithilfe von PowerShell](quickstart-create-postgresql-server-database-using-azure-powershell.md)