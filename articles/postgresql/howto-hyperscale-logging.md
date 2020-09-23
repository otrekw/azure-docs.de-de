---
title: 'Protokolle: Hyperscale (Citus) – Azure Database for PostgreSQL'
description: 'Zugreifen auf Datenbankprotokolle für Azure Database for PostgreSQL: Hyperscale (Citus)'
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: 1dc7bc8e119de7c8fdcf09713286be2633457486
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90895875"
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

- [Erste Schritte mit Log Analytics-Abfragen](/azure/azure-monitor/log-query/get-started-portal)
- Weitere Informationen zu [Azure Event Hubs](/azure/event-hubs/event-hubs-about)
