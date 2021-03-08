---
title: Microsoft 365 Defender-Integration in Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie die Integration von Microsoft 365 Defender in Azure Sentinel Ihnen die Möglichkeit eröffnet, Azure Sentinel als universelle Warteschlange für Vorfälle zu verwenden und gleichzeitig die Stärken von M365D zu nutzen, um bei der Untersuchung von M365-Sicherheitsvorfällen zu helfen, und informieren Sie sich, wie Sie die erweiterten Huntingdaten der Defender-Komponenten in Azure Sentinel erfassen.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: abace18db51a7a571ecc66d50253277fbd2296d3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743496"
---
# <a name="microsoft-365-defender-integration-with-azure-sentinel"></a>Microsoft 365 Defender-Integration in Azure Sentinel

> [!IMPORTANT]
> Der Microsoft 365 Defender-Connector ist derzeit als **VORSCHAU** verfügbar. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

> [!IMPORTANT]
>
> **Microsoft 365 Defender** wurde bisher als **Microsoft Threat Protection** oder **MTP** bezeichnet.
>
> **Microsoft Defender for Endpoint** wurde bisher als **Microsoft Defender Advanced Threat Protection** oder **MDATP** bezeichnet.
>
> Die alten Namen werden möglicherweise eine Zeit lang weiterhin verwendet.

## <a name="incident-integration"></a>Vorfallsintegration

Die Vorfallsintegration [Microsoft 365 Defender (M365D)](/microsoft-365/security/mtp/microsoft-threat-protection) von Azure Sentinel ermöglicht es Ihnen, alle M365D-Vorfälle in Azure Sentinel zu streamen und sie zwischen beiden Portalen synchron zu halten. Vorfälle aus M365D (früher bekannt als Microsoft Threat Protection oder MTP) enthalten alle zugehörigen Warnungen, Entitäten und relevanten Informationen, die Ihnen genügend Kontext bieten, um Triage und erste Untersuchungen in Azure Sentinel durchzuführen. Sobald sie in Sentinel enthalten sind, bleiben Vorfälle bidirektional mit M365D synchronisiert, sodass Sie die Vorteile beider Portale bei Ihrer Vorfallsuntersuchung nutzen können.

Diese Integration ermöglicht die Verwaltung von Microsoft 365-Sicherheitsvorfällen in Azure Sentinel als Teil der primären Warteschlange für Vorfälle im gesamten Unternehmen, sodass Sie M365-Vorfälle zusammen mit Vorfällen aus allen anderen Cloud- und lokalen Systemen sehen und korrelieren können. Gleichzeitig können Sie die einzigartigen Stärken und Möglichkeiten von M365D für tiefgreifende Untersuchungen und eine M365-spezifische Benutzererfahrung im gesamten M365-Ökosystem nutzen. M365 Defender ergänzt und gruppiert Warnungen aus mehreren M365-Produkten, wodurch sowohl die Größe der Warteschlange für Vorfälle im SOC reduziert als auch die Zeit bis zur Lösung des Problems verkürzt wird. Die folgenden Komponentendienste sind Teil des M365 Defender-Stapels:

- **Microsoft Defender für Endpunkt** (MDE, vormals MDATP)
- **Microsoft Defender for Identity** (MDI, vormals AATP)
- **Microsoft Defender für Office 365** (MDO, vormals O365ATP)
- **Microsoft Cloud App Security (MCAS)**

Zusätzlich zum Erfassen von Warnungen von diesen Komponenten generiert M365 Defender eigene Warnungen. Die Anwendung erstellt Vorfälle aus allen diesen Warnungen und sendet sie an Azure Sentinel.

### <a name="common-use-cases-and-scenarios"></a>Gängige Anwendungsfälle und -szenarien

- One-Click-Verbindung von M365 Defender-Vorfällen, einschließlich aller Warnungen und Entitäten aus M365 Defender-Komponenten, in Azure Sentinel.

- Bidirektionale Synchronisierung zwischen Sentinel- und M365D-Vorfällen bezüglich Status, Besitzer und Schließungsgrund.

