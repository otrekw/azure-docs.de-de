---
title: Verwenden von SQL-Datenbank-DAC-Paketen und Stream Analytics-Aufträgen mit Azure SQL Database Edge | Microsoft-Dokumentation
description: Informationen zur Verwendung von Stream Analytics-Aufträgen in SQL Database Edge
keywords: sql database edge, stream analytics, sqlpackage
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 11/04/2019
ms.openlocfilehash: 21a8bb6953fd879b17816361f536596571678697
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74384157"
---
# <a name="using-sql-database-dac-packages-and-stream-analytics-jobs-with-sql-database-edge"></a>Verwenden von SQL-Datenbank-DAC-Paketen und Stream Analytics-Aufträgen mit SQL Database Edge

Azure SQL Database Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und Edgebereitstellungen. Sie basiert auf den aktuellen Versionen der Microsoft SQL Server-Datenbank-Engine, die branchenführende Funktionen für Leistung, Sicherheit und Abfrageverarbeitung bietet. Zusammen mit den branchenführenden Funktionen für die Verwaltung relationaler Datenbanken von SQL Server bietet Azure SQL Database Edge integrierte Streamingfunktionen für Echtzeitanalysen und komplexe Ereignisverarbeitung.

Azure SQL Database Edge bietet auch eine native Implementierung von „SqlPackage.exe“, mit der Sie während der Bereitstellung von SQL Database Edge ein [SQL-Datenbank-DAC](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications)-Paket bereitstellen können.

Über die IoT Edge-Moduloption `module twin's desired properties` macht Azure SQL Database Edge zwei optionale Parameter verfügbar:

```json
{
    "properties.desired":
    {
        "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
        "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
    }
}
```

