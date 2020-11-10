---
title: 'Schnellstart: Erstellen eines Servers – Azure-Portal – Azure Database for MySQL'
description: Diese Schnellstartanleitung führt Sie durch die Verwendung des Azure-Portals, um in etwa fünf Minuten schnell einen Azure Database for MySQL-Server zu erstellen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 7/15/2020
ms.openlocfilehash: 2cae0187643eb596bd98bcd99a588a4d214e6f6e
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93341211"
---
# <a name="quickstart-create-an-azure-database-for-mysql-server-in-the-azure-portal"></a>Schnellstart: Erstellen eines Azure Database for MySQL-Servers im Azure-Portal

Azure-Datenbank für MySQL ist ein verwalteter Dienst, mit dem Sie hochverfügbare MySQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal in etwa fünf Minuten einen Azure Database for MySQL-Server erstellen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Navigieren Sie in Ihrem Webbrowser zum [Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-an-azure-database-for-mysql-server"></a>Erstellen eines Servers für Azure-Datenbank für MySQL
Ein Server für Azure-Datenbank für MySQL wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-pricing-tiers.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](../azure-resource-manager/management/overview.md) erstellt.

Führen Sie die folgenden Schritte aus, um einen Server für Azure-Datenbank für MySQL zu erstellen:

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für MySQL** aus. Sie können auch **MySQL** in das Suchfeld eingeben, um nach dem Dienst zu suchen.

  
>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png" alt-text="Option „Azure Database for MySQL“":::

3. Geben Sie im Formular für den neuen Server folgende Informationen an:
    
>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/4-create-form.png" alt-text="Formular für die Servererstellung":::

**Einstellung** | **Empfohlener Wert** | **Feldbeschreibung** 
---|---|---
Subscription | Ihr Abonnement | Wählen Sie das Azure-Abonnement aus, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet wird.
Resource group | *myresourcegroup* | Geben Sie einen neuen oder vorhandenen Ressourcengruppennamen an. Die Ressourcengruppe kann verwendet werden, um die Abhängigkeiten zu organisieren, die zu einem einzelnen Projekt gehören.
Servername | Eindeutiger Servername | Geben Sie einen eindeutigen Namen ein, der Ihren Azure Database for MySQL-Server identifiziert. Beispiel: „mysqldbserver“. Der Servername darf nur Kleinbuchstaben, Ziffern und den Bindestrich (-) enthalten. Es muss zwischen drei und 63 Zeichen lang sein.
Datenquelle |*None* | Wählen Sie *Keine* aus, um einen neuen Server ohne Vorlage zu erstellen. (Die Option *Sicherung* können Sie auswählen, wenn Sie einen Server auf der Grundlage einer Geosicherung eines vorhandenen Azure Database for MySQL-Servers erstellen.)
Serveradministratoranmeldung | myadmin | Geben Sie einen Benutzernamen für Ihren Serveradministrator ein. Der Benutzername des Administrators darf nicht auf **azure_superuser** , **admin** , **administrator** , **root** , **guest** oder **public** festgelegt werden.
Kennwort | *Beliebig* | Geben Sie ein neues Kennwort für das Serveradministratorkonto an. Das Kennwort muss zwischen 8 und 128 Zeichen lang sein und eine Kombination aus Groß- oder Kleinbuchstaben, Ziffern und nicht alphanumerischen Zeichen (!, $, #, % usw.) aufweisen.
Kennwort bestätigen | *Beliebig*| Bestätigen Sie das Kennwort des Administratorkontos.
Position | *Die Region, die Ihren Benutzern am nächsten liegt*| Wählen Sie den Standort aus, der Ihren Benutzern oder anderen Azure-Anwendungen am nächsten liegt.
Version | *Die aktuelle Hauptversion*| Die aktuelle Hauptversion (es sei denn, Sie haben besondere Anforderungen, die eine andere Version erfordern).
Compute + Speicher | **Universell** , **Gen 5** , **2 virtuelle Kerne** , **5 GB** , **7 Tage** , **Georedundant** |Die Compute-, Speicher- und Sicherungskonfigurationen für Ihren neuen Server. Wählen Sie **Server konfigurieren** aus. Wählen Sie nun den entsprechenden Tarif aus. Weitere Informationen hierzu finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/mysql/). Wählen Sie zum Aktivieren der Serversicherungen in georedundantem Speicher unter **Optionen für Sicherungsredundanz** die Option **Georedundant** aus. Klicken Sie auf **OK**.

   > [!NOTE]
   > Ziehen Sie die Verwendung des Tarifs „Basic“ in Betracht, wenn geringe Compute- und E/A-Leistung für Ihre Workload angemessen ist. Beachten Sie, dass im Tarif „Basic“ erstellte Server später nicht auf „Universell“ oder „Arbeitsspeicheroptimiert“ skaliert werden können. 

4. Wählen Sie **Bewerten + erstellen** aus, um den Server bereitzustellen. Die Bereitstellung kann bis zu 20 Minuten dauern.
   
5. Wählen Sie auf der Symbolleiste die Option **Benachrichtigungen** (Glockensymbol) aus, um den Bereitstellungsprozess zu überwachen.
   
