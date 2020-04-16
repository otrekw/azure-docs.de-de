---
title: Kopieren von Dateien aus mehreren Containern
description: Erfahren Sie, wie Sie mit Azure Data Factory eine Lösungsvorlage verwenden, um Dateien aus mehreren Containern zu kopieren.
services: data-factory
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 11/1/2018
ms.openlocfilehash: 383b70bbb02e7a200c7ec0a994f7cf11e9b9520e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414817"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>Kopieren von Dateien aus mehreren Containern mit Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel wird eine Lösungsvorlage beschrieben, mit der Sie Dateien aus mehreren Containern zwischen Dateispeichern kopieren können. Beispielsweise könnten Sie Ihr Data Lake-Repository aus AWS S3 in Azure Data Lake Storage migrieren. Oder Sie können die Vorlage verwenden, um alles aus einem Azure Blob Storage-Konto in ein anderes zu replizieren.

> [!NOTE]
> Wenn Sie Dateien aus einem einzelnen Container kopieren möchten, ist es effizienter, mit dem [Tool „Daten kopieren“](copy-data-tool.md) eine Pipeline mit einer einzelnen Kopieraktivität zu erstellen. Die Vorlage in diesem Artikel ist für dieses einfache Szenario umfangreicher als notwendig.

## <a name="about-this-solution-template"></a>Informationen zu dieser Lösungsvorlage

Diese Vorlage zählt die Container in Ihrem Quellspeicher auf. Anschließend kopiert sie diese Container in den Zielspeicher.

Die Vorlage enthält drei Aktivitäten:
- **GetMetadata** dient zum Überprüfen Ihres Quellspeichers und zum Abrufen der Containerliste.
- **ForEach** dient zum Abrufen der Containerliste von der Aktivität **GetMetadata** sowie zum Durchlaufen der Liste und zum Übergeben der einzelnen Container an die Kopieraktivität.
- **Copy** dient zum Kopieren der einzelnen Container aus dem Quell- in den Zielspeicher.

Die Vorlage definiert die folgenden Parameter:
- *SourceFileFolder* ist der Ordnerpfad Ihres Datenquellenspeichers, aus dem Sie eine Liste der Container abrufen können. Bei dem Pfad handelt es sich um das Stammverzeichnis, das mehrere Containerordner enthält. Der Standardwert dieses Parameters lautet `sourcefolder`.
- *SourceFileDirectory* ist der Unterordnerpfad im Stammverzeichnis des Datenquellenspeichers. Der Standardwert dieses Parameters lautet `subfolder`.
- *DestinationFileFolder* ist der Ordnerpfad, unter dem die Dateien in Ihren Zielspeicher kopiert werden. Der Standardwert dieses Parameters lautet `destinationfolder`.
- *DestinationFileDirectory* ist der Unterordnerpfad, unter dem die Dateien in Ihren Zielspeicher kopiert werden. Der Standardwert dieses Parameters lautet `subfolder`.

## <a name="how-to-use-this-solution-template"></a>So verwenden Sie diese Lösungsvorlage

1. Wechseln Sie zur Vorlage **Kopieren von Containern mit mehreren Dateien zwischen Dateispeichern**. Erstellen Sie eine **neue Verbindung** mit Ihrem Quellspeicher. In den Quellspeicher möchten Sie Dateien aus mehreren Containern kopieren.

    ![Erstellen einer neuen Verbindung mit der Quelle](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. Erstellen Sie eine **neue Verbindung** mit Ihrem Zielspeicher.

    ![Erstellen einer neuen Verbindung mit dem Ziel](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. Klicken Sie auf **Diese Vorlage verwenden**.

    ![„Diese Vorlage verwenden“](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. Daraufhin wird die Pipeline wie im folgenden Beispiel angezeigt:

    ![Pipeline anzeigen](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. Klicken Sie auf **Debuggen**, geben Sie die **Parameter** ein, und klicken Sie dann auf **Fertig stellen**.

    ![Führen Sie die Pipeline aus.](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. Überprüfen Sie das Ergebnis.

    ![Überprüfen des Ergebnisses](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen von Massenkopiervorgängen mit Steuertabelle für eine Datenbank](solution-template-bulk-copy-with-control-table.md)

- [Kopieren von Dateien aus mehreren Containern mit Azure Data Factory](solution-template-copy-files-multiple-containers.md)
