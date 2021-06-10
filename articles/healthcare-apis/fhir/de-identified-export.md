---
title: Exportieren anonymisierter Daten (Vorschauversion) für Azure API for FHIR
description: In diesem Artikel wird beschrieben, wie Sie den anonymisierten Export einrichten und verwenden.
author: ranvijaykumar
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 9/28/2020
ms.author: ranku
ms.openlocfilehash: ba0af51eaf15211c2214f9457235afa250d92b7b
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111814443"
---
# <a name="exporting-de-identified-data-preview"></a>Exportieren anonymisierter Daten (Vorschauversion)

> [!Note] 
> Die Ergebnisse variieren bei Verwendung des anonymisierten Exports je nach Faktoren, z. B. den eingegebenen Daten oder den vom Kunden ausgewählten Funktionen. Microsoft kann die anonymisierten Exportausgaben nicht auswerten oder die Geeignetheit für Anwendungsfälle und Complianceanforderungen des Kunden bestimmen. Es ist nicht garantiert, dass der anonymisierte Export bestimmte gesetzliche, behördliche oder Complianceanforderungen erfüllt.

Der Befehl $export kann auch verwendet werden, um anonymisierte Daten vom FHIR-Server zu exportieren. Er verwendet die Anonymisierungs-Engine der [FHIR-Tools für die Anonymisierung](https://github.com/microsoft/FHIR-Tools-for-Anonymization) und nimmt Details zur Anonymisierungskonfiguration in den Abfrageparametern entgegen. Sie können eine eigene Anonymisierungskonfigurationsdatei erstellen oder die [Beispielkonfigurationsdatei](https://github.com/microsoft/FHIR-Tools-for-Anonymization#sample-configuration-file-for-hipaa-safe-harbor-method) für die HIPAA Safe Harbor-Methode als Ausgangspunkt verwenden. 

 `https://<<FHIR service base URL>>/$export?_container=<<container_name>>&_anonymizationConfig=<<config file name>>&_anonymizationConfigEtag=<<ETag on storage>>`

> [!Note] 
> Zurzeit unterstützt Azure API for FHIR den anonymisierten Export nur auf Systemebene ($export).

|Query parameter (Abfrageparameter)            | Beispiel |Optionalität| BESCHREIBUNG|
|---------------------------|---------|-----------|------------|
| _\_anonymizationConfig_   |DemoConfig.json|Erforderlich für den anonymisierten Export |Name der Konfigurationsdatei. Weitere Informationen finden Sie im Format der Konfigurationsdatei [hier](https://github.com/microsoft/FHIR-Tools-for-Anonymization#configuration-file-format). Diese Datei sollte in einem Container mit dem Namen **anonymization** in dem Azure Storage-Konto gespeichert werden, das als Exportspeicherort konfiguriert ist. |
| _\_anonymizationConfigEtag_|"0x8D8494A069489EC"|Optional für den anonymisierten Export|Dies ist das ETag der Konfigurationsdatei. Sie können das ETag mit dem Azure Storage-Explorer aus der Blobeigenschaft abrufen.|

> [!IMPORTANT]
> Sowohl der unformatierte als auch der anonymisierte Export wird in dasselbe Azure Storage-Konto geschrieben, das im Rahmen der Exportkonfiguration angegeben wurde. Es wird empfohlen, für verschiedene anonymisierte Konfigurationen unterschiedliche Container zu verwenden und den Benutzerzugriff auf Containerebene zu verwalten.
