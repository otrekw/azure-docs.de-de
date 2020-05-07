---
title: Zugreifen auf Überwachungsprotokolle – Azure CLI – Azure Database for MariaDB
description: In diesem Artikel wird beschrieben, wie Sie über die Azure CLI die Überwachungsprotokolle in Azure Database for MariaDB konfigurieren und darauf zugreifen.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/13/2020
ms.openlocfilehash: e9716f0fa8e0ae44d614bbb28ed6846105e683d6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81384127"
---
# <a name="configure-and-access-audit-logs-in-the-azure-cli"></a>Konfigurieren von und Zugreifen auf Überwachungsprotokolle über die Azure CLI

Sie können die [Überwachungsprotokolle in Azure Database for MariaDB](concepts-audit-logs.md) über die Azure CLI konfigurieren.

> [!IMPORTANT]
> Diese Überwachungsprotokollfunktion ist derzeit in der Vorschauphase.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:

- [Azure Database for MariaDB-Server](quickstart-create-mariadb-server-database-using-azure-portal.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Diese Anleitung setzt die Verwendung von Azure CLI-Version 2.0 oder höher voraus. Geben Sie zum Bestätigen der Version an der Eingabeaufforderung von Azure CLI `az --version` ein. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="configure-audit-logging"></a>Konfigurieren der Überwachungsprotokollierung

Aktivieren und konfigurieren Sie die Überwachungsprotokollierung mit den folgenden Schritten: 

1. Aktivieren Sie Überwachungsprotokolle, indem Sie den Parameter **audit_logs_enabled** auf „ON“ festlegen. 
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_enabled --resource-group myresourcegroup --server mydemoserver --value ON
    ```

1. Wählen Sie die zu protokollierenden [Ereignistypen](concepts-audit-logs.md#configure-audit-logging) aus, indem Sie den Parameter **audit_log_events** aktualisieren.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_events --resource-group myresourcegroup --server mydemoserver --value "ADMIN,CONNECTION"
    ```

1. Fügen Sie alle MariaDB-Benutzer hinzu, die von der Protokollierung ausgeschlossen werden sollen, indem Sie den Parameter **audit_log_exclude_users** aktualisieren. Geben Sie Benutzer an, indem Sie ihre MariaDB-Benutzernamen bereitstellen.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_exclude_users --resource-group myresourcegroup --server mydemoserver --value "azure_superuser"
    ```

1. Fügen Sie alle MariaDB-Benutzer hinzu, die in die Protokollierung einbezogen werden sollen, indem Sie den Parameter **audit_log_exclude_users** aktualisieren. Geben Sie Benutzer an, indem Sie ihre MariaDB-Benutzernamen bereitstellen.
    ```azurecli-interactive
    az mariadb server configuration set --name audit_log_include_users --resource-group myresourcegroup --server mydemoserver --value "sampleuser"
    ```

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Überwachungsprotokollen](concepts-audit-logs.md) in Azure Database for MariaDB.
- Informieren Sie sich, wie Überwachungsprotokolle im [Azure-Portal](howto-configure-audit-logs-portal.md) konfiguriert werden.