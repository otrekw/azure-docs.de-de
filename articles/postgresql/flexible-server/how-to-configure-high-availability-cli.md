---
title: Verwalten zonenredundanter Hochverfügbarkeit – Azure CLI – Azure Database for PostgreSQL Flexible Server
description: In diesem Artikel wird beschrieben, wie Sie die zonenredundante Hochverfügbarkeit in Azure Database for PostgreSQL Flexible Server mit Azure-CLI konfigurieren.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 05/29/2021
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: b9d4a5faccd228e4eba25b6818b3dbcc0df5402a
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026082"
---
# <a name="manage-zone-redundant-high-availability-in-azure-database-for-postgresql-flexible-server-with-azure-cli"></a>Verwalten von zonenredundanter Hochverfügbarkeit für Azure Database for PostgreSQL Flexible Server mit Azure CLI

> [!NOTE]
> Azure Database for PostgreSQL Flexible Server befindet sich in der öffentlichen Vorschau. 

Der Artikel beschreibt, wie Sie die zonenredundante Hochverfügbarkeitskonfiguration zum Zeitpunkt der Servererstellung in Ihrem flexiblen Server aktivieren oder deaktivieren können. Sie können die zonenredundante Hochverfügbarkeit auch nach der Servererstellung deaktivieren. Das Aktivieren zonenredundanten Hochverfügbarkeit nach der Servererstellung wird nicht unterstützt.

Die Hochverfügbarkeitsfunktion stellt ein physisch getrenntes primäres und Standbyreplikat in unterschiedlichen Zonen bereit. Weitere Informationen finden Sie unter [Dokumentation zu Hochverfügbarkeitskonzepten](./concepts/../concepts-high-availability.md). Durch die Aktivierung/Deaktivierung der Hochverfügbarkeit werden keine Ihrer anderen Einstellungen geändert, einschließlich der VNET-Konfiguration, der Firewalleinstellungen und der Sicherungsaufbewahrung. Das Deaktivieren der Hochverfügbarkeit wirkt sich nicht auf die Konnektivität und die Vorgänge Ihrer Anwendung aus.

> [!IMPORTANT]
> Eine Liste der Regionen, die zonenredundante Hochverfügbarkeit unterstützen, finden Sie [hier](./overview.md#azure-regions). 

## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Installieren Sie die Azure CLI, oder upgraden Sie sie auf die neueste Version. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).
-  Melden Sie sich mit dem Befehl [az login](/cli/azure/reference-index#az_login) beim Azure-Konto an. Beachten Sie die Eigenschaft **id**, die auf die **Abonnement-ID** für Ihr Azure-Konto verweist.

    ```azurecli-interactive
    az login
    ````

- Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem Sie den Server mit dem ```az account set```-Befehl erstellen.
`
    ```azurecli
    az account set --subscription <subscription id>
    ```

## <a name="enable-high-availability-during-server-creation"></a>Aktivieren von Hochverfügbarkeit während der Servererstellung
Sie können Server nur mit den Tarifen „Universell“ oder „Arbeitsspeicheroptimiert“ mit Hochverfügbarkeit erstellen. Sie können Hochverfügbarkeit für einen Server nur während der Erstellung aktivieren.

**Syntax:**

```azurecli
az postgres flexible-server create [--high-availability {Disabled, Enabled}]
                                [--resource-group]
                                [--name]
```

**Beispiel:**
```azurecli
az postgres flexible-server create --name myservername --sku-name Standard-D2ds_v4 --resource-group myresourcegroup --high-availability Enabled
```

## <a name="disable-high-availability"></a>Deaktivieren der Hochverfügbarkeit

Sie können die Hochverfügbarkeit mit dem Befehl [az postgres flexible-server update](/cli/azure/postgres/flexible-server#az_postgres_flexible_server_update) deaktivieren. Beachten Sie, dass das Deaktivieren von Hochverfügbarkeit nur unterstützt wird, wenn der Server mit Hochverfügbarkeit konfiguriert wurde. 

```azurecli
az postgres flexible-server update [--high-availability {Disabled, Enabled}]
                                [--resource-group]
                                [--name]
```

**Beispiel:**
```azurecli
az postgres flexible-server update --resource-group myresourcegroup --name myservername --high-availability Disabled
```


## <a name="next-steps"></a>Nächste Schritte

-   Weitere Informationen zur [Geschäftskontinuität](./concepts-business-continuity.md)
-   Weitere Informationen zur [zonenredundanten Hochverfügbarkeit](./concepts-high-availability.md)