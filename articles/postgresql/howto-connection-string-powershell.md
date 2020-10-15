---
title: 'Generieren einer Verbindungszeichenfolge mit PowerShell: Azure Database for PostgreSQL'
description: Dieser Artikel enthält ein Azure PowerShell-Beispiel zum Generieren einer Verbindungszeichenfolge für die Verbindungsherstellung mit Azure Database for PostgreSQL.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.custom: mvc, devx-track-azurepowershell
ms.topic: how-to
ms.date: 8/6/2020
ms.openlocfilehash: 29641bdc3696f0722ca430dde912768071fd9b16
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91708168"
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
