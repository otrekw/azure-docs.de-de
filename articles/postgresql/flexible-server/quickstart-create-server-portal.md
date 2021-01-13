---
title: 'Schnellstart: Erstellen eines Servers – Azure-Portal – Azure Database for PostgreSQL Flexible Server'
description: Dies ist eine Schnellstartanleitung zum Erstellen und Verwalten einer Azure Database for PostgreSQL Flexible Server-Instanz über die Benutzeroberfläche des Azure-Portals.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/22/2020
ms.openlocfilehash: 978fcb8c8cf9d2ffd929da5b76d7fd2ff109f420
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535857"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---flexible-server-in-the-azure-portal"></a>Schnellstart: Erstellen einer Azure Database for PostgreSQL Flexible Server-Instanz im Azure-Portal

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Azure-Datenbank für PostgreSQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal in etwa fünf Minuten eine Azure Database for PostgreSQL Flexible Server-Instanz erstellen.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Browsen Sie zum [Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Ein Azure Database for PostgreSQL-Server wird mit einer konfigurierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../../azure-resource-manager/management/overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um einen Azure-Datenbank für PostgreSQL-Server zu erstellen:

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für PostgreSQL** aus.

    :::image type="content" source="./media/quickstart-create-database-portal/1-create-database.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

3. Wählen Sie die Bereitstellungsoption **Flexible Server** aus.

   :::image type="content" source="./media/quickstart-create-database-portal/2-select-deployment-option.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

4. Füllen Sie dann das Formular **Grundlagen** mit den folgenden Informationen aus:

    :::image type="content" source="./media/quickstart-create-database-portal/3-create-basics.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

    Einstellung|Empfohlener Wert|BESCHREIBUNG
    ---|---|---
    Subscription|Ihr Abonnementname|Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet werden soll.
    Resource group|*myresourcegroup*| Ein neuer Ressourcengruppenname oder ein bereits vorhandener Name aus Ihrem Abonnement
    Servername |*mydemoserver*|Ein eindeutiger Name, der Ihren Azure-Datenbank für PostgreSQL-Server identifiziert. Der Domänenname *postgres.database.azure.com* wird an den angegebenen Servernamen angefügt. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss 3 bis 63 Zeichen umfassen.
    Administratorbenutzername |*myadmin*| Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht **azure_superuser** , **azure_pg_admin** , **admin** , **administrator** , **root** , **guest** , oder **public** lauten. Er kann nicht mit **pg_** beginnen.
    Kennwort |Ihr Kennwort| Ein neues Kennwort für das Serveradministratorkonto. Es muss zwischen acht und 128 Zeichen lang sein. Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0 bis 9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
    Standort|Die Region, die Ihren Benutzern am nächsten liegt| Der Standort, der Ihren Benutzern am nächsten ist.
    Version|Die aktuelle Hauptversion| Die aktuelle PostgreSQL-Hauptversion (es sei denn, Sie haben besondere Anforderungen).
    Compute und Speicher | **Universell** , **4 virtuelle Kerne** , **512 GB** , **7 Tage** | Die Compute-, Speicher- und Sicherungskonfigurationen für Ihren neuen Server. Wählen Sie **Server konfigurieren** aus. *Universell* , *4 virtuelle Kerne* , *512 GB* und *7 Tage* sind die Standardwerte für **Computetarif** , **Virtuelle Kerne** , **Speicher** und **Aufbewahrungszeitraum für Sicherung** . Sie können diese Schieberegler unverändert lassen oder sie anpassen. Klicken Sie auf **OK** , um die Tarifauswahl zu speichern. Der nächste Screenshot zeigt die gewählten Optionen.

    :::image type="content" source="./media/quickstart-create-database-portal/4-pricing-tier.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::
    
5. Konfigurieren von Netzwerkoptionen

    Auf der Registerkarte „Netzwerk“ können Sie auswählen, wie der Server erreichbar sein soll. Azure-Datenbank für PostgreSQL erstellt eine Firewall auf Serverebene. Sie verhindert, dass externe Anwendungen und Tools eine Verbindung mit dem Server oder Datenbanken auf dem Server herstellen – es sei denn, Sie erstellen eine Regel, um die Firewall für bestimmte IP-Adressen zu öffnen. Es wird empfohlen, den Server öffentlich zugänglich zu machen:

    :::image type="content" source="./media/quickstart-create-database-portal/5-networking.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

    Außerdem ist es ratsam, ihn auf Ihre eigene IP-Adresse zu beschränken:

    :::image type="content" source="./media/quickstart-create-database-portal/6-add-client-ip.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

6. Wählen Sie **Überprüfen + erstellen** aus, um ihre Auswahl zu überprüfen. Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Dieser Vorgang kann einige Minuten dauern.

7. Klicken Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** (eine Glocke), um den Bereitstellungsprozess zu überwachen. Nach Abschluss der Bereitstellung können Sie auf **An Dashboard anheften** klicken, um auf Ihrem Azure-Portal-Dashboard eine Kachel für diesen Server zu erstellen, über die Sie direkt zur **Übersicht** des Servers gelangen. Wenn Sie auf **Zu Ressource wechseln** klicken, wird die **Übersicht** des Servers geöffnet.

    :::image type="content" source="./media/quickstart-create-database-portal/7-notifications.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

   Standardmäßig werden **postgres** -Datenbanken unter dem Server erstellt. Die [postgres](https://www.postgresql.org/docs/12/static/app-initdb.html)-Datenbank ist eine Standarddatenbank für die Verwendung durch Benutzer, Hilfsprogramme und Drittanbieteranwendungen. (Die andere Standarddatenbank ist **azure_maintenance** . Ihre Funktion besteht darin, die Prozesse verwalteter Dienste von Benutzeraktionen zu trennen. Sie können auf diese Datenbank nicht zugreifen.

    > [!NOTE]
    > Die Kommunikation für Verbindungen mit Azure-Datenbank für PostgreSQL erfolgt über Port 5432. Wenn Sie versuchen, eine Verbindung von einem Unternehmensnetzwerk aus herzustellen, wird der ausgehende Datenverkehr über Port 5432 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Server herstellen, wenn Ihre IT-Abteilung Port 5432 öffnet.
    >

## <a name="get-the-connection-information"></a>Abrufen der Verbindungsinformationen

Wenn wir unseren Azure-Datenbank für PostgreSQL-Server erstellt haben, wird eine Standarddatenbank namens **postgres** erstellt. Für die Verbindungsherstellung mit Ihrem Datenbankserver benötigen Sie den vollständigen Servernamen und die Administratoranmeldeinformationen. Diese Werte haben Sie möglicherweise zuvor im Rahmen des Schnellstartartikels notiert. Andernfalls finden Sie den Servernamen und die Anmeldeinformationen im Portal auf der Seite **Übersicht** für Ihren Server.

Öffnen Sie die Seite **Übersicht** für Ihren Server. Notieren Sie sich den **Servernamen** und den **Anmeldenamen des Serveradministrators** . Zeigen Sie mit dem Cursor auf das jeweilige Feld. Daraufhin wird rechts neben dem Text ein Kopiersymbol angezeigt. Wählen Sie das Kopiersymbol aus, um die Werte zu kopieren.

 :::image type="content" source="./media/quickstart-create-database-portal/8-server-name.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

## <a name="connect-to-the-postgresql-database-using-psql"></a>Herstellen einer Verbindung mit der PostgreSQL-Datenbank mithilfe von psql

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

2. Erstellen Sie eine leere Datenbank namens „mypgsqldb“, indem Sie an der Eingabeaufforderung den folgenden Befehl eingeben:

    ```bash
    CREATE DATABASE mypgsqldb;
    ```

3. Führen Sie an der Eingabeaufforderung den folgenden Befehl zum Wechseln der Verbindung zur neu erstellten Datenbank **mypgsqldb** aus:

    ```bash
    \c mypgsqldb
    ```

4. Geben Sie `\q` ein, und drücken Sie anschließend die EINGABETASTE, um psql zu beenden.

Sie haben über psql eine Verbindung mit dem Azure Database for PostgreSQL-Server hergestellt und eine leere Benutzerdatenbank erstellt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Die im Rahmen dieser Schnellstartanleitung erstellten Ressourcen können auf zwei Arten bereinigt werden. Sie können die Azure-Ressourcengruppe einschließlich aller darin enthaltenen Ressourcen löschen. Falls die anderen Ressourcen erhalten bleiben sollen, löschen Sie nur die Serverressource.

> [!TIP]
> Andere Schnellstartanleitungen in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie weitere Schnellstartanleitungen verwenden möchten, überspringen Sie die Bereinigung der in dieser Schnellstartanleitung erstellten Ressourcen. Falls Sie nicht fortfahren möchten, können Sie die in dieser Schnellstartanleitung erstellten Ressourcen anhand der folgenden Schritte im Azure-Portal löschen.

So löschen Sie die gesamte Ressourcengruppe, einschließlich des neu erstellten Servers

1. Navigieren Sie im Portal zu Ihrer Ressourcengruppe. Wählen Sie im Menü auf der linken Seite **Ressourcengruppen** aus. Wählen Sie anschließend den Namen Ihrer Ressourcengruppe (in diesem Beispiel **myresourcegroup** ) aus.

2. Wählen Sie auf der Ressourcengruppenseite die Option **Löschen** aus. Geben Sie dann den Namen der Ressourcengruppe (in diesem Beispiel **myresourcegroup** ) in das Textfeld ein, um das Löschen zu bestätigen. Klicken Sie auf **Löschen** .

So löschen Sie ausschließlich den neu erstellten Server

1. Navigieren Sie im Portal zu Ihrem Server, falls er nicht geöffnet ist. Wählen Sie im Menü auf der linken Seite die Option **Alle Ressourcen** aus. Suchen Sie anschließend nach dem Server, den Sie erstellt haben.

2. Wählen Sie auf der Seite **Übersicht** die Option **Löschen** aus.

    :::image type="content" source="./media/quickstart-create-database-portal/9-delete.png" alt-text="„Azure Database for PostgreSQL“ im Menü":::

3. Bestätigen Sie den Namen des Servers, den Sie löschen möchten, und zeigen Sie die betroffenen Datenbanken auf diesem Server an. Geben Sie den Servernamen (in diesem Beispiel **mydemoserver** ) in das Textfeld ein. Klicken Sie auf **Löschen** .

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Bereitstellen einer Django-Web-App mit PostgreSQL in Azure App Service](tutorial-django-app-service-postgres.md)