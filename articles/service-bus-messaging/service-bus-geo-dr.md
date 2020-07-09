---
title: Georedundante Notfallwiederherstellung in Azure Service Bus | Microsoft-Dokumentation
description: Verwenden von geografischen Regionen für das Failover und zum Durchführen der Notfallwiederherstellung in Azure Service Bus
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: fcdeb499b8ebecc4ecddbfcbe32b812ce7e3efe5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341484"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Georedundante Notfallwiederherstellung in Azure Service Bus

Falls gesamte Azure-Regionen oder -Datencenter ausfallen (wenn keine [Verfügbarkeitszonen](../availability-zones/az-overview.md) verwendet werden), ist es von entscheidender Bedeutung, dass die Datenverarbeitung in einer anderen Region oder in einem anderen Datencenter fortgesetzt werden kann. Daher ist die *georedundante Notfallwiederherstellung* eine wichtige Funktion für jedes Unternehmen. Azure Service Bus unterstützt die georedundante Notfallwiederherstellung auf Namespaceebene.

Das Feature für die georedundante Notfallwiederherstellung ist für die Service Bus Premium-SKU global verfügbar. 

>[!NOTE]
> Mit der georedundanten Notfallwiederherstellung wird derzeit nur sichergestellt, dass die Metadaten (Warteschlangen, Themen, Abonnements und Filter) vom primären Namespace in den sekundären Namespace kopiert werden, wenn sie gekoppelt sind.

## <a name="outages-and-disasters"></a>Ausfälle und Notfälle

Es ist wichtig, dass Sie den Unterschied zwischen „Ausfällen“ und „Notfällen“ kennen. 

Ein *Ausfall* ist die vorübergehende Nichtverfügbarkeit von Azure Service Bus und kann einige Komponenten des Diensts, z.B. einen Nachrichtenspeicher, oder selbst das gesamte Rechenzentrum betreffen. Nachdem das Problem behoben wurde, ist Service Bus aber wieder verfügbar. In der Regel führt ein Ausfall nicht zum Verlust von Nachrichten oder anderen Daten. Ein Beispiel für einen Ausfall ist ein Stromausfall im Rechenzentrum. Bei einigen Ausfällen handelt es sich nur um kurze Verbindungsunterbrechungen aufgrund von vorübergehenden Problemen bzw. Netzwerkproblemen. 

Ein *Notfall* ist als dauerhafter oder längerer Ausfall eines Service Bus-Clusters, einer Azure-Region oder eines Datencenters definiert. Die Region oder das Datencenter wird ggf. wieder verfügbar, kann aber auch für mehrere Stunden oder Tage ausfallen. Beispiele für Notfälle sind Feuer, Überflutung und Erdbeben. Ein Notfall, der dauerhaft wird, kann zum Verlust von Nachrichten, Ereignissen oder anderen Daten führen. In den meisten Fällen kommt es allerdings zu keinem Datenverlust, und Nachrichten können wiederhergestellt werden, sobald das Rechenzentrum gesichert ist.

Die Funktion der georedundanten Notfallwiederherstellung von Azure Service Bus ist eine Lösung zur Notfallwiederherstellung. Die Konzepte und der Workflow, die in diesem Artikel beschrieben werden, gelten für Notfallszenarien und nicht für vorübergehende Ausfälle. Eine ausführliche Erläuterung der Notfallwiederherstellung in Microsoft Azure finden Sie in [diesem Artikel](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Allgemeine Konzepte und Begriffe

Bei der Funktion zur Notfallwiederherstellung wird die Notfallwiederherstellung von Metadaten implementiert, und sie basiert auf speziellen primären und sekundären Namespaces. Beachten Sie, dass die Funktion zur georedundanten Notfallwiederherstellung nur für die [Premium-SKU](service-bus-premium-messaging.md) verfügbar ist. Sie müssen keine Änderungen an den Verbindungszeichenfolgen vornehmen, da die Verbindung über einen Alias hergestellt wird.

In diesem Artikel werden die folgenden Begriffe verwendet:

-  *Alias*: Der Name für die Konfiguration einer von Ihnen eingerichteten Notfallwiederherstellung. Der Alias stellt einen einzelnen, stabilen, vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) als Verbindungszeichenfolge bereit. Anwendungen verwenden diese Alias-Verbindungszeichenfolge, um eine Verbindung mit einem Namespace herzustellen. Mit einem Alias wird sichergestellt, dass die Verbindungszeichenfolge unverändert bleibt, wenn das Failover ausgelöst wird.

