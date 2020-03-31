---
title: IoT Plug & Play-Bibliotheken und SDKs
description: Informationen zu den Geräte- und Dienstbibliotheken, die für die Entwicklung von IoT Plug & Play-fähigen Lösungen zur Verfügung stehen.
author: dominicbetts
ms.author: dobett
ms.date: 01/08/2020
ms.topic: reference
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 394edc24cfeefb96cb2f412985cb990a2306bb09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064333"
---
# <a name="iot-plug-and-play-libraries-and-sdks"></a>IoT Plug & Play-Bibliotheken und SDKs

Die IoT Plug & Play-Bibliotheken und SDKs ermöglichen es Entwicklern, IoT-Lösungen mithilfe einer Vielzahl von Programmiersprachen auf mehreren Plattformen zu erstellen. Die nachstehende Tabelle enthält Links zu Beispielen und Schnellstarts, die Ihnen den Einstieg erleichtern sollen:

## <a name="microsoft-supported-libraries-and-sdks"></a>Von Microsoft unterstützte Bibliotheken und SDKs

| Plattform | Bibliothek/Paket | Quellcode | Beispiel | Schnellstart | Verweis |
| -------- | ------- | ----------- | ------ | ---------- | --------- |
| C/Linux  | [Geräte-SDK auf apt-get](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#aptgetpackage) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Clientbeispiele für Digitaler Zwilling](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [Herstellen einer Verbindung mit IoT Hub](./quickstart-connect-pnp-device-c-linux.md) | [Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Windows  | [Geräte-SDK auf Vcpkg](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/doc/setting_up_vcpkg.md#setup-c-sdk-vcpkg-for-windows-development-environment) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Clientbeispiele für Digitaler Zwilling](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) | [Herstellen einer Verbindung mit IoT Hub](./quickstart-connect-pnp-device-c-windows.md) | [Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/mbed  | [Geräte-SDK auf EMBED](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#mbed) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Clientbeispiele für Digitaler Zwilling](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/Arduino  | [Geräte-SDK in Arduino-IDE](https://github.com/Azure/azure-iot-sdk-c/blob/public-preview/iothub_client/readme.md#arduino) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Clientbeispiele für Digitaler Zwilling](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C/iOS  | [Geräte-SDK auf CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient) | [GitHub](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview) | [Clientbeispiele für Digitaler Zwilling](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/digitaltwin_client/samples) |  | [Referenz](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/) |
| C#    | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Devices.DigitalTwin.Client) | [GitHub](https://github.com/Azure/azure-iot-sdk-csharp) | [Beispiele für Digitaler Zwilling](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/digitaltwin/Samples) | [Herstellen einer Verbindung mit IoT Hub](./quickstart-connect-pnp-device-csharp.md) | [Referenz](https://docs.microsoft.com/dotnet/api/overview/azure/iot/client?view=azure-dotnet) |
| Java   | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure.sdk.iot/digital-twin-device-client-preview/1.0.0) | [GitHub](https://github.com/Azure/azure-iot-sdk-java) | [Beispiele für Digitaler Zwilling](https://github.com/Azure-Samples/azure-iot-samples-java/tree/master/digital-twin/Samples) | [Herstellen einer Verbindung mit IoT Hub](./quickstart-connect-pnp-device-java.md) | [Referenz](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device) |
| Node.js | [NPM](https://www.npmjs.com/package/azure-iot-digitaltwins-device) | [GitHub](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview) | [Beispiele für Digitaler Zwilling](https://github.com/Azure/azure-iot-sdk-node/tree/digitaltwins-preview/digitaltwins/samples) | [Herstellen einer Verbindung mit IoT Hub](./quickstart-connect-pnp-device-node.md) | [Referenz](https://docs.microsoft.com/javascript/api/azure-iot-device/) |

## <a name="iot-hub-support"></a>IoT Hub-Unterstützung

IoT Plug & Play-Gerätefunktionen werden nur von [kostenlosen IoT Hubs und IoT Hubs des Standard-Tarifs](../iot-hub/iot-hub-scaling.md)unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Zusätzlich zu den Geräte-SDKs und Bibliotheken können Sie Rest-APIs verwenden, um mit den Modellrepositorys zu interagieren.