---
title: Abfragen einer SQL-Datenbank mithilfe des Abfrage-Editors im Azure-Portal (Vorschau)
description: Hier erfahren Sie, wie Sie den Abfrage-Editor verwenden, um T-SQL-Abfragen (Transact-SQL) für eine Datenbank in Azure SQL-Datenbank auszuführen.
titleSuffix: Azure SQL Database
keywords: Verbindung mit SQL-Datenbank herstellen, SQL-Datenbank abfragen, Azure-Portal, Portal, Abfrage-Editor
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: quickstart
author: Ninarn
ms.author: ninarn
ms.reviewer: sstein
ms.date: 03/01/2021
ms.openlocfilehash: b2b614002a380ecd9c7eb6ec40e9dbd734b99628
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692843"
---
# <a name="quickstart-use-the-azure-portals-query-editor-preview-to-query-an-azure-sql-database"></a>Schnellstart: Verwenden des Abfrage-Editors (Vorschau) des Azure-Portals, um eine Azure SQL-Datenbank abzufragen
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Der Abfrage-Editor ist ein Tool im Azure-Portal zur Ausführung von SQL-Abfragen für Ihre Datenbank in Azure SQL-Datenbank oder Ihr Data Warehouse in Azure Synapse Analytics.

In dieser Schnellstartanleitung erfahren Sie, wie Sie den Abfrage-Editor verwenden, um T-SQL-Abfragen (Transact-SQL) für eine Datenbank auszuführen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="create-a-database-with-sample-data"></a>Erstellen einer Datenbank mit Beispieldaten

Für diese Schnellstartanleitung wird die AdventureWorksLT-Beispieldatenbank benötigt. Falls Sie nicht über eine Arbeitskopie der AdventureWorksLT-Beispieldatenbank in SQL-Datenbank verfügen, können Sie mit der folgenden Schnellstartanleitung im Handumdrehen eine Kopie erstellen:

[Schnellstart: Erstellen einer Datenbank in Azure SQL-Datenbank über das Azure-Portal, PowerShell oder die Azure CLI](single-database-create-quickstart.md)

### <a name="set-an-azure-active-directory-admin-for-the-server-optional"></a>Festlegen eines Azure Active Directory-Administrators für den Server (optional)

Wenn Sie einen Azure Active Directory-Administrator (Azure AD) konfigurieren, können Sie eine einzelne Identität verwenden, um sich beim Azure-Portal und bei Ihrer Datenbank anzumelden. Führen Sie die unten angegebenen Schritte aus, wenn Sie Azure AD für die Verbindungsherstellung mit dem Abfrage-Editor nutzen möchten.

Dieser Prozess ist optional. Alternativ können Sie auch die SQL-Authentifizierung verwenden, um die Verbindung mit dem Abfrage-Editor herzustellen.

> [!NOTE]
> * E-Mail-Konten (etwa „outlook.com“, „gmail.com“, „yahoo.com“ usw.) werden noch nicht als Azure AD-Administratoren unterstützt. Stellen Sie sicher, dass Sie einen Benutzer auswählen, der entweder nativ in Azure AD erstellt oder in einen Azure AD-Verbund eingefügt wurde.
> * Die Azure AD-Administratoranmeldung funktioniert für Konten, für die die zweistufige Authentifizierung aktiviert ist, aber vom Abfrage-Editor wird die zweistufige Authentifizierung nicht unterstützt.

1. Navigieren Sie im Azure-Portal zu Ihrem SQL-Datenbank-Server.

2. Wählen Sie im Menü **SQL Server** die Option **Active Directory-Administrator** aus.

3. Wählen Sie auf der Seite **Active Directory-Administrator** der SQL Server-Instanz in der Symbolleiste die Option **Administrator festlegen** aus.

    ![Auswählen von Active Directory](./media/connect-query-portal/select-active-directory.png)

4. Geben Sie auf der Seite **Administrator hinzufügen** im Suchfeld einen Benutzer oder eine Gruppe ein, nach dem bzw. nach der Sie suchen möchten, und wählen Sie ihn bzw. sie als Administrator aus. Wählen Sie anschließend die Schaltfläche **Auswählen** aus.

