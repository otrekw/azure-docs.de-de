---
title: Was ist Azure Cache for Redis?
description: Azure Cache for Redis bietet Funktionen wie cachefremdes Muster, Content Caching, Zwischenspeicherung von Benutzersitzungen, Auftrags- und Nachrichtenwarteschlangen sowie verteilte Transaktionen.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 03/11/2020
ms.openlocfilehash: 38936000e426d560237295105b5456429d9ae16d
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79126365"
---
# <a name="azure-cache-for-redis-description"></a>Beschreibung von Azure Cache for Redis

Azure Cache for Redis bietet einen auf der Open-Source-Software [Redis](https://redis.io/) basierenden In-Memory-Datenspeicher. Als Cache verbessert Redis die Leistung und Skalierbarkeit von Systemen, die stark von Back-End-Datenspeichern abhängig sind. Zur Verbesserung der Leistung werden häufig verwendete Daten in einen schnellen Speicher kopiert, der sich in der Nähe der Anwendung befindet. Bei Azure Cache for Redis befindet sich dieser schnelle Speicher im Arbeitsspeicher. Er wird also nicht durch eine Datenbank vom Datenträger geladen.

Azure Cache for Redis kann als In-Memory-Datenstrukturspeicher, verteilte nicht relationale Datenbank und Nachrichtenbroker verwendet werden. Die Anwendungsleistung wird durch die Nutzung der Leistung des Redis-Moduls mit niedriger Latenz und hohem Durchsatz verbessert.

Azure Cache for Redis bietet Zugriff auf einen sicheren, dedizierten Redis Cache. Azure Cache for Redis wird von Microsoft verwaltet und in Azure gehostet, und jede Anwendung innerhalb oder außerhalb von Azure kann darauf zugreifen.

## <a name="using-azure-cache-for-redis"></a>Verwenden von Azure Cache for Redis

Azure Cache for Redis verbessert die Anwendungsleistung durch die Unterstützung gängiger Anwendungsarchitekturmuster. Nachfolgend sind einige der am häufigsten verwendeten Muster aufgeführt:

| Muster      | Beschreibung                                        |
| ------------ | -------------------------------------------------- |
| [Cache-Aside](cache-web-app-cache-aside-leaderboard.md) | Datenbanken sind häufig zu groß, um sie direkt in einen Cache zu laden. Das [cachefremde](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) Muster wird üblicherweise verwendet, um Daten nur bei Bedarf in den Cache zu laden. Wenn das System Änderungen an den Daten vornimmt, kann es auch den Cache aktualisieren, der dann an andere Clients verteilt wird. Darüber hinaus kann das System ein Ablaufdatum für Daten festlegen oder eine Entfernungsrichtlinie verwenden, um Datenaktualisierungen im Cache auszulösen.|
| [Inhaltszwischenspeicherung](cache-aspnet-output-cache-provider.md) | Viele Webseiten werden auf der Grundlage von Vorlagen mit statischen Inhalten wie Kopfzeilen, Fußzeilen und Bannern generiert. Diese statischen Elemente ändern sich in der Regel nur selten. Im Vergleich zu Back-End-Datenspeichern ermöglicht ein In-Memory-Cache einen schnelleren Zugriff auf statische Inhalte. Dieses Muster trägt zur Verringerung der Verarbeitungszeit und der Serverauslastung sowie zur Erhöhung der Reaktionsfähigkeit von Webservern bei. Dadurch benötigen Sie ggf. weniger Server, um die Last zu bewältigen. Zur Unterstützung dieses Musters mit ASP.NET bietet Azure Cache for Redis den Redis-Ausgabecacheanbieter.|
| [Zwischenspeicherung der Benutzersitzung](cache-aspnet-session-state-provider.md) | Dieses Muster wird häufig bei Einkaufswagen und anderen Benutzerverlaufsdaten verwendet, die eine Webanwendung möglicherweise mit Benutzercookies verknüpfen möchte. Die Speicherung von zu vielen Inhalten in einem Cookie kann sich negativ auf die Leistung auswirken, weil die Cookiegröße zunimmt und mit jeder Anforderung übergeben und überprüft wird. Bei einer typischen Lösung wird das Cookie als Schlüssel zum Abfragen der Daten in einer Datenbank verwendet. Die Verwendung eines In-Memory-Cache wie Azure Cache for Redis, um einem Benutzer Informationen zuzuordnen, ist wesentlich schneller als die Interaktion mit einer vollständigen relationalen Datenbank. |
| Einreihen von Aufträgen in eine Warteschlange und Message Queuing | Anwendungen fügen Aufgaben häufig einer Warteschlange hinzu, wenn die Ausführung der mit der Anforderung verbundenen Vorgänge länger dauert. Zeitintensive Vorgänge werden einer Warteschlange hinzugefügt und der Reihe nach verarbeitet – nicht selten von einem anderen Server.  Diese Methode der Verzögerung von Arbeit wird als Aufgaben-Queuing bezeichnet. Azure Cache for Redis bietet eine verteilte Warteschlange, um dieses Muster in Ihrer Anwendung zu ermöglichen.|
| Verteilte Transaktionen | Anwendungen benötigen mitunter eine Reihe von Befehlen für einen Back-End-Datenspeicher, um einen einzelnen atomischen Vorgang auszuführen. Alle Befehle müssen erfolgreich ausgeführt oder jeweils mittels Rollback in den ursprünglichen Zustand zurückversetzt werden. Azure Cache for Redis unterstützt das Ausführen eines Batchs von Befehlen als einzelne [Transaktion](https://redis.io/topics/transactions). |

## <a name="azure-cache-for-redis-offerings"></a>Azure Cache for Redis-Angebote

Azure Cache for Redis ist in folgenden Tarifen verfügbar:

| Tarif | Beschreibung |
|---|---|
Basic | Ein Cache mit einem einzelnen Knoten. Dieser Tarif unterstützt mehrere Arbeitsspeichergrößen (250 MB–53 GB) und eignet sich perfekt für Entwicklungs-/Testworkloads und nicht kritische Workloads. Für den Basic-Tarif gilt keine Vereinbarung zum Servicelevel (Service-Level Agreement, SLA). |
| Standard | Ein replizierter Cache in einer Primär-/Sekundärkonfiguration mit zwei Knoten, die von Azure verwaltet wird, mit Hochverfügbarkeits-SLA (99,9 Prozent). |
| Premium | Die Dienstebene „Premium“ ist die Ebene für Unternehmen. Caches im Premium-Tarif unterstützen mehr Funktionen und bieten einen höheren Durchsatz bei niedrigeren Latenzen. Caches im Premium-Tarif werden auf leistungsfähigerer Hardware bereitgestellt und bieten im Vergleich zu den Tarifen „Basic“ und „Standard“ eine bessere Leistung. Dieser Vorteil bedeutet, dass der Durchsatz für einen Cache derselben Größe in der Premium-Dienstebene höher ist als in der Standard-Dienstebene. |

> [!TIP]
> Weitere Informationen zur Größe, zum Durchsatz und zur Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Cache for Redis](cache-faq.md#what-azure-cache-for-redis-offering-and-size-should-i-use).
>

Der Cache kann nach der Erstellung auf einen höheren Tarif skaliert werden. Die Herunterskalierung auf einen niedrigeren Tarif wird nicht unterstützt. Ausführliche Anweisungen zur Skalierung finden Sie unter [How to Scale Azure Cache for Redis](cache-how-to-scale.md) (Skalieren von Azure Cache for Redis) und [How to automate a scaling operation](cache-how-to-scale.md#how-to-automate-a-scaling-operation) (Automatisieren eines Skalierungsvorgangs).

### <a name="feature-comparison"></a>Funktionsvergleiche

Auf der Seite [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/) finden Sie einen detaillierten Vergleich der einzelnen Tarife. In der folgenden Tabelle werden einige der unterstützten Features nach Tarif aufgeführt:

| Funktionsbeschreibung | Premium | Standard | Basic |
| ------------------- | :-----: | :------: | :---: |
| [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) |✔|✔|-|
| [Redis-Datenpersistenz](cache-how-to-premium-persistence.md) |✔|-|-|
| [Redis-Cluster](cache-how-to-premium-clustering.md) |✔|-|-|
| [Sicherheit über Firewallregeln](cache-configure.md#firewall) |✔|✔|✔|
| Verschlüsselung während der Übertragung |✔|✔|✔|
| [Verbesserte Sicherheit und Isolation mit VNet](cache-how-to-premium-vnet.md) |✔|-|-|
| [Import/Export](cache-how-to-import-export-data.md) |✔|-|-|
| [Geplante Updates](cache-administration.md#schedule-updates) |✔|✔|✔|
| [Georeplikation](cache-how-to-geo-replication.md) |✔|-|-|
| [Neustart](cache-administration.md#reboot) |✔|✔|✔|

## <a name="next-steps"></a>Nächste Schritte

* [ASP.NET-Web-App-Schnellstart](cache-web-app-howto.md): Erstellen Sie eine einfache ASP.NET-Web-App, die Azure Cache for Redis verwendet.
* [.NET-Schnellstart](cache-dotnet-how-to-use-azure-redis-cache.md): Erstellen Sie eine .NET-App, die Azure Cache for Redis verwendet.
* [.NET Core-Schnellstart](cache-dotnet-core-quickstart.md): Erstellen Sie eine .NET Core-App, die Azure Cache for Redis verwendet.
* [Node.js-Schnellstart](cache-nodejs-get-started.md): Erstellen Sie eine einfache Node.js-App, die Azure Cache for Redis verwendet.
* [Java-Schnellstart](cache-java-get-started.md): Erstellen Sie eine einfache Java-App, die Azure Cache for Redis verwendet.
* [Python-Schnellstart](cache-python-get-started.md): Erstellen Sie eine Python-App, die Azure Cache for Redis verwendet.
