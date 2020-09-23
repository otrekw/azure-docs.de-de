---
title: Integrierte und anpassbare Warnungen und Empfehlungen des Sicherheitsmoduls für Azure RTOS
description: Hier erhalten Sie Informationen zu Sicherheitswarnungen und empfohlenen Abhilfemaßnahmen unter Verwendung des IoT-Sicherheitsmoduls von Azure RTOS.
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
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: cf4924f8a9b97487e64e12ab80df92f2b2a81de2
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90931457"
---
# <a name="security-module-for-azure-rtos-security-alerts-and-recommendations-preview"></a>Sicherheitswarnungen und -empfehlungen für das Sicherheitsmodul für Azure RTOS (Vorschau)

Das Sicherheitsmodul für Azure RTOS analysiert fortwährend Ihre IoT-Lösung mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor potenziellen böswilligen Aktivitäten und verdächtigen Systemänderungen zu warnen. Darüber hinaus können Sie – basierend auf Ihrer Kenntnis des erwarteten Geräteverhaltens und Baselines – benutzerdefinierte Warnungen erstellen.

Eine Warnung des Sicherheitsmoduls für Azure RTOS fungiert als Indikator für eine potenzielle Kompromittierung, die untersucht und behoben werden sollte. Eine Empfehlung des Sicherheitsmoduls für Azure RTOS identifiziert einen schwachen Sicherheitsstatus, der behoben und aktualisiert werden sollte. 

In diesem Artikel finden Sie eine Liste der integrierten Warnungen und Empfehlungen, die basierend auf den Standardbereichen ausgelöst werden, und die Sie mit Ihren eigenen Werten, basierend auf erwartetem oder Baselineverhalten anpassen können. 

Weitere Informationen zur Funktionsweise der Anpassung von Warnungen im Defender für IoT-Dienst finden Sie unter [anpassbare Warnungen](concept-customizable-security-alerts.md). Die spezifischen Warnungen und Empfehlungen, die zur Anpassung verfügbar sind, wenn Sie das Sicherheitsmodul für Azure RTOS verwenden, werden in den folgenden Tabellen ausführlich erläutert. 

## <a name="security-module-for-azure-rtos-supported-security-alerts"></a>Sicherheitsmodul für Azure RTOS: unterstützte Sicherheitswarnungen

### <a name="device-related-security-alerts"></a>Gerätebezogene Sicherheitswarnungen

|Gerätebezogene Sicherheitswarnungsaktivität  |Warnungsname  |
|---------|---------|
|IP-Adresse| Kommunikation mit einer verdächtigen IP-Adresse erkannt|
|X.509-Gerätezertifikat-Fingerabdruck|X.509-Gerätezertifikat-Fingerabdruck-Konflikt|
|X.509-Zertifikat| x.509-Zertifikat abgelaufen|
|SAS-Token| SAS-Token abgelaufen|
|SAS-Token| Ungültige SAS-Tokensignatur|

### <a name="iot-hub-related-security-alerts"></a>IoT Hub-bezogene Sicherheitswarnungen

|IoT Hub-Sicherheitswarnungsaktivität  |Warnungsname  |
|---------|---------|
|Hinzufügen eines Zertifikats    |  Nicht erfolgreichen Versuch, ein Zertifikat zu einem IoT Hub hinzuzufügen, erkannt       |
|Hinzufügen oder Bearbeiten einer Diagnoseeinstellung    | Versuch, eine Diagnoseeinstellung für einen IoT Hub hinzuzufügen oder zu bearbeiten, erkannt      |
|Löschen eines Zertifikats    |  Nicht erfolgreichen Versuch, ein Zertifikat von einem IoT Hub zu löschen, erkannt       |
|Löschen einer Diagnoseeinstellung    |  Versuch, eine Diagnoseeinstellung von einem IoT Hub zu löschen, erkannt      |
|Gelöschtes Zertifikat    | Löschen eines Zertifikats von einem IoT Hub erkannt        |
|Neues Zertifikat     |  Hinzufügen eines neuen Zertifikats zu einem IoT Hub erkannt       |