5. Wählen Sie auf der Symbolleiste der Seite **Active Directory-Administrator** der SQL Server-Instanz die Option **Speichern** aus.

## <a name="using-sql-query-editor"></a>Verwenden des SQL-Abfrage-Editors

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie die Datenbank aus, die Sie abfragen möchten.

2. Wählen Sie im Menü **SQL-Datenbank** die Option **Abfrage-Editor (Vorschau)** aus.

    ![Suchen des Abfrage-Editors](./media/connect-query-portal/find-query-editor.PNG)

### <a name="establish-a-connection-to-the-database"></a>Herstellen einer Verbindung mit der Datenbank

Sie sind zwar bereits beim Portal angemeldet, für den Zugriff auf die Datenbank müssen aber dennoch Anmeldeinformationen angegeben werden. Für die Verbindungsherstellung mit der Datenbank können Sie die SQL-Authentifizierung oder Azure Active Directory verwenden.

#### <a name="connect-using-sql-authentication"></a>Herstellen einer Verbindung per SQL-Authentifizierung

1. Geben Sie auf der Seite **Anmeldung** unter **SQL Server-Authentifizierung** im Feld **Anmeldename** einen Anmeldenamen und im Feld **Kennwort** ein Kennwort für einen Benutzer mit Zugriff auf die Datenbank ein. Verwenden Sie im Zweifelsfall den Anmeldenamen und das Kennwort für den Serveradministrator des Datenbankservers.

    ![Anmelden](./media/connect-query-portal/login-menu.png)

2. Klicken Sie auf **OK**.

#### <a name="connect-using-azure-active-directory"></a>Herstellen einer Verbindung mit Azure Active Directory

Sehen Sie sich im **Abfrage-Editor (Vorschauversion)** die Seite **Anmeldung** im Abschnitt **Active Directory-Authentifizierung** an. Die Authentifizierung wird automatisch durchgeführt. Wenn Sie ein Azure AD-Administrator der Datenbank sind, wird dann eine Meldung mit dem Hinweis angezeigt, dass Sie angemeldet sind. Wählen Sie dann die Schaltfläche **Als *\<your user or group ID>* fortsetzen** aus. Sollte auf der Seite angezeigt werden, dass Sie sich nicht erfolgreich angemeldet haben, muss die Seite möglicherweise aktualisiert werden.

### <a name="query-a-database-in-sql-database"></a>Abfragen einer Datenbank in SQL-Datenbank

Die folgenden Beispielabfragen sollten erfolgreich für die AdventureWorksLT-Beispieldatenbank ausgeführt werden.

#### <a name="run-a-select-query"></a>Ausführen einer SELECT-Abfrage

1. Fügen Sie die folgende Abfrage im Abfrage-Editor ein:

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. Wählen Sie **Ausführen** aus, und sehen Sie sich dann die Ausgabe im Bereich **Ergebnisse** an.

   ![Abfrage-Editor – Ergebnisse](./media/connect-query-portal/query-editor-results.png)

3. Sie können die Abfrage optional als SQL-Datei speichern oder die zurückgegebenen Daten als JSON-, CSV- oder XML-Datei exportieren.

#### <a name="run-an-insert-query"></a>Ausführen einer INSERT-Abfrage

Führen Sie die folgende T-SQL-Anweisung vom Typ [INSERT](/sql/t-sql/statements/insert-transact-sql/) aus, um in der Tabelle `SalesLT.Product` ein neues Produkt hinzuzufügen.

1. Ersetzen Sie die vorherige Abfrage durch diese.

    ```sql
    INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
    VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```


2. Wählen Sie **Ausführen**, um eine neue Zeile in die Tabelle `Product` einzufügen. Im Bereich **Nachrichten** wird folgende Meldung angezeigt: **Abfrage erfolgreich: Betroffene Zeilen: 1**.


#### <a name="run-an-update-query"></a>Ausführen einer UPDATE-Abfrage

Führen Sie die folgende T-SQL-Anweisung vom Typ [UPDATE](/sql/t-sql/queries/update-transact-sql/) aus, um Ihr neues Produkt zu ändern.

