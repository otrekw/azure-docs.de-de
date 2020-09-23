---
title: 'Sicherheitsmodul für Azure RTOS: Übersicht'
description: Erfahren Sie mehr über die Unterstützung und Implementierung des Sicherheitsmoduls für Azure RTOS als Teil des Azure Security Center für IoT-Diensts.
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
ms.date: 09/07/2020
ms.author: mlottner
ms.openlocfilehash: bcf98f503b0f0c8bf76435d9884297d5f53d65e6
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514474"
---
# <a name="overview-security-module-for-azure-rtos-preview"></a>Übersicht: Sicherheitsmodul für Azure RTOS (Vorschau)

Das RTOS-Sicherheitsmodul für Azure Security Center für IoT bietet eine umfassende Sicherheitslösung für Azure RTOS-Geräte. Azure RTOS wird nun in das Azure IoT-Sicherheitsmodul integriert ausgeliefert und bietet Abdeckung für häufige Bedrohungen und potenzielle bösartige Aktivitäten auf Echtzeit-Betriebssystemgeräten. 

![Azure Security Center für IoT – Azure RTOS](./media/architecture/azure-rtos-security-monitoring.png)


Sicherheitsmodul für Azure RTOS bietet die folgenden Funktionen: 
- Erkennung schädlicher Netzwerkaktivitäten
- Auf benutzerdefinierten Warnungen basierende Basislinienüberwachung für Geräteverhalten
- Verbesserung der Gerätesicherheit

### <a name="detection-of-malicious-network-activities"></a>Erkennung schädlicher Netzwerkaktivitäten

Die eingehenden und ausgehenden Netzwerkaktivitäten jedes Geräts werden überwacht (unterstützte Protokolle: TCP, UDP, ICMP auf IPv4 und IPv6). Azure Security Center für IoT überprüft jede dieser Netzwerkaktivitäten anhand des Microsoft Threat Intelligence-Feeds. Der Feed wird in Echtzeit mit Millionen eindeutiger Bedrohungsindikatoren aktualisiert, die weltweit gesammelt werden. 

### <a name="device-behavior-baselining-based-on-custom-alerts"></a>Auf benutzerdefinierten Warnungen basierende Basislinienüberwachung für Geräteverhalten

Die Basislinienüberwachung ermöglicht das Clustering von Geräten in Sicherheitsgruppen und das Festlegen des erwarteten Verhaltens jeder Gruppe. Da IoT-Geräte in der Regel in gut definierten und begrenzten Szenarios betrieben werden, ist es einfach, eine Basislinie zu erstellen, die das erwartete Verhalten mithilfe eines Satzes von Parametern definiert. Jede Abweichung von der Basislinie löst eine Warnung aus. 

### <a name="improve-your-device-security-hygiene"></a>Verbesserung der Gerätesicherheit

Wenn Sie die empfohlene Infrastruktur nutzen, die von Azure Security Center für IoT bereitgestellt wird, erhalten Sie Informationen und Einblicke in die Probleme in Ihrer Umgebung, die sich auf die Sicherheit Ihrer Geräte auswirken und diese schädigen. Eine schwache IoT-Gerätesicherheit kann dazu führen, dass ohne eine Änderung potenzielle Angriffe erfolgreich sind, da Sicherheit immer am schwächsten Glied innerhalb einer Organisation gemessen wird. 

## <a name="get-started-protecting-azure-rtos-devices"></a>Erste Schritte zum Schutz von Azure RTOS-Geräten

Das Sicherheitsmodul für Azure RTOS steht als kostenloser Download für Ihre Geräte zur Verfügung. Der Azure Security Center für IoT-Clouddienst ist mit einer 30-tägigen Testversion pro Azure-Abonnement verfügbar. Laden Sie das [Sicherheitsmodul für Azure RTOS](https://github.com/azure-rtos/iot-security-module-preview) herunter, um loszulegen. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen über das Sicherheitsmodul für den Azure RTOS-Dienst erhalten. Weitere Informationen zu dem Sicherheitsmodul und den ersten Schritten damit finden Sie in den folgenden Artikeln:

- [Konzepte des IoT-Sicherheitsmoduls für Azure RTOS](concept-rtos-security-module.md)
- [Schnellstart: IoT-Sicherheitsmodul für Azure RTOS](quickstart-azure-rtos-security-module.md)


