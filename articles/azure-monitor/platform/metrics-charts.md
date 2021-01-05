---
title: Erweiterte Funktionen von Azure Metrik-Explorer
description: Erfahren Sie mehr über erweiterte Funktionen von Azure Monitor-Metrik-Explorer.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: be3d3f11e90c17bd8c4792418500da651039e480
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97562802"
---
# <a name="advanced-features-of-azure-metrics-explorer"></a>Erweiterte Funktionen von Azure Metrik-Explorer

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie mit grundlegenden Funktionen von Metrik-Explorer vertraut sind. Wenn Sie ein neuer Benutzer sind und erfahren möchten, wie Sie Ihr erstes Metrikdiagramm erstellen können, lesen Sie [Erste Schritte mit Azure Metrik-Explorer](metrics-getting-started.md).

## <a name="metrics-in-azure"></a>Metriken in Azure

[Metriken in Azure Monitor](data-platform-metrics.md) sind eine Reihe von Messwerten und Zahlen, die im Lauf der Zeit gesammelt und gespeichert werden. Es gibt Standardmetriken (bzw. sogenannte „Plattformmetriken“) und benutzerdefinierte Metriken. Die Standardmetriken werden Ihnen von der Azure-Plattform selbst bereitgestellt. Standardmetriken stellen die Integritäts- und Nutzungsstatistik Ihrer Azure-Ressourcen dar. Benutzerdefinierte Metriken werden hingegen von Ihren Anwendungen mithilfe der [Application Insights-API für benutzerdefinierte Ereignisse und Metriken](../app/api-custom-events-metrics.md), der [Azure-Diagnoseerweiterung (Windows Azure Diagnostics, WAD)](./diagnostics-extension-overview.md) oder durch die [Azure Monitor-REST-API](./metrics-store-custom-rest-api.md) an Azure gesendet.

## <a name="create-views-with-multiple-metrics-and-charts"></a>Erstellen von Ansichten mit mehreren Metriken und Diagrammen

Sie können Diagramme erstellen, in denen mehrere Metrikzeilen grafisch dargestellt oder mehrere Metrikzeilen gleichzeitig angezeigt werden. Diese Funktionalität bietet Ihnen folgende Möglichkeiten:

- Korrelieren verwandter Metriken im gleichen Diagramm, um zu sehen, in welcher Beziehung ein Wert zu einem anderen steht
- Anzeigen von Metriken mit unterschiedlichen Maßeinheiten in unmittelbarer Nähe zueinander
- Visuelles Aggregieren und Vergleichen von Metriken aus mehreren Ressourcen

Wenn Sie beispielsweise über fünf Speicherkonten verfügen und wissen möchten, wie viel Gesamtspeicherplatz diese belegen, können Sie ein (gestapeltes) Flächendiagramm erstellen, das die einzelnen und die Summe aller Werte zu bestimmten Zeitpunkten anzeigt.

### <a name="multiple-metrics-on-the-same-chart"></a>Mehrere Metriken in demselben Diagramm

