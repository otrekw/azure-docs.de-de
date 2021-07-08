---
title: 'Schnellstart: Systemvoraussetzungen'
description: In dieser Schnellstartanleitung erhalten Sie Informationen zu den für die Durchführung von Azure Defender für IoT erforderlichen Systemvoraussetzungen.
ms.date: 11/30/2020
ms.topic: quickstart
ms.openlocfilehash: f770a6436563439603403e0aedf62d496a38d805
ms.sourcegitcommit: a038863c0a99dfda16133bcb08b172b6b4c86db8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2021
ms.locfileid: "113011101"
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

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche

Bei Defender für IoT wird der gesamte Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum „Europa, Westen“ weitergeleitet. Der Datenverkehr aller restlichen Regionen wird an das regionale Rechenzentrum „USA, Mitte“ weitergeleitet.

Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Ermitteln der erforderlichen Appliances](how-to-identify-required-appliances.md)
