---
title: Azure Defender für IoT
description: Informationen zu Azure Defender für IoT
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 2f41aac085fef62e0356a93a07823f21d7ba3667
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91448344"
---
# <a name="introduction-to-azure-defender-for-iot"></a>Einführung in Azure Defender für IoT

Vereinheitlichen Sie die Sicherheitsverwaltung, und ermöglichen Sie End-to-End-Bedrohungserkennung und -analyse über Hybrid Cloud-Workloads und Ihre Azure IoT-Lösung hinweg.

Azure Defender für IoT bietet:

- **Ressourcenerkennung und Netzwerkzuordnung**, einschließlich Details wie Gerätehersteller, Gerätetyp und die Art der Kommunikation von Geräten im Netzwerk
- **Sicherheitsrisikomanagement**, einschließlich Informationen zu CVEs, geöffneten Ports und nicht autorisierten Internetverbindungen
- **Fortlaufende Bedrohungsüberwachung** mit Echtzeitwarnungen, die auf anomale oder nicht autorisierte Aktivitäten hinweisen, z. B. gezielte Angriffe oder Schadsoftware

Ausführliche Informationen finden Sie in der [dedizierten Dokumentation](https://docs.microsoft.com/azure/asc-for-iot/overview).

## <a name="availability"></a>Verfügbarkeit
|Aspekt|Details|
|----|:----|
|Status des Release:|Allgemein verfügbar (Generally Available, GA)|
|Preise:|Erfordert [Azure Defender](security-center-pricing.md)|
|Erforderliche Rollen und Berechtigungen:|Schreibberechtigungen für die NSGs des Computers|
|Clouds:|![Ja](./media/icons/yes-icon.png) Kommerzielle Clouds<br>![Ja](./media/icons/yes-icon.png) National/Sovereign (US Gov, China Gov, andere Gov)|
|||

## <a name="what-devices-can-azure-defender-for-iot-secure"></a>Welche Geräte können von Azure Defender für IoT geschützt werden?
Durch die Kombination der Funktionen von Security Center, Azure Defender und der CyberX-Technologie ohne Agents können Sie Folgendes schützen:

- **Greenfield-Geräte**, die über IoT Hub verbunden sind. Hiermit können Organisationen ihre IoT-Initiativen beschleunigen, indem sie sowohl moderne, Internet-native Geräte als auch herkömmliche ICS/SCADA-Geräte mit einer einzelnen einheitlichen Lösung schützen.
    - Integrieren Sie neue Geräte, und wenden Sie Sicherheitsrichtlinien auf Ihre Workloads (Blattgeräte, Microsoft Edge-Geräte, IoT Hub) an, um die Einhaltung von Sicherheitsstandards und einen verbesserten Sicherheitsstatus zu gewährleisten.

- **Nicht verwaltete Brownfield-Geräte**, die in OT-Umgebungen (Operational Technology) eingesetzt werden, z. B. Fertigung, Gebäudeverwaltungssystemen (BMS), Biowissenschaften, Energie- und Wasserversorger, Öl und Gas sowie Logistik. 
    - Zum Schutz von nicht verwalteten Geräten verwendet Azure Defender für IoT einen lokalen Sensor für eine passive, nicht invasive Netzwerkverkehrsanalyse (Network Traffic Analysis, NTA), die keinerlei Auswirkungen auf die Leistung von OT-Umgebungen hat. 
    - Außerdem ist ein tief greifendes Verständnis spezieller IoT/OT-Protokolle (in Verbindung mit patentierten, IoT/OT-fähigen Verhaltensanalysen und maschinellem Lernen) integriert, sodass keine Regeln oder Signaturen konfiguriert werden müssen. Dies führt dazu, dass Bereitstellungen normalerweise in Minuten oder Stunden anstelle von Tagen oder Wochen erfolgen. 


## <a name="azure-defender-for-iot-integration-with-azure-sentinel"></a>Integration von Azure Defender für IoT mit Azure Sentinel
Um eine einheitliche IoT/OT-Sicherheitsüberwachung und -Governance zu ermöglichen, kann Azure Defender für IoT nativ mit [Azure Sentinel](../sentinel/overview.md) integriert werden.

SecOps-Teams haben folgende Möglichkeiten:

- Schnelles Erkennen von IoT/OT-Bedrohungen und Reagieren auf diese über in Sentinel enthaltene, OT-spezifische SOAR-Playbooks
- Fortlaufend aktualisierte IoT/OT-spezifische Threat Intelligence von Section 52, dem internen IoT/OT Threat Intelligence-Team von Microsoft
- Integrieren von Azure Defender für IoT in vorhandene SOC-Workflows und Sicherheitstools von Drittanbietern wie Splunk, IBM QRadar und ServiceNow


## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie etwas über Azure Defender für IoT in Azure Security Center erfahren. Weitere Informationen finden Sie unter

- [Einführung in Azure Security Center für IoT](../asc-for-iot/overview.md)
