---
title: Ausführen des Exports durch Aufrufen des Befehls „$export“ in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie FHIR-Daten mithilfe von $export exportiert werden.
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 2/19/2021
ms.author: cavoeg
ms.openlocfilehash: 9ed78baed35312b9a33c71a3e49b7e9dca22eb9f
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487218"
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

Beim Exportieren von Daten wird eine separate Datei für jeden Ressourcentyp erstellt. Um sicherzustellen, dass die exportierten Dateien nicht zu groß werden, wird eine neue Datei erstellt, wenn die Größe einer einzelnen exportierten Datei 64 MB überschreitet. Infolgedessen erhalten Sie für jeden Ressourcentyp möglicherweise mehrere Dateien, die durchnummeriert werden (d. h. Patient-1.ndjson, Patient-2.ndjson). 


> [!Note] 
> `Patient/$export` und `Group/[ID]/$export` exportieren möglicherweise doppelt vorhandene Ressourcen, wenn sich die Ressource in einem Depot mehrerer Ressourcen befindet oder in mehreren Gruppen vorhanden ist.

Darüber hinaus wird die Überprüfung des Exportstatus über die URL, die von der Standortkopfzeile während der Warteschlangendauer zurückgegeben wird, sowie das Abbrechen des eigentlichen Exportauftrags unterstützt.

### <a name="exporting-fhir-data-to-adls-gen2"></a>Exportieren von FHIR-Daten nach ADLS Gen2

Derzeit wird „$export“ für ADLS Gen2-fähige Speicherkonten unterstützt. Hierbei gilt aber die folgende Einschränkung:

- Benutzer können noch keine [hierarchischen Namespaces](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) verwenden. Es gibt keine Möglichkeit, den zielgerichteten Export in ein bestimmtes Unterverzeichnis des Containers durchzuführen. Es kann nur ein bestimmter Container angegeben werden (wobei für jeden Export ein neuer Ordner erstellt wird).

- Nach Abschluss eines Exportvorgangs werden nicht noch einmal Daten in diesen Ordner exportiert, da für die nachfolgenden Exporte in denselben Container dann ein neu erstellter Ordner genutzt wird.


## <a name="settings-and-parameters"></a>Einstellungen und Parameter

