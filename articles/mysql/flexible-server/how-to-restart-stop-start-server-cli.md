---
title: Neu starten/Beenden/Starten – Azure-Portal – Azure Database for MySQL Flexible Server
description: In diesem Artikel wird beschrieben, wie Sie Vorgänge in Azure Database for MySQL über die Azure CLI neu starten/beenden/starten.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 03/30/2021
ms.openlocfilehash: cf4f053167c477fd68c8c56f6f03ee15b7877638
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108738732"
---
# <a name="restartstopstart-an-azure-database-for-mysql---flexible-server-preview"></a>Neu starten/Beenden/Starten einer Instanz von Azure Database for MySQL – Flexible Server (Vorschau)

> [!IMPORTANT]
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

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

- Erstellen Sie über den Befehl ```az mysql flexible-server create``` eine MySQL Flexible Server-Instanz, falls Sie noch keine erstellt haben.

    ```azurecli
    az mysql flexible-server create --resource-group myresourcegroup --name myservername
    ```

## <a name="stop-a-running-server"></a>Beenden eines ausgeführten Servers
Um einen Server zu beenden, führen Sie den Befehl ```az mysql flexible-server stop``` aus. Wenn Sie einen [lokalen Kontext](/cli/azure/config/param-persist) verwenden, müssen Sie keine Argumente angeben.

**Syntax:**
```azurecli
az mysql flexible-server stop  [--name]
                               [--resource-group]
                               [--subscription]
```

**Beispiel ohne lokalen Kontext:**
```azurecli
az mysql flexible-server stop  --resource-group --name myservername
```

**Beispiel mit lokalem Kontext:**
```azurecli
az mysql flexible-server stop
```

## <a name="start-a-stopped-server"></a>Starten eines beendeten Servers
Um einen Server zu starten, führen Sie den Befehl ```az mysql flexible-server start``` aus. Wenn Sie einen [lokalen Kontext](/cli/azure/config/param-persist) verwenden, müssen Sie keine Argumente angeben.

**Syntax:**
```azurecli
az mysql flexible-server start [--name]
                               [--resource-group]
                               [--subscription]
```

**Beispiel ohne lokalen Kontext:**
```azurecli
az mysql flexible-server start  --resource-group --name myservername
```

**Beispiel mit lokalem Kontext:**
```azurecli
az mysql flexible-server start
```

> [!IMPORTANT]
> Sobald der Server erfolgreich neu gestartet wurde, sind alle Verwaltungsvorgänge für den flexiblen Server verfügbar.

## <a name="restart-a-server"></a>Neustarten eines Servers
Um einen Server neu zu starten, führen Sie den Befehl ```az mysql flexible-server restart``` aus. Wenn Sie einen [lokalen Kontext](/cli/azure/config/param-persist) verwenden, müssen Sie keine Argumente angeben.

**Syntax:**
```azurecli
az mysql flexible-server restart [--name]
                                 [--resource-group]
                                 [--subscription]
```

**Beispiel ohne lokalen Kontext:**
```azurecli
az mysql flexible-server restart  --resource-group --name myservername
```

**Beispiel mit lokalem Kontext:**
```azurecli
az mysql flexible-server restart
```


> [!IMPORTANT]
> Sobald der Server erfolgreich neu gestartet wurde, sind alle Verwaltungsvorgänge für den flexiblen Server verfügbar.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Netzwerke in Azure Database for MySQL Flexible Server](./concepts-networking.md).
- [Erstellen und Verwalten von virtuellen Netzwerken für Azure Database for MySQL Flexible Server mit dem Azure-Portal](./how-to-manage-virtual-network-portal.md).

