---
title: Verwalten von Servern für Azure Database for PostgreSQL – Flexible Server mithilfe der Azure CLI
description: In diesem Artikel erfahren Sie, wie Sie Azure Database for PostgreSQL – Flexible Server über die Azure CLI verwalten.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 8e24dd6cb8a1fa90f1a6caf9117ab3c344c00b12
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913873"
---
# <a name="manage-an-azure-database-for-postgresql---flexible-server-using-the-azure-cli"></a>Verwalten von Azure Database for PostgreSQL – Flexible Server mithilfe der Azure CLI

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

In diesem Artikel erfahren Sie, wie Sie Ihre in Azure bereitgestellten flexiblen Server verwalten. Zu den Verwaltungsaufgaben gehören die Compute- und Speicherskalierung, das Zurücksetzen des Administratorkennworts und das Anzeigen von Serverdetails.

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen. Für den Artikel müssen Sie mindestens Version 2.0 der Azure-Befehlszeilenschnittstelle lokal ausführen. Führen Sie den Befehl `az --version` aus, um die installierte Version anzuzeigen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Sie müssen sich mithilfe des Befehls [az login](/cli/azure/reference-index#az-login) bei Ihrem Konto anmelden. Beachten Sie die Eigenschaft **id** , die auf die **Abonnement-ID** für Ihr Azure-Konto verweist.

```azurecli-interactive
az login
```

Wählen Sie mithilfe des Befehls [az account set](/cli/azure/account) das Abonnement unter Ihrem Konto aus. Notieren Sie sich aus der Ausgabe von **az login** den Wert für **id**. Sie verwenden ihn im Befehl als Wert für das Argument **subscription**. Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource fakturiert sein sollte. Verwenden Sie [az account list](/cli/azure/account#az-account-list), um alle Abonnements abzurufen.

```azurecli
az account set --subscription <subscription id>
```

> [!Important]
> Wenn Sie noch keinen flexiblen Server erstellt haben, erstellen Sie nun einen, um mit dieser exemplarischen Vorgehensweise zu beginnen.

## <a name="scale-compute-and-storage"></a>Skalieren von Compute und Speicher

Mithilfe des folgenden Befehls können Sie Ihren Computetarif, Ihre virtuelle Kerne und Ihren Speicher mühelos hochskalieren. Sie können den gesamten Servervorgang anzeigen, indem Sie den Befehl [az postgres flexible-server server overview](https://docs.microsoft.com/cli/azure/postgres/flexible-server) ausführen.

```azurecli-interactive
az postgres flexible-server update --resource-group myresourcegroup --name mydemoserver --sku-name Standard_D4ds_v3 --storage-size 6144
```

Hier sind die Details zu den obigen Argumenten aufgeführt:

**Einstellung** | **Beispielwert** | **Beschreibung**
---|---|---
name | mydemoserver | Geben Sie einen eindeutigen Namen für Ihren Server ein. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Es muss zwischen drei und 63 Zeichen lang sein.
resource-group | myresourcegroup | Geben Sie den Namen der Azure-Ressourcengruppe an.
sku-name|Standard_D4ds_v3|Geben Sie den Namen des Computetarifs und der Computegröße ein. Unterliegt der Konvention Standard_{VM size} (Kurzform) Weitere Informationen finden Sie unter [Azure Database for MySQL – Tarife](../concepts-pricing-tiers.md).
storage-size | 6.144 | Die Speicherkapazität des Servers (Einheit: MB). Mindestens 5.120 und erhöht sich in Inkrementen von 1.024.

> [!IMPORTANT]
> Der Speicher kann nicht herunterskaliert werden. 

## <a name="manage-postgresql-databases-on-a-server"></a>Verwalten von PostgreSQL-Datenbanken auf einem Server

Es gibt verschiedene Anwendungen, mit denen Sie eine Verbindung mit Ihrem Azure-Datenbank für PostgreSQL-Server herstellen können. Wenn auf Ihrem Clientcomputer PostgreSQL installiert ist, können Sie mit einer lokalen Instanz von [psql](https://www.postgresql.org/docs/current/static/app-psql.html) eine Verbindung mit einer Azure-PostgreSQL-Serverinstanz herstellen. Wir stellen jetzt mit dem Befehlszeilen-Hilfsprogramm psql eine Verbindung mit dem Azure-PostgreSQL-Server her.

1. Führen Sie den folgenden psql-Befehl aus, um für eine PostgreSQL-Serverinstanz eine Verbindung mit einer Azure-Datenbank herzustellen:

   ```bash
   psql --host=<servername> --port=<port> --username=<user> --dbname=<dbname>
   ```

   Mit dem folgenden Befehl wird beispielsweise mit den Zugriffsanmeldeinformationen eine Verbindung mit der Standarddatenbank **postgres** auf Ihrem PostgreSQL-Server **mydemoserver.postgres.database.azure.com** hergestellt. Geben Sie das `<server_admin_password>` ein, das Sie bei der Aufforderung zur Kennworteingabe ausgewählt haben.
  
   ```bash
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin --dbname=postgres
   ```

   Nachdem die Verbindung hergestellt wurde, wird vom psql-Hilfsprogramm eine PostgreSQL-Eingabeaufforderung für die Eingabe von SQL-Befehlen angezeigt. In der Ausgabe der Erstverbindung wird unter Umständen eine Warnung angezeigt, wenn die verwendete Version des psql-Programms von der Version des Azure Database for PostgreSQL-Servers abweicht.

   psql-Beispielausgabe:

   ```bash
   psql (11.3, server 12.1)
   WARNING: psql major version 11, server major version 12.
            Some psql features might not work.
   SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
   Type "help" for help.

   postgres=>
   ```

   > [!TIP]
   > Wenn die Firewall nicht zum Zulassen der IP-Adresse Ihres Clients konfiguriert ist, tritt der folgende Fehler auf:
   >
   > "psql: FATAL:  no pg_hba.conf entry for host `<IP address>`, user "myadmin", database "postgres", SSL on FATAL: SSL connection is required. Specify SSL options and retry.
   >
   > Vergewissern Sie sich, dass die IP des Clients im oben genannten Schritt die Firewall-Regeln zulässig ist.

2. Erstellen Sie eine leere Datenbank namens „postgresdb“ über die Eingabeaufforderung, indem Sie den folgenden Befehl eingeben:

    ```bash
    CREATE DATABASE postgresdb;
    ```

3. Fügen Sie den folgenden Befehl über die Eingabeaufforderung aus, um zur Verbindung zur neu erstellten Datenbank **postgresdb** zu wechseln:

    ```bash
    \c postgresdb
    ```

4. Geben Sie `\q` ein, und drücken Sie anschließend die EINGABETASTE, um psql zu beenden.

Sie haben über psql eine Verbindung mit dem Azure Database for PostgreSQL-Server hergestellt und eine leere Benutzerdatenbank erstellt.

## <a name="reset-admin-password"></a>Zurücksetzen des Administratorkennworts
Mit diesem Befehl können Sie das Kennwort für die Administratorrolle ändern.
```azurecli-interactive
az postgres flexible-server update --resource-group myresourcegroup --name mydemoserver --admin-password <new-password>
```

> [!IMPORTANT]
> Stellen Sie sicher, dass das Kennwort aus mindestens 8 Zeichen und maximal 128 Zeichen besteht.
> Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen.

## <a name="delete-a-server"></a>Löschen eines Servers

Wenn Sie nur den flexiblen PostgreSQL-Server löschen möchten, können Sie den Befehl [az postgres flexible-server delete](https://docs.microsoft.com/cli/azure/postgres/flexible-server#az-PostgreSQL-flexible-server-delete) ausführen.

```azurecli-interactive
az postgres flexible-server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Nächste Schritte

- [Grundlegendes zu Sicherungs- und Wiederherstellungskonzepten](concepts-backup-restore.md)
- [Optimieren und Überwachen des Servers](concepts-monitoring.md)