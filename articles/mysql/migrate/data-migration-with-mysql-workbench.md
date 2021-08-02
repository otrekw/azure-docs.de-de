---
title: 'Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Datenmigration mit MySQL Workbench'
description: Führen Sie alle Schritte in der Setupanleitung aus, um eine Umgebung zu erstellen, die die folgenden Schritte unterstützt.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 05/26/2021
ms.openlocfilehash: 24be41c768c808cb8d33a0fc74b2a950b6111797
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111958660"
---
# <a name="mysql-on-premises-to-azure-database-for-mysql-migration-guide-data-migration-with-mysql-workbench"></a>Leitfaden zur Migration einer lokalen MySQL-Instanz zu Azure Database for MySQL: Datenmigration mit MySQL Workbench

### <a name="setup"></a>Einrichten

Führen Sie alle Schritte in der Setupanleitung aus, um eine Umgebung zu erstellen, die die folgenden Schritte unterstützt.

### <a name="configuring-server-parameters-source"></a>Konfigurieren von Serverparametern (Quelle)

Je nach gewähltem Migrationstyp (offline oder online) empfiehlt es sich gegebenenfalls, zu ermitteln, ob Sie die Serverparameter ändern müssen, um eine schnelle ausgehende Übertragung der Daten zu unterstützen. Bei einer Onlinemigration ist es u. U. nicht erforderlich, Serverparameter zu ändern, da Sie wahrscheinlich eine `binlog`-Replikation durchführen, bei der die Daten automatisch synchronisiert werden. Wenn Sie jedoch eine Offlinemigration durchführen, können Sie die Serverparameter von der Workloadunterstützung auf die Exportunterstützung umstellen, nachdem der Anwendungsdatenverkehr beendet wurde.

### <a name="configuring-server-parameters-target"></a>Konfigurieren von Serverparametern (Ziel)

Überprüfen Sie die Serverparameter, bevor Sie den Importvorgang in Azure Database for MySQL starten. Serverparameter können über das [Azure-Portal](../howto-server-parameters.md) abgerufen und festgelegt werden. Sie können aber auch die [Azure PowerShell für MySQL-Cmdlets](../howto-configure-server-parameters-using-powershell.md) verwenden, um die Änderungen vorzunehmen.

Führen Sie das folgende PowerShell-Skript aus, um alle Parameter abzurufen:

```
\[Net.ServicePointManager\]::SecurityProtocol = \[Net.SecurityProtocolType\]::Tls
12

Install-Module -Name Az.MySql
Connect-AzAccount
$rgName = "{RESOURCE\_GROUP\_NAME}";
$serverName = "{SERVER\_NAME}";
Get-AzMySqlConfiguration -ResourceGroupName $rgName -ServerName $serverName
```

  - Falls Sie das mysql-Tool verwenden möchten, laden Sie das [Stammzertifikat für die Zertifizierungsstelle](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) in „c:\\temp“ herunter. (Das Verzeichnis muss erstellt werden.)

    > [!NOTE] 
    > Das Zertifikat kann sich ändern. Informationen zum aktuellen Zertifikat finden Sie unter [Konfigurieren von SSL-Verbindungen in der Anwendung für eine sichere Verbindung mit Azure Database for MySQL](../howto-configure-ssl.md).

  - Führen Sie den folgenden Befehl an einer Eingabeaufforderung aus, und denken Sie daran, die Token zu aktualisieren:

```
mysql --host {servername}.mysql.database.azure.com --database mysql --user 
{u sername}@{servername} -p --ssl-ca=c:\\temp\\BaltimoreCyberTrustRoot.crt.cer
-A -e "SHOW GLOBAL VARIABLES;" \> c:\\temp\\settings\_azure.txt
```