- Nutzen der Funktionen zur Gruppierung und Anreicherung von M365 Defender-Warnungen in Azure Sentinel, wodurch sich die Zeit bis zur Lösung des Problems verkürzt.

- In-Context Deep Link zwischen einem Azure Sentinel-Vorfall und seinem parallelen M365 Defender-Vorfall, um Untersuchungen über beide Portale hinweg zu erleichtern.

### <a name="connecting-to-microsoft-365-defender"></a>Herstellen einer Verbindung mit Microsoft 365 Defender

Sobald Sie den Microsoft 365 Defender-Datenconnector zum [Erfassen von Vorfällen und Warnungen](connect-microsoft-365-defender.md) aktiviert haben, werden M365D-Vorfälle in der Azure Sentinel-Vorfallswarteschlange mit **Microsoft 365 Defender** im Feld **Produktname** angezeigt, kurz nachdem sie in M365 Defender generiert wurden.
- Es kann bis zu 10 Minuten ab dem Zeitpunkt dauern, an dem ein Vorfall, der in M365 Defender generiert wird, in Azure Sentinel angezeigt wird.

- Vorfälle werden ohne zusätzliche Kosten erfasst und synchronisiert.

Sobald die M365 Defender-Integration verbunden ist, werden alle Komponentenwarnungsconnectors (MDE, MDI, MDO, MCAS) automatisch im Hintergrund verbunden, wenn dies nicht bereits geschehen ist. Wenn nach der Verbindung von M365 Defender Komponentenlizenzen erworben wurden, werden die Warnungen und Vorfälle aus dem neuen Produkt weiterhin ohne zusätzliche Konfiguration oder Gebühren an Azure Sentinel weitergeleitet.

### <a name="m365-defender-incidents-and-microsoft-incident-creation-rules"></a>M365 Defender-Vorfälle und Microsoft-Regeln für die Erstellung von Vorfällen

- Vorfälle, die von M365 Defender generiert werden, werden auf der Basis von Warnungen, die aus M365-Sicherheitsprodukten stammen, mithilfe von benutzerdefinierter M365-Logik erstellt.

- Die Microsoft-Regeln für die Erstellung von Vorfällen in Azure Sentinel erstellen ebenfalls Vorfälle aus denselben Warnungen, wobei eine (andere) benutzerdefinierte Azure Sentinel-Logik verwendet wird.

- Die gemeinsame Verwendung beider Mechanismen wird vollständig unterstützt, und diese Konfiguration kann verwendet werden, um den Übergang zur neuen M365 Defender-Logik für die Erstellung von Vorfällen zu erleichtern. Dies führt jedoch zu **duplizierten Ereignissen** für dieselben Warnungen.

- Um die Erstellung doppelter Vorfälle für dieselben Warnungen zu vermeiden, empfehlen wir Kunden, alle **Microsoft-Vorfallerstellungsregeln** für M365-Produkte (MDE, MDI und MDO – siehe MCAS unten) zu deaktivieren, wenn sie M365 Defender verbinden. Markieren Sie hierzu das entsprechende Kontrollkästchen auf der Seite „Connector“. Beachten Sie, dass in diesem Fall alle Filter, die durch die Regeln zur Vorfallerstellung angewendet wurden, nicht auf die M365 Defender-Vorfallsintegration angewendet werden.

- Für Microsoft Cloud App Security-Warnungen (MCAS-Warnungen) werden derzeit nicht alle Warnungstypen in M365 Defender übernommen. Um sicherzustellen, dass Sie weiterhin Vorfälle für alle MCAS-Warnungen erhalten, müssen Sie die **Microsoft-Regeln für die Erstellung von Vorfällen** für die Warnungstypen beibehalten oder erstellen, die *nicht in M365D übernommen werden*.

### <a name="working-with-m365-defender-incidents-in-azure-sentinel-and-bi-directional-sync"></a>Arbeiten mit M365 Defender-Vorfällen in Azure Sentinel und bidirektionale Synchronisierung

