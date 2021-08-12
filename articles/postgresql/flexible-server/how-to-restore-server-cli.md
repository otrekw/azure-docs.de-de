---
title: Wiederherstellen von Azure Database for PostgreSQL – Flexible Server mit Azure CLI
description: In diesem Artikel wird beschrieben, wie Sie Wiederherstellungsvorgänge in Azure Database for PostgreSQL mit der Azure CLI durchführen.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 05/29/2021
ms.openlocfilehash: f14bae80cad6fd9fc6ee534e8c8d1d9b6df617db
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026126"
---
# <a name="point-in-time-restore-of-a-azure-database-for-postgresql---flexible-server-with-azure-cli"></a>Zeitpunktwiederherstellung von Azure Database for PostgreSQL – Flexible Server mit Azure CLI


> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server ist derzeit in der öffentlichen Vorschau verfügbar.

In diesem Artikel wird Schritt für Schritt beschrieben, wie Sie Zeitpunktwiederherstellungen in einer Flexible Server-Instanz mithilfe von Sicherungen durchführen.

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

## <a name="restore-a-server-from-backup-to-a-new-server"></a>Wiederherstellen eines Servers aus einer Sicherung auf einem neuen Server

Sie können den folgenden Befehl ausführen, um einen Server in einer frühesten vorhandenen Sicherung wiederherzustellen.

**Verwendung**
```azurecli
az postgres flexible-server restore --restore-time
                                 --source-server
                                 [--ids]
                                 [--location]
                                 [--name]
                                 [--no-wait]
                                 [--resource-group]
                                 [--subscription]
```

**Beispiel**: Stellen Sie einen Server aus dieser ```2021-03-03T13:10:00Z```-Sicherungsmomentaufnahme wieder her.

```azurecli
az postgres server restore \
--name mydemoserver-restored \
--resource-group myresourcegroup \
--restore-point-in-time "2021-05-05T13:10:00Z" \
--source-server mydemoserver
```

Die Dauer der Wiederherstellung hängt von der Größe der auf dem Server gespeicherten Daten ab.

## <a name="perform-post-restore-tasks"></a>Durchführen der Aufgaben nach der Wiederherstellung
Nachdem die Wiederherstellung abgeschlossen ist, sollten Sie die folgenden Aufgaben durchführen, um Ihre Benutzer und Anwendungen wieder in den betriebsbereiten Zustand zu versetzen:

- Umleiten von Clients und Clientanwendungen an den neuen Server, wenn der neue Server den ursprünglichen Server ersetzen soll
- Stellen Sie sicher, dass geeignete VNET-Regeln vorhanden sind, damit Benutzer eine Verbindung herstellen können. Diese Regeln werden nicht vom ursprünglichen Server kopiert.
- Sicherstellen, dass geeignete Anmeldungen und Berechtigungen auf Datenbankebene vorhanden sind
- Konfigurieren von Warnungen nach Bedarf für den neuen Wiederherstellungsserver

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur [Geschäftskontinuität](concepts-business-continuity.md)
* Weitere Informationen zu [Sicherung und Wiederherstellung](concepts-backup-restore.md)  

