---
title: 'Migrationsleitfaden: SAP ASE zu Azure SQL-Datenbank'
description: In dieser Anleitung lernen Sie, wie Sie Ihre SAP ASE-Datenbanken mit dem SQL Server-Migrationsassistenten für den SAP Adapter Server Enterprise auf eine Azure SQL-Datenbank migrieren.
ms.service: sql-database
ms.subservice: migration-guide
ms.custom: ''
ms.devlang: ''
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.date: 03/19/2021
ms.openlocfilehash: 6eab839912769ad15723083185c7942f31e3df11
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108136551"
---
# <a name="migration-guide-sap-ase-to-azure-sql-database"></a>Migrationsleitfaden: SAP ASE zu Azure SQL-Datenbank

[!INCLUDE[appliesto-sqldb-sqlmi](../../includes/appliesto-sqldb.md)]

In dieser Anleitung lernen Sie, wie Sie Ihre SAP Adapter Server Enterprise-Datenbanken (ASE) zu einer Azure SQL-Datenbank [migrieren](https://azure.microsoft.com/migration/migration-journey), indem Sie [SQL Server Migration](https://azure.microsoft.com/en-us/migration/sql-server/) Assistant für SAP Adapter Server Enterprise verwenden.

Weitere Migrationsleitfäden finden Sie im [Leitfaden zur Azure-Datenbankmigration](/data-migration). 

## <a name="prerequisites"></a>Voraussetzungen 

Bevor Sie mit der Migration der Access-Datenbank zu einer SQL-Datenbank beginnen, gehen Sie folgendermaßen vor:

- Überprüfen Sie, ob Ihre Quellumgebung unterstützt wird. 
- Laden Sie den [SQL Server Migration Assistant für SAP Adaptive Server Enterprise (früher SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256) herunter.
- Stellen Sie sicher, dass Sie über Konnektivität und ausreichende Berechtigungen für den Zugriff auf Quelle und Ziel verfügen.

## <a name="pre-migration"></a>Vor der Migration

Nachdem Sie diese Voraussetzungen erfüllt haben, können Sie die Topologie Ihrer Umgebung ermitteln und die Durchführbarkeit Ihrer [Azure-Cloudmigration](https://azure.microsoft.com/migration) bewerten.

### <a name="assess"></a>Bewerten

Verwenden Sie den [SQL Server Migration Assistant (SSMA) für SAP Adaptive Server Enterprise (früher SAP Sybase ASE)](https://www.microsoft.com/en-us/download/details.aspx?id=54256), um Datenbankobjekte und Daten zu überprüfen, die Datenbanken für die Migration zu bewerten und um Sybase-Datenbankobjekte und anschließend Daten zu Azure SQL-Datenbank zu migrieren. Weitere Informationen finden Sie unter [SQL Server Migration Assistant für Sybase (SybaseToSQL)](/sql/ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql).

Führen Sie die folgenden Schritte aus, um eine Bewertung zu erstellen: 

1. Öffnen Sie SSMA für Sybase. 
1. Wählen Sie **Datei** und dann **Neues Projekt** aus. 
1. Geben Sie im Bereich **Neues Projekt** einen Namen und einen Speicherort für das Projekt ein, und wählen Sie dann in der Dropdown Liste **Migrieren zu** die Option **Azure SQL-Datenbank** aus. 
1. Wählen Sie **OK** aus.
1. Geben Sie in der Anzeige **mit Sybase verbinden** die Details der SAP-Verbindung ein. 
1. Klicken Sie mit der rechten Maustaste auf die SAP-Datenbank, die Sie migrieren möchten, und wählen Sie **Bericht erstellen** aus. Dadurch wird ein HTML-Bericht generiert. Alternativ können Sie rechts oben die Registerkarte **Bericht erstellen** auswählen.
1. Sehen Sie sich den HTML-Bericht an, um die Konvertierungsstatistiken und etwaige Fehler oder Warnungen zu verstehen. Sie können den Bericht auch in Excel öffnen, um einen Bestand der Oracle-Objekte sowie Informationen zum Aufwand für Schemakonvertierungen zu erhalten. Der Standardspeicherort für den Bericht ist der Berichtsordner in SSMAProjects. Beispiel:

   `drive:\<username>\Documents\SSMAProjects\MySAPMigration\report\report_<date>` 

### <a name="validate-the-type-mappings"></a>Überprüfen der Typzuordnungen

Bevor Sie die Schema-Konvertierung durchführen, validieren Sie die Standard-Datentyp-Zuordnungen oder ändern Sie gemäß den Richtlinien. Sie können dies tun, indem Sie **Tools** > **Project Settings** wählen, oder Sie können das Type Mapping für jede Tabelle ändern, indem Sie die Tabelle im **SAP ASE Metadata Explorer** auswählen.

### <a name="convert-the-schema"></a>Konvertieren des Schemas

Gehen Sie folgendermaßen vor, um das Schema zu konvertieren:

1. (Fakultativ) Zum Konvertieren von dynamischen oder spezialisierten Abfragen klicken Sie mit der rechten Maustaste auf den Knoten, und wählen Sie dann **Anweisung hinzufügen** aus. 
1. Wählen Sie die Registerkarte **mit Azure SQL-Datenbank verbinden** aus, und geben Sie dann die Details für Ihre SQL-Datenbank ein. Sie können eine Verbindung mit einer vorhandenen Datenbank herstellen oder einen neuen Namen angeben. In diesem Fall wird auf dem Zielserver eine Datenbank erstellt.
1. Klicken Sie im **Sybase Metadata Explorer** mit der rechten Maustaste auf das Schema, mit dem Sie arbeiten, und wählen Sie dann **Schema konvertieren** aus. 
1. Nachdem das Schema konvertiert wurde, vergleichen und überprüfen Sie die konvertierte Struktur mit der ursprünglichen Struktur, um potenzielle Probleme zu identifizieren. 

   Nach der Schemakonvertierung können Sie dieses Projekt lokal speichern, um das Schema offline zu warten. Wählen Sie hierzu **Datei**  >  **Projekt speichern** aus. So können Sie das Quell- und Zielschema offline auswerten und warten, bevor Sie das Schema auf Ihrer SQL-Datenbank veröffentlichen.

1. Wählen Sie in der **Ausgabe** Anzeige **Ergebnisse überprüfen** aus, und überprüfen Sie die Fehler der **Fehlerliste** Anzeige. 
1. Speichern Sie das Projekt für eine Übung zur Offlineschemakorrektur lokal. Wählen Sie hierzu **Datei**  >  **Projekt speichern** aus. So können Sie das Quell- und Zielschema offline auswerten und warten, bevor Sie das Schema auf Ihrer SQL-Datenbank veröffentlichen.

## <a name="migrate-the-databases"></a>Migrieren Sie die Datenbanken 

Nachdem Sie die notwendigen Voraussetzungen geschaffen und die Aufgaben im Zusammenhang mit der *Vormigrationsphase* abgeschlossen haben, können Sie die Schema- und Datenmigration durchführen.Sie die Schema- und Datenmigration starten.

Führen Sie die folgenden Schritte aus, um das Schema zu veröffentlichen und die Daten zu migrieren: 

1. Veröffentlichen Sie das Schema. Klicken Sie in der Anzeige des **Azure SQL Database Metadata Explorers** mit der rechten Maustaste auf die Datenbank, mit der Sie arbeiten, und wählen Sie dann **mit Datenbank synchronisieren** aus. Mit dieser Aktion wird das SAP ASE-Schema in Ihrer SQL-Datenbank veröffentlicht.

1. Migrieren der Daten. Klicken Sie in der Anzeige des **SAP ASE Metadata Explorers** mit der rechten Maustaste auf das zu migrierende Schema oder Objekt, und wählen Sie dann **Daten migrieren** aus. Alternativ können Sie rechts oben die Registerkarte **Daten migrieren** auswählen. 

   Aktivieren Sie das Kontrollkästchen neben dem Datenbanknamen, um die Daten einer gesamten Datenbank zu migrieren. Wenn Sie Daten aus einzelnen Tabellen migrieren möchten, erweitern Sie die Datenbank, erweitern Sie die **Tabellen**, und aktivieren Sie dann das Kontrollkästchen neben der Tabelle. Deaktivieren Sie das Kontrollkästchen, um Daten aus einzelnen Tabellen auszulassen. 
1. Überprüfen Sie nach Abschluss der Migration den **Bericht zur Datenmigration**. 
1. Validieren Sie die Migration, indem Sie die Daten und das Schema überprüfen. Stellen Sie eine Verbindung mit Ihrer Datenbank in Azure SQL-Datenbank her, indem Sie das [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) verwenden.

## <a name="post-migration"></a>Nach der Migration 

Nach erfolgreichem Abschluss der *Migrationsphase* müssen Sie eine Reihe von Aufgaben ausführen, um sicherzustellen, dass alles so reibungslos und effizient wie möglich funktioniert.

### <a name="remediate-applications"></a>Korrigieren von Anwendungen

Wenn die Daten in die Zielumgebung migriert wurden, müssen alle Anwendungen, die zuvor die Quelle verwendet haben, beginnen das Ziel zu verwenden. Hierfür sind in einigen Fällen Änderungen an den Anwendungen erforderlich.

### <a name="perform-tests"></a>Durchführen von Tests

Das Testvorgehen für die Datenbankmigration umfasst die folgenden Aktivitäten:

1. **Entwickeln von Validierungstests:** Für das Testen der Datenbankmigration müssen Sie SQL-Abfragen verwenden. Sie müssen die Validierungsabfragen erstellen, die sowohl gegen die Quell- als auch gegen die Zieldatenbank ausgeführt werden. Ihre Validierungsabfragen sollten den von Ihnen definierten Bereich abdecken.

1. **Einrichten der Testumgebung:** Die Testumgebung sollte eine Kopie der Quelldatenbank und der Zieldatenbank enthalten. Stellen Sie sicher, dass Sie die Testumgebung isolieren.

1. **Führen Sie die Validierungstests aus**: Führen Sie die Validierungstests für die Quelle und das Ziel aus, und analysieren Sie anschließend die Ergebnisse.

1. **Führen Sie Leistungstests aus**: Führen Sie Leistungstests für die Quelle und das Ziel aus, und analysieren und vergleichen Sie anschließend die Ergebnisse.


### <a name="optimize"></a>Optimieren

Die Phase nach der Migration ist entscheidend für den Abgleich von Problemen mit der Datengenauigkeit, die Überprüfung der Vollständigkeit und das Beheben von Leistungsproblemen der Workload.

Weitere Informationen zu diesen Problemen und den Schritten zu ihrer Behebung finden Sie in der [Anleitung zur Validierung und Optimierung nach der Migration](/sql/relational-databases/post-migration-validation-and-optimization-guide).


## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die Dienste und Tools von Microsoft und Drittanbietern, mit denen Sie verschiedene Datenbank- und Datenmigrationsszenarios sowie Spezialaufgaben ausführen können, finden Sie unter [Dienste und Tools für die Datenmigration](../../../dms/dms-tools-matrix.md).

- Weitere Informationen zu Azure SQL-Datenbank:
   - [Übersicht über SQL-Datenbank](../../database/sql-database-paas-overview.md)
   - [Azure-Gesamtkostenrechner](https://azure.microsoft.com/pricing/tco/calculator/)  

- Weitere Informationen zum Framework und zum Einführungszyklus von Cloudmigrationen finden Sie unter:
   -  [Cloud Adoption Framework für Azure](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-scale)
   -  [Bewährte Methoden für die Kostenermittlung und Größenanpassung von zu Azure migrierten Workloads](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs) 
   -  [Ressourcen zur Cloudmigration](https://azure.microsoft.com/migration/resources)

- Informationen zum Bewerten der Anwendungszugriffsebene finden Sie unter [Data Access Migration Toolkit (Vorschauversion)](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit).
- Ausführliche Informationen zum Ausführen von A/B-Tests auf Datenzugriffsebene finden Sie unter [Assistent für Datenbankexperimente](/sql/dea/database-experimentation-assistant-overview).