M365 Defender-Vorfälle werden in der Azure Sentinel-Vorfallswarteschlange mit dem Produktnamen **Microsoft 365 Defender** und mit ähnlichen Details und Funktionen wie alle anderen Sentinel-Vorfälle angezeigt. Jeder Vorfall enthält einen Link, der zurück zum parallelen Vorfall im M365 Defender-Portal führt.

Wenn sich der Vorfall in M365 Defender weiterentwickelt und weitere Warnungen oder Entitäten hinzugefügt werden, wird der Azure Sentinel-Vorfall entsprechend aktualisiert.

Änderungen am Status, am Schließungsgrund oder an der Zuweisung eines M365-Vorfalls, die entweder in M365D oder in Azure Sentinel vorgenommen werden, werden auch in der Warteschlange des jeweils anderen Vorfalls entsprechend aktualisiert. Die Synchronisierung erfolgt in beiden Portalen ohne Verzögerung sofort nach der Änderung des Vorfalls. Möglicherweise ist eine Aktualisierung erforderlich, um die neuesten Änderungen anzuzeigen.

In M365 Defender können alle Warnungen aus einem Vorfall in einen anderen übertragen werden, was zu einer Zusammenführung der Vorfälle führt. Wenn dies geschieht, spiegeln die Azure Sentinel-Vorfälle die Änderungen wider. Ein Vorfall enthält alle Warnungen der beiden ursprünglichen Vorfälle, und der andere Vorfall wird automatisch geschlossen und mit dem Tag „umgeleitet“ versehen.

> [!NOTE]
> Vorfälle in Azure Sentinel können maximal 150 Warnungen enthalten. M365D Vorfälle können eine größere Anzahl aufweisen. Wenn ein M365D-Vorfall mit mehr als 150 Warnungen mit Azure Sentinel synchronisiert wird, wird der Sentinel-Vorfall mit „150+“ Warnungen angezeigt und bietet einen Link zum parallelen Vorfall in M365D, wo Sie den vollständigen Satz von Warnungen anzeigen können.

## <a name="advanced-hunting-event-collection"></a>Erfassung erweiterter Huntingereignisse

Mit dem Microsoft 365 Defender-Connector können Sie auch **erweiterte Huntingereignisse** (eine Art von Ereignisrohdaten) aus Microsoft 365 Defender und den zugehörigen Komponentendiensten in Azure Sentinel streamen. Sie können derzeit [erweiterte Huntingereignisse](/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview) von [Microsoft Defender für Endpunkt (MDATP)](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) erfassen und diese direkt in eigens erstellte Tabellen in Ihrem Azure Sentinel-Arbeitsbereich streamen. Diese Tabellen bauen auf dem gleichen Schema auf, das im Microsoft 365 Defender-Portal verwendet wird, wodurch Sie vollständigen Zugriff auf die gesamte Reihe erweiterter Huntingereignisse erhalten und Folgendes erreichen können:

- Einfaches Kopieren Ihrer vorhandenen erweiterten Huntingabfragen für Microsoft Defender für Endpunkt in Azure Sentinel.

- Verwenden der Ereignisrohprotokolle zum Gewinnen zusätzlicher Erkenntnisse für Ihre Warnungen, Hunting und Untersuchungen sowie Korrelieren dieser Ereignisse mit anderen Ereignissen aus zusätzlichen Datenquellen in Azure Sentinel.

- Speichern der Protokolle mit verlängerter Aufbewahrungsdauer, jenseits der Standardaufbewahrungsdauer von 30 Tagen in Microsoft Defender for Endpoint oder Microsoft 365 Defender. Dies erreichen Sie durch Konfigurieren der Aufbewahrungsdauer Ihres Arbeitsbereichs oder der tabellenweisen Aufbewahrung in Log Analytics.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie von der Verwendung von Microsoft 365 Defender zusammen mit Azure Sentinel profitieren können, indem Sie den Microsoft 365 Defender-Connector verwenden.

- Rufen Sie Anweisungen zum [Aktivieren des Microsoft 365 Defender-Connectors](connect-microsoft-365-defender.md) ab.
- Erstellen Sie [benutzerdefinierte Warnungen](tutorial-detect-threats-custom.md), und [untersuchen Sie Vorfälle](tutorial-investigate-cases.md).
