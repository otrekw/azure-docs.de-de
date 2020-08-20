---
title: 'Generieren einer Verbindungszeichenfolge mit PowerShell: Azure Database for PostgreSQL'
description: Dieser Artikel enthält ein Azure PowerShell-Beispiel zum Generieren einer Verbindungszeichenfolge für die Verbindungsherstellung mit Azure Database for PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/6/2020
ms.openlocfilehash: a8e82c1863fde7a7580300606949e00a34c4e58c
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87907605"
---
# <a name="how-to-generate-an-azure-database-for-postgresql-connection-string-with-powershell"></a>Generieren einer Azure Database for PostgreSQL-Verbindungszeichenfolge mit PowerShell

In diesem Artikel wird veranschaulicht, wie Sie eine Verbindungszeichenfolge für einen Azure Database for PostgreSQL-Server generieren. Mit einer Verbindungszeichenfolge können Sie über zahlreiche verschiedene Anwendungen eine Verbindung mit einer Azure Database for PostgreSQL-Instanz herstellen.

## <a name="requirements"></a>Requirements (Anforderungen)

In diesem Artikel werden die Ressourcen, die in der folgenden Anleitung erstellt wurden, als Ausgangspunkt verwendet:

* [Schnellstart: Erstellen eines Azure Database for PostgreSQL-Servers mithilfe von PowerShell](quickstart-create-postgresql-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Mit dem Cmdlet `Get-AzPostgreSqlConnectionString` wird eine Verbindungszeichenfolge zum Herstellen einer Verbindung zwischen Anwendungen und Azure Database for PostgreSQL generiert. Im folgenden Beispiel wird die Verbindungszeichenfolge für einen PHP-Client von **mydemoserver** zurückgegeben.

```azurepowershell-interactive
Get-AzPostgreSqlConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
host=mydemoserver.postgres.database.azure.com port=5432 dbname={your_database} user=myadmin@mydemoserver password={your_password} sslmode=require
```

Gültige Werte für den Parameter `Client` sind u. a.:

* ADO&#46;NET
* JDBC
* Node.js
* PHP
* Python
* Ruby
* WebApp

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Anpassen eines Azure Database for PostgreSQL-Serverparameters mithilfe von PowerShell](howto-configure-server-parameters-using-powershell.md)
