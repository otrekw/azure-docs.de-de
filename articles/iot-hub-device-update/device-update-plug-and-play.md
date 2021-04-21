---
title: Grundlegendes zur Verwendung von IoT Plug & Play durch Device Update for IoT Hub | Microsoft-Dokumentation
description: Device Update for IoT Hub verwendet IoT Plug & Play zum Erkennen und Verwalten von Geräten, die OTA-Updates (Over-the-Air) empfangen können.
author: ValOlson
ms.author: valls
ms.date: 2/14/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: fbc3502952e11830ef9abb06cb709fcc60288343
ms.sourcegitcommit: 425420fe14cf5265d3e7ff31d596be62542837fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107739528"
---
# <a name="device-update-for-iot-hub-and-iot-plug-and-play"></a>Device Update for IoT Hub und IoT Plug & Play

Device Update for IoT Hub verwendet [IoT Plug & Play](../iot-pnp/index.yml) zum Erkennen und Verwalten von Geräten, die OTA-Updates (Over-the-Air) empfangen können. Der Device Update-Dienst sendet Eigenschaften und Nachrichten an Geräte und empfängt diese von Geräten, die PnP-Schnittstellen verwenden. Device Update for IoT Hub erfordert, dass IoT-Geräte die folgenden Schnittstellen und die model-id wie unten beschrieben implementieren.

## <a name="adu-core-interface"></a>ADU Core-Schnittstelle

Die Schnittstelle „ADUCoreInterface“ wird verwendet, um Updateaktionen und Metadaten an Geräte zu senden und den Updatestatus von Geräten zu empfangen. Die Schnittstelle „ADU Core“ ist in zwei Objekteigenschaften aufgeteilt.

Der erwartete Komponentenname im Modell ist **azureDeviceUpdateAgent**, wenn diese Schnittstelle implementiert wird. Weitere Informationen zu [Azure IoT PnP-Komponenten](../iot-pnp/concepts-components.md)

### <a name="agent-metadata"></a>Agent-Metadaten

Agent-Metadaten enthalten Felder, die vom Gerät oder Device Update-Agent zum Senden von Informationen und des Status an Device Update-Dienste verwendet werden.

|Name|Schema|Direction|BESCHREIBUNG|Beispiel|
|----|------|---------|-----------|-----------|
|resultCode|integer|Gerät an Cloud|Ein Code, der Informationen zum Ergebnis der letzten Aktualisierungsaktion enthält. Kann bei Erfolg oder Fehler mit Daten aufgefüllt werden und sollte der [HTTP-Statuscodespezifikation](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) genügen.|500|
|extendedResultCode|integer|Gerät an Cloud|Ein Code, der zusätzliche Informationen zum Ergebnis enthält. Kann bei Erfolg oder Fehler mit Daten aufgefüllt werden.|0x80004005|
|state|integer|Gerät an Cloud|Dabei handelt es sich um einen Integerwert, der den aktuellen Status des Device Update-Agents angibt. Weitere Informationen siehe unten. |Idle|
|installedUpdateId|Zeichenfolge|Gerät an Cloud|Eine ID des Updates, das zurzeit (über Device Update) installiert wird. Dieser Wert ist NULL für ein Gerät, das noch nie ein Update über Device Update erhalten hat.|Null|
|`deviceProperties`|Karte|Gerät an Cloud|Der Satz von Eigenschaften, die den Hersteller und das Modell enthalten.|Weitere Informationen siehe unten.

#### <a name="state"></a>State

