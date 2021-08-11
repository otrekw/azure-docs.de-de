---
title: 'Beenden/Starten - Azure CLI - Azure Database for PostgreSQL: Flexible Server'
description: In diesem Artikel wird beschrieben, wie Sie Vorgänge in Azure Database for PostgreSQL über die Azure CLI beenden/starten.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 05/29/2021
ms.openlocfilehash: bebcc9e29620c8e739735553dd64e791c5bcdb00
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026508"
---
# <a name="stopstart-azure-database-for-postgresql---flexible-server-preview-using-azure-cli"></a>Beenden/Starten von Azure Database for PostgreSQL: Flexible Server (Vorschau) mithilfe der Azure CLI

> [!IMPORTANT]
> Azure Database for PostgreSQL: Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

In diesem Artikel erfahren Sie, wie Sie flexible Server mithilfe der Azure CLI neu starten, starten und beenden.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Installieren Sie die Azure CLI, oder upgraden Sie sie auf die neueste Version. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).
-  Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az_login) beim Azure-Konto an. Beachten Sie die Eigenschaft **id**, die auf die **Abonnement-ID** für Ihr Azure-Konto verweist.

    ```azurecli-interactive
    az login
    ````

- Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem Sie den Server mithilfe des Befehls ```az account set``` erstellen möchten.
`
    ```azurecli
    az account set --subscription <subscription id>
    ```

- Erstellen Sie mithilfe des Befehls ```az postgres flexible-server create``` eine PostgreSQL Flexible Server-Instanz, sofern Sie noch keine erstellt haben.

    ```azurecli
    az postgres flexible-server create --resource-group myresourcegroup --name myservername
    ```

## <a name="stop-a-running-server"></a>Beenden eines ausgeführten Servers
Um einen Server zu beenden, führen Sie den Befehl ```az postgres flexible-server stop``` aus. Wenn Sie einen [lokalen Kontext](/cli/azure/config/param-persist) verwenden, müssen Sie keine Argumente angeben.

**Syntax:**
```azurecli
az postgres flexible-server stop  [--name]
                               [--resource-group]
                               [--subscription]
```

**Beispiel ohne lokalen Kontext:**
```azurecli
az postgres flexible-server stop  --resource-group --name myservername
```

**Beispiel mit lokalem Kontext:**
```azurecli
az postgres flexible-server stop
```

## <a name="start-a-stopped-server"></a>Starten eines beendeten Servers
Um einen Server zu starten, führen Sie den Befehl ```az postgres flexible-server start``` aus. Wenn Sie einen [lokalen Kontext](/cli/azure/config/param-persist) verwenden, müssen Sie keine Argumente angeben.

**Syntax:**
```azurecli
az postgres flexible-server start [--name]
                               [--resource-group]
                               [--subscription]
```

**Beispiel ohne lokalen Kontext:**
```azurecli
az postgres flexible-server start  --resource-group --name myservername
```

**Beispiel mit lokalem Kontext:**
```azurecli
az postgres flexible-server start
```

> [!IMPORTANT]
> Sobald der Server erfolgreich neu gestartet wurde, sind alle Verwaltungsvorgänge für den flexiblen Server verfügbar.

## <a name="next-steps"></a>Nächste Schritte
- Hier erfahren Sie mehr über den [Neustart von Azure Database for PostgreSQL Flexible Server](./how-to-restart-server-cli.md).


