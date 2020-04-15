---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 30/03/2020
ms.author: orspodek
ms.openlocfilehash: 9ae820097f8515dc44c67d95366f96c241cb77a1
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522818"
---
## <a name="select-an-ingestion-type"></a>Auswahl eines Erfassungstyps

Wählen Sie für **Erfassungstyp** eine der folgenden Optionen aus:
   * **Aus Speicher**: Fügen Sie im Feld **Mit Speicher verknüpfen** die URL des Speicherkontos hinzu. Verwenden Sie die [Blob-SAS-URL](/azurevs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) für private Speicherkonten.
   
      ![1-Klick-Erfassung aus Speicher](media/data-explorer-one-click-ingestion-types/from-storage-blob.png)

    * **Aus Datei**: Wählen Sie **Durchsuchen** aus, um nach der Datei zu suchen, oder ziehen Sie die Datei in das Feld.
  
      ![1-Klick-Erfassung aus Datei](media/data-explorer-one-click-ingestion-types/from-file.png)

    * **Aus Container**: Fügen Sie im Feld **Mit Speicher verknüpfen** die [SAS-URL](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) des Containers hinzu, und geben Sie optional die Stichprobengröße ein.

      ![1-Klick-Erfassung aus Container](media/data-explorer-one-click-ingestion-types/from-container.png)

  Es wird eine Stichprobe der Daten angezeigt. Sie können diese Anzeige filtern, um nur Dateien anzuzeigen, die mit bestimmten Zeichen beginnen und/oder auf bestimmte Zeichen enden. Die Vorschau wird automatisch aktualisiert, sobald Sie die Filter anpassen.
  
  Sie können die Anzeige beispielsweise so filtern, dass alle Dateien aufgelistet werden, die mit dem Begriff *Daten* beginnen und auf die Erweiterung *.csv.gz* enden.

  ![Filter bei der 1-Klick-Erfassung](media/data-explorer-one-click-ingestion-types/from-container-with-filter.png)
