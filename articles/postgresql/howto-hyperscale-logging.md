---
title: 'Protokolle: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: 'Zugreifen auf Datenbankprotokolle für Azure Database for PostgreSQL: Hyperscale (Citus)'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: 4ca64a7793d229e7910fb122fb33996b1ac0c4fe
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87094971"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>Protokolle in Azure Database for PostgreSQL: Hyperscale (Citus)

PostgreSQL-Protokolle sind auf jedem Knoten einer Hyperscale-Servergruppe (Citus) verfügbar. Sie können Protokolle an einen Speicherserver oder einen Analysedienst senden. Die Protokolle dienen zur Identifizierung, Behebung und Reparatur von Konfigurationsfehlern und suboptimaler Leistung.

## <a name="accessing-logs"></a>Zugreifen auf Protokolle

Öffnen Sie den Knoten im Azure-Portal, um auf PostgreSQL-Protokolle für einen Hyperscale-Koordinator- (Citus) oder -Workerknoten zuzugreifen:

![Liste der Knoten](media/howto-hyperscale-logging/choose-node.png)

Öffnen Sie für den ausgewählten Knoten die **Diagnoseeinstellungen**, und klicken Sie auf **+ Diagnoseeinstellung hinzufügen**.

![Schaltfläche „Diagnoseeinstellungen hinzufügen“](media/howto-hyperscale-logging/diagnostic-settings.png)

Wählen Sie einen Namen für die neuen Diagnoseeinstellungen aus, und aktivieren Sie das Feld **PostgreSQLLogs**.  Wählen Sie aus, welche(s) Ziel(e) die Protokolle empfangen sollen.

![PostgreSQL-Protokolle auswählen](media/howto-hyperscale-logging/diagnostic-create-setting.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Log Analytics-Abfragen](/azure/azure-monitor/log-query/get-started-portal)
- Weitere Informationen zu [Azure Event Hubs](/azure/event-hubs/event-hubs-about)