Es handelt sich um den Status, der vom Device Update-Agent gemeldet wird, nachdem eine Aktion vom Device Update-Dienst empfangen wurde. `State` wird als Antwort auf eine gemeldet `Action` (siehe `Actions` unten), die über den Device Update-Dienst an den Device Update-Agent gesendet wird. Weitere Informationen zu Anforderungen, die zwischen dem Device Update-Dienst und dem Device Update-Agent fließen, finden Sie in der [Workflowübersicht](understand-device-update.md#device-update-agent).

|Name|Wert|BESCHREIBUNG|
|---------|-----|-----------|
|Idle|0|Das Gerät ist bereit, eine Aktion vom Device Update-Dienst zu empfangen. Nach einem erfolgreichen Update wird der Status in den Status `Idle` zurückgesetzt.|
|DownloadSucceeded|2|Ein erfolgreicher Download.|
|InstallSucceeded|4|Eine erfolgreiche Installation.|
|Fehler|255|Fehler beim Aktualisieren.|

#### <a name="device-properties"></a>Geräteeigenschaften

Es handelt sich um den Satz der Eigenschaften, die den Hersteller und das Modell enthalten.

|Name|Schema|Direction|BESCHREIBUNG|
|----|------|---------|-----------|
|Hersteller|Zeichenfolge|Gerät an Cloud|Der Gerätehersteller, der über `deviceProperties` gemeldet wird. Diese Eigenschaft wird aus einem von zwei Speicherorten gelesen. Die Schnittstelle AzureDeviceUpdateCore versucht zunächst, den Wert aduc_manufacturer aus der [Konfigurationsdatei](device-update-configuration-file.md) zu lesen.  Wenn der Wert in der Konfigurationsdatei keine Daten enthält, wird standardmäßig die Kompilierzeitdefinition für ADUC_DEVICEPROPERTIES_MANUFACTURER gemeldet. Diese Eigenschaft wird nur zur Startzeit gemeldet.|
|model|Zeichenfolge|Gerät an Cloud|Das Gerätemodell des Geräts, das über gemeldet wird `deviceProperties`. Diese Eigenschaft wird aus einem von zwei Speicherorten gelesen. Die Schnittstelle AzureDeviceUpdateCore versucht zunächst, den Wert aduc_model aus der [Konfigurationsdatei](device-update-configuration-file.md) zu lesen.  Wenn der Wert in der Konfigurationsdatei keine Daten enthält, wird standardmäßig die Kompilierzeitdefinition für ADUC_DEVICEPROPERTIES_MODEL gemeldet. Diese Eigenschaft wird nur zur Startzeit gemeldet.|
|aduVer|Zeichenfolge|Gerät an Cloud|Version des Device Update-Agents, der auf dem Gerät ausgeführt wird. Dieser Wert wird nur dann aus dem Build gelesen, wenn während der Kompilierungszeit ENABLE_ADU_TELEMETRY_REPORTING auf 1 (TRUE) festgelegt ist. Kunden können sich entscheiden, die Versionsberichterstellung zu deaktivieren, indem Sie den Wert auf 0 (FALSE) festlegen. [Anpassen der Eigenschaften des Device Update-Agents](https://github.com/Azure/iot-hub-device-update/blob/main/docs/agent-reference/how-to-build-agent-code.md).|
|doVer|Zeichenfolge|Gerät an Cloud|Version des Übermittlungsoptimierungs-Agents, der auf dem Gerät ausgeführt wird. Der Wert wird nur dann aus dem Build gelesen, wenn während der Kompilierungszeit ENABLE_ADU_TELEMETRY_REPORTING auf 1 (TRUE) festgelegt ist. Kunden können sich entscheiden, die Versionsberichterstellung zu deaktivieren, indem Sie den Wert auf 0 (FALSE) festlegen. [Anpassen von Eigenschaften des Übermittlungsoptimierungs-Agents](https://github.com/microsoft/do-client/blob/main/README.md#building-do-client-components).|

### <a name="service-metadata"></a>Dienstmetadaten

Dienstmetadaten enthalten Felder, die von den Device Update-Diensten zum Kommunizieren von Aktionen und Daten an den Device Update-Agent verwendet werden.

|Name|Schema|Direction|BESCHREIBUNG|
|----|------|---------|-----------|
|action|integer|Cloud zu Gerät|Dabei handelt es sich um einen Integerwert, der einer Aktion entspricht, die der Agent ausführen soll. Die unten aufgeführten Werte.|
|updateManifest|Zeichenfolge|Cloud zu Gerät|Wird verwendet, um den Inhalt eines Updates zu beschreiben. Wird aus dem [Importmanifest](import-update.md#create-a-device-update-import-manifest) generiert.|
|updateManifestSignature|JSON-Objekt|Cloud zu Gerät|Eine JSON Web Signature (JWS) mit JSON Web Keys, die für die Quellüberprüfung verwendet werden.|
|fileUrls|Karte|Cloud zu Gerät|Ordnet `FileHash` `DownloadUri` zu. Informiert den Agent, welche Dateien heruntergeladen werden sollen und welcher Hash verwendet werden soll, um zu bestätigen, dass die Dateien ordnungsgemäß heruntergeladen wurden.|

#### <a name="action"></a>Aktion

`Actions` unten stellt die Aktionen dar, die vom Device Update-Agent ausgeführt werden, wie vom Device Update-Dienst angegeben. Der Device Update-Agent meldet einen `State` (siehe Abschnitt „`State`“ oben) und verarbeitet die empfangene `Action`. Weitere Informationen zu Anforderungen, die zwischen dem Device Update-Dienst und dem Device Update-Agent fließen, finden Sie in der [Workflowübersicht](understand-device-update.md#device-update-agent).

|Name|Wert|BESCHREIBUNG|
|---------|-----|-----------|
|Download|0|Herunterladen von veröffentlichtem Inhalt oder des Updates und sämtlicher anderer erforderlicher Inhalte.|
|Installieren|1|Installieren des Inhalts oder Updates. In der Regel bedeutet dies, dass der Installer für den Inhalt oder das Update aufgerufen wird.|
|Anwenden|2|Finalisieren des Updates. Weist das System an,einen Neustart auszuführen,wenn erforderlich.|
|Abbrechen|255|Beendet die Verarbeitung der aktuellen Aktion und setzt den Status auf `Idle` zurück. Wird auch verwendet, um den Agent im Status `Failed` anzuweisen, in den Status `Idle` zurückzukehren.|

## <a name="device-information-interface"></a>Geräteinformationsschnittstelle

Die Geräteinformationsschnittstelle ist ein Konzept, das in der [IoT Plug & Play-Architektur](../iot-pnp/overview-iot-plug-and-play.md) verwendet wird. Sie enthält Gerät-zu-Cloud-Eigenschaften, die Informationen zur Hardware und zum Betriebssystem des Geräts bereitstellen. Device Update for IoT Hub verwendet die Eigenschaften DeviceInformation.manufacturer und DeviceInformation.model für Telemetriedaten und Diagnosen verwendet. Weitere Informationen zur Geräteinformationsschnittstelle finden Sie in diesem [Beispiel](https://devicemodels.azure.com/dtmi/azure/devicemanagement/deviceinformation-1.json).

Der erwartete Komponentenname im Modell ist **deviceInformation**, wenn diese Schnittstelle implementiert wird. [Weitere Informationen zu Azure IoT PnP-Komponenten](../iot-pnp/concepts-components.md)

|Name|type|Schema|Direction|BESCHREIBUNG|Beispiel|
|----|----|------|---------|-----------|-----------|
|Hersteller|Eigenschaft|Zeichenfolge|Gerät an Cloud|Firmenname des Geräteherstellers. Dieser kann mit dem Namen des OEM (Originalgeräteherstellers) identisch sein.|Contoso|
|model|Eigenschaft|Zeichenfolge|Gerät an Cloud|Name oder ID des Gerätemodells.|IoT Edge-Gerät|
|swVersion|Eigenschaft|Zeichenfolge|Gerät an Cloud|Version der Software auf Ihrem Gerät. Bei swVersion kann es sich um die Version Ihrer Firmware handeln.|4.15.0-122|
|osName|Eigenschaft|Zeichenfolge|Gerät an Cloud|Der Name des Betriebssystems auf dem Gerät.|Ubuntu Server 18.04|
|processorArchitecture|Eigenschaft|Zeichenfolge|Gerät an Cloud|Architektur des Prozessors auf dem Gerät.|ARM64|
|processorManufacturer|Eigenschaft|Zeichenfolge|Gerät an Cloud|Der Name des Herstellers des Prozessors auf dem Gerät.|Microsoft|
|TotalStorage|Eigenschaft|Zeichenfolge|Gerät an Cloud|Gesamter verfügbarer Speicher auf dem Gerät in KB.|2048|
|totalMemory|Eigenschaft|Zeichenfolge|Gerät an Cloud|Gesamter verfügbarer Arbeitsspeicher auf dem Gerät in KB.|256|

## <a name="model-id"></a>Modell-ID 

Die Modell-ID gibt an, wie intelligente Geräte ihre Funktionen für Azure IoT-Anwendungen mit IoT Plug & Play ankündigen. Weitere Informationen zum Erstellen von intelligenten Geräten zum Ankündigen ihrer Funktionen für Azure IoT-Anwendungen finden Sie im [Entwicklerleitfaden für IoT Plug & Play-Geräte](../iot-pnp/concepts-developer-guide-device.md).

Für Device Update for IoT Hub muss das intelligente IoT Plug & Play-Gerät im Rahmen der Geräteverbindung eine Modell-ID mit dem Wert **dtmi:AzureDeviceUpdate;1** ankündigen. [Weitere Informationen zum Ankündigen einer Modell-ID](../iot-pnp/concepts-developer-guide-device.md#model-id-announcement).