---
title: Konfigurieren von Parametern – Azure Database for PostgreSQL (Flexibler Server)
description: In diesem Artikel wird beschrieben, wie Sie mit der Azure-Befehlszeilenschnittstelle die Postgres-Parameter in Azure Database for PostgreSQL (Flexibler Server) konfigurieren.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.devlang: azurecli
ms.topic: how-to
ms.date: 9/22/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 315e4439ca66a3fbadac228c013797f516bc2940
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105605763"
---
# <a name="customize-server-parameters-for-azure-database-for-postgresql---flexible-server-using-azure-cli"></a>Anpassen der Serverparameter für Azure Database for PostgreSQL (Flexibler Server) mithilfe der Azure-Befehlszeilenschnittstelle

Sie können Konfigurationsparameter für einen Azure PostgreSQL-Server mithilfe der Befehlszeilenschnittstelle (Azure CLI) auflisten, anzeigen und aktualisieren. Auf Serverebene ist eine Teilmenge der Engine-Parameter verfügbar und kann geändert werden. 

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Erstellen Sie einen Azure Database for PostgreSQL-Server und eine -Datenbank gemäß [Erstellen einer Azure Database for PostgreSQL](quickstart-create-server-cli.md).
- Installieren Sie die Befehlszeilenschnittstelle [Azure CLI](/cli/azure/install-azure-cli) auf Ihrem Computer, oder verwenden Sie [Azure Cloud Shell](../../cloud-shell/overview.md) im Azure-Portal über Ihren Browser.

## <a name="list-server-parameters-for-a-flexible-server"></a>Auflisten von Serverparametern für einen flexiblen Server

Um alle änderbaren Parameter in einem Server mit ihren Werten aufzulisten, führen Sie den Befehl [az postgres flexible-server parameter list](/cli/azure/postgres/flexible-server/parameter) aus.

Sie können die Serverparameter für den Server **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup** auflisten.

```azurecli-interactive
az postgres flexible-server parameter list --resource-group myresourcegroup --server-name mydemoserver
```

## <a name="show-server-parameter-details"></a>Anzeigen von Serverparameterdetails

Um Details zu einem bestimmten Parameter für einen Server anzuzeigen, führen Sie den Befehl [az postgres flexible-server parameter show](/cli/azure/postgres/flexible-server/parameter) aus.

Dieses Beispiel zeigt Details des Serverparameters **log\_min\_messages** für den Server **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup**.

```azurecli-interactive
az postgres flexible-server parameter show --name log_min_messages --resource-group myresourcegroup --server-name mydemoserver
```

## <a name="modify-server-parameter-value"></a>Ändern des Serverparameterwerts

Sie können auch den Wert eines bestimmten Serverparameters ändern und dadurch den zugrunde liegenden Konfigurationswert für die PostgreSQL-Server-Engine aktualisieren. Um den Parameter zu aktualisieren, verwenden Sie den Befehl [az postgres flexible-server parameter set](/cli/azure/postgres/flexible-server/parameter). 

Aktualisieren Sie den Serverparameter **log\_min\_messages** des Servers **mydemoserver.postgres.database.azure.com** in der Ressourcengruppe **myresourcegroup**.

```azurecli-interactive
az postgres flexible-server parameter set --name log_min_messages --value INFO --resource-group myresourcegroup --server-name mydemoserver
```

Wenn Sie den Wert eines Parameters zurücksetzen möchten, lassen Sie einfach den optionalen Parameter `--value` weg. Der Dienst wendet dann den Standardwert an. Im obigen Beispiel könnte dies so aussehen:

```azurecli-interactive
az postgres flexible-server parameter set --name log_min_messages --resource-group myresourcegroup --server-name mydemoserver
```

Mit diesem Befehl wird der Parameter **log\_min\_messages** auf den Standardwert **WARNING** zurückgesetzt. Weitere Informationen zu Serverparametern und deren zulässigen Werten finden Sie in der PostgreSQL-Dokumentation zum [Festlegen von Parametern](https://www.postgresql.org/docs/12/config-setting.html).

## <a name="next-steps"></a>Nächste Schritte

- Informationen dazu, wie Sie Serverprotokolle konfigurieren und darauf zugreifen, finden Sie unter [Serverprotokolle in Azure Database for PostgreSQL](concepts-logging.md).
