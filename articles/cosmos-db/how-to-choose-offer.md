---
title: Auswählen des richtigen Durchsatzangebots in Azure Cosmos DB
description: In diesem Artikel erfahren Sie, wie Sie für Ihre Workload zwischen dem standardmäßig (manuell) bereitgestellten und dem automatisch skalierten Durchsatz auswählen.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: dech
ms.openlocfilehash: b8c4fd3804bfa02b86f62912641eb936ff8cd15e
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83662928"
---
# <a name="how-to-choose-between-standard-manual-and-autoscale-provisioned-throughput"></a>Auswählen zwischen standardmäßig (manuell) bereitgestelltem und automatisch skaliertem Durchsatz 

Azure Cosmos DB unterstützt zwei Arten oder Angebote von bereitgestelltem Durchsatz: den standardmäßig (manuell) bereitgestellten und den automatisch skalierten Durchsatz. Beide Durchsatztypen eignen sich für unternehmenskritische Workloads, die eine hohe Leistung und Skalierung erfordern und hinsichtlich des Durchsatzes, der Verfügbarkeit, der Latenz und der Konsistenz denselben Azure Cosmos DB-SLAs unterliegen.

In diesem Artikel wird beschrieben, wie Sie für Ihre Workload zwischen dem standardmäßig (manuell) bereitgestellten und dem automatisch skalierten Durchsatz auswählen. 

## <a name="overview-of-provisioned-throughput-types"></a>Übersicht über bereitgestellte Durchsatztypen
Bevor Sie sich eingehender mit den Unterschieden zwischen dem standardmäßig (manuell) bereitgestellten und dem automatisch skalierten Durchsatz befassen, müssen Sie zunächst verstehen, wie der bereitgestellte Durchsatz in Azure Cosmos DB funktioniert. 

Wenn Sie bereitgestellten Durchsatz verwenden, legen Sie den Durchsatz fest. Dieser wird in Anforderungseinheiten pro Sekunde (Request Units per Second, RU/s) gemessen, die für Ihre Workload erforderlich sind. Der Dienst stellt die Kapazität bereit, die zur Erfüllung der Durchsatzanforderungen benötigt wird. Datenbankvorgänge des Diensts, z. B. Lese-, Schreib- und Abfragevorgänge, verbrauchen eine bestimmte Menge von Anforderungseinheiten. Weitere Informationen zu [Anforderungseinheiten](request-units.md)

In der folgenden Tabelle wird der standardmäßig (manuell) bereitgestellte Durchsatz allgemein mit dem automatisch skalierten Durchsatz verglichen.