Unter Ihrem Server werden standardmäßig folgende Datenbanken erstellt: **information_schema** , **mysql** , **performance_schema** und **sys**.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurieren einer Firewallregel auf Serverebene
Der erstellte Server wird standardmäßig durch eine Firewall geschützt und ist nicht öffentlich zugänglich. Wenn Sie den Zugriff auf Ihre IP-Adresse gewähren möchten, navigieren Sie im Azure-Portal zu Ihrer Serverressource, und wählen Sie im Menü auf der linken Seite die Option **Verbindungssicherheit** für Ihre Serverressource aus. Wenn Sie nicht wissen, wie Sie Ihre Ressource finden können, lesen Sie die Informationen unter [Öffnen von Ressourcen](../azure-resource-manager/management/manage-resources-portal.md#open-resources).

>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/add-current-ip-firewall.png" alt-text="Verbindungssicherheit – Firewallregeln":::
   
Wählen Sie nun **Aktuelle Client-IP-Adresse hinzufügen** und dann **Speichern** aus. Sie können weitere IP-Adressen hinzufügen oder einen IP-Adressbereich angeben, um über diese IP-Adressen eine Verbindung mit dem Server herzustellen. Weitere Informationen finden Sie unter [Verwalten von Firewallregeln für den Azure Database for MySQL-Server](./concepts-firewall-rules.md).

> [!NOTE]
> Überprüfen Sie, ob Ihr Netzwerk ausgehenden Datenverkehr über Port 3306 zulässt. Dieser Port wird von Azure Database for MySQL verwendet, um Verbindungsprobleme zu vermeiden.  

## <a name="connect-to-azure-database-for-mysql-server-using-mysql-command-line-client"></a>Herstellen einer Verbindung mit dem Azure Database for MySQL-Server mithilfe des mysql-Befehlszeilenclients
Sie können [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) oder [MySQL Workbench](./connect-workbench.md) auswählen, um aus Ihrer lokalen Umgebung eine Verbindung mit dem Server herzustellen. In dieser Schnellstartanleitung führen wir **mysql.exe** in [Azure Cloud Shell](../cloud-shell/overview.md) aus, um eine Verbindung mit dem Server herzustellen.

1. Starten Sie Azure Cloud Shell im Portal, indem Sie oben links auf das hervorgehobene Symbol klicken. Notieren Sie sich den Servernamen, den Anmeldenamen des Serveradministrators, das Kennwort und das Abonnement für den neu erstellten Server im Abschnitt **Übersicht** , wie in der nachfolgenden Abbildung gezeigt.

    >[!NOTE]
    >Wenn Sie Cloud Shell zum ersten Mal starten, wird eine Eingabeaufforderung zum Erstellen einer Ressourcengruppe und eines Speicherkontos angezeigt. Dieser Schritt ist nur ein Mal erforderlich. Diese Komponenten werden für alle Sitzungen automatisch angefügt. 

   >[!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-mysql-server-database-using-azure-portal/use-in-cloud-shell.png" alt-text="Vollständige Ansicht von Cloud Shell im Portal":::
2. Führen Sie im Azure Cloud Shell-Terminal den folgenden Befehl aus. Ersetzen Sie die Werte durch den tatsächlichen Servernamen und den Anmeldenamen des Administratorbenutzers. Der Benutzername des Administrators erfordert „@\<servername>“, wie unten für Azure Database for MySQL gezeigt.  

  ```azurecli-interactive
  mysql --host=mydemoserver.mysql.database.azure.com --user=myadmin@mydemoserver -p 
  ```

  So sieht die Anzeige im Cloud Shell-Terminal aus:
  ```
  Requesting a Cloud Shell.Succeeded.
  Connecting terminal...

  Welcome to Azure Cloud Shell

  Type "az" to use Azure CLI
  Type "help" to learn about Cloud Shell

  user@Azure:~$mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p
  Enter password:
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 64796
  Server version: 5.6.42.0 Source distribution

  Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.

  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  mysql>
  ```
3. Erstellen Sie im gleichen Azure Cloud Shell-Terminal eine Datenbank namens **guest**. 
  ```
  mysql> CREATE DATABASE guest;
  Query OK, 1 row affected (0.27 sec)
  ```
4. Wechseln zur Datenbank **guest**
  ```
  mysql> USE guest;
  Database changed 
  ```
5. Geben Sie ```quit``` ein, und drücken Sie anschließend die EINGABETASTE, um mysql zu beenden.   

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie haben erfolgreich einen Azure Database for MySQL-Server in einer Ressourcengruppe erstellt.  Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, können Sie sie löschen, indem Sie die Ressourcengruppe oder einfach den MySQL-Server löschen. Um die Ressourcengruppe zu löschen, gehen Sie wie folgt vor:
1. Suchen Sie im Azure-Portal nach **Ressourcengruppen** , und wählen Sie die entsprechende Option aus. 
2. Wählen Sie in der Liste der Ressourcengruppen den Namen Ihrer Ressourcengruppe aus.
3. Wählen Sie auf der Seite „Übersicht“ der Ressourcengruppe die Option **Ressourcengruppe löschen** aus.
4. Geben Sie im Bestätigungsdialogfeld den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

Zum Löschen des Servers können Sie auf der Seite **Übersicht** Ihres Servers auf die Schaltfläche **Löschen** klicken, wie hier gezeigt:
> [!div class="mx-imgBorder"]
> :::image type="content" source="media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png" alt-text="Löschen Ihrer Ressourcen":::

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
>[Erstellen einer PHP-App unter Windows mit MySQL](../app-service/tutorial-php-mysql-app.md)
