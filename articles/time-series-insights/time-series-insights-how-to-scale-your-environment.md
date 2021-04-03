---
title: Skalieren der Umgebung – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Azure Time Series Insights-Umgebung über das Azure-Portal skalieren.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 09/29/2020
ms.custom: seodec18
ms.openlocfilehash: 0dadf523c5d17ffb91f4fefa71b52d1d1855c978
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91570226"
---
# <a name="how-to-scale-your-azure-time-series-insights-gen1-environment"></a>Vorgehensweise zur Skalierung Ihrer Azure Time Series Insights Gen1-Umgebung

> [!CAUTION]
> Dies ist ein Artikel zu Azure Time Series Insights Gen1.

In diesem Artikel erfahren Sie, wie Sie die Kapazität Ihrer Azure Time Series Insights-Umgebung über das [Azure-Portal](https://portal.azure.com) ändern. Die Kapazität ist der Multiplikator, der auf die Erfassungsrate, Speicherkapazität und Kosten der gewählten SKU angewendet wird.

Über das Azure-Portal können Sie die Kapazität innerhalb einer bestimmten Preis-SKU erhöhen oder verringern.

Die Tarif-SKU darf hingegen nicht geändert werden. Eine Umgebung mit der SKU „S1“ kann also beispielsweise nicht in eine S2-Umgebung konvertiert werden (und umgekehrt).

## <a name="ga-limits"></a>Einschränkungen der allgemeinen Verfügbarkeit

[!INCLUDE [Azure Time Series Insights GA limits](../../includes/time-series-insights-ga-limits.md)]

## <a name="change-the-capacity-of-your-environment"></a>Ändern der Kapazität Ihrer Umgebung

1. Navigieren Sie im Azure-Portal zu Ihrer Azure Time Series Insights-Umgebung, und wählen Sie sie aus.

1. Wählen Sie im Menü für Ihre Azure Time Series Insights-Umgebung die Option **Speicherkonfiguration** aus.

   [![Konfigurieren Ihrer Azure Time Series Insights-Kapazität](media/scale-your-environment/scale-your-environment-configure.png)](media/scale-your-environment/scale-your-environment-configure.png#lightbox)

1. Passen Sie die Kapazität mithilfe des Schiebereglers **Kapazität** an Ihre Anforderungen in den Bereichen Erfassungsrate und Speicherkapazität an. **Erfassungsrate**, **Speicherkapazität** und **voraussichtliche Kosten** werden dynamisch aktualisiert, um die Auswirkungen der Änderung zu zeigen.

   [![Konfigurieren Ihrer Umgebung mit dem Kapazitätsschieberegler](media/scale-your-environment/scale-your-environment-slider.png)](media/scale-your-environment/scale-your-environment-slider.png#lightbox)

   Alternativ können Sie die Zahl für den Kapazitätsmultiplikator in das Textfeld rechts neben dem Schieberegler eingeben.

1. Klicken Sie auf **Speichern**, um die Umgebung zu skalieren. Die Statusanzeige wird angezeigt, bis die Änderung committet wurde.

1. Überprüfen, ob die neue Kapazität [ausreichend ist, um eine Drosselung zu verhindern](time-series-insights-diagnose-and-solve-problems.md)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Grundlagen der Datenaufbewahrung in Azure Time Series Insights](time-series-insights-concepts-retention.md).

- Machen Sie sich mit dem [Konfigurieren der Datenaufbewahrung in Azure Time Series Insights](time-series-insights-how-to-configure-retention.md) vertraut.

- Informieren Sie sich über die [Planung Ihrer Umgebung](time-series-insights-environment-planning.md).
