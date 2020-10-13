---
title: Beenden/Starten – Azure-Portal – Azure Database for MySQL-Server
description: In diesem Artikel wird beschrieben, wie Sie Vorgänge in Azure Database for MySQL beenden/starten.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 95be6aa576d9d059ce419443f8c7e32af5ff397a
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91826207"
---
# <a name="stopstart-an-azure-database-for-mysql"></a>Beenden/Starten einer Azure Database for MySQL-Instanz

> [!IMPORTANT]
> Die Funktion zum Beenden/Starten für Azure Database for MySQL befindet sich derzeit in der öffentlichen Vorschau.

Dieser Artikel enthält Schrittanleitungen zum Beenden und Starten des Einzelservers.

## <a name="prerequisites"></a>Voraussetzungen

Zum Durcharbeiten dieses Leitfadens benötigen Sie Folgendes:

-   Sie benötigen einen Azure Database for MySQL-Einzelserver.

> [!NOTE]
> Beachten Sie die Einschränkung beim [Beenden/Starten](concepts-servers.md#limitations-of-stopstart-operation).

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-azure-portal"></a>Beenden/Starten einer Azure Database for MySQL-Instanz mithilfe des Azure-Portals

### <a name="stop-a-running-server"></a>Beenden eines ausgeführten Servers

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) den MySQL-Server aus, den Sie beenden möchten.

2.  Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Beenden**.

    :::image type="content" source="./media/howto-stop-start-server/mysql-stop-server.png" alt-text="Beenden eines Azure Database for MySQL-Servers":::

    > [!NOTE]
    > Nachdem der Server beendet wurde, sind keine anderen Verwaltungsvorgänge für den Einzelserver mehr verfügbar.

### <a name="start-a-stopped-server"></a>Starten eines beendeten Servers

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) den Einzelserver aus, den Sie starten möchten.

2.  Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Starten**.

    :::image type="content" source="./media/howto-stop-start-server/mysql-start-server.png" alt-text="Beenden eines Azure Database for MySQL-Servers":::

    > [!NOTE]
    > Nachdem der Server gestartet wurde, sind alle anderen Verwaltungsvorgänge für den Einzelserver wieder verfügbar.

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-cli"></a>Beenden/Starten einer Azure Database for MySQL-Instanz mithilfe der Befehlszeilenschnittstelle

### <a name="stop-a-running-server"></a>Beenden eines ausgeführten Servers

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) den MySQL-Server aus, den Sie beenden möchten.

2.  Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Beenden**.

    ```azurecli-interactive
    az mysql server stop --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Nachdem der Server beendet wurde, sind keine anderen Verwaltungsvorgänge für den Einzelserver mehr verfügbar.

### <a name="start-a-stopped-server"></a>Starten eines beendeten Servers

1.  Wählen Sie im [Azure-Portal](https://portal.azure.com/) den Einzelserver aus, den Sie starten möchten.

2.  Klicken Sie auf der Seite **Übersicht** in der Symbolleiste auf die Schaltfläche **Starten**.

    ```azurecli-interactive
    az mysql server start --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Nachdem der Server gestartet wurde, sind alle anderen Verwaltungsvorgänge für den Einzelserver wieder verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).
