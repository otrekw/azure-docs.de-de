---
title: Was ist Azure Cache for Redis?
description: Azure Cache for Redis bietet Funktionen wie cachefremdes Muster, Content Caching, Zwischenspeicherung von Benutzersitzungen, Auftrags- und Nachrichtenwarteschlangen sowie verteilte Transaktionen.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: overview
ms.date: 05/12/2020
ms.openlocfilehash: 3751560125ea8ac6cc00ed63521bff30b751e688
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009595"
---
# <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Azure Cache for Redis bietet einen auf der Open-Source-Software [Redis](https://redis.io/) basierenden In-Memory-Datenspeicher. Redis verbessert die Leistung und Skalierbarkeit einer Anwendung, die intensiven Gebrauch von Back-End-Datenspeichern macht. Mit Redis können große Mengen von Anwendungsanforderungen verarbeitet werden, indem häufig verwendete Daten im Arbeitsspeicher des Servers behalten werden, um schnelle Schreib- und Lesevorgänge zu ermöglichen. Redis stellt eine kritische Datenspeicherlösung mit geringer Wartezeit und hohem Durchsatz für moderne Anwendungen bereit.

Azure Cache for Redis bietet Redis als verwalteten Dienst. Dadurch erhalten Sie sichere und dedizierte Redis-Serverinstanzen sowie vollständige Redis-API-Kompatibilität. Der Dienst wird von Microsoft betrieben und in Azure gehostet, und jede Anwendung innerhalb oder außerhalb von Azure kann darauf zugreifen.

Azure Cache for Redis kann unter anderem als Cache für verteilte Daten oder Inhalte, als Sitzungsspeicher oder als Nachrichtenbroker verwendet werden. Azure Cache for Redis kann als eigenständige Lösung oder parallel mit einem anderen Azure-Datenbankdienst wie etwa Azure SQL oder Cosmos DB bereitgestellt werden.

## <a name="key-scenarios"></a>Wichtige Szenarien
Azure Cache for Redis verbessert die Anwendungsleistung durch die Unterstützung gängiger Anwendungsarchitekturmuster. Nachfolgend sind einige der am häufigsten verwendeten Muster aufgeführt:

