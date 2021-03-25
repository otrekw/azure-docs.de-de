---
title: Beispiel für Azure Monitor-Metrikdiagramme
description: Erfahren Sie mehr über die Visualisierung Ihrer Azure Monitor-Daten.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.openlocfilehash: b7e848e4565e2b1badd9bc418d28ea984a0306b2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051105"
---
# <a name="metric-chart-examples"></a>Beispiele für Metrikdiagramme 

Die Azure-Plattform stellt [mehr als tausend Metriken](./metrics-supported.md) bereit, von denen viele Dimensionen umfassen. Durch die Verwendung von [Dimensionsfiltern](./metrics-charts.md), das Anwenden einer [Aufteilung](./metrics-charts.md), die Steuerung des Diagrammtyps und eine Anpassung der Diagrammeinstellungen können Sie leistungsstarke Diagnoseansichten und Dashboards erstellen, die Einblick in den Zustand Ihrer Infrastruktur und Anwendungen bieten. Dieser Artikel zeigt einige Beispiele für die Diagramme, die Sie mit dem [Metrik-Explorer](./metrics-charts.md) erstellen können, und erklärt die notwendigen Schritte zur Konfiguration jedes dieser Diagramme.

Möchten Sie Ihre Diagrammbeispiele für andere Benutzer zur Verfügung stellen? Tragen Sie zu dieser Seite auf GitHub bei, und teilen Sie hier Ihre eigenen Diagrammbeispiele!

## <a name="website-cpu-utilization-by-server-instances"></a>CPU-Auslastung einer Website nach Serverinstanzen

Dieses Diagramm zeigt, ob die CPU-Auslastung für eine App Service-Ressource innerhalb des akzeptablen Bereichs liegt. Durch eine Aufschlüsselung nach Serverinstanz kann bestimmt werden, ob die Last in geeigneter Weise verteilt wurde. Aus dem Diagramm wird ersichtlich, dass die App vor 6 Uhr morgens auf einer einzelnen Serverinstanz ausgeführt und dann durch Hinzufügen einer weiteren Instanz zentral hochskaliert wurde.

![Liniendiagramm der durchschnittlichen CPU-Nutzung in Prozent nach Serverinstanz](./media/metrics-charts/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Wie wird dieses Diagramm konfiguriert?

Wählen Sie Ihre App Service-Ressource aus, und suchen Sie nach der Metrik **CPU-Prozentsatz**. Klicken Sie dann auf **Aufteilung anwenden**, und wählen Sie die Dimension **Instanz** aus.

## <a name="application-availability-by-region"></a>Anwendungsverfügbarkeit nach Region

Zeigen Sie die Verfügbarkeit Ihrer Anwendung nach Regionen an, um festzustellen, an welchen geografischen Standorten Probleme vorliegen. Dieses Diagramm zeigt die Application Insights-Metrik zur Verfügbarkeit. Sie können sehen, dass für die überwachte Anwendung im Rechenzentrum „USA, Osten“ keine Probleme vorliegen, dass es aber in den Regionen „USA, Westen“ und „Asien, Osten“ teilweise zu Verfügbarkeitsproblemen kommt.

![Diagramm der durchschnittlichen Verfügbarkeit nach Standort](./media/metrics-charts/availability-by-location.png)

### <a name="how-to-configure-this-chart"></a>Wie wird dieses Diagramm konfiguriert?

Sie müssen zunächst die [Application Insights-Verfügbarkeitsüberwachung](../app/monitor-web-app-availability.md) für Ihre Website aktivieren. Anschließend wählen Sie die Application Insights-Ressource aus, und geben die Metrik zur Verfügbarkeit an. Wenden Sie auf die Dimension **Ausführungsort** eine Aufteilung an.

## <a name="volume-of-failed-storage-account-transactions-by-api-name"></a>Anzahl fehlerhafter Speicherkontotransaktionen nach API-Name

Für Ihre Speicherkontoressource wird eine übermäßige Anzahl fehlerhafter Transaktionen verzeichnet. Anhand der Transaktionsmetrik können Sie feststellen, welche API für die übermäßige Fehler verantwortlich ist. Beachten Sie, dass das folgende Diagramm beim Aufteilen mit der gleichen Dimension (API-Name) konfiguriert wurde und nach dem fehlerhaften Antworttyp gefiltert wird:

![Balkendiagramm der API-Transaktionen](./media/metrics-charts/split-and-filter-example.png)

### <a name="how-to-configure-this-chart"></a>Wie wird dieses Diagramm konfiguriert?

Bei der Metrikauswahl geben Sie Ihr Speicherkonto und die Metrik **Transaktionen** an. Ändern Sie den Diagrammtyp in **Balkendiagramm**. Klicken Sie auf **Aufteilung anwenden**, und wählen Sie die Dimension **API-Name** aus. Klicken Sie anschließend auf **Filter hinzufügen**, und wählen Sie erneut die Dimension **API-Name** aus. Wählen Sie im Filterdialogfeld die APIs aus, die im Diagramm angezeigt werden sollen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Azure Monitor-[Arbeitsmappen](../visualize/workbooks-overview.md)
* Weitere Informationen zum [Metrik-Explorer](metrics-charts.md)