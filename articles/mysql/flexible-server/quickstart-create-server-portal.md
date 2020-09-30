---
title: 'Schnellstart: Erstellen einer Azure Database for MySQL Flexible Server-Instanz mit dem Azure-Portal'
description: Dieser Artikel führt Sie durch die Verwendung des Azure-Portals, um in wenigen Minuten eine Azure Database for MySQL Flexible Server-Instanz zu erstellen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 9/29/2020
ms.openlocfilehash: 70f2cf183a9bd93b6066516cb68e99ee21cdc1ac
ms.sourcegitcommit: f796e1b7b46eb9a9b5c104348a673ad41422ea97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2020
ms.locfileid: "91569636"
---
# <a name="quickstart-use-the-azure-portal-to-create-an-azure-database-for-mysql-flexible-server"></a>Schnellstart: Verwenden des Azure-Portals zum Erstellen einer Azure Database for MySQL Flexible Server-Instanz

Azure Database for MySQL Flexible Server ist ein verwalteter Dienst, mit dem Sie hochverfügbare MySQL-Serverinstanzen in der Cloud ausführen, verwalten und skalieren können. In dieser Schnellstartanleitung erfahren Sie, wie Sie mit dem Azure-Portal eine Flexible Server-Instanz erstellen.

> [!IMPORTANT] 
> Azure Database for MySQL Flexible Server befindet sich aktuell in der öffentlichen Vorschau.

