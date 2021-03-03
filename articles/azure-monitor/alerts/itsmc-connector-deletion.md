---
title: Löschen des ITSM-Connectors und der zugeordneten Aktionen
description: In diesem Artikel wird erläutert, wie Sie den ITSM-Connector und die zugehörigen Aktionsgruppen löschen.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: 1fbaa48d104642984e604bc66a3aa914fbfed44c
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601787"
---
# <a name="deletion-of-unused-itsm-connectors"></a>Löschen nicht verwendeter ITSM-Connectors

Der Löschvorgang für nicht verwendete Connectors umfasst zwei Phasen:

1. Löschen der zugeordneten Aktionen: Alle Aktionen, die dem ITSM-Connector zugeordnet sind, sollten gelöscht werden. Dies sollte erfolgen, damit nicht durch Aktionen ohne Connectors Fehler in Ihrem Abonnement verursacht werden.

2. Löschen des nicht verwendeten ITSM-Connectors

## <a name="deletion-of-the-associated-actions"></a>Löschen der zugeordneten Aktionen

1. Um die Aktionsgruppe zu finden, navigieren Sie zu „Monitor“. ![Screenshot der Auswahl von „Monitor“](media/itsmc-connector-deletion/itsmc-monitor-selection.png)

2. Wählen Sie „Warnungen“ aus. ![Screenshot der Auswahl von „Warnungen“](media/itsmc-connector-deletion/itsmc-alert-selection.png)
3. Wählen Sie „Aktionen verwalten“ aus. ![Screenshot der Auswahl von „Aktionen verwalten“](media/itsmc-connector-deletion/itsmc-actions-selection.png)
4. Wählen Sie alle ITSM-Connectors aus, die mit Cherwell verbunden sind. ![Screenshot der mit Cherwell verbundenen ITSM-Connectors](media/itsmc-connector-deletion/itsmc-actions-screen.png)
5. Löschen Sie die Aktionsgruppe. ![Screenshot der Löschung der Aktionsgruppe](media/itsmc-connector-deletion/itsmc-action-deletion.png)

## <a name="deletion-of-the-unused-itsm-connector"></a>Löschen des nicht verwendeten ITSM-Connectors

1. Suchen Sie in LA auf der Suchleiste oben nach „ServiceDesk“, und wählen Sie die Option aus. ![Screenshot der Suche und Auswahl von „ServiceDesk“ in LA](media/itsmc-connector-deletion/itsmc-connector-selection.png)
2. Wählen Sie die ITSM-Verbindungen und dann den Cherwell-Connector aus. ![Screenshot der Cherwell-ITSM-Connectors](media/itsmc-connector-deletion/itsmc-cherwell-connector.png)
3. Wählen Sie „Löschen“ aus. ![Screenshot der Löschung des ITSM-Connectors](media/itsmc-connector-deletion/itsmc-connector-deletion.png)

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)
