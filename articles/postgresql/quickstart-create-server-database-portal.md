---
title: 'Schnellstart: Erstellen eines Servers – Azure-Portal – Azure Database for PostgreSQL-Einzelserver'
description: In dieser Schnellstartanleitung erstellen und verwalten Sie einen Azure Database for PostgreSQL-Server über das Azure-Portal.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/27/2020
ms.openlocfilehash: 6e43d00722dd86934c8f95e06a3b8b590b263d61
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91705159"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-server-by-using-the-azure-portal"></a>Schnellstart: Erstellen eines Azure Database for PostgreSQL-Servers mithilfe des Azure-Portals

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung wird veranschaulicht, wie Sie in etwa fünf Minuten über das Azure-Portal einen Azure Database for PostgreSQL-Server erstellen.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Browsen Sie zum [Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Ein Azure Database for PostgreSQL-Server wird mit einer konfigurierten Gruppe von [Compute- und Speicherressourcen](./concepts-pricing-tiers.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/management/overview.md) erstellt.

So erstellen Sie einen Azure Database for PostgreSQL-Server:

1. Wählen Sie links oben im Portal **Ressource erstellen** aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-database-portal/1-create-database.png" alt-text="Screenshot: Azure Database for PostgreSQL im Menü.":::

3. Wählen Sie die Bereitstellungsoption **Einzelserver** aus.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-database-portal/select-deployment-option.png" alt-text="Screenshot: Azure Database for PostgreSQL im Menü.":::

4. Füllen Sie das Formular **Grundlagen** mit den folgenden Informationen aus:

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-database-portal/create-basics.png" alt-text="Screenshot: Azure Database for PostgreSQL im Menü.":::

   Einstellung|Vorgeschlagener Wert|BESCHREIBUNG
   ---|---|---
   Subscription|Ihr Abonnementname|Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
   Resource group|*myresourcegroup*| Ein neuer Ressourcengruppenname oder ein bereits vorhandener Name aus Ihrem Abonnement
   Servername |*mydemoserver*|Ein eindeutiger Name, der Ihren Azure-Datenbank für PostgreSQL-Server identifiziert. Der Domänenname *postgres.database.azure.com* wird an den angegebenen Servernamen angefügt. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss zwischen 3 und 63 Zeichen enthalten.
   Datenquelle | **None** | Wählen Sie **Keine** aus, um einen neuen Server ohne Vorlage zu erstellen. (Die Option **Sicherung** können Sie auswählen, wenn Sie einen Server auf der Grundlage einer Geosicherung eines vorhandenen Azure Database for PostgreSQL-Servers erstellen.)
   Administratorbenutzername |*myadmin*| Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht **azure_superuser**, **azure_pg_admin**, **admin**, **administrator**, **root**, **guest**, oder **public** lauten. Er kann nicht mit **pg_** beginnen.
   Kennwort |Ihr Kennwort| Ein neues Kennwort für das Serveradministratorkonto. Es muss 8 bis 128 Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0 bis 9) und nicht alphanumerische Zeichen (z. B. !, $, #, %).
   Standort|Die Region, die Ihren Benutzern am nächsten liegt| Der Standort, der Ihren Benutzern am nächsten ist
   Version|Die aktuelle Hauptversion| Die aktuelle PostgreSQL-Hauptversion (es sei denn, Sie haben besondere Anforderungen).
   Compute und Speicher | **Universell**, **Gen 5**, **2 virtuelle Kerne**, **5 GB**, **7 Tage**, **Georedundant** | Die Compute-, Speicher- und Sicherungskonfigurationen für Ihren neuen Server. Wählen Sie **Server konfigurieren** aus. Wählen Sie als Nächstes einen geeigneten Tarif aus. Weitere Informationen finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/postgresql/server/). Wählen Sie zum Aktivieren der Serversicherungen in georedundantem Speicher unter **Optionen für Sicherungsredundanz** die Option **Georedundant** aus. Klicken Sie auf **OK**.

   > [!NOTE]
   > Ziehen Sie die Verwendung des Tarifs „Basic“ in Betracht, wenn geringe Compute- und E/A-Leistung für Ihre Workload angemessen ist. Beachten Sie, dass im Tarif „Basic“ erstellte Server später nicht auf „Universell“ oder „Arbeitsspeicheroptimiert“ skaliert werden können. 
   
5. Wählen Sie **Überprüfen + erstellen** aus, um ihre Auswahl zu überprüfen. Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Dieser Vorgang kann einige Minuten dauern.

6. Klicken Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** (eine Glocke), um den Bereitstellungsprozess zu überwachen. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus, um die Seite **Übersicht** des Servers zu öffnen.

