---
title: Komponenten und Voraussetzungen
description: Hier finden Sie Details zu allen Voraussetzungen für den Einstieg in den Azure Defender für IoT-Dienst.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.custom: references_regions
ms.openlocfilehash: 19263f8db58c8d20288d3ae74c24efd85667bc33
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90930452"
---
# <a name="azure-defender-for-iot-prerequisites"></a>Voraussetzungen für Azure Defender für IoT

Dieser Artikel erläutert die verschiedenen Komponenten des Defender für IoT-Diensts, die Anforderungen für den Einstieg und die grundlegenden Konzepte des Diensts.

## <a name="minimum-requirements"></a>Mindestanforderungen

- IoT Hub-Standard-Tarif
  - Azure-Rollenberechtigungen auf der Ebene **Besitzer**
- [Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)
- Azure Security Center (empfohlen)
  - Die Verwendung von Azure Security Center ist eine Empfehlung und keine Anforderung. Ohne Azure Security Center können Sie Ihre anderen Azure-Ressourcen in IoT Hub nicht anzeigen.

## <a name="working-with-defender-for-iot-service"></a>Arbeiten mit dem Defender für IoT-Dienst

Defender für IoT-Einblicke und -Berichte sind über Azure IoT Hub und Azure Security Center verfügbar. Um Defender für IoT auf Ihrer Instanz von Azure IoT Hub zu aktivieren, ist ein Konto mit Berechtigungen der Ebene **Besitzer** erforderlich. Nach der Aktivierung von Defender für IoT in Ihrer IoT Hub-Instanz werden Defender für IoT-Einblicke in Azure IoT Hub als das Feature **Sicherheit** und in Azure Security Center als **IoT** angezeigt.

## <a name="supported-service-regions"></a>Unterstützte Servicebereiche

Defender für IoT wird derzeit für IoT Hub-Instanzen in den folgenden Azure-Regionen unterstützt:

- USA (Mitte)
- East US
- USA (Ost) 2
- USA, Westen-Mitte
- USA (Westen)
- USA, Westen 2
- USA, Süden-Mitte
- USA Nord Mitte
- Kanada, Mitte
- Kanada, Osten
- Nordeuropa
- Brasilien Süd
- Frankreich, Mitte
- UK, Westen
- UK, Süden
- Europa, Westen
- Nordeuropa
- Japan, Westen
- Japan, Osten
- Australien, Südosten
- Australien (Osten)
- Asien, Osten
- Asien, Südosten
- Korea, Mitte
- Korea, Süden
- Indien, Mitte
- Indien (Süden)

Defender für IoT leitet den gesamten Datenverkehr aus allen europäischen Regionen an das regionale Rechenzentrum von „Europa, Westen“ und aus allen anderen Regionen an das regionale Rechenzentrum von „USA, Mitte“ weiter.

## <a name="wheres-my-iot-hub"></a>Wo ist meine IoT Hub-Instanz?

Überprüfen Sie Ihren IoT Hub-Standort, um die Verfügbarkeit des Diensts zu sicherzustellen, bevor Sie beginnen.

1. Öffnen Sie Ihre IoT Hub-Instanz.
1. Klicken Sie auf **Overview**.
1. Überprüfen Sie, ob der aufgeführte Standort mit einer der [unterstützten Dienstregionen](#supported-service-regions) übereinstimmt.

## <a name="supported-platforms-for-agents"></a>Unterstützte Plattformen für Agents

Defender für IoT-Agents unterstützt eine wachsende Anzahl von Geräten und Plattformen. In der [Liste der unterstützten Plattformen](how-to-deploy-agent.md) können Sie Ihre vorhandene oder geplante Gerätebibliothek überprüfen.

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [Übersicht](overview.md) über Azure IoT-Sicherheit.
- Erfahren Sie, wie Sie [den Dienst aktivieren](quickstart-onboard-iot-hub.md).
- Lesen der [Häufig gestellten Fragen zu Defender für IoT](resources-frequently-asked-questions.md)
- Erkunden von [Grundlegendem zu Defender für IoT-Warnungen](concept-security-alerts.md)
