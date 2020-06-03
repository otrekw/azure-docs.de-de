---
title: Verwenden von Azure Stream Analytics-Edge-Aufträgen mit Azure SQL Edge (Vorschau)
description: Hier erfahren Sie mehr über das Verwenden von Stream Analytics-Aufträgen in Azure SQL Edge (Vorschau).
keywords: SQL Edge, Stream Analytics,
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 7c15312b48e7118517894d8ffd4807e4892e03a3
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233142"
---
# <a name="using-azure-stream-analytics-jobs-with-sql-edge"></a>Verwenden von Azure Stream Analytics-Aufträgen mit SQL Edge

Azure SQL Edge (Vorschau) ist eine optimierte relationale Datenbank-Engine für IoT- und Edge-Bereitstellungen. Sie basiert auf den aktuellen Versionen der Microsoft SQL Server-Datenbank-Engine, die branchenführende Funktionen für Leistung, Sicherheit und Abfrageverarbeitung bietet. Zusammen mit den branchenführenden Funktionen für die Verwaltung relationaler Datenbanken von SQL Server bietet Azure SQL Edge integrierte Streamingfunktionen für Echtzeitanalysen und komplexe Ereignisverarbeitung.

Azure SQL Edge enthält eine native Implementierung der Stream Analytics-Runtime. Diese Implementierung ermöglicht es Ihnen, einen Azure Stream Analytics-Edge-Auftrag zu erstellen und als SQL Edge-Streamingauftrag bereitzustellen. Azure Stream Analytics-Aufträge können in SQL Edge mithilfe des ASAJobInfo-Parameters bereitgestellt werden, der über die Option `module twin's desired properties` des SQL Edge-Moduls verfügbar gemacht wird:

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
| ASAJobInfo | Azure Blob Storage-URI für den ASA Edge-Auftrag.

## <a name="create-an-azure-stream-analytics-edge-job"></a>Erstellen eines Azure Stream Analytics-Edge-Auftrags

1. Öffnen Sie das Azure-Portal.

2. Erstellen Sie einen neuen **Azure Stream Analytics für IoT Edge**-Auftrag. Wählen Sie die Hostumgebung für **Edge** aus.

3. Definieren Sie eine Ein- und Ausgabe für den Azure Stream Analytics-Auftrag. Jede SQL-Ausgabe, die Sie hier einrichten, ist an eine einzelne Tabelle in der Datenbank gebunden. Wenn ein Streaming von Daten an mehrere Tabellen erforderlich ist, müssen Sie mehrere SQL-Datenbank-Ausgaben erstellen. Sie können die SQL-Ausgaben so konfigurieren, dass sie auf verschiedene Datenbanken verweisen.

    **Eingabe**. Wählen Sie „EdgeHub“ als Eingabe für den Edgeauftrag aus, und geben Sie die erforderlichen Ressourceninformationen ein.

    **Ausgabe**. Wählen Sie „SQL-Datenbank“ als Ausgabe aus. Wählen Sie **SQL-Datenbankeinstellungen manuell bereitstellen** aus. Geben Sie die Konfigurationsdetails für die Datenbank und Tabelle ein.

    |Feld      | BESCHREIBUNG |
    |---------------|-------------|
    |Ausgabealias | Name des Ausgabealias.|
    |Datenbank | Name der SQL-Datenbank. Dies muss ein gültiger Name einer Datenbank in der SQL Edge-Instanz sein.|
    |Servername | Name (oder IP-Adresse) und Portnummer für die SQL Server-Instanz. Bei einer SQL Edge-Bereitstellung können Sie **tcp:.,1433** für den Servernamen verwenden.|
    |Username | SQL-Anmeldekonto, das über Datenleser- und Datenschreiberzugriff auf die zuvor angegebene Datenbank verfügt.|
    |Kennwort | Kennwort für das zuvor angegebene SQL-Anmeldekonto.|
    |Tabelle | Name der Tabelle, die für den Streamingauftrag ausgegeben wird.|
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

6. Wählen Sie unter **Konfigurieren** die Option **Veröffentlichen** und dann die Schaltfläche **Veröffentlichen** aus. Speichern Sie den SAS-URI zur Verwendung mit dem SQL Edge-Modul.

## <a name="deploy-azure-stream-analytics-edge-job-to-sql-edge"></a>Bereitstellen des Stream Analytics-Edge-Auftrags in SQL Edge

Wenn Sie den Streamingauftrag im SQL Edge-Modul bereitstellen möchten, aktualisieren Sie die Konfiguration des Moduls so, dass es den SAS-URI für den Streamingauftrag aus dem vorhergehenden Schritt enthält. So aktualisieren Sie das SQL Edge-Modul:

1. Navigieren Sie im Azure-Portal zu Ihrer IoT Hub-Bereitstellung.

2. Wählen Sie im linken Bereich die Option **IoT Edge** aus.

3. Suchen Sie auf der Seite **IoT Edge** nach der IoT Edge-Instanz, in der das SQL Edge-Modul bereitgestellt wurde, und wählen Sie sie aus.

4. Wählen Sie auf der Geräteseite **IoT Edge-Gerät** die Option **Modul festlegen** aus.

5. Wählen Sie auf der Seite **Module festlegen** für das SQL Edge-Modul **Konfigurieren** aus.

6. Wählen Sie im Bereich **Benutzerdefinierte IoT Edge-Module** die Option **Gewünschte Eigenschaften für Modulzwilling festlegen** aus. Aktualisieren Sie die gewünschten Eigenschaften, um den URI für die Option `ASAJobInfo` einzubeziehen, wie im folgenden Beispiel gezeigt wird.

    > [!NOTE]
    > Der SAS-URI im nachstehenden JSON-Code ist nur ein Beispiel. Ersetzen Sie diesen URI durch den tatsächlichen URI aus Ihrer Bereitstellung.

    ```json
        {
            "properties.desired":
            {
                "ASAJobInfo": "<<<SAS URL For the published ASA Edge Job>>>>"
            }
        }
    ```

7. Wählen Sie **Speichern** aus.

8. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** aus.

9. Wählen Sie auf der Seite **Module festlegen** die Option **Weiter** und dann **Senden** aus.

10. Nach dem Modulupdate wird die Stream Analytics-Auftragsdatei heruntergeladen, entzippt und für die SQL Edge-Instanz bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von SQL Edge über das Azure-Portal](deploy-portal.md)

- [Streamen von Daten](stream-data.md)

- [Machine Learning und KI mit ONNX in SQL Edge (Vorschau)](onnx-overview.md)
