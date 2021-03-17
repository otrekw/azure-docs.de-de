---
title: 'Schnellstart: Konfigurieren und Aktivieren des Sicherheitsmoduls für Azure RTOS'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie das Sicherheitsmodul für den Azure RTOS-Dienst in Ihre Azure IoT Hub-Instanz integrieren und darin aktivieren.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: 19a439ec48d4a8705ffb46db7ca037b51449083d
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102447298"
---
# <a name="quickstart-security-module-for-azure-rtos"></a>Schnellstart: Sicherheitsmodul für Azure RTOS 

Dieser Artikel enthält eine Erläuterung der Voraussetzungen vor den ersten Schritten, und zwar wird darin erläutert, wie Sie das Sicherheitsmodul für den Azure RTOS-Dienst auf einer IoT Hub-Instanz aktivieren. Besitzen Sie derzeit keine IoT Hub-Instanz, lesen Sie zum Einstieg die Informationen unter [Erstellen eines IoT Hubs über das Portal](../iot-hub/iot-hub-create-through-portal.md).

## <a name="prerequisites"></a>Voraussetzungen 

### <a name="supported-devices"></a>Unterstützte Geräte

- ST STM32F746G Discovery Kit
- NXP i.MX RT1060 EVK
- Mikrochip SAM E54 Xplained Pro EVK

Laden Sie eine der ZIP-Dateien für das jeweilige Board und Tool (IAR, die semi-IDE oder den PC) Ihrer Wahl aus der [GitHub-Ressource des Sicherheitsmoduls für Azure RTOS](https://github.com/azure-rtos/azure-iot-preview/releases) herunter, kompilieren Sie sie, und führen Sie sie aus.

### <a name="azure-resources"></a>Azure-Ressourcen

Die nächste Phase bei den ersten Schritten ist die Vorbereitung Ihrer Azure-Ressourcen. Sie benötigen einen IoT Hub, und wir empfehlen einen Log Analytics-Arbeitsbereich. Für IoT Hub benötigen Sie Ihre IoT Hub-Verbindungszeichenfolge, um eine Verbindung mit Ihrem Gerät herzustellen. 
  
### <a name="iot-hub-connection"></a>IoT Hub-Verbindung

Für die ersten Schritte ist eine IoT Hub-Verbindung ist erforderlich. 

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**.

1. Navigieren Sie zu **IoT-Geräte**.

1. Klicken Sie auf **Erstellen**.

1. Kopieren Sie die IoT-Verbindungszeichenfolge in die [Konfigurationsdatei](how-to-azure-rtos-security-module.md).

Die Verbindungsanmeldeinformationen werden aus der Benutzeranwendungskonfiguration **HOST_NAME**, **DEVICE_ID** und **DEVICE_SYMMETRIC_KEY** übernommen.

Das Sicherheitsmodul für Azure RTOS verwendet Azure IoT Middleware-Verbindungen basierend auf dem **MQTT**-Protokoll.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie den nächsten Artikel, um die Konfiguration und Anpassung Ihrer Lösung abschließen zu können.

> [!div class="nextstepaction"]
> [Konfigurieren des Sicherheitsmoduls für Azure RTOS](how-to-azure-rtos-security-module.md)
