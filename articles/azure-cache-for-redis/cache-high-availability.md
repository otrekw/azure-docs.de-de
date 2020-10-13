---
title: Hochverfügbarkeit für Azure Cache for Redis
description: Informationen zu Hochverfügbarkeitsfunktionen und -optionen bei Azure Cache for Redis
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: yegu
ms.openlocfilehash: 145be11436eb4d0c4f6b892e5239ccacd838d780
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91654060"
---
# <a name="high-availability-for-azure-cache-for-redis"></a>Hochverfügbarkeit für Azure Cache for Redis

Azure Cache for Redis verfügt über integrierte Hochverfügbarkeit. Das Ziel dieser Hochverfügbarkeitsarchitektur besteht darin sicherzustellen, dass Ihre verwaltete Redis-Instanz funktioniert, auch wenn die zugrunde liegenden virtuellen Computer durch geplante oder ungeplante Ausfälle beeinträchtigt werden. Sie bietet deutlich höhere Prozentsätze als dies beim Hosten von Redis auf einem einzelnen virtuellen Computer möglich ist.

Azure Cache for Redis implementiert Hochverfügbarkeit durch Verwendung mehrerer virtueller Computer (als *Knoten* bezeichnet) für einen Cache. Diese Knoten werden so konfiguriert, dass die Datenreplikation und das Failover in koordinierter Weise erfolgen. Außerdem werden Wartungsvorgänge wie Redis-Softwarepatches orchestriert. In den Tarifen „Standard“ und „Premium“ sind verschiedene Hochverfügbarkeitsoptionen verfügbar:

