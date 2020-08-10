---
title: 'Generieren einer Verbindungszeichenfolge mit PowerShell: Azure Database for MySQL'
description: Diese Schnellstartanleitung enthält ein Azure PowerShell-Beispiel zum Generieren einer Verbindungszeichenfolge für die Verbindungsherstellung mit Azure Database for MySQL.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, devx-track-azurepowershell
ms.topic: quickstart
ms.date: 8/3/2020
ms.openlocfilehash: 9de569e09e1b8bcf29f47077cbe15f0a6c40ddd1
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2020
ms.locfileid: "87544917"
---
# <a name="azure-database-for-mysql-generate-a-connection-string-with-powershell"></a>Azure Database for MySQL: Generieren einer Verbindungszeichenfolge mit PowerShell

In diesem Artikel wird veranschaulicht, wie Sie eine Verbindungszeichenfolge für einen Azure Database for MySQL-Server generieren. Mit einer Verbindungszeichenfolge können Sie über zahlreiche verschiedene Anwendungen eine Verbindung mit einer Azure Database for MySQL-Instanz herstellen.

## <a name="requirements"></a>Requirements (Anforderungen)

In diesem Artikel werden die Ressourcen, die in der folgenden Anleitung erstellt wurden, als Ausgangspunkt verwendet:

* [Schnellstart: Erstellen eines Azure Database for MySQL-Servers mithilfe von PowerShell](quickstart-create-mysql-server-database-using-azure-powershell.md)

## <a name="get-the-connection-string"></a>Abrufen der Verbindungszeichenfolge

Mit dem Cmdlet `Get-AzMySqlConnectionString` wird eine Verbindungszeichenfolge zum Herstellen einer Verbindung mit Azure Database for MySQL generiert. Im folgenden Beispiel wird die Verbindungszeichenfolge für einen PHP-Client von **mydemoserver** zurückgegeben.

```azurepowershell-interactive
Get-AzMySqlConnectionString -Client PHP -Name mydemoserver -ResourceGroupName myresourcegroup
```

```Output
$con=mysqli_init();mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "mydemoserver.mysql.database.azure.com", "myadmin@mydemoserver", {your_password}, {your_database}, 3306);
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
> [Entwerfen einer Azure Database for MySQL-Instanz mithilfe von PowerShell](tutorial-design-database-using-powershell.md)
