---
author: dominicbetts
ms.author: dobett
ms.service: iot-pnp
ms.topic: include
ms.date: 09/24/2020
ms.openlocfilehash: 706e936590f5f47ee2989bf77b3caad67a8f28fc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91579198"
---
Mit IoT Plug & Play können Sie intelligente Geräte erstellen, deren Funktionen für Azure IoT-Anwendungen verfügbar gemacht werden. Bei IoT Plug & Play-Geräten ist keine manuelle Konfiguration erforderlich, wenn ein Kunde sie mit IoT Plug & Play-fähigen Anwendungen verbindet.

Ein intelligentes Gerät kann direkt mithilfe von [Modulen](../articles/iot-hub/iot-hub-devguide-module-twins.md) oder [IoT Edge-Modulen](../articles/iot-edge/about-iot-edge.md) implementiert werden

In dieser Anleitung werden die grundlegenden Schritte beschrieben, die zum Erstellen eines Geräts, Moduls oder IoT Edge-Moduls erforderlich sind, das den [IoT Plug & Play-Konventionen](../articles/iot-pnp/concepts-convention.md) entspricht.

Führen Sie die folgenden Schritte aus, um ein IoT Plug & Play-Gerät, -Modul oder IoT Edge-Modul zu erstellen:

1. Stellen Sie sicher, dass Ihr Gerät entweder das MQTT- oder das MQTT über WebSockets-Protokoll verwendet, um eine Verbindung mit Azure IoT Hub herzustellen.
1. Erstellen Sie ein [Digital Twins Definition Language (DTDL)](https://github.com/Azure/opendigitaltwins-dtdl)-Modell zum Beschreiben Ihres Geräts. Weitere Informationen finden Sie unter [Verstehen der Komponenten in IoT Plug & Play-Modellen](../articles/iot-pnp/concepts-components.md).
1. Aktualisieren Sie Ihr Gerät oder Modul, um `model-id` als Teil der Geräteverbindung anzukündigen.
1. Implementieren Sie Telemetrie, Eigenschaften und Befehle anhand der [IoT Plug & Play-Konventionen](../articles/iot-pnp/concepts-convention.md).

Sobald die Geräte- oder Modulimplementierung abgeschlossen ist, verwenden Sie den [Azure IoT-Explorer](../articles/iot-pnp/howto-use-iot-explorer.md), um zu überprüfen, ob das Gerät den IoT Plug & Play-Konventionen entspricht.
