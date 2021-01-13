---
title: Systemanforderungen
description: Enthält Informationen zu den Systemanforderungen, die für die Ausführung von Azure Defender für IoT erfüllt sein müssen.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 11/30/2020
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: 8ee3afcae69ca6c082452e590eb8370bcc122af4
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2020
ms.locfileid: "97844459"
---
# <a name="system-prerequisites"></a>Systemanforderungen
Dieser Artikel enthält eine Beschreibung der Systemanforderungen für die Ausführung von Azure Defender für IoT.

## <a name="minimum-requirements"></a>Mindestanforderungen

- Netzwerkswitches, die die Überwachung des Datenverkehrs über den SPAN-Port unterstützen.
- Hardwareappliances für NTA-Sensoren.
- Die Rolle „Mitwirkender“ für das Azure-Abonnement. Sie wird nur beim Onboarding zum Definieren von committeten Geräten und der Verbindung mit Azure Sentinel benötigt.
- Rolle **Mitwirkender** für Azure IoT Hub (Tarif „Free“ oder „Standard“) für die cloudbasierte Verwaltung. Stellen Sie sicher, dass das Feature **Azure Defender für IoT** aktiviert ist.
- Für die Unterstützung von Sicherheitsmodulen auf Geräteebene unterstützen Defender für IoT-Agents immer mehr Geräte und Plattformen. Zeigen Sie die Liste mit den [unterstützten Plattformen](how-to-deploy-agent.md) an.

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche

Bei Defender für IoT wird der gesamte Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum „Europa, Westen“ weitergeleitet. Der Datenverkehr aller restlichen Regionen wird an das regionale Rechenzentrum „USA, Mitte“ weitergeleitet.

Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub).

## <a name="see-also"></a>Weitere Informationen

- [Ermitteln der erforderlichen Appliances](how-to-identify-required-appliances.md)
- [Azure Defender für IoT: Netzwerkeinrichtung](how-to-set-up-your-network.md)
