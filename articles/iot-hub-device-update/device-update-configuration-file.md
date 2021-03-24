---
title: Grundlegendes zur Konfigurationsdatei von Device Update for Azure IoT Hub | Microsoft-Dokumentation
description: Grundlegendes zur Konfigurationsdatei von Device Update for Azure IoT Hub.
author: ValOlson
ms.author: valls
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 39ecd9d6d0deec942d5c8cce9357c779a4b328d3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101660373"
---
# <a name="device-update-for-iot-hub-configuration-file"></a>Konfigurationsdatei von Device Update for IoT Hub

„adu-conf.txt“ ist eine optionale Datei, die zum Verwalten der folgenden Konfigurationen erstellt werden kann.

* AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["manufacturer"]
* AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["model"]
* DeviceInformation.manufacturer
* DeviceInformation.model
* Geräteverbindungszeichenfolge (wenn sie dem Device Update-Agent nicht bekannt ist).

## <a name="purpose"></a>Zweck
Der Device Update-Agent versucht zunächst, die `manufacturer`- und `model`-Werte vom Gerät abzurufen, um sie für die [Schnittstellenebene](device-update-agent-overview.md#the-interface-layer) zu verwenden. Wenn dies fehlschlägt, sucht der Device Update-Agent anschließend nach der Datei „adu-conf.txt“ und verwendet die Werte aus dieser Datei. Wenn beide Versuche nicht erfolgreich sind, verwendet der Device Update-Agent [Standardwerte](https://github.com/Azure/iot-hub-device-update/blob/main/CMakeLists.txt).

Erfahren Sie mehr über die [ADU Core-Schnittstelle](https://github.com/Azure/iot-hub-device-update/tree/main/src/agent/adu_core_interface) und die [Geräteinformationsschnittstelle](https://github.com/Azure/iot-hub-device-update/tree/main/src/agent/device_info_interface).

## <a name="file-location"></a>Dateispeicherort

Erstellen Sie im Linux-System auf der Partition oder dem Datenträger mit dem Namen `adu` eine Textdatei namens „adu-conf.txt“ mit den folgenden Feldern.

## <a name="list-of-fields"></a>Liste der Felder

|Name|BESCHREIBUNG|
|-----------|--------------------|
|connection_string|Vorab bereitgestellte Verbindungszeichenfolge, die das Gerät zum Herstellen einer Verbindung mit dem IoT Hub verwenden kann. Hinweis: Nicht erforderlich, wenn Sie den Device Update-Agent über den [Azure IoT-Identitätsdienst](https://azure.github.io/iot-identity-service/) bereitstellen.|
|aduc_manufacturer|Wird von der `AzureDeviceUpdateCore:4.ClientMetadata:4`-Schnittstelle gemeldet, um das Gerät für die Updatebereitstellung zu klassifizieren.|
|aduc_model|Wird von der `AzureDeviceUpdateCore:4.ClientMetadata:4`-Schnittstelle gemeldet, um das Gerät für die Updatebereitstellung zu klassifizieren.|
|Hersteller|Wird vom Device Update-Agent als Teil der `DeviceInformation`-Schnittstelle gemeldet.|
|model|Wird vom Device Update-Agent als Teil der `DeviceInformation`-Schnittstelle gemeldet.|

## <a name="example-adu-conftxt-file-contents"></a>Inhalt einer Beispieldatei „adu-conf.txt“

```markdown

connection_string = `HostName=<yourIoTHubName>;DeviceId=<yourDeviceId>;SharedAccessKey=<yourSharedAccessKey>`
aduc_manufacturer = <value to send through `AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["manufacturer"]`
aduc_model = <value to send through `AzureDeviceUpdateCore:4.ClientMetadata:4.deviceProperties["model"]`
manufacturer = <value to send through `DeviceInformation.manufacturer`
model = <value to send through `DeviceInformation.manufacturer`
```
