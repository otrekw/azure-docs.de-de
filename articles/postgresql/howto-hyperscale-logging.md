---
title: 'Protokolle: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: 'Zugreifen auf Datenbankprotokolle für Azure Database for PostgreSQL: Hyperscale (Citus)'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: ca3cc2873fbc6db72b10c80daecddf1471e30ff4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100577068"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>Protokolle in Azure Database for PostgreSQL: Hyperscale (Citus)

PostgreSQL-Protokolle sind auf jedem Knoten einer Hyperscale-Servergruppe (Citus) verfügbar. Sie können Protokolle an einen Speicherserver oder einen Analysedienst senden. Die Protokolle dienen zur Identifizierung, Behebung und Reparatur von Konfigurationsfehlern und suboptimaler Leistung.

## <a name="accessing-logs"></a>Zugreifen auf Protokolle

Öffnen Sie den Knoten im Azure-Portal, um auf PostgreSQL-Protokolle für einen Hyperscale-Koordinator- (Citus) oder -Workerknoten zuzugreifen:

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="Liste der Knoten":::

Öffnen Sie für den ausgewählten Knoten die **Diagnoseeinstellungen**, und klicken Sie auf **+ Diagnoseeinstellung hinzufügen**.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="Schaltfläche „Diagnoseeinstellungen hinzufügen“":::

Wählen Sie einen Namen für die neuen Diagnoseeinstellungen aus, und aktivieren Sie das Feld **PostgreSQLLogs**.  Wählen Sie aus, welche(s) Ziel(e) die Protokolle empfangen sollen.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="PostgreSQL-Protokolle auswählen":::

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit Log Analytics-Abfragen](../azure-monitor/logs/log-analytics-tutorial.md)
- Weitere Informationen zu [Azure Event Hubs](../event-hubs/event-hubs-about.md)