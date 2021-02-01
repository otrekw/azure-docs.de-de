---
title: Ausführen des Exports durch Aufrufen des Befehls „$export“ in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie FHIR-Daten mithilfe von $export exportiert werden.
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 1/21/2021
ms.author: cavoeg
ms.openlocfilehash: 48dbd0892c9ec02f203edba55d1104f1ab0118a8
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98737607"
---
# <a name="how-to-export-fhir-data"></a>Exportieren von FHIR-Daten


Mit der Funktion für Massenexport können Daten vom FHIR-Server gemäß der [FHIR-Spezifikation](https://hl7.org/fhir/uv/bulkdata/export/index.html) exportiert werden. 

Bevor Sie $export verwenden, müssen Sie sicherstellen, dass Azure API for FHIR für die Verwendung dieser Funktion konfiguriert ist. Informationen zum Konfigurieren von Exporteinstellungen und zum Erstellen eines Azure Storage-Kontos finden Sie auf der [Seite zum Konfigurieren von Exportdaten](configure-export-data.md).

## <a name="using-export-command"></a>Verwenden $export-Befehls

Nachdem Sie Azure API for FHIR für den Export konfiguriert haben, können Sie den $export-Befehl verwenden, um die Daten aus dem Dienst zu exportieren. Die Daten werden in dem Speicherkonto gespeichert, das Sie beim Konfigurieren des Exports angegeben haben. Informationen zum Aufrufen des Befehls $export auf dem FHIR-Server finden Sie in der Dokumentation zur [$export-Spezifikation von HL7 FHIR](https://hl7.org/Fhir/uv/bulkdata/export/index.html). 

Die Azure API for FHIR unterstützt $export auf folgenden Ebenen:
* [System:](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---system-level-export) `GET https://<<FHIR service base URL>>/$export>>`
* [Patient:](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---all-patients) `GET https://<<FHIR service base URL>>/Patient/$export>>`
* [Gruppe von Patienten*:](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---group-of-patients) Azure API for FHIR exportiert alle zugehörigen Ressourcen, aber nicht die Eigenschaften der Gruppe: `GET https://<<FHIR service base URL>>/Group/[ID]/$export>>`



> [!Note] 
> `Patient/$export` und `Group/[ID]/$export` exportieren möglicherweise doppelt vorhandene Ressourcen, wenn sich die Ressource in einem Depot mehrerer Ressourcen befindet oder in mehreren Gruppen vorhanden ist.

Darüber hinaus wird die Überprüfung des Exportstatus über die URL, die von der Standortkopfzeile während der Warteschlangendauer zurückgegeben wird, sowie das Abbrechen des eigentlichen Exportauftrags unterstützt.

## <a name="settings-and-parameters"></a>Einstellungen und Parameter

### <a name="headers"></a>Header
Es gibt zwei erforderliche Headerparameter, die für $export-Aufträge festgelegt werden müssen. Die Werte werden durch die aktuelle [$export-Spezifikation](https://hl7.org/Fhir/uv/bulkdata/export/index.html#headers) definiert.
* **Accept:** application/fhir+json
* **Prefer:** respond-async

### <a name="query-parameters"></a>Abfrageparameter
Die Azure API for FHIR unterstützt die folgenden Abfrageparameter, die alle optional sind:
|Query parameter (Abfrageparameter)        | Definiert durch die FHIR-Spezifikation?    |  Beschreibung|
|------------------------|---|------------|
| \_outputFormat | Ja | Unterstützt derzeit drei Werte gemäß FHIR-Spezifikation: „application/fhir+ndjson“, „application/ndjson“ oder nur „ndjson“. Alle Exportaufträge geben `ndjson` zurück, und der übergebene Wert hat keine Auswirkung auf das Codeverhalten. |
| \_since | Ja | Ermöglicht nur das Exportieren von Ressourcen, die seit dem angegebenen Zeitpunkt geändert wurden |
| \_type | Ja | Ermöglicht die Angabe, welche Ressourcentypen eingeschlossen werden sollen. Beispielsweise gibt „\_type=Patient“ nur Patientenressourcen zurück.|
| \_typeFilter | Ja | Um eine differenziertere Filterung anzufordern, können Sie „\_typeFilter“ zusammen mit dem Parameter „\_type“ verwenden. Der Wert des Parameters „_typeFilter“ ist eine durch Trennzeichen getrennte Liste von FHIR-Abfragen, mit denen die Ergebnisse weiter eingegrenzt werden. |
| \_container | Nein |  Gibt den Container innerhalb des konfigurierten Speicherkontos an, in den die Daten exportiert werden sollen. Wenn ein Container angegeben wird, werden die Daten in diesen Container in einen neuen Ordner mit entsprechendem Namen exportiert. Wenn kein Container angegeben wird, erfolgt der Export in einen neuen Container mit dem Zeitstempel und der Auftrags-ID als Namen. |


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie FHIR-Ressourcen mithilfe des Befehls $export exportieren. Als Nächstes erfahren Sie, wie Sie anonymisierte Daten exportieren:
 
>[!div class="nextstepaction"]
>[Exportieren anonymisierter Daten](de-identified-export.md)