Wenn Sie über kein Azure-Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.
Navigieren Sie in Ihrem Webbrowser zum [Azure-Portal](https://portal.azure.com/). Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden. Die Standardansicht ist Ihr Dienstdashboard.

## <a name="create-an-azure-database-for-mysql-flexible-server"></a>Erstellen einer Instanz von Azure Database for MySQL Flexible Server

Eine Flexible Server-Instanz wird mit einer definierten Gruppe von [Compute- und Speicherressourcen](./concepts-compute-storage.md) erstellt. Der Server wird in einer [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/management/overview) erstellt.

Führen Sie die folgenden Schritte aus, um eine Flexible Server-Instanz zu erstellen:

1. Wählen Sie links oben im Portal **Ressource erstellen** (+) aus.

2. Wählen Sie **Datenbanken** > **Azure-Datenbank für MySQL** aus. Sie können auch **MySQL** in das Suchfeld eingeben, um nach dem Dienst zu suchen.

    > :::image type="content" source="./media/quickstart-create-server-portal/navigate-to-mysql.png" alt-text="Option „Azure Database for MySQL“":::

3. Wählen Sie **Flexible Server** als Bereitstellungsoption aus.
     
    > :::image type="content" source="./media/quickstart-create-server-portal/deployment-option.png" alt-text="Option „Azure Database for MySQL“":::    

4. Füllen Sie dann das Formular **Grundlagen** mit den folgenden Informationen aus: 

    > :::image type="content" source="./media/quickstart-create-server-portal/create-form.png" alt-text="Option „Azure Database for MySQL“"::: 
                                    
    |**Einstellung**|**Empfohlener Wert**|**Beschreibung**|
    |---|---|---|
    Subscription|Ihr Abonnementname|Das Azure-Abonnement, das Sie für Ihren Server verwenden möchten. Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement aus, über das die Ressource abgerechnet werden soll.|
    Resource group|*myresourcegroup*| Ein neuer Ressourcengruppenname oder ein bereits vorhandener Name aus Ihrem Abonnement|
    Servername |*mydemoserver*|Ein eindeutiger Name, der Ihre Flexible Server-Instanz identifiziert. Der Domänenname *mysql.database.azure.com* wird an den angegebenen Servernamen angefügt. Der Servername darf nur Kleinbuchstaben, Zahlen und den Bindestrich (-) enthalten. Er muss 3 bis 63 Zeichen umfassen.|
    Administratorbenutzername |*mydemouser*| Ihr eigenes Anmeldekonto für die Verbindungsherstellung mit dem Server. Der Administratoranmeldename darf nicht **azure_superuser**, **admin**, **administrator**, **root**, **guest** oder **public** lauten.|
    Kennwort |Ihr Kennwort| Ein neues Kennwort für das Serveradministratorkonto. Es muss zwischen acht und 128 Zeichen lang sein. Das Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: Englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0 bis 9) und nicht alphanumerische Zeichen (!, $, #, % usw.).|
    Region|Die Region, die Ihren Benutzern am nächsten liegt| Der Standort, der Ihren Benutzern am nächsten ist.|
    Version|5.7| MySQL-Hauptversion|
    Compute und Speicher | **Burstfähig**, **Standard_B1ms**, **10 GiB**, **7 Tage** | Die Compute-, Speicher- und Sicherungskonfigurationen für Ihren neuen Server. Wählen Sie **Server konfigurieren** aus. *Burstfähig*, *Standard_B1ms*, *10 GiB* und *7 Tage* sind die Standardwerte für **Computetarif**, **Computegröße**, **Speicher** und **Aufbewahrungszeitraum für Sicherung**. Sie können diese Schieberegler unverändert lassen oder sie anpassen. Um die Compute- und Speicherauswahl zu speichern und die Konfiguration fortzusetzen, klicken Sie auf **Speichern**. Der folgende Screenshot zeigt die Compute- und Speicheroptionen.|
    
    > :::image type="content" source="./media/quickstart-create-server-portal/compute-storage.png" alt-text="Option „Azure Database for MySQL“":::

5. Konfigurieren von Netzwerkoptionen

    Auf der Registerkarte „Netzwerk“ können Sie auswählen, wie der Server erreichbar sein soll. Azure Database for MySQL Flexible Server bietet mit *Öffentlicher Zugriff (zugelassene IP-Adressen)* und *Privater Zugriff (VNET-Integration)* zwei Optionen zum Herstellen einer Verbindung mit Ihrer Serverinstanz. Mit *Öffentlicher Zugriff (zugelassene IP-Adressen)* ist der Zugriff auf Ihren Server auf zugelassene IP-Adressen beschränkt, die einer Firewallregel hinzugefügt werden. Sie verhindert, dass externe Anwendungen und Tools eine Verbindung mit der Serverinstanz oder mit Datenbanken auf dem Server herstellen – es sei denn, Sie erstellen eine Regel, um die Firewall für bestimmte IP-Adressen oder einen bestimmten IP-Adressbereich zu öffnen. Mit *Privater Zugriff (VNET-Integration)* ist der Zugriff auf Ihren Server auf Ihr virtuelles Netzwerk beschränkt. In dieser Schnellstartanleitung erfahren Sie, wie Sie den öffentlichen Zugriff aktivieren, um eine Verbindung mit der Serverinstanz herzustellen. Im Artikel zu [Konzepten](./concepts-networking.md) erfahren Sie mehr über Konnektivitätsmethoden.

    > [!NOTE]
    > Die Konnektivitätsmethode kann nicht geändert werden, nachdem der Server erstellt wurde. Wenn Sie z. B. während der Erstellung *Privater Zugriff (VNET-Integration)* ausgewählt haben, können Sie nach der Erstellung nicht zu *Öffentlicher Zugriff (zugelassene IP-Adressen)* wechseln. Es wird dringend empfohlen, eine Serverinstanz mit privatem Zugriff zu erstellen, um mithilfe der VNET-Integration sicher auf den Server zugreifen zu können. Weitere Informationen zum privaten Zugriff finden Sie im Artikel zu [Konzepten](./concepts-networking.md).

    > :::image type="content" source="./media/quickstart-create-server-portal/networking.png" alt-text="Option „Azure Database for MySQL“":::  

6. Wählen Sie **Überprüfen + erstellen** aus, um Ihre Flexible Server-Konfiguration zu überprüfen.

7. Wählen Sie **Erstellen** aus, um den Server bereitzustellen. Die Bereitstellung kann einige Minuten dauern.

8. Wählen Sie auf der Symbolleiste die Option **Benachrichtigungen** (Glockensymbol) aus, um den Bereitstellungsprozess zu überwachen. Nach Abschluss der Bereitstellung können Sie auf **An Dashboard anheften** klicken, um auf Ihrem Azure-Portal-Dashboard eine Kachel für die Flexible Server-Instanz zu erstellen, über die Sie direkt zur **Übersicht** des Servers gelangen. Wenn Sie auf **Zu Ressource wechseln** klicken, wird die **Übersicht** des Servers geöffnet.

Unter Ihrem Server werden standardmäßig folgende Datenbanken erstellt: **information_schema**, **mysql**, **performance_schema** und **sys**.

> [!NOTE]
> Überprüfen Sie, ob Ihr Netzwerk ausgehenden Datenverkehr über Port 3306 zulässt. Dieser Port wird von Azure Database for MySQL Flexible Server verwendet, um Verbindungsprobleme zu vermeiden.  

## <a name="connect-to-using-mysql-command-line-client"></a>Herstellen einer Verbindung mithilfe des mysql-Befehlszeilenclients

Wenn Sie Ihre Flexible Server-Instanz mit *Privater Zugriff (VNET-Integration)* erstellt haben, müssen Sie eine Verbindung mit der Serverinstanz über eine Ressource innerhalb desselben virtuellen Netzwerks herstellen. Sie können einen virtuellen Computer erstellen und zum virtuellen Netzwerk hinzufügen, das mit Ihrer Flexible Server-Instanz erstellt wurde.

Wenn Sie Ihre Flexible Server-Instanz mit *Öffentlicher Zugriff (zulässige IP-Adressen)* erstellt haben, können Sie die lokale IP-Adresse der Liste der Firewallregeln für die Serverinstanz hinzufügen.

Sie können [mysql.exe](https://dev.mysql.com/doc/refman/8.0/en/mysql.html) oder [MySQL Workbench](./connect-workbench.md) auswählen, um aus Ihrer lokalen Umgebung eine Verbindung mit dem Server herzustellen. 

Stellen Sie mit mysql.exe mithilfe des folgenden Befehls eine Verbindung her. Ersetzen Sie die Werte durch den tatsächlichen Servernamen und das Kennwort. 

```bash
 mysql -h mydemoserver.mysql.database.azure.com -u mydemouser -p
```
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Sie haben erfolgreich eine Azure Database for MySQL Flexible Server-Instanz in einer Ressourcengruppe erstellt.  Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, können Sie sie löschen, indem Sie die Ressourcengruppe oder einfach den MySQL-Server löschen. Um die Ressourcengruppe zu löschen, gehen Sie wie folgt vor:

1. Suchen Sie im Azure-Portal nach **Ressourcengruppen**, und wählen Sie die entsprechende Option aus.
1. Wählen Sie in der Liste der Ressourcengruppen den Namen Ihrer Ressourcengruppe aus.
1. Wählen Sie auf der Seite „Übersicht“ der Ressourcengruppe die Option **Ressourcengruppe löschen** aus.
1. Geben Sie im Bestätigungsdialogfeld den Namen Ihrer Ressourcengruppe ein, und wählen Sie **Löschen** aus.

Zum Löschen des Servers können Sie auf der Seite **Übersicht** Ihres Servers auf die Schaltfläche **Löschen** klicken, wie hier gezeigt:

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-server-portal/delete-server.png" alt-text="Option „Azure Database for MySQL“":::

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer PHP-Web-App (Laravel) mit MySQL](tutorial-php-database-app.md)
