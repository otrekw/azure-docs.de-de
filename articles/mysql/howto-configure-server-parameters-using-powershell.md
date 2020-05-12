---
title: Konfigurieren von Serverparametern – Azure PowerShell – Azure Database for MySQL
description: In diesem Artikel wird beschrieben, wie Sie mit PowerShell die Dienstparameter in Azure Database for MySQL konfigurieren.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurepowershell
ms.topic: conceptual
ms.date: 4/29/2020
ms.openlocfilehash: 0de816d25bbc1563885413d8dbd52dc7bda7d538
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614163"
---
# <a name="customize-azure-database-for-mysql-server-parameters-using-powershell"></a>Anpassen eines Azure Database for MySQL-Serverparameters mithilfe von PowerShell

Sie können Konfigurationsparameter für einen Azure Database for MySQL-Server mithilfe von PowerShell auflisten, anzeigen und aktualisieren. Auf Serverebene ist eine Teilmenge der Engine-Konfigurationen verfügbar und kann geändert werden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

- Lokale Installation des [Moduls „Az PowerShell“](/powershell/azure/install-az-ps) oder von [Azure Cloud Shell](https://shell.azure.com/) im Browser
- Ein [Azure Database for MySQL-Server](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Solange nur eine Vorschauversion des PowerShell-Moduls „Az.MySql“ verfügbar ist, müssen Sie es separat über das AZ PowerShell-Modul installieren. Verwenden Sie dazu den folgenden Befehl: `Install-Module -Name Az.MySql -AllowPrerelease`.
> Sobald das PowerShell-Modul „Az.MySql“ allgemein verfügbar ist, wird es in die zukünftigen Releases des Az PowerShell-Moduls integriert und in Azure Cloud Shell nativ zur Verfügung gestellt.

Wenn Sie PowerShell lieber lokal verwenden möchten, stellen Sie mithilfe des Cmdlets [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount) eine Verbindung mit Ihrem Azure-Konto her.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Auflisten der Serverkonfigurationsparameter für Azure Database for MySQL

Führen Sie das Cmdlet `Get-AzMySqlConfiguration` aus, um alle änderbaren Parameter eines Servers mit ihren Werten aufzulisten.

Mit dem folgenden Beispiel werden die Serverkonfigurationsparameter für den Server **mydemoserver** in der Ressourcengruppe **myresourcegroup** aufgelistet.

```azurepowershell-interactive
Get-AzMySqlConfiguration -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

Die Definition der einzelnen aufgeführten Parameter finden Sie in der MySQL-Referenz im Abschnitt [Server System Variables](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Anzeigen von Details zu Serverkonfigurationsparametern

Um Details zu einem bestimmten Konfigurationsparameter für einen Server anzuzeigen, führen Sie das Cmdlet `Get-AzMySqlConfiguration` aus und geben dabei den Parameter **Name** an.

Mit diesem Beispiel werden Details des Serverkonfigurationsparameters **slow\_query\_log** für den Server **mydemoserver** in der Ressourcengruppe **myresourcegroup** angezeigt.

```azurepowershell-interactive
Get-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Ändern des Werts von Serverkonfigurationsparametern

Sie können den Wert eines bestimmten Serverkonfigurationsparameters ändern und dadurch den zugrunde liegenden Konfigurationswert für die MySQL-Server-Engine aktualisieren. Zum Aktualisieren der Konfiguration verwenden Sie das Cmdlet `Update-AzMySqlConfiguration`.

So aktualisieren Sie den Serverkonfigurationsparameter **slow\_query\_log** für den Server **mydemoserver** in der Ressourcengruppe **myresourcegroup**

```azurepowershell-interactive
Update-AzMySqlConfiguration -Name slow_query_log -ResourceGroupName myresourcegroup -ServerName mydemoserver -Value On
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Automatische Speichervergrößerung für einen Azure Database for MySQL-Server mithilfe von PowerShell](howto-auto-grow-storage-powershell.md)
