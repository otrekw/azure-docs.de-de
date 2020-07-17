---
title: Unterstützte Datenspeicher in Azure Data Share
description: Erfahren Sie mehr über die Datenspeicher, die zur Verwendung in Azure Data Share unterstützt werden.
ms.service: data-share
author: joannapea
ms.author: joanpo
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: 8263e2b2d3d8d029cd28f6c8ea6c556fb9fe23be
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108293"
---
# <a name="supported-data-stores-in-azure-data-share"></a>Unterstützte Datenspeicher in Azure Data Share

Azure Data Share bietet eine offene und flexible Datenfreigabe, einschließlich der Möglichkeit zur Freigabe aus und in unterschiedliche Datenspeicher. Datenanbieter können Daten aus einem Datenspeichertyp freigeben, und ihre Datenconsumer können den Datenspeicher auswählen, in dem Daten empfangen werden sollen. 

In diesem Artikel erfahren Sie mehr über die umfangreiche Gruppe von Azure-Datenspeichern, die in Azure Data Share unterstützt werden. Außerdem erhalten Sie Informationen zu den Kombinationen von Datenspeichern, die von Datenanbietern und Datenconsumern genutzt werden können. 

## <a name="what-data-stores-are-supported-in-azure-data-share"></a>Welche Datenspeicher werden in Azure Data Share unterstützt? 

In der folgenden Tabelle sind die unterstützten Datenquellen für Azure Data Share aufgeführt. 

| Datenspeicher | Momentaufnahmebasierte Freigabe | Direkte Freigabe 
|:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob Storage |✓ | |
| Azure Data Lake Storage Gen1 |✓ | |
| Azure Data Lake Storage Gen2 |✓ ||
| Azure SQL-Datenbank |Öffentliche Vorschau | |
| Azure Synapse Analytics (ehemals Azure SQL DW) |Öffentliche Vorschau | |
| Azure-Daten-Explorer | |Öffentliche Vorschau |

## <a name="data-store-support-matrix"></a>Datenspeicher-Unterstützungsmatrix

Azure Data Share bietet Datenconsumern Flexibilität bei der Entscheidung für einen Datenspeicher, in dem Daten empfangen werden. Beispielsweise können Daten, die von Azure SQL-Datenbank freigegeben werden, in Azure Data Lake Store Gen2, Azure SQL-Datenbank oder Azure Synapse Analytics empfangen werden. Kunden können beim Konfigurieren einer empfangenen Datenfreigabe das Format auswählen, in dem Daten empfangen werden sollen. 

In der folgenden Tabelle sind die verschiedenen Kombinationen und Auswahlmöglichkeiten aufgeführt, die Datenconsumern beim Akzeptieren und Konfigurieren ihrer Datenfreigabe zur Verfügung stehen. Weitere Informationen zum Konfigurieren von Datasetzuordnungen finden Sie unter [Konfigurieren von Datasetzuordnungen](how-to-configure-mapping.md).

| Datenspeicher | Azure Blob Storage | Azure Data Lake Storage Gen1 | Azure Data Lake Storage Gen2 | Azure SQL-Datenbank | Azure Synapse Analytics | Azure-Daten-Explorer
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob Storage | ✓ || ✓ ||
| Azure Data Lake Storage Gen1 | ✓ | | ✓ ||
| Azure Data Lake Storage Gen2 | ✓ | | ✓ ||
| Azure SQL-Datenbank | ✓ | | ✓ | ✓ | ✓ ||
| Azure Synapse Analytics (ehemals Azure SQL DW) | ✓ | | ✓ | ✓ | ✓ ||
| Azure-Daten-Explorer |||||| ✓ |

