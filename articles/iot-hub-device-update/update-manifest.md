---
title: Updatemanifest für Device Update for IoT Hub | Microsoft-Dokumentation
description: Erfahren Sie, wie Eigenschaften während eines Updates vom Device Update-Dienst an das Gerät gesendet werden.
author: andrewbrownmsft
ms.author: andbrown
ms.date: 2/17/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: bbd39f7752a1a482350a7231a0bb5a3710591b1d
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102030680"
---
# <a name="device-update-for-iot-hub-update-manifest"></a>Updatemanifest für Device Update for IoT Hub

## <a name="overview"></a>Übersicht

Device Update for IoT Hub verwendet ein _Updatemanifest_, um Aktionen und auch Metadaten zu kommunizieren, die diese Aktionen über die [AzureDeviceUpdateCore.OrchestratorMetadata:4](./device-update-plug-and-play.md)-Schnittstelleneigenschaften unterstützen.
In diesem Dokument werden die Grundlagen beschrieben, wie die `updateManifest`-Eigenschaft in der Schnittstelle `AzureDeviceUpdateCore.OrchestratorMetadata:4` vom Device Update-Agent verwendet wird. Die `AzureDeviceUpdateCore.OrchestratorMetadata:4`-Schnittstelleneigenschaften werden vom Device Update for IoT Hub-Dienst an den Device Update-Agent gesendet. Das `updateManifest` ist ein serialisiertes JSON-Objekt, das vom Device Update-Agent analysiert wird.

### <a name="an-example-update-manifest"></a>Beispiel für ein Updatemanifest

```JSON
{
    "manifestVersion": "1",
    "updateId": {
        "provider": "DuTest",
        "name": "DuTestUser",
        "version": "2020.611.534.16"
    },
    "updateType": "microsoft/swupdate:1",
    "installedCriteria": "1.0",
    "files": {
        "00000": {
            "fileName": "image.swu",
            "sizeInBytes": 256000,
            "hashes": {
                "sha256": "IhIIxBJpLfazQOk/PVi6SzR7BM0jf4HDqw+6gdZ3vp8="
            }
        }
    },
    "createdDateTime": "2020-06-12T00:38:13.9350278"
}
```

Der Zweck des Updatemanifests besteht darin, den Inhalt eines Updates zu beschreiben, d. h. seine Identität, den Typ, die Installationskriterien und die Metadaten der Updatedatei. Darüber hinaus ist das Updatemanifest kryptografisch signiert, damit der Device Update-Agent dessen Echtheit überprüfen kann. Weitere Informationen zum sicheren Importieren von Inhalten mithilfe des Updatemanifests finden Sie in der [Dokumentation zur Device Update-Sicherheit](./device-update-security.md).

## <a name="import-manifest-vs-update-manifest"></a>Importmanifest und Updatemanifest

Es ist wichtig, die Unterschiede zwischen den Konzepten des Importmanifests und des Updatemanifests in Device Update for IoT Hub zu verstehen. 
* Das [Importmanifest](./import-concepts.md) wird von demjenigen erstellt, der das entsprechende Update erstellt. Es beschreibt den Inhalt des Updates, das in Device Update for IoT Hub importiert werden soll. 
* Das Updatemanifest wird automatisch vom Device Update for IoT Hub-Dienst generiert und verwendet dabei einige der Eigenschaften, die im Importmanifest definiert wurden. Es wird verwendet, um dem Device Update-Agent während des Updatevorgangs relevante Informationen zu übermitteln. 

Jeder Manifesttyp verfügt über ein eigenes Schema und eine eigene Schemaversion.

## <a name="update-manifest-properties"></a>Eigenschaften des Updatemanifests

Die allgemeinen Definitionen der Eigenschaften des Updatemanifests finden Sie in den Schnittstellendefinitionen, die Sie wiederum [hier](./device-update-plug-and-play.md) finden können. Um einen genaueren Einblick in den Kontext zu bekommen, betrachten wir die Eigenschaften und ihre Verwendung im System genauer.

### <a name="updateid"></a>updateId

Enthält `provider`, `name` und `version`, die die genaue Device Update for IoT Hub-Updateidentität darstellt, die zur Ermittlung kompatibler Geräte für das Update verwendet wird.

### <a name="updatetype"></a>updateType

Stellt den Typ des Updates dar, der von einer bestimmten Art von Updatehandler behandelt wird. Er folgt der Form von `microsoft/swupdate:1` für ein imagebasiertes Update und `microsoft/apt:1` für ein paketbasiertes Update (siehe Abschnitt `Update Handler Types` unten).

### <a name="installedcriteria"></a>installedCriteria

Eine Zeichenfolge, die Informationen enthält, die der Updatehandler des Device Update-Agents benötigt, um festzustellen, ob das Update auf dem Gerät installiert ist. Der Abschnitt `Update Handler Types` dokumentiert das Format von `installedCriteria`, für jeden von Device Update for IoT Hub unterstützten Updatetyp.

### <a name="files"></a>Dateien

Weist den Device Update-Agent an, welche Dateien heruntergeladen werden sollen, und teilt den verwendeten Hashwert mit, um zu überprüfen, ob die Dateien ordnungsgemäß heruntergeladen wurden.
Hier folgt ein genauerer Blick auf den Inhalt der Eigenschaft `files`:

```json
"files":{
        <FILE_ID_STRING>:{
            "fileName":<STRING>,
            "sizeInBytes":<INTEGER>,
            "hashes":{
                <HASH-TYPE>:<HASH-STRING>
            }
        }
    }
```

Außerhalb von `updateManifest` besteht das `fileUrls`-Array aus JSON-Objekten.

```json
"fileUrls":{
      <FILE_ID_STRING>: <URL-in-String-Format>
 }
```

Sowohl `FILE_ID_STRING` in `fileUrls` und `files` sind identisch („0000“ in `files` weist die URL bei „0000“ in `fileUrls` auf).

### <a name="manifestversion"></a>manifestVersion

Eine Zeichenfolge, die die Schemaversion darstellt.

## <a name="update-handler-types"></a>Typen von Updatehandlern

|Update-Methode|Updatehandlertyp|Updatetyp|Installierte Kriterien|Erwartete Dateien für die Veröffentlichung|
|-------------|-------------------|----------|-----------------|--------------|
|Imagebasiert|SWUpdate|„microsoft/swupdate:version“|Das Referenzimage speichert den Hinweis auf seine Version in der Datei „/etc/adu-version“.  |SWU-Datei, die das SWUpdate-Image enthält|
|Paketbasiert|APT|„microsoft/apt:version“|`<name>` + "-" + `<version>` (definierte Eigenschaften in der APT-Manifestdatei|`<APT Update Manifest>`.json, die die APT-Konfiguration und die Paketliste enthält|

