---
title: Datenkonsistenzprüfung in der Kopieraktivität
description: Hier erfahren Sie, wie Sie die Datenkonsistenzprüfung in der Kopieraktivität in Azure Data Factory aktivieren.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: ''
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 3/27/2020
ms.author: yexu
ms.openlocfilehash: a386c7d44cf5ba7eda895006cda7ce1fa9b798ac
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83663708"
---
#  <a name="data-consistency-verification-in-copy-activity-preview"></a>Datenkonsistenzprüfung in der Kopieraktivität (Vorschau)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Wenn Sie Daten aus dem Quell- in den Zielspeicher verschieben, bietet die Azure Data Factory-Kopieraktivität eine Option, mit der Sie eine zusätzliche Datenkonsistenzprüfung durchführen können. Damit stellen Sie sicher, dass die Daten nicht nur erfolgreich aus dem Quell- in den Zielspeicher kopiert werden, sondern auch dass überprüft wird, ob sie zwischen Quell- und Zielspeicher konsistent sind. Sobald während der Datenverschiebung inkonsistente Daten gefunden wurden, können Sie entweder die Kopieraktivität abbrechen oder auch noch die restlichen Daten kopieren, indem Sie die Einstellung für Fehlertoleranz aktivieren, damit inkonsistente Daten übersprungen werden. Die Namen der übersprungenen Objekte können Sie durch Aktivieren der Einstellung für das Sitzungsprotokoll in der Kopieraktivität abrufen. 

> [!IMPORTANT]
> Dieses Feature befindet sich zurzeit in der Vorschauphase mit den folgenden Einschränkungen, an denen wir aktiv arbeiten:
>- Die Datenkonsistenzprüfung ist nur für Binärdateien verfügbar, die zwischen dateibasierten Speichern mit dem Verhalten „PreserveHierarchy“ in der Kopieraktivität kopiert werden. Beim Kopieren von Tabellendaten ist die Datenkonsistenzprüfung in der Kopieraktivität noch nicht verfügbar.
>- Wenn Sie die Einstellung „Sitzungsprotokoll“ in der Kopieraktivität aktivieren, um die übersprungenen inkonsistenten Dateien zu protokollieren, kann die Vollständigkeit der Protokolldatei bei einem Fehlschlagen der Aktivität nicht 100%-ig garantiert werden.
>- Das Sitzungsprotokoll enthält nur inkonsistente Dateien, bei denen die erfolgreich kopierten Dateien bisher nicht protokolliert werden.

## <a name="supported-data-stores"></a>Unterstützte Datenspeicher

### <a name="source-data-stores"></a>Quelldatenspeicher

-   [Azure Blob Storage](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)
-   [Azure File Storage](connector-azure-file-storage.md)
-   [Amazon S3](connector-amazon-simple-storage-service.md)
-   [Dateisystem](connector-file-system.md)
-   [HDFS](connector-hdfs.md)

### <a name="destination-data-stores"></a>Zieldatenspeicher

-   [Azure Blob Storage](connector-azure-blob-storage.md)
-   [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md)
-   [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md)
-   [Azure File Storage](connector-azure-file-storage.md)
-   [Dateisystem](connector-file-system.md)


## <a name="configuration"></a>Konfiguration
Das folgende Beispiel zeigt eine JSON-Definition, mit der die Datenkonsistenzprüfung in der Kopieraktivität aktiviert wird: 

```json
"typeProperties": { 
"source": { 
        "type": "BinarySource", 
        "storeSettings": { 
            "type": "AzureDataLakeStoreReadSettings", 
            "recursive": true 
        } 
    }, 
    "sink": { 
        "type": "BinarySink", 
        "storeSettings": { 
            "type": "AzureDataLakeStoreWriteSettings" 
        } 
}, 
    "validateDataConsistency": true, 
    "skipErrorFile": { 
        "dataInconsistency": true 
    }, 
    "logStorageSettings": { 
        "linkedServiceName": { 
            "referenceName": "ADLSGen2_storage", 
            "type": "LinkedServiceReference" 
        }, 
        "path": "/sessionlog/" 
} 
} 
```

