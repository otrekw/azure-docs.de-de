---
title: Konzepte für die Verteilung von Daten und horizontales Hochskalieren mit der Arc-fähigen PostgreSQL Hyperscale-Servergruppe
titleSuffix: Azure Arc enabled data services
description: Konzepte für die Verteilung von Daten mit einer Arc-fähigen PostgreSQL Hyperscale-Servergruppe
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: TheJY
ms.author: jeanyd
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: c01da4aed9e27296ea7b570420bb190b16749848
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90931548"
---
# <a name="concepts-for-distributing-data-with-arc-enabled-postgresql-hyperscale-server-group"></a>Konzepte für die Verteilung von Daten mit einer Arc-fähigen PostgreSQL Hyperscale-Servergruppe

In diesem Artikel werden Schlüsselkonzepte erläutert, die wichtig sind, um den größten Nutzen aus Azure Arc-aktivierten PostgreSQL Hyperscale-Servergruppen zu ziehen.
Die Links zu den Artikeln unten verweisen auf die Konzepte, die für Azure Database for PostgreSQL Hyperscale (Citus) erläutert werden. Es handelt sich um dieselbe Technologie wie Azure Arc-fähige PostgreSQL Hyperscale-Servergruppen, sodass dieselben Konzepte und Perspektiven gelten.

**Worin besteht der Unterschied?**
- _Azure Database for PostgreSQL Hyperscale (Citus)_

Dies ist der Hyperscale-Formfaktor der Postgres-Datenbank-Engine, der als Database-as-a-Service in Azure (PaaS) verfügbar ist. Er wird von der Citus-Erweiterung unterstützt, die die Hyperscale-Umgebung ermöglicht. In diesem Formfaktor wird der Dienst in den Microsoft-Rechenzentren ausgeführt und von Microsoft betrieben.

- _PostgreSQL Hyperscale mit Azure Arc-Aktivierung_

Dies ist der Hyperscale-Formfaktor der Postgres-Datenbank-Engine, der mit dem Azure Arc-fähigen Datendienst zur Verfügung gestellt wird. In diesem Formfaktor stellen unsere Kunden die Infrastruktur bereit, die die Systeme hostet, und betreiben sie.

Die wichtigsten Konzepte für Azure Arc-aktivierte PostgreSQL Hyperscale-Servergruppen werden unten zusammengefasst:

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="nodes-and-tables"></a>Knoten und Tabellen
Es ist wichtig, mit den folgenden Konzepten vertraut zu sein, um den größten Nutzen aus Azure Arc-aktivierte PostgreSQL Hyperscale-Servergruppen zu ziehen:
- Spezialisierte Postgres-Knoten in Azure Arc-aktivierten PostgreSQL Hyperscale-Servergruppen: Koordinator und Worker
- Tabellentypen: verteilte Tabellen, Verweistabellen und lokale Tabellen
- Shards

Weitere Informationen finden Sie unter [Knoten und Tabellen in Azure Database for PostgreSQL – Hyperscale (Citus)](../../postgresql/concepts-hyperscale-nodes.md). 

## <a name="determine-the-application-type"></a>Ermitteln des Anwendungstyps
Die eindeutige Identifizierung des Anwendungstyps, den Sie erstellen, ist wichtig. Warum? Weil das Ausführen effizienter Abfragen für eine Azure Arc-fähige PostgreSQL Hyperscale-Servergruppe erfordert, dass die Tabellen ordnungsgemäß auf die Server verteilt sind. Die empfohlene Verteilung variiert abhängig vom Anwendungstyp und den jeweiligen Abfragemustern. Es gibt im Wesentlichen zwei Arten von Anwendungen, die mit Azure Arc-fähigen Postgres Hyperscale-Servergruppen gut funktionieren:
- Mehrinstanzenfähige Anwendungen
- Echtzeitanwendungen

Der erste Schritt bei der Datenmodellierung besteht darin, herauszufinden, welche davon Ihrer Anwendung am ähnlichsten ist.

Einzelheiten dazu finden Sie unter [Ermitteln des Anwendungstyps](../../postgresql/concepts-hyperscale-app-type.md).


## <a name="choose-a-distribution-column"></a>Auswählen einer Verteilungsspalte
Warum sollte eine verteilte Spalte ausgewählt werden?

Dies ist eine der wichtigsten Modellierungsentscheidungen, die Sie treffen. Azure Arc-fähige PostgreSQL Hyperscale-Servergruppen speichern Zeilen basierend auf dem Wert der Verteilungsspalte der Zeilen in Shards. Durch die richtige Wahl werden verwandte Daten in denselben physischen Knoten gruppiert, was zu schnellen Abfragen führt und Unterstützung für alle SQL-Funktionen hinzufügt. Durch eine falsche Auswahl wird das System langsam ausgeführt und unterstützt nicht alle SQL-Funktionen auf allen Knoten. In diesem Artikel erhalten Sie Tipps für die Verteilungsspalte für die beiden häufigsten Hyperscale-Szenarien.

Einzelheiten dazu finden Sie unter[Auswählen von Verteilungsspalten](../../postgresql/concepts-hyperscale-choose-distribution-column.md).


## <a name="table-colocation"></a>Tabellenzusammenstellung

Colocation bedeutet, verwandte Informationen zusammen auf denselben Knoten zu speichern. Abfragen können schnell erfolgen, wenn alle erforderlichen Daten ohne Netzwerkdatenverkehr verfügbar sind. Durch das Zusammenstellen verwandter Daten auf unterschiedlichen Knoten können Abfragen effizient parallel auf den einzelnen Knoten ausgeführt werden.

Weitere Informationen finden Sie unter [Tabellen Zusammenstellung](../../postgresql/concepts-hyperscale-colocation.md).


## <a name="next-steps"></a>Nächste Schritte
- [Informationen zum Erstellen von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen](create-postgresql-hyperscale-server-group.md)
- [Erfahren Sie mehr über das Aufskalieren von Azure Arc-fähigen PostgreSQL Hyperscale-Servergruppen, die in Ihrem Arc-Datencontroller erstellt wurden](scale-out-postgresql-hyperscale-server-group.md).
- [Weitere Informationen zu Azure Arc-fähigen Data Services](https://azure.microsoft.com/services/azure-arc/hybrid-data-services)
- [Weitere Informationen zu Azure Arc](https://aka.ms/azurearc)

