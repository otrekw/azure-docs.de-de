---
title: Behandlung von Konnektivitätsproblemen
description: Behandlung von Konnektivitätsproblemen in SQL Analytics
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/27/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 689a2e549c2627c607b6549f164e55a73318f63e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350045"
---
# <a name="troubleshooting-connectivity-issues"></a>Behandlung von Konnektivitätsproblemen

In diesem Artikel werden allgemeine Problembehandlungsverfahren für das Herstellen von Verbindungen mit Ihrer SQL Analytics-Datenbank aufgeführt.
- [Überprüfen der Verfügbarkeit des Diensts](sql-data-warehouse-troubleshoot-connectivity.md#check-service-availability)
- [Überprüfen auf angehaltene Vorgänge oder Skalierungen](sql-data-warehouse-troubleshoot-connectivity.md#check-for-paused-or-scaling-operation)
- [Überprüfen der Firewalleinstellungen](sql-data-warehouse-troubleshoot-connectivity.md#check-your-firewall-settings)
- [Überprüfen der Einstellungen für VNETs und Dienstendpunkte](sql-data-warehouse-troubleshoot-connectivity.md#check-your-vnetservice-endpoint-settings)
- [Überprüfen der Treiber auf Aktualität](sql-data-warehouse-troubleshoot-connectivity.md#check-for-the-latest-drivers)
- [Überprüfen der Verbindungszeichenfolge](sql-data-warehouse-troubleshoot-connectivity.md#check-your-connection-string)
- [Zeitweilige Verbindungsprobleme](sql-data-warehouse-troubleshoot-connectivity.md#intermittent-connection-issues)
- [Häufige Fehlermeldungen](sql-data-warehouse-troubleshoot-connectivity.md#common-error-messages)

## <a name="check-service-availability"></a>Überprüfen der Verfügbarkeit des Diensts

Überprüfen Sie, ob der Dienst verfügbar ist. Navigieren Sie im Azure-Portal zu der SQL Analytics-Datenbank, mit der Sie eine Verbindung herstellen möchten. Klicken Sie im Inhaltsverzeichnis auf der linken Seite auf **Diagnose und Problembehandlung**.

![Auswählen der Ressourcenintegrität](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

Der Status Ihres SQL Analytics wird hier angezeigt. Wenn der Dienst nicht als **Verfügbar** angezeigt wird, sind weitere Handlungen erforderlich.

![Dienst verfügbar](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

Wenn Ihre Ressourcenintegrität anzeigt, dass Ihre SQL Analytics-Instanz angehalten wurde oder skaliert wird, führen Sie die Anleitungen zum Fortsetzen der Instanz aus.

![Dienst angehalten](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) Hier finden Sie weitere Informationen zur Ressourcenintegrität.

## <a name="check-for-paused-or-scaling-operation"></a>Überprüfen auf angehaltene Vorgänge oder Skalierungen

Sehen Sie im Portal nach, ob Ihre SQL Analytics-Instanz angehalten wurde oder skaliert wird.

![Dienst angehalten](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

Wenn Sie feststellen, dass der Dienst pausiert wurde oder skaliert wird, überprüfen Sie, ob dies im Rahmen eines Wartungszeitplans geschieht. In der *Übersicht* für Ihre SQL Analytics-Instanz im Portal sehen Sie den ausgewählten Wartungszeitplan.

![Übersicht über den Wartungszeitplan](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

Kontaktieren Sie andernfalls Ihren IT-Administrator, um auszuschließen, dass es sich bei der Wartung um ein geplantes Ereignis handelt. Führen Sie zur Fortsetzung der SQL Analytics-Instanz die [hier](https://docs.microsoft.com/azure/sql-data-warehouse/pause-and-resume-compute-portal#resume-compute) aufgeführten Schritte aus.

## <a name="check-your-firewall-settings"></a>Überprüfen der Firewalleinstellungen

Die SQL Analytics-Datenbank kommuniziert über Port 1433.   Wenn Sie versuchen, eine Verbindung über ein Unternehmensnetzwerk herzustellen, wird ausgehender Datenverkehr über Port 1433 von der Firewall Ihres Netzwerks unter Umständen nicht zugelassen. In diesem Fall können Sie nur dann eine Verbindung mit Ihrem Azure SQL-Datenbank-Server herstellen, wenn Ihre IT-Abteilung Port 1433 öffnet. [Hier](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#create-and-manage-ip-firewall-rules) finden Sie weitere Informationen zu den Firewallkonfigurationen.

## <a name="check-your-vnetservice-endpoint-settings"></a>Überprüfen der Einstellungen für VNETs und Dienstendpunkte

Wenn die Fehler 40914 und 40615 bei Ihnen auftreten, finden Sie in diesem Artikel die [Fehlerbeschreibung und Problembehebung](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="check-for-the-latest-drivers"></a>Überprüfen der Treiber auf Aktualität

### <a name="software"></a>Software

Stellen Sie sicher, dass Sie die neuesten Tools verwenden, um eine Verbindung mit Ihrer SQL Analytics-Datenbank herzustellen:

* SSMS
* Azure Data Studio
* SQL Server Data Tools (Visual Studio)

### <a name="drivers"></a>Treiber

Stellen Sie sicher, dass Sie die aktuelle Version der Treiber verwenden.  Wenn Sie ältere Versionen der Treiber verwenden, kann das zu unerwartetem Verhalten führen, da diese keine neuen Features unterstützen.

* [ODBC](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server)
* [JDBC](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)
* [OLE DB](https://docs.microsoft.com/sql/connect/oledb/download-oledb-driver-for-sql-server)
* [PHP](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server)

## <a name="check-your-connection-string"></a>Überprüfen der Verbindungszeichenfolge

Überprüfen Sie, ob die Verbindungszeichenfolgen ordnungsgemäß eingerichtet sind.  Im Anschluss finden Sie einige Beispiele.  Im folgenden Artikel finden Sie weitere Informationen zu [Verbindungszeichenfolgen](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connection-strings).

ADO.NET-Verbindungszeichenfolge

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

ODBC-Verbindungszeichenfolge

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP-Verbindungszeichenfolge

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

Verbindungszeichenfolge für JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>Zeitweilige Verbindungsprobleme

Überprüfen Sie, ob die Auslastung des Servers hoch ist und sich viele Anforderungen in der Warteschlange befinden. Möglicherweise müssen Sie Ihre SQL Analytics-Instanz für zusätzliche Ressourcen zentral hochskalieren.

## <a name="common-error-messages"></a>Häufige Fehlermeldungen

In diesem Artikel finden Sie die [Fehlerbeschreibung und Problembehebung](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615) zu den Fehlern 40914 und 40615.

## <a name="still-having-connectivity-issues"></a>Treten weiterhin Konnektivitätsprobleme auf?
Erstellen Sie ein [Supportticket](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket), damit das entsprechende Team Sie bei der Problembehebung unterstützen kann.
