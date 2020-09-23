---
title: Tabellenspeicherausgabe für Azure Stream Analytics
description: In diesem Artikel wird Azure-Tabellenspeicher als Ausgabe für Azure Stream Analytics beschrieben.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 3e5b747e55c8f246167bcf8bbde3542146e69706
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90891950"
---
# <a name="table-storage-output-from-azure-stream-analytics"></a>Tabellenspeicherausgabe für Azure Stream Analytics

[Azure-Tabellenspeicher](../storage/common/storage-introduction.md) bietet einen hoch verfügbaren, in hohem Maße skalierbaren Speicher, sodass eine Anwendung automatisch an die Bedürfnisse der Benutzer angepasst werden kann. Tabellenspeicher ist der NoSQL-Schlüssel-/Attributspeicher von Microsoft, der für strukturierte Daten genutzt werden kann, die weniger Einschränkungen hinsichtlich des Schemas aufweisen. Azure-Tabellenspeicher kann zum Speichern von Daten für dauerhafte Archivierung und effizienten Abruf verwendet werden.

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Tabellenausgabe.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diesen Tabellenspeicher weiterzuleiten. |
| Speicherkonto |Der Name des Speicherkontos, an das Sie die Ausgabe senden. |
| Speicherkontoschlüssel |Der Zugriffsschlüssel, der dem Speicherkonto zugeordnet ist. |
| Tabellenname |Der Name der Tabelle. Die Tabelle wird erstellt, wenn sie nicht vorhanden ist. |
| Partitionsschlüssel |Der Name der Ausgabespalte, die den Partitionsschlüssel enthält. Der Partitionsschlüssel ist ein eindeutiger Bezeichner für die Partition innerhalb einer Tabelle, die den ersten Teil des Primärschlüssels einer Entität bildet. Dabei handelt es sich um einen Zeichenfolgenwert, der bis zu 1 KB groß sein kann. |
| Zeilenschlüssel |Der Name der Ausgabespalte, die den Zeilenschlüssel enthält. Der Zeilenschlüssel ist ein eindeutiger Bezeichner für eine Entität innerhalb einer Partition. Er bildet den zweiten Teil des Primärschlüssels einer Entität. Der Zeilenschlüssel ist ein Zeichenfolgenwert, der bis zu 1 KB groß sein kann. |
| Batchgröße |Dies ist die Anzahl von Datensätzen für einen Batchvorgang. Der Standardwert (100) ist für die meisten Aufträge ausreichend. Weitere Details zur Änderung dieser Einstellung finden Sie in der [TableBatchOperation-Spezifikation](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table.tablebatchoperation). |

## <a name="partitioning"></a>Partitionierung

Der Partitionsschlüssel ist eine beliebige Ausgabespalte. Die Anzahl der Ausgabeschreiber folgt der Eingabepartitionierung für [vollständig parallelisierte Abfragen](stream-analytics-scale-jobs.md).

## <a name="output-batch-size"></a>Ausgabebatchgröße

Die maximale Nachrichtengröße finden Sie unter [Grenzwerte für Azure Storage](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits). Der Standardwert beträgt 100 Entitäten pro Einzeltransaktion, aber Sie können bei Bedarf einen niedrigeren Wert konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mit der Azure CLI](quick-create-azure-cli.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe einer ARM-Vorlage](quick-create-azure-resource-manager.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mit Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio Code](quick-create-visual-studio-code.md)
