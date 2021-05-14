---
title: Anfängliche Größe von Servergruppen – Hyperscale (Citus) – Azure Database for PostgreSQL
description: Auswählen der richtigen Anfangsgröße für Ihren Anwendungsfall
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 04/07/2021
ms.openlocfilehash: 8b87cfc8276d13ccc7e12a4901489ea0b1e770a5
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107012505"
---
# <a name="pick-initial-size-for-hyperscale-citus-server-group"></a>Auswählen der Anfangsgröße für eine Hyperscale (Citus)-Servergruppe

Die Größe einer Servergruppe bezieht sich auf die Anzahl von Knoten und deren Hardwarekapazität und lässt sich [ganz einfach ändern](howto-hyperscale-scale-grow.md). Für neue Servergruppen müssen Sie aber dennoch eine Anfangsgröße auswählen. Im Folgenden finden Sie einige Tipps für eine sinnvolle Festlegung dieses Werts.

## <a name="use-cases"></a>Anwendungsfälle

Hyperscale (Citus) wird häufig auf folgende Weise verwendet.

### <a name="multi-tenant-saas"></a>Mehrinstanzenfähige SaaS

Wenn Sie von einer vorhandenen PostgreSQL-Datenbankinstanz mit einem einzelnen Knoten zu Hyperscale (Citus) migrieren, empfiehlt sich die Auswahl eines Clusters, in dem die Anzahl der virtuellen Workerkerne und der Gesamtarbeitsspeicher den Werten der ursprünglichen Instanz entspricht. In solchen Szenarien konnten wir Leistungssteigerungen auf das Zwei- bis Dreifache feststellen, weil das Sharding die Ressourcennutzung verbessert, kleinere Indizes ermöglicht usw.

Die Anzahl der virtuellen Kerne ist dabei tatsächlich die einzige Entscheidung. Die RAM-Zuweisung wird derzeit basierend auf der Anzahl virtueller Kerne festgelegt. Eine Beschreibung finden Sie auf der Seite mit den [Konfigurationsoptionen für Hyperscale (Citus)](concepts-hyperscale-configuration-options.md).
Der Koordinatorknoten benötigt nicht so viel RAM wie Worker, aber es gibt keine Möglichkeit, RAM und virtuelle Kerne unabhängig voneinander auszuwählen.

### <a name="real-time-analytics"></a>Echtzeitanalysen

Virtuelle Kerne insgesamt: Wenn Arbeitsdaten in den RAM passen, können Sie von einer linearen Leistungsverbesserung bei Hyperscale (Citus) proportional zur Anzahl der Workerkerne ausgehen. Berücksichtigen Sie beim Ermitteln der richtigen Anzahl von virtuellen Kernen für Ihre Anforderungen die aktuelle Latenz für Abfragen in Ihrer Einzelknoten-Datenbank und die erforderliche Latenz in Hyperscale (Citus). Teilen Sie die aktuelle Wartezeit durch die gewünschte Wartezeit, und runden Sie das Ergebnis.

Worker-RAM: Der beste Fall wäre die Bereitstellung von so viel Speicher, dass der Großteil des Arbeitssatzes in den Speicher passt. Die Art der Abfragen, die Ihre Anwendung verwendet, wirkt sich auf die Arbeitsspeicheranforderungen aus. Sie können EXPLAIN ANALYZE für eine Abfrage ausführen, um zu ermitteln, wie viel Arbeitsspeicher diese Abfrage benötigt. Denken Sie daran, dass virtuelle Kerne und RAM zusammen skaliert werden, wie im Artikel [Konfigurationsoptionen für Hyperscale (Citus)](concepts-hyperscale-configuration-options.md) beschrieben.

## <a name="choosing-a-hyperscale-citus-tier"></a>Auswählen eines Tarifs für Hyperscale (Citus)

> [!IMPORTANT]
> Der Basic-Tarif von Hyperscale (Citus) ist zurzeit als Vorschauversion verfügbar.  Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Previewfunktionen für Hyperscale (Citus)](hyperscale-preview-features.md).

In den obigen Abschnitten wird erläutert, wie viele virtuelle Kerne und wie viel RAM für jeden Anwendungsfall benötigt wird. Sie können diese Anforderungen erfüllen, indem Sie zwischen zwei Hyperscale (Citus)-Tarifen wählen: dem Basic- und dem Standard-Tarif.

Beim Basic-Tarif wird ein einzelner Datenbankknoten verwendet, um die Verarbeitung durchzuführen, während der Standard-Tarif mehr Knoten zulässt. Die Tarife sind ansonsten identisch und bieten die gleichen Features. In einigen Fällen können die virtuellen Kerne und der Speicherplatz eines einzelnen Knotens auf die benötigte Größe skaliert werden, und in anderen Fällen ist die Kombination mehrerer Knoten erforderlich.

Einen Vergleich der Tarife finden Sie auf der Konzeptseite [Basic-Tarif](concepts-hyperscale-tiers.md).

## <a name="next-steps"></a>Nächste Schritte

- [Skalieren einer Servergruppe](howto-hyperscale-scale-grow.md)
- Erfahren Sie mehr über [Leistungsoptionen](concepts-hyperscale-configuration-options.md) für Servergruppen.
