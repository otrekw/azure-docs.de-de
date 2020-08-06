---
title: 'Schnellstart: Erstellen eines Servers – Azure CLI – Azure Database for PostgreSQL-Einzelserver'
description: Schnellstartanleitung zum Erstellen eines Azure Database for PostgreSQL-Einzelservers mit der Azure CLI (Befehlszeilenschnittstelle).
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 06/25/2020
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: cb88f085e18526a17621d3c4960a5aad6db727ad
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496592"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---single-server-using-the-azure-cli"></a>Schnellstart: Erstellen eines Azure Database for PostgreSQL-Einzelservers mit der Azure CLI

> [!TIP]
> Erwägen Sie die Verwendung eines vereinfachten [az postgres up](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up)-Azure CLI-Befehls (derzeit in der Vorschauphase). Probieren Sie den [Schnellstart](./quickstart-create-server-up-azure-cli.md) aus.

In dieser Schnellstartanleitung erfahren Sie, wie Sie mit den Befehlen der [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in [Azure Cloud Shell](https://shell.azure.com) in fünf Minuten einen Azure Database for PostgreSQL-Server erstellen.  Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

>[!Note]
>Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI]( https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

## <a name="prerequisites"></a>Voraussetzungen
Für den Artikel müssen Sie mindestens Version 2.0 der Azure-Befehlszeilenschnittstelle lokal ausführen. Führen Sie den Befehl `az --version` aus, um die installierte Version anzuzeigen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

Sie müssen sich mithilfe des Befehls [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) bei Ihrem Konto anmelden. Beachten Sie die Eigenschaft **id**, die auf die **Abonnement-ID** für Ihr Azure-Konto verweist. 

```azurecli-interactive
az login
```

Wählen Sie mithilfe des Befehls [az account set](/cli/azure/account) die Abonnement-ID unter Ihrem Konto aus. Notieren Sie sich aus der Ausgabe von **az login** den Wert für **id**. Sie verwenden ihn im Befehl als Wert für das Argument **subscription**. Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource fakturiert sein sollte. Verwenden Sie [az account list](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az-account-list), um alle Abonnements abzurufen.

```azurecli
az account set --subscription <subscription id>
```
## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Erstellen Sie mithilfe des Befehls [az group create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) eine [Azure-Ressourcengruppe](../azure-resource-manager/management/overview.md) und anschließend in dieser Ressourcengruppe Ihren PostgreSQL-Server. Geben Sie daher einen eindeutigen Namen an. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `myresourcegroup` am Standort `westus` erstellt.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

Erstellen Sie mit dem Befehl [az postgres server create](/cli/azure/postgres/server) eine [Azure-Datenbank für PostgreSQL-Server](overview.md). Ein Server kann mehrere Datenbanken enthalten.

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 
```
Hier sind die Details zu den obigen Argumenten aufgeführt: 

**Einstellung** | **Beispielwert** | **Beschreibung**
---|---|---
name | mydemoserver | Wählen Sie einen eindeutigen Namen, der Ihre Azure-Datenbank für PostgreSQL-Server identifiziert. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Es muss zwischen drei und 63 Zeichen lang sein.
resource-group | myresourcegroup | Geben Sie den Namen der Azure-Ressourcengruppe an.
location | westus | Der Azure-Standort für den Server.
admin-user | myadmin | Der Benutzername für die Administratoranmeldung. Dieser darf nicht **azure_superuser**, **admin**, **administrator**, **root**, **guest** oder **public** lauten.
admin-password | *sicheres Kennwort* | Das Kennwort des Administratorbenutzers. Es muss zwischen acht und 128 Zeichen lang sein. Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen und nicht alphanumerische Zeichen.
sku-name|GP_Gen5_2|Geben Sie den Namen des Tarifs und der Computekonfiguration ein. Folgt der Konvention „{Tarif} _{Computegeneration}_ {virtuelle Kerne}“ in Kurzform. Weitere Informationen finden Sie unter [Azure Database for PostgreSQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/server/).

>[!IMPORTANT] 
>- Die Standardversion von PostgreSQL auf dem Server lautet 9.6. Alle unterstützten Versionen finden Sie [hier](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions).
>- Alle Argumente für den Befehl **az mysql server create** finden Sie in [diesem Referenzdokument](https://docs.microsoft.com/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create).
>- SSL ist standardmäßig auf dem Server aktiviert. Weitere Informationen zu SSL finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit der Azure-Datenbank für MySQL](./concepts-ssl-connection-security.md).

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene 
Der erstellte Server ist standardmäßig durch Firewallregeln geschützt und nicht öffentlich zugänglich. Sie können die Firewallregel auf dem Server mithilfe des Befehls [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule) konfigurieren, damit Ihre lokale Umgebung Zugriff zum Herstellen einer Verbindung mit dem Server erhält. 

Das folgende Beispiel erstellt eine Firewallregel namens `AllowMyIP`, die Verbindungen über eine bestimmte IP-Adresse (192.168.0.1) zulässt. Ersetzen Sie die IP-Adresse oder den IP-Adressbereich durch den Ort, von dem Sie eine Verbindung herstellen möchten.  Wenn Sie nicht wissen, wie Sie Ihre IP-Adresse ermitteln, navigieren Sie zu [https://whatismyipaddress.com/](https://whatismyipaddress.com/), um Ihre IP-Adresse abzurufen.


```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
>  Stellen Sie sicher, dass die Firewall Ihres Netzwerks den Port 5432 zulässt. Dieser wird von Azure Database for PostgreSQL-Servern verwendet, um Konnektivitätsprobleme zu vermeiden. 

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Zum Herstellen einer Verbindung zum Server müssen Sie Hostinformationen und Anmeldeinformationen für den Zugriff angeben.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mydemoserver
```

Das Ergebnis liegt im JSON-Format vor. Notieren Sie sich die Werte für **administratorLogin** und **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-azure-database-for-postgresql-server-using-psql"></a>Herstellen einer Verbindung mit Azure Database for PostgreSQL mithilfe von psql
[**psql**](https://www.postgresql.org/docs/current/static/app-psql.html) ist ein beliebter Client für die Verbindungsherstellung mit PostgreSQL-Servern. Sie können mithilfe von **psql** und [Azure Cloud Shell](../cloud-shell/overview.md) eine Verbindung mit Ihrem Server herstellen. Alternativ dazu können Sie psql in Ihrer lokalen Umgebung verwenden, sofern verfügbar. Mit Ihrem neuen PostgreSQL-Server wird bereits die leere Datenbank „postgres“ erstellt, die Sie wie folgt zur Verbindungsherstellung mithilfe von psql verwenden können: 

   ```bash
 psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

   > [!TIP]
   > Wenn Sie lieber einen URL-Pfad zum Herstellen einer Verbindung mit Postgres verwenden, codiert die URL das @-Zeichen im Benutzernamen mit `%40`. Die Verbindungszeichenfolge für psql sieht beispielsweise so aus:
   > ```
   > psql postgresql://myadmin%40mydemoserver@mydemoserver.postgres.database.azure.com:5432/postgres
   > ```


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Wenn Sie diese Ressourcen nicht für einen anderen Schnellstart bzw. ein anderes Tutorial benötigen, können Sie sie mit dem folgenden Befehl löschen: 

```azurecli-interactive
az group delete --name myresourcegroup
```

Wenn Sie nur den neu erstellten Server löschen möchten, können Sie den Befehl [az postgres server delete](/cli/azure/postgres/server) ausführen.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)
> 
> [Bereitstellen einer Django-Web-App mit PostgreSQL](../app-service/containers/tutorial-python-postgresql-app.md)
>
> [Herstellen einer Verbindung mit einer Node.js-App](./connect-nodejs.md)

