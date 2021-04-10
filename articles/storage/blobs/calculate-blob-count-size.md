---
title: Berechnen der Anzahl und Größe von Blobs mit dem Azure Storage-Bestand
description: Erfahren Sie, wie Sie die Anzahl und Gesamtgröße von Blobs pro Container berechnen.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/10/2021
ms.service: storage
ms.subservice: blobs
ms.topic: how-to
ms.openlocfilehash: 92e5b00cd655677cdc3096bc2142dfe1b704adf2
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "102637066"
---
# <a name="calculate-blob-count-and-total-size-per-container-using-azure-storage-inventory"></a>Berechnen der Anzahl und Gesamtgröße von Blobs pro Container mit dem Azure Storage-Bestand

In diesem Artikel werden der Azure Blob Storage-Bestand und Azure Synapse verwendet, um die Blobanzahl und die Gesamtgröße von Blobs pro Container zu berechnen. Diese Werte sind bei der Optimierung der Blobverwendung pro Container nützlich.

Die Blobmetadaten werden bei dieser Methode nicht einbezogen. Bei der Funktion für den Azure Blob Storage-Bestand wird die REST-API [List Blobs](/rest/api/storageservices/list-blobs) mit Standardparametern verwendet. Daher werden in diesem Beispiel keine Momentaufnahmen, „$“-Container usw. unterstützt.

> [!IMPORTANT]
> Der Blobbestand befindet sich derzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten rechtliche Bedingungen. Sie gelten für diejenigen Azure-Features, die sich in der Beta- oder Vorschauversion befinden oder aber anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="enable-inventory-reports"></a>Aktivieren von Bestandsberichten

Im ersten Schritt dieser Methode werden in Ihrem Speicherkonto [Bestandsberichte aktiviert](blob-inventory.md#enable-inventory-reports). Möglicherweise müssen Sie nach dem Aktivieren von Bestandsberichten bis zu 24 Stunden warten, bis der erste Bericht generiert werden kann.

Wenn Sie einen Bestandsbericht analysieren möchten, müssen Sie sich Lesezugriff für Blobs in dem Container gewähren, in dem sich die CSV-Datei des Berichts befindet.

1. Navigieren Sie zu dem Container mit der CSV-Berichtsdatei für den Bestand.
1. Wählen Sie **Zugriffssteuerung (IAM)** und dann **Add role assignments** (Rollenzuweisungen hinzufügen) aus.

    :::image type="content" source="media/calculate-blob-count-size/access.png" alt-text="Auswählen von „Add role assignments“ (Rollenzuweisungen hinzufügen)":::

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Storage-Blobdatenleser** aus.

    :::image type="content" source="media/calculate-blob-count-size/add-role-assignment.png" alt-text="Hinzufügen von „Storage-Blobdatenleser“ über die Dropdownliste":::

1. Geben Sie im Feld **Auswählen** die E-Mail-Adresse des Kontos ein, das Sie zum Ausführen des Berichts verwenden.

## <a name="create-an-azure-synapse-workspace"></a>Erstellen eines Azure Synapse-Arbeitsbereichs

[Erstellen Sie einen Azure Synapse-Arbeitsbereich](/azure/synapse-analytics/get-started-create-workspace), in dem Sie eine SQL-Abfrage ausführen, um die Bestandsergebnisse zu protokollieren.

## <a name="create-the-sql-query"></a>Erstellen der SQL-Abfrage

Führen Sie nach dem Erstellen des Azure Synapse-Arbeitsbereichs die folgenden Schritte aus.

1. Navigieren Sie zu [https://web.azuresynapse.net](https://web.azuresynapse.net).
1. Wählen Sie die Registerkarte **Entwickeln** am linken Rand aus.
1. Wählen Sie das große Pluszeichen (+) aus, um ein Element hinzuzufügen.
1. Wählen Sie **SQL-Skript** aus.

    :::image type="content" source="media/calculate-blob-count-size/synapse-sql-script.png" alt-text="Auswählen von „SQL-Skript“ zum Erstellen einer neuen Abfrage":::

## <a name="run-the-sql-query"></a>Ausführen der SQL-Abfrage

1. Fügen Sie die folgende SQL-Abfrage in Ihrem Azure Synapse-Arbeitsbereich hinzu, um [die CSV-Bestandsdatei zu lesen](/azure/synapse-analytics/sql/query-single-csv-file#read-a-csv-file).

    Verwenden Sie für den `bulk`-Parameter die URL der CSV-Datei für den Bestandsbericht, die analysiert werden soll.

    ```sql
    SELECT LEFT([Name], CHARINDEX('/', [Name]) - 1) AS Container, 
            COUNT(*) As TotalBlobCount,
            SUM([Content-Length]) As TotalBlobSize
    FROM OPENROWSET(
        bulk '<URL to your inventory CSV file>',
        format='csv', parser_version='2.0', header_row=true
    ) AS Source
    GROUP BY LEFT([Name], CHARINDEX('/', [Name]) - 1)
    ```

1. Geben Sie im Eigenschaftenbereich rechts einen Namen für die SQL-Abfrage ein.

1. Veröffentlichen Sie die SQL-Abfrage durch Drücken von STRG+S oder durch Auswählen der Schaltfläche **Alle veröffentlichen**.

1. Wählen Sie die Schaltfläche **Ausführen** aus, um die SQL-Abfrage auszuführen. Die Anzahl und Gesamtgröße der Blobs pro Container werden im **Ergebnisbereich** angezeigt.

    :::image type="content" source="media/calculate-blob-count-size/output.png" alt-text="Ausgabe nach Ausführung des Skripts zum Berechnen der Anzahl und Gesamtgröße der Blobs":::

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten von Blobdaten mit dem Azure Storage-Blobbestand](blob-inventory.md)
- [Berechnen der Gesamtabrechnungsgröße eines Blobcontainers](../scripts/storage-blobs-container-calculate-billing-size-powershell.md)
