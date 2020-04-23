---
title: Ausführen von SSIS-Paketen mit dem Agent einer verwalteten Azure SQL-Datenbankinstanz
description: Es wird beschrieben, wie Sie SSIS-Pakete mit dem Agent einer verwalteten Azure SQL-Datenbankinstanz ausführen.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: b3b7a25149a9d075c81b30307ade2beb71907637
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394060"
---
# <a name="execute-ssis-packages-by-azure-sql-managed-instance-agent"></a>Ausführen von SSIS-Paketen mit dem Agent einer verwalteten Azure SQL-Datenbankinstanz
In diesem Artikel wird beschrieben, wie Sie ein SSIS-Paket (SQL Server Integration Services) mit dem Agent einer verwalteten Azure SQL-Datenbankinstanz ausführen. Dieses Feature ermöglicht ein ähnliches Verhalten wie beim Planen von SSIS-Paketen mit dem SQL Server-Agent in Ihrer lokalen Umgebung.

Mit diesem Feature können Sie SSIS-Pakete ausführen, die in der SSISDB der verwalteten Azure SQL-Datenbankinstanz oder einem Dateisystem wie Azure Files gespeichert sind.

## <a name="prerequisites"></a>Voraussetzungen
Laden Sie die neueste SSMS-Version (18.5 oder höher) herunter, und installieren Sie sie, wenn Sie dieses Feature verwenden möchten. Laden Sie es von [dieser Website](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) herunter.

Außerdem müssen Sie eine Azure-SSIS Integration Runtime in Azure Data Factory bereitstellen, für die eine verwaltete Azure SQL-Instanz als Endpunktserver verwendet wird. Falls Sie die Bereitstellung noch nicht durchgeführt haben, können Sie dies nachholen, indem Sie die Anleitung im [Tutorial](tutorial-create-azure-ssis-runtime-portal.md) befolgen. 

## <a name="run-ssis-packages-in-ssisdb-by-azure-sql-managed-instance-agent"></a>Ausführen von SSIS-Paketen in der SSISDB mit dem Agent einer verwalteten Azure SQL-Datenbankinstanz
In diesem Schritt verwenden Sie den Agent einer verwalteten Azure SQL-Datenbankinstanz, um SSIS-Pakete aufzurufen, die in der SSISDB in einer verwalteten Azure SQL-Datenbankinstanz gespeichert sind.
1. Stellen Sie in der aktuellen Version von SSMS eine Verbindung mit der verwalteten Azure SQL-Datenbankinstanz her.
2. Erstellen Sie einen neuen Agent-Auftrag und einen neuen Auftragsschritt.

