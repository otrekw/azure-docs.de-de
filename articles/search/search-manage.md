---
title: Dienstverwaltung im Portal
titleSuffix: Azure Cognitive Search
description: Verwalten eines Azure Cognitive Search-Diensts (gehosteter Cloudsuchdienst in Microsoft Azure) mit dem Azure-Portal.
manager: nitinme
author: HeidiSteen
ms.author: heidist
tags: azure-portal
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/24/2020
ms.openlocfilehash: 84ddc4b427f6dc168c044f34b41e81e3b0ff19e5
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88935040"
---
# <a name="service-administration-for-azure-cognitive-search-in-the-azure-portal"></a>Dienstverwaltung für Azure Cognitive Search im Azure-Portal

> [!div class="op_single_selector"]
>
> * [PowerShell](search-manage-powershell.md)
> * [REST-API](/rest/api/searchmanagement/)
> * [.NET SDK](/dotnet/api/microsoft.azure.management.search)
> * [Portal](search-manage.md)
> * [Python](https://pypi.python.org/pypi/azure-mgmt-search/0.1.0)> 

Azure Cognitive Search ist ein vollständig verwalteter cloudbasierter Suchdienst zum Erstellen einer umfassenden Suchoberfläche für benutzerdefinierte Apps. In diesem Artikel werden die Dienstverwaltungsaufgaben beschrieben, die Sie im [Azure-Portal](https://portal.azure.com) für einen zuvor bereitgestellten Suchdienst ausführen können. Die Dienstverwaltung ist nicht sehr umfangreich und auf die folgenden Aufgaben beschränkt:

* Überprüfen des Speichers über den Link **Nutzung** auf der Mitte der Seite
* Überprüfen der Abfragevolumen und Wartezeiten über den Link **Überwachung** auf der Mitte der Seite und Überprüfen, ob Anforderungen gedrosselt wurden
* Verwalten des Zugriffs über die Seite **Schlüssel** auf der linken Seite
* Anpassen der Kapazität über die Seite **Skalierung** auf der linken Seite

Die gleichen Aufgaben, die im Portal ausgeführt werden, können auch programmgesteuert über die [Verwaltungs-APIs](/rest/api/searchmanagement/) und das [PowerShell-Modul „Az.Search“](search-manage-powershell.md) ausgeführt werden. Verwaltungsaufgaben werden vollständig über Portal- und befehlsorientierte Benutzerschnittstellen dargestellt. Es gibt keine bestimmte Verwaltungsaufgabe, die nur in einer Modalität verfügbar ist.

Azure Cognitive Search nutzt andere Azure-Dienste zur detaillierteren Überwachung und Verwaltung. An sich sind die einzigen in einem Suchdienst gespeicherten Daten Inhalte (Indizes, Indexer und Datenquellendefinitionen sowie andere Objekte). Metriken, die auf Portalseiten gemeldet werden, werden alle 30 Tage aus internen Protokollen abgerufen. Für die benutzergesteuerte Protokollaufbewahrung und zusätzliche Ereignisse benötigen Sie [Azure Monitor](../azure-monitor/index.yml). 

## <a name="fixed-service-properties"></a>Feste Diensteigenschaften

Einige Aspekte eines Suchdiensts werden beim Bereitstellen des Diensts festgelegt und können später nicht mehr geändert werden:

* Dienstname (Sie können einen Dienst nicht umbenennen)
* Dienstspeicherort (Sie können einen intakten Dienst derzeit nicht in eine andere Region verschieben)
* Maximale Anzahl von Replikaten und Partitionen (wird durch den Tarif, Basic oder Standard, bestimmt)

Wenn Sie mit dem Basic-Tarif gestartet sind, bei dem nur eine Partition zulässig ist, und nun mehr Partitionen benötigen, müssen Sie [einen neuen Dienst mit einem höheren Tarif erstellen](search-create-service-portal.md) und Ihren Inhalt im neuen Dienst neu erstellen. 

## <a name="administrator-rights"></a>Administratorrechte

Die Bereitstellung oder Außerbetriebnahme des eigentlichen Diensts kann von einem Administrator oder Co-Administrator des Azure-Abonnements durchgeführt werden.

Jeder mit Zugriff auf die Dienst-URL und einen API-Schlüssel hat auch Zugriff auf den Inhalt. Weitere Informationen zu Schlüsseln finden Sie unter [Erstellen und Verwalten von API-Schlüsseln für einen Dienst für die kognitive Azure-Suche](search-security-api-keys.md).

* Der schreibgeschützte Zugriff auf den Dienst ist in den Abfragerechten enthalten, die einer Clientanwendung üblicherweise gewährt werden, indem ihr die URL sowie ein Abfrage-API-Schlüssel zur Verfügung gestellt werden.
* Der Lese-/Schreibzugriff ermöglicht das Hinzufügen, Löschen oder Ändern von Serverobjekten, einschließlich der API-Schlüssel, Indizes, Indexer, Datenquellen und Zeitpläne. Der Lese-/Schreibzugriff wird erteilt, indem die URL und ein Administrator-API-Schlüssel bereitgestellt werden.

Rechte für das Dienstbereitstellungsgerät werden über Rollenzuweisungen gewährt. Der [rollenbasierte Zugriff in Azure (Azure Role-Based Access Control, Azure RBAC)](../role-based-access-control/overview.md) ist ein Autorisierungssystem für die Bereitstellung von Azure-Ressourcen, das auf [Azure Resource Manager](../azure-resource-manager/management/overview.md) basiert. 

Im Kontext von Azure Cognitive Search bestimmen die [Azure-Rollenzuweisungen](search-security-rbac.md), wer Aufgaben ausführen kann, unabhängig davon, ob Sie das [Portal](search-manage.md), [PowerShell](search-manage-powershell.md) oder die [Verwaltungs-REST-APIs](/rest/api/searchmanagement/search-howto-management-rest-api) verwenden:

* Erstellen oder Löschen eines Diensts
* Skalieren des Diensts
* Löschen oder erneutes Generieren von API-Schlüsseln
* Aktivieren der Diagnoseprotokollierung (Erstellen von Diensten)
* Aktivieren von Traffic Analytics (Erstellen von Diensten)

> [!TIP]
> Mithilfe von Azure-weit gültigen Mechanismen können Sie ein Abonnement oder eine Ressource sperren, um die versehentliche oder nicht autorisierte Löschung Ihres Suchdiensts durch Benutzer mit Administratorrechten zu verhindern. Weitere Informationen finden Sie unter [Sperren von Ressourcen zum Verhindern unerwarteter Löschungen](../azure-resource-manager/management/lock-resources.md).

## <a name="logging-and-system-information"></a>Protokollierungs- und Systeminformationen

Im Basic-Tarif und höher überwacht Microsoft alle Azure Cognitive Search-Dienste auf eine Verfügbarkeit von 99,9% gemäß der Vereinbarung zum Servicelevel (SLA). Wenn der Dienst langsam ist oder der Anforderungsdurchsatz unter die SLA-Schwellenwerte fällt, überprüfen Supportteams die verfügbaren Protokolldateien und kümmern sich um die Lösung des Problems.

Azure Cognitive Search nutzt [Azure Monitor](../azure-monitor/index.yml), um Indizierungs- und Abfrageaktivität zu erfassen und zu speichern. Ein Suchdienst selbst speichert nur seinen Inhalt (Indizes, Indexerdefinitionen, Datenquellendefinitionen, Skillsetdefinitionen, Synonymzuordnungen). Zwischenspeicherungen und protokollierte Informationen werden außerhalb des Diensts gespeichert, oftmals in einem Azure Storage-Konto. Weitere Informationen zum Protokollieren von Indizierungs- und Abfrageworkloads finden Sie unter [Sammeln und Analysieren von Protokolldaten für Azure Cognitive Search](search-monitor-logs.md).

Allgemeine Informationen zu Ihrem Dienst erhalten Sie, wenn Sie nur die Möglichkeiten in Azure Cognitive Search verwenden, folgendermaßen:

* Über die Dienstseite **Übersicht** durch Benachrichtigungen, Eigenschaften und Statusmeldungen
* Über [PowerShell](search-manage-powershell.md) oder die [REST-API für die Verwaltung](/rest/api/searchmanagement/), um [Diensteigenschaften abzurufen](/rest/api/searchmanagement/services): Auf programmgesteuerter Ebene werden keine neuen Informationen oder Vorgänge bereitgestellt. Die Schnittstellen sind vorhanden, sodass Sie Skripts schreiben können.

## <a name="monitor-resource-usage"></a>Überwachen der Ressourcenauslastung

Die Ressourcenüberwachung im Dashboard ist auf die Informationen im Dienst-Dashboard und einige weitere Metriken beschränkt, die Sie durch Abfragen an den Dienst erhalten. Im Bereich Nutzung im Dienst-Dashboard können Sie direkt ablesen, ob die Partitionsressourcen für Ihre Anwendung angemessen sind. Sie können externe Ressourcen, wie z.B. die Azure-Überwachung, bereitstellen, wenn Sie protokollierte Ereignisse erfassen und aufzeichnen möchten. Weitere Informationen finden Sie unter [Überwachung von Azure Cognitive Search](search-monitor-usage.md).

Mit der Suchdienst-REST-API können Sie Gesamtzahlen für Dokumente und Indizes programmgesteuert abrufen: 

* [Indexstatistiken abrufen](/rest/api/searchservice/Get-Index-Statistics)
* [Dokumentenanzahl](/rest/api/searchservice/count-documents)

## <a name="disaster-recovery-and-service-outages"></a>Notfallwiederherstellung und Dienstausfälle

Obwohl wir Ihre Daten retten können, bietet Azure Cognitive Search kein sofortiges Failover des Diensts, wenn ein Ausfall auf Cluster- oder Rechenzentrumsebene auftritt. Wenn ein Cluster im Rechenzentrum fehlschlägt, wird das Betriebsteam dieses ermitteln und daran arbeiten, den Dienst wiederherzustellen. Während der Dienstwiederherstellung kommt es zu Ausfallzeiten, Sie können jedoch laut [Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/) Dienstguthaben anfordern, um damit die Nichtverfügbarkeit des Diensts kompensieren zu können. 

Sollte ein unterbrechungsfreier Dienst im Falle schwerwiegender Fehler, die nicht der Kontrolle von Microsoft unterliegen, benötigt werden, können Sie [einen zusätzlichen Dienst](search-create-service-portal.md) in einer anderen Region bereitstellen und eine Georeplikationsstrategie implementieren, um sicherzustellen, dass Indizes für alle Dienste vollständig redundant sind.

Kunden, die [Indexer](search-indexer-overview.md) zum Auffüllen und Aktualisieren von Indizes verwenden, können die Notfallwiederherstellung über geospezifische Indizes ausführen, welche die gleiche Datenquelle verwenden. Zwei Dienste in unterschiedlichen Regionen, die jeweils einen Indexer ausführen, könnten die gleiche Datenquelle indizieren, um Georedundanz zu erzielen. Wenn Sie Daten aus Datenquellen indizieren, die auch georedundant sind, sollten Sie darauf achten, dass Azure Cognitive Search-Indexer eine inkrementelle Indizierung (Zusammenführen von Updates neuer, geänderter oder gelöschter Dokumente) nur aus primären Replikaten ausführen können. Achten Sie im Falle eines Failoverereignisses darauf, den Indexer erneut auf das neue primäre Replikat auszurichten. 

Wenn Sie keine Indexer verwenden, können Sie Ihren Anwendungscode verwenden, um Objekte und Daten gleichzeitig per Pushvorgang an verschiedene Dienste zu übermitteln. Weitere Informationen finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Cognitive Search](search-performance-optimization.md).

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Da Azure Cognitive Search keine primäre Datenspeicherlösung ist, wird kein formales Verfahren für Self-Service-Sicherung und -Wiederherstellung bereitgestellt. Sie können jedoch den **index-backup-restore**-Beispielcode in diesem [Azure Cognitive Search .NET-Beispielrepository](https://github.com/Azure-Samples/azure-search-dotnet-samples) verwenden, um Ihre Indexdefinition und die Momentaufnahme in einer Reihe von JSON-Dateien zu sichern, und diese Dateien dann bei Bedarf zu verwenden, um den Index wiederherzustellen. Mit diesem Tool können Sie auch Indizes zwischen Dienstebenen verschieben.

Andernfalls ist Ihr Anwendungscode, der zum Erstellen und Auffüllung eines Index verwendet wird, de facto die Wiederherstellungsoption, wenn Sie aus Versehen einen Index löschen. Um einen Index neu zu erstellen, würden Sie ihn (sofern vorhanden) löschen, den Index im Dienst wiederherstellen und ihn erneut laden, indem Sie Daten aus Ihrem primären Datenspeicher abrufen.

## <a name="scale-up-or-down"></a>Hoch- oder Herunterskalieren

Jeder Suchdienst enthält zunächst ein Minimum von einem Replikat und einer Partition. Wenn Sie sich für einen [Tarif mit mehr Kapazität registriert haben](search-limits-quotas-capacity.md), klicken Sie im linken Navigationsbereich auf **Skalieren**, um die Ressourcennutzung anzupassen.

Wenn Sie zusätzliche Kapazität über eine dieser Ressourcen hinzufügen, wird diese vom Dienst automatisch verwendet. Sie müssen nichts weiter tun. Allerdings tritt eine kleine Verzögerung auf, bevor die Auswirkungen der neuen Ressource spürbar sind. Die Bereitstellung zusätzlicher Ressourcen kann 15 Minuten oder mehr in Anspruch nehmen.

### <a name="add-replicas"></a>Hinzufügen von Replikaten

Fügen Sie Replikate hinzu, um die Anzahl der Abfragen pro Sekunde (Queries per Second QPS) zu steigern oder Hochverfügbarkeit einzurichten. Jedes Replikat enthält eine Kopie eines Index. Jedes zusätzliche Replikat führt also zu einem zusätzlichen Index, der zur Behandlung von Abfragen verwendet werden kann. Für die Hochverfügbarkeit sind mindestens drei Replikate erforderlich (weitere Informationen finden Sie unter [Anpassen der Kapazität in Azure Cognitive Search](search-capacity-planning.md)).

Suchdienste mit mehr Replikaten können Abfragen per Lastenausgleich auf eine größere Anzahl von Indizes verteilen. Für ein gegebenes Abfragevolumen ist ein höherer Durchsatz an Abfragen möglich, wenn mehr Kopien des Index zur Beantwortung bereitstehen. Wenn Sie eine hohe Latenz in Ihren Abfragen bemerken, können Sie eine positive Auswirkung auf die Leistung erwarten, sobald die zusätzlichen Replikate online sind.

Auch wenn der Abfragedurchsatz beim Hinzufügen von Replikaten ansteigt, wird dieser mit zusätzlichen Replikaten nicht exakt verdoppelt, verdreifacht usw. Alle Suchanwendungen unterliegen externen Faktoren, die die Abfrageleistung beeinträchtigen können. Komplexe Abfragen und Netzwerklatenz sind zwei Faktoren, die die Antwortzeiten von Abfragen beeinflussen.

### <a name="add-partitions"></a>Hinzufügen von Partitionen

Es ist üblicher, Replikate hinzuzufügen. Wenn der Speicher jedoch eingeschränkt ist, können Sie mehr Partitionen hinzufügen, um mehr Kapazität zu erreichen. Durch den Tarif, mit dem Sie den Dienst bereitgestellt haben, wird bestimmt, ob Partitionen hinzugefügt werden können. Im Basic-Tarif kann nur eine Partition verwendet werden. In den Standard-Tarifen und höheren Tarifen werden zusätzliche Partitionen unterstützt.

Partitionen werden in Teilern von 12 hinzugefügt (1, 2, 3, 4, 6 oder 12). Dies ist ein Artefakt für das Sharding (Horizontales Partitionieren). Indizes werden in 12 Shards erstellt, die entweder in 1 Partition gespeichert oder in gleichen Teilen auf 2, 3, 4, 6 oder 12 Partitionen (1 Shard pro Partition) verteilt werden können.

### <a name="remove-replicas"></a>Entfernen von Replikaten

Nach Zeiten mit hohem Abfragevolumen können Sie mit dem Schieberegler die Replikate reduzieren, wenn sich die Suchabfragelast wieder normalisiert hat (z. B. nach Abschluss des Weihnachtsgeschäfts). Keine weiteren Schritte sind erforderlich. Beim Senken der Replikatanzahl werden virtuelle Computer im Rechenzentrum freigegeben. Ihre Abfrage- und Datenerfassungsoperationen werden nun auf weniger VMs ausgeführt als zuvor. Als Mindestanforderung muss ein Replikat vorhanden sein.

### <a name="remove-partitions"></a>Entfernen von Partitionen

Im Gegensatz zum Entfernen von Replikaten, bei dem Sie keine weiteren Schritte ausführen müssen, erfordert die Senkung des verbrauchten Speicherplatzes unter Umständen einigen Arbeitsaufwand. Wenn Sie in der Lösung beispielsweise drei Partitionen verwenden, wird bei der Reduzierung auf ein oder zwei Partitionen ein Fehler generiert, falls der neue Speicherplatz geringer ist als für das Hosten des Indexes erforderlich. Wie Sie sicherlich schon vermuten, können Sie entweder Indizes oder Dokumente innerhalb eines Index löschen, um Speicherplatz freizugeben, oder die aktuelle Konfiguration beibehalten.

Es ist nicht möglich, herauszufinden, welche Index-Shards auf welcher Partition gespeichert sind. Jede Partition bietet ca. 25 GB an Speicherplatz an. Sie müssen Ihren Speicherverbrauch also auf einen Wert senken, der von Ihren konfigurierten Partitionen unterstützt werden kann. Wenn Sie auf eine Partition zurückschalten, müssen alle 12 Shards in diese Partition passen.

Für die zukünftige Planung können Sie den Speicherverbrauch prüfen (siehe [Abrufen der Index-Statistiken](/rest/api/searchservice/Get-Index-Statistics)), um herauszufinden, wie viel Speicher Sie tatsächlich nutzen. 

## <a name="next-steps"></a>Nächste Schritte

* Automatisieren mit [PowerShell](search-manage-powershell.md)

* Informieren über Techniken zur [Leistung und Optimierung](search-performance-optimization.md)

* Informieren über [Sicherheitsfeatures](search-security-overview.md) zum Schützen von Inhalt und Vorgängen

* Aktivieren der [Diagnoseprotokollierung](search-monitor-logs.md) zum Überwachen von Abfrage- und Indizierungsworkloads