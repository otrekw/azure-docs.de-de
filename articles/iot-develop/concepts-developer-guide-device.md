---
title: Leitfaden für Geräteentwickler (C) – IoT Plug & Play | Microsoft-Dokumentation
description: Dies ist eine Beschreibung von IoT Plug & Play für C-Geräteentwickler.
author: rido-min
ms.author: rmpablos
ms.date: 11/19/2020
ms.topic: conceptual
ms.service: iot-develop
services: iot-develop
zone_pivot_groups: programming-languages-set-twenty-six
ms.openlocfilehash: f21ce89c265bc97d393c1ea2766acfb6fe63d6ee
ms.sourcegitcommit: 8669087bcbda39e3377296c54014ce7b58909746
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2021
ms.locfileid: "114406513"
---
# <a name="iot-plug-and-play-device-developer-guide"></a>Leitfaden für IoT Plug & Play-Geräteentwickler

Mit IoT Plug & Play können Sie intelligente Geräte erstellen, deren Funktionen für Azure IoT-Anwendungen verfügbar gemacht werden. Bei IoT Plug & Play-Geräten ist keine manuelle Konfiguration erforderlich, wenn ein Kunde sie mit IoT Plug & Play-fähigen Anwendungen verbindet.

Ein intelligentes Gerät kann direkt mithilfe von [Modulen](../iot-hub/iot-hub-devguide-module-twins.md) oder [IoT Edge-Modulen](../iot-edge/about-iot-edge.md) implementiert werden

In dieser Anleitung werden die grundlegenden Schritte beschrieben, die zum Erstellen eines Geräts, Moduls oder IoT Edge-Moduls erforderlich sind, das den [IoT Plug & Play-Konventionen](../iot-develop/concepts-convention.md) entspricht.

Führen Sie die folgenden Schritte aus, um ein IoT Plug & Play-Gerät, -Modul oder IoT Edge-Modul zu erstellen:

1. Stellen Sie sicher, dass Ihr Gerät entweder das MQTT- oder das MQTT über WebSockets-Protokoll verwendet, um eine Verbindung mit Azure IoT Hub herzustellen.
1. Erstellen Sie ein [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)-Modell zum Beschreiben Ihres Geräts. Weitere Informationen finden Sie unter [Verstehen der Komponenten in IoT Plug & Play-Modellen](concepts-modeling-guide.md).
1. Aktualisieren Sie Ihr Gerät oder Modul, um `model-id` als Teil der Geräteverbindung anzukündigen.
1. Implementieren Sie Telemetrie, Eigenschaften und Befehle anhand der [IoT Plug & Play-Konventionen](concepts-convention.md).

Sobald die Geräte- oder Modulimplementierung abgeschlossen ist, verwenden Sie den [Azure IoT-Explorer](../iot-fundamentals/howto-use-iot-explorer.md), um zu überprüfen, ob das Gerät den IoT Plug & Play-Konventionen entspricht.

:::zone pivot="programming-language-ansi-c"

[!INCLUDE [iot-pnp-device-devguide-c](../../includes/iot-pnp-device-devguide-c.md)]

:::zone-end

:::zone pivot="programming-language-csharp"

[!INCLUDE [iot-pnp-device-devguide-csharp](../../includes/iot-pnp-device-devguide-csharp.md)]

:::zone-end

:::zone pivot="programming-language-java"

[!INCLUDE [iot-pnp-device-devguide-java](../../includes/iot-pnp-device-devguide-java.md)]

:::zone-end

:::zone pivot="programming-language-javascript"

[!INCLUDE [iot-pnp-device-devguide-node](../../includes/iot-pnp-device-devguide-node.md)]

:::zone-end

:::zone pivot="programming-language-python"

[!INCLUDE [iot-pnp-device-devguide-python](../../includes/iot-pnp-device-devguide-python.md)]

:::zone-end

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich mit der IoT Plug & Play-Geräteentwicklung vertraut gemacht haben, finden Sie weitere Informationen in folgenden Artikeln:

- [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)
- [C-Geräte-SDK](/azure/iot-hub/iot-c-sdk-ref/)
- [IoT-REST-API](/rest/api/iothub/device)
- [Verstehen der Komponenten in IoT Plug & Play-Modellen](concepts-modeling-guide.md)
- [Leitfaden für IoT Plug & Play-Dienstentwickler](concepts-developer-guide-service.md)