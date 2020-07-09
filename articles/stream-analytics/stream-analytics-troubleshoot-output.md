---
title: Problembehandlung von Azure Stream Analytics-Ausgaben
description: In diesem Artikel werden Verfahren zur Problembehandlung Ihrer Ausgangsverbindungen in Azure Stream Analytics-Aufträgen beschrieben.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: fc35e6a723afab3f230aa91e4b6895aead35e141
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037068"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Problembehandlung von Azure Stream Analytics-Ausgaben

In diesem Artikel werden häufige Probleme mit Azure Stream Analytics-Ausgabeverbindungen sowie das Troubleshooting von Ausgabeproblemen beschrieben. Für viele Troubleshootingschritte müssen Ressourcen- und andere Diagnoseprotokolle für Ihren Stream Analytics-Auftrag aktiviert sein. Wenn Sie Ressourcenprotokolle nicht aktiviert haben, finden Sie weitere Informationen unter [Problembehandlung von Azure Stream Analytics mit Ressourcenprotokollen](stream-analytics-job-diagnostic-logs.md).

## <a name="the-job-doesnt-produce-output"></a>Der Auftrag erzeugt keine Ausgabe.

1. Überprüfen Sie für die einzelnen Ausgaben die Verbindung mit Ausgaben mithilfe der Schaltfläche **Verbindung testen**.
1. Sehen Sie sich die [Überwachungsmetriken](stream-analytics-monitoring.md) auf der Registerkarte **Überwachen** an. Da die Werte aggregiert werden, werden die Metriken mit einer Verzögerung von wenigen Minuten angezeigt.

   * Wenn der Wert **Eingabeereignisse** größer als null ist, kann der Auftrag die Eingabedaten lesen. Wenn der Wert **Eingabeereignisse** nicht größer als null ist, liegt ein Problem mit der Eingabe des Auftrags vor. Weitere Informationen finden Sie unter [Troubleshooting bei Eingabeverbindungen](stream-analytics-troubleshoot-input.md).
   * Wenn der Wert **Datenkonvertierungsfehler** größer als null und ansteigend ist, finden Sie unter [Datenfehler in Azure Stream Analytics](data-errors.md) ausführliche Informationen zu Datenkonvertierungsfehlern.
   * Wenn der Wert **Laufzeitfehler** größer als null ist, empfängt der Auftrag Daten, generiert jedoch beim Verarbeiten der Abfrage Fehler. Navigieren Sie zur Fehlersuche zu [Überwachungsprotokolle](../azure-resource-manager/management/view-activity-logs.md), und filtern Sie dann nach dem Status **Fehler**.
   * Wenn der Wert **Eingabeereignisse** größer als null und der Wert **Ausgabeereignisse** gleich null ist, gilt eine der folgenden Aussagen:
      * Die Abfrageverarbeitung führte zu null Ausgabeereignissen.
      * Ereignisse oder Felder sind unter Umständen falsch formatiert, daher wurden bei der Abfrageverarbeitung keine Ausgaben generiert.
      * Im Rahmen des Auftrags können Daten aus Konnektivitäts- oder Authentifizierungsgründen nicht per Push an die Ausgabesenke übertragen werden.

   Die Meldungen der Vorgangsprotokolle enthalten weitere Details, einschließlich Details zu den Vorgängen. Ausgenommen sind Fälle, in denen die Abfragelogik alle Ereignisse herausfiltert. Wenn die Verarbeitung für mehrere Ereignisse Fehler generiert, werden die Fehler alle 10 Minuten aggregiert.

## <a name="the-first-output-is-delayed"></a>Verzögerung bei der ersten Ausgabe

Wenn ein Stream Analytics-Auftrag gestartet wird, werden die Eingabeereignisse gelesen. Unter bestimmten Umständen kann es jedoch zu einer Verzögerung bei der Ausgabe kommen.

