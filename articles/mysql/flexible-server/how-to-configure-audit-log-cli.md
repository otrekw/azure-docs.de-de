---
title: 'Konfigurieren von Überwachungsprotokollen mit der Azure CLI: Azure Database for MySQL: Flexibler Server'
description: In diesem Artikel erfahren Sie, wie Sie über die Azure CLI die Überwachungsprotokolle in Azure Database for MySQL Flexible Server konfigurieren und auf die Protokolle zugreifen.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 03/30/2021
ms.openlocfilehash: 9a633e9201c78bda0ff2c88abd6a5dd2e67acdf3
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108738861"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql---flexible-server-using-the-azure-cli"></a>Konfigurieren von Überwachungsprotokollen für Azure Database for MySQL Flexible Server und Zugreifen auf diese Protokolle über die Azure CLI

> [!IMPORTANT]
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

In diesem Artikel erfahren Sie, wie Sie [Überwachungsprotokolle](concepts-audit-logs.md) für Ihren flexiblen MySQL-Server über die Azure CLI konfigurieren.

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

- Erstellen Sie mithilfe des Befehls ```az mysql flexible-server create``` eine MySQL Flexible Server-Instanz, sofern Sie noch keine erstellt haben.

    ```azurecli
    az mysql flexible-server create --resource-group myresourcegroup --name myservername
    ```

## <a name="configure-audit-logging"></a>Konfigurieren der Überwachungsprotokollierung

>[!IMPORTANT]
> Es wird empfohlen, nur die Ereignistypen und Benutzer zu protokollieren, die für Ihre Überwachungszwecke erforderlich sind, um sicherzustellen, dass die Leistung Ihres Servers nicht stark beeinträchtigt wird.

Aktivieren und konfigurieren Sie die Überwachungsprotokollierung.

```azurecli
# Enable audit logs
az mysql flexible-server parameter set \
--name audit_log_enabled \
--resource-group myresourcegroup \
--server-name mydemoserver \
--value ON
```

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich über [Überwachungsprotokolle](concepts-audit-logs.md).
