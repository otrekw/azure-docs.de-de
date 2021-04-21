---
title: Verwalten eines Servers – Azure CLI – Azure Database for PostgreSQL
description: Hier erfahren Sie, wie Sie einen Azure Database for PostgreSQL-Server über die Azure-Befehlszeilenschnittstelle (Azure CLI) verwalten.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: how-to
ms.date: 9/22/2020
ms.openlocfilehash: 36151a9afda0bb23ee244ee778a30f30a41243ee
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107791461"
---
# <a name="manage-an-azure-database-for-postgresql-single-server-using-the-azure-cli"></a>Verwalten eines Azure Database for PostgreSQL-Einzelservers über die Azure CLI

In diesem Artikel erfahren Sie, wie Sie Ihre in Azure bereitgestellten Einzelserver verwalten. Zu den Verwaltungsaufgaben gehören die Compute- und Speicherskalierung, das Zurücksetzen des Administratorkennworts und das Anzeigen von Serverdetails.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Für den Artikel müssen Sie mindestens Version 2.0 der Azure-Befehlszeilenschnittstelle lokal ausführen. Führen Sie den Befehl `az --version` aus, um die installierte Version anzuzeigen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Sie müssen sich mithilfe des Befehls [az login](/cli/azure/reference-index#az_login) bei Ihrem Konto anmelden. Beachten Sie die Eigenschaft **id**, die auf die **Abonnement-ID** für Ihr Azure-Konto verweist.

```azurecli-interactive
az login
```

Wählen Sie mithilfe des Befehls [az account set](/cli/azure/account) das Abonnement unter Ihrem Konto aus. Notieren Sie sich aus der Ausgabe von **az login** den Wert für **id**. Sie verwenden ihn im Befehl als Wert für das Argument **subscription**. Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource fakturiert sein sollte. Verwenden Sie [az account list](/cli/azure/account#az_account_list), um alle Abonnements abzurufen.

```azurecli
az account set --subscription <subscription id>
```

Wenn Sie noch keinen Server erstellt haben, lesen Sie diesen [Schnellstart](quickstart-create-server-database-azure-cli.md), um einen zu erstellen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="scale-compute-and-storage"></a>Skalieren von Compute und Speicher

Mit dem folgenden Befehl können Sie Ihren Tarif sowie Compute und Speicher problemlos hochskalieren. Unter [az postgres server overview](/cli/azure/mysql/server) können Sie einen Überblick über den gesamten Servervorgang anzeigen, den Sie durchführen können.

```azurecli-interactive
az postgres server update --resource-group myresourcegroup --name mydemoserver --sku-name GP_Gen5_4 --storage-size 6144
```

Hier sind die Details zu den obigen Argumenten aufgeführt:

**Einstellung** | **Beispielwert** | **Beschreibung**
---|---|---
name | mydemoserver | Geben Sie einen eindeutigen Namen für Ihren Azure Database for PostgreSQL-Server ein. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Es muss zwischen drei und 63 Zeichen lang sein.
resource-group | myresourcegroup | Geben Sie den Namen der Azure-Ressourcengruppe an.
sku-name|GP_Gen5_2|Geben Sie den Namen des Tarifs und der Computekonfiguration ein. Folgt der Konvention „{Tarif} _{Computegeneration}_ {virtuelle Kerne}“ in Kurzform. Weitere Informationen finden Sie unter [Azure Database for MySQL – Tarife](./concepts-pricing-tiers.md).
storage-size | 6.144 | Die Speicherkapazität des Servers (Einheit: MB). Mindestens 5.120 und erhöht sich in Inkrementen von 1.024.

> [!Important]
> - Der Speicher kann hochskaliert werden (ein Herunterskalieren ist jedoch nicht möglich).
> - Das Hochskalieren von „Basic“ zum Tarif „Universell“ oder „Arbeitsspeicheroptimiert“ wird nicht unterstützt. Sie können [mithilfe eines Bash-Skripts](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/upgrade-from-basic-to-general-purpose-or-memory-optimized-tiers/ba-p/830404) oder [mithilfe von PostgreSQL Workbench](https://techcommunity.microsoft.com/t5/azure-database-support-blog/how-to-scale-up-azure-database-for-mysql-from-basic-tier-to/ba-p/369134) manuell hochskalieren.


## <a name="manage-postgresql-databases-on-a-server"></a>Verwalten Sie PostgreSQL-Datenbanken auf einem Server.
Sie können die Eigenschaften einer Datenbank auf Ihrem Server mit einem der folgenden Befehle erstellen, löschen, auflisten und anzeigen:

| Cmdlet | Verwendung| BESCHREIBUNG |
| --- | ---| --- |
|[az postgres db create](/cli/azure/sql/db#az_mysql_db_create)|```az postgres db create -g myresourcegroup -s mydemoserver -n mydatabasename``` |Erstellt eine Datenbank|
|[az postgres db delete](/cli/azure/sql/db#az_mysql_db_delete)|```az postgres db delete -g myresourcegroup -s mydemoserver -n mydatabasename```|Löscht eine Datenbank von Ihrem Server. Mit diesem Befehl wird der Server nicht gelöscht. |
|[az postgres db list](/cli/azure/sql/db#az_mysql_db_list)|```az postgres db list -g myresourcegroup -s mydemoserver```|Listet alle Datenbanken auf dem Server auf.|
|[az postgres db show](/cli/azure/sql/db#az_mysql_db_show)|```az postgres db show -g myresourcegroup -s mydemoserver -n mydatabasename```|Zeigt weitere Details der Datenbank an.|

## <a name="update-admin-password"></a>Aktualisieren des Administratorkennworts
Mit diesem Befehl können Sie das Kennwort für die Administratorrolle ändern.
```azurecli-interactive
az postgres server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!Important]
>  Stellen Sie sicher, dass das Kennwort aus mindestens 8 Zeichen und maximal 128 Zeichen besteht.
> Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen.

## <a name="delete-a-server"></a>Löschen eines Servers
Wenn Sie nur den PostgreSQL-Einzelserver löschen möchten, können Sie den Befehl [az postgres server delete](/cli/azure/mysql/server#az_mysql_server_delete) ausführen.

```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Nächste Schritte
- [Neustarten eines Servers](howto-restart-server-cli.md)
- [Wiederherstellen eines Servers in einem fehlerhaften Zustand](howto-restore-server-cli.md)
- [Überwachen und Optimieren des Servers](concepts-monitoring.md)