Die neue Datei \`settings\_azure.txt\` enthält die Standardparameter für den Azure Database for MySQL-Server (siehe Anhang A).

Um die Migration zu unterstützen, legen Sie die MySQL-Zielinstanzparameter so fest, dass eine schnellere eingehende Übertragung ermöglich ist. Vor dem Starten der Datenmigration sollten die folgenden Serverparameter festgelegt werden:

  - `max\_allowed\_packet`: Legen Sie den Parameter auf `1073741824` (1 GB) bzw. auf die größte Größe einer Zeile in der Datenbank fest, um Überlaufprobleme durch lange Zeilen zu vermeiden. Passen Sie diesen Parameter ggf. an, wenn große BLOB-Zeilen gepullt (oder gelesen) werden müssen.

  - `innodb\_buffer\_pool\_size`: Skalieren Sie den Server im Portal auf der Tarifseite während der Migration auf die arbeitsspeicheroptimierte SKU mit 32 virtuellen Kernen hoch, um „innodb\_buffer\_pool\_size“ zu erhöhen. „Innodb\_buffer\_pool\_size“ kann nur durch Hochskalieren von Computeressourcen für den Azure Database for MySQL-Server erhöht werden. Informationen zum maximalen Wert für den Tarif finden Sie unter [Serverparameter in Azure Database for MySQL](../concepts-server-parameters.md#innodb_buffer_pool_size). Der maximale Wert in einem arbeitsspeicheroptimiertem System mit 32 virtuellen Kernen ist `132070244352`.

  - `innodb\_io\_capacity` & `innodb\_io\_capacity\_max`: Ändern Sie den Parameter in `9000`, um die E/A-Nutzung zu verbessern und die Migrationsgeschwindigkeit zu optimieren.

  - `max\_connections`: Wenn Sie ein Tool verwenden, das mehrere Threads generiert, um den Durchsatz zu erhöhen, erhöhen Sie die Anzahl von Verbindungen, um das Tool zu unterstützen. Der Standardwert ist `151`, der maximale Wert ist `5000`.

    > [!NOTE] 
    > Führen Sie die Skalierung mit Bedacht durch. Bestimmte Vorgänge (etwa die Speicherskalierung) können nicht rückgängig gemacht werden.

Diese Einstellungen können mithilfe der folgenden Azure PowerShell-Cmdlets aktualisiert werden:

```
Install-Module -Name Az.MySql
$rgName = " {RESOURCE\_GROUP\_NAME}";
$serverName = "{SERVER\_NAME}";

Select-AzSubscription -Subscription "{SUBSCRIPTION\_ID}"
Update-AzMySqlConfiguration -Name max\_allowed\_packet -ResourceGroupName
$rgna me -ServerName $serverName -Value 1073741824  
Update-AzMySqlConfiguration -Name innodb\_buffer\_pool\_size -ResourceGroupName 
$rgname -ServerName $serverName -Value 16106127360
Update-AzMySqlConfiguration -Name innodb\_io\_capacity -ResourceGroupName 
$rgna me -ServerName $serverName -Value 9000
Update-AzMySqlConfiguration -Name innodb\_io\_capacity\_max -ResourceGroupName 
$ rgname -ServerName $serverName -Value 9000

\#required if you have functions

