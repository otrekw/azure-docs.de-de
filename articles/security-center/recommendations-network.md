---
title: Azure Security Center-Empfehlungen für Netzwerke
description: In diesem Artikel werden die Azure Security Center-Sicherheitsempfehlungen aufgeführt, die Ihnen beim Schützen Ihrer Netzwerkressourcen helfen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2019
ms.author: memildin
ms.openlocfilehash: 1b8d84286d14949c007d1ece3d089a7606464aaf
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781842"
---
# <a name="network-recommendations---reference-guide"></a>Netzwerkempfehlungen: Referenzhandbuch

Dieser Artikel enthält eine vollständige Liste der Empfehlungen, die Ihnen möglicherweise im Azure Security Center bezüglich der Topologie Ihres Netzwerks und Ihrer Endpunkte mit Internetzugriff angezeigt werden.

[Hier](security-center-network-recommendations.md) finden Sie eine Erläuterung dazu, wie Sie diese Empfehlungen finden und Probleme beheben.

## <a name="network-recommendations"></a>Netzwerkempfehlungen

|Name der Empfehlung|BESCHREIBUNG|severity|Sicherheitsbewertung|Ressourcentyp|
|----|----|----|----|----|----|
|Netzwerksicherheitsgruppen auf Subnetzebene sollten aktiviert werden.|Aktivieren Sie Netzwerksicherheitsgruppen zum Steuern des Netzwerkzugriffs für in Ihren Subnetzen bereitgestellte Ressourcen.|Hoch/Mittel|30|Subnet|
|Virtuellen Computern muss eine Netzwerksicherheitsgruppe zugeordnet sein.|Aktivieren Sie Netzwerksicherheitsgruppen zum Steuern des Netzwerkzugriffs für Ihre virtuellen Computer.|Hoch/Mittel|30|Virtueller Computer|
|Der Zugriff sollte für tolerante Netzwerksicherheitsgruppen mit VMs mit Internetzugriff eingeschränkt werden.|Stärken Sie die Netzwerksicherheitsgruppen Ihrer VMs mit Internetzugriff, indem Sie den Zugriff auf Ihre bestehenden Zulassungsregeln einschränken.|Hoch|20|Virtueller Computer|
|Die Regeln für Webanwendungen in IaaS-Netzwerksicherheitsgruppen sollten verstärkt werden.|Härten Sie die Netzwerksicherheitsgruppe (NSG) Ihrer virtuellen Computer, auf denen Webanwendungen ausgeführt werden, mit NSG-Regeln, die in Bezug auf Webanwendungsports zu tolerant sind.|Hoch|20|Virtueller Computer|
|Der Zugriff auf App Services sollte eingeschränkt werden.|Schränken Sie den Zugriff auf Ihre App Services ein, indem Sie die Netzwerkkonfiguration ändern, um eingehenden Datenverkehr aus Bereichen abzulehnen, die zu weit gefasst sind.|Hoch|10|App Service|
|Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden.|Härten Sie die Netzwerksicherheitsgruppe Ihrer virtuellen Computer, um den Zugriff auf Verwaltungsports einzuschränken.|Hoch|10|Virtueller Computer|
DDoS Protection Standard sollte aktiviert sein.|Schützen Sie virtuelle Netzwerke, die Anwendungen mit öffentlichen IP-Adressen enthalten, indem Sie den DDoS-Schutzdienststandard aktivieren. DDoS-Schutz ermöglicht die Entschärfung von volumetrischen Netzwerk- und Protokollangriffen.|Hoch|10|Virtuelles Netzwerk|
|Die IP-Weiterleitung auf dem virtuellen Computer muss deaktiviert sein.|Aktivieren Sie die IP-Weiterleitung. Wenn die IP-Weiterleitung auf der NIC eines virtuellen Computers aktiviert ist, kann der Computer Datenverkehr empfangen, der für andere Ziele bestimmt ist. Da die IP-Weiterleitung nur selten benötigt wird (z. B. bei Verwendung des virtuellen Computers als virtuelles Netzwerkgerät), sollte dieser Vorgang vom Netzwerksicherheitsteam geprüft werden.|Mittel|10|Virtueller Computer|
|Zugriff auf Webanwendung nur über HTTPS gestatten|Aktivieren Sie Zugriff vom Typ „Nur HTTPS“ für Webanwendungen. Durch die Verwendung von HTTPS wird die Server-/Dienstauthentifizierung sichergestellt, und Daten werden bei der Übertragung vor Abhörangriffen auf der Vermittlungsschicht geschützt.|Mittel|20|Webanwendung|
|Die Just-In-Time-Netzwerkzugriffssteuerung sollte auf virtuelle Computer angewendet werden.|Wenden Sie die Just-In-Time-VM-Zugriffssteuerung (JIT) an, um den Zugriff auf ausgewählte Ports dauerhaft zu sperren und autorisierten Benutzern zu ermöglichen, sie per JIT nur für einen begrenzten Zeitraum zu öffnen.|Hoch|20|Virtueller Computer|
|Zugriff auf Funktions-App nur über HTTPS gestatten|Aktivieren Sie den Zugriff vom Typ „Nur HTTPS“ für Funktions-Apps. Durch die Verwendung von HTTPS wird die Server-/Dienstauthentifizierung sichergestellt, und Daten werden bei der Übertragung vor Abhörangriffen auf der Vermittlungsschicht geschützt.|Mittel|20|Funktionen-App|
|Sichere Übertragung in Speicherkonten sollte aktiviert werden.|Ermöglichen Sie die sichere Übertragung auf Speicherkonten. Sichere Übertragung ist eine Option, die erzwingt, dass Ihr Storage-Konto nur Anforderungen von sicheren Verbindungen (HTTPS) akzeptiert. Durch die Verwendung von HTTPS wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, z. B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking.|Hoch|20|Speicherkonto|


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Empfehlungen für andere Arten von Azure-Ressourcen finden Sie in den folgenden Themen:

* [Überwachen der Identität und des Zugriffs](security-center-identity-access.md)
* [Schützen Ihrer Computer und Anwendungen](security-center-virtual-machine-protection.md)
* [Schützen Ihres Azure SQL-Diensts](security-center-sql-service-recommendations.md)