| Option | BESCHREIBUNG | Verfügbarkeit | Standard | Premium |
| ------------------- | ------- | ------- | :------: | :---: |
| [Standardreplikation](#standard-replication)| Replizierte Konfiguration mit zwei Knoten in einem Rechenzentrum oder einer Verfügbarkeitszone (VZ) mit automatischem Failover | 99,9 % |✔|✔|
| [Mehrere Replikate](#multiple-replicas) | Replizierte Konfiguration mit mehreren Knoten in einer oder mehreren Verfügbarkeitszonen mit automatischem Failover | 99,95 % (mit Zonenredundanz) |-|✔|
| [Zonenredundanz](#zone-redundancy) | Replizierte Konfiguration mit mehreren Knoten in allen Verfügbarkeitszonen mit automatischem Failover | 99,95 % (mit mehreren Replikaten) |-|✔|
| [Georeplikation](#geo-replication) | Verknüpfte Cacheinstanzen in zwei Regionen mit benutzergesteuertem Failover | 99,9 % (für eine einzelne Region) |-|✔|

## <a name="standard-replication"></a>Standardreplikation

Eine Azure Cache for Redis-Instanz im Tarif „Standard“ oder „Premium“ wird standardmäßig auf zwei Redis-Servern ausgeführt. Die beiden Server werden auf dedizierten virtuellen Computern gehostet. Bei Open-Source-Redis können Anforderungen zum Schreiben von Daten nur auf einem Server verarbeitet werden. Bei diesem Server handelt es sich um den *primären* Knoten, der andere Server ist der *Replikatknoten*. Nach dem Bereitstellen der Serverknoten weist Azure Cache for Redis diesen die primäre und die Replikatrolle zu. Der primäre Knoten dient normalerweise der Verarbeitung von Schreib- und Leseanforderungen von Redis-Clients. Bei einem Schreibvorgang wird ein neuer Schlüssel und eine Schlüsselaktualisierung in den internen Speicher des Knotens übertragen und sofort eine Antwort an den Client gesendet. Der Vorgang wird asynchron an das Replikat weitergeleitet.

:::image type="content" source="media/cache-high-availability/replication.png" alt-text="Einrichten der Datenreplikation":::
   
>[!NOTE]
>Normalerweise kommuniziert ein Redis-Client bei allen Lese- und Schreibanforderungen mit dem primären Knoten in einem Redis-Cache. Bestimmte Redis-Clients können so konfiguriert werden, dass sie vom Replikatknoten lesen.
>
>

Wenn der primäre Knoten in einem Redis-Cache nicht verfügbar ist, wird der Replikatknoten automatisch zum primären Knoten höher gestuft. Dieser Prozess wird als *Failover* bezeichnet. Der Replikatknoten wartet vor dem Höherstufen ausreichend lange, für den Fall, dass der primäre Knoten schnell wiederhergestellt wird. Bei einem Failover stellt Azure Cache for Redis einen neuen virtuellen Computer bereit und verbindet ihn als Replikatknoten mit dem Cache. Der Replikatknoten führt eine vollständige Datensynchronisierung mit dem primären Knoten durch, sodass eine weitere Kopie der Cachedaten vorhanden ist.

Ein primärer Knoten kann als Teil einer geplanten Wartungsaktivität (z. B. Redis-Softwareupdate oder Betriebssystemupdate) außer Betrieb genommen werden. Er kann auch aufgrund ungeplanter Ereignisse wie z. B. Fehlern in der zugrunde liegenden Hardware, Software oder im Netzwerk ausfallen. Unter [Failover und Patching für Azure Cache for Redis](cache-failover.md) finden Sie eine ausführliche Erläuterung zu den verschiedenen Redis-Failovertypen. Eine Azure Cache for Redis-Instanz durchläuft während ihrer Lebensdauer viele Failover. Die Hochverfügbarkeitsarchitektur ist so konzipiert, dass diese Änderungen in einem Cache für die Clients so transparent wie möglich sind.

## <a name="multiple-replicas"></a>Mehrere Replikate

>[!NOTE]
>Dies ist als Vorschauversion verfügbar.
>
>

Azure Cache for Redis ermöglicht im Tarif „Premium“ zusätzliche Replikatknoten. Ein [Cache mit mehreren Replikaten](cache-how-to-multi-replicas.md) kann mit bis zu drei Replikatknoten konfiguriert werden. Durch mehrere Replikate verbessert sich im Allgemeinen die Resilienz, da der primäre Knoten durch zusätzliche Knoten abgesichert wird. Auch bei mehreren Replikaten kann eine Azure Cache for Redis-Instanz dennoch durch einen Ausfall eines Rechenzentrums oder einen Ausfall in der gesamten Verfügbarkeitszone beeinträchtigt werden. Sie können die Cacheverfügbarkeit erhöhen, indem Sie mehrere Replikate in Verbindung mit [Zonenredundanz](#zone-redundancy) verwenden.

## <a name="zone-redundancy"></a>Zonenredundanz

>[!NOTE]
>Dies ist als Vorschauversion verfügbar.
>
>

Azure Cache for Redis unterstützt im Tarif „Premium“ zonenredundante Konfigurationen. Bei einem [zonenredundanten Cache](cache-how-to-zone-redundancy.md) können die zugehörigen Knoten in unterschiedlichen [Azure-Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview) derselben Region platziert werden. Ausfälle im Rechenzentrum oder in einer Verfügbarkeitszone werden als Single Point of Failure behoben. Damit wird die Gesamtverfügbarkeit des Caches erhöht.

Das folgende Diagramm veranschaulicht die zonenredundante Konfiguration:

:::image type="content" source="media/cache-high-availability/zone-redundancy.png" alt-text="Einrichten der Datenreplikation":::
   
Azure Cache for Redis verteilt Knoten in einem zonenredundanten Cache im Roundrobin-Verfahren auf die ausgewählten Verfügbarkeitszonen. Außerdem wird festgelegt, welcher Knoten anfänglich als primärer Knoten verwendet wird.

Ein zonenredundanter Cache ermöglicht ein automatisches Failover. Wenn der aktuelle primäre Knoten nicht verfügbar ist, wird er durch einen der Replikatknoten ersetzt. Wenn sich der neue primäre Knoten in einer anderen Verfügbarkeitszone befindet, bedeutet dies für die Anwendung möglicherweise eine längere Cacheantwortzeit. Verfügbarkeitszonen sind geografisch getrennt. Durch den Wechsel zwischen Verfügbarkeitszonen ändert sich die physische Entfernung zwischen den Orten, an denen die Anwendung und der Cache gehostet werden. Diese Änderung wirkt sich auf Roundtrip-Netzwerklatenzen zwischen Anwendung und Cache aus. Bei den meisten Anwendungen wird davon ausgegangen, dass die zusätzliche Latenz innerhalb eines akzeptablen Bereichs liegt. Es wird empfohlen, Ihre Anwendung zu testen, um sicherzustellen, dass sie mit einem zonenredundanten Cache ordnungsgemäß ausgeführt wird.

## <a name="geo-replication"></a>Georeplikation

Die Georeplikation ist im Wesentlichen für die Notfallwiederherstellung konzipiert. Damit haben Sie die Möglichkeit, eine Azure Cache for Redis-Instanz in einer anderen Azure-Region zu konfigurieren, um den primären Cache zu sichern. Unter [Einrichten der Georeplikation für Azure Cache for Redis](cache-how-to-geo-replication.md) finden Sie eine ausführliche Erläuterung der Funktionsweise der Georeplikation.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Konfigurieren von Hochverfügbarkeitsoptionen von Azure Cache for Redis:

* [Premium-Dienstebenen für Azure Cache for Redis](cache-overview.md#service-tiers)
* [Hinzufügen von Replikaten zu Azure Cache for Redis](cache-how-to-multi-replicas.md)
* [Aktivieren der Zonenredundanz für Azure Cache for Redis](cache-how-to-zone-redundancy.md)
* [Einrichten der Georeplikation für Azure Cache for Redis](cache-how-to-geo-replication.md)