| Muster      | BESCHREIBUNG                                        |
| ------------ | -------------------------------------------------- |
| [Datencache](cache-web-app-cache-aside-leaderboard.md) | Datenbanken sind häufig zu groß, um sie direkt in einen Cache zu laden. Das [cachefremde](https://docs.microsoft.com/azure/architecture/patterns/cache-aside) Muster wird üblicherweise verwendet, um Daten nur bei Bedarf in den Cache zu laden. Wenn das System Änderungen an den Daten vornimmt, kann es auch den Cache aktualisieren, der dann an andere Clients verteilt wird. Darüber hinaus kann das System ein Ablaufdatum für Daten festlegen oder eine Entfernungsrichtlinie verwenden, um Datenaktualisierungen im Cache auszulösen.|
| [Inhaltscache](cache-aspnet-output-cache-provider.md) | Viele Webseiten werden auf der Grundlage von Vorlagen mit statischen Inhalten wie Kopfzeilen, Fußzeilen und Bannern generiert. Diese statischen Elemente ändern sich in der Regel nur selten. Im Vergleich zu Back-End-Datenspeichern ermöglicht ein In-Memory-Cache einen schnelleren Zugriff auf statische Inhalte. Dieses Muster trägt zur Verringerung der Verarbeitungszeit und der Serverauslastung sowie zur Erhöhung der Reaktionsfähigkeit von Webservern bei. Dadurch benötigen Sie ggf. weniger Server, um die Last zu bewältigen. Zur Unterstützung dieses Musters mit ASP.NET bietet Azure Cache for Redis den Redis-Ausgabecacheanbieter.|
| [Sitzungsspeicher](cache-aspnet-session-state-provider.md) | Dieses Muster wird häufig bei Einkaufswagen und anderen Benutzerverlaufsdaten verwendet, die eine Webanwendung möglicherweise mit Benutzercookies verknüpfen möchte. Die Speicherung von zu vielen Inhalten in einem Cookie kann sich negativ auf die Leistung auswirken, weil die Cookiegröße zunimmt und mit jeder Anforderung übergeben und überprüft wird. Bei einer typischen Lösung wird das Cookie als Schlüssel zum Abfragen der Daten in einer Datenbank verwendet. Die Verwendung eines In-Memory-Cache wie Azure Cache for Redis, um einem Benutzer Informationen zuzuordnen, ist wesentlich schneller als die Interaktion mit einer vollständigen relationalen Datenbank. |
| Einreihen von Aufträgen in eine Warteschlange und Message Queuing | Anwendungen fügen Aufgaben häufig einer Warteschlange hinzu, wenn die Ausführung der mit der Anforderung verbundenen Vorgänge länger dauert. Zeitintensive Vorgänge werden einer Warteschlange hinzugefügt und der Reihe nach verarbeitet – nicht selten von einem anderen Server.  Diese Methode der Verzögerung von Arbeit wird als Aufgaben-Queuing bezeichnet. Azure Cache for Redis bietet eine verteilte Warteschlange, um dieses Muster in Ihrer Anwendung zu ermöglichen.|
| Verteilte Transaktionen | Anwendungen benötigen mitunter eine Reihe von Befehlen für einen Back-End-Datenspeicher, um einen einzelnen atomischen Vorgang auszuführen. Alle Befehle müssen erfolgreich ausgeführt oder jeweils mittels Rollback in den ursprünglichen Zustand zurückversetzt werden. Azure Cache for Redis unterstützt das Ausführen eines Batchs von Befehlen als einzelne [Transaktion](https://redis.io/topics/transactions). |

## <a name="service-tiers"></a>Dienstebenen
Azure Cache for Redis ist in folgenden Tarifen verfügbar:

| Tarif | BESCHREIBUNG |
|---|---|
Basic | Ein Cache mit einem einzelnen Knoten. Dieser Tarif unterstützt mehrere Arbeitsspeichergrößen (250 MB–53 GB) und eignet sich perfekt für Entwicklungs-/Testworkloads und nicht kritische Workloads. Für den Basic-Tarif steht keine Vereinbarung zum Servicelevel (Service-Level Agreement, SLA) zur Verfügung. |
| Standard | Ein replizierter Cache in einer von Azure verwalteten Konfiguration mit zwei Knoten (primärer Knoten und Replikat) und [SLA](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) für Hochverfügbarkeit. |
| Premium | Die Dienstebene „Premium“ ist die Ebene für Unternehmen. Caches im Premium-Tarif unterstützen mehr Funktionen und bieten einen höheren Durchsatz bei niedrigeren Latenzen. Caches im Premium-Tarif werden auf leistungsfähigerer Hardware bereitgestellt und bieten im Vergleich zu den Tarifen „Basic“ und „Standard“ eine bessere Leistung. Dieser Vorteil bedeutet, dass der Durchsatz für einen Cache derselben Größe in der Premium-Dienstebene höher ist als in der Standard-Dienstebene. |

### <a name="feature-comparison"></a>Funktionsvergleiche
Unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/) finden Sie einen detaillierten Vergleich der einzelnen Tarife. In der folgenden Tabelle werden einige der unterstützten Features nach Tarif aufgeführt:

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

### <a name="choosing-the-right-tier"></a>Auswählen der richtigen Ebene
Berücksichtigen Sie bei der Tarifwahl für Azure Cache for Redis Folgendes:

* **Arbeitsspeicher**: Der Basic-Tarif und der Standard-Tarif bieten 250 MB bis 53 GB. Der Premium-Tarif bietet bis zu 1,2 TB (als Cluster) oder 120 GB (nicht gruppiert). Weitere Informationen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/).
* **Netzwerkleistung**: Bei einer Workload, die einen hohen Durchsatz erfordert, bietet der Premium-Tarif im Vergleich zum Standard- oder Basic-Tarif eine größere Bandbreite. Zudem haben die größeren Caches aufgrund des zugrunde liegenden virtuellen Computers, der den Cache hostet, bei jedem Tarif eine höhere Bandbreite. Weitere Informationen finden Sie unter [Azure Cache for Redis – Leistung](cache-planning-faq.md#azure-cache-for-redis-performance).
* **Durchsatz**: Der Premium-Tarif bietet den maximal verfügbaren Durchsatz. Wenn Cacheserver oder -clients die Bandbreitengrenzwerte erreichen, können Timeouts auf der Clientseite auftreten. Ausführlichere Informationen finden Sie in der unten stehenden Tabelle.
* **Hochverfügbarkeit**: Für Azure Cache for Redis wird die Verfügbarkeit eines Standard-/Premium-Caches gemäß unserer [SLA](https://azure.microsoft.com/support/legal/sla/cache/v1_0/) garantiert. Die SLA deckt nur die Konnektivität zu den Cacheendpunkten ab. Sie bezieht sich dagegen nicht auf Schutz vor Datenverlusten. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Premium-Tarif zu verwenden, um den Schutz vor Datenverlusten zu erhöhen.
* **Redis-Datenpersistenz**: Der Tarif "Premium" ermöglicht die Persistenz der Cachedaten in einem Azure Storage-Konto. In einem Basic- oder Standard-Cache werden alle Daten nur im Arbeitsspeicher gespeichert. Probleme mit der zugrunde liegenden Infrastruktur können zu potenziellen Datenverlusten führen. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Premium-Tarif zu verwenden, um den Schutz vor Datenverlusten zu erhöhen. Azure Cache for Redis bietet RDB- und AOF-Optionen (Vorschauversion) bei der Redis-Persistenz. Weitere Informationen finden Sie unter [Konfigurieren von Persistenz für Azure Cache for Redis vom Typ „Premium“](cache-how-to-premium-persistence.md).
* **Redis-Cluster**: Zum Erstellen von Caches mit einer Größe von über 120 GB oder zum horizontalen Partitionieren („Sharding“) von Daten über mehrere Redis-Knoten hinweg können Sie das im Premium-Tarif verfügbare Redis-Clustering verwenden. Für Hochverfügbarkeit besteht jeder Knoten aus einem Paar aus primärem Cache und Replikatcache. Weitere Informationen finden Sie unter [Konfigurieren von Clustern für Azure Cache for Redis vom Typ „Premium“](cache-how-to-premium-clustering.md).
* **Verbesserte Sicherheit und Netzwerkisolation**: Die Bereitstellung über Azure Virtual Network (VNET) bietet ein höheres Maß an Sicherheit und Isolation sowohl für Ihre Azure Cache for Redis-Instanz als auch für Ihre Subnetze. Sie profitieren außerdem von Richtlinien für die Zugriffssteuerung sowie von weiteren Features zur Begrenzung des Zugriffs. Weitere Informationen finden Sie unter [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Cache for Redis vom Typ „Premium“](cache-how-to-premium-vnet.md).
* **Konfigurieren von Redis**: Sowohl im Standard- als auch im Premium-Tarif können Sie Redis für Keyspace-Benachrichtigungen konfigurieren.
* **Maximale Anzahl von Clientverbindungen**: Der Premium-Tarif bietet die maximale Anzahl von Clients, die eine Verbindung mit Redis herstellen können, mit einer größeren Anzahl an Verbindungen für größere Caches. Clustering erhöht nicht die Anzahl von Verbindungen, die für einen gruppierten Cache verfügbar sind. Weitere Informationen finden Sie unter [Azure Cache for Redis – Preise](https://azure.microsoft.com/pricing/details/cache/).
* **Dedizierter Kern für Redis-Server**: Im Premium-Tarif verfügen alle Cachegrößen über einen dedizierten Kern für Redis. Im Basic- oder Standard-Tarif verfügen alle Cachegrößen ab C1 über einen dedizierten Kern für Redis-Server.
* **Singlethread-Verarbeitung**: Von Redis wird entwurfsbedingt nur ein einzelner Thread für die Befehlsverarbeitung verwendet. Von Azure Cache for Redis werden auch zusätzliche Kerne für die E/A-Verarbeitung genutzt. Die Nutzung zusätzlicher Kerne trägt zur Verbesserung der Durchsatzleistung bei, auch wenn sich dadurch ggf. keine lineare Skalierung ergibt. Darüber hinaus verfügen größere virtuelle Computer in der Regel über höhere Bandbreitengrenzwerte als kleinere. Dadurch lässt sich eine Sättigung des Netzwerks vermeiden, die Timeouts in Ihrer Anwendung zur Folge hat.
* **Leistungsverbesserungen**: Caches im Premium-Tarif werden auf Hardware mit schnelleren Prozessoren bereitgestellt, die im Vergleich zu den Tarifen Basic oder Standard eine bessere Leistung bieten. Caches im Premium-Tarif erreichen einen höheren Durchsatz und geringere Wartezeiten. Weitere Informationen finden Sie unter [Azure Cache for Redis – Leistung](cache-planning-faq.md#azure-cache-for-redis-performance).

Der Cache kann nach der Erstellung auf einen höheren Tarif skaliert werden. Die Herunterskalierung auf einen niedrigeren Tarif wird nicht unterstützt. Ausführliche Anweisungen zur Skalierung finden Sie unter [How to Scale Azure Cache for Redis](cache-how-to-scale.md) (Skalieren von Azure Cache for Redis) und [How to automate a scaling operation](cache-how-to-scale.md#how-to-automate-a-scaling-operation) (Automatisieren eines Skalierungsvorgangs).

## <a name="next-steps"></a>Nächste Schritte
* [ASP.NET-Web-App-Schnellstart](cache-web-app-howto.md): Erstellen Sie eine einfache ASP.NET-Web-App, die Azure Cache for Redis verwendet.
* [.NET-Schnellstart](cache-dotnet-how-to-use-azure-redis-cache.md): Erstellen Sie eine .NET-App, die Azure Cache for Redis verwendet.
* [.NET Core-Schnellstart](cache-dotnet-core-quickstart.md): Erstellen Sie eine .NET Core-App, die Azure Cache for Redis verwendet.
* [Node.js-Schnellstart](cache-nodejs-get-started.md): Erstellen Sie eine einfache Node.js-App, die Azure Cache for Redis verwendet.
* [Java-Schnellstart](cache-java-get-started.md): Erstellen Sie eine einfache Java-App, die Azure Cache for Redis verwendet.
* [Python-Schnellstart](cache-python-get-started.md): Erstellen Sie eine Python-App, die Azure Cache for Redis verwendet.
