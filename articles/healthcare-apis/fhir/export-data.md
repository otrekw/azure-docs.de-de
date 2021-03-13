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
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103019154"
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

### <a name="exporting-fhir-data-to-adls-gen2"></a>Exportieren von fhir-Daten in ADLS Gen2

Zurzeit unterstützen wir $Export für ADLS Gen2 aktivierte Speicher Konten mit der folgenden Einschränkung:

- Der Benutzer kann noch keine [hierarchischen Namespaces](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-namespace) nutzen. Es gibt keine Möglichkeit, auf den Export in ein bestimmtes Unterverzeichnis innerhalb des Containers zu Zielen. Wir bieten nur die Möglichkeit, einen bestimmten Container als Ziel festzustellen (in dem wir für jeden Export einen neuen Ordner erstellen).

- Nachdem ein Export Vorgang durchgeführt wurde, exportieren wir nichts mehr in diesen Ordner, da nachfolgende Exporte in denselben Container in einem neu erstellten Ordner gespeichert werden.


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

## <a name="secure-export-to-azure-storage"></a>Sicherer Export nach Azure Storage

Die Azure-API für den Betrieb von Azure unterstützt einen sicheren Export Vorgang. Eine Möglichkeit zum Ausführen eines sicheren Exports besteht darin, bestimmte IP-Adressen zuzulassen, die der Azure-API für fhir zugeordnet sind, um auf das Azure Storage-Konto zuzugreifen. Abhängig davon, ob sich das Speicherkonto am selben Standort oder an einem anderen Speicherort als der Azure-API für den Bhir befindet, unterscheiden sich die Konfigurationen.

### <a name="when-the-azure-storage-account-is-in-a-different-region"></a>Wenn sich das Azure-Speicherkonto in einer anderen Region befindet

Wählen Sie im Portal das Blatt "Netzwerk" des Azure-Speicher Kontos aus. 

   :::image type="content" source="media/export-data/storage-networking.png" alt-text="Azure Storage Netzwerkeinstellungen." lightbox="media/export-data/storage-networking.png":::
   
Wählen Sie "ausgewählte Netzwerke" aus, und geben Sie die IP-Adresse im Feld **Adressbereich** im Abschnitt der Firewall \| IP-Bereiche hinzufügen an, um den Zugriff aus dem Internet oder Ihren lokalen Netzwerken zuzulassen. Die IP-Adresse in der folgenden Tabelle finden Sie in der Azure-Region, in der die Azure-API für den fhir-Dienst bereitgestellt wird.

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
| Europa, Norden         | 52.146.131.52     |
| Südafrika, Norden   | 102.133.220.197   |
| USA Süd Mitte     | 13.73.254.220     |
| Asien, Südosten       | 23.98.108.42      |
| Schweiz, Norden    | 51.107.60.95      |
| UK, Süden             | 51.104.30.170     |
| UK, Westen              | 51.137.164.94     |
| USA, Westen-Mitte      | 52.150.156.44     |
| Europa, Westen          | 20.61.98.66       |
| USA, Westen 2            | 40.64.135.77      |

### <a name="when-the-azure-storage-account-is-in-the-same-region"></a>Wenn sich das Azure-Speicherkonto in derselben Region befindet

Der Konfigurationsprozess ist mit dem oben genannten identisch, mit dem Unterschied, dass stattdessen ein bestimmter IP-Adressbereich im CIDR-Format verwendet wird, 100.64.0.0/10. Der Grund für den IP-Adressbereich, der 100.64.0.0 – 100.127.255.255 enthält, muss angegeben werden, da die tatsächliche vom Dienst verwendete IP-Adresse variiert, jedoch für jede $Export Anforderung innerhalb des Bereichs liegt.

> [!Note] 
> Möglicherweise wird stattdessen eine private IP-Adresse im Bereich von 10.0.2.0/24 verwendet. In diesem Fall wird der $Export Vorgang nicht erfolgreich ausgeführt. Sie können die $Export Anforderung wiederholen, aber es gibt keine Garantie dafür, dass die IP-Adresse im Bereich von 100.64.0.0/10 das nächste Mal verwendet wird. Das ist das bekannte Netzwerk Verhalten. Die Alternative besteht darin, das Speicherkonto in einer anderen Region zu konfigurieren.
    
## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie FHIR-Ressourcen mithilfe des Befehls $export exportieren. Als Nächstes erfahren Sie, wie Sie anonymisierte Daten exportieren:
 
>[!div class="nextstepaction"]
>[Exportieren anonymisierter Daten](de-identified-export.md)
