---
title: Erreichen von Georedundanz für Azure Stream Analytics-Aufträge
description: In diesem Artikel wird beschrieben, wie Sie bei Azure Stream Analytics-Aufträgen Georedundanz anstelle von geografischem Failover erreichen können.
author: an-emma
ms.author: raan
ms.service: stream-analytics
ms.topic: how-to
ms.date: 08/26/2020
ms.openlocfilehash: 86a90cbcbb3381e853dcea766364642a89dafd7b
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98015520"
---
# <a name="achieve-geo-redundancy-for-azure-stream-analytics-jobs"></a>Erreichen von Georedundanz für Azure Stream Analytics-Aufträge

Azure Stream Analytics bietet kein automatisches geografisches Failover, Sie können jedoch Georedundanz erzielen, indem Sie in mehreren Azure-Regionen identische Stream Analytics-Aufträge bereitstellen. Jeder Auftrag stellt eine Verbindung mit lokalen Ein- und Ausgabequellen her. Die Anwendung muss sowohl Eingabedaten an die beiden regionalen Eingänge senden als auch eine Abstimmung zwischen den beiden regionalen Ausgängen durchführen. Die Stream Analytics-Aufträge sind zwei separate Entitäten.

Das folgende Diagramm zeigt ein Beispiel für eine georedundante Bereitstellung von Stream Analytics-Aufträgen mit Event Hub-Eingabe und Azure-Datenbank-Ausgabe.

:::image type="content" source="media/geo-redundancy/geo-redundant-jobs.png" alt-text="Diagramm von georedundanten Stream Analytics-Aufträgen":::

## <a name="primarysecondary-strategy"></a>Strategie mit primärer/sekundärer Region

Die Anwendung muss verwalten, welche Ausgabedatenbank einer Region als primäre Datenbank und welche als sekundäre Datenbank betrachtet wird. Bei einem Ausfall der primären Region wechselt die Anwendung zur sekundären Datenbank und beginnt mit dem Lesen von Updates aus dieser Datenbank. Der eigentliche Mechanismus zum Minimieren doppelter Lesevorgänge ist von Ihrer Anwendung abhängig. Sie können diesen Prozess vereinfachen, indem Sie zusätzliche Informationen in die Ausgabe schreiben. Beispielsweise können Sie jeder Ausgabe einen Zeitstempel oder eine Sequenz-ID hinzufügen, damit doppelte Zeilen auf triviale Weise übersprungen werden können. Sobald die primäre Region wieder hergestellt ist, holt sie über ähnliche Mechanismen den Status der sekundären Datenbank auf.

Auch wenn verschiedene Ein- und Ausgabetypen unterschiedliche Georeplikationsoptionen zulassen, wird das in diesem Artikel beschriebene Muster empfohlen, um Georedundanz zu erreichen, da es Flexibilität und Kontrolle sowohl für Ereignisproduzenten als auch für Ereignisconsumer bietet.

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Verwalten von Azure Stream Analytics-Aufträgen mithilfe von PowerShell](stream-analytics-monitor-and-manage-jobs-use-powershell.md)
* [Datengesteuertes Debuggen in Azure Stream Analytics](stream-analytics-job-diagram-with-metrics.md)