Eigenschaft | BESCHREIBUNG | Zulässige Werte | Erforderlich
-------- | ----------- | -------------- | -------- 
validateDataConsistency | Wenn Sie für diese Eigenschaft den Wert „true“ festlegen, prüft die Kopieraktivität die Dateigröße, „LastModifiedDate“ (Datum der letzten Änderung) und die MD5-Prüfsumme für jedes aus dem Quell- in den Zielspeicher kopierte Objekt, um die Datenkonsistenz zwischen den beiden Speichern sicherzustellen. Beachten Sie, dass die Kopierleistung durch Aktivieren dieser Option beeinträchtigt wird.  | True<br/>False (Standardwert) | Nein
dataInconsistency | Eines der Schlüssel-Wert-Paare in der „skipErrorFile“-Eigenschaftensammlung zur Bestimmung, ob Sie die inkonsistenten Daten überspringen möchten.<br/> – True: Sie möchten den Rest kopieren, indem Sie inkonsistente Daten überspringen.<br/> – False: Sie möchten die Kopieraktivität abbrechen, sobald inkonsistente Daten gefunden wurden.<br/>Beachten Sie, dass diese Eigenschaft nur gültig ist, wenn Sie „validateDataConsistency“ als „True“ festlegen.  | True<br/>False (Standardwert) | Nein
logStorageSettings | Eine Gruppe von Eigenschaften, die angegeben werden können, um das Sitzungsprotokoll zum Protokollieren übersprungener Objekte zu aktivieren. | | Nein
linkedServiceName | Der verknüpfte Dienst von [Azure Blob Storage](connector-azure-blob-storage.md#linked-service-properties) oder [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md#linked-service-properties) zum Speichern der Sitzungsprotokolldateien. | Die Namen eines verknüpften Diensts vom Typ `AzureBlobStorage` oder `AzureBlobFS`, der auf die Instanz verweist, in der Sie die Protokolldateien speichern. | Nein
path | Der Pfad der Protokolldateien. | Geben Sie den Pfad an, in dem die Protokolldateien gespeichert werden sollen. Wenn Sie keinen Pfad angeben, erstellt der Dienst automatisch einen Container. | Nein

>[!NOTE]
>- Die Datenkonsistenz wird im Stagingkopierszenario nicht unterstützt. 
>- Beim Kopieren von Binärdateien aus einem beliebigen Storage-Speicher in Azure Blob Storage oder Azure Data Lake Storage Gen2 überprüft die Kopieraktivität die Dateigröße und die MD5-Prüfsumme, um die Datenkonsistenz zwischen Quell- und Zielspeicher sicherzustellen. 
>- Beim Kopieren von Binärdateien aus einem beliebigen anderen Storage-Speicher als Azure Blob Storage oder Azure Data Lake Storage Gen2 überprüft die Kopieraktivität die Dateigröße, um die Datenkonsistenz zwischen Quell- und Zielspeicher sicherzustellen.


## <a name="monitoring"></a>Überwachung

### <a name="output-from-copy-activity"></a>Ausgabe aus der Kopieraktivität
Nach Abschluss aller Kopieraktivitätsausführungen wird das Ergebnis der Datenkonsistenzprüfung aus der Ausgabe der jeweiligen Ausführung angezeigt:

```json
"output": {
            "dataRead": 695,
            "dataWritten": 186,
            "filesRead": 3,  
            "filesWritten": 1, 
            "filesSkipped": 2, 
            "throughput": 297,
            "logPath": "https://myblobstorage.blob.core.windows.net//myfolder/a84bf8d4-233f-4216-8cb5-45962831cd1b/",
            "dataConsistencyVerification": 
           { 
                "VerificationResult": "Verified", 
                "InconsistentData": "Skipped" 
           } 
        }

```
Hier können Sie die Details der Datenkonsistenzprüfung aus der Eigenschaft „dataConsistencyVerification“ sehen.

Wert von **VerificationResult**: 
-   **Überprüft**:  Ihre kopierten Daten wurden auf Konsistenz zwischen Quell- und Zielspeicher überprüft. 
-   **NotVerified** (NichtÜberprüft): Ihre kopierten Daten wurden nicht auf Konsistenz überprüft, weil Sie „validateDataConsistency“ in der Kopieraktivität nicht aktiviert haben. 
-   **Nicht unterstützt**: Ihre kopierten Daten wurden nicht auf Konsistenz überprüft, weil die Datenkonsistenzprüfung bei diesem bestimmten Kopierpaar nicht unterstützt wird. 

Wert von **InconsistentData**: 
-   **Gefunden**: Die ADF-Kopieraktivität hat inkonsistente Daten gefunden. 
-   **Übersprungen**: Die ADF-Kopieraktivität hat inkonsistente Daten gefunden und übersprungen. 
-   **Keine:** Die ADF-Kopieraktivität hat keine inkonsistenten Daten gefunden. Der Grund: Bei der Überprüfung wurde festgestellt, dass Ihre Daten zwischen Quell- und Zielspeicher konsistent sind, oder Sie haben „validateDataConsistency“ in der Kopieraktivität deaktiviert. 

### <a name="session-log-from-copy-activity"></a>Sitzungsprotokoll aus der Kopieraktivität

Wenn Sie so konfigurieren, dass die inkonsistente Datei protokolliert wird, können Sie die Protokolldatei unter diesem Pfad finden: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/copyactivity-logs/[copy-activity-name]/[copy-activity-run-id]/[auto-generated-GUID].csv`.  Die Protokolldateien sind die CSV-Dateien. 

Das Schema einer Protokolldatei lautet wie folgt:

Column | BESCHREIBUNG 
-------- | -----------  
Timestamp | Der Zeitstempel, der angibt, wann ADF die inkonsistenten Dateien überspringt.
Ebene | Die Protokollstufe dieses Elements. Sie lautet bei dem Element, mit dem das Überspringen der Datei angezeigt wird, „Warning“ (Warnung).
Vorgangsname | Das Vorgangsverhalten der ADF-Kopieraktivität für jede Datei. Es lautet „FileSkip“ zur Angabe der Datei, die übersprungen werden soll.
OperationItem | Der Name der zu überspringenden Datei.
`Message` | Weitere Informationen zu den Ursachen für das Überspringen von Dateien.

Das Beispiel für eine Protokolldatei lautet wie folgt: 
```
Timestamp, Level, OperationName, OperationItem, Message
2020-02-26 06:22:56.3190846, Warning, FileSkip, "sample1.csv", "File is skipped after read 548000000 bytes: ErrorCode=DataConsistencySourceDataChanged,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Source file 'sample1.csv' is changed by other clients during the copy activity run.,Source=,'." 
```
In der vorstehenden Protokolldatei können Sie sehen, dass die Datei „sample1.csv“ übersprungen wurde, weil die Konsistenz zwischen Quell- und Zielspeicher nicht überprüft werden konnte. Weitere Details zu dem Grund, aus dem „sample1.csv“ inkonsistent wird: Die Datei wurde von anderen Anwendungen bei gleichzeitiger Ausführung der ADF-Kopieraktivität geändert. 



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den anderen Artikeln zur Kopieraktivität:

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Copy activity fault tolerance (Fehlertoleranz der Kopieraktivität)](copy-activity-fault-tolerance.md)