Eine leere Datenbank (**postgres**) wird erstellt. Darüber hinaus wird die Datenbank **azure_maintenance** angezeigt. Mit dieser Datenbank werden die Prozesse des verwalteten Diensts von den Benutzeraktionen getrennt. Auf die Datenbank **azure_maintenance** kann nicht zugegriffen werden.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene
Der von Ihnen erstellte Server ist standardmäßig nicht öffentlich zugänglich. Sie müssen Ihrer IP-Adresse Berechtigungen erteilen. Navigieren Sie im Azure-Portal zu Ihrer Serverressource, und wählen Sie im Menü auf der linken Seite die Option **Verbindungssicherheit** für Ihre Serverressource aus. Wenn Sie nicht sicher sind, wie Sie Ihre Ressource finden, lesen Sie die Informationen unter [Öffnen von Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resources-portal#open-resources).

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-database-portal/add-current-ip-firewall.png" alt-text="Screenshot: Azure Database for PostgreSQL im Menü.":::
  
Wählen Sie **Aktuelle Client-IP-Adresse hinzufügen** und dann **Speichern** aus. Sie können weitere IP-Adressen hinzufügen oder einen IP-Adressbereich angeben, um über diese IP-Adressen eine Verbindung mit dem Server herzustellen. Weitere Informationen finden Sie unter [Firewallregeln in Azure Database for PostgreSQL – Einzelserver](./concepts-firewall-rules.md).
   
> [!NOTE]
> Überprüfen Sie, ob Ihr Netzwerk ausgehenden Datenverkehr über Port 5432 zulässt, um Verbindungsprobleme zu vermeiden. Azure Database for PostgreSQL nutzt diesen Port.  

## <a name="connect-to-azure-database-for-postgresql-server-by-using-psql"></a>Herstellen einer Verbindung mit einem Azure Database for PostgreSQL-Server mithilfe von psql

Sie können die beliebten PostgreSQL-Clients [psql](http://postgresguide.com/utilities/psql.html) oder [pgAdmin](https://www.pgadmin.org/docs/pgadmin4/latest/connecting.html) verwenden. In dieser Schnellstartanleitung wird die Verbindung mithilfe von psql in [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) über das Azure-Portal hergestellt.

1. Notieren Sie sich den Servernamen, den Anmeldenamen des Serveradministrators, das Kennwort und die Abonnement-ID für den neu erstellten Server im Abschnitt **Übersicht** Ihres Servers.

2. Öffnen Sie Azure Cloud Shell im Portal, indem Sie oben links das Symbol auswählen.

   > [!NOTE]
   > Wenn Sie Cloud Shell zum ersten Mal öffnen, wird eine Eingabeaufforderung zum Erstellen einer Ressourcengruppe und eines Speicherkontos angezeigt. Dieser Schritt ist nur einmal erforderlich. Diese Komponenten werden für alle Sitzungen automatisch angefügt. 

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="media/quickstart-create-database-portal/use-in-cloud-shell.png" alt-text="Screenshot: Azure Database for PostgreSQL im Menü.":::

3. Führen Sie den nachstehenden Befehl im Azure Cloud Shell-Terminal aus. Ersetzen Sie die Werte durch den tatsächlichen Servernamen und den Anmeldenamen des Administratorbenutzers. Verwenden Sie die leere Datenbank **postgres** mit dem Administratorbenutzer im folgenden Format: `<admin-username>@<servername>`.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```
 
   So sieht die Anzeige im Cloud Shell-Terminal aus:
   
   ```bash
    Requesting a Cloud Shell.Succeeded.
    Connecting terminal...

    Welcome to Azure Cloud Shell
 
    Type "az" to use Azure CLI
    Type "help" to learn about Cloud Shell

    user@Azure:~$psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
    Password for user myadmin@mydemoserver.postgres.database.azure.com:
    psql (12.2 (Ubuntu 12.2-2.pgdg16.04+1), server 11.6)
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
    Type "help" for help.

    postgres=>
    ```
4. Erstellen Sie im gleichen Azure Cloud Shell-Terminal eine Datenbank namens **guest**.

   ```bash
   postgres=> CREATE DATABASE guest;
   ```

5. Sie können Verbindungen auf die neu erstellte Datenbank **guest** umstellen.

   ```bash
   \c guest
   ```
6. Geben Sie `\q` ein, und drücken Sie anschließend die EINGABETASTE, um psql zu schließen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie haben erfolgreich einen Azure Database for PostgreSQL-Server in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, können Sie sie löschen, indem Sie entweder die Ressourcengruppe oder den PostgreSQL-Server löschen. 

So löschen Sie die Ressourcengruppe:

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. 
2. Wählen Sie in der Liste der Ressourcengruppen den Namen Ihrer Ressourcengruppe aus.
3. Wählen Sie auf der Seite **Übersicht** der Ressourcengruppe die Option **Ressourcengruppe löschen** aus.
4. Geben Sie im Bestätigungsdialogfeld den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

Wählen Sie zum Löschen des Servers auf der Seite **Übersicht** Ihres Servers die Schaltfläche **Löschen** aus:

> [!div class="mx-imgBorder"]
> :::image type="content" source="media/quickstart-create-database-portal/12-delete.png" alt-text="Screenshot: Azure Database for PostgreSQL im Menü.":::

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)
