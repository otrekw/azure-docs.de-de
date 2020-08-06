---
title: Azure RTOS-Unterstützung
description: Erfahren Sie mehr über die Unterstützung von Azure RTOS im Azure Security Center für IoT-Dienst.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2020
ms.author: mlottner
ms.openlocfilehash: af4579d25b94fc12f67cfc91d0f6a5af4b35c810
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094357"
---
# <a name="azure-security-center-for-iot-security-solution-for-azure-rtos"></a>Azure Security Center für IoT-Sicherheitslösung für Azure RTOS 

Das Azure Security Center für IoT-Sicherheitsmodul bietet eine umfassende Sicherheitslösung für Azure RTOS-Geräte. Azure RTOS verfügt über ein integriertes Sicherheitsmodul, das gängige Bedrohungen für Geräte mit Echtzeitbetriebssystem abdeckt. 

![Azure Security Center für IoT – Azure RTOS](./media/architecture/azure-rtos-security-monitoring.png)


Das Azure Security Center für IoT-Sicherheitsmodul mit Azure RTOS-Unterstützung umfasst die folgenden Funktionen: 
- Erkennung schädlicher Netzwerkaktivitäten
- Auf benutzerdefinierten Warnungen basierende Basislinienüberwachung für Geräteverhalten
- Verbesserung der Gerätesicherheit

### <a name="detection-of-malicious-network-activities"></a>Erkennung schädlicher Netzwerkaktivitäten

Die eingehenden und ausgehenden Netzwerkaktivitäten jedes Geräts werden überwacht (unterstützte Protokolle: TCP, UDP, ICMP auf IPv4 und IPv6). Azure Security Center für IoT überprüft jede dieser Netzwerkaktivitäten anhand des Microsoft Threat Intelligence-Feeds. Der Feed wird in Echtzeit mit Millionen eindeutiger Bedrohungsindikatoren aktualisiert, die weltweit gesammelt werden. 

### <a name="device-behavior-baselining-based-on-custom-alerts"></a>Auf benutzerdefinierten Warnungen basierende Basislinienüberwachung für Geräteverhalten

Die Basislinienüberwachung ermöglicht das Clustering von Geräten in Sicherheitsgruppen und das Festlegen des erwarteten Verhaltens jeder Gruppe. Da IoT-Geräte in der Regel in gut definierten und begrenzten Szenarios betrieben werden, ist es einfach, eine Basislinie zu erstellen, die das erwartete Verhalten mithilfe eines Satzes von Parametern definiert. Jede Abweichung von der Basislinie löst eine Warnung aus. 

### <a name="improve-your-device-security-hygiene"></a>Verbesserung der Gerätesicherheit

Wenn Sie die empfohlene Infrastruktur nutzen, die von Azure Security Center für IoT bereitgestellt wird, erhalten Sie Informationen und Einblicke in die Probleme in Ihrer Umgebung, die sich auf die Sicherheit Ihrer Geräte auswirken und diese schädigen. Eine schlechte IoT-Gerätesicherheit kann dazu führen, dass ohne eine Änderung potenzielle Angriffe erfolgreich sind, da Sicherheit immer am schwächsten Glied innerhalb einer Organisation gemessen wird. 

## <a name="get-started-protecting-azure-rtos-devices"></a>Erste Schritte zum Schutz von Azure RTOS-Geräten

- Das Azure Security Center für IoT-Sicherheitsmodul für Azure RTOS steht als kostenloser Download für Ihre Geräte zur Verfügung. Der Azure Security Center für IoT-Clouddienst ist mit einer 30-tägigen Testversion pro Azure-Abonnement verfügbar. Laden Sie das [Azure Security Center für IoT-Sicherheitsmodul für Azure RTOS](https://github.com/azure-rtos/iot-security-module-preview) herunter, um zu beginnen. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie sich über die Azure RTOS-Unterstützung im Azure Security Center für IoT informiert. Weitere Informationen zum Einstieg in Ihre Sicherheitslösung in IoT Hub und zu deren Aktivierung finden Sie in den folgenden Artikeln:

- [ASC für IoT-Voraussetzungen](service-prerequisites.md)
- [Erste Schritte](getting-started.md)
- [Konfigurieren Ihrer IoT-Lösung](quickstart-configure-your-solution.md)
- [Enable security in IoT Hub (Aktivieren der Sicherheit in IoT Hub)](quickstart-onboard-iot-hub.md)
- [Azure Security Center für IoT – Häufig gestellte Fragen (FAQ)](resources-frequently-asked-questions.md)
- [Azure Security Center für IoT – Sicherheitswarnungen](concept-security-alerts.md)
