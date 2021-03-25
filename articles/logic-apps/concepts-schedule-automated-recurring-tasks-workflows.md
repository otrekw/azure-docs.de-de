---
title: Planen von wiederkehrenden Aufgaben und Workflows in Azure Logic Apps
description: Übersicht über die Planung von wiederkehrenden automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm, azla
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: e9fbafa9f3c33d10496e84f61e1f2b97f6328d3b
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100581812"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Planen und Ausführen von wiederkehrenden automatisierten Aufgaben, Prozessen und Workflows mit Azure Logic Apps

Logic Apps unterstützt Sie beim Erstellen und Ausführen von automatisierten wiederkehrenden Aufgaben und Prozessen nach einem bestimmten Zeitplan. Mit einem Logik-App-Workflow, der mit einem integrierten Trigger vom Typ „Serie“ oder „Gleitendes Fenster“ (beides Zeitplantrigger) beginnt, können Sie Aufgaben sofort, zu einem späteren Zeitpunkt oder in sich wiederholenden Intervallen ausführen. Sie können Dienste innerhalb und außerhalb von Azure aufrufen (z.B. über HTTP- oder HTTPS-Endpunkte), Nachrichten in Azure-Diensten wie Azure Storage und Azure Service Bus posten oder Dateien in eine Dateifreigabe hochladen. Mit dem Serientrigger können Sie auch komplexe Zeitpläne und Wiederholungseinstellungen für die Ausführung von Aufgaben einrichten. Weitere Informationen zu den integrierten Zeitplantriggern und -aktionen finden Sie unter [Zeitplantrigger](#schedule-triggers) bzw. unter [Zeitplanaktionen](#schedule-actions). 

> [!TIP]
> Sie können wiederkehrende Workloads planen und ausführen, ohne eine separate Logik-App für jeden geplanten Auftrag zu erstellen. Dadurch umgehen Sie auch das [Limit für Workflows pro Region und Abonnement](../logic-apps/logic-apps-limits-and-config.md#definition-limits). Stattdessen können Sie das Logik-App-Muster verwenden, das durch die [Azure-Schnellstartvorlage „Logic App job scheduler“ (Logik-App-Auftragsplaner)](https://github.com/Azure/azure-quickstart-templates/tree/master/301-logicapps-jobscheduler/) erstellt wird.
>
> Die Vorlage „Logic App job scheduler“ (Logik-App-Auftragsplaner) erstellt die Logik-App „CreateTimerJob“, die wiederum die Logik-App „TimerJob“ aufruft. Anschließend können Sie die Logik-App „CreateTimerJob“ als API aufrufen, indem Sie eine HTTP-Anforderung ausführen und dabei einen Zeitplan als Eingabe übergeben. Bei jedem Aufruf der Logik-App „CreateTimerJob“ wird auch die Logik-App „TimerJob“ aufgerufen. Dadurch wird eine neue TimerJob-Instanz erstellt, die kontinuierlich auf der Grundlage des angegebenen Zeitplans oder bis zum Erreichen eines bestimmten Limits ausgeführt wird. Auf diese Weise können Sie beliebig viele TimerJob-Instanzen ausführen, ohne sich Gedanken über die Workflowlimits machen zu müssen, da Instanzen keine einzelnen Logik-App-Workflowdefinitionen oder -ressourcen sind.

Die folgende Liste enthält einige Beispielaufgaben, die Sie mit den integrierten Zeitplantriggern ausführen können:

* Abrufen von internen Daten, z.B. durch tägliches Ausführen einer gespeicherten SQL-Prozedur.

* Abrufen von externen Daten, indem alle 15 Minuten ein Pull für die Wetterprognose eines Wetterdiensts ausgeführt wird.

* Senden von Berichtsdaten, z.B. eine E-Mail mit einer Zusammenfassung aller Bestellungen der letzten Woche, die einen bestimmten Betrag übersteigen.

* Verarbeiten von Daten, z.B. Komprimieren der hochgeladenen Bilder des Tages an jedem Wochentag außerhalb der Spitzenzeiten.

* Bereinigen von Daten, z.B. Löschen aller Tweets, die älter als drei Monate sind.

* Archivieren von Daten, z.B. Senden von Rechnungen per Push an einen Sicherungsdienst um 1:00 Uhr jeden Morgen für die nächsten neun Monate.

Sie können auch die integrierten Zeitplanaktionen nutzen, um den Workflow vor dem Ausführen der nächsten Aktion anzuhalten. Beispiele:

* Versand einer E-Mail mit einem Statusupdate bis zum Erreichen eines Werktags verzögern.

* Workflow verzögern, bis ein HTTP-Aufruf abgeschlossen werden konnte, und erst dann den Vorgang fortsetzen und das Ergebnis abrufen.

Dieser Artikel beschreibt die Funktionen für die integrierten Zeitplantrigger und -aktionen.

<a name="schedule-triggers"></a>

## <a name="schedule-triggers"></a>Zeitplantrigger

Sie können Ihren Logik-App-Workflow mit dem Trigger „Serie“ oder „Gleitendes Fenster“ starten, was mit keinem bestimmten Dienst oder System verknüpft ist. Diese Trigger starten Ihren Workflow und führen ihn gemäß den von Ihnen angegebenen Wiederholungseinstellungen aus. Sie können das Intervall und die Häufigkeit auswählen, z. B. die Anzahl von Sekunden, Minuten, Stunden, Tagen oder Monaten. Sie können auch Startdatum und -uhrzeit sowie die Zeitzone festlegen. Bei jeder Triggerauslösung erstellt Logic Apps eine neue Workflowinstanz für Ihre Logik-App und führt diese aus.

Im Folgenden finden Sie die Unterschiede zwischen den beiden Triggern:

* **Serie**: Führt Ihren Workflow in regelmäßigen Intervallen aus, basierend auf dem von Ihnen angegebenen Zeitplan. Wenn der Trigger Wiederholungen auslässt, beispielsweise wegen Unterbrechungen oder deaktivierten Workflows, verarbeitet der Serientrigger diese fehlenden Wiederholungen nicht, sondern startet die Wiederholungen beim nächsten geplanten Intervall neu.

  Wenn Sie **Tag** als Häufigkeit auswählen, können Sie die genaue Tageszeit in Stunden und Minuten angeben, z. B. jeden Tag um 14:30 Uhr. Wenn Sie **Woche** als Häufigkeit auswählen, können Sie auch bestimmte Wochentage angeben, z.B. Mittwoch und Samstag. Sie können auch ein Startdatum und eine Uhrzeit zusammen mit einer Zeitzone für den Serienzeitplan angeben.

  > [!TIP]
  > Wenn eine Serie kein bestimmtes [Startdatum und keine Startuhrzeit](#start-time) angibt, wird die erste Wiederholung trotz der Serieneinrichtung Ihres Triggers sofort ausgeführt, wenn Sie die Logik-App speichern oder bereitstellen. Um dieses Verhalten zu vermeiden, geben Sie ein Startdatum und eine Startuhrzeit für die Ausführung der ersten Wiederholung an.
  >
  > Wenn eine Serie keine anderen erweiterten Zeitplanungsoptionen angibt wie spezifische Zeiten zum Ausführen zukünftiger Wiederholungen, basieren diese Wiederholungen auf der letzten Ausführungszeit. Hieraus ergibt sich, dass die Startzeiten für diese Wiederholungen sich aufgrund von Faktoren wie Wartezeiten während Speicheraufrufen verschieben können. Um sicherzustellen, dass Ihre Logik-App keine Wiederholung verpasst, insbesondere wenn die Häufigkeit im Bereich von Tagen oder länger liegt, probieren Sie diese Optionen aus:
  >
  > * Geben Sie ein Startdatum und eine Uhrzeit für die Wiederholung an, und geben Sie die spezifischen Zeiten an, zu denen nachfolgende Wiederholungen ausgeführt werden sollen, indem Sie dazu die Eigenschaften namens **Zu diesen Stunden** und **Zu diesen Minuten**, die nur für die Häufigkeiten **Tag** und **Woche** verfügbar sind.
  >
  > * Verwenden Sie den Trigger [Gleitendes Fenster](../connectors/connectors-native-sliding-window.md) anstelle des Triggers „Wiederholung“.

  Weitere Informationen finden Sie unter [Erstellen, Planen und Ausführen von wiederkehrenden Aufgaben und Workflows mit dem Serientrigger](../connectors/connectors-native-recurrence.md).

* **Gleitendes Fenster**: Führt Ihren Workflow in regelmäßigen Intervallen aus, wobei Daten in kontinuierlichen Blöcken verarbeitet werden. Wenn der Trigger Wiederholungen verpasst, beispielsweise wegen Unterbrechungen oder deaktivierten Workflows, kehrt der Trigger „Gleitendes Fenster“ zurück und verarbeitet die fehlenden Wiederholungen.

  Sie können ein Startdatum und eine Startuhrzeit, eine Zeitzone und einen Zeitraum angeben, um den jede Wiederholung in Ihrem Workflow verzögert werden soll. Diese Trigger unterstützt keine erweiterten Zeitpläne, z. B. genaue Uhrzeiten in Stunden und Minuten oder bestimmte Tage der Woche. Weitere Informationen finden Sie unter [Erstellen, Planen und Ausführen von wiederkehrenden Aufgaben und Workflows mit dem Trigger „Gleitendes Fenster“](../connectors/connectors-native-sliding-window.md).

<a name="schedule-actions"></a>

## <a name="schedule-actions"></a>Zeitplanaktionen

Nach jeder Aktion in Ihrem Logik-App-Workflow können Sie die Aktionen „Verzögern“ und „Verzögern bis“ verwenden, damit Ihr Workflow vor der Ausführung der nächsten Aktionen eine bestimmte Zeit wartet.

* **Verzögern**: Vor der Ausführung der nächsten Aktion wird eine bestimmte Anzahl von Zeiteinheiten lang gewartet, z.B. Sekunden, Minuten, Stunden, Tage, Wochen oder Monate. Weitere Informationen finden Sie unter [Verzögern der nächsten Aktion in Workflows](../connectors/connectors-native-delay.md).

* **Verzögern bis**: Mit der Ausführung der nächsten Aktion wird bis zum angegebenen Datum und zur angegebenen Uhrzeit gewartet. Weitere Informationen finden Sie unter [Verzögern der nächsten Aktion in Workflows](../connectors/connectors-native-delay.md).

<a name="start-time"></a>

## <a name="patterns-for-start-date-and-time"></a>Muster für Startdatum und -uhrzeit

Die folgenden Muster veranschaulichen, wie Sie die Wiederholung mit dem Startdatum und der Startuhrzeit steuern können und wie der Logic Apps-Dienst diese Wiederholungen ausführt:

| Startzeit | Wiederholung ohne Zeitplan | Wiederholung mit Zeitplan (nur Serientrigger) |
|------------|-----------------------------|----------------------------------------------------|
| {keine} | Die erste Workload wird sofort ausgeführt. <p>Zukünftige Workloads werden basierend auf der letzten Ausführungszeit ausgeführt. | Die erste Workload wird sofort ausgeführt. <p>Zukünftige Workloads werden basierend auf dem angegebenen Zeitplan ausgeführt. |
| Startuhrzeit liegt in der Vergangenheit | Trigger **Serie**: Die Ausführungszeiten werden basierend auf der angegebenen Startuhrzeit berechnet, und vergangene Ausführungszeiten werden verworfen. <p><p>Die nächste Workload wird zum nächsten zukünftigen Ausführungszeitpunkt ausgeführt. <p><p>Zukünftige Workloads werden basierend auf der letzten Ausführungszeit ausgeführt. <p><p>Trigger **Gleitendes Fenster**: Die Ausführungszeiten werden basierend auf der angegebenen Startuhrzeit berechnet, und vergangene Ausführungszeiten werden berücksichtigt. <p><p>Zukünftige Workloads werden basierend auf der angegebenen Startzeit ausgeführt. <p><p>Eine ausführlichere Erläuterung finden Sie im Beispiel im Anschluss an diese Tabelle. | Die erste Workload wird *frühestens* zur Startuhrzeit basierend auf dem Zeitplan ausgeführt, der anhand der Startuhrzeit berechnet wurde. <p><p>Zukünftige Workloads werden basierend auf dem angegebenen Zeitplan ausgeführt. <p><p>**Hinweis:** Wenn Sie eine Wiederholung mit einem Zeitplan angeben, aber keine Stunden oder Minuten für den Zeitplan, berechnet die Logik-App zukünftige Ausführungszeiten anhand der Stunden bzw. Minuten der ersten Ausführungszeit. |
| Startuhrzeit ist jetzt oder liegt in der Zukunft | Die erste Workload wird zur angegebenen Startuhrzeit ausgeführt. <p><p>Trigger **Wiederholung**: Zukünftige Workloads werden basierend auf der letzten Ausführungszeit ausgeführt. <p><p>Trigger **Gleitendes Fenster**: Zukünftige Workloads werden basierend auf der angegebenen Startzeit ausgeführt. | Die erste Workload wird *frühestens* zur Startuhrzeit basierend auf dem Zeitplan ausgeführt, der anhand der Startuhrzeit berechnet wurde. <p><p>Zukünftige Workloads werden basierend auf dem angegebenen Zeitplan ausgeführt. <p>**Hinweis:** Wenn Sie eine Wiederholung mit einem Zeitplan angeben, aber keine Stunden oder Minuten für den Zeitplan, berechnet die Logik-App zukünftige Ausführungszeiten anhand der Stunden bzw. Minuten der ersten Ausführungszeit. |
||||

*Beispiel für eine Startuhrzeit in der Vergangenheit mit Wiederholung, aber ohne Zeitplan*

Annahme: Das aktuelle Datum ist der 8. September 2017, die aktuelle Uhrzeit ist 13:00 Uhr. Sie geben den 7. September 2017 als Startdatum und 14:00 Uhr als Startuhrzeit (also einen Zeitpunkt in der Vergangenheit) sowie eine Wiederholung an, die alle zwei Tage ausgeführt wird.

| Startzeit | Die aktuelle Zeit | Serie | Zeitplan |
|------------|--------------|------------|----------|
| 2017-09-**07** T14:00:00Z <br>(**07**.09.2017 um 14:00 Uhr) | 2017-09-**08** T13:00:00Z <br>(**08**.09.2017 um 13:00 Uhr) | Alle zwei Tage | {keine} |
|||||

Für den Serientrigger berechnet die Logic Apps-Engine die Ausführungszeiten basierend auf der Startuhrzeit, verwirft vergangene Ausführungszeiten, verwendet die nächste in der Zukunft liegende Startuhrzeit für die erste Ausführung und berechnet zukünftige Ausführungen basierend auf der letzten Ausführungszeit.

Diese Wiederholung sieht wie folgt aus:

| Startzeit | Erste Ausführungszeit | Zukünftige Ausführungszeiten |
|------------|----------------|------------------|
| 2017-09-**07** um 14:00 Uhr | 2017-09-**09** um 14:00 Uhr | 2017-09-**11** um 14:00 Uhr </br>2017-09-**13** um 14:00 Uhr </br>2017-09-**15** um 14:00 Uhr </br>usw. |
||||

Es gilt also: Unabhängig davon, wie weit die angegebene Startzeit zurückliegt – z.B. am **05**.09.2017 um 14:00 Uhr oder am **01**.09.2017 um 14:00 Uhr –, wird für die erste Ausführung immer die nächste in der Zukunft liegende Startzeit verwendet.

Für den Trigger „Gleitendes Fenster“ berechnet die Logic Apps-Engine die Ausführungszeiten basierend auf der Startzeit, berücksichtigt vergangene Ausführungszeiten, verwendet die Startzeit für die erste Ausführung und berechnet zukünftige Ausführungen basierend auf der Startzeit.

Diese Wiederholung sieht wie folgt aus:

| Startzeit | Erste Ausführungszeit | Zukünftige Ausführungszeiten |
|------------|----------------|------------------|
| 2017-09-**07** um 14:00 Uhr | 2017-09-**08** um 13:00 (aktuelle Zeit) | 2017-09-**09** um 14:00 Uhr </br>2017-09-**11** um 14:00 Uhr </br>2017-09-**13** um 14:00 Uhr </br>2017-09-**15** um 14:00 Uhr </br>usw. |
||||

Es gilt also: Unabhängig davon, wie weit die angegebene Startzeit zurückliegt – z.B. am **05**.09.2017 um 14:00 Uhr oder am **01**.09.2017 um 14:00 Uhr –, wird für die erste Ausführung immer die angegebene Startzeit verwendet.

<a name="daylight-saving-standard-time"></a>

## <a name="recurrence-for-daylight-saving-time-and-standard-time"></a>Wiederholung für Sommerzeit und Normalzeit

Wiederkehrende, integrierte Trigger berücksichtigen den von Ihnen festgelegten Zeitplan, einschließlich der von Ihnen angegebenen Zeitzone. Wenn Sie keine Zeitzone auswählen, könnte sich die Sommerzeit darauf auswirken, wann Trigger ausgeführt werden, z. B. ein Verschieben der Startzeit um eine Stunde nach vorne, wenn die Sommerzeit beginnt, und eine Stunde zurück, wenn die Sommerzeit endet. Beim Planen von Aufträgen fügt Logic Apps die Nachricht zur Verarbeitung in die Warteschlange ein und gibt an, wann diese Nachricht verfügbar wird, basierend auf der UTC-Zeit, zu der der letzte Auftrag ausgeführt wurde, und der UTC-Zeit, zu der die Ausführung des nächsten Auftrags geplant ist.

Um diese Verschiebung zu vermeiden, damit Ihre Logik-App zu Ihrer angegebenen Startzeit ausgeführt wird, stellen Sie sicher, dass Sie eine Zeitzone auswählen. Auf diese Weise wird auch die UTC-Zeit für Ihre Logik-App verschoben, um die Sommerzeitumstellung zu berücksichtigen.

<a name="dst-window"></a>

> [!NOTE]
> Bei Triggern, die zwischen 2:00 Uhr und 3:00 Uhr beginnen, treten möglicherweise Probleme auf, da die Sommerzeitumstellung um 2:00 Uhr erfolgt, was dazu führen kann, dass die Startzeit ungültig oder mehrdeutig wird. Wenn Sie über mehrere Logik-Apps innerhalb desselben mehrdeutigen Intervalls verfügen, können sich diese überlappen. Aus diesem Grund sollten Sie die Startzeiten zwischen 2:00 Uhr und 3:00 Uhr vermeiden.

Nehmen wir beispielsweise an, Sie verfügen über zwei Logik-Apps, die täglich ausgeführt werden. Eine Logik-App wird um 1:30 Uhr Ortszeit ausgeführt, während die andere eine Stunde später um 2:30 Uhr Ortszeit ausgeführt wird. Was geschieht mit den Startzeiten für diese Apps, wenn die Sommerzeit beginnt bzw. endet?

* Werden die Trigger überhaupt ausgeführt, wenn die Zeit eine Stunde nach vorne verschoben wird?

* Werden die Trigger zweimal ausgeführt, wenn die Zeit eine Stunde zurück verschoben wird?

Wenn diese Logik-Apps die Zeitzone UTC-6:00 Central Time (USA und Kanada) verwenden, wird in dieser Simulation veranschaulicht, wie die UTC-Zeiten in 2019 verschoben wurden, um die Sommerzeitumstellungen zu berücksichtigen, wobei nach Bedarf um eine Stunde zurück oder nach vorne verschoben wurde, damit die Apps weiterhin zu den erwarteten lokalen Zeiten ausgeführt wurden, ohne Ausführungen zu überspringen oder zu duplizieren.

* **10.03.2019: Die Sommerzeit beginnt um 2:00 Uhr, wodurch die Zeit um eine Stunde nach vorne verschoben wird.**

  Um nach dem Beginn der Sommerzeit diese zu kompensieren, verschiebt sich die UTC-Zeit um eine Stunde zurück, sodass Ihre Logik-App weiterhin zur selben Ortszeit ausgeführt wird:

  * Logik-App #1

    | Date | Uhrzeit (Ortszeit) | Uhrzeit (UTC) | Notizen |
    |------|--------------|------------|-------|
    | 09.03.2019 | 1:30:00 | 7:30:00 | Die UTC-Zeit vor dem Tag, an dem die Sommerzeit beginnt. |
    | 10.03.2019 | 1:30:00 | 7:30:00 | UTC ist identisch, da die Sommerzeit noch nicht begonnen hat. |
    | 11.03.2019 | 1:30:00 | 6:30:00 AM | Die UTC-Zeit wurde um eine Stunde zurück verschoben, nachdem die Sommerzeit begonnen hat. |
    |||||

  * Logik-App #2

    | Date | Uhrzeit (Ortszeit) | Uhrzeit (UTC) | Notizen |
    |------|--------------|------------|-------|
    | 09.03.2019 | 2:30:00 | 8:30:00 | Die UTC-Zeit vor dem Tag, an dem die Sommerzeit beginnt. |
    | 10.03.2019 | 3:30:00* | 8:30:00 | Die Sommerzeit hat bereits begonnen, daher wurde die Ortszeit eine Stunde nach vorne verschoben, weil sich die UTC-6:00-Zeitzone in UTC-5:00 ändert. Weitere Informationen finden Sie unter [Trigger, die zwischen 2:00 Uhr und 3:00 Uhr starten](#dst-window). |
    | 11.03.2019 | 2:30:00 | 7:30:00 | Die UTC-Zeit wurde um eine Stunde zurück verschoben, nachdem die Sommerzeit begonnen hat. |
    |||||

* **03.11.2019: Die Sommerzeit endet um 2:00 Uhr und verschiebt die Zeit um eine Stunde zurück.**

  Zur Kompensation verschiebt sich die UTC-Zeit um eine Stunde nach vorne, sodass Ihre Logik-App weiterhin zur selben Ortszeit ausgeführt wird:

  * Logik-App #1

    | Date | Uhrzeit (Ortszeit) | Uhrzeit (UTC) | Notizen |
    |------|--------------|------------|-------|
    | 02.11.2019 | 1:30:00 | 6:30:00 AM ||
    | 03.11.2019 | 1:30:00 | 6:30:00 AM ||
    | 04.11.2019 | 1:30:00 | 7:30:00 ||
    |||||

  * Logik-App #2

    | Date | Uhrzeit (Ortszeit) | Uhrzeit (UTC) | Notizen |
    |------|--------------|------------|-------|
    | 02.11.2019 | 2:30:00 | 7:30:00 ||
    | 03.11.2019 | 2:30:00 | 8:30:00 ||
    | 04.11.2019 | 2:30:00 | 8:30:00 ||
    |||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Einmalige Ausführung

Wenn Sie Ihre Logik-App nur einmal in der Zukunft ausführen möchten, können Sie folgende Vorlage verwenden: **Planer: Einmalige Aufträge ausführen** öffnen. Wählen Sie nach dem Erstellen einer neuen Logik-App, aber vor dem Öffnen des Designers für Logik-Apps im Abschnitt **Vorlagen** in der Liste **Kategorie** die Option **Zeitplan** aus, und klicken Sie dann auf die folgende Vorlage:

![Auswählen der Vorlage „Scheduler: Einmalige Aufträge ausführen](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

Wenn Sie Ihre Logik-App mit dem Trigger **Beim Empfang einer HTTP-Anforderung – Anforderung** starten, können Sie die Startzeit als Parameter für den Trigger übergeben. Verwenden Sie **Verzögern bis – Zeitplan** als erste Aktion, und geben Sie die Uhrzeit ein, zu der die nächste Aktion ausgeführt wird.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Beispiele für Serien

Im Folgenden finden Sie einige Beispiele für Serien, die Sie für die Trigger einrichten können, die die Optionen unterstützen:

| Trigger | Serie | Intervall | Häufigkeit | Startzeit | An diesen Tagen | Zu diesen Stunden | Zu diesen Minuten | Hinweis |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Serie, <br>Schiebefenster | Ausführung alle 15 Minuten (ohne Startdatum und -uhrzeit) | 15 | Minute | {keine} | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird sofort gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. |
| Serie, <br>Schiebefenster | Ausführung alle 15 Minuten (mit Startdatum und -uhrzeit) | 15 | Minute | *startDate* T *startTime* Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. |
| Serie, <br>Schiebefenster | Ausführung einmal pro Stunde zur vollen Stunde (mit Startdatum und -uhrzeit) | 1 | Hour | *startDate* Thh:00:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet. Zukünftige Wiederholungen werden jede Stunde beim Minutenwert „00“ ausgeführt, den Logic Apps aus der Startzeit berechnet. <p>Wenn für die Häufigkeit „Week“ (Woche) oder „Month“ (Monat) ausgewählt wurde, wird der Zeitplan nur an einem Tag der Woche bzw. einem Tag des Monats ausgeführt. |
| Serie, <br>Schiebefenster | Ausführung täglich einmal pro Stunde (ohne Startdatum und -uhrzeit) | 1 | Hour | {keine} | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird sofort gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. <p>Wenn für die Häufigkeit „Week“ (Woche) oder „Month“ (Monat) ausgewählt wurde, wird der Zeitplan nur an einem Tag der Woche bzw. einem Tag des Monats ausgeführt. |
| Serie, <br>Schiebefenster | Ausführung täglich einmal pro Stunde (mit Startdatum und -uhrzeit) | 1 | Hour | *startDate* T *startTime* Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet, und anschließend werden basierend auf der letzten Ausführungszeit zukünftige Wiederholungen berechnet. <p>Wenn für die Häufigkeit „Week“ (Woche) oder „Month“ (Monat) ausgewählt wurde, wird der Zeitplan nur an einem Tag der Woche bzw. einem Tag des Monats ausgeführt. |
| Serie, <br>Schiebefenster | Ausführung einmal pro Stunde jeweils 15 Minuten nach der vollen Stunde (mit Startdatum und -uhrzeit) | 1 | Hour | *startDate* T00:15:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet. Zukünftige Wiederholungen werden jede Stunde beim Minutenwert „15“ ausgeführt, den Logic Apps aus der Startzeit berechnet, sodass die Ausführungen also um 00:15 Uhr, 01:15 Uhr, 02:15 Uhr usw. stattfinden. |
| Serie | Ausführung einmal pro Stunde jeweils 15 Minuten nach der vollen Stunde (ohne Startdatum und -uhrzeit) | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Dieser Zeitplan wird um 00:15, 1:15, 2:15 usw. ausgeführt. Außerdem entspricht dieser Zeitplan der Häufigkeit „Hour“ (Stunde) mit einer Startuhrzeit von „15“ Minuten. |
| Serie | Ausführung alle 15 Minuten beim angegebenen Minutenwert (ohne Startdatum und -uhrzeit). | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dieser Zeitplan wird erst gestartet, wenn die nächste angegebene 15-Minuten-Marke erreicht ist. |
| Serie | Ausführung täglich um 8 Uhr *plus* die Minutenmarkierung, zu der Sie Ihre Logik-App speichern. | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8 | {keine} | Ohne Startdatum und -uhrzeit wird dieser Zeitplan auf Grundlage des Zeitpunkts ausgeführt, zu dem Sie die Logik-App speichern (PUT-Vorgang). |
| Serie | Ausführung täglich um 8:00 Uhr (mit Startdatum und -uhrzeit) | 1 | Day (Tag) | *startDate* T08:00:00Z | {nicht verfügbar} | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet. Zukünftige Vorkommen werden täglich um 8:00 Uhr ausgeführt. | 
| Serie | Ausführung täglich um 8:00 Uhr (ohne Startdatum und -uhrzeit) | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8 | 00 | Dieser Zeitplan wird jeden Tag um 8:00 Uhr ausgeführt. |
| Serie | Ausführung täglich um 8:00 Uhr und 16:00 Uhr | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8, 16 | 0 | |
| Serie | Ausführung täglich um 8:30 Uhr, 8:45 Uhr, 16:30 Uhr und 16:45 Uhr. | 1 | Day (Tag) | {keine} | {nicht verfügbar} | 8, 16 | 30, 45 | |
| Serie | Ausführung jeden Samstag um 17:00 Uhr (ohne Startdatum und -uhrzeit) | 1 | Week | {keine} | „Saturday“ (Samstag) | 17 | 0 | Dieser Zeitplan wird jeden Samstag um 17:00 Uhr ausgeführt. |
| Serie | Ausführung jeden Samstag um 17:00 Uhr (mit Startdatum und -uhrzeit) | 1 | Week | *startDate* T17:00:00Z | „Saturday“ (Samstag) | {keine} | {keine} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur entsprechenden Uhrzeit gestartet, also hier am 9. September 2017 um 17:00 Uhr. Zukünftige Wiederholungen werden jeden Samstag um 17:00 Uhr ausgeführt. |
| Serie | Ausführung jeden Dienstag und Donnerstag um 17 Uhr *plus* die Minutenmarkierung, zu der Sie Ihre Logik-App speichern.| 1 | Week | {keine} | „Tuesday“ (Dienstag), „Thursday“ (Donnerstag) | 17 | {keine} | |
| Serie | Ausführung einmal pro Stunde während der Geschäftszeiten. | 1 | Week | {keine} | Wählen Sie alle Tage mit Ausnahme von Samstag und Sonntag aus. | Wählen Sie die gewünschten Stunden des Tages aus. | Wählen Sie die gewünschten Minuten der Stunde aus. | Wenn Ihre Geschäftszeiten beispielsweise den Zeitraum von 8:00 Uhr bis 17:00 Uhr umfassen, wählen Sie „8, 9, 10, 11, 12, 13, 14, 15, 16, 17“ als Stunden des Tages *plus* „0“ als Minuten der Stunde aus. |
| Serie | Ausführung einmal pro Tag an Wochenenden | 1 | Week | {keine} | „Saturday“ (Samstag), „Sunday“ (Sonntag) | Wählen Sie die gewünschten Stunden des Tages aus. | Wählen Sie wie gewünscht die Minuten der Stunde aus. | Dieser Zeitplan wird jeden Samstag und Sonntag basierend auf dem angegebenen Zeitplan ausgeführt. |
| Serie | Ausführung alle zwei Wochen nur am Montag im Abstand von 15 Minuten | 2 | Week | {keine} | „Monday“ (Montag) | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Dieser Zeitplan wird jeden zweiten Montag jeweils zur 15-Minuten-Marke ausgeführt. |
| Serie | Ausführung jeden Monat | 1 | Month (Monat) | *startDate* T *startTime* Z | {nicht verfügbar} | {nicht verfügbar} | {nicht verfügbar} | Dieser Zeitplan wird *frühestens* am angegebenen Startdatum zur angegebenen Uhrzeit gestartet. Zukünftige Wiederholungen werden basierend auf Startdatum und -uhrzeit berechnet. Wenn Sie Startdatum und -uhrzeit nicht angeben, werden in diesem Zeitplan das Datum und die Uhrzeit der Erstellung verwendet. |
| Serie | Ausführung jede Stunde an einem Tag des Monats | 1 | Month (Monat) | {siehe Hinweis} | {nicht verfügbar} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {siehe Hinweis} | Wenn Sie Startdatum und -uhrzeit nicht angeben, werden in diesem Zeitplan das Datum und die Uhrzeit der Erstellung verwendet. Geben Sie die Minuten der Stunde oder eine Startuhrzeit an, oder verwenden Sie die Uhrzeit der Erstellung, um die Minuten für den Zeitplan der Wiederholung zu steuern. Wenn die Startuhrzeit oder die Uhrzeit der Erstellung beispielsweise 8:25 Uhr lautet, wird dieser Zeitplan um 8:25, 9:25, 10:25 usw. ausgeführt. |
|||||||||

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen, Planen und Ausführen von wiederkehrenden Aufgaben und Workflows mit dem Serientrigger](../connectors/connectors-native-recurrence.md)
* [Erstellen, Planen und Ausführen von wiederkehrenden Aufgaben und Workflows mit dem Trigger „Gleitendes Fenster“](../connectors/connectors-native-sliding-window.md)
* [Anhalten von Workflows mit Verzögerungsaktionen](../connectors/connectors-native-delay.md)