|BESCHREIBUNG|Standard (manuell)|Autoscale|
|-------------|------|-------|
|Am besten geeignet für|Workloads mit stetigem oder vorhersagbarem Datenverkehr|Workloads mit variablem oder nicht vorhersagbarem Datenverkehr Weitere Informationen finden Sie unter [Anwendungsfälle des automatisch skalierten Durchsatzes](provision-throughput-autoscale.md#use-cases-of-autoscale).|
|Funktionsweise|Sie stellen eine festgelegte Anzahl von RU/s (`T`) bereit, die sich im Zeitverlauf nicht ändert, sofern Sie sie nicht manuell bearbeiten. Pro Sekunde können Sie einen Durchsatz von bis zu `T` RU/s verwenden. <br/><br/>Wenn Sie den standardmäßig (manuell) bereitgestellten Durchsatz z. B. auf 400 RU/s festlegen, bleibt dieser auch bei 400 RU/s.|Sie legen die maximale Anzahl von RU/s fest (`Tmax`), die das System nicht überschreiten soll. Der Durchsatz `T` wird im System automatisch skaliert, sodass `0.1* Tmax <= T <= Tmax`. <br/><br/>Wenn Sie die maximale Anzahl von RU/s für die Autoskalierung z. B. auf 4.000 RU/s festlegen, wird im System zwischen 400–4.000 RU/s skaliert.|
|Einsatzgebiete|Sie möchten Ihre Durchsatzkapazität (RU/s) manuell verwalten und selbst skalieren.<br/><br/>Ihre bereitgestellten RU/s werden stark und konsistent genutzt. Wenn Sie in einem Monat eine bestimmte Anzahl `T` von RU/s bereitstellen und diese in 66 % (oder mehr) der Stunden aufbrauchen, werden Sie wahrscheinlich mit dem standardmäßig (manuell) bereitgestellten Durchsatz sparen.<br/><br/>Diese Schätzung basiert auf einem Vergleich zwischen dem Wert von `T` beim standardmäßig (manuell) bereitgestellten Durchsatz und dem Wert von `Tmax` beim automatisch skalierten Durchsatz. |Azure Cosmos DB soll Ihre Durchsatzkapazität (RU/s) und die Skalierung der Nutzung entsprechend verwalten.<br/><br/>Die Nutzung Ihrer RU/s ist variabel oder schwer vorhersagbar. Wenn Sie in einem Monat eine bestimmte Höchstzahl `Tmax` von RU/s per Autoskalierung bereitstellen und diese Höchstzahl `Tmax` in 66 % (oder weniger) der Stunden aufbrauchen, werden Sie wahrscheinlich mit dem automatisch skalierten Durchsatz sparen.<br/><br/>Diese Schätzung basiert auf einem Vergleich zwischen dem Wert von `Tmax` beim automatisch skalierten Durchsatz und dem Wert von `T` beim standardmäßig (manuell) bereitgestellten Durchsatz.|
|Abrechnungsmodell|Die Abrechnung erfolgt auf Stundenbasis für die bereitgestellten RU/s und unabhängig davon, wie viele RUs verbraucht wurden.<br/><br/>Beispiel: <li>Sie stellen 400 RU/s bereit.</li><li>1\. Stunde: keine Anforderungen</li><li>2\. Stunde: Anforderungen im Umfang von 400 RU/s</li><br/><br/>Für beide Stunden (erste und zweite) werden Ihnen 400 RU/s zum [Tarif des standardmäßig (manuell) bereitgestellten Durchsatzes](https://azure.microsoft.com/pricing/details/cosmos-db/) in Rechnung gestellt.|Die Abrechnung erfolgt auf Stundenbasis. Ihnen wird die höchste Anzahl von RU/s berechnet, auf die das System in der Stunde skaliert wurde. <br/><br/>Beispiel: <li>Sie legen als Höchstzahl für die Autoskalierung 4.000 RU/s fest (System skaliert von 400 bis 4.000 RU/s).</li><li>1\. Stunde: Das System wurde auf einen Höchstwert von 3.500 RU/s skaliert.</li><li>2\. Stunde: Das System wurde aufgrund von zu geringer Auslastung auf den Minimalwert von 400 RU/s (immer 10 % von `Tmax`) herunterskaliert.</li><br/><br/>Ihnen werden die 3.500 RU/s in der 1. Stunde und die 400 RU/s in 2. Stunde zum [Tarif des automatisch skalierten Durchsatzes](https://azure.microsoft.com/pricing/details/cosmos-db/) in Rechnung gestellt. Der für die Autoskalierung geltende Tarif pro RU/s beträgt das 1,5-Fache des Tarifs für den standardmäßig (manuell) bereitgestellten Durchsatz.
|Was geschieht, wenn die bereitgestellten RU/s überschritten werden?|Die Anzahl der bereitgestellten RU/s bleibt konstant bei der festgelegten Menge. Alle Anforderungen, die in einer Sekunde die Anzahl der bereitgestellten RU/s übersteigen, unterliegen einer Durchsatzbeschränkung. In der Antwort wird empfohlen, vor einem neuen Versuch noch etwas zu warten. Sie können die Anzahl der RU/s bei Bedarf manuell erhöhen oder verringern.| Das System skaliert die RU/s bis hin zu der für die Autoskalierung festgelegten Höchstzahl von RU/s. Alle Anforderungen, die in einer Sekunde die Höchstzahl der automatisch skalierten RU/s übersteigen, unterliegen einer Durchsatzbeschränkung. In der Antwort wird empfohlen, vor einem neuen Versuch noch etwas zu warten.|

## <a name="understand-your-traffic-patterns"></a>Grundlegendes zu Ihren Datenverkehrsmustern

### <a name="new-applications"></a>Neue Anwendungen ###
Wenn Sie eine neue Anwendung erstellen und Ihr Datenverkehrsmuster noch nicht kennen, sollten Sie mit den Einstiegspunkt-RU/s (oder der Mindestzahl von RU/s) beginnen, um eine überflüssige Bereitstellung am Anfang zu vermeiden. Wenn Ihre Anwendung klein ist und keine hohe Skalierbarkeit erfordert, sollten Sie nur die minimalen Einstiegspunkt-RU/s bereitstellen, um die Kosten zu optimieren. In beiden Fällen eignet sich sowohl der standardmäßig (manuell) bereitgestellte und der automatisch skalierte Durchsatz. Folgendes sollten Sie bedenken:

Wenn Sie den standardmäßig (manuell) bereitgestellten Durchsatz mit einem Einstiegspunkt von 400 RU/s bereitstellen, können Sie nicht mehr als 400 RU/s nutzen, es sei denn, Sie ändern den festgelegten Durchsatz manuell. Ihnen werden pro Stunde 400 RU/s zum Tarif des standardmäßig (manuell) bereitgestellten Durchsatzes berechnet.

Wenn Sie den Durchsatz per Autoskalierung mit einem Einstiegspunkt von maximal 4.000 RU/s bereitstellen, skaliert die Ressource von 400 bis 4.000 RU/s. Da der automatisch skalierte Durchsatz pro RU/s mit dem 1,5-fachen Tarif des standardmäßig (manuell) bereitgestellten Durchsatzes abgerechnet wird, wird Ihre Rechnung für die Stunden, in denen das System auf den Minimalwert von 400 RU/s herunterskaliert wurde, höher als ausfallen, als wenn die 400 RU/s manuell bereitgestellt worden wären. Dafür können Sie mit der Autoskalierung jederzeit bis zu 4.000 RU/s nutzen, falls die Auslastung Ihrer Anwendung plötzlich steigt, ohne dass eine Benutzeraktion erforderlich ist. Sie sollten generell die Vorteile abwägen, die ein konstant verfügbarer Höchstwert von RU/s mit sich bringt, der zum 1,5-fachen Tarif abgerechnet wird.

Mit dem [Capacity Planer](estimate-ru-with-capacity-planner.md) von Azure Cosmos DB können Sie Ihre Durchsatzanforderungen schätzen lassen. 

### <a name="existing-applications"></a>Bestehende Anwendungen ###

Wenn Sie eine vorhandene Anwendung mit einem standardmäßig (manuell) bereitgestellten Durchsatz haben, können Sie mit [Azure Monitor-Metriken](../azure-monitor/insights/cosmosdb-insights-overview.md) bestimmen, ob sich Ihr Datenverkehrsmuster für die Autoskalierung eignet. 

Suchen Sie dazu zuerst für die Datenbank oder den Container nach der [normalisierten Verbrauchsmetrik für Anforderungseinheiten](monitor-normalized-request-units.md#view-the-normalized-request-unit-consumption-metric). Die normalisierte Auslastung ist ein Maß für die aktuelle Nutzung Ihres standardmäßig (manuell) bereitgestellten Durchsatzes. Je näher die Zahl bei 100 % liegt, desto näher sind Sie der vollständigen Nutzung der bereitgestellten RU/s. [Weitere Informationen zur Metrik](monitor-normalized-request-units.md#view-the-normalized-request-unit-consumption-metric)

Ermitteln Sie als Nächstes, wie sich die normalisierte Auslastung im Zeitverlauf ändert. Wenn Sie feststellen, dass Ihre normalisierte Auslastung variabel oder nicht vorhersehbar ist, sollten Sie in Betracht ziehen, die Autoskalierung für Ihre Datenbank oder Ihren Container zu aktivieren. Wenn Ihre Auslastung dagegen konstant und vorhersagbar ist, sollten Sie weiterhin besser den standardmäßig (manuell) bereitgestellten Durchsatz verwenden. 

> [!TIP]
> Beim standardmäßig (manuell) bereitgestellten Durchsatz können Sie mithilfe der normalisierten Auslastungsmetrik die tatsächlichen RU/s schätzen, die Sie verwenden würden, wenn der Durchsatz automatisch skaliert werden würde. Multiplizieren Sie dazu die normalisierte Auslastung zu einem bestimmten Zeitpunkt mit den aktuell bereitgestellten Standard-RU/s (manuell bereitgestellt). Wenn Sie beispielsweise 5.000 RU/s bereitgestellt haben und die normalisierte Auslastung bei 90 % liegt, dann beträgt die Nutzung der RU/s 0,9 x 5.000 = 4.500 RU/s. Wenn Sie feststellen, dass Ihr Datenverkehrsmuster variabel ist, Sie aber zu viele oder zu wenige RU/s bereitgestellt haben, sollten Sie die Autoskalierung aktivieren und die Einstellung für die maximale Anzahl von RU/s entsprechend ändern.

## <a name="measure-and-monitor-your-usage"></a>Messen und Überwachen der Auslastung
Nachdem Sie einen Durchsatztyp festgelegt haben, sollten Sie Ihre Anwendung im Zeitverlauf überwachen und bei Bedarf Anpassungen vornehmen. 

Wenn Sie die Autoskalierung verwenden, können Sie sich mit Azure Monitor die bereitgestellte maximale Anzahl von RU/s (**Autoscale Max Throughput** [Maximaler Durchsatz bei Autoskalierung]) sowie die RU/s anzeigen lassen, auf die das System zurzeit skaliert ist (**Provisioned Throughput** [Bereitgestellter Durchsatz]). Nachstehend finden Sie ein Beispiel für eine variable oder nicht vorhersehbare Workload mit automatisch skaliertem Durchsatz. Beachten Sie, dass das System die RU/s auf den Mindestwert von 10 % der maximalen RU/s skaliert (in diesem Fall 5.000 RU/s bzw. 50.000 RU/s), wenn kein Datenverkehr vorhanden ist. 

![Beispiel für eine Workload mit automatisch skaliertem Durchsatz](media/how-to-choose-offer/autoscale-metrics-azure-monitor.png)

> [!NOTE]
> Wenn Sie den standardmäßig (manuell) bereitgestellten Durchsatz verwenden, bezieht sich die Metrik **Provisioned Throughput** (Bereitgestellte Durchsatz) auf den Wert, den Sie als Benutzer festgelegt haben. Bei Verwendung des automatisch skalierten Durchsatzes bezieht sich diese Metrik auf die Anzahl von RU/s, auf die das System zurzeit skaliert ist.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie den [RU-Planer](https://cosmos.azure.com/capacitycalculator/), um den Durchsatz für Ihre Workloads zu schätzen.
* Mit [Azure Monitor](monitor-cosmos-db.md#view-operation-level-metrics-for-azure-cosmos-db) können Sie Ihre vorhandenen Workloads überwachen.
* Informieren Sie sich darüber, wie Sie [automatisch skalierten Durchsatz für eine Datenbank oder einen Container in Azure Cosmos DB bereitstellen](how-to-provision-autoscale-throughput.md).
* Erfahren Sie mehr in den [häufig gestellten Fragen zur Autoskalierung](autoscale-faq.md).