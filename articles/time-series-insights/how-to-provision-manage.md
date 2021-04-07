---
title: Verwalten einer Gen2-Umgebung – Azure Time Series | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Azure Time Series Insights Gen2-Umgebung verwalten.
author: shipra1mishra
ms.author: shmishr
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 03/15/2020
ms.custom: seodec18
ms.openlocfilehash: cbd28bdf5318bdaf932447f5d1f936d2c614a7f3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103461894"
---
# <a name="manage-azure-time-series-insights-gen2"></a>Verwalten von Azure Time Series Insights Gen2

Nachdem Sie Ihre Azure Time Series Insights Gen2-Umgebung mithilfe [der Azure CLI](./how-to-create-environment-using-cli.md) oder im [Azure-Portal](./how-to-create-environment-using-portal.md) erstellt haben, können Sie Ihre Zugriffsrichtlinien und andere Umgebungsattribute entsprechend Ihren geschäftlichen Anforderungen ändern.

## <a name="manage-the-environment"></a>Verwalten der Umgebung

Sie können Ihre Azure Time Series Insights Gen2-Umgebung über das [Azure-Portal](https://portal.azure.com/) verwalten. Wenn Sie Ihre Umgebung über das Azure-Portal verwalten, müssen Sie einige wichtige Unterschiede zwischen einer Gen2-Umgebung und Gen1 S1- oder Gen1 S2-Umgebungen berücksichtigen:

* Der Bereich **Übersicht** für Gen2 im Azure-Portal weist die folgenden Änderungen auf:

  * Die Kapazität wird entfernt, da sie nicht für Gen2-Umgebungen gilt.
  * Die Eigenschaft **Time Series-ID** wird hinzugefügt. Sie bestimmt, wie Ihre Daten partitioniert werden.
  * Verweisdatasets wurden entfernt.
  * Die angezeigte URL leitet Sie zum [Azure Time Series Insights-Explorer](./concepts-ux-panels.md) weiter.
  * Der Name Ihres Azure-Speicherkontos wird aufgeführt.

* Der Bereich **Konfigurieren** im Azure-Portal wurde entfernt, weil Skalierungseinheiten nicht für Azure Time Series Insights Gen2-Umgebungen gelten. Sie können jedoch **Speicherkonfiguration** verwenden, um den neu eingeführten warmen Speicher zu konfigurieren.

* Der Bereich **Verweisdaten** im Azure-Portal wurde in Azure Time Series Insights Gen2 entfernt, da das Konzept von Verweisdaten durch das [Zeitreihenmodell (Time Series Model, TSM)](./concepts-model-overview.md) ersetzt wurde.

:::image type="content" source="media/v2-update-manage/create-and-manage-overview-confirm.png" alt-text="Azure Time Series Insights Gen2-Umgebung im Azure-Portal":::

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die Liste mit den [bewährten Methoden für die Streamingerfassung](./concepts-streaming-ingestion-event-sources.md#streaming-ingestion-best-practices) an
* Machen Sie sich mit [Diagnose und Problembehandlung](./how-to-diagnose-troubleshoot.md) vertraut
