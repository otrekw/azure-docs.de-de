---
title: 'Db2 zu SQL Server auf Azure VM: Anleitung zur Migration'
description: In diesem Handbuch erfahren Sie, wie Sie Ihre IBM Db2-Datenbanken mithilfe des SQL Server-Migrationsassistenten für Db2 auf SQL Server auf Azure VM migrieren.
ms.custom: ''
ms.service: virtual-machines-sql
ms.subservice: migration-guide
ms.devlang: ''
ms.topic: how-to
author: markjones-msft
ms.author: markjon
ms.reviewer: mathoma
ms.date: 11/06/2020
ms.openlocfilehash: ff241f468db2e56b73ba10b5621e8a8ab40a19b6
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "106554143"
---
# <a name="migration-guide-ibm-db2-to-sql-server-on-azure-vm"></a>Anleitung zur Migration: IBM Db2 zu SQL Server auf Azure VM
[!INCLUDE[appliesto--sqlmi](../../includes/appliesto-sqlvm.md)]

In diesem Handbuch erfahren Sie, wie Sie Ihre IBM Db2-Datenbanken mithilfe des SQL Server-Migrationsassistenten für Db2 auf SQL Server auf Azure VM migrieren. 

Weitere Migrationsanleitungen finden Sie unter[Azure Database Migration Guides](https://docs.microsoft.com/data-migration). 

## <a name="prerequisites"></a>Voraussetzungen

Um Ihre Db2-Datenbank zu SQL Server zu migrieren, benötigen Sie:

- Zur Überprüfung, ob Ihre [Quellumgebung](/sql/ssma/db2/installing-ssma-for-Db2-client-Db2tosql#prerequisites) unterstützt wird.
- [SQL Server Migration Assistant (SSMA) für Db2](https://www.microsoft.com/download/details.aspx?id=54254).
- [Konnektivität](../../virtual-machines/windows/ways-to-connect-to-sql.md) zwischen Ihrer Quellumgebung und Ihrer SQL Server-VM in Azure 
- Ein Ziel-[SQL-Server auf Azure VM](../../virtual-machines/windows/create-sql-vm-portal.md). 

## <a name="pre-migration"></a>Vor der Migration

Nachdem Sie die Voraussetzungen erfüllt haben, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit Ihrer Migration beurteilen. 

### <a name="assess"></a>Bewerten 

Verwenden Sie SSMA für DB2, um Datenbankobjekte und Daten zu überprüfen und Datenbanken für die Migration zu bewerten. 

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen:

1. Öffnen Sie [SSMA für Db2](https://www.microsoft.com/download/details.aspx?id=54254). 
1. Wählen Sie **Datei** > **Neues Projekt**. 
1. Geben Sie einen Projektnamen und einen Speicherort für Ihr Projekt an. Wählen Sie dann ein SQL Server-Migrationsziel aus der Dropdown-Liste und wählen Sie **OK**.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/new-project.png" alt-text="Screenshot, das die zu spezifizierenden Projektdetails anzeigt.":::


1. Geben Sie unter **Verbinden mit Db2**, Werte für die Db2-Verbindungsdetails ein.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/connect-to-Db2.png" alt-text="Screenshot, der die Optionen zur Verbindung mit Ihrer Db2-Instanz anzeigt.":::


1. Klicken Sie mit der rechten Maustaste auf das Db2-Schema, das Sie migrieren möchten, und wählen Sie dann **Bericht erstellent**. Dadurch wird ein HTML-Bericht generiert. Alternativ können Sie nach dem Auswählen des Schemas in der Navigationsleiste auf **Create report** (Bericht erstellen) klicken.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/create-report.png" alt-text="Screenshot, der zeigt, wie ein Bericht erstellt wird.":::

1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um eine Bestandsaufnahme der Db2-Objekte und des Aufwands für die Schemakonvertierung zu erhalten. Der Standardstandort für den Bericht ist im Berichtsordner innerhalb von *SSMAProjects*.

   Beispiel: `drive:\<username>\Documents\SSMAProjects\MyDb2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/report.png" alt-text="Screenshot des Berichts, den Sie überprüfen, um eventuelle Fehler oder Warnungen zu erkennen.":::


### <a name="validate-data-types"></a>Überprüfen von Datentypen

Validieren Sie die Standard-Datentyp-Zuordnungen und ändern Sie bei Bedarf entsprechend den Anforderungen. Gehen Sie dazu folgendermaßen vor: 

1. Klicken Sie im Menü auf **Tools**. 
1. Klicken auf **Project Settings** (Projekteinstellungen). 
1. Klicken Sie auf die Registerkarte **Type Mappings** (Typzuordnungen).

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/type-mapping.png" alt-text="Screenshot, der die Auswahl des Schemas und der Typ-Mapping anzeigt.":::

1. Sie können die Typzuordnung für jede Tabelle ändern, indem Sie die Tabelle im **Db2 Metadata Explorer** auswählen. 

### <a name="convert-schema"></a>Schema konvertieren 

Führen Sie die folgenden Schritte aus, um das Schema zu konvertieren:

1. (Optional:) Fügen Sie Anweisungen dynamische Abfragen oder Ad-hoc-Abfragen hinzu. Klicken Sie mit der rechten Maustaste auf den Knoten, und klicken Sie dann auf **Add statements** (Anweisungen hinzufügen). 
1. Klicken Sie auf **Connect to SQL Server** (Mit SQL Server verbinden). 
    1. Geben Sie die Verbindungsdetails ein, um sich mit Ihrer Instanz von SQL Server auf Ihrer Azure-VM zu verbinden. 
    1. Stellen Sie entweder eine Verbindung mit einer bereits vorhandenen Datenbank auf dem Zielserver her, oder geben Sie einen neuen Namen an, um eine neue Datenbank auf dem Zielserver zu erstellen. 
    1. Geben Sie Details zur Authentifizierung an. 
    1. Wählen Sie **Verbinden** aus.

    :::image type="content" source="../../../../includes/media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png" alt-text="Screenshot, der die Details zeigt, die für die Verbindung mit Ihrem SQL Server auf Azure VM erforderlich sind.":::

1. Klicken Sie mit der rechten Maustaste auf das Schema, und klicken Sie dann auf **Convert Schema** (Schema konvertieren). Alternativ können Sie nach dem Auswählen des Schemas in der oberen Navigationsleiste auf **Convert Schema** (Schema konvertieren) klicken.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/convert-schema.png" alt-text="Screenshot, der die Auswahl des Schemas und dessen Konvertierung zeigt.":::

1. Vergleichen und überprüfen Sie nach Abschluss der Konvertierung die Struktur des Schemas, um mögliche Probleme zu identifizieren. Gehen Sie die Probleme auf der Grundlage der Empfehlungen an. 

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/compare-review-schema-structure.png" alt-text="Screenshot, der den Vergleich und die Überprüfung der Struktur des Schemas zeigt, um mögliche Probleme zu identifizieren.":::

1. Wählen Sie im **Fensterbereich** Ausgabe die Option **Ergebnisse überprüfen**. Überprüfen Sie die Fehler im Fenster **Fehlerliste**. 
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie im Menü **Date** den Eintrag **Projekt speichernt** . Dies gibt Ihnen die Möglichkeit, die Quell- und Zielschemata offline zu bewerten und Korrekturen durchzuführen, bevor Sie das Schema auf SQL Server auf Azure VM veröffentlichen können.

## <a name="migrate"></a>Migrate

Wenn Sie die Bewertung Ihrer Datenbanken und die Behandlung etwaiger Abweichungen abgeschlossen haben, besteht der nächste Schritt in der Durchführung des Migrationsprozesses.

Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und Ihre Daten zu migrieren:

1. Veröffentlichen Sie das Schema. Klicken Sie im **SQL Server Metadata Explorer** im Knoten **Databases** mit der rechten Maustaste auf die Datenbank. Wählen Sie dann **Mit Datenbank synchronisieren**.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/synchronize-with-database.png" alt-text="Screenshot, der die Option zur Synchronisierung mit der Datenbank zeigt.":::

1. Migrieren Sie die Daten. Klicken Sie im **Db2 Metadata Explorer** mit der rechten Maustaste auf die Datenbank oder das Objekt, das Sie migrieren möchten, und wählen Sie **Daten migrieren**. Alternativ können Sie in der Navigationsleiste **Daten migrieren** auswählen. Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um die Daten einer gesamten Datenbank zu migrieren. Um Daten aus einzelnen Tabellen zu migrieren, erweitern Sie die Datenbank, erweitern Sie **Tabellen** und aktivieren Sie dann das Kontrollkästchen neben der Tabelle. Deaktivieren Sie das Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/migrate-data.png" alt-text="Screenshot, der die Auswahl des Schemas und die Auswahl der Datenmigration zeigt.":::

1. Geben Sie die Verbindungsdetails sowohl für die Db2- als auch für die SQL-Server-Instanzen an. 
1. Nach Abschluss der Migration können Sie den **Datenmigrationsbericht** anzeigen:  

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/data-migration-report.png" alt-text="Screenshot, der zeigt, wo der Datenmigrationsbericht eingesehen werden kann.":::

1.  Verbinden Sie sich mit Ihrer Instanz von SQL Server auf Azure VM, indem Sie [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) verwenden. Validieren Sie die Migration, indem Sie die Daten und das Schema überprüfen.

   :::image type="content" source="media/db2-to-sql-on-azure-vm-guide/compare-schema-in-ssms.png" alt-text="Screenshot, der den Vergleich des Schemas in SQL Server Management Studio zeigt.":::

## <a name="post-migration"></a>Nach der Migration 

Nachdem die Migration abgeschlossen ist, müssen Sie eine Reihe von Aufgaben nach der Migration durchführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen 

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testen umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests**: Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die für die Quell- und die Zieldatenbank ausgeführt werden sollen. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.
1. **Einrichten der Testumgebung**: Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.
1. **Ausführen der Validierungstests**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.
1. **Leistungstests ausführen**: Führen Sie Leistungstests mit der Quelle und dem Ziel durch und analysieren und vergleichen Sie dann die Ergebnisse.

## <a name="migration-assets"></a>Migrationsressourcen 

Weitere Unterstützung erhalten Sie in Form der folgenden Ressourcen, die im Rahmen eines Auftrags für ein echtes Migrationsprojekt entwickelt wurden:

|Asset  |BESCHREIBUNG  |
|---------|---------|
|[Data Workload Assessment Model and Tool (Datenarbeitsauslastungs-Bewertungsmodell und -tool)](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Dieses Tool stellt für eine angegebene Arbeitsauslastung Informationen zu empfohlenen optimalen Zielplattformen, zur Cloudbereitschaft und zum Korrekturbedarf für Anwendungen/Datenbanken bereit. Es bietet eine einfache Berechnung und Berichterstellung mit nur einem Klick, die Ihnen durch Bereitstellung eines automatisierten und einheitlichen Zielplattform-Entscheidungsprozesses dabei helfen, umfangreiche Bewertungen zu beschleunigen.|
|[Db2 zOS-Paket zur Erkennung und Bewertung von Datenbeständen](https://github.com/microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|Nach dem Ausführen des SQL-Skripts für eine-Datenbank können Sie die Ergebnisse in eine Datei im Dateisystem exportieren. Es werden verschiedene Dateiformate unterstützt, z. B. *.csv, damit Sie die Ergebnisse in externen Tools wie Tabellen erfassen können. Diese Methode kann nützlich sein, wenn Sie Ergebnisse einfach für Teams freigeben möchten, die die Workbench nicht installiert haben.|
|[IBM Db2 LUW-Bestandsskripte und -Artefakte](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20Db2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|Dieses Asset enthält eine SQL-Abfrage, die auf IBM Db2 LUW Version 11.1-Systemtabellen zugreift und eine Zählung der Objekte nach Schema und Objekttyp, eine grobe Schätzung der "Rohdaten" in jedem Schema und die Größenordnung der Tabellen in jedem Schema liefert, wobei die Ergebnisse in einem CSV-Format gespeichert werden.|
|[Db2 LUW pure scale auf Azure - Einrichtungsanleitunge](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/db2%20PureScale%20on%20Azure.pdf)|Dieser Leitfaden dient als Startpunkt für einen Db2-Implementierungsplan. Obwohl sich die geschäftlichen Anforderungen unterscheiden werden, gilt das gleiche Grundmuster. Dieses Architekturmuster kann auch für OLAP-Anwendungen auf Azure verwendet werden.|

Das Data SQL Engineering-Team hat diese Ressourcen entwickelt. Die Kernaufgabe dieses Teams ist die Freigabe und Beschleunigung komplexer Modernisierungsprojekte für die Migration von Datenplattformen auf die Azure-Datenplattform von Microsoft.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich nach der Migration den [Leitfaden für die Überprüfung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide) an. 

Informationen zu Diensten und Tools von Microsoft und Drittanbietern, die Sie bei verschiedenen Datenbank- und Datenmigrationsszenarien unterstützen können, finden Sie unter [Datenmigrationsdienste und -Tools](../../../dms/dms-tools-matrix.md).

Videoinhalte finden Sie unter [Übersicht über die Migrationsreise](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).
