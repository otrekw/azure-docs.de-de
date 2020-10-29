---
title: Komponenten und Voraussetzungen
description: Hier finden Sie Details zu allen Voraussetzungen für den Einstieg in den Azure Defender für IoT-Dienst.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2020
ms.author: rkarlin
ms.custom: references_regions
ms.openlocfilehash: b0913dc48f807c26396a38e31d293877b4561b7d
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92089178"
---
# <a name="azure-defender-for-iot-prerequisites"></a>Voraussetzungen für Azure Defender für IoT

Dieser Artikel erläutert die verschiedenen Komponenten des Defender für IoT-Diensts, die Anforderungen für den Einstieg und die grundlegenden Konzepte des Diensts.

## <a name="minimum-requirements"></a>Mindestanforderungen

- Überwachung ohne Agents für IoT- und OT-Geräte (auf der Grundlage von CyberX-Technologie)
    - Netzwerkswitches, die die Überwachung des Datenverkehrs über den SPAN-Port unterstützen
    - Hardwareappliances für den NTA-Sensor. Weitere Informationen finden Sie unter [Zertifizierte Hardware](https://aka.ms/AzureDefenderforIoTBareMetalAppliance).
    - Rolle **Mitwirkender** für das Azure-Abonnement (nur während des Onboardings zum Definieren von committeten Geräten erforderlich)
    - Rolle **Mitwirkender** für IoT Hub (Tarif „Free“ oder „Standard“) (für cloudbasierte Verwaltung)
- Sicherheit für verwaltete IoT-Geräte, die über Azure IoT Hub verwaltet werden
    - Rolle **Mitwirkender** für IoT Hub (Tarif „Standard“)
    - IoT Hub: Die Umschaltfläche für das Feature **Azure Defender für IoT** muss aktiviert sein.
    - Für die Unterstützung von Sicherheitsmodulen auf Geräteebene  
        - Defender für IoT-Agents unterstützt eine wachsende Anzahl von Geräten und Plattformen (siehe die [Liste unterstützter Plattformen](how-to-deploy-agent.md)).


## <a name="supported-service-regions"></a>Unterstützte Servicebereiche

Weitere Informationen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=iot-hub). 

Defender für IoT leitet den gesamten Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum von „Europa, Westen“ und aus allen anderen Regionen an das regionale Rechenzentrum von „USA, Mitte“ weiter.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht](overview.md) über Azure IoT-Sicherheit.
- Erfahren Sie, wie Sie [den Dienst aktivieren](quickstart-onboard-iot-hub.md).
- Lesen der [Häufig gestellten Fragen zu Defender für IoT](resources-frequently-asked-questions.md)
- Erkunden von [Grundlegendem zu Defender für IoT-Warnungen](concept-security-alerts.md)
