---
title: Konfigurieren von Azure SQL Edge (Vorschau)
description: Informationen zum Konfigurieren von Azure SQL Edge (Vorschau)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: c38bb6100665cc9456b66608660bdca520b934c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84636239"
---
# <a name="configure-azure-sql-edge-preview"></a>Konfigurieren von Azure SQL Edge (Vorschau)

Azure SQL Edge unterstützt die Konfiguration mit einer der folgenden beiden Optionen:

- Umgebungsvariablen
- Verwenden der Datei „mssql.conf“, die im Ordner „/var/opt/mssql“ abgelegt wurde.

> [!NOTE]
> Durch das Festlegen von Umgebungsvariablen werden die in der Datei „mssql.conf“ angegebenen Einstellungen überschrieben.

## <a name="configure-by-using-environment-variables"></a>Konfigurieren mithilfe von Umgebungsvariablen

Azure SQL Edge stellt mehrere verschiedene Umgebungsvariablen bereit, mit denen der SQL Edge-Container konfiguriert werden kann. Diese Umgebungsvariablen sind eine Teilmenge derjenigen, die unter Linux für SQL Server verfügbar sind. Weitere Informationen zu Umgebungsvariablen für SQL Server unter Linux finden Sie unter [Umgebungsvariablen](/sql/linux/sql-server-linux-configure-environment-variables/).

Die folgenden Umgebungsvariablen für SQL Server unter Linux werden von Azure SQL Edge nicht unterstützt. Falls definiert, werden sie während der Containerinitialisierung ignoriert.

| Umgebungsvariable | BESCHREIBUNG |
|-----|-----|
| **MSSQL_ENABLE_HADR** | Aktivieren der Verfügbarkeitsgruppe. Beispielsweise bedeutet **1** aktiviert und **0** deaktiviert. |

> [!IMPORTANT]
> Die **MSSQL_PID**-Umgebungsvariable für SQL Edge akzeptiert nur **Premium** und **Developer** als gültige Werte. Azure SQL Edge unterstützt keine Initialisierung mithilfe eines Product Key.

> [!NOTE]
> Laden Sie die [Microsoft-Software-Lizenzbedingungen](https://go.microsoft.com/fwlink/?linkid=2128283) für Azure SQL Edge herunter.

### <a name="specify-the-environment-variables"></a>Angeben der Umgebungsvariablen

Geben Sie Umgebungsvariablen für SQL Edge an, wenn Sie den Dienst über das [Azure-Portal](deploy-portal.md) bereitstellen. Sie können sie entweder im Abschnitt **Umgebungsvariablen** der Modulbereitstellung oder als Teil der **Optionen für die Containererstellung** hinzufügen.

Fügen Sie Werte in **Umgebungsvariablen** hinzu.

![Festlegen mithilfe einer Liste mit Umgebungsvariablen](media/configure/set-environment-variables.png)

Fügen Sie Werte in **Optionen für die Containererstellung** hinzu.

![Festlegen mithilfe von Optionen für die Containererstellung](media/configure/set-environment-variables-using-create-options.png)

## <a name="configure-by-using-an-mssqlconf-file"></a>Konfigurieren mithilfe der Datei „mssql.conf“

Azure SQL Edge enthält nicht wie SQL Server unter Linux das [Konfigurationshilfsprogramm mssql-conf](/sql/linux/sql-server-linux-configure-mssql-conf/). Sie müssen die Datei „mssql.conf“ manuell konfigurieren und auf dem persistenten Speicherlaufwerk ablegen, das dem Ordner „/var/opt/mssql/“ im Modul „SQL Edge“ zugeordnet ist. Beim Bereitstellen von SQL Edge über Azure Marketplace wird diese Zuordnung als Option **Mounts** in den **Optionen zur Containererstellung** angegeben.

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

Die folgenden Optionen von „mssql.conf“ gelten nicht für SQL Edge:

|Option|BESCHREIBUNG|
|:---|:---|
|**Kundenfeedback** | Festlegen, ob SQL Server Feedback an Microsoft sendet. |
|**Datenbank-E-Mail-Profil** | Festlegen des Standardprofils von Datenbank-E-Mails für SQL Server für Linux. |
|**Hochverfügbarkeit** | Aktivieren von Verfügbarkeitsgruppen. |
|**Microsoft Distributed Transaction Coordinator** | Konfigurieren von MS DTC unter Linux und Durchführen einer Problembehandlung. Zusätzliche Konfigurationsoptionen für verteilte Transaktionen werden für SQL Edge ebenfalls nicht unterstützt. Weitere Informationen zu diesen zusätzlichen Konfigurationsoptionen finden Sie unter [Konfigurieren von MSDTC](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#msdtc). |
|**Lizenzbedingungen für ML Services** | Akzeptieren Sie für Azure Machine Learning-Pakete die EULAs für R und Python. Gilt nur für SQL Server 2019.|
|**outboundnetworkaccess** |Aktivieren des Zugriffs auf ausgehenden Netzwerkdatenverkehr für R-, Python- und Java-Erweiterungen für [Machine Learning Services](/sql/linux/sql-server-linux-setup-machine-learning/).|

Die folgende Beispieldatei für „mssql.conf“ funktioniert für SQL Edge. Weitere Informationen zum Format der Datei „mssql.conf“ finden Sie unter [mssql.conf-Format](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#mssql-conf-format).

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

## <a name="next-steps"></a>Nächste Schritte

- [Herstellen einer Verbindung mit Azure SQL Edge](connect.md)
- [Erstellen einer End-to-End-IoT-Lösung mit SQL Edge](tutorial-deploy-azure-resources.md)