|Feld | BESCHREIBUNG |
|------|-------------|
| SqlPackage | Azure Blob Storage-URI für die *.ZIP-Datei mit dem SQL-Datenbank-DAC-Paket.
| ASAJobInfo | Azure Blob Storage-URI für den ASA Edge-Auftrag. Weitere Informationen finden Sie unter [Veröffentlichen eines ASA Edge-Auftrags für SQL Database Edge](/azure/sql-database-edge/stream-analytics#using-streaming-jobs-with-sql-database-edge).

## <a name="using-sql-database-dac-packages-with-sql-database-edge"></a>Verwenden von SQL-Datenbank-DAC-Paketen mit SQL Database Edge

Wenn Sie ein SQL-Datenbank-DAC-Paket (*.DACPAC) mit SQL Database Edge verwenden möchten, führen Sie die folgenden Schritte aus:

1. Erstellen oder extrahieren Sie ein SQL-Datenbank-DAC-Paket. Informationen zum Generieren eines DAC-Pakets für eine vorhandene SQL Server-Datenbank finden Sie unter [Extrahieren einer DAC aus einer Datenbank](/sql/relational-databases/data-tier-applications/extract-a-dac-from-a-database/).

2. Komprimieren Sie die *.DACPAC-Datei, und laden Sie sie in ein Azure Blob Storage-Konto hoch. Weitere Informationen zum Hochladen von Dateien in Azure Blob Storage finden Sie unter [Hochladen, Herunterladen und Auflisten von Blobs über das Azure-Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

3. Generieren Sie eine Shared Access Signature (SAS) für die ZIP-Datei über das Azure-Portal. Weitere Informationen finden Sie unter [Delegieren des Zugriffs mit Shared Access Signatures (SAS)](../storage/common/storage-sas-overview.md).

4. Aktualisieren Sie die Konfiguration des SQL Database Edge-Moduls, um den SAS-URI für das DAC-Paket einzubeziehen. Führen Sie die folgenden Schritte zum Aktualisieren des SQL Database Edge-Moduls aus:

    1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Bereitstellung.

    2. Wählen Sie im linken Bereich die Option **IoT Edge** aus.

    3. Suchen Sie auf der Seite **IoT Edge** nach der IoT Edge-Instanz, in der das SQL Database Edge-Modul bereitgestellt wurde, und wählen Sie es aus.

    4. Wählen Sie auf der Geräteseite **IoT Edge-Gerät** die Option **Modul festlegen** aus.

    5. Wählen Sie auf der Seite **Module festlegen** für das SQL Database Edge-Modul **Konfigurieren** aus.

    6. Wählen Sie im Bereich **Benutzerdefinierte IoT Edge-Module** die Option **Gewünschte Eigenschaften für Modulzwilling festlegen** aus. Aktualisieren Sie die gewünschten Eigenschaften, um den URI für die Option `SQLPackage` einzubeziehen, wie im folgenden Beispiel gezeigt wird.

        > [!NOTE]
        > Der SAS-URI im nachstehenden JSON-Code ist nur ein Beispiel. Ersetzen Sie diesen URI durch den tatsächlichen URI aus Ihrer Bereitstellung.

        ```json
            {
                "properties.desired":
                {
                    "SqlPackage": "https://StorageAccountName.blob.core.windows.net/SQLpackageFiles/MeasurementsDB.zip?sp=r&st=2019-09-01T00:00:00Z&se=2019-09-30T00:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=Uh8X0E50qzlxkiKVBJKU3ccVQYwF235qTz7AXp4R3gI%3D",
                }
            }
        ```

    7. Wählen Sie **Speichern** aus.

    8. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** aus.

    9. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** und dann **Senden** aus.

5. Nach dem Modulupdate wird die DAC-Paketdatei heruntergeladen, entzippt und für die SQL Database Edge-Instanz bereitgestellt.

## <a name="using-streaming-jobs-with-sql-database-edge"></a>Verwenden von Streamingaufträgen mit SQL Database Edge

Azure SQL Database Edge enthält eine native Implementierung der Stream Analytics-Runtime. Diese Implementierung ermöglicht es Ihnen, einen Azure Stream Analytics-Edgeauftrag zu erstellen und als SQL Database Edge-Streamingauftrag bereitzustellen. Führen Sie zum Erstellen eines Stream Analytics-Edgeauftrags die folgenden Schritte aus:

1. Wechseln Sie mithilfe der Vorschau-[URL](https://portal.azure.com/?microsoft_azure_streamanalytics_edgeadapterspreview=true) zum Azure-Portal. Diese Vorschau-URL ermöglicht es Ihnen, eine SQL-Datenbank-Ausgabe für einen Stream Analytics-Edgeauftrag zu konfigurieren.

2. Erstellen Sie einen neuen **Azure Stream Analytics für IoT Edge**-Auftrag. Wählen Sie die Hostumgebung für **Edge** aus.

3. Definieren Sie eine Ein- und Ausgabe für den Azure Stream Analytics-Auftrag. Jede SQL-Ausgabe, die Sie hier einrichten, ist an eine einzelne Tabelle in der Datenbank gebunden. Wenn ein Streaming von Daten an mehrere Tabellen erforderlich ist, müssen Sie mehrere SQL-Datenbank-Ausgaben erstellen. Sie können die SQL-Ausgaben so konfigurieren, dass sie auf verschiedene Datenbanken verweisen.

    **Eingabe**. Wählen Sie „EdgeHub“ als Eingabe für den Edgeauftrag aus, und geben Sie die erforderlichen Ressourceninformationen ein.

    **Ausgabe**. Wählen Sie „SQL-Datenbank“ als Ausgabe aus. Wählen Sie **SQL-Datenbankeinstellungen manuell bereitstellen** aus. Geben Sie die Konfigurationsdetails für die Datenbank und Tabelle ein.

    |Feld      | BESCHREIBUNG |
    |---------------|-------------|
    |Ausgabealias | Name des Ausgabealias.|
    |Datenbank | Name der SQL-Datenbank. Dies muss ein gültiger Name einer Datenbank in der SQL Database Edge-Instanz sein.|
    |Servername | Name (oder IP-Adresse) und Portnummer für die SQL Server-Instanz. Bei einer SQL Database Edge-Bereitstellung können Sie **tcp:.,1433** für den Servernamen verwenden.|
    |Username | SQL-Anmeldekonto, das über Datenleser- und Datenschreiberzugriff auf die zuvor angegebene Datenbank verfügt.|
    |Kennwort | Kennwort für das zuvor angegebene SQL-Anmeldekonto.|
    |Table | Name der Tabelle, die für den Streamingauftrag ausgegeben wird.|
    |Partitionierung erben| Ermöglicht das Erben des Partitionierungsschemas Ihres vorherigen Abfrageschritts oder Ihrer vorherigen Eingabe. Wenn diese Option aktiviert ist, können Sie einen besseren Durchsatz erwarten, wenn Sie in eine datenträgerbasierte Tabelle schreiben und es eine vollständig parallele Topologie für Ihren Auftrag gibt.|
    |Batchgröße| Die maximale Anzahl von Datensätzen, die bei jeder Masseneinfügungstransaktion gesendet werden.|

    Hier ist eine Beispielkonfiguration für die Ein- und Ausgabe:

    ```txt
        Input:
            Input from EdgeHub
            Input alias: input
            Event serialization format: JSON
            Encoding: UTF-8
            Event compression type: None

        Output:
            Output alias: output
            Database:  MeasurementsDB
            Server name: tcp:.,1433
            Username: sa
            Password: <Password>
            Table: TemperatureMeasurements
            Inherit Partitioning: Merge all input partitions into a single writer
            Max batch count: 10000
    ```

    > [!NOTE]
    > Weitere Informationen zum SQL-Ausgabeadapter für Azure Stream Analytics finden Sie unter [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](../stream-analytics/stream-analytics-sql-output-perf.md).

4. Definieren Sie die ASA-Auftragsabfrage für den Edgeauftrag. Diese Abfrage muss die definierten Eingabe- und Ausgabealiase als Eingabe- und Ausgabenamen verwenden. Weitere Informationen finden Sie in der [Referenz zur Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference).

5. Legen Sie die Speicherkontoeinstellungen für den Edgeauftrag fest. Das Speicherkonto wird als Veröffentlichungsziel für den Edgeauftrag verwendet.

6. Wählen Sie unter **Konfigurieren** die Option **Veröffentlichen** und dann die Schaltfläche **Veröffentlichen** aus. Speichern Sie den SAS-URI zur Verwendung mit dem SQL Database Edge-Modul.

### <a name="deploy-the-stream-analytics-edge-job-to-sql-database-edge"></a>Bereitstellen des Stream Analytics-Edgeauftrags in SQL Database Edge

Wenn Sie den Streamingauftrag im SQL Database Edge-Modul bereitstellen möchten, aktualisieren Sie die Konfiguration des Moduls so, dass es den SAS-URI für den Streamingauftrag aus dem vorhergehenden Schritt enthält. So aktualisieren Sie das SQL Database Edge-Modul:

1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Bereitstellung.

2. Wählen Sie im linken Bereich die Option **IoT Edge** aus.

3. Suchen Sie auf der Seite **IoT Edge** nach der IoT Edge-Instanz, in der das SQL Database Edge-Modul bereitgestellt wurde, und wählen Sie sie aus.

4. Wählen Sie auf der Geräteseite **IoT Edge-Gerät** die Option **Modul festlegen** aus.

5. Wählen Sie auf der Seite **Module festlegen** für das SQL Database Edge-Modul **Konfigurieren** aus.

6. Wählen Sie im Bereich **Benutzerdefinierte IoT Edge-Module** die Option **Gewünschte Eigenschaften für Modulzwilling festlegen** aus. Aktualisieren Sie die gewünschten Eigenschaften so, dass sie den URI für die Option `ASAJobInfo` enthalten, wie im folgenden Beispiel gezeigt wird.

    > [!NOTE]
    > Der SAS-URI im nachstehenden JSON-Code ist nur ein Beispiel. Ersetzen Sie diesen URI durch den tatsächlichen URI aus Ihrer Bereitstellung.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "https://storageAccountName.blob.core.windows.net/asajobs/ASAEdgeJobs/5a9b34db-7a5c-4606-adae-4c8609eaa1c7/d85b5aa6-4d38-4703-bb34-af7f0bd7916d/ASAEdgeJobDefinition.zip?sv=2018-03-28&sr=b&sig=HH%2BFMsEy378RaTxIy2Xo6rM6wDaqoBaZ5zFDBqdZiS0%3D&st=2019-09-01T22%3A24%3A34Z&se=2019-09-30T22%3A34%3A34Z&sp=r"   
            }
        }
    ```

7. Wählen Sie **Speichern** aus.

8. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** aus.

9. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** und dann **Senden** aus.

10. Nach dem Modulupdate wird die Stream Analytics-Auftragsdatei heruntergeladen, entzippt und für die SQL Database Edge-Instanz bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

- Details zu Preisen und Verfügbarkeit finden Sie unter [Azure SQL Database Edge](https://azure.microsoft.com/services/sql-database-edge/).
- Fordern Sie die Aktivierung von Azure SQL Database Edge für Ihr Abonnement an.
- Informationen zu den ersten Schritten finden Sie unter [Bereitstellen von SQL Database Edge über das Azure-Portal](deploy-portal.md).