1. Ersetzen Sie die vorherige Abfrage durch diese.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Wählen Sie **Ausführen**, um die angegebene Zeile in der Tabelle `Product` zu aktualisieren. Im Bereich **Nachrichten** wird folgende Meldung angezeigt: **Abfrage erfolgreich: Betroffene Zeilen: 1**.

#### <a name="run-a-delete-query"></a>Ausführen einer DELETE-Abfrage

Führen Sie die folgende T-SQL-Anweisung vom Typ [DELETE](/sql/t-sql/statements/delete-transact-sql/) aus, um das neue Produkt zu entfernen.

1. Ersetzen Sie die vorherige Abfrage durch diese:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Wählen Sie **Ausführen**, um die angegebene Zeile in der Tabelle `Product` zu löschen. Im Bereich **Nachrichten** wird folgende Meldung angezeigt: **Abfrage erfolgreich: Betroffene Zeilen: 1**.


## <a name="troubleshooting-and-considerations"></a>Problembehandlung und Überlegungen

Bei der Verwendung des Abfrage-Editors sind ein paar Dinge zu beachten:

### <a name="configure-local-network-settings"></a>Konfigurieren von lokalen Netzwerkeinstellungen

Sollte im Abfrage-Editor der Fehler
 - *Durch Ihre lokalen Netzwerkeinstellungen werden möglicherweise Abfragen vom Abfrage-Editor verhindert. Klicken Sie hier, um Anweisungen zum Konfigurieren der Netzwerkeinstellungen zu erhalten.*
 - *Die Verbindung mit dem Server konnte nicht hergestellt werden. Dies weist möglicherweise auf ein Problem mit der lokalen Firewallkonfiguration oder mit Ihren Einstellungen des Netzwerkproxys hin.*

Der Grund ist, dass für den Abfrage-Editor die Ports 443 und 1443 für die Kommunikation genutzt werden. Sie müssen sicherstellen, dass Sie ausgehenden HTTPS-Datenverkehr für diese Ports aktiviert haben. In den Anleitungen unten wird die entsprechende Vorgehensweise für verschiedene Betriebssysteme beschrieben. Unter Umständen müssen Sie mit der IT-Abteilung Ihres Unternehmens zusammenarbeiten, damit die Genehmigung zum Öffnen dieser Verbindung in Ihrem lokalen Netzwerk gewährt wird.

#### <a name="steps-for-windows"></a>Schritte für Windows

1. Öffnen Sie **Windows Defender Firewall**.
2. Wählen Sie im Menü auf der linken Seite die Option **Erweiterte Einstellungen** aus.
3. Wählen Sie unter **Windows Defender Firewall mit erweiterter Sicherheit** im Menü auf der linken Seite die Option **Ausgangsregeln** aus.
4. Wählen Sie im Menü auf der rechten Seite die Option **Neue Regel...** aus.

Führen Sie im **Assistenten für neue ausgehende Regeln** die folgenden Schritte aus:

1. Wählen Sie **Port** als Typ der zu erstellenden Regel aus. Wählen Sie **Weiter** aus.
2. Wählen Sie **TCP** aus.
3. Wählen Sie **Bestimmte Remoteports** aus, und geben Sie „443, 1443“ ein. Wählen Sie anschließend **Weiter** aus.
4. Wählen Sie „Verbindung zulassen, wenn sie sicher ist“ aus.
5. Wählen Sie **Weiter** und dann erneut **Weiter** aus.
5. Behalten Sie die Auswahl für „Domäne“, „Privat“ und „Öffentlich“ bei.
6. Geben Sie der Regel einen Namen, z. B. „Zugriff auf Azure SQL-Abfrage-Editor“, und fügen Sie optional eine Beschreibung hinzu. Wählen Sie anschließend **Fertig stellen** aus.

#### <a name="steps-for-mac"></a>Schritte für Macintosh
1. Öffnen Sie im Apple-Menü die Option **System Preferences** (Systemeinstellungen).
2. Klicken Sie auf **Security & Privacy** (Sicherheit und Datenschutz).
3. Klicken Sie auf **Firewall**.
4. Gehen Sie wie folgt vor, falls die Firewall deaktiviert ist: Wählen Sie unten die Option **Click the lock to make changes** (Auf Schloss klicken, um Änderungen vorzunehmen) und dann **Turn on Firewall** (Firewall aktivieren) aus.
4. Klicken Sie auf **Firewall Options** (Firewalloptionen).
5. Wählen Sie im Fenster **Security & Privacy** (Sicherheit und Datenschutz) die folgende Option aus: „Automatically allow signed software to receive incoming connections“ (Eingehende Verbindungen für signierte Software automatisch zulassen).

