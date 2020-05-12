---
title: Planen der Ausführung von SSIS-Paketen mit einem Agent für die verwaltete Azure SQL-Datenbank-Instanz
description: Erfahren Sie, wie Sie Ausführungen von SSIS-Paketen mit einem Agent für die verwaltete Azure SQL-Datenbank-Instanz planen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: f230e4d33686b006b20e856d5e8033847e3f3d67
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2020
ms.locfileid: "82628485"
---
# <a name="schedule-ssis-package-executions-by-using-azure-sql-database-managed-instance-agent"></a>Planen der Ausführung von SSIS-Paketen mit einem Agent für die verwaltete Azure SQL-Datenbank-Instanz

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

In diesem Artikel wird beschrieben, wie Sie ein SSIS-Paket (SQL Server Integration Services) mit dem Agent einer verwalteten Azure SQL-Datenbank-Instanz ausführen. Dieses Feature bietet ein ähnliches Verhalten wie bei der Planung von SSIS-Paketen mit dem SQL Server-Agent in Ihrer lokalen Umgebung.

Mit diesem Feature können Sie SSIS-Pakete ausführen, die in der SSISDB in einer verwalteten Azure SQL-Datenbank-Instanz oder einem Dateisystem wie Azure Files gespeichert sind.

## <a name="prerequisites"></a>Voraussetzungen
Laden Sie die neueste Version von SQL Server Management Studio (SSMS) [herunter](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017), bei der es sich um Version 18.5 handelt, und installieren Sie sie, wenn Sie dieses Feature verwenden möchten.

Sie müssen auch eine [Azure-SSIS Integration Runtime](tutorial-create-azure-ssis-runtime-portal.md) in Azure Data Factory bereitstellen. Dabei wird eine verwaltete Azure SQL-Datenbank-Instanz als Endpunktserver verwendet. 

## <a name="run-an-ssis-package-in-ssisdb"></a>Ausführen eines SSIS-Pakets in SSISDB
In diesem Verfahren verwenden Sie den Agent für die verwaltete Azure SQL-Datenbank-Instanz zum Aufrufen eines SSIS-Pakets, das in SSISDB gespeichert ist.