Große Zeitwerte in zeitlichen Abfrageelementen können zur Ausgabeverzögerung beitragen. Um die richtige Ausgabe über große Zeitfenster zu erzeugen, liest der Streamingauftrag Daten ab dem spätestmöglichen Zeitpunkt zum Füllen des Zeitfensters. Die Daten können bis zu sieben Tage in der Vergangenheit liegen. Es wird keine Ausgabe erzeugt, bis die ausstehenden Eingabeereignisse gelesen wurden. Dieses Problem kann auftreten, wenn das System ein Upgrade der Streamingaufträge ausführt. Bei einem Upgrade wird der Auftrag neu gestartet. Solche Upgrades finden in der Regel alle paar Monate statt.

Formulieren Sie Ihre Stream Analytics-Abfrage entsprechend. Wenn Sie ein großes Zeitfenster für temporale Elemente in der Abfragesyntax des Auftrags verwenden, kann dies zu einer Verzögerung bei der ersten Ausgabe beim Start oder Neustart des Auftrags führen. Als großes Zeitfenster wird eine längere Zeit als mehrere Stunden (bis zu sieben Tage) angesehen.

Eine Lösung für diese Art einer Verzögerung der ersten Ausgabe ist die Verwendung von Abfrageparallelisierungstechniken, z. B. Partitionierung der Daten. Sie können auch weitere Streamingeinheiten hinzufügen, um den Durchsatz zu verbessern, bis der Auftrag wieder aktuell ist.  Weitere Informationen finden Sie unter [Überlegungen beim Erstellen von Stream Analytics-Aufträgen](stream-analytics-concepts-checkpoint-replay.md).

Diese Faktoren haben Auswirkungen auf die Zeit bis zur ersten Ausgabe:

* Verwendung von Aggregaten im Fenstermodus (etwa eine GROUP BY-Klausel von rollierenden, springenden und gleitenden Fenstern):

  * Für Aggregate von rollierenden oder springenden Fenstern werden die Ergebnisse am Ende des Fensterzeitraums generiert.
  * Für ein gleitendes Fenster werden die Ergebnisse generiert, wenn ein Ereignis in das gleitende Fenster eintritt oder dieses verlässt.
  * Wenn Sie die Verwendung eines großen Fensters planen (z. B. mehr als eine Stunde), empfiehlt es sich, ein springendes oder gleitendes Fenster auszuwählen. Mit diesen Fenstertypen erfolgt eine häufigere Ausgabe.

* Verwendung von temporalen Joins (z. B. JOIN mit DATEDIFF):
  * Übereinstimmungen werden generiert, sobald beide Seiten der übereinstimmenden Ereignisse eintreten.
  * Daten ohne Übereinstimmung (z. B. LEFT OUTER JOIN) werden am Ende des DATEDIFF-Fensters für jedes Ereignis auf der linken Seite generiert.

* Verwendung von temporalen Analysefunktionen (etwa ISFIRST, LAST und LAG mit LIMIT DURATION):
  * Bei analytischen Funktionen wird die Ausgabe für jedes Ereignis generiert. Es gibt keine Verzögerung.

## <a name="the-output-falls-behind"></a>Zurückfallende Ausgabe

Während des normalen Betriebs eines Auftrags kann die Ausgabe immer längere Verzögerungen aufweisen. Wenn die Ausgabe auf diese Weise zurückfällt, können Sie die Grundursachen durch Untersuchen der folgenden Faktoren ermitteln:

* Ob die Downstreamsenke gedrosselt wird.
* Ob die Upstreamquelle gedrosselt wird.
* Ob die Verarbeitungslogik in der Abfrage rechenintensiv ist.

