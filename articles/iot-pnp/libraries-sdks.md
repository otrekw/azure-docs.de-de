---
title: IoT Plug & Play-Bibliotheken und SDKs
description: Informationen zu den Geräte- und Dienstbibliotheken, die für die Entwicklung von IoT Plug & Play-fähigen Lösungen zur Verfügung stehen.
author: rido-min
ms.author: rmpablos
ms.date: 07/22/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 5638cd9973c6a4df809e0b200efe85b067aae026
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407794"
---
# <a name="microsoft-sdks-for-iot-plug-and-play"></a>Microsoft-SDKs für IoT Plug & Play

Die IoT Plug & Play-Bibliotheken und SDKs ermöglichen es Entwicklern, IoT-Lösungen mithilfe einer Vielzahl von Programmiersprachen auf mehreren Plattformen zu erstellen. Die nachstehende Tabelle enthält Links zu Beispielen und Schnellstarts, die Ihnen den Einstieg erleichtern sollen:

## <a name="device-sdks-ga"></a>Geräte-SDKs (allgemeine Verfügbarkeit)

| Sprache | Paket | Coderepository | Beispiele | Schnellstart | Verweis |
|---|---|---|---|---|---|
| C (Gerät) | [vcpkg 1.3.9](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/setting_up_vcpkg.md) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/releases/tag/2020-07-19) | [Beispiele](https://github.com/Azure/azure-iot-sdk-c/tree/2020-07-19/iothub_client/samples/pnp) | [Herstellen einer Verbindung mit IoT Hub](quickstart-connect-device-c.md) | [Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| .NET (Gerät) | [NuGet 1.27.0](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/1.27.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/) | [Beispiele](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device/samples/PnpDeviceSamples) | [Herstellen einer Verbindung mit IoT Hub](quickstart-connect-device-csharp.md) | [Referenz](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client?view=azure-dotnet) |
| Java (Gerät) | [Maven 1.24.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-device-client/1.24.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/master/) | [Beispiele](https://github.com/Azure/azure-iot-sdk-java/tree/master/device/iot-device-samples/pnp-device-sample) | [Herstellen einer Verbindung mit IoT Hub](quickstart-connect-device-java.md) | [Referenz](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device?view=azure-java-stable) |
| Python (Gerät) | [pip 2.1.4](https://pypi.org/project/azure-iot-device/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/master/) | [Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/master/azure-iot-device/samples/pnp) | [Herstellen einer Verbindung mit IoT Hub](quickstart-connect-device-python.md) | [Referenz](https://docs.microsoft.com/python/api/azure-iot-device/azure.iot.device?view=azure-python) |
| Node (Gerät) | [npm 1.17.0](https://www.npmjs.com/package/azure-iot-device)  | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/master/) | [Beispiele](https://github.com/Azure/azure-iot-sdk-node/tree/master/device/samples/pnp) | [Herstellen einer Verbindung mit IoT Hub](quickstart-connect-device-node.md) | [Referenz](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-node-latest) |

## <a name="device-sdks-preview"></a>Geräte-SDKs (Vorschau)

| Sprache | Coderepository/Beispiele |
|---|---|
|Azure SDK für Embedded| [GitHub](https://github.com/Azure/azure-sdk-for-c/#) |
|Azure RTOS-IoT-Middleware| [GitHub](https://github.com/azure-rtos/azure-iot-preview#) |
|Azure RTOS – Leitfäden mit ersten Schritten | [GitHub](https://github.com/azure-rtos/getting-started) |

## <a name="service-sdks-preview"></a>Dienst-SDKs (Vorschau)

| Sprache | Paket | Coderepository | Beispiele | Schnellstart | Verweis |
|---|---|---|---|---|---|
| .NET – IoT Hub-Dienst (Vorschau) | [NuGet 1.27.1-preview-002](https://www.nuget.org/packages/Microsoft.Azure.Devices/1.27.1-preview-002 ) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp/tree/pnp-preview-refresh) | [Beispiele](https://github.com/Azure/azure-iot-sdk-csharp/tree/pnp-preview-refresh/iothub/service/samples/PnpServiceSamples) | N/V | – |
| Java – IoT Hub-Dienst (Vorschau) | [Maven 1.1.0](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/iot-service-client-preview/1.1.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java/tree/pnp-preview-refresh) | [Beispiele](https://github.com/Azure/azure-iot-sdk-java/tree/pnp-preview-refresh/service/iot-service-samples/pnp-service-sample) | N/V | – |
| Node – IoT Hub-Dienst (Vorschau) | [npm 1.12.4-pnp-refresh.4](https://www.npmjs.com/package/azure-iothub/v/1.12.4-pnp-refresh.4) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/) | [Beispiele](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/service/samples) | N/V | – |
| Python – IoT Hub/Digital Twins-Dienst (Vorschau) | [pip 2.2.1rc1](https://pypi.org/project/azure-iot-hub/2.2.1rc1/) | [GitHub](https://github.com/Azure/azure-iot-sdk-python/tree/pnp-preview-refresh) | [Beispiele](https://github.com/Azure/azure-iot-sdk-python/tree/pnp-preview-refresh/azure-iot-hub/samples) | [Interaktion mit IoT Hub Digital Twins-API](quickstart-service-python.md) | – |
| Node – Digital Twins-Dienst (Vorschau) | [npm 1.0.0-pnp-refresh.3](https://www.npmjs.com/package/azure-iot-digitaltwins-service/v/1.0.0-pnp-refresh.3) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/) | [Beispiele](https://github.com/Azure/azure-iot-sdk-node/tree/pnp-preview-refresh/digitaltwins/samples/service/javascript) | [Interaktion mit IoT Hub Digital Twins-API](quickstart-service-node.md) | – |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Testen der SDKs und Bibliotheken finden Sie im [Entwicklerhandbuch](concepts-developer-guide.md) und in den [Schnellstartanleitungen für Geräte](quickstart-connect-device-c.md) und [Schnellstartanleitungen für Dienste](quickstart-service-node.md).
