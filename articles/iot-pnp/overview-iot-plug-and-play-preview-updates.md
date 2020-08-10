---
title: Neuigkeiten IoT Plug & Play-Vorschauaktualisierung | Microsoft-Dokumentation
description: Informationen zu Neuigkeiten beim Release IoT Plug & Play-Vorschauaktualisierung.
author: rido-min
ms.author: rmpablos
ms.date: 07/06/2020
ms.topic: overview
ms.custom: mvc
ms.service: iot-pnp
services: iot-pnp
manager: eliotgra
ms.openlocfilehash: 60ad7f5df4f13d626d7a2c24990c7f48db28d01d
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475225"
---
# <a name="iot-plug-and-play-preview-refresh"></a>IoT Plug & Play-Vorschauaktualisierung

In diesem Artikel werden die wichtigsten Änderungen in den SDKs, Bibliotheken, Tools und Diensten in der IoT Plug & Play-Vorschau, Release vom Juli 2020, beschrieben. Die vorherige Release für IoT Plug & Play-Vorschau wurde im August 2019 veröffentlicht.

## <a name="digital-twins-definition-language-dtdl"></a>Digital Twins Definition Language (DTDL)

Dieses Release fügt Unterstützung für [DTDL v2](https://github.com/Azure/opendigitaltwins-dtdl) hinzu und kennzeichnet [DTDL v1](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL/v1-preview) als veraltet.

In der folgenden Liste sind die wichtigsten Unterschiede zwischen DTDL v1 und DTDL v2 aufgeführt. In DTDL v2:

- Modell-IDs haben das Präfix `dtmi` statt `urn`. Digital Twin Model-Identifier (DTMI) ersetzen die in DTDL v1 verwendeten IDs für digitale Zwillinge mit dem Präfix `urn`. Der Version ist jetzt ein `;` vorangestellt. Wenn Sie vorher beispielsweise `urn:CompanyName:Model:1` verwendet haben, verwenden Sie jetzt `dtmi:CompanyName:Model;1`.
- Legen Sie den Wert für `@context` auf `dtmi:dtdl:context;2` statt `http://azureiot.com/v1/contexts/IoTModel.json` fest.
- Verwenden Sie zum Modellieren eines Geräts den Typ **Schnittstelle** statt **CapabilityModel**.
- **Komponenten** ersetzen Instanzen von **Schnittstellen**. Sie können **Beziehungen** und **Komponenten** als Teil des Inhalts einer **Schnittstelle** definieren.
- Eine **Schnittstelle** kann nur von einer anderen **Schnittstelle** erben.
- Sie können DTDL mithilfe von _erweiterbaren semantischen Typen_ erweitern. Dieses erweiterbare Typsystem bietet größere Flexibilität als die hartcodierten semantischen Typen wie Temperatur und Feuchtigkeit in DTDL v1.
- Die Eigenschaft **displayUnit** wurde entfernt.
- Sie können in einem Namen keine führenden oder nachstehenden Unterstriche verwenden. Führende Unterstriche in einem Namen sind für das System reserviert.

Zum Arbeiten mit DTDL v1 müssen Sie die vorherige Version des SDKs und den Azure IoT-Explorer 0.10.x verwenden. Zum Arbeiten mit DTDL v2 benötigen Sie die neueste Version des SDKs und den Azure IoT-Explorer 0.11.x.

## <a name="no-component-and-multiple-component-implementations"></a>Keine Implementierungen von Komponenten und mehreren Komponenten

Einfache Geräte, die einige Telemetriedaten, Befehle oder Eigenschaften verwenden, können in einer einzigen Schnittstelle ohne Verwendung von Komponenten beschrieben werden. Aus jedem vorhandenen Gerät kann durch Ankündigung der **Modell-ID** ohne Änderungen an der vorhandenen Geräteimplementierung ein IoT Plug & Play-Gerät werden.

Komplexere Geräte könnten Telemetriedaten, Befehle und Eigenschaften auf verschiedenen Schnittstellen gruppieren, um die Komplexität zu verwalten und die Wiederverwendung geräteübergreifend zu ermöglichen. Diese Geräte müssen zur Einhaltung einer Reihe von einfachen Regeln aktualisiert werden, die in den [IoT Plug and Play Preview message conventions](concepts-convention.md) (Konventionen zu Nachrichten bei IoT Plug & Play-Vorschau) definiert sind.

## <a name="registration-and-discovery"></a>Registrierung und Ermittlung

In dieser Version kündigen Geräte ihre **Modell-ID** bei IoT Hub auf jeder Verbindung an. IoT Hub speichert die **Modell-ID** zwischen und ermöglicht so Back-End-Lösungen das Abrufen der **Modell-ID** mithilfe der Eigenschaft `modelId` für Gerätezwillinge. Die **Modell-ID** kann auch aus `$metadata.$model` im digitalen Zwilling abgerufen werden.

## <a name="microsoft-defined-interfaces"></a>Von Microsoft definierte Schnittstellen

Die folgenden von Microsoft definierten Schnittstellen sind veraltet und werden im neuen Modellrepository nicht mehr veröffentlicht:

- **urn:azureiot:ModelDiscovery:DigitalTwin:1**
- **urn:azureiot:ModelDiscovery:ModelInformation:1**

Die folgende Schnittstelle wird im neuen Modellrepository veröffentlicht: `dtmi:azure:DeviceManagement:DeviceInformation;1` verfügbar in der URL [https://repo.azureiotrepository.com/Models/dtmi:azure:DeviceManagement:DeviceInformation;1?api-version=2020-05-01-preview](https://repo.azureiotrepository.com/Models/dtmi:azure:DeviceManagement:DeviceInformation;1?api-version=2020-05-01-preview).

## <a name="digitaltwinchangeevents"></a>DigitalTwinChangeEvents

Die Ereignisstruktur der [Ereignisquelle](../iot-hub/iot-hub-devguide-messages-d2c.md#non-telemetry-events) **DigitalTwinChangeEvents** wurde geändert, um das Format **JSON-Patch** verwenden zu können. Diese Änderung ist eine Breaking Change ohne Unterstützung von Abwärtskompatibilität.

## <a name="message-routing"></a>Nachrichtenweiterleitung

Bei Telemetrienachrichten gibt es die folgenden Änderungen innerhalb der Auflistung [SystemProperties-](../iot-hub/iot-hub-devguide-messages-construct.md).

Sie enthält jetzt die Eigenschaft **dt-dataschema**, bei der es sich um die vom Gerät registrierte **Modell-ID** handelt.

Die Eigenschaft **dt-subject** stellt die Komponente zum Senden einer Telemetrienachricht dar.

Die Eigenschaft **iothub-interface-name** ist veraltet.

## <a name="device-and-service-sdks"></a>Geräte- und Dienst-SDKs

Es gibt keine Abwärtskompatibilität mit früheren Vorschauversionen der SDKs. Sie müssen Ihren Code ändern, wenn Sie zur neuesten Vorschauversion eines SDKs wechseln.

Bei dem auf der Konvention basierenden Ansatz sind keine separaten Geräteclient-SDKs erforderlich. In diesem Vorschaurelease sind die vorhandenen **DigitalTwinClient**-Bibliotheken in allen Sprachen veraltet. Stattdessen wurden die IoT Hub-Geräteclient-SDKs aktualisiert und enthalten jetzt eine Option zum Ankündigen der **Modell-ID**.

Bei Geräten, die keine Komponenten verwenden, sind nur minimale Codeänderungen erforderlich, sodass nur die **Modell-ID** angekündigt wird. Komplexere Geräte, die mehrere Komponenten verwenden, erfordern möglicherweise einige wiederverwendbare Funktionen zum Implementieren der [Konventionen](concepts-convention.md). Gerätebeispiele enthalten eine Reihe von Funktionen, die Sie in Ihrer Geräteimplementierung wiederverwenden könnten.

### <a name="service-sdks"></a>Dienst-SDKs

Die Dienst-SDK steht in [Node.js](https://github.com/Azure/azure-iot-sdk-node/blob/digitaltwins-preview/digitaltwins/service/readme.md) und [Python](https://github.com/Azure/azure-iot-sdk-python/blob/digitaltwins-preview/azure-iot-hub/README.md) zur Verfügung.

## <a name="vs-code-extension"></a>VS Code-Erweiterung

Die Erweiterung [Azure IoT Device Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) bietet Erstellungsunterstützung für DTDL v1, Integration in die vorherige Version des Modellrepositorys und Codegenerierung.

Wenn Sie in VS Code Erstellungsunterstützung für DTDL v2 benötigen, installieren Sie im Code die neue [DTDL-Erweiterung](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl). Die Erweiterung bietet keine Integration in das Modellrepository oder die Codegenerierung. Modelle im Repository werden jetzt über eine [Webbenutzeroberfläche](https://aka.ms/iotmodelrepo) oder eine [Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/pnp?view=azure-cli-latest) verwaltet.

## <a name="digital-twin-service-side-rest-apis"></a>Dienstseitige Rest-APIs für den digitalen Zwilling

Die [dienstseitigen Rest-APIs für den digitalen Zwilling](https://docs.microsoft.com/rest/api/iothub/service/digitaltwin) und Nutzlasten wurden geändert. Die unterstützten APIs dienen zu Folgendem:

- Abrufen eines digitalen Zwillings
- Aufrufen eines Befehls
- Aktualisieren eines digitalen Zwillings mithilfe von **JSON-Patch**-Nutzlast

Die vorhandenen REST-APIs werden in dieser Version weiterhin unterstützt.

## <a name="model-repository"></a>Modellrepository

Es gibt jetzt ein einziges Modellrepository, das sowohl veröffentlichte öffentliche Modelle als auch private, durch RBAC geschützte Unternehmensmodelle enthält. Alle Modelle haben einen eindeutigen Bezeichner und sind nach ihrer Erstellung unveränderlich.

Vorhandene Unternehmensmodellrepositorys aus dem vorherigen Release werden in diesem Release nicht unterstützt. Sie können die Website [Azure Certified for IoT](https://preview.catalog.azureiotsolutions.com/products) weiterhin zum Verwalten der alten DTDL v1-Modelle verwenden. Sie können diese Website jedoch nicht mehr zum Registrieren, Testen und Zertifizieren von Geräten verwenden.

## <a name="azure-iot-central"></a>Azure IoT Central

Azure IoT Central wird zurzeit aktualisiert, um das Release IoT Plug & Play-Vorschauaktualisierung zu unterstützen.