1. Stellen Sie in der aktuellen Version von SSMS eine Verbindung mit der verwalteten Azure SQL-Datenbank-Instanz her.
1. Erstellen Sie einen neuen Agent-Auftrag und einen neuen Auftragsschritt. Klicken Sie unter **SQL Server-Agent** mit der rechten Maustaste auf den Ordner **Aufträge**, und wählen Sie dann **Neuer Auftrag** aus.

   ![Auswahl zum Erstellen eines neuen Agent-Auftrags](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. Wählen Sie auf der Seite **Neuer Auftragsschritt** den Typ **SQL Server Integration Services-Paket** aus.

   ![Auswahl zum Erstellen eines neuen SSIS-Auftragsschritts](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. Wählen Sie auf der Registerkarte **Paket** als Paketquellentyp die Option **SSIS-Katalog** aus.
1. Da sich die SSISDB in einer verwalteten Azure SQL-Datenbank-Instanz befindet, müssen Sie keine Authentifizierung angeben.
1. Geben Sie ein SSIS-Paket aus der SSISDB an.

   ![Registerkarte „Paket“ mit Auswahl für den Paketquellentyp](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

1. Auf der Registerkarte **Konfiguration** haben Sie folgende Möglichkeiten:
  
   - Geben Sie Parameterwerte unter **Parameter** an.
   - Setzen Sie die Werte unter **Verbindungs-Manager** außer Kraft.
   - Setzen Sie die Eigenschaft außer Kraft, und wählen Sie die Protokollierungsebene unter **Erweitert** aus.

   ![Registerkarte „Konfiguration“ mit Auswahl für den Paketquellentyp](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

1. Wählen Sie **OK** aus, um die Agent-Auftragskonfiguration zu speichern.
1. Starten Sie den Agent-Auftrag, um das SSIS-Paket auszuführen.


## <a name="run-an-ssis-package-in-the-file-system"></a>Ausführen eines SSIS-Pakets im Dateisystem
In diesem Verfahren verwenden Sie den Agent für die verwaltete Azure SQL-Datenbank-Instanz zum Ausführen eines SSIS-Pakets, das im Dateisystem gespeichert ist.

1. Stellen Sie in der aktuellen Version von SSMS eine Verbindung mit der verwalteten Azure SQL-Datenbank-Instanz her.
1. Erstellen Sie einen neuen Agent-Auftrag und einen neuen Auftragsschritt. Klicken Sie unter **SQL Server-Agent** mit der rechten Maustaste auf den Ordner **Aufträge**, und wählen Sie dann **Neuer Auftrag** aus.

   ![Auswahl zum Erstellen eines neuen Agent-Auftrags](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. Wählen Sie auf der Seite **Neuer Auftragsschritt** den Typ **SQL Server Integration Services-Paket** aus.

   ![Auswahl zum Erstellen eines neuen SSIS-Auftragsschritts](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. Auf der Registerkarte **Paket**:

   1. Wählen Sie für **Paketquelle** die Option **Dateisystem** aus.
   
   1. Für **Dateiquellentyp**:   

      - Wenn Ihr Paket zu Azure Files hochgeladen wird, wählen Sie **Azure-Dateifreigabe** aus.

        ![Optionen für den Dateiquellentyp](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)
      
        Der Paketpfad ist **`\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`** .
      
        Geben Sie unter **Anmeldeinformationen für den Paketdateizugriff** den Kontonamen und Kontoschlüssel der Azure-Datei ein, um auf die Azure-Datei zuzugreifen. Die Domäne wird auf **Azure** festgelegt.

      - Wenn Ihr Paket auf eine Netzwerkfreigabe hochgeladen wird, wählen Sie **Netzwerkfreigabe** aus.
      
        Der Paketpfad ist der UNC-Pfad Ihrer Paketdatei mit der Erweiterung „DTSX“.
      
        Geben Sie die entsprechende Domäne, den Benutzernamen und das Kennwort ein, um auf die Paketdatei für die Netzwerkfreigabe zuzugreifen.
   1. Falls Ihre Paketdatei mit einem Kennwort verschlüsselt ist, müssen Sie die Option **Verschlüsselungskennwort** auswählen und das Kennwort eingeben.
1. Geben Sie auf der Registerkarte **Konfigurationen** den Pfad zur Konfigurationsdatei ein, falls Sie eine Konfigurationsdatei zum Ausführen des SSIS-Pakets benötigen.
   Wenn Sie Ihre Konfiguration in Azure Files speichern, lautet der Konfigurationspfad **`\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`** .
1. Auf der Registerkarte **Ausführungsoptionen** können Sie auswählen, ob zum Ausführen des SSIS-Pakets die **Windows-Authentifizierung** oder **32-Bit-Runtime** verwendet werden soll.
1. Auf der Registerkarte **Protokollierung** können Sie den Protokollierungspfad und die entsprechenden Anmeldeinformationen für den Protokollierungszugriff zur Speicherung der Protokolldateien auswählen. 
   Standardmäßig entspricht der Protokollierungspfad dem Paketordnerpfad, und die Anmeldeinformationen für den Protokollierungszugriff entsprechen den Anmeldeinformationen für den Paketzugriff.
   Wenn Sie Ihre Protokolle in Azure Files speichern, lautet Ihr Protokollierungspfad **`\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`** .
1. Auf der Registerkarte **Werte festlegen** können Sie den Eigenschaftspfad und den Wert zum Überschreiben der Paketeigenschaften eingeben.
 
   Geben Sie also beispielsweise zum Überschreiben des Werts Ihrer Benutzervariablen den zugehörigen Pfad im folgenden Format ein: **`\Package.Variables[User::<variable name>].Value`** .
1. Wählen Sie **OK** aus, um die Agent-Auftragskonfiguration zu speichern.
1. Starten Sie den Agent-Auftrag, um das SSIS-Paket auszuführen.


## <a name="cancel-ssis-package-execution"></a>Abbrechen der SSIS-Paketausführung
Um die Paketausführung über einen Auftrag des Agents einer verwalteten Azure SQL-Datenbank-Instanz abzubrechen, sollten Sie die unten angegebenen Schritte ausführen, anstatt den Agent-Auftrag direkt zu beenden:

1. Suchen Sie in **msdb.dbo.sysjobs** nach der **jobId** Ihres SQL-Agents.
1. Verwenden Sie die folgende Abfrage, um anhand der Auftrags-ID nach der entsprechenden **executionId** von SSIS zu suchen:
   ```sql
   select * from ssisdb.internal.execution_parameter_values_noncatalog where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
   ```
1. Klicken Sie mit der rechten Maustaste auf den SSISDB-Katalog, und wählen Sie dann **Aktive Vorgänge** aus.

   ![„Aktive Vorgänge“ im Kontextmenü für den SSISDB-Katalog](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

1. Beenden Sie den entsprechenden Vorgang basierend auf der **executionId**.

## <a name="next-steps"></a>Nächste Schritte
Sie können SSIS-Pakete auch mit Azure Data Factory planen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Azure Data Factory-Ereignisauslöser](how-to-create-event-trigger.md). 