![Neuer Agent-Auftrag](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. Wählen Sie auf der Seite **Neuer Auftragsschritt** den Typ **SQL Server Integration Services-Paket** aus.

![Neuer SSIS-Auftragsschritt](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. Wählen Sie auf der Registerkarte **Paket** als Paketquellentyp die Option **SSIS-Katalog** aus.
5. Da sich die SSISDB auf derselben verwalteten Azure SQL-Datenbankinstanz befindet, müssen Sie keine Authentifizierung angeben.
6. Geben Sie ein SSIS-Paket aus Ihrer SSISDB an.

![Paketquellentyp: SSIS-Katalog](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

7. Auf der Registerkarte **Konfigurationen** können Sie Werte für **Parameter** angeben, Werte in **Verbindungs-Manager** und die Option **Eigenschaft** überschreiben und die Option **Protokolliergrad** auswählen.

![Paketquellentyp: Konfiguration des SSIS-Katalogs](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

8. Klicken Sie nach Abschluss aller oben beschriebenen Konfigurationsschritte auf **OK**, um die Konfiguration des Agent-Auftrags zu speichern.
9. Starten Sie den Agent-Auftrag, um das SSIS-Paket auszuführen.


## <a name="run-ssis-packages-in-file-system-by-azure-sql-managed-instance-agent"></a>Ausführen von SSIS-Paketen in einem Dateisystem mit dem Agent einer verwalteten Azure SQL-Datenbankinstanz
In diesem Schritt verwenden Sie den Agent einer verwalteten Azure SQL-Datenbankinstanz, um SSIS-Pakete, die in einem Dateisystem gespeichert sind, für die Ausführung aufzurufen.
1. Stellen Sie in der aktuellen Version von SSMS eine Verbindung mit der verwalteten Azure SQL-Datenbankinstanz her.
2. Erstellen Sie einen neuen Agent-Auftrag und einen neuen Auftragsschritt.

   ![Neuer Agent-Auftrag](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. Wählen Sie auf der Seite **Neuer Auftragsschritt** den Typ **SQL Server Integration Services-Paket** aus.

   ![Neuer SSIS-Auftragsschritt](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. Wählen Sie auf der Registerkarte **Paket** als Paketquellentyp die Option **Dateisystem** aus.

   ![Paketquellentyp: Dateisystem](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)

   1. Wählen Sie **Azure-Dateifreigabe** als Dateiquellentyp aus, wenn Ihr Paket in Azure Files hochgeladen wird.
      - Der Paketpfad lautet **\\<storage account name>.file.core.windows.net\<Name der Dateifreigabe>\<Name des Pakets>.dtsx**.
      - Geben Sie Kontoname und -schlüssel für die Azure-Datei unter **Anmeldeinformationen für den Paketdateizugriff** ein, um den Zugriff zu ermöglichen. Die Domäne wird auf **Azure** festgelegt.
   2. Wenn Ihr Paket auf eine Netzwerkfreigabe hochgeladen wird, sollten Sie **Netzwerkfreigabe** als Dateiquellentyp auswählen.
      - Der Paketpfad ist der **UNC-Pfad** Ihrer Paketdatei mit der Erweiterung „DTSX“.
      - Geben Sie die entsprechende **Domäne**, den **Benutzernamen** und das **Kennwort** ein, um auf die Paketdatei für die Netzwerkfreigabe zuzugreifen.
   3. Falls Ihre Paketdatei mit einem Kennwort verschlüsselt ist, müssen Sie die Option **Verschlüsselungskennwort** auswählen und das Kennwort eingeben.

 5. Geben Sie auf der Registerkarte **Konfigurationen** den **Pfad zur Konfigurationsdatei** ein, falls Sie eine Konfigurationsdatei zum Ausführen des SSIS-Pakets benötigen.
 6. Auf der Registerkarte **Ausführungsoptionen** können Sie auswählen, ob zum Ausführen des SSIS-Pakets die **Windows-Authentifizierung** oder **32-Bit-Runtime** verwendet werden soll.
 7. Auf der Registerkarte **Protokollierung** können Sie den **Protokollierungspfad** und die entsprechenden Anmeldeinformationen für den Protokollierungszugriff zur Speicherung der Protokolldateien auswählen. Standardmäßig entspricht der Protokollierungspfad dem Paketordnerpfad, und die Anmeldeinformationen für den Protokollierungszugriff entsprechen den Anmeldeinformationen für den Paketzugriff.
 8. Auf der Registerkarte **Werte festlegen** können Sie den **Eigenschaftspfad** und den **Wert** zum Überschreiben der Paketeigenschaften eingeben.
 Geben Sie also beispielsweise zum Überschreiben des Werts Ihrer Benutzervariablen den zugehörigen Pfad im folgenden Format ein: **\Package.Variables[User::<variable name>].Value**.
 9. Klicken Sie nach Abschluss aller oben beschriebenen Konfigurationsschritte auf **OK**, um die Konfiguration des Agent-Auftrags zu speichern.
 10. Starten Sie den Agent-Auftrag, um das SSIS-Paket auszuführen.


 ## <a name="cancel-ssis-package-execution"></a>Abbrechen der SSIS-Paketausführung
 Um die Paketausführung über einen Auftrag des Agents einer verwalteten Azure SQL-Datenbankinstanz abzubrechen, sollten Sie die unten angegebenen Schritte ausführen, anstatt den Agent-Auftrag direkt zu beenden.
 1. Suchen Sie in **msdb.dbo.sysjobs** nach der **jobId** Ihres SQL-Agents.
 2. Verwenden Sie die folgende Abfrage, um anhand der Auftrags-ID nach der entsprechenden **executionId** von SSIS zu suchen:
    ```sql
    select * from ssisdb.internal.execution_parameter_values_noncatalog where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
    ```
 3. Wählen Sie im SSIS-Katalog die Option **Aktive Vorgänge** aus.

    ![Paketquellentyp: Dateisystem](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

 4. Beenden Sie den entsprechenden Vorgang basierend auf der **executionId**.

## <a name="next-steps"></a>Nächste Schritte
 Sie können SSIS-Pakete auch mit Azure Data Factory planen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Azure Data Factory-Ereignisauslöser](how-to-create-event-trigger.md). 