-  *Primärer/sekundärer Namespace*: Die Namespaces, die dem Alias entsprechen. Der primäre Namespace ist aktiv und empfängt Nachrichten. (Dies kann ein bereits vorhandener oder ein neuer Namespace sein.) Der sekundäre Namespace ist passiv und empfängt keine Nachrichten. Die Metadaten zwischen beiden Namespaces werden synchronisiert, sodass beide nahtlos und ohne Änderung des Anwendungscodes oder der Verbindungszeichenfolge Nachrichten entgegennehmen können. Um sicherzustellen, dass nur der aktive Namespace Nachrichten empfängt, müssen Sie den Alias verwenden. 

-  *Metadaten*: Entitäten, z.B. Warteschlangen, Themen und Abonnements und die dazugehörigen Eigenschaften des Diensts, die dem Namespace zugeordnet sind. Beachten Sie, dass nur Entitäten und ihre Einstellungen automatisch repliziert werden. Nachrichten werden nicht repliziert.

-  *Failover*: Der Vorgang zum Aktivieren des sekundären Namespace.

## <a name="setup"></a>Einrichten

Der folgende Abschnitt enthält eine Übersicht über die Einrichtung der Kopplung zwischen den Namespaces.

![1][]

Der Einrichtungsvorgang verläuft wie folgt:

1. Sie stellen einen ***primären*** Service Bus Premium-Namespace bereit.

2. Sie stellen einen ***sekundären*** Service Bus Premium-Namespace in *einer anderen Region* als der Region bereit, in der der primäre Namespace bereitgestellt wird. Dies ermöglicht die Fehlerisolierung zwischen unterschiedlichen Rechenzentrumsregionen.

3. Sie erstellen die Kopplung zwischen dem primären und dem sekundären Namespace, um den ***Alias*** zu erhalten.

    >[!NOTE] 
    > Wenn Sie [Ihren Azure Service Bus Standard-Namespace zu Azure Service Bus Premium migriert haben](service-bus-migrate-standard-premium.md), müssen Sie den bereits vorhandenen Alias (d.h. Ihre Service Bus Standard-Namespaceverbindungszeichenfolge) verwenden, um die Notfallwiederherstellungs-Konfiguration über die **PS/CLI** oder **REST-API** zu erstellen.
    >
    >
    > Dies liegt daran, dass während der Migration Ihre Azure Service Bus Standard-Namespaceverbindungszeichenfolge bzw. der DNS-Name selbst zu einem Alias für Ihren Azure Service Bus Premium-Namespace geworden ist.
    >
    > Ihre Clientanwendungen müssen mit diesem Alias (z.B. der Azure Service Bus Standard-Namespaceverbindungszeichenfolge) die Verbindung mit dem Premium-Namespace herstellen, in dem die Kopplung für die Notfallwiederherstellung eingerichtet wurde.
    >
    > Wenn Sie das Portal verwenden, um die Konfiguration für die Notfallwiederherstellung einzurichten, wird das Portal diesen Nachteil vor Ihnen verbergen.


4. Mithilfe des ***Alias*** aus Schritt 3 verbinden Sie Ihre Clientanwendungen mit dem für die georedundante Notfallwiederherstellung aktivierten primären Namespace. Der Alias verweist zunächst auf den primären Namespace.

5. [Optional:] Sie fügen einige Überwachungsfunktionen hinzu, um zu ermitteln, ob ein Failover erforderlich ist.

## <a name="failover-flow"></a>Failoverablauf

Ein Failover wird manuell vom Kunden (explizit durch einen Befehl oder über Geschäftslogik des Clients, mit der der Befehl ausgelöst wird) und nie von Azure ausgelöst. Dadurch erhält der Kunde den vollständigen Besitz und die vollständige Transparenz für die Auflösung von Ausfällen im Backbone von Azure.

![4][]

Nach dem Auslösen des Failovers:

1. Die Verbindungszeichenfolge des ***Alias*** wird so aktualisiert, dass sie auf den sekundären Premium-Namespace verweist.

2. Clients (Sender und Empfänger) stellen automatisch eine Verbindung mit dem sekundären Namespace her.

3. Die vorhandene Kopplung zwischen dem primären und sekundären Premium-Namespace wird getrennt.

Nach dem Initiieren des Failovers:

1. Bei einem weiteren Ausfall muss ein erneutes Failover möglich sein. Richten Sie daher einen weiteren passiven Namespace ein, und aktualisieren Sie die Kopplung. 

2. Übertragen Sie Nachrichten mithilfe von Pull aus dem primären Namespace, nachdem er wieder verfügbar ist. Verwenden Sie diesen Namespace danach für das reguläre Messaging außerhalb Ihrer eingerichteten geografischen Wiederherstellung, oder löschen Sie den alten primären Namespace.

> [!NOTE]
> Es wird nur die Semantik für „Fail Forward“ unterstützt. In diesem Szenario führen Sie das Failover und anschließend die Reparatur mit einem neuen Namespace durch. Ein Failback wird nicht unterstützt, z.B. in einem SQL-Cluster. 

