---
title: Knoten – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Erfahren Sie mehr über die beiden Knotentypen (Koordinator und Worker) in einer Servergruppe in Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: af743ca56572f507091db01f11d3283294a9e3d5
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382796"
---
# <a name="nodes-in-azure-database-for-postgresql--hyperscale-citus"></a>Knoten in Azure Database for PostgreSQL – Hyperscale (Citus)

Der Hostingtyp von Hyperscale (Citus) ermöglicht es Azure Database for PostgreSQL-Servern (als „Knoten“ bezeichet), sich in einer „Shared-Nothing-Architektur“ zu koordinieren. Die Knoten in einer Servergruppe speichern zusammen mehr Daten und verwenden mehr CPU-Kerne als es auf einem einzelnen Server möglich wäre. Außerdem ermöglicht die Architektur das Skalieren der Datenbank durch Hinzufügen weiterer Knoten zur Servergruppe.

## <a name="coordinator-and-workers"></a>Koordinator und Worker

Jede Servergruppe verfügt über einen Koordinatorknoten und mehrere Worker(knoten). Anwendungen senden ihre Abfragen an den Koordinatorknoten, der sie an die relevanten Worker weiterleitet und die Ergebnisse sammelt. Anwendungen können keine direkte Verbindung mit Workern herstellen.

Hyperscale (Citus) ermöglicht es dem Datenbankadministrator, Tabellen zu *verteilen* und verschiedene Zeilen auf verschiedenen Workerknoten zu speichern. Verteilte Tabellen sind entscheidend für die Leistung von Hyperscale. Wenn Tabellen nicht verteilt werden, verbleiben Sie vollständig auf dem Koordinatorknoten und profitieren nicht von computerübergreifender Parallelität.

Der Koordinator leitet jede Abfrage für verteilte Tabellen entweder an einen einzelnen Workerknoten weiter oder parallelisiert sie über mehrere Knoten, je nachdem, ob sich die erforderlichen Daten auf einem einzelnen oder auf mehreren Knoten befinden. Der Koordinator entscheidet anhand von Metadatentabellen, welche Aktion auszuführen ist. In diesen Tabellen werden die DNS-Namen und die Integrität der Workerknoten sowie die Verteilung der Daten auf Knoten nachverfolgt.

## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [verteilten Tabellen](concepts-hyperscale-distributed-data.md)
