---
title: Optimieren des Gen2-Cache
description: Erfahren Sie, wie Sie den Gen2-Cache im Azure-Portal überwachen.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: performance
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 924705b7ce1d324583077797714491bdf3fc5cc9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721216"
---
# <a name="how-to-monitor-the-gen2-cache"></a>Überwachen des Gen2-Cache
In der Gen2-Speicherarchitektur werden die am häufigsten abgefragten Columnstore-Segmente automatisch in einem Cache eingeordnet, der sich auf NVMe-basierten SSDs befindet, die für Gen2-Data Warehouses entwickelt wurden. Es wird eine bessere Leistung erreicht, wenn bei Ihren Abfragen Segmente abgerufen werden, die sich im Cache befinden. In diesem Artikel wird beschrieben, wie eine langsame Abfrageleistung überwacht und behoben wird, indem bestimmt wird, ob die Workload den Gen2-Cache optimal nutzt.  
## <a name="troubleshoot-using-the-azure-portal"></a>Behandeln von Problemen über das Azure-Portal
Mithilfe von Azure Monitor können Sie Metriken des Gen2-Cache anzeigen, um Probleme mit der Abfrageleistung zu beheben. Klicken Sie zunächst im Azure-Portal auf „Monitor“:

![Azure Monitor](./media/sql-data-warehouse-cache-portal/cache_0.png)

Wählen Sie die Schaltfläche „Metriken“ aus, und geben Sie die Informationen für **Abonnement**, **Ressourcen** **gruppe**, **Ressourcentyp** und **Ressourcenname** für Ihr Data Warehouse an.

Die wesentlichen Metriken für die Problembehandlung des Gen2-Cache sind **Prozentsatz der Cachetreffer** und **Cacheverwendung in Prozent**. Konfigurieren Sie das Azure-Metrikdiagramm so, dass diese beiden Metriken angezeigt werden.

![Cachemetriken](./media/sql-data-warehouse-cache-portal/cache_1.png)


## <a name="cache-hit-and-used-percentage"></a>Prozentsatz der Cachetreffer und Cacheverwendung in Prozent
In der folgenden Matrix sind Szenarien basierend auf den Werten der Cachemetriken beschrieben:

|                                | **Hoher Prozentsatz der Cachetreffer** | **Niedriger Prozentsatz der Cachetreffer** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Hohe Cacheverwendung in Prozent** |          Szenario 1           |          Szenario 2          |
| **Geringe Cacheverwendung in Prozent**  |          Szenario 3           |          Szenario 4          |

**Szenario 1:** Sie verwenden den Cache auf optimale Weise. [Behandeln Sie Probleme](sql-data-warehouse-manage-monitor.md) in anderen Bereichen, die möglicherweise zu einer Verlangsamung Ihrer Abfragen führen.

**Szenario 2:** Das aktuelle Arbeitsdataset passt nicht in den Cache, sodass aufgrund der physischen Lesevorgänge ein niedriger Prozentsatz an Cachetreffern anfällt. Sie können die Leistungsebene zentral hochskalieren und die Workload erneut ausführen, um den Cache zu füllen.

**Szenario 3:** Die Abfragen werden wahrscheinlich unabhängig von Problemen mit dem Cache langsam ausgeführt. [Behandeln Sie Probleme](sql-data-warehouse-manage-monitor.md) in anderen Bereichen, die möglicherweise zu einer Verlangsamung Ihrer Abfragen führen. Sie können zudem [Ihre Instanz zentral herunterskalieren](sql-data-warehouse-manage-monitor.md), um die Cachegröße zu reduzieren und so Kosten zu sparen. 

**Szenario 4:** Die Abfragen wurden möglicherweise aufgrund eines „kalten“ Cache langsam ausgeführt. Sie können Ihre Abfragen erneut ausführen, da das Arbeitsdataset sich nun im Cache befinden sollte. 

> [!IMPORTANT]
> Wenn der Prozentsatz der Cachetreffer oder die Cacheverwendung in Prozent nach der erneuten Ausführung der Workload nicht aktualisiert wird, befindet sich Ihr Arbeitssatz möglicherweise bereits im Arbeitsspeicher. Nur gruppierte Columnstore-Tabellen werden zwischengespeichert.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur allgemeinen Optimierung der Abfrageleistung finden Sie unter [Überwachen der Abfrageausführung](../sql-data-warehouse/sql-data-warehouse-manage-monitor.md#monitor-query-execution).