### <a name="headers"></a>Header
Es gibt zwei erforderliche Headerparameter, die für $export-Aufträge festgelegt werden müssen. Die Werte werden durch die aktuelle [$export-Spezifikation](https://hl7.org/Fhir/uv/bulkdata/export/index.html#headers) definiert.
* **Accept:** application/fhir+json
* **Prefer:** respond-async

### <a name="query-parameters"></a>Abfrageparameter
Die Azure API for FHIR unterstützt die folgenden Abfrageparameter, die alle optional sind:

|Query parameter (Abfrageparameter)        | Definiert durch die FHIR-Spezifikation?    |  BESCHREIBUNG|
|------------------------|---|------------|
| \_outputFormat | Ja | Unterstützt derzeit drei Werte gemäß FHIR-Spezifikation: „application/fhir+ndjson“, „application/ndjson“ oder nur „ndjson“. Alle Exportaufträge geben `ndjson` zurück, und der übergebene Wert hat keine Auswirkung auf das Codeverhalten. |
| \_since | Ja | Ermöglicht nur das Exportieren von Ressourcen, die seit dem angegebenen Zeitpunkt geändert wurden |
| \_type | Ja | Ermöglicht die Angabe, welche Ressourcentypen eingeschlossen werden sollen. Beispielsweise gibt „\_type=Patient“ nur Patientenressourcen zurück.|
| \_typeFilter | Ja | Um eine differenziertere Filterung anzufordern, können Sie „\_typeFilter“ zusammen mit dem Parameter „\_type“ verwenden. Der Wert des Parameters „_typeFilter“ ist eine durch Trennzeichen getrennte Liste von FHIR-Abfragen, mit denen die Ergebnisse weiter eingegrenzt werden. |
| \_container | Nein |  Gibt den Container innerhalb des konfigurierten Speicherkontos an, in den die Daten exportiert werden sollen. Wenn ein Container angegeben wird, werden die Daten in diesen Container in einen neuen Ordner mit entsprechendem Namen exportiert. Wenn kein Container angegeben wird, erfolgt der Export in einen neuen Container mit dem Zeitstempel und der Auftrags-ID als Namen. |

## <a name="secure-export-to-azure-storage"></a>Sicherer Export in Azure Storage

Für Azure API for FHIR wird ein sicherer Exportvorgang unterstützt. Eine Möglichkeit zum Durchführen eines sicheren Exports besteht darin, für bestimmte IP-Adressen, die Azure API for FHIR zugeordnet sind, den Zugriff auf das Azure-Speicherkonto zuzulassen. Die Konfigurationen richten sich jeweils danach, ob sich das Speicherkonto an demselben Ort wie Azure API for FHIR oder einem anderen Ort befindet.

### <a name="when-the-azure-storage-account-is-in-a-different-region"></a>Azure-Speicherkonto in einer anderen Region

Wählen Sie im Portal das Blatt „Netzwerk“ für das Azure-Speicherkonto aus. 

   :::image type="content" source="media/export-data/storage-networking.png" alt-text="Netzwerkeinstellungen für Azure-Speicher" lightbox="media/export-data/storage-networking.png":::
   
Wählen Sie die Option „Ausgewählte Netzwerke“ aus, und geben Sie die IP-Adresse im Feld **Adressbereich** unter dem Abschnitt „Firewall“ \| „Add IP ranges to allow access from the internet or your on-premises networks“ (IP-Adressbereiche hinzufügen, um Zugriff aus dem Internet oder Ihren lokalen Netzwerken zuzulassen) an. Die IP-Adresse für die Azure-Region, in der der Dienst Azure API for FHIR bereitgestellt wird, können Sie der Tabelle unten entnehmen.

|**Azure-Region**         |**Öffentliche IP-Adresse** |
|:----------------------|:-------------------|
| Australien (Osten)       | 20.53.44.80       |
| Kanada, Mitte       | 20.48.192.84      |
| USA (Mitte)           | 52.182.208.31     |
| East US              | 20.62.128.148     |
| USA (Ost) 2            | 20.49.102.228     |
| USA, Osten 2 (EUAP)       | 20.39.26.254      |
| Deutschland, Norden        | 51.116.51.33      |
| Deutschland, Westen-Mitte | 51.116.146.216    |
| Japan, Osten           | 20.191.160.26     |
| Korea, Mitte        | 20.41.69.51       |
| USA Nord Mitte     | 20.49.114.188     |
| Nordeuropa         | 52.146.131.52     |
| Südafrika, Norden   | 102.133.220.197   |
| USA Süd Mitte     | 13.73.254.220     |
| Asien, Südosten       | 23.98.108.42      |
| Schweiz, Norden    | 51.107.60.95      |
| UK, Süden             | 51.104.30.170     |
| UK, Westen              | 51.137.164.94     |
| USA, Westen-Mitte      | 52.150.156.44     |
| Europa, Westen          | 20.61.98.66       |
| USA, Westen 2            | 40.64.135.77      |

### <a name="when-the-azure-storage-account-is-in-the-same-region"></a>Azure-Speicherkonto in derselben Region

Der Konfigurationsprozess entspricht dem obigen Prozess, aber mit der Ausnahme, dass ein bestimmter IP-Adressbereich im CIDR-Format verwendet wird: 100.64.0.0/10. Der IP-Adressbereich (100.64.0.0 bis 100.127.255.255) muss angegeben werden, da die tatsächlich vom Dienst genutzte IP-Adresse variiert. Sie liegt für jede $export-Anforderung aber immer in diesem Bereich.

> [!Note] 
> Es ist möglich, dass stattdessen eine private IP-Adresse im Bereich 10.0.2.0/24 genutzt wird. In diesem Fall ist der $export-Vorgang nicht erfolgreich. Sie können versuchen, die $export-Anforderung erneut zu senden. Es ist aber nicht garantiert, dass beim nächsten Mal eine IP-Adresse aus dem Bereich 100.64.0.0/10 verwendet wird. Dies ist das bekannte standardmäßige Netzwerkverhalten. Die Alternative besteht darin, das Speicherkonto in einer anderen Region zu konfigurieren.
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie FHIR-Ressourcen mithilfe des Befehls $export exportieren. Als Nächstes erfahren Sie, wie Sie anonymisierte Daten exportieren:
 
>[!div class="nextstepaction"]
>[Exportieren anonymisierter Daten](de-identified-export.md)
