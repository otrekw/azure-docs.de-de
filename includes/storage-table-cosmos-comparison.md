---
title: include file
description: include file
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: include
ms.date: 01/08/2021
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: a7e34f077ce1b2541168df40f2806fdb24a63a79
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98050753"
---
Wenn Sie derzeit Azure Table Storage verwenden, bietet Ihnen der Wechsel zur Azure Cosmos DB-Tabellen-API folgende Vorteile:

|Funktion | Azure-Tabellenspeicher | Azure Cosmos DB-Tabellen-API |
| --- | --- | --- |
| Latency | Schnell, aber keine Obergrenzen für die Wartezeit. | Wartezeit im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge, unterstützt durch weniger als 10 ms Wartezeit bei Lese- und weniger als 15 ms Wartezeit bei Schreibvorgängen im 99. Perzentil, bei beliebiger Skalierung weltweit. |
| Throughput | Variables Durchsatzmodell. Tabellen verfügen über eine maximale Skalierbarkeit von 20.000 Vorgängen/s. | Hochgradig skalierbar mit einem [dedizierten reservierten Durchsatz pro Tabelle](../articles/cosmos-db/request-units.md), der durch SLAs abgedeckt ist. Konten haben keine Obergrenze für den Durchsatz und unterstützen pro Tabelle mehr als 10 Millionen Vorgänge/s (im Modus „Bereitgestellter Durchsatz“). |
| Globale Verteilung | Einzelne Region mit einer optionalen sekundären Leseregion für Hochverfügbarkeit. Es kann kein Failover initiiert werden. | [Globale, sofort einsatzbereite Verteilung](../articles/cosmos-db/distribute-data-globally.md) für 1 bis mehr als 30 Regionen. Unterstützung von [automatischen und manuellen Failovern](../articles/cosmos-db/high-availability.md) zu jeder Zeit und an jedem Ort der Welt. |
| Indizierung | Nur primärer Index für PartitionKey und RowKey. Keine sekundären Indizes. | Automatische und vollständige Indizierung für alle Eigenschaften, keine Indexverwaltung. |
| Abfrage | Abfrageausführung verwendet Index für Primärschlüssel, andernfalls die Suche. | Abfragen können die automatische Indizierung für Eigenschaften für schnelle Abfragezeiten nutzen. |
| Konsistenz | „Stark“ in der primären Region. „Möglich“ in der sekundären Region. | [Fünf klar definierte Konsistenzebenen](../articles/cosmos-db/consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz, Durchsatz und Konsistenz basierend auf Ihren Anwendungsanforderungen. |
| Preise | Nutzungsbasiert. | Die verfügbaren Modi lauten [Nutzungsbasiert](../articles/cosmos-db/serverless.md) und [Bereitgestellte Kapazität](../articles/cosmos-db/set-throughput.md). |
| SLAs | Verfügbarkeit von 99,99%. | Verfügbarkeit von 99,99% (SLA) für alle Konten für eine einzelne Region und alle Konten für mehrere Regionen mit gelockerter Konsistenz sowie Leseverfügbarkeit von 99,999% für alle Datenbankkonten für mehrere Regionen ([branchenführende umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/)) mit allgemeiner Verfügbarkeit. |
