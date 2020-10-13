---
title: Ausführen des Exports durch Aufrufen des Befehls „$export“ in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie den anonymisierten Export einrichten und verwenden.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: 433d3391e7b2b99d72ea71f1b4d48c3e04a46afc
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91819979"
---
# <a name="how-to-export-fhir-data"></a>Exportieren von FHIR-Daten

Mit der Funktion für Massenexport können Daten vom FHIR-Server gemäß der [FHIR-Spezifikation](https://hl7.org/fhir/uv/bulkdata/export/index.html) exportiert werden. 

Bevor Sie $export verwenden, müssen Sie sicherstellen, dass Azure API for FHIR für die Verwendung dieser Funktion konfiguriert ist. Informationen zum Konfigurieren von Exporteinstellungen und zum Erstellen eines Azure Storage-Kontos finden Sie auf der [Seite zum Konfigurieren von Exportdaten](configure-export-data.md).

## <a name="using-export-command"></a>Verwenden $export-Befehls

Nachdem Sie Azure API for FHIR für den Export konfiguriert haben, können Sie den $export-Befehl verwenden, um die Daten aus dem Dienst zu exportieren. Die Daten werden in dem Speicherkonto gespeichert, das Sie beim Konfigurieren des Exports angegeben haben. Informationen zum Aufrufen des Befehls $export auf dem FHIR-Server finden Sie in der Dokumentation zur [$export-Spezifikation](https://hl7.org/Fhir/uv/bulkdata/export/index.html). 

Der Befehl $export in Azure API for FHIR nimmt einen optionalen Parameter _\_container_ an, mit dem der Container im konfigurierten Speicherkonto angegeben wird, in das die Daten exportiert werden sollen. Wenn ein Container angegeben wird, werden die Daten in diesen Container in einen neuen Ordner mit dem Namen des Containers exportiert. Wenn kein Container angegeben wird, werden die Daten in einen neuen Container exportiert.

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

## <a name="supported-scenarios"></a>Unterstützte Szenarios

Azure API for FHIR unterstützt $export auf System-, Patienten- und Gruppenebene. Beim Gruppenexport werden alle zugehörigen Ressourcen exportiert, die Merkmale der Gruppe werden jedoch nicht exportiert.

> [!Note] 
> $export exportiert doppelte vorhandene Ressourcen, wenn sich die Ressource in einem Depot mehrerer Ressourcen befindet oder in mehreren Gruppen vorhanden ist.

Darüber hinaus wird die Überprüfung des Exportstatus über die URL, die vom Location-Header während der Warteschlangendauer zurückgegeben wird, sowie das Abbrechen des eigentlichen Exportauftrags unterstützt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie FHIR-Ressourcen mithilfe des Befehls $export exportieren. Nun können Sie sich mit unseren unterstützten Features vertraut machen.
 
>[!div class="nextstepaction"]
>[Unterstützte Features](fhir-features-supported.md)
