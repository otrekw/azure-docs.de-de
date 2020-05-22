---
title: Konfigurieren von Azure SQL Edge (Vorschau)
description: Informationen zum Konfigurieren von Azure SQL Edge (Vorschau)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 5dcdd1604674ca56fb7a646d4c571d63bd2c0e3e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594009"
---
# <a name="configure-azure-sql-edge-preview"></a>Konfigurieren von Azure SQL Edge (Vorschau)

Azure SQL Edge unterstützt die Konfiguration mit einer der folgenden beiden Optionen:

- Verwenden von Umgebungsvariablen.
- Verwenden der Datei „mssql.conf“, die im Ordner „/var/opt/mssql“ abgelegt wurde.

> [!NOTE]
> Durch das Festlegen von Umgebungsvariablen werden die in der Datei „mssql.conf“ angegebenen Einstellungen überschrieben.

## <a name="configure-using-environment-variables"></a>Konfigurieren mithilfe von Umgebungsvariablen

Azure SQL Edge stellt mehrere verschiedene Umgebungsvariablen bereit, mit denen der SQL Edge-Container konfiguriert werden kann. Diese Umgebungsvariablen sind eine Teilmenge der Umgebungsvariablen, die für SQL Server unter Linux verfügbar sind. Weitere Informationen zu Umgebungsvariablen für SQL Server für Linux unter Linux finden Sie unter [Umgebungsvariablen](/sql/linux/sql-server-linux-configure-environment-variables/).

Die folgenden Umgebungsvariablen für SQL Server unter Linux werden für Azure SQL Edge nicht unterstützt. Wenn diese Umgebungsvariablen definiert sind, werden sie während der Containerinitialisierung ignoriert.

| Umgebungsvariable | BESCHREIBUNG |
|-----|-----|
| **MSSQL_ENABLE_HADR** | Aktivieren der Verfügbarkeitsgruppe. Beispielsweise bedeutet „1“ aktiviert und „0“ deaktiviert. |

> [!IMPORTANT]
> Die *MSSQL_PID*-Umgebungsvariable für SQL Edge akzeptiert nur **Premium** und **Developer** als gültige Werte. Azure SQL Edge unterstützt keine Initialisierung mithilfe eines Product Key.

> [!NOTE]
> Informationen zum Herunterladen des Endbenutzerlizenzvertrags für Azure SQL Edge finden Sie unter [Endbenutzerlizenzvertrag](https://go.microsoft.com/fwlink/?linkid=2128283).

### <a name="specifying-the-environment-variables"></a>Angeben der Umgebungsvariablen

Umgebungsvariablen für SQL Edge können beim Bereitstellen von Azure SQL Edge über das [Azure-Portal](deploy-portal.md) angegeben werden. Diese können entweder im Abschnitt „Environment Variables“ (Umgebungsvariablen) der Modulbereitstellung oder als Teil der Containererstellungsoption (wie unten beschrieben) hinzugefügt werden.

*Festlegen mit Umgebungsvariablenoptionen*

![Festlegen mit Umgebungsvariablenliste](media/configure/set-environment-variables.png)

*Festlegen mit Containererstellungsoptionen*

![Festlegen mit Containererstellungsoptionen](media/configure/set-environment-variables-using-create-options.png)

## <a name="configure-using-mssqlconf-file"></a>Konfigurieren mithilfe der Datei „mssql.conf“

In Azure SQL Edge ist das [mssql-conf-Konfigurationshilfsprogramm](/sql/linux/sql-server-linux-configure-mssql-conf/) nicht wie in SQL Server unter Linux enthalten. Daher muss die Datei „mssql.conf“ manuell konfiguriert und auf dem permanenten Speicherlaufwerk abgelegt werden, das im SQL Edge-Modul dem Ordner „/var/opt/mssql/“ zugeordnet ist. Beim Bereitstellen von SQL Edge aus Azure Marketplace wird diese Zuordnung als Option „Mounts“ in der Containererstellungsoption angegeben.

```json
    {
        "Mounts": [
          {
            "Type": "volume",
            "Source": "sqlvolume",
            "Target": "/var/opt/mssql"
          }
        ]
      }
    }
```

Die folgenden mssql.conf-Optionen sind nicht auf SQL Edge anwendbar:
</br></br>
|Option|BESCHREIBUNG|
|:---|:---|
|**Kundenfeedback** | Festlegen, ob SQL Server Feedback an Microsoft sendet. |
|**Datenbank-E-Mail-Profil** | Festlegen des Standardprofils von Datenbank-E-Mails für SQL Server für Linux. |
|**Hochverfügbarkeit** | Aktivieren von Verfügbarkeitsgruppen. |
|**Microsoft Distributed Transaction Coordinator** | Konfigurieren von MS DTC unter Linux und Durchführen einer Problembehandlung. Zusätzliche Konfigurationsoptionen für verteilte Transaktionen werden für SQL Edge ebenfalls nicht unterstützt. Weitere Informationen zu diesen zusätzlichen Konfigurationsoptionen finden Sie unter [Konfigurieren von MSDTC](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#msdtc). |
|**Lizenzbedingungen für ML Services** | Akzeptieren der R- und Python-EULAs für Machine Learning Services-Pakete. Gilt nur für SQL Server 2019.|
|**outboundnetworkaccess** |Aktivieren des Zugriffs auf ausgehenden Netzwerkdatenverkehr für R-, Python- und Java-Erweiterungen für [Machine Learning Services](/sql/linux/sql-server-linux-setup-machine-learning/).|

Eine mssql.conf-Beispieldatei, die für SQL Edge funktioniert, wird unten bereitgestellt. Weitere Informationen zum Format der Datei „mssql.conf“ finden Sie unter [mssql.conf-Format](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#mssql-conf-format).

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/backup/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/log/

[language]
lcid = 1033

[memory]
memorylimitmb = 6144

[sqlagent]
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[traceflag]
traceflag0 = 3604
traceflag1 = 3605
traceflag2 = 1204
```

## <a name="next-step"></a>Nächster Schritt

- [Herstellen einer Verbindung mit Azure SQL Edge](connect.md)
- [Erstellen einer End-to-End-IoT-Lösung mit SQL Edge](tutorial-deploy-azure-resources.md)
