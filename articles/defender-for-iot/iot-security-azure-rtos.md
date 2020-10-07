---
title: 'Sicherheitsmodul für Azure RTOS: Übersicht'
description: Erfahren Sie mehr über die Unterstützung und Implementierung des Sicherheitsmoduls für Azure RTOS als Teil des Defender für IoT-Diensts.
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
ms.openlocfilehash: 22bd12bbdcccef2fd0e9010f926cd18e95d42967
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91761854"
---
# <a name="overview-defender-for-iot-security-module-for-azure-rtos-preview"></a>Übersicht: Defender für IoT-Sicherheitsmodul für Azure RTOS (Vorschau)

Das Defender für IoT-Sicherheitsmodul für RTOS bietet eine umfassende Sicherheitslösung für Azure RTOS-Geräte. Azure RTOS wird nun in das Azure IoT-Sicherheitsmodul integriert ausgeliefert und bietet Abdeckung für häufige Bedrohungen und potenzielle bösartige Aktivitäten auf Echtzeit-Betriebssystemgeräten. 

![Defender für IoT Azure RTOS](./media/architecture/azure-rtos-security-monitoring.png)


Sicherheitsmodul für Azure RTOS bietet die folgenden Funktionen: 
- Erkennung schädlicher Netzwerkaktivitäten
- Auf benutzerdefinierten Warnungen basierende Basislinienüberwachung für Geräteverhalten
- Verbesserung der Gerätesicherheit

## <a name="detection-of-malicious-network-activities"></a>Erkennung schädlicher Netzwerkaktivitäten

Die eingehenden und ausgehenden Netzwerkaktivitäten jedes Geräts werden überwacht (unterstützte Protokolle: TCP, UDP, ICMP auf IPv4 und IPv6). Defender für IoT überprüft jede dieser Netzwerkaktivitäten anhand des Microsoft Threat Intelligence-Feeds. Der Feed wird in Echtzeit mit Millionen eindeutiger Bedrohungsindikatoren aktualisiert, die weltweit gesammelt werden. 

## <a name="device-behavior-baselining-based-on-custom-alerts"></a>Auf benutzerdefinierten Warnungen basierende Basislinienüberwachung für Geräteverhalten

Die Basislinienüberwachung ermöglicht das Clustering von Geräten in Sicherheitsgruppen und das Festlegen des erwarteten Verhaltens jeder Gruppe. Da IoT-Geräte in der Regel in gut definierten und begrenzten Szenarios betrieben werden, ist es einfach, eine Basislinie zu erstellen, die das erwartete Verhalten mithilfe eines Satzes von Parametern definiert. Jede Abweichung von der Basislinie löst eine Warnung aus. 

## <a name="improve-your-device-security-hygiene"></a>Verbesserung der Gerätesicherheit

Wenn Sie die empfohlene Infrastruktur nutzen, die von Defender für IoT bereitgestellt wird, erhalten Sie Informationen und Einblicke in die Probleme in Ihrer Umgebung, die sich auf die Sicherheit Ihrer Geräte auswirken und diese schädigen. Eine schwache IoT-Gerätesicherheit kann dazu führen, dass ohne eine Änderung potenzielle Angriffe erfolgreich sind, da Sicherheit immer am schwächsten Glied innerhalb einer Organisation gemessen wird. 

## <a name="get-started-protecting-azure-rtos-devices"></a>Erste Schritte zum Schutz von Azure RTOS-Geräten

Das Sicherheitsmodul für Azure RTOS steht als kostenloser Download für Ihre Geräte zur Verfügung. Der Defender für IoT-Clouddienst ist mit einer 30-tägigen Testversion pro Azure-Abonnement verfügbar. Laden Sie das [Sicherheitsmodul für Azure RTOS](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/defender-for-iot/iot-security-azure-rtos.md) herunter, um loszulegen. 


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen über das Sicherheitsmodul für den Azure RTOS-Dienst erhalten. Weitere Informationen zu dem Sicherheitsmodul und den ersten Schritten damit finden Sie in den folgenden Artikeln:

- [Konzepte des IoT-Sicherheitsmoduls für Azure RTOS](concept-rtos-security-module.md)
- [Schnellstart: IoT-Sicherheitsmodul für Azure RTOS](quickstart-azure-rtos-security-module.md)
