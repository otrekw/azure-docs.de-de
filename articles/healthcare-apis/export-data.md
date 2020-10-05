---
title: Ausführen des Exports durch Aufrufen des Befehls „$export“ in Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie den anonymisierten Export einrichten und verwenden.
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/26/2020
ms.author: matjazl
ms.openlocfilehash: 83509b5f452ab7cf88774561c12d7aa2cf3b46cf
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482316"
---
# <a name="how-to-export-fhir-data"></a>Exportieren von FHIR-Daten

Informationen zum Konfigurieren von Exporteinstellungen und zum Erstellen eines Azure Storage-Kontos finden Sie [hier](configure-export-data.md).

## <a name="exporting-fhir-resources-using-export-command"></a>Exportieren von FHIR-Ressourcen mit dem Befehl $export

Nachdem Sie Azure API for FHIR für den Export konfiguriert haben, können Sie nun den Befehl $export verwenden, um die Daten aus dem Dienst in das beim Konfigurieren des Exports angegebene Speicherkonto zu exportieren. Informationen zum Aufrufen des Befehls $export auf dem FHIR-Server finden Sie in der Dokumentation zur Spezifikation von $export unter [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html). 

Der Befehl $export in Azure API for FHIR verwendet einen optionalen _\_container_-Parameter, mit dem der Container in dem konfigurierten Speicherkonto, in das die Daten exportiert werden sollen, angegeben werden kann.

`https://<<FHIR service base URL>>/$export?_container=<<container_name>>`

> [!IMPORTANT]
> Beachten Sie, dass Azure API for FHIR derzeit nur den Export auf Systemebene unterstützt, wie es in der Spezifikation von &export unter [https://hl7.org/Fhir/uv/bulkdata/export/index.html](https://hl7.org/Fhir/uv/bulkdata/export/index.html) definiert ist. Außerdem wird derzeit nur der Abfrageparameter _\_since_ unterstützt.

## <a name="exporting-de-identified-data-preview"></a>Exportieren anonymisierter Daten (Vorschauversion)

Der Befehl $export kann auch verwendet werden, um anonymisierte Daten vom FHIR-Server zu exportieren. Er verwendet die Anonymisierungs-Engine der [FHIR-Tools für die Anonymisierung](https://github.com/microsoft/FHIR-Tools-for-Anonymization) und nimmt Details zur Anonymisierungskonfiguration in den Abfrageparametern entgegen. Sie können eine eigene Anonymisierungskonfigurationsdatei erstellen oder die [Beispielkonfigurationsdatei](https://github.com/microsoft/FHIR-Tools-for-Anonymization#sample-configuration-file-for-hipaa-safe-harbor-method) für die HIPAA Safe Harbor-Methode als Ausgangspunkt verwenden. 

 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

|Query parameter (Abfrageparameter)            | Beispiel |Optionalität| BESCHREIBUNG|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |DemoConfig.json|Erforderlich für den anonymisierten Export |Name der Konfigurationsdatei. Weitere Informationen finden Sie im Format der Konfigurationsdatei [hier](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format). Diese Datei sollte in einem Container mit dem Namen **anonymization** in dem Azure Storage-Konto gespeichert werden, das als Exportspeicherort konfiguriert ist. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Optional für den anonymisierten Export|Dies ist das ETag der Konfigurationsdatei. Sie können das ETag mit dem Azure Storage-Explorer aus der Blobeigenschaft abrufen.|

> [!IMPORTANT]
> Beachten Sie, dass sowohl der unformatierte als auch der anonymisierte Export in das gleiche Azure Storage-Konto geschrieben wird, das im Rahmen der Exportkonfiguration angegeben wurde. Es wird empfohlen, für verschiedene anonymisierte Konfigurationen unterschiedliche Container zu verwenden und den Benutzerzugriff auf Containerebene zu verwalten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie FHIR-Ressourcen mithilfe des Befehls „$export“ exportieren (einschließlich anonymisierte Daten). Als Nächstes können Sie Ihre Exportdaten konfigurieren:
 
>[!div class="nextstepaction"]
>[Konfigurieren von Exportdaten](configure-export-data.md)
