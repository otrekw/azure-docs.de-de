---
title: 'Import Data (Daten importieren): Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul Import Data in Azure Machine Learning verwenden, um Daten aus vorhandenen Clouddatendiensten in eine Machine Learning-Pipeline zu laden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 8b1e4c60e6054141e71d2509fe34c7ca9baf9ca6
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/25/2020
ms.locfileid: "77598458"
---
# <a name="import-data-module"></a>Modul „Import Data“ (Daten importieren)

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie dieses Modul, um Daten aus vorhandenen Clouddatendiensten in eine Machine Learning-Pipeline zu laden. 

> [!Note]
> Alle von diesem Modul bereitgestellten Funktionen können über **Datenspeicher** und **Datasets** auf der Landing Page des Arbeitsbereichs ausgeführt werden. Die Verwendung von **Datenspeichern** und **Datasets** wird empfohlen, da hierbei zusätzliche Features wie Datenüberwachung eingeschlossen sind. Weitere Informationen finden Sie in den Artikeln [Zugreifen auf Daten](../how-to-access-data.md) und [Registrieren von Datasets](../how-to-create-register-datasets.md).
> Nachdem Sie ein Dataset registriert haben, finden Sie es auf der Designer-Oberfläche in der Kategorie **Datasets** -> **My Datasets** (Meine Datasets). Dieses Modul ist für Benutzer von Studio (klassisch) reserviert, die eine vertraute Benutzeroberfläche verwenden möchten. 
>

Das Modul **Import Data** unterstützt das Lesen von Daten aus den folgenden Quellen:

- URL über HTTP
- Azure-Cloudspeicher über [**Datenspeicher**](../how-to-access-data.md)
    - Azure-Blobcontainer
    - Azure-Dateifreigabe
    - Azure Data Lake
    - Azure Data Lake Gen2
    - Azure SQL-Datenbank
    - Azure PostgreSQL    

Bevor Sie Cloudspeicher verwenden können, müssen Sie zunächst einen Datenspeicher in Ihrem Azure Machine Learning-Arbeitsbereich registrieren. Weitere Informationen hierzu finden Sie unter [Zugreifen auf Daten](../how-to-access-data.md). 

Nachdem Sie die gewünschten Daten definiert und eine Verbindung mit der Quelle hergestellt haben, leitet **[Import Data](./import-data.md)** den Datentyp jeder Spalte basierend auf den darin enthaltenen Werten ab und lädt die Daten in Ihre Designer-Pipeline. **Import Data** gibt ein Dataset aus, das mit beliebigen Designer-Pipelines verwendet werden kann.

Wenn sich Ihre Quelldaten ändern, können Sie das Dataset aktualisieren und neue Daten hinzufügen, indem Sie [Import Data](./import-data.md) erneut ausführen.

## <a name="how-to-configure-import-data"></a>Konfigurieren von „Import Data“ (Daten importieren)

1. Fügen Sie Ihrer Pipeline das Modul **Import Data** (Daten importieren) hinzu. Sie finden dieses Modul in der Kategorie **Data Input and Output** (Dateieingabe und -ausgabe) im Designer.

1. Wählen Sie das Modul aus, um den rechten Bereich zu öffnen.

1. Wählen Sie **Datenquelle** und dann den Datenquellentyp aus. Dabei kann es sich um „HTTP“ oder „Datenspeicher“ handeln.

    Wenn Sie „Datenspeicher“ auswählen, können Sie vorhandene Datenspeicher auswählen, die bereits in Ihrem Azure Machine Learning-Arbeitsbereich registriert sind, oder einen neuen Datenspeicher erstellen. Anschließend definieren Sie den Pfad der zu importierenden Daten im Datenspeicher. Sie können den Pfad ganz einfach durchsuchen, indem Sie auf **Pfad durchsuchen** ![import-data-path](media/module/import-data-path.png) klicken.

1. Wählen Sie das Vorschauschema aus, um die Spalten zu filtern, die Sie einschließen möchten. Sie können auch erweiterte Einstellungen wie Trennzeichen in den Analyseoptionen definieren.

    ![import-data-preview](media/module/import-data.png)

1. Das Kontrollkästchen **Ausgabe erneut generieren** legt fest, ob das Modul zum erneuten Generieren der Ausgabe zur Laufzeit ausgeführt werden soll. 

    Die Option ist standardmäßig deaktiviert. Dies bedeutet, dass das System die Ausgabe der letzten Ausführung wiederverwendet, um die Laufzeit zu verkürzen, wenn das Modul bereits mit denselben Parametern ausgeführt wurde. 

    Wenn sie ausgewählt ist, führt das System das Modul erneut aus, um die Ausgabe neu zu generieren. Wählen Sie also diese Option aus, wenn die zugrunde liegenden Daten im Speicher aktualisiert werden, um die neuesten Daten zu erhalten.


1. Ausführen der Pipeline.

    Wenn die Daten durch Import Data in den Designer geladen werden, wird der Datentyp jeder Spalte basierend auf den darin enthaltenen Werten (numerisch oder kategorisch) abgeleitet.

    Sofern eine Spaltenüberschrift vorhanden ist, wird diese zur Benennung der Spalten im Ausgabedataset verwendet.

    Wenn keine Spaltenüberschriften in den Daten enthalten sind, werden neue Spaltennamen im Format col1, col2,... , coln* generiert.

## <a name="results"></a>Ergebnisse

Nachdem der Import abgeschlossen wurde, klicken Sie auf das Ausgabedataset, und wählen Sie **Visualize** (Visualisieren) aus, um zu überprüfen, ob die Daten erfolgreich importiert wurden.

Wenn Sie die Daten zur Wiederverwendung speichern möchten, anstatt bei jeder Pipelineausführung ein neues Dataset zu importieren, wählen Sie das Symbol für **Dataset registrieren** unter der Registerkarte **Ausgaben** im rechten Bereich des Moduls aus. Wählen Sie einen Namen für das Dataset aus. Das gespeicherte Dataset enthält die Daten zum Zeitpunkt der Speicherung. Das Dataset wird bei erneuter Ausführung der Pipeline nicht aktualisiert. Das gilt auch, wenn sich das Dataset in der Pipeline ändert. Dies kann hilfreich sein, um Momentaufnahmen von Daten zu erstellen.

Nach dem Importieren der Daten können einige zusätzliche Vorbereitungen zur Modellierung und Analyse erforderlich sein:

- Verwenden Sie [Edit Metadata](./edit-metadata.md) (Metadaten bearbeiten), um Spaltennamen zu ändern, einen abweichenden Datentyp für eine Spalte festzulegen, oder um anzugeben, dass einige Spalten für Bezeichnungen oder Features stehen.

- Verwenden Sie [Select Columns in Dataset](./select-columns-in-dataset.md) (Spalten im Dataset auswählen), um eine Teilmenge von Spalten auszuwählen, die während der Modellierung transformiert oder verwendet werden sollen. Über das Modul [Add Columns](./add-columns.md) (Spalten hinzufügen) lassen sich die transformierten oder entfernten Spalten ganz einfach wieder mit dem ursprünglichen Dataset verknüpfen.  

- Verwenden Sie [Partition and Sample](./partition-and-sample.md) (Partitionieren und Stichprobe entnehmen), um das Dataset zu unterteilen, ein Sampling durchzuführen oder um Top-N-Zeilen abzurufen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 
