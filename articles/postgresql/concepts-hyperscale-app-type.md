---
title: Ermitteln des Anwendungstyps – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Identifizieren der Anwendung für effektive Modellierung verteilter Daten
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 92333857177d33307d6997bfcbdf79787d3ab127
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90895952"
---
# <a name="determining-application-type"></a>Ermitteln des Anwendungstyps

Wenn Sie effiziente Abfragen für eine Hyperscale (Citus)-Servergruppe ausführen möchten, müssen die Tabellen ordnungsgemäß auf die Server verteilt werden. Die empfohlene Verteilung variiert abhängig vom Anwendungstyp und den jeweiligen Abfragemustern.

Es gibt im Wesentlichen zwei Arten von Anwendungen, die mit Hyperscale (Citus) gut funktionieren. Der erste Schritt bei der Datenmodellierung besteht darin, herauszufinden, welche davon Ihrer Anwendung am ähnlichsten ist.

## <a name="at-a-glance"></a>Auf einen Blick

| Mehrinstanzenfähige Anwendungen                                 | Echtzeitanwendungen                                |
|-----------------------------------------------------------|-------------------------------------------------------|
| Manchmal Dutzende oder Hunderte von Tabellen im Schema          | Kleine Anzahl von Tabellen                                |
| Abfragen im Zusammenhang mit einem Mandanten (Unternehmen/Store) gleichzeitig | Relativ einfache Analyseabfragen mit Aggregationen |
| OLTP-Workloads für Webclients                    | Hohes Erfassungsvolumen von überwiegend unveränderlichen Daten           |
| OLAP-Workloads, die analytische Abfragen pro Mandant bedienen   | Häufig konzentriert auf große Ereignistabellen            |

## <a name="examples-and-characteristics"></a>Beispiele und Merkmale

**Mehrinstanzenfähige Anwendung**

> Dabei handelt es sich in der Regel um SaaS-Anwendungen für andere Unternehmen, Konten oder Organisationen. Die meisten SaaS-Anwendungen sind grundsätzlich relational. Sie besitzen eine natürliche Dimension, in der Daten auf Knoten verteilt werden können: einfach Shard nach tenant\_id.
>
> Hyperscale (Citus) ermöglicht es Ihnen, Ihre Datenbank auf Millionen von Mandanten aufzuskalieren, ohne dass Sie Ihre Anwendung umstrukturieren müssen. Sie können die relationale Semantik beibehalten, die Sie benötigen, etwa Joins, Fremdschlüsseleinschränkungen, Transaktionen, ACID und Konsistenz.
>
> -   **Beispiele**: Websites, die Shops für andere Unternehmen hosten, z. B. eine digitale Marketinglösung oder ein Verkaufsautomatisierungstool.
> -   **Merkmale**: Abfragen im Zusammenhang mit einem einzelnen Mandanten, anstatt Informationen mandantenübergreifend zu verknüpfen. Dies schließt OLTP-Workloads für Webclients sowie OLAP-Workloads ein, die analytische Abfragen pro Mandant bedienen. Das Vorhandensein von Dutzenden oder Hunderten von Tabellen in Ihrem Datenbankschema ist ebenfalls ein Indikator für das mehrinstanzenfähige Datenmodell.
>
> Das Skalieren einer mehrinstanzenfähigen App mit Hyperscale (Citus) erfordert auch minimale Änderungen am Anwendungscode. Wir bieten Unterstützung für beliebte Frameworks wie Ruby on Rails und Django.

**Echtzeitanalysen**

> Anwendungen, die massive Parallelität benötigen, um Hunderte von Kernen für schnelle Ergebnisse in numerischen, statistischen oder zählenden Abfragen zu koordinieren.  Durch Sharding und Parallelisierung von SQL-Abfragen über mehrere Knoten hinweg ermöglicht Hyperscale (Citus) das Durchführen von Echtzeitabfragen in Milliarden von Datensätzen in weniger als einer Sekunde.
>
> Tabellen in Echtzeitanalyse-Datenmodellen werden in der Regel durch Spalten wie user\_id, host\_id oder device\_id verteilt.
>
> -   **Beispiele**: Analysedashboards für Kunden, die Antwortzeiten von unter einer Sekunde erfordern.
> -   **Merkmale**: Wenige Tabellen, die sich oft um eine große Tabelle mit Geräte-, Standort- oder Benutzerereignissen gruppieren und ein hohes Erfassungsvolumen von zumeist unveränderlichen Daten erfordern. Relativ einfache (aber rechenintensive) Analyseabfragen, die mehrere Aggregationen und GROUP BY-Anweisungen betreffen.

Wenn Ihre Situation einem der oben beschriebenen Fälle ähnelt, besteht der nächste Schritt darin, zu entscheiden, wie das Sharding Ihrer Daten in der Servergruppe erfolgen soll. Die Auswahl der Verteilungsspalten durch den Datenbankadministrator muss den Zugriffsmustern typischer Abfragen entsprechen, um optimale Leistung zu gewährleisten.

## <a name="next-steps"></a>Nächste Schritte

* [Auswählen einer Verteilungsspalte](concepts-hyperscale-choose-distribution-column.md) für Tabellen in der Anwendung für effiziente Verteilung von Daten