Um die Ausgabedetails anzuzeigen, wählen Sie im Azure-Portal den Streamingauftrag und dann **Auftragsdiagramm** aus. Für jede Eingabe gibt es eine Backlogereignismetrik pro Partition. Wenn die Metrik weiter ansteigt, weist dies auf begrenzte Systemressourcen hin. Diese Steigung kann auf eine Drosselung der Ausgabesenke oder eine hohe CPU-Auslastung zurückzuführen sein. Weitere Informationen finden Sie unter [Datengesteuertes Debuggen mithilfe des Auftragsdiagramms](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Warnung vor Schlüsselverletzungen bei der Ausgabe von Azure SQL-Datenbank

Wenn Sie eine Azure SQL-Datenbank-Instanz als Ausgabe für einen Stream Analytics-Auftrag konfigurieren, werden Datensätze als Masseneintrag in der Zieltabelle hinzugefügt. Grundsätzlich garantiert Azure Stream Analytics [At-Least-Once-Übermittlung](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) an die Ausgabesenke. Sie können nach wie vor [Exactly-Once-Übermittlung]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) an eine SQL-Ausgabe erzielen, wenn für eine SQL-Tabelle eine UNIQUE-Einschränkung definiert ist.

Wenn Sie Einschränkungen für eindeutige Schlüssel in der SQL-Tabelle einrichten, entfernt Azure Stream Analytics doppelte Datensätze. Es teilt die Daten in Batches auf und fügt diese rekursiv ein, bis ein einziger doppelter Datensatz gefunden wird. Beim Aufteilen und Einfügen werden die Duplikate einzeln ignoriert. Bei einem Streamingauftrag mit vielen doppelten Zeilen ist dieses Verfahren ineffizient und zeitaufwendig. Wenn Sie im Aktivitätsprotokoll mehrere Warnmeldungen zu Schlüsselverstößen in der letzten Stunde finden, ist es wahrscheinlich, dass Ihre SQL-Ausgabe den gesamten Auftrag verlangsamt.

Um dieses Problem zu beheben, [konfigurieren Sie den Index]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql), der die Schlüsselverstöße verursacht, indem Sie die Option IGNORE_DUP_KEY aktivieren. Diese Option ermöglicht es SQL, doppelte Werte bei Masseneinfügungen zu ignorieren. Azure SQL-Datenbank gibt lediglich eine Warnmeldung anstelle eines Fehlers aus. Dies führt dazu, dass Azure Stream Analytics keine Fehler aufgrund von Primärschlüsselverletzungen mehr erzeugt.

Beachten Sie die folgenden Beobachtungen, wenn Sie IGNORE_DUP_KEY für mehrere Typen von Indizes konfigurieren:

* IGNORE_DUP_KEY kann nicht für einen Primärschlüssel oder eine UNIQUE-Einschränkung festgelegt werden, die ALTER INDEX verwendet. Sie müssen den Index löschen und neu erstellen.  
* Sie können IGNORE_DUP_KEY mithilfe von ALTER INDEX für einen eindeutigen Index festlegen. Diese Instanz unterscheidet sich von einer PRIMARY KEY-/UNIQUE-Einschränkung und wird mithilfe einer CREATE INDEX- oder INDEX-Definition erstellt.  
* Die Option IGNORE_DUP_KEY gilt nicht für ColumnStore-Indizes, da für diese keine Eindeutigkeit erzwungen werden kann.  

## <a name="column-names-are-lowercase-in-azure-stream-analytics-10"></a>Spaltennamen werden in Azure Stream Analytics (1.0) in Kleinbuchstaben umgewandelt.

Bei Verwendung des ursprünglichen Kompatibilitätsgrads (1.0) werden Spaltennamen von Azure Stream Analytics in Kleinbuchstaben geändert. Dieses Verhalten wurde bei höheren Kompatibilitätsgraden behoben. Um die Groß-/Kleinschreibung beizubehalten, wechseln Sie zu einem Kompatibilitätsgrad ab 1.1. Weitere Informationen finden Sie unter [Kompatibilitätsgrad für Stream Analytics-Aufträge](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level).

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Weitere Unterstützung finden Sie auf der [Frageseite von Microsoft Q&A (Fragen und Antworten) zu Azure Stream Analytics](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Referenz zur Azure Stream Analytics-Abfragesprache](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Stream Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)