## <a name="share-from-a-storage-account"></a>Freigeben über ein Speicherkonto
Azure Data Share unterstützt die Freigabe von Dateien, Ordnern und Dateisystemen über Azure Data Lake Gen1 und Azure Data Lake Gen2. Außerdem unterstützt es die Freigabe von Blobs, Ordnern und Containern über Azure Blob Storage. Derzeit wird nur das Blockblob unterstützt. Wenn Dateisysteme, Container oder Ordner in einer auf Momentaufnahmen basierenden Freigabe freigegeben werden, kann der Datenconsumer wahlweise eine vollständige Kopie der Freigabedaten erstellen oder die Funktion der inkrementellen Momentaufnahme nutzen, um nur neue oder aktualisierte Dateien zu kopieren. Die inkrementelle Momentaufnahme basiert auf dem Zeitpunkt der letzten Änderung der Dateien. Vorhandene Dateien mit demselben Namen werden überschrieben.

## <a name="share-from-a-sql-based-source"></a>Freigeben über eine SQL-basierte Quelle
Azure Data Share unterstützt die Freigabe von Tabellen oder Sichten aus Azure SQL-Datenbank und Azure Synapse Analytics (ehemals „Azure SQL DW“). Der Datenconsumer kann die Daten wahlweise in Azure Data Lake Store Gen2 oder Azure Blob Storage als CSV- oder Parquet-Datei akzeptieren. Beachten Sie, dass CSV das standardmäßige Dateiformat ist. Der Datenconsumer kann die Daten bei Bedarf im Parquet-Format erhalten. Dies kann beim Empfang der Daten in den Datasetzuordnungseinstellungen erfolgen. 

Wenn Sie Daten in Azure Data Lake Store Gen2 oder Azure Blob Storage akzeptieren, überschreiben vollständige Momentaufnahmen den Inhalt der Zieldatei. 

Ein Datenconsumer kann auswählen, Daten in einer Tabelle seiner Wahl zu empfangen. Wenn die Zieltabelle in diesem Szenario nicht bereits vorhanden ist, erstellt Azure Data Share die SQL-Tabelle mit dem Quellschema. Wenn bereits eine Zieltabelle mit demselben Namen vorhanden ist, wird sie gelöscht und mit der letzten vollständigen Momentaufnahme überschrieben. Bei der Zuordnung der Zieltabelle können ein alternatives Schema und ein alternativer Tabellenname angegeben werden. Inkrementelle Momentaufnahmen werden derzeit nicht unterstützt. 

Die Freigabe von SQL-basierten Quellen umfasst Voraussetzungen für Firewallregeln und -berechtigungen. Ausführliche Informationen finden Sie im Abschnitt zu den Voraussetzungen im [Tutorial: Freigeben von Daten mithilfe von Azure Data Share](share-your-data.md).

## <a name="share-from-azure-data-explorer"></a>Freigeben über Azure Data Explorer
Azure Data Share unterstützt die Möglichkeit zur direkten Freigabe von Datenbanken über Azure Data Explorer-Cluster. Der Datenanbieter kann auf Datenbank- oder Clusterebene freigeben. Wenn der Datenconsumer auf Datenbankebene freigegeben wurde, kann er nur auf die vom Datenanbieter freigegebene(n) Datenbank(en) zugreifen. Wenn der Datenconsumer auf Clusterebene freigegeben wurde, kann er auf alle Datenbanken aus dem Cluster des Anbieters zugreifen, einschließlich zukünftiger Datenbanken, die vom Datenanbieter erstellt werden.

Für den Zugriff auf freigegebene Datenbanken benötigt der Datenconsumer seinen eigenen Azure Data Explorer-Cluster. Der Azure Data Explorer-Cluster des Datenconsumers muss sich im selben Azure-Rechenzentrum wie der Azure Data Explorer-Cluster des Datenanbieters befinden. Beim Herstellen einer Freigabebeziehung erstellt Azure Data Share eine symbolische Verknüpfung zwischen den Azure Data Explorer-Clustern des Anbieters und des Consumers.

Azure Data Explorer unterstützt zwei Modi der Datenerfassung: Batch und Streaming. Daten, die von Batch in der freigegebenen Datenbank empfangen wurden, werden auf der Datenconsumerseite innerhalb weniger Sekunden bis zu wenigen Minuten angezeigt. Bei Daten, die vom Streaming empfangen wurden, könnte es bis zu 24 Stunden dauern, bis sie auf der Datenconsumerseite angezeigt werden. 

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).
