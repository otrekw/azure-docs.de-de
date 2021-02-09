---
title: Wichtige bevorstehende Änderungen an Azure Security Center
description: Bevorstehende Änderungen an Azure Security Center, die Sie beachten sollten und für die Sie ggf. planen müssen
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/25/2021
ms.author: memildin
ms.openlocfilehash: d5de16c8156762a229d6c707080bc197dc206a7c
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99475589"
---
# <a name="important-upcoming-changes-to-azure-security-center"></a>Wichtige bevorstehende Änderungen an Azure Security Center

> [!IMPORTANT]
> Die Informationen auf dieser Seite beziehen sich auf Vorabversionen von Produkten oder Features, die vor einer möglichen Veröffentlichung für den Handel wesentlich verändert werden können. Microsoft gewährt in Bezug auf die hier angegebenen Informationen keine Zusicherungen oder Garantien, weder ausdrücklich noch konkludent.

Auf dieser Seite erfahren Sie, welche Änderungen für Security Center geplant sind. Es werden geplante Änderungen am Produkt beschrieben, die sich auf Dinge wie Ihre Sicherheitsbewertung oder Workflows auswirken könnten.

Die neuesten Versionshinweise finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).


## <a name="planned-changes"></a>Geplante Änderungen

- [Empfehlungen zum Schutz von Kubernetes-Workloads bald allgemein verfügbar](#kubernetes-workload-protection-recommendations-will-soon-be-released-for-general-availability-ga)
- [Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ als veraltet eingestuft](#two-recommendations-from-apply-system-updates-security-control-being-deprecated)
- [Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung](#enhancements-to-sql-data-classification-recommendation)


### <a name="kubernetes-workload-protection-recommendations-will-soon-be-released-for-general-availability-ga"></a>Empfehlungen zum Schutz von Kubernetes-Workloads bald allgemein verfügbar

**Geschätztes Datum für die Änderung:** Februar 2021

Die unter [Schützen Ihrer Kubernetes-Workloads](kubernetes-workload-protections.md) beschriebenen Empfehlungen zum Schutz von Kubernetes-Workloads befinden sich derzeit in der Vorschauphase. Während der Vorschauphase einer Empfehlung werden Ressourcen durch diese Empfehlung nicht in einen fehlerhaften Zustand versetzt, und sie wird nicht in die Berechnungen Ihrer Sicherheitsbewertung einbezogen.

Diese Empfehlungen werden in Kürze allgemein verfügbar gemacht, wodurch sie dann auch *in die Bewertungsberechnung einbezogen werden*. Falls Sie sie noch nicht umgesetzt haben, kann sich dies geringfügig auf Ihre Sicherheitsbewertung auswirken.

Setzen Sie sie nach Möglichkeit um. (Eine entsprechende Anleitung finden Sie unter [Umsetzen von Empfehlungen in Azure Security Center](security-center-remediate-recommendations.md).)

Die Empfehlungen zum Schutz von Kubernetes-Workloads umfassen Folgendes:

- Azure Policy-Add-On für Kubernetes muss auf Ihren Clustern installiert und aktiviert sein
- Für Container müssen CPU- und Arbeitsspeicherlimits erzwungen werden
- Privilegierte Container müssen vermieden werden
- Unveränderliches (schreibgeschütztes) Stammdateisystem für Container erzwingen
- Container mit Rechteausweitung müssen vermieden werden
- Das Ausführen von Containern als Root-Benutzer muss vermieden werden
- Container mit Freigabe sensibler Hostnamespaces vermeiden
- Linux-Funktionen mit den niedrigsten Berechtigungen für Container erzwingen
- Verwendung von HostPath-Volumeeinbindungen von Pods auf eine bekannte Liste beschränken
- Container dürfen nur an zulässigen Ports lauschen
- Dienste dürfen nur an zulässigen Ports lauschen
- Verwendung von Hostnetzwerken und -ports einschränken
- Überschreiben oder Deaktivieren des AppArmor-Profils für Container einschränken
- Containerimages sollten nur von vertrauenswürdigen Registrierungen bereitgestellt werden             

Weitere Informationen zu diesen Empfehlungen finden Sie unter [Schützen Ihrer Kubernetes-Workloads](kubernetes-workload-protections.md).

### <a name="two-recommendations-from-apply-system-updates-security-control-being-deprecated"></a>Zwei Empfehlungen der Sicherheitskontrolle „Systemupdates anwenden“ als veraltet eingestuft 

**Geschätztes Datum für die Änderung:** Februar 2021

Für die beiden folgenden Empfehlungen ist geplant, sie im Februar 2021 als veraltet einzustufen:

- **Ihre Computer sollten neu gestartet werden, damit Systemupdates wirksam werden.** Dies kann zu geringfügigen Auswirkungen auf Ihre Sicherheitsbewertung führen.
- **Der Überwachungs-Agent sollte auf Ihren Computern installiert werden.** Diese Empfehlung gilt nur für lokale Computer. Ein Teil der Logik wird in eine andere Empfehlung übertragen: **Log Analytics-Agent-Integritätsprobleme müssen auf Computern gelöst werden**. Dies kann zu geringfügigen Auswirkungen auf Ihre Sicherheitsbewertung führen.

Wir empfehlen Ihnen, Ihre Konfigurationen für den fortlaufenden Export und die Workflowautomatisierung zu überprüfen, um zu ermitteln, ob diese Empfehlungen darin enthalten sind. Darüber hinaus sollten Sie alle Dashboards oder anderen Überwachungstools, für die diese ggf. genutzt werden, entsprechend aktualisieren.

Weitere Informationen zu diesen Empfehlungen finden Sie auf der [Referenzseite zu Sicherheitsempfehlungen](recommendations-reference.md).


### <a name="enhancements-to-sql-data-classification-recommendation"></a>Verbesserungen für die Empfehlung zur SQL-Datenklassifizierung

**Geschätztes Datum für die Änderung:** Q2 2021

Die aktuelle Version der Empfehlung **Sensible Daten in Ihren SQL-Datenbanken müssen klassifiziert werden** in der Sicherheitskontrolle **Datenklassifizierung anwenden** wird durch eine neue Version ersetzt, die stärker an der Microsoft-Strategie zur Datenklassifizierung ausgerichtet ist. Infolgedessen:

- Die Empfehlung geht nicht mehr in Ihre Sicherheitsbewertung ein.
- Die Sicherheitskontrolle („Datenklassifizierung anwenden“) geht nicht mehr in Ihre Sicherheitsbewertung ein.
- Die ID der Empfehlung ändert sich ebenfalls (derzeit b0df6f56-862d-4730-8597-38c0fd4ebd59).



## <a name="next-steps"></a>Nächste Schritte

Alle aktuellen Änderungen am Produkt finden Sie unter [Neuerungen in Azure Security Center](release-notes.md).
