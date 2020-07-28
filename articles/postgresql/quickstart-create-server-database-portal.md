---
title: 'Schnellstart: Erstellen eines Servers – Azure-Portal – Azure Database for PostgreSQL-Einzelserver'
description: Schnellstartanleitung zum Erstellen und Verwalten eines Azure Database for PostgreSQL-Einzelservers über die Benutzeroberfläche des Azure-Portals.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/27/2020
ms.openlocfilehash: 9741e00cdcc4907c547ebfcc6f8e7df7a3b32891
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86529671"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-server-in-the-azure-portal"></a>Schnellstart: Erstellen eines Azure-Datenbank für PostgreSQL-Servers im Azure-Portal

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal in etwa fünf Minuten einen Azure Database for PostgreSQL-Server erstellen.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Browsen Sie zum [Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Ein Azure Database for PostgreSQL-Server wird mit einer konfigurierten Gruppe von [Compute- und Speicherressourcen](./concepts-pricing-tiers.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/management/overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um einen Azure-Datenbank für PostgreSQL-Server zu erstellen:
1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus.

   > [!div class="mx-imgBorder"]
   > ![„Azure Database for PostgreSQL“ im Menü](./media/quickstart-create-database-portal/1-create-database.png)

3. Wählen Sie die Bereitstellungsoption **Einzelserver** aus.

   > [!div class="mx-imgBorder"]
   > ![Auswählen der Bereitstellungsoption „Einzelserver“ für Azure Database for PostgreSQL](./media/quickstart-create-database-portal/select-deployment-option.png)

4. Füllen Sie dann das Formular **Grundlagen** mit den folgenden Informationen aus:

   > [!div class="mx-imgBorder"]
   > ![Erstellen eines Servers](./media/quickstart-create-database-portal/create-basics.png)

   Einstellung|Empfohlener Wert|BESCHREIBUNG
   ---|---|---
   Subscription|Ihr Abonnementname|Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
   Resource group|*myresourcegroup*| Ein neuer Ressourcengruppenname oder ein bereits vorhandener Name aus Ihrem Abonnement
   Servername |*mydemoserver*|Ein eindeutiger Name, der Ihren Azure-Datenbank für PostgreSQL-Server identifiziert. Der Domänenname *postgres.database.azure.com* wird an den angegebenen Servernamen angefügt. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss 3 bis 63 Zeichen umfassen.
   Datenquelle | *None* | Wählen Sie *Keine* aus, um einen neuen Server ohne Vorlage zu erstellen. (Die Option *Sicherung* können Sie auswählen, wenn Sie einen Server auf der Grundlage einer Geosicherung eines vorhandenen Azure Database for PostgreSQL-Servers erstellen.)
   Administratorbenutzername |*myadmin*| Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht **azure_superuser**, **azure_pg_admin**, **admin**, **administrator**, **root**, **guest**, oder **public** lauten. Er kann nicht mit **pg_** beginnen.
   Kennwort |Ihr Kennwort| Ein neues Kennwort für das Serveradministratorkonto. Es muss zwischen acht und 128 Zeichen lang sein. Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0 bis 9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
   Standort|Die Region, die Ihren Benutzern am nächsten liegt| Der Standort, der Ihren Benutzern am nächsten ist.
   Version|Die aktuelle Hauptversion| Die aktuelle PostgreSQL-Hauptversion (es sei denn, Sie haben besondere Anforderungen).
   Compute und Speicher | **Universell**, **Gen 5**, **2 virtuelle Kerne**, **5 GB**, **7 Tage**, **Georedundant** | Die Compute-, Speicher- und Sicherungskonfigurationen für Ihren neuen Server. Wählen Sie **Server konfigurieren** aus. Wählen Sie als Nächstes den entsprechenden Tarif aus. Weitere Informationen hierzu finden Sie unter [Azure Database for PostgreSQL – Preise](https://azure.microsoft.com/pricing/details/postgresql/server/). Wählen Sie zum Aktivieren der Serversicherungen in georedundantem Speicher unter **Optionen für Sicherungsredundanz** die Option **Georedundant** aus. Klicken Sie auf **OK**.

   > [!NOTE]
   > Ziehen Sie die Verwendung des Tarifs „Basic“ in Betracht, wenn geringe Compute- und E/A-Leistung für Ihre Workload angemessen ist. Beachten Sie, dass im Tarif „Basic“ erstellte Server später nicht auf „Universell“ oder „Arbeitsspeicheroptimiert“ skaliert werden können. 
   
5. Wählen Sie **Überprüfen + erstellen** aus, um ihre Auswahl zu überprüfen. Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Dieser Vorgang kann einige Minuten dauern.

6. Klicken Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** (eine Glocke), um den Bereitstellungsprozess zu überwachen. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus. Daraufhin wird die Seite **Übersicht** des Servers geöffnet.

Eine leere Datenbank (**postgres**) wird erstellt. Darüber hinaus wird die Datenbank **azure_maintenance** angezeigt. Mit dieser Datenbank werden die Prozesse verwalteter Dienste von Benutzeraktionen getrennt. Sie können nicht auf die Datenbank **azure_maintenance** zugreifen.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene
Der erstellte Server ist standardmäßig nicht öffentlich zugänglich, und Sie müssen Berechtigungen für Ihre IP-Adresse erteilen. Wenn Sie den Zugriff auf Ihre IP-Adresse gewähren möchten, navigieren Sie im Azure-Portal zu Ihrer Serverressource, und wählen Sie im Menü auf der linken Seite die Option **Verbindungssicherheit** für Ihre Serverressource aus. Wenn Sie nicht sicher sind, wie Sie Ihre Ressource finden, lesen Sie die Informationen unter [Öffnen von Ressourcen](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resources-portal#open-resources).

> [!div class="mx-imgBorder"]
> ![Verbindungssicherheit – Firewallregeln](./media/quickstart-create-database-portal/add-current-ip-firewall.png)
  
Wählen Sie nun **Aktuelle Client-IP-Adresse hinzufügen** und dann **Speichern** aus. Sie können weitere IP-Adressen hinzufügen oder einen IP-Adressbereich angeben, um über diese IP-Adressen eine Verbindung mit dem Server herzustellen. Weitere Informationen finden Sie unter [Firewallregeln in Azure Database for PostgreSQL – Einzelserver](./concepts-firewall-rules.md).
   
> [!NOTE]
> Überprüfen Sie, ob Ihr Netzwerk ausgehenden Datenverkehr über Port 5432 zulässt. Dieser Port wird von Azure Database for PostgreSQL verwendet, um Verbindungsprobleme zu vermeiden.  

## <a name="connect-to-azure-database-for-postgresql-server-using-psql"></a>Herstellen einer Verbindung mit Azure Database for PostgreSQL mithilfe von psql

Sie können die beliebten PostgreSQL-Clients [psql](http://postgresguide.com/utilities/psql.html) oder [pgAdmin](https://www.pgadmin.org/docs/pgadmin4/latest/connecting.html) verwenden. In dieser Schnellstartanleitung wird die Verbindung mithilfe von psql in [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) über das Azure-Portal hergestellt.

1. Notieren Sie sich den Servernamen, den Anmeldenamen des Serveradministrators, das Kennwort und die Abonnement-ID für den neu erstellten Server im Abschnitt **Übersicht** Ihres Servers, wie in der nachfolgenden Abbildung gezeigt.

2. Starten Sie Azure Cloud Shell im Portal, indem Sie oben links das Symbol auswählen, wie in der Abbildung unten gezeigt.

   > [!NOTE]
   > Wenn Sie Cloud Shell zum ersten Mal starten, wird eine Eingabeaufforderung zum Erstellen einer Ressourcengruppe und eines Speicherkontos angezeigt. Dieser Schritt ist nur einmal erforderlich. Diese Komponenten werden für alle Sitzungen automatisch angefügt. 

   > [!div class="mx-imgBorder"]
   > ![Öffnen von Azure Cloud Shell](media/quickstart-create-database-portal/use-in-cloud-shell.png)

3. Führen Sie im Azure Cloud Shell-Terminal den folgenden Befehl aus. Ersetzen Sie die Werte durch den tatsächlichen Servernamen und den Anmeldenamen des Administratorbenutzers. Verwenden Sie die leere Datenbank **postgres** mit dem Administratorbenutzer im Format „<Administratorbenutzername> @<servername>“, wie nachfolgend für Azure Database for PostgreSQL gezeigt:

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```
 
   So sieht die Anzeige im Cloud Shell-Terminal aus:
   
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

5. Jetzt können Sie Verbindungen auf die neu erstellte Datenbank **guest** umstellen.

   ```bash
   \c guest
   ```
6. Geben Sie `\q` ein, und drücken Sie anschließend die EINGABETASTE, um psql zu beenden. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie haben erfolgreich einen Azure Database for PostgreSQL-Server in einer Ressourcengruppe erstellt.  Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, können Sie sie löschen, indem Sie die Ressourcengruppe oder einfach den PostgreSQL-Server löschen. Um die Ressourcengruppe zu löschen, gehen Sie wie folgt vor:

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus. 
2. Wählen Sie in der Liste der Ressourcengruppen den Namen Ihrer Ressourcengruppe aus.
3. Wählen Sie auf der Seite „Übersicht“ der Ressourcengruppe die Option **Ressourcengruppe löschen** aus.
4. Geben Sie im Bestätigungsdialogfeld den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

Zum Löschen des Servers können Sie auf der Seite **Übersicht** Ihres Servers auf die Schaltfläche **Löschen** klicken, wie hier gezeigt:
> [!div class="mx-imgBorder"]
> ![Löschen Ihrer Ressourcen](media/quickstart-create-database-portal/12-delete.png)

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Migrieren der Datenbank mit Export und Import](./howto-migrate-using-export-and-import.md)
