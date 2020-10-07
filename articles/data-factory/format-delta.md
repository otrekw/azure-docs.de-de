---
title: Deltaformat in Azure Data Factory
description: Transformieren und Verschieben von Daten aus einer Delta Lake-Instanz unter Verwendung des Deltaformats
author: djpmsft
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: daperlov
ms.openlocfilehash: 3e1c5f3b360960779dd58c8c05b25885df81d2e9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91276502"
---
# <a name="delta-format-in-azure-data-factory"></a>Deltaformat in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel erfahren Sie, wie Sie Daten unter Verwendung des Deltaformats in eine bzw. aus einer Delta Lake-Instanz kopieren, die in [Azure Data Lake Store Gen2](connector-azure-data-lake-storage.md) oder in [Azure Blob Storage](connector-azure-blob-storage.md) gespeichert ist. Dieser Connector ist als [Inlinedataset](data-flow-source.md#inline-datasets) in Zuordnungsdatenflüssen sowohl als Quelle als auch als Senke verfügbar.

> [!NOTE]
> Der Deltaformatconnector für Zuordnungsdatenflüsse ist zurzeit als Public Preview verfügbar.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4ALTs]

## <a name="mapping-data-flow-properties"></a>Eigenschaften von Mapping Data Flow

Dieser Connector ist als [Inlinedataset](data-flow-source.md#inline-datasets) in Zuordnungsdatenflüssen sowohl als Quelle als auch als Senke verfügbar.

### <a name="source-properties"></a>Quelleigenschaften

Die folgende Tabelle enthält die von einer Deltaquelle unterstützten Eigenschaften. Sie können diese Eigenschaften auf der Registerkarte **Quelloptionen** bearbeiten.

| Name | Beschreibung | Erforderlich | Zulässige Werte | Datenflussskript-Eigenschaft |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Format | Das Format muss `delta` sein | ja | `delta` | format |
| Dateisystem | Der Container bzw. das Dateisystem der Delta Lake-Instanz. | ja | String | fileSystem |
| Ordnerpfad | Das Verzeichnis der Delta Lake-Instanz. | ja | String | folderPath |
| Komprimierungstyp | Der Komprimierungstyp der Deltatabelle. | nein | `bzip2`<br>`gzip`<br>`deflate`<br>`ZipDeflate`<br>`snappy`<br>`lz4` | compressionType |
| Komprimierungsgrad | Wählen Sie aus, ob die Komprimierung schnellstmöglich abgeschlossen oder die resultierende Datei optimal komprimiert werden soll. | Erforderlich, wenn `compressedType` angegeben wird. | `Optimal` oder `Fastest` | compressionLevel |
| Zeitreise | Wählen Sie aus, ob eine ältere Momentaufnahme einer Deltatabelle abgefragt werden soll. | nein | Abfragen nach Zeitstempel: Timestamp <br> Abfragen nach Version: Integer | timestampAsOf <br> versionAsOf |
| Finden keiner Dateien zulässig | „true“ gibt an, dass kein Fehler ausgelöst wird, wenn keine Dateien gefunden werden. | nein | `true` oder `false` | ignoreNoFilesFound |

#### <a name="import-schema"></a>Schema importieren

Delta ist nur als Inlinedataset verfügbar und verfügt standardmäßig über kein zugeordnetes Schema. Zum Abrufen von Spaltenmetadaten klicken Sie auf der Registerkarte **Projektion** auf die Schaltfläche **Schema importieren**. Auf diese Weise können Sie auf die Spaltennamen und Datentypen verweisen, die durch den Korpus angegeben sind. Zum Importieren des Schemas muss eine [Datenfluss-Debugsitzung](concepts-data-flow-debug-mode.md) aktiv sein, und es muss eine CDM-Entitätsdefinitionsdatei vorhanden sein, auf die verwiesen werden kann.
 

### <a name="delta-source-script-example"></a>Skriptbeispiel für Deltaquelle

```
source(output(movieId as integer,
            title as string,
            releaseDate as date,
            rated as boolean,
            screenedOn as timestamp,
            ticketPrice as decimal(10,2)
            ),
    store: 'local',
    format: 'delta',
    versionAsOf: 0,
    allowSchemaDrift: false,
    folderPath: $tempPath + '/delta'
  ) ~> movies
```

### <a name="sink-properties"></a>Senkeneigenschaften

Die folgende Tabelle enthält die von einer Deltasenke unterstützten Eigenschaften. Sie können diese Eigenschaften auf der Registerkarte **Einstellungen** bearbeiten.

| Name | Beschreibung | Erforderlich | Zulässige Werte | Datenflussskript-Eigenschaft |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Format | Das Format muss `delta` sein | ja | `delta` | format |
| Dateisystem | Der Container bzw. das Dateisystem der Delta Lake-Instanz. | ja | String | fileSystem |
| Ordnerpfad | Das Verzeichnis der Delta Lake-Instanz. | ja | String | folderPath |
| Komprimierungstyp | Der Komprimierungstyp der Deltatabelle. | nein | `bzip2`<br>`gzip`<br>`deflate`<br>`ZipDeflate`<br>`snappy`<br>`lz4` | compressionType |
| Komprimierungsgrad | Wählen Sie aus, ob die Komprimierung schnellstmöglich abgeschlossen oder die resultierende Datei optimal komprimiert werden soll. | Erforderlich, wenn `compressedType` angegeben wird. | `Optimal` oder `Fastest` | compressionLevel |
| Vakuum | Geben Sie für ältere Versionen der Tabelle den Schwellenwert für die Aufbewahrung in Stunden an. Bei einem Wert von Null oder weniger werden standardmäßig 30 Tage verwendet. | ja | Integer | vacuum |
| Updatemethode | Geben Sie an, welche Updatevorgänge für die Delta Lake-Instanz zulässig sind. Für Methoden, bei denen es sich nicht um Einfügevorgänge handelt, ist zuerst eine Zeilenänderungstransformation erforderlich, um Zeilen zu markieren. | ja | `true` oder `false` | deletable <br> insertable <br> updateable <br> upsertable |

### <a name="delta-sink-script-example"></a>Skriptbeispiel für Deltasenke

Das zugehörige Datenflussskript ist:

```
moviesAltered sink(
          input(movieId as integer,
                title as string
            ),
           mapColumn(
                movieId,
                title
            ),
           insertable: true,
           updateable: true,
           deletable: true,
           upsertable: false,
           keys: ['movieId'],
            store: 'local',
           format: 'delta',
           vacuum: 180,
           folderPath: $tempPath + '/delta'
           ) ~> movieDB
```

### <a name="known-limitations"></a>Bekannte Einschränkungen

Beim Schreiben in eine Deltasenke wird in der Überwachungsausgabe die Anzahl geschriebener Zeilen aufgrund einer bekannten Einschränkung nicht zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

* Erstellen Sie eine [Quelltransformation](data-flow-source.md) in einem Zuordnungsdatenfluss.
* Erstellen Sie eine [Senkentransformation](data-flow-sink.md) in einem Zuordnungsdatenfluss.
* Erstellen Sie eine [Zeilenänderungstransformation](data-flow-alter-row.md), um Zeilen als Einfüge-, Aktualisierungs-, Upsert- oder Löschvorgänge zu markieren.