Zuerst [erstellen Sie ein neues Diagramm](metrics-getting-started.md#create-your-first-metric-chart). Klicken Sie auf **Metrik hinzufügen**, und wiederholen Sie die Schritte, um eine weitere Metrik im gleichen Diagramm hinzuzufügen.

   > [!NOTE]
   > In der Regel wird davon abgeraten, in einem Diagramm Metriken mit unterschiedlichen Maßeinheiten (d.h. „Millisekunden“ und „Kilobytes“) oder deutlich abweichender Skala zu verwenden. In diesem Fall sollten Sie vielmehr verschiedene Diagramme verwenden. Klicken Sie auf die Schaltfläche „Diagramm hinzufügen“, um mehrere Diagramme im Metrik-Explorer zu erstellen.

### <a name="multiple-charts"></a>Mehrere Diagramme

Klicken Sie auf **Diagramm hinzufügen**, und erstellen Sie ein weiteres Diagramm mit einer anderen Metrik.

### <a name="order-or-delete-multiple-charts"></a>Sortieren oder Löschen mehrerer Diagramme

Klicken Sie zum Sortieren oder Löschen mehrerer Diagramme auf die Auslassungszeichen ( **...** ), um das Diagrammmenü zu öffnen, und wählen Sie das entsprechende Menüelement (**Nach oben**, **Nach unten** oder **Löschen**) aus.

## <a name="changing-aggregation"></a>Ändern der Aggregation

Wenn Sie einem Diagramm eine Metrik hinzufügen, wählt der Metrik-Explorer automatisch deren Standardaggregation aus. In den grundlegenden Szenarien ist die Standardeinstellung sinnvoll, doch Sie können eine andere Aggregation verwenden, um zusätzliche Einblicke in die Metrik zu erhalten. Wenn Sie verschiedene Aggregationen in einem Diagramm anzeigen möchten, müssen Sie verstehen, wie diese vom Metrik-Explorer verarbeitet werden. 

Bei Metriken handelt es sich um die Reihe von Messwerten (oder „Metrikwerte“), die über den Zeitraum erfasst werden. Wenn Sie ein Diagramm zeichnen, werden die Werte der ausgewählten Metrik separat über das *Aggregationsintervall* aggregiert. Sie wählen die Dauer des Aggregationsintervalls [mithilfe des Zeitauswahlbereichs im Metrik-Explorer](metrics-getting-started.md#select-a-time-range) aus. Wenn Sie das Aggregationsintervall nicht explizit auswählen, wird die Zeitgranularität automatisch basierend auf dem aktuell ausgewählten Zeitraum ausgewählt. Sobald das Aggregationsintervall festgelegt ist, werden die Metrikwerte, die während jedes Aggregationsintervalls erfasst wurden, aggregiert und in das Diagramm eingefügt (jeweils ein Datenpunkt pro Aggregationsintervall).

Angenommen, das Diagramm zeigt die Metrik **Serverantwortzeit** mit dem Aggregationstyp **Average** (Durchschnitt) über den Zeitraum **letzte 24 Stunden**:

- Wenn die Zeitgranularität auf 30 Minuten festgelegt ist, wird das Diagramm aus 48 aggregierten Datenpunkten gezeichnet (z. B. werden beim Liniendiagramm 48 Punkte auf der Diagrammzeichnungsfläche verbunden). Das heißt, 24 Stunden x 2 Datenpunkte pro Stunde. Jeder Datenpunkt stellt den *Durchschnitt* aller erfassten Antwortzeiten für Serveranforderungen dar, die während des jeweils relevanten 30-Minuten-Zeitraums aufgetreten sind.
- Wenn Sie die Zeitgranularität auf 15 Minuten festlegen, erhalten Sie 96 aggregierte Datenpunkte.  Das heißt, 24 Stunden x 4 Datenpunkte pro Stunde.

Im Metrik-Explorer sind fünf Aggregationstypen für grundlegende Statistiken verfügbar: **Sum**, **Count**, **Min**, **Max** und **Average**. Der Aggregationstyp **Sum** wird manchmal auch als **Total** bezeichnet. Bei vielen Metriken blendet Metrik-Explorer die Aggregationen aus, die vollkommen irrelevant sind und nicht verwendet werden können.

- **Sum**: Die Summe aller Werte, die über das Aggregationsintervall erfasst wurden.
- **Count**: Die Anzahl der Messwerte, die über das Aggregationsintervall erfasst wurden. **Count** entspricht in dem Fall, dass die Metrik immer mit dem Wert 1 erfasst wird, dem Aggregationstyp **Sum**. Dies tritt üblicherweise auf, wenn die Metrik die Anzahl unterschiedlicher Ereignisse nachverfolgt und jeder Messwert ein Ereignis darstellt (d. h., der Code löst bei jedem Eintreffen einer neuen Anforderung einen Metrikdatensatz aus).
- **Average**: Der Durchschnitt der Metrikwerte, die über das Aggregationsintervall erfasst wurden.
- **Min**: Der niedrigste Wert, der über das Aggregationsintervall erfasst wurde.
- **Max**: Der höchste Wert, der über das Aggregationsintervall erfasst wurde.

## <a name="apply-filters-to-charts"></a>Anwenden von Filtern auf Diagramme

Sie können Filter auf Diagramme anwenden, die Metriken mit Dimensionen aufweisen. Nehmen wir beispielsweise an, die Metrik „Anzahl von Transaktionen“ weist eine Dimension mit dem Namen „Antworttyp“ auf, die angibt, ob die Antwort von Transaktionen erfolgreich war oder nicht. Durch Filtern nach dieser Dimension wird dann eine Diagrammzeile gezeichnet, die nur erfolgreiche (oder nur fehlerhafte) Transaktionen anzeigt. 

### <a name="to-add-a-filter"></a>So fügen Sie einen Filter hinzu

1. Wählen Sie oberhalb des Diagramms **Filter hinzufügen** aus.

2. Wählen Sie aus, nach welcher Dimension (Eigenschaft) gefiltert werden soll.

   ![Screenshot mit den Dimensionen (Eigenschaften), die gefiltert werden können.](./media/metrics-charts/028.png)

3. Wählen Sie beim Zeichnen des Diagramms aus, welche Dimensionswerte eingeschlossen werden sollen (in diesem Beispiel werden die Filterergebnisse erfolgreicher Speichertransaktionen gezeigt):

   ![Screenshot, der das Herausfiltern der erfolgreichen Speichertransaktionen zeigt.](./media/metrics-charts/029.png)

4. Klicken Sie nach der Auswahl der Filterwerte außerhalb des Filterselektors, um ihn zu schließen. Nun zeigt das Diagramm an, wie viele Speichertransaktionen fehlerhaft sind:

   ![Screenshot, der anzeigt, wie viele Speichertransaktionen fehlerhaft sind.](./media/metrics-charts/030.png)

5. Sie können die Schritte 1 bis 4 wiederholen, um mehrere Filter auf dieselben Diagramme anzuwenden.



## <a name="apply-splitting-to-a-chart"></a>Anwenden des Teilens auf ein Diagramm

Sie können eine Metrik nach Dimension unterteilen, um die verschiedenen Segmente der Metrik gegenüberzustellen und die äußeren Segmente einer Dimension zu identifizieren.

### <a name="apply-splitting"></a>Anwenden des Teilens

1. Klicken Sie oberhalb des Diagramms auf **Teilung anwenden**.
 
   > [!NOTE]
   > Das Teilen kann nicht bei Diagrammen verwendet werden, die mehrere Metriken aufweisen. Auch können Sie zwar mehrere Filter anwenden, jedoch kann nur eine Teilungsdimension auf ein beliebiges einzelnes Diagramm angewendet werden.

2. Wählen Sie eine Dimension, in der Ihr Diagramm segmentiert werden soll:

   ![Screenshot, der die ausgewählte Dimension anzeigt, in der Sie Ihr Diagramm segmentieren möchten.](./media/metrics-charts/031.png)

   Das Diagramm zeigt nun mehrere Zeilen an, eine Zeile für jedes Segment der Dimension:

   ![Screenshot, der mehrere Zeilen anzeigt: eine Zeile für jedes Segment der Dimension.](./media/metrics-charts/032.png)

3. Klicken Sie außerhalb des **Gruppierungsselektors**, um ihn zu schließen.

   > [!NOTE]
   > Blenden Sie in einer Dimension durch Filtern und Teilen Segmente aus, die für Ihr Szenario nicht relevant sind, und stellen Sie eine bessere Übersichtlichkeit der Diagramme sicher.

## <a name="lock-boundaries-of-chart-y-axis"></a>Festlegen der Grenzen der Y-Achse des Diagramms

Wenn das Diagramm geringfügige Schwankungen größerer Werte enthält, ist es wichtig, den Bereich der Y-Achse festzulegen. 

Ein Beispiel: Wenn die Menge an erfolgreichen Anforderungen von 99,99 Prozent auf 99,5 Prozent fällt, bedeutet das unter Umständen eine deutliche Verschlechterung der Servicequalität. Bei Verwendung der Standardeinstellungen des Diagramms ist eine solche geringfügige Schwankung jedoch nur sehr schwer oder sogar überhaupt nicht erkennbar. In diesem Fall können Sie die Untergrenze des Diagramms auf 99 Prozent festlegen, wodurch ein geringfügiger Rückgang leichter zu erkennen ist. 

Ein weiteres Beispiel wären Schwankungen beim verfügbaren Arbeitsspeicher, bei denen der Wert aus technischen Gründen niemals „0“ erreicht. Wenn Sie hier den Bereich auf einen höheren Wert festlegen, ist eine geringere Arbeitsspeicherverfügbarkeit leichter zu erkennen. 

Verwenden Sie zum Steuern des Bereichs der Y-Achse das Diagrammmenü „...“, und wählen Sie **Diagrammeinstellungen** aus, um auf die erweiterten Diagrammeinstellungen zuzugreifen.

![Screenshot, auf dem die Option „Diagrammeinstellungen“ hervorgehoben ist.](./media/metrics-charts/033.png)

 Ändern Sie die Werte im Abschnitt für den Bereich der Y-Achse, oder verwenden Sie die Schaltfläche **Automatisch**, um die Standardwerte wiederherzustellen.
 
 ![Screenshot, auf dem der Abschnitt für den Bereich der Y-Achse hervorgehoben ist.](./media/metrics-charts/034.png)

> [!WARNING]
> Wenn Sie die Grenzen der Y-Achse für Diagramme festlegen möchten, die verschiedene Anzahlwerte oder Summen für einen Zeitraum nachverfolgen und somit Anzahl-, Summen-, Minimum- oder Maximum-Aggregationen verwenden, muss in der Regel anstelle der automatischen Standardwerte auch eine feste Zeitgranularität angegeben werden. Dies ist erforderlich, da sich die Werte in Diagrammen ändern, wenn der Benutzer die Größe des Browserfensters oder die Bildschirmauflösung ändert, was eine automatische Änderung der Zeitgranularität zur Folge hat. Die resultierende Änderung der Zeitgranularität wirkt sich auf die Darstellung des Diagramms aus und macht die aktuelle Auswahl des Y-Achsenbereichs ungültig.

## <a name="change-colors-of-chart-lines"></a>Ändern der Farben von Diagrammlinien

Nachdem Sie die Diagramme konfiguriert haben, werden den Diagrammlinien automatisch Farben aus einer Standardpalette zugewiesen. Sie können diese Farben ändern.

Um die Farbe einer Diagrammlinie zu ändern, klicken Sie auf den farbigen Balken in der zum Diagramm gehörenden Legende. Das Dialogfeld für die Farbauswahl wird geöffnet. Verwenden Sie die Farbauswahl, um die Farbe für die Linie zu konfigurieren.

![Screenshot, der das Ändern der Farbe zeigt.](./media/metrics-charts/035.png)

Nachdem Sie die Diagrammfarben konfiguriert haben, bleiben sie in dieser Form erhalten, wenn Sie das Diagramm an ein Dashboard anheften. Im folgenden Abschnitt wird erläutert, wie Sie ein Diagramm anheften.

## <a name="pin-charts-to-dashboards"></a>Anheften von Diagrammen an Dashboards

Nach der Konfiguration der Diagramme sollten Sie sie zu Dashboards hinzufügen, um sie – beispielsweise im Kontext einer anderen Überwachungstelemetrie – erneut anzeigen zu können oder Ihrem Team zur Verfügung zu stellen.

So heften Sie ein konfiguriertes Diagramm an ein Dashboard an

Klicken Sie nach der Konfiguration Ihres Diagramms rechts oben im Diagramm auf **An Dashboard anheften**.

![Screenshot, der das Anheften an das Diagramm zeigt.](./media/metrics-charts/036.png)

## <a name="create-alert-rules"></a>Erstellen von Warnungsregeln

Sie können die Kriterien, die Sie zum Visualisieren Ihrer Metriken festgelegt haben, als Grundlage für eine Metrik basierend auf einer Warnungsregel verwenden. Die neue Warnungsregel enthält Ihre Dimensionen für Zielressource, Metrik, Teilung und Filter aus dem Diagramm. Sie können diese Einstellungen später im Erstellungsbereich für Warnungsregeln ändern.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Zum Erstellen einer neuen Warnungsregel klicken Sie auf **Neue Warnungsregel**.

![Schaltfläche „Neue Warnungsregel“ rot hervorgehoben](./media/metrics-charts/042.png)

Sie wechseln in den Bereich zum Erstellen von Warnungsregeln, in dem die zugrunde liegenden Metrikdimensionen aus dem Diagramm vorab ausgefüllt wurden, um das Generieren benutzerdefinierter Warnungsregeln zu vereinfachen.

![Erstellen einer Warnungsregel](./media/metrics-charts/041.png)

Lesen Sie [diesen Artikel](alerts-metric.md), um mehr über das Einrichten von Metrikwarnungen zu erfahren.

## <a name="troubleshooting"></a>Problembehandlung

*In meinem Diagramm werden keine Daten angezeigt.*

* Filter gelten für alle Diagramme im Bereich. Stellen Sie sicher, dass Sie bei der Arbeit mit einem Diagramm keinen Filter festgelegt haben, mit dem alle Daten eines anderen Diagramms ausgeschlossen werden.

* Wenn Sie verschiedene Filter für verschiedene Diagramme festlegen möchten, sollten Sie diese in unterschiedlichen Blättern erstellen und als separate Favoriten speichern. Sie können sie auch im Dashboard anheften, damit sie nebeneinander angezeigt werden.

* Wenn Sie ein Diagramm nach einer Eigenschaft segmentieren, die nicht in der Metrik definiert ist, sind im Diagramm keine Daten enthalten. Versuchen Sie, die Segmentierung (Aufteilung) zu löschen, oder wählen Sie eine andere Eigenschaft aus.

## <a name="next-steps"></a>Nächste Schritte

  Lesen Sie [Erstellen von benutzerdefinierten KPI-Dashboards](../learn/tutorial-app-dashboards.md), um mehr über die Best Practices zum Erstellen von Dashboards mit ausführbaren Aktionen und Metriken zu erfahren.
