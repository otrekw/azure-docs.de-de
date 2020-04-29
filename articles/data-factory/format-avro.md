---
title: Avro-Format in Azure Data Factory
description: In diesem Thema wird der Umgang mit dem Avro-Format in Azure Data Factory beschrieben.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: jingwang
ms.openlocfilehash: 931287fa2a4104069b101236bec9f76bb7193e8d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416352"
---
# <a name="avro-format-in-azure-data-factory"></a>Avro-Format in Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Nutzen Sie diesen Artikel, wenn Sie die **Avro-Dateien analysieren oder Daten im Avro-Format schreiben** möchten. 

Das Avro-Format wird für folgende Connectors unterstützt: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Dateisystem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [HTTP](connector-http.md) und [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Avro-Dataset unterstützt werden.

| Eigenschaft         | BESCHREIBUNG                                                  | Erforderlich |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | Die type-Eigenschaft des Datasets muss auf **Avro** festgelegt werden. | Ja      |
| location         | Speicherorteinstellungen der Datei(en) Jeder dateibasierte Connector verfügt unter `location` über seinen eigenen Speicherorttyp und unterstützte Eigenschaften. **Informationen hierzu finden Sie im Abschnitt „Dataset-Eigenschaften“ des Artikels über Connectors**. | Ja      |
| avroCompressionCodec | Der Komprimierungscodec, der beim Schreiben in Avro-Dateien verwendet werden soll. Beim Lesen von Avro-Dateien legt Data Factory den Codec für die Komprimierung automatisch anhand der Dateimetadaten fest.<br>Unterstützte Typen sind **none** (Standard), **deflate** und **snappy**. Hinweis: „Snappy“ wird derzeit von der Kopieraktivität für das Lesen/Schreiben von Avro-Dateien nicht unterstützt. | Nein       |

> [!NOTE]
> Leerzeichen im Spaltennamen werden bei Avro-Dateien nicht unterstützt.

Nachfolgend sehen Sie ein Beispiel für ein Avro-Dataset in Azure Blob Storage:

```json
{
    "name": "AvroDataset",
    "properties": {
        "type": "Avro",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "avroCompressionCodec": "snappy"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Avro-Quelle und -Senke unterstützt werden.

### <a name="avro-as-source"></a>Avro als Quelle

Die folgenden Eigenschaften werden im Abschnitt ***\*source\**** der Kopieraktivität unterstützt.

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **AvroSource** festgelegt werden. | Ja      |
| storeSettings | Eine Gruppe von Eigenschaften für das Lesen von Daten aus einem Datenspeicher. Jeder dateibasierte Connector verfügt unter `storeSettings` über eigene unterstützte Leseeinstellungen. **Informationen hierzu finden Sie im Abschnitt über die Eigenschaften der Kopieraktivität im Artikel über Connectors**. | Nein       |

### <a name="avro-as-sink"></a>Avro als Senke

Die folgenden Eigenschaften werden im Abschnitt ***\*sink\**** der Kopieraktivität unterstützt:

| Eigenschaft      | BESCHREIBUNG                                                  | Erforderlich |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **AvroSink** festgelegt werden. | Ja      |
| storeSettings | Eine Gruppe von Eigenschaften für das Schreiben von Daten in einen Datenspeicher. Jeder dateibasierte Connector verfügt unter `storeSettings` über eigene unterstützte Schreibeinstellungen. **Informationen hierzu finden Sie im Abschnitt über die Eigenschaften der Kopieraktivität im Artikel über Connectors**. | Nein       |

## <a name="data-type-support"></a>Datentypunterstützung

### <a name="copy-activity"></a>Copy-Aktivität
[Komplexe Datentypen von Avro](https://avro.apache.org/docs/current/spec.html#schema_complex) (Datensätze, Enumerationen, Arrays, Zuordnungen, Unions und Konstanten) werden in der Kopieraktivität nicht unterstützt.

### <a name="data-flows"></a>Datenflüsse
Beim Arbeiten mit Avro-Dateien in Datenflüssen können Sie komplexe Datentypen lesen und schreiben, Sie müssen aber zuerst unbedingt das physische Schema aus dem Dataset löschen. In Datenflüssen können Sie die logische Projektion festlegen und Spalten ableiten, bei denen es sich um komplexe Strukturen handelt. Anschließend können diese Felder einer Avro-Datei automatisch zugeordnet werden.

## <a name="next-steps"></a>Nächste Schritte

- [Kopieraktivität – Übersicht](copy-activity-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [GetMetadata-Aktivität](control-flow-get-metadata-activity.md)
