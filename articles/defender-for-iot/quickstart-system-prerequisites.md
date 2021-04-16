---
title: 'Schnellstart: Systemvoraussetzungen'
description: In dieser Schnellstartanleitung erhalten Sie Informationen zu den für die Durchführung von Azure Defender für IoT erforderlichen Systemvoraussetzungen.
ms.date: 11/30/2020
ms.topic: quickstart
ms.openlocfilehash: 2aae849b6ee772c2aa29c680f3b107af3ed600b0
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2021
ms.locfileid: "106382945"
---
# <a name="quickstart-system-prerequisites"></a>Schnellstart: Systemvoraussetzungen

Dieser Artikel enthält eine Beschreibung der Systemanforderungen für die Ausführung von Azure Defender für IoT.

## <a name="prerequisites"></a>Voraussetzungen

- Keine

## <a name="minimum-requirements"></a>Mindestanforderungen

- Netzwerkswitches, die die Überwachung des Datenverkehrs über den SPAN-Port unterstützen.
- Hardwareappliances für NTA-Sensoren.
- Die Rolle „Mitwirkender“ für das Azure-Abonnement. Sie wird nur beim Onboarding zum Definieren von committeten Geräten und der Verbindung mit Azure Sentinel benötigt.
- Rolle **Mitwirkender** für Azure IoT Hub (Tarif „Free“ oder „Standard“) für die cloudbasierte Verwaltung. Stellen Sie sicher, dass das Feature **Azure Defender für IoT** aktiviert ist.
- Für die Unterstützung von Defender-IoT-Micro-Agents auf Geräteebene unterstützen Defender für IoT-Agents immer mehr Geräte und Plattformen. Zeigen Sie die Liste mit den [unterstützten Plattformen](how-to-deploy-agent.md) an.

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche

Bei Defender für IoT wird der gesamte Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum „Europa, Westen“ weitergeleitet. Der Datenverkehr aller restlichen Regionen wird an das regionale Rechenzentrum „USA, Mitte“ weitergeleitet.

Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ermitteln der erforderlichen Appliances](how-to-identify-required-appliances.md)