Update-AzMySqlConfiguration -Name log\_bin\_trust\_function\_creators 
-ResourceGr oupName $rgname -ServerName $serverName -Value ON
```
### <a name="data"></a>Daten

#### <a name="tool-choice"></a>Toolauswahl

Nachdem die Datenbankobjekte und Benutzer aus dem Quellsystem migriert wurden, kann die Migration beginnen. Für Datenbanken unter der MySQL-Version 8.0 kann Azure DMS nicht zum Migrieren der Workload verwendet werden. Stattdessen müssen Migrationsbenutzer MySQL Workbench verwenden.

#### <a name="manual-import-and-export-steps"></a>Manuelle Import- und Exportschritte

  - Öffnen Sie MySQL Workbench, und stellen Sie eine Verbindung als Stammbenutzer der lokalen Datenbank her.

  - Wählen Sie unter \*\*Verwaltung\*\* die Option \*\*Datenexport\*\* aus. Wählen Sie das Schema „**reg\_app**“ aus.

  - Wählen Sie unter **Zu exportierende Objekte** die Optionen **Gespeicherte Prozeduren und Funktionen sichern**, **Ereignisse sichern** und **Trigger sichern** aus.

  - Wählen Sie unter **Exportoptionen** die Option **In eigenständige Datei exportieren** aus.

  - Aktivieren Sie außerdem das Kontrollkästchen **Erstellungsschema einschließen**. Die folgende Abbildung zeigt die korrekte mysqldump-Konfiguration:

    ![Einschließen des Erstellungsschemas](./media/image6.jpg)

    **Test**

  - Sollte eine dieser Optionen nicht verfügbar ist, ist sie wahrscheinlich durch den Ausgabebereich verdeckt. Ändern Sie einfach das Editorlayout.

    ![Editorlayout](./media/image7.jpg)

    **Test**

  - Wählen Sie die Registerkarte **Exportfortschritt** aus.

  - Wählen Sie **Export starten** aus. Daraufhin werden von MySQL Workbench Aufrufe an das Tool `mysqldump` übermittelt.

  - Öffnen Sie das neu erstellte Exportskript.

  - Ändern Sie alle Anweisungen vom Typ `DEFINER` entweder in einen gültigen Benutzer, oder entfernen Sie sie vollständig.

    > [!NOTE] 
    > Hierzu können Sie `--skip-definer` im mysqldump-Befehl übergeben. In MySQL Workbench ist das nicht möglich. Daher müssen die Zeilen in den Exportbefehlen manuell entfernt werden. Wir weisen hier auf vier Elemente hin, die entfernt werden müssen. Es kann aber noch weitere Elemente geben, die möglicherweise bei der Migration zwischen MySQL-Versionen Fehler verursachen (z. B. neue reservierte Wörter).

  - Ändern Sie Anweisungen vom Typ `SET GLOBAL` entweder in einen gültigen Benutzer, oder entfernen Sie sie vollständig.

  - Achten Sie darauf, dass `sql\_mode` nicht auf `NO\_AUTO\_CREATE\_USER` festgelegt ist.

  - Entfernen Sie die Funktion `hello\_world`.

  - Erstellen Sie in MySQL Workbench eine neue Verbindung mit Azure Database for MySQL.

      - Geben Sie als Hostnamen den vollständigen Server-DNS ein (z. B. `servername.mysql.database.azure.com`).

      - Geben Sie den Benutzernamen ein (z. B. `sqlroot@servername`).

      - Wählen Sie die Registerkarte **SSL** aus.

      - Navigieren Sie für die SSL-Zertifizierungsstellendatei zur Schlüsseldatei **BaltimoreCyberTrustRoot.crt.cer**.

      - Wählen Sie **Verbindung testen** aus, und vergewissern Sie sich, dass die Verbindung erfolgreich hergestellt wird.

      - Klicken Sie auf **OK**.

        ![Dialogfeld „MySQL-Verbindung“](./media/image8.jpg)

        **Das Dialogfeld „MySQL-Verbindung“ wird angezeigt.**

  - Wählen Sie **Datei \> SQL-Skript öffnen** aus.

  - Navigieren Sie zur Sicherungsdatei, und wählen Sie **Öffnen** aus.

  - Wählen Sie **Execute**(Ausführen).

### <a name="update-applications-to-support-ssl"></a>Aktualisieren von Anwendungen zur Unterstützung von SSL

  - Wechseln Sie in Visual Studio Code zur Java Server-API.

  - Öffnen Sie die Datei **launch.json**.

  - Aktualisieren Sie **DB\_CONNECTION\_URL** in `jdbc:mysql://serverDNSname:3306/reg\_app?useUnicode=true\&useJDBCCompliantT imezoneShift=true\&useLegacyDatetimeCode=false\&serverTimezone=UTC\&verifySe rverCertificate=true\&useSSL=true\&requireSSL=true\&noAccessToProcedureBodie s=true.` Beachten Sie die zusätzlichen SSL-Parameter.

  - Aktualisieren Sie **DB\_USER\_NAME** in **conferenceuser@servername** .

  - Starten Sie die Debugkonfiguration, und vergewissern Sie sich, dass die Anwendung lokal mit der neuen Datenbank funktioniert.

### <a name="revert-server-parameters"></a>Zurücksetzen von Serverparametern

Die folgenden Parameter können in der Azure Database for MySQL-Zielinstanz geändert werden. Diese Parameter können über das Azure-Portal oder mithilfe der [Azure PowerShell für MySQL-Cmdlets](../howto-configure-server-parameters-using-powershell.md) festgelegt werden.

```
$rgName = "YourRGName";
$serverName = "servername";
Update-AzMySqlConfiguration -Name max\_allowed\_packet -ResourceGroupName 
$rgna me -ServerName $serverName -Value 536870912
Update-AzMySqlConfiguration -Name innodb\_buffer\_pool\_size -ResourceGroupName 
$rgname -ServerName $serverName -Value 16106127360
Update-AzMySqlConfiguration -Name innodb\_io\_capacity -ResourceGroupName $rgna 
me -ServerName $serverName -Value 200
Update-AzMySqlConfiguration -Name innodb\_io\_capacity\_max -ResourceGroupName 
$ rgname -ServerName $serverName -Value 2000
```
### <a name="change-connection-string-for-the-java-api"></a>Ändern der Verbindungszeichenfolge für die Java-API

  - Verwenden Sie die folgenden Befehle, um die Verbindungszeichenfolge für die App Service-Java-API zu ändern:

```
$rgName = "YourRGName"; 
$app_name = "servername";
az webapp config appsettings set -g $rgName -n $app_name 
--settings DB_CONNECTION_URL={DB_CONNECTION_URL}
```

> [!NOTE] 
> Denken Sie daran, dass Sie die Verbindungszeichenfolge über das Portal festlegen können.

  - Starten Sie die App Service-API neu.

```
az webapp restart -g $rgName -n $app\_name
```
Sie haben erfolgreich eine Migration einer lokalen Instanz zu Azure Database for MySQL durchgeführt.  


> [!div class="nextstepaction"]  
> [Verwaltung nach der Migration](./post-migration-management.md)