Sie können das Failover entweder mit Überwachungssystemen oder mit benutzerdefinierten Überwachungslösungen automatisieren. Für diese Art der Automatisierung sind aber zusätzliche Planungs- und Arbeitsschritte erforderlich, was den Rahmen dieses Artikels sprengen würde.

![2][]

## <a name="management"></a>Verwaltung

Falls Sie einen Fehler gemacht haben (z.B. eine Kopplung der falschen Regionen während des anfänglichen Setups), können Sie die Kopplung der beiden Namespaces jederzeit trennen. Wenn Sie die gekoppelten Namespaces als reguläre Namespaces verwenden möchten, löschen Sie den Alias.

## <a name="use-existing-namespace-as-alias"></a>Verwenden des vorhandenen Namespace als Alias

Bei einem Szenario, für das Sie die Verbindungen von Producern und Consumern nicht ändern können, können Sie Ihren Namespacenamen als Aliasnamen verwenden. Den Beispielcode finden Sie auf [GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Beispiele

In den [Beispielen auf GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) wird veranschaulicht, wie Sie ein Failover einrichten und initiieren. In diesen Beispielen werden die folgenden Konzepte veranschaulicht:

- Ein .NET-Beispiel und Einstellungen, die in Azure Active Directory erforderlich sind, um unter Verwendung von Azure Resource Manager und Service Bus eine georedundante Notfallwiederherstellung einzurichten und zu aktivieren.
- Schritte, die zum Ausführen des Beispielcodes erforderlich sind
- Verwenden eines vorhandenen Namespace als Alias
- Schritte zur alternativen Aktivierung der georedundanten Notfallwiederherstellung über PowerShell oder die Befehlszeilenschnittstelle
- [Senden und Empfangen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) aus dem aktuellen primären oder sekundären Namespace unter Verwendung des Alias

## <a name="considerations"></a>Überlegungen

Beachten Sie für diesen Release Folgendes:

1. Berücksichtigen Sie bei der Failoverplanung auch den Zeitfaktor. Falls beispielsweise länger als 15 bis 20 Minuten keine Konnektivität vorhanden ist, empfiehlt es sich unter Umständen, das Failover zu initiieren.

2. Die Tatsache, dass keine Daten repliziert werden, bedeutet, dass derzeit keine aktiven Sitzungen repliziert werden. Außerdem kann es sein, dass die Duplikaterkennung und geplante Nachrichten nicht funktionieren. Neue Sitzungen, neue geplante Nachrichten und neue Duplikate funktionieren. 

3. Die Durchführung eines Failovers für eine komplexe verteilte Infrastruktur sollte mindestens einmal [durchgespielt](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) werden.

4. Das Synchronisieren von Entitäten kann einige Zeit dauern (etwa eine Minute pro 50 bis 100 Entitäten). Abonnements und Regeln zählen ebenfalls zu den Entitäten.

## <a name="availability-zones"></a>Verfügbarkeitszonen

Die Service Bus-Premium-SKU unterstützt zudem [Verfügbarkeitszonen](../availability-zones/az-overview.md), die fehlerisolierte Standorte innerhalb einer Azure-Region bieten.

> [!NOTE]
> Die Unterstützung für Verfügbarkeitszonen für Azure Service Bus Premium ist nur in [Azure-Regionen](../availability-zones/az-region.md) verfügbar, in denen Verfügbarkeitszonen vorhanden sind.

Sie können Verfügbarkeitszonen nur für neue Namespaces über das Azure-Portal aktivieren. Service Bus bietet keine Unterstützung für die Migration vorhandener Namespaces. Sie können die Zonenredundanz nicht deaktivieren, wenn Sie sie für Ihren Namespace aktiviert haben.

![3][]

## <a name="private-endpoints"></a>Private Endpunkte
Dieser Abschnitt enthält zusätzliche Überlegungen zur Verwendung der georedundanten Notfallwiederherstellung mit Namespaces, bei denen private Endpunkte verwendet werden. Allgemeine Informationen zur Verwendung von privaten Endpunkten mit Service Bus finden Sie unter [Integrieren von Azure Service Bus in den Azure Private Link-Dienst](private-link-service.md).

### <a name="new-pairings"></a>Neue Kopplungen
Wenn Sie versuchen, eine Kopplung zwischen einem primären Namespace mit einem privaten Endpunkt und einem sekundären Namespace ohne einen privaten Endpunkt zu erstellen, wird die Kopplung nicht durchgeführt. Die Kopplung erfolgt nur, wenn sowohl der primäre als auch der sekundäre Namespace einen privaten Endpunkt aufweist. Wir empfehlen Ihnen, für den primären und den sekundären Namespace sowie für virtuelle Netzwerke, in denen private Endpunkte erstellt werden, die gleichen Konfigurationen zu verwenden. 

> [!NOTE]
> Wenn Sie versuchen, den primären Namespace mit einem privaten Endpunkt und dem sekundären Namespace zu koppeln, prüft der Überprüfungsprozess lediglich, ob ein privater Endpunkt im sekundären Namespace vorhanden ist. Es wird nicht überprüft, ob der Endpunkt funktioniert oder nach dem Failover ausgeführt wird. Sie selbst müssen sicherstellen, dass der sekundäre Namespace mit dem privaten Endpunkt nach dem Failover erwartungsgemäß funktioniert.
>
> Gehen Sie wie folgt vor, um die Konfigurationen des privaten Endpunkts zu testen: Senden Sie von einem Ort außerhalb des virtuellen Netzwerks eine [Get queues](/rest/api/servicebus/queues/get)-Anforderung an den sekundären Namespace, und überprüfen Sie, ob Sie eine Fehlermeldung vom Dienst erhalten.

### <a name="existing-pairings"></a>Vorhandene Kopplungen
Wenn die Kopplung zwischen dem primären und dem sekundären Namespace bereits vorhanden ist, kann für den primären Namespace kein privater Endpunkt erstellt werden. Erstellen Sie zunächst einen privaten Endpunkt für den sekundären Namespace und dann einen privaten Endpunkt für den primären Endpunkt, um das Problem zu beheben.

> [!NOTE]
> Für den sekundären Namespace ist nur Lesezugriff möglich. Konfigurationen des privaten Endpunkts können dagegen aktualisiert werden. 

### <a name="recommended-configuration"></a>Empfohlene Konfiguration
Beim Erstellen einer Konfiguration für die Notfallwiederherstellung Ihrer Anwendung und Service Bus müssen Sie private Endpunkte für primäre und sekundäre Service Bus-Namespaces für die virtuellen Netzwerke erstellen, die primäre und sekundäre Instanzen der Anwendung enthalten.

Angenommen, Sie verfügen über die beiden virtuellen Netzwerke VNET-1 und VNET-2 sowie diesen primären und sekundären Namespace: ServiceBus-Namespace1-Primary, ServiceBus-Namespace2-Secondary. Sie müssen dann die folgenden Schritte ausführen: 

- Erstellen Sie für „ServiceBus-Namespace1-Primary“ zwei private Endpunkte, an denen Subnetze aus VNET-1 und VNET-2 verwendet werden.
- Erstellen Sie für „ServiceBus-Namespace2-Secondary“ zwei private Endpunkte, an denen dieselben Subnetze aus VNET-1 und VNET-2 verwendet werden. 

![Private Endpunkte und virtuelle Netzwerke](./media/service-bus-geo-dr/private-endpoints-virtual-networks.png)


Der Vorteil dieses Ansatzes besteht darin, dass ein Failover auf Anwendungsebene unabhängig vom Service Bus-Namespace erfolgen kann. Betrachten Sie die folgenden Szenarien: 

**Nur Anwendungsfailover:** In diesem Fall ist die Anwendung nicht in VNET-1 vorhanden, wird aber in VNET-2 verschoben. Da beide privaten Endpunkte sowohl in VNET-1 als auch in VNET-2 für primäre und sekundäre Namespaces konfiguriert sind, wird die Anwendung ausgeführt. 

**Nur Failover für Service Bus-Namespaces:** Da auch in diesem Fall beide privaten Endpunkte in beiden virtuellen Netzwerken für primäre und sekundäre Namespaces konfiguriert sind, wird die Anwendung ausgeführt. 

> [!NOTE]
> Eine Anleitung zur georedundanten Notfallwiederherstellung eines virtuellen Netzwerks finden Sie unter [Virtuelles Netzwerk: Geschäftskontinuität](../virtual-network/virtual-network-disaster-recovery-guidance.md).

## <a name="next-steps"></a>Nächste Schritte

- [Hier](/rest/api/servicebus/disasterrecoveryconfigs) finden Sie Informationen zur REST-API-Referenz für die georedundante Notfallwiederherstellung.
- Führen Sie das [Beispiel auf GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2) zur georedundanten Notfallwiederherstellung aus.
- Sehen Sie sich das [Beispiel zur georedundanten Notfallwiederherstellung, mit dem Nachrichten an einen Alias gesendet werden](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) an.

Weitere Informationen zum Service Bus-Messaging finden Sie in folgenden Artikeln:

* [Service Bus-Warteschlangen, -Themen und -Abonnements](service-bus-queues-topics-subscriptions.md)
* [Erste Schritte mit Service Bus-Warteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Verwenden von Service Bus-Themen und -Abonnements](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST-API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png
