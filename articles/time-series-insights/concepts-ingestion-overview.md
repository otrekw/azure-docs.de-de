---
title: Übersicht über die Datenerfassung – Azure Time Series Insights | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über die Datenerfassung für Azure Time Series Insights.
author: lyrana
ms.author: lyhughes
manager: deepakpalled
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 06/03/2020
ms.custom: seodec18
ms.openlocfilehash: ecee6817b58639d1b60264c818ab9517e5d133e0
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86049396"
---
# <a name="azure-time-series-insights-data-ingestion-overview"></a>Übersicht über die Azure Time Series Insights-Datenerfassung

Ihre Azure Time Series Insights-Umgebung (TSI) enthält eine *Erfassungs-Engine*, mit der Zeitreihenstreamingdaten gesammelt, verarbeitet und gespeichert werden. Wenn Daten bei der oder den Ereignisquellen eintreffen, verarbeitet und speichert Azure Time Series Insights diese in Quasi-Echtzeit.

[![Übersicht über die Datenerfassung](media/concepts-ingress-overview/ingress-overview.png)](media/concepts-ingress-overview/ingress-overview.png#lightbox)

## <a name="ingestion-topics"></a>Themen zur Erfassung

In den folgenden Artikeln wird die Datenverarbeitung einschließlich der empfohlenen Best Practices ausführlich behandelt:

* Informieren Sie sich über [Ereignisquellen](concepts-streaming-ingestion-event-sources.md) und Empfehlungen zum Auswählen eines Zeitstempels für die Ereignisquelle.

* Lesen Sie, welche [Datentypen](concepts-supported-data-types.md) unterstützt werden.

* Erfahren Sie, wie die Erfassungs-Engine eine Reihe von [Regeln](./concepts-json-flattening-escaping-rules.md) auf Ihre JSON-Eigenschaften anwendet, um Ihre Speicherkontospalten zu erstellen.

* Überprüfen Sie die [Durchsatzbeschränkungen](concepts-streaming-throughput-limitations.md) in Ihrer Umgebung, um die erforderliche Skalierung zu planen.

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich als Nächstes über [Ereignisquellen](concepts-streaming-ingestion-event-sources.md) für Ihre Azure Time Series Insights-Umgebung. 
