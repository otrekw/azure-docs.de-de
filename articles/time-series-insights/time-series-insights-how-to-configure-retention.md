---
title: Konfigurieren der Vermerkdauer in Ihrer Umgebung – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Vermerkdauer in Ihrer Azure Time Series Insights-Umgebung konfigurieren.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: seodec18
ms.openlocfilehash: 524e47e5be142b720687ad48ee9407da23284bd4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81605046"
---
# <a name="configuring-retention-in-time-series-insights"></a>Konfigurieren der Vermerkdauer in Time Series Insights

In diesem Artikel wird beschrieben, wie Sie die **Datenaufbewahrungsdauer** und das **Verhalten bei Überschreitung des Speicherlimits** in Azure Time Series Insights konfigurieren.

## <a name="summary"></a>Zusammenfassung

Jede Azure Time Series Insights-Umgebung verfügt über eine Einstellung zum Konfigurieren der **Datenaufbewahrungsdauer**. Der Wert kann zwischen 1 und 400 Tagen liegen. Die Daten werden anhand der Speicherkapazität der Umgebung oder der Aufbewahrungsdauer (1 - 400) gelöscht, je nachdem, welcher Fall zuerst eintritt.

Jede Time Series Insights-Umgebung verfügt über die zusätzliche Einstellung **Verhalten bei Überschreitung des Speicherlimits**. Diese Einstellung steuert das Verhalten bei Eingang und Bereinigung, wenn in einer Umgebung die maximale Kapazität erreicht ist. Sie können zwischen zwei Verhaltensweisen wählen:

- **Alte Daten bereinigen** (Standard)
- **Eingang anhalten**

Ausführliche Informationen zu einem besseren Verständnis dieser Einstellungen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Konfigurieren der Datenaufbewahrung

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Suchen Sie die vorhandene Time Series Insights-Umgebung. Wählen Sie im Azure-Portal im Menü auf der linken Seite **Alle Ressourcen** aus. Wählen Sie Ihre Time Series Insights-Umgebung aus.

1. Wählen Sie unter der Überschrift **Einstellungen** die Option **Speicherkonfiguration** aus.

    [![Wählen Sie unter „Einstellungen“ die Option „Speicherkonfiguration“ aus](media/data-retention/configure-data-retention.png)](media/data-retention/configure-data-retention.png#lightbox).

1. Wählen Sie die **Datenaufbewahrungsdauer** (in Tagen) aus, um die Vermerkdauer über den Schieberegler oder durch Eingabe einer Zahl in das Textfeld zu konfigurieren.

1. Notieren Sie die Einstellung **Kapazität**, da diese Konfiguration Auswirkungen auf die maximale Anzahl von Datenereignissen und die Gesamtspeicherkapazität für das Speichern von Daten hat.

1. Schalten Sie die Einstellung **Verhalten bei Überschreitung des Speicherlimits** um. Wählen Sie als Verhalten **Alte Daten bereinigen** oder **Eingang anhalten** aus.

    [![Eingang anhalten – Akzeptieren und Speichern](media/data-retention/pause-ingress-accept-and-save.png)](media/data-retention/pause-ingress-accept-and-save.png#lightbox).

1. Lesen Sie die Dokumentation, um die potenziellen Risiken von Datenverlusten zu verstehen. Wählen Sie zum Konfigurieren der Änderungen **Speichern** aus.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Grundlegendes zur Vermerkdauer in Time Series Insights](time-series-insights-concepts-retention.md).

- Machen Sie sich mit der [Vorgehensweise zur Skalierung Ihrer Time Series Insights-Umgebung](time-series-insights-how-to-scale-your-environment.md) vertraut.

- Informieren Sie sich über die [Planung Ihrer Umgebung](time-series-insights-environment-planning.md).
