---
title: Unterstützte Datenquellen und Dateitypen
description: Dieser Artikel enthält konzeptionelle Details zu den Datenquellen und Dateitypen, die in Purview unterstützt werden.
author: viseshag
ms.author: viseshag
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/24/2020
ms.custom: references_regions
ms.openlocfilehash: 15ade6fca3885bfabba7a23e2c3d8e561a9e6a0c
ms.sourcegitcommit: eda26a142f1d3b5a9253176e16b5cbaefe3e31b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109738462"
---
# <a name="supported-data-sources-and-file-types-in-azure-purview"></a>Unterstützte Datenquellen und Dateitypen in Azure Purview

In diesem Artikel sind die Datenquellen, Dateitypen und Überprüfungskonzepte erläutert, die in Purview unterstützt werden.

## <a name="supported-data-sources"></a>Unterstützte Datenquellen

Purview unterstützt alle [hier aufgeführten Datenquellen](purview-connector-overview.md).

## <a name="file-types-supported-for-scanning"></a>Dateitypen, die für Überprüfungen unterstützt werden

Die folgenden Dateitypen werden für Überprüfungen, Schemaextraktion und Klassifizierung unterstützt, wo dies zutrifft:

- Dateierweiterungen der unterstützten strukturierten Dateiformate: AVRO, ORC, PARQUET, CSV, JSON, PSV, SSV, TSV, TXT, XML, GZIP
- Dateierweiterungen der unterstützten Dokumentdateiformate: DOC, DOCM, DOCX, DOT, ODP, ODS, ODT, PDF, POT, PPS, PPSX, PPT, PPTM, PPTX, XLC, XLS, XLSB, XLSM, XLSX, XLT
- Purview unterstützt auch benutzerdefinierte Dateierweiterungen und benutzerdefinierte Parser.
 
> [!Note]
> Jede GZIP-Datei muss einer einzelnen enthaltenen CSV-Datei zugeordnet werden. GZIP-Dateien unterliegen System- und benutzerdefinierten Klassifizierungsregeln. Das Scannen einer GZIP-Datei, die mehreren enthaltenen Dateien oder einem anderen Dateityp als CSV zugeordnet ist, wird derzeit nicht unterstützt. Darüber hinaus unterstützt der Purview-Scanner das Scannen von mit Snappy komprimierten PARQUET- und AVRO-Dateitypen für die Schemaextraktion und -klassifizierung.

> [!Note]
> Der Purview-Scanner unterstützt keine komplexen Datentypen in AVRO-, ORC- und PARQUET-Dateitypen für die Schemaextraktion.   

## <a name="sampling-within-a-file"></a>Sampling (Überprüfung) innerhalb einer Datei

In der Purview-Terminologie:
- L1-Überprüfung (Scan): Extrahiert grundlegende Informationen und Metadaten wie Dateiname, Dateigröße und vollqualifizierter Name
- L2-Überprüfung: Extrahiert das Schema für strukturierte Dateitypen und Datenbanktabellen
- L3-Überprüfung: Extrahiert das Schema, sofern verfügbar, und wendet System- und benutzerdefinierte Klassifizierungsregeln auf die überprüfte Datei an

Bei allen strukturierten Dateiformaten werden Dateien vom Purview-Scanner auf folgende Weise überprüft:

- Bei strukturierten Dateitypen überprüft der Scanner 128 Zeilen in jeder Spalte oder 1 MB, je nachdem, welcher Wert kleiner ist.
- Bei Dokumentdateiformaten überprüft der Scanner 20 MB von jeder Datei.
    - Ist eine Dokumentdatei größer als 20 MB, wird sie keiner tiefen Überprüfung unterzogen (sie wird nicht klassifiziert). In diesem Fall erfasst Purview nur grundlegende Metadaten, z. B. Dateiname und vollqualifizierter Name.

## <a name="resource-set-file-sampling"></a>Ressourcensatzüberprüfung

Ein Ordner oder eine Gruppe von Partitionsdateien wird in Purview als *Ressourcensatz* erkannt, wenn der Ordner oder die Gruppe mit einer Systemressourcensatzrichtlinie oder einer benutzerdefinierten Ressourcensatzrichtlinie übereinstimmt. Wird ein Ressourcensatz erkannt, überprüft Purview jeden darin enthaltenen Ordner. Weitere Informationen zu Ressourcensätzen finden Sie [hier](concept-resource-sets.md).

Dateiüberprüfung für Ressourcensätze nach Dateitypen:

- **Dateien mit Trennzeichen (CSV, PSV, SSV, TSV)** : 1 aus 100 Dateien wird in einem Ordner oder einer Gruppe von Partitionsdateien, die als „ Ressourcensatz“' angesehen werden, überprüft (L3-Überprüfung).
- **Data Lake-Dateitypen (Parquet, Avro, Orc)** : 1 aus 18446744073709551615 (long max) Dateien wird in einem Ordner oder einer Gruppe von Partitionsdateien, die als *Ressourcensatz* angesehen werden, überprüft (L3-Überprüfung).
- **Andere strukturierte Dateitypen (JSON, XML, TXT)** : 1 aus 100 Dateien wird in einem Ordner oder einer Gruppe von Partitionsdateien, die als „ Ressourcensatz“' angesehen werden, überprüft (L3-Überprüfung).
- **SQL-Objekte und CosmosDB-Entitäten**: Jede Datei wird einer L3-Überprüfung unterzogen.
- **Dokumentdateitypen**: Jede Datei wird einer L3-Überprüfung unterzogen. Ressourcensatzmuster gelten für diese Dateitypen nicht.

## <a name="classification"></a>Klassifizierung

Alle 105 Systemklassifizierungsregeln gelten für strukturierte Dateiformate. Nur die MCE-Klassifizierungsregeln gelten für Dokumentdateitypen (nicht die nativen RegEx-Muster der Datenüberprüfung, sondern Bloomfilter-basierte Erkennung). Weitere Informationen zu den unterstützten Klassifizierungen finden Sie unter [Unterstützte Klassifizierungen in Azure Purview](supported-classifications.md).

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Scannen von Daten mit Azure Purview (Vorschau)](tutorial-scan-data.md)
- [Verwalten von Datenquellen in Azure Purview (Vorschau)](manage-data-sources.md).