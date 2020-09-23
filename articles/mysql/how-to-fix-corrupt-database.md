---
title: Beheben von Datenbankbeschädigungen – Azure Database for MySQL
description: Erfahren Sie, wie Sie Datenbankbeschädigungsprobleme für Azure Database for MySQL beheben können.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 815b24d25e674e5460cc50d7eb6871f740994893
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931225"
---
# <a name="troubleshoot-database-corruption-on-azure-database-for-mysql"></a>Beheben von Datenbankbeschädigungen für Azure Database for MySQL
[!INCLUDE[applies-to-single-flexible-server](includes/applies-to-single-flexible-server.md)]

Eine Datenbankbeschädigung kann für Ihre Anwendung zu Downtime führen, und es ist auch wichtig, das Problem rechtzeitig zu beheben, um Datenverluste zu vermeiden. Wenn eine Datenbankbeschädigung auftritt, wird der folgende Fehler in Ihren Serverprotokollen angezeigt **InnoDB: Beschädigte Datenbankseite auf Datenträger oder Fehler**.

In diesem Leitfaden erfahren Sie, wie Sie eine beschädigte Datenbank oder Tabelle reparieren können. Azure Database for MySQL verwendet die InnoDB-Engine und bietet automatisierte Vorgänge zur Überprüfung und Reparatur von Beschädigungen. InnoDB prüft auf beschädigte Seiten, indem es auf jeder gelesenen Seite Prüfsummen durchführt, und wenn es eine Abweichung der Prüfsumme feststellt, stoppt es automatisch den MySQL-Server.

Testen Sie eine der folgenden Optionen, um Ihre Probleme mit Datenbankbeschädigungen schnell zu beheben.

## <a name="restart-your-mysql-server"></a>Neustarten des MySQL-Servers

In der Regel stellen Sie fest, dass eine Datenbank oder Tabelle beschädigt ist, wenn Ihre Anwendung auf die Datenbank oder Tabelle zugreift. Da InnoDB einen Mechanismus zur Wiederherstellung nach einem Absturz bietet, der die meisten Probleme beheben kann, wenn der Server neu gestartet wird. Daher sollte ein Neustart des Servers dem Server helfen, sich von einem Absturz zu erholen, der zu einem fehlerhaften Zustand der Datenbank geführt hat.

##  <a name="resolve-data-corruption-with-dump-and-restore-method"></a>Beheben von Datenbeschädigungen mit der Methode „Sichern und Wiederherstellen“

Es wird empfohlen, das Problem beschädigter Daten mit einer **Sichern-und-Wiederherstellen-Methode** zu lösen. Dazu gehört es, Zugriff auf die beschädigte Tabelle zu erhalten, mithilfe des Hilfsprogramms **mysqldump** eine logische Sicherung der Tabelle zu erstellen, die die Tabellenstruktur und die darin enthaltenen Daten beibehält, und dann die Tabelle wieder in die Datenbank zurückzuladen.

### <a name="backup-your-database-or-tables"></a>Sichern der Datenbank oder Tabellen

> [!Important]
> - Stellen Sie sicher, dass Sie eine Firewallregel konfiguriert haben, um von Ihrem Clientcomputer aus auf den Server zugreifen zu können. Erfahren Sie, wie Sie [Firewallregeln auf einem einzelnen Server](howto-manage-firewall-using-portal.md) und [Firewallregel auf einem flexiblen Server](flexible-server/how-to-connect-tls-ssl.md) konfigurieren.
> - Verwenden Sie die SSL-Option ```--ssl-cert``` für **mysqldump**, wenn SSL aktiviert ist.

Erstellen einer Sicherungsdatei über die Befehlszeile „mysqldump“ mit diesem Befehl

```
$ mysqldump [--ssl-cert=/path/to/pem] -h [host] -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Es müssen folgende Parameter bereitgestellt werden:
- [ssl-cert=/path/to/pem] Laden Sie das SSL-Zertifikat auf Ihren Clientcomputer herunter, und geben Sie im Befehl den Pfad darin an. Verwenden Sie dies NICHT, wenn SSL deaktiviert ist.
- [host] ist Ihr Azure Database for MySQL-Server
- [uname] ist der Benutzername des Serveradministrators
- [pass] ist das Kennwort für Ihren Administratorbenutzer
- [dbname] ist der Name Ihrer Datenbank
- [backupfile.sql] ist der Dateiname für die Datenbanksicherung

> [!Important]
> - Verwenden Sie für einzelne Server das Format ```admin-user@servername```, um ```myserveradmin``` in den nachstehenden Befehlen zu ersetzen.
> - Verwenden Sie für flexible Server das Format ```admin-user```, um ```myserveradmin``` in den nachstehenden Befehlen zu ersetzen.

Wenn eine bestimmte Tabelle beschädigt ist, wählen Sie mithilfe dieses Beispiels bestimmte Tabellen in Ihrer Datenbank zur Sicherung aus
```
$ mysqldump --ssl-cert=</path/to/pem> -h mydemoserver.mysql.database.azure.com -u myserveradmin -p testdb table1 table2 > testdb_tables_backup.sql
```

Um eine oder mehrere Datenbanken zu sichern, verwenden Sie den Schalter „--database“, und listen Sie die Datenbanknamen durch Leerzeichen getrennt auf.

```
$ mysqldump --ssl-cert=</path/to/pem>  -h mydemoserver.mysql.database.azure.com -u myserveradmin -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql
```

###  <a name="restore-your-database-or-tables"></a>Wiederherstellen der Datenbank oder Tabellen

Die folgenden Schritte zeigen Ihnen, wie Sie Ihre Datenbank oder Tabellen wiederherstellen können. Sobald die Sicherungsdatei erstellt ist, können Sie die Tabelle oder die Datenbanken mithilfe des ***mysql**-Hilfsprogramms wiederherstellen. Führen Sie den Befehl wie unten gezeigt aus:

```
mysql  --ssl-cert=</path/to/pem> -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Hier folgt ein Beispiel für die Wiederherstellung von ```testdb``` aus einer Sicherungsdatei, die mit **mysqldump** erstellt wurde. 

> [!Important]
> - Verwenden Sie für einzelne Server das Format ```admin-user@servername```, um ```myserveradmin``` in dem nachstehenden Befehl zu ersetzen.
> - Verwenden Sie für flexible Server das Format ```admin-user```, um ```myserveradmin``` in dem nachstehenden Befehl zu ersetzen. 

```
$ mysql --ssl-cert=</path/to/pem> -h mydemoserver.mysql.database.azure.com -u myserveradmin -p testdb < testdb_backup.sql
```

## <a name="next-steps"></a>Nächste Schritte
Wenn die oben genannten Schritte nicht zur Lösung des Problems beitragen, können Sie jederzeit den gesamten Server wiederherstellen.
- [Wiederherstellen von Azure Database for MySQL Single Server](howto-restore-server-portal.md)
- [Wiederherstellen von Azure Database for MySQL Flexible Server](flexible-server/how-to-restore-server-portal.md)



