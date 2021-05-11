---
title: 'Azure Peering Service: Zugreifen auf Verbindungstelemetrie '
description: In diesem Tutorial lernen Sie, wie Sie auf Verbindungstelemetrie zugreifen.
services: peering-service
author: gthareja
ms.service: peering-service
ms.topic: tutorial
ms.date: 04/06/2021
ms.author: gatharej
ms.openlocfilehash: 2b019596c87ef3beca1ff26a9366250d188bdfbc
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108202675"
---
# <a name="tutorial-accessing-peering-service-connection-telemetry"></a>Tutorial: Zugreifen auf Peering Service-Verbindungstelemetrie

 In diesem Tutorial erfahren Sie, wie Sie auf die Telemetrie für Ihre Peering Service-Verbindungen zugreifen.
 
 Die Verbindungstelemetrie bietet Einblicke, die für die Konnektivität zwischen dem Standort des Kunden und dem Microsoft-Netzwerk erfasst werden. In diesem Artikel erfahren Sie, wie Sie den Latenzbericht und Präfixstatus für eine bestimmte Azure Peering Service-Verbindung anzeigen. 

Zum Zugreifen auf Peering Service-Verbindungstelemetrie müssen Sie eine Peering Service-Verbindung im Azure-Portal erstellen. Informationen zum Erstellen einer Verbindung finden Sie unter [Erstellen einer Peering Service-Verbindung – Azure-Portal](azure-portal.md).


## <a name="view-a-latency-report"></a>Anzeigen eines Latenzberichts

Führen Sie zum Anzeigen eines Latenzberichts für eine bestimmte Peering Service-Verbindung die folgenden Schritte aus.

1. Wählen Sie im linken Bereich **Alle Ressourcen** und dann die Peering Service-Verbindung aus. Wählen Sie dann unter **Präfixe** die Option **Öffnen** aus. 

   ![Auswählen der Peering Service-Verbindung](./media/peering-service-measure/peering-service-measure-menu.png)

2. Eine Latenzberichtseite für alle Präfixe, die dieser Peering Service-Verbindung zugeordnet sind, wird angezeigt. **Die Peering Service-Verbindungen unterstützen Latenzdaten nur für das Präfix „/24“ oder größere Präfixe.**

      ![Seite „Latenzbericht“](./media/peering-service-measure/peering-service-latency-report.png)

3. Der Bericht wird standardmäßig für jede Stunde aktualisiert, die auf dieser Seite angezeigt wird. Wenn Sie den Bericht für andere Zeitachsen anzeigen möchten, wählen Sie die entsprechende Option aus **Datenanzeige für die letzten:** aus. 

## <a name="view-prefix-state-report"></a>Anzeigen des Präfixstatusberichts

1. Wenn Sie die Ereignisse für ein bestimmtes Präfix anzeigen möchten, wählen Sie dessen Namen und dann im linken Bereich **Präfixereignisse** aus. Die erfassten Ereignisse werden angezeigt.


   ![Präfixereignisse](./media/peering-service-measure/peering-service-prefix-event.png)

 Einige der möglichen Ereignisse, die in der Liste **Präfixereignisse** erfasst werden, werden hier gezeigt.

| **Präfixereignisse** | **Ereignistyp**|**Erläuterung**|
|-----------|---------|---------|
| PrefixAnnouncementEvent |Information|Eine Präfixankündigung wurde empfangen.|
| PrefixWithdrawalEvent|Warnung| Eine Zurücknahme des Präfixes wurde empfangen. |
| PrefixBackupRouteAnnouncementEvent |Information|Eine Ankündigung der Präfixsicherungsroute wurde empfangen. |
| PrefixBackupRouteWithdrawalEvent|Warnung|Eine Zurücknahme der Präfixsicherungsroute wurde empfangen. |
| PrefixActivePath |Information| Aktuelle aktive Präfixroute   |
| PrefixBackupPath | Information|Aktuelle Präfixsicherungsroute   |
| PrefixOriginAsChangeEvent|Kritisch| Ein exaktes Präfix wurde mit anderer ursprünglicher autonomer Systemnummer (für aktive Route) empfangen.| 
| PrefixBackupRouteOriginAsChangeEvent  | Fehler|Ein Präfix wurde mit anderer ursprünglicher autonomer Systemnummer (für Sicherungsroute) empfangen.  |

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Peering Service-Verbindung finden Sie unter [Peering Service-Verbindung](connection.md).
- Informationen zur Peering Service-Verbindungstelemetrie finden Sie unter [Peering Service-Verbindungstelemetrie](connection-telemetry.md).