## <a name="security-module-for-azure-rtos-supported-customizable-alerts"></a>Sicherheitsmodul für Azure RTOS: unterstützte anpassbare Warnungen

### <a name="device-related-customizable-alerts"></a>Gerätebezogene, anpassbare Warnungen

|Gerätebezogene Aktivität |Warnungsname  |
|---------|---------|
|Aktive Verbindungen|Die Anzahl aktiver Verbindungen liegt nicht innerhalb des zulässigen Bereichs.|
|Cloud-zu-Gerät-Nachrichten in **MQTT**|Die Anzahl von Cloud-zu-Gerät-Nachrichten im **MQTT**-Protokoll liegt nicht innerhalb des zulässigen Bereichs.|
|Ausgehende Verbindung| Ausgehende Verbindung zu einer unzulässigen IP-Adresse|

### <a name="hub-related-customizable-alerts"></a>Hubbezogene, anpassbare Warnungen 

|Hubbezogene Aktivität  |Warnungsname  |
|---------|---------|
|Bereinigungen der Befehlswarteschlange     |  Die Anzahl von Befehlswarteschlangenbereinigungen liegt außerhalb des zulässigen Bereichs.       |
|Cloud-zu-Gerät-Nachrichten im **MQTT**-Protokoll    |  Die Anzahl von Cloud-zu-Gerät-Nachrichten im **MQTT**-Protokoll liegt außerhalb des zulässigen Bereichs.       |
|Gerät-zu-Cloud-Nachrichten im **MQTT**-Protokoll    | Die Anzahl von Gerät-zu-Cloud-Nachrichten im **MQTT**-Protokoll liegt außerhalb des zulässigen Bereichs.        |
|Direkte Methodenaufrufe     |  Die Anzahl direkter Methodenaufrufe liegt außerhalb des zulässigen Bereichs.       |
|Abgelehnte Cloud-zu-Gerät-Nachrichten im **MQTT**-Protokoll     |   Die Anzahl von abgelehnten Cloud-zu-Gerät-Nachrichten im **MQTT**-Protokoll liegt außerhalb des zulässigen Bereichs.      |
|Updates an Zwillingsmodulen     |  Die Anzahl von Updates an Zwillingsmodulen liegt außerhalb des zulässigen Bereichs.       |
|Nicht autorisierte Vorgänge    |  Die Anzahl nicht autorisierter Vorgänge liegt außerhalb des zulässigen Bereichs.       |

## <a name="security-module-for-azure-rtos-supported-recommendations"></a>Sicherheitsmodul für Azure RTOS: unterstützte Empfehlungen

### <a name="device-related-recommendations"></a>Gerätebezogene Empfehlungen

|Gerätebezogene Aktivität  |Name der Empfehlung |
|---------|---------|
|Anmeldeinformationen für die Authentifizierung    |  Identische Authentifizierungsinformationen wurden von mehreren Geräten verwendet       |

### <a name="hub-related-recommendations"></a>Hubbezogene Empfehlungen

|IoT Hub-bezogene Aktivität  |Name der Empfehlung |
|---------|---------|
|IP-Filterrichtlinie   |  Die Standard-IP-Filterrichtlinie muss auf **Verweigern** festgelegt sein  |
|IP-Filterregel| Die IP-Filterregel umfasst einen großen IP-Adressbereich.|
|Diagnoseprotokolle|Vorschlag zur Aktivierung von Diagnoseprotokollen in IoT Hub|

### <a name="all-defender-for-iot-alerts-and-recommendations"></a>Alle Defender für IoT-Warnungen und -Empfehlungen

Eine vollständige Liste aller Warnungen und Empfehlungen des Defender für IoT-Diensts finden Sie unter IoT-[Sicherheitswarnungen](concept-security-alerts.md), IoT-Sicherheits[empfehlungen](concept-recommendations.md).

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Sicherheitsmodul für Azure RTOS](quickstart-azure-rtos-security-module.md)
- [Konfigurieren und Anpassen des Sicherheitsmoduls für Azure RTOS](how-to-azure-rtos-security-module.md)
- Lesen Sie in [Sicherheitsmodul für Azure RTOS-API](azure-rtos-security-module-api.md) nach.