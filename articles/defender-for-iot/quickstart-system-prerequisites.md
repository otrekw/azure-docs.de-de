---
title: Systemanforderungen
description: Enthält Informationen zu den Systemanforderungen, die für die Ausführung von Azure Defender für IoT erfüllt sein müssen.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/30/2020
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: 4b5db049e6d1cfe76bdd0d5cd6d7360e0b98bad0
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103489810"
---
# <a name="system-prerequisites"></a>Systemanforderungen
Dieser Artikel enthält eine Beschreibung der Systemanforderungen für die Ausführung von Azure Defender für IoT.

## <a name="minimum-requirements"></a>Mindestanforderungen

- Netzwerkswitches, die die Überwachung des Datenverkehrs über den SPAN-Port unterstützen.
- Hardwareappliances für NTA-Sensoren.
- Die Rolle „Mitwirkender“ für das Azure-Abonnement. Sie wird nur beim Onboarding zum Definieren von committeten Geräten und der Verbindung mit Azure Sentinel benötigt.
- Rolle **Mitwirkender** für Azure IoT Hub (Tarif „Free“ oder „Standard“) für die cloudbasierte Verwaltung. Stellen Sie sicher, dass das Feature **Azure Defender für IoT** aktiviert ist.
- Für die Unterstützung von Defender-IoT-Micro-Agents auf Geräteebene unterstützen Defender für IoT-Agents immer mehr Geräte und Plattformen. Zeigen Sie die Liste mit den [unterstützten Plattformen](how-to-deploy-agent.md) an.

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche

Bei Defender für IoT wird der gesamte Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum „Europa, Westen“ weitergeleitet. Der Datenverkehr aller restlichen Regionen wird an das regionale Rechenzentrum „USA, Mitte“ weitergeleitet.

Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).

## <a name="see-also"></a>Weitere Informationen

- [Ermitteln der erforderlichen Appliances](how-to-identify-required-appliances.md)
- [Azure Defender für IoT: Netzwerkeinrichtung](how-to-set-up-your-network.md)