#### <a name="steps-for-linux"></a>Schritte für Linux
Führen Sie diese Befehle aus, um die iptables zu aktualisieren:
  ```
  sudo iptables -A OUTPUT -p tcp --dport 443 -j ACCEPT
  sudo iptables -A OUTPUT -p tcp --dport 1443 -j ACCEPT
  ```

### <a name="connection-considerations"></a>Verbindungsaspekte

* Bei öffentlichen Verbindungen mit dem Abfrage-Editor müssen Sie [Ihre ausgehende IP-Adresse den Firewallzulassungsregeln des Servers hinzufügen](firewall-create-server-level-portal-quickstart.md), um auf Ihre Datenbanken und Data Warehouses zugreifen zu können.

* Wenn Sie auf dem Server eine Private Link-Verbindung eingerichtet haben und von einer IP-Adresse im privaten virtuellen Netzwerk aus eine Verbindung mit dem Abfrage-Editor herstellen, funktioniert der Editor, ohne dass die Client-IP-Adresse den Firewallregeln des SQL-Datenbank-Servers hinzugefügt werden muss.

* Die grundlegendsten beiden RBAC-Berechtigungen, die für die Nutzung des Abfrage-Editors benötigt werden, sind der Lesezugriff auf den Server und auf die Datenbank. Alle Benutzer, die über diese Zugriffsebene verfügen, können auf den Abfrage-Editor zugreifen. Wenn Sie den Zugriff für bestimmte Benutzer beschränken möchten, müssen Sie verhindern, dass diese sich mit Anmeldeinformationen für Azure Active Directory oder die SQL-Authentifizierung beim Abfrage-Editor anmelden können. Falls sich die Benutzer nicht selbst als AAD-Administrator für den Server zuweisen oder auf ein SQL-Administratorkonto zugreifen oder dieses hinzufügen können, sollte es für sie nicht möglich sein, den Abfrage-Editor zu verwenden.

* Das Herstellen einer Verbindung mit der `master`-Datenbank wird vom Abfrage-Editor nicht unterstützt.

* Abfrage-Editor kann mit `ApplicationIntent=ReadOnly` keine Verbindung mit einer Replikatdatenbank herstellen

* Gehen Sie bei Anzeige einer Fehlermeldung wie „X-CSRF-Signature-Header konnte nicht überprüft werden“ wie folgt vor, um das Problem zu beheben:

    * Vergewissern Sie sich, dass die Uhr Ihres Computers auf die richtige Uhrzeit und Zeitzone festgelegt ist. Sie können auch versuchen, die Zeitzone Ihres Computers an Azure anzupassen, indem Sie nach der Zeitzone für den Ort Ihrer Instanz suchen, z. B. „USA, Osten“, Pacific-Region usw.
    * Falls Sie sich in einem Proxynetzwerk befinden, sollten Sie sicherstellen, dass der Anforderungsheader „X-CSRF-Signature“ nicht geändert oder verworfen wird.

### <a name="other-considerations"></a>Weitere Überlegungen

* Durch Drücken von **F5** wird die Seite des Abfrage-Editors aktualisiert, und aktuell bearbeitete Abfragen gehen verloren.

* Bei der Abfrageausführung gilt ein Timeout von fünf Minuten.

* Der Abfrage-Editor unterstützt für geografische Datentypen nur die zylindrische Projektion.

* IntelliSense wird für Datenbanktabellen und -sichten nicht unterstützt. Der Editor unterstützt jedoch AutoVervollständigen für Namen, die schon einmal eingegeben wurden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Unterstützung von Transact-SQL (T-SQL) in Azure SQL-Datenbank finden Sie unter [Auflösen von Transact-SQL-Unterschieden während der Migration zur SQL-Datenbank](transact-sql-tsql-differences-sql-server.md).
