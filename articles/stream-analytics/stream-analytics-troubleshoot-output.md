---
title: Problembehandlung von Azure Stream Analytics-Ausgaben
description: In diesem Artikel werden Verfahren zur Problembehandlung Ihrer Ausgangsverbindungen in Azure Stream Analytics-Aufträgen beschrieben.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/31/2020
ms.custom: seodec18
ms.openlocfilehash: 305632a0faa1eb7e217e86d36c5159e557df7aaf
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409249"
---
# <a name="troubleshoot-azure-stream-analytics-outputs"></a>Problembehandlung von Azure Stream Analytics-Ausgaben

In diesem Artikel werden häufige Probleme mit Azure Stream Analytics-Ausgabeverbindungen beschrieben, die Behandlung von Ausgabeproblemen und die Behebung der Probleme. Viele Schritte zur Problembehandlung erfordern die Aktivierung von Diagnoseprotokollen für Ihren Stream Analytics-Auftrag. Wenn Sie die Diagnoseprotokolle nicht aktiviert haben, finden Sie weitere Informationen unter [Problembehandlung bei Azure Stream Analytics mit Diagnoseprotokollen](stream-analytics-job-diagnostic-logs.md).

## <a name="output-not-produced-by-job"></a>Nicht vom Auftrag erzeugte Ausgabe

1.  Überprüfen Sie für die einzelnen Ausgaben die Verbindung mit Ausgaben mithilfe der Schaltfläche **Verbindung testen**.

2.  Sehen Sie sich die [**Überwachungsmetriken**](stream-analytics-monitoring.md) auf der Registerkarte **Überwachen** an. Da die Werte aggregiert werden, werden die Metriken mit einer Verzögerung von wenigen Minuten angezeigt.
   * Wenn Eingabeereignisse größer als 0 sind, ist der Auftrag in der Lage, Eingabedaten zu lesen. Wenn Eingabeereignisse nicht größer als 0 sind, liegt ein Problem mit der Auftragseingabe vor. Weitere Informationen zur Problembehandlung bei Eingabeverbindungen finden Sie unter [Problembehandlung für Eingabeverbindungen](stream-analytics-troubleshoot-input.md).
   * Wenn Datenkonvertierungsfehler größer als 0 und ansteigend sind, finden Sie unter [Azure Stream Analytics-Datenfehler](data-errors.md) ausführliche Informationen zu Datenkonvertierungsfehlern.
   * Wenn die Laufzeitfehler größer als 0 sind, kann Ihr Auftrag Daten empfangen, aber er erzeugt Fehler bei der Verarbeitung der Abfrage. Navigieren Sie zur Fehlersuche zu [Überwachungsprotokolle](../azure-resource-manager/management/view-activity-logs.md) und filtern Sie nach dem Status *Fehler*.
   * Wenn „InputEvents“ größer als 0 und „OutputEvents“ gleich 0 ist, trifft eine der folgenden Aussagen zu:
      * Die Abfrageverarbeitung hat zu null Ausgabeereignissen geführt.
      * Ereignisse oder Felder sind unter Umständen falsch formatiert, daher wurden bei der Abfrageverarbeitung keine Ausgaben generiert.
      * Im Rahmen des Auftrags können Daten aus Konnektivitäts- oder Authentifizierungsgründen nicht per Push an die Ausgabesenke übertragen werden.

   Für alle zuvor genannten Fehler enthalten die Meldungen der Vorgangsprotokolle weitere Details (einschließlich Details dazu, was geschieht). Ausgenommen sind Fälle, in denen die Abfragelogik alle Ereignisse herausgefiltert hat. Wenn die Verarbeitung mehrerer Ereignisse Fehler generiert, werden die Fehler alle 10 Minuten aggregiert.

## <a name="job-output-is-delayed"></a>Verzögerung bei der Auftragsausgabe

### <a name="first-output-is-delayed"></a>Verzögerung bei der ersten Ausgabe

Wenn ein Stream Analytics-Auftrag gestartet wird, werden die Eingabeereignisse gelesen, aber es kann unter bestimmten Umständen zu einer Verzögerung bei der Ausgabe kommen.

Große Zeitwerte in zeitlichen Abfrageelementen können zur Ausgabeverzögerung beitragen. Um eine korrekte Ausgabe für große Zeitfenster zu erzeugen, startet der Streamingauftrag mit dem Lesen der Daten vom spätesten möglichen Zeitpunkt (bis zu sieben Tage her), um das Zeitfenster zu füllen. Während dieser Zeit wird keine Ausgabe erzeugt, bis das Auslesen der ausstehenden Eingabeereignisse abgeschlossen ist. Dieses Problem kann auftreten, wenn das System ein Upgrade der Streamingaufträge ausführt und damit den Auftrag neu startet. Solche Upgrades finden in der Regel alle paar Monate statt.

Formulieren Sie Ihre Stream Analytics-Abfrage daher nach Ihrem Ermessen. Wenn Sie ein großes Zeitfenster (mehr als mehrere Stunden, bis zu sieben Tage) für zeitliche Elemente in der Abfragesyntax des Auftrags verwenden, kann dies zu einer Verzögerung bei der ersten Ausgabe beim Start oder Neustart des Auftrags führen.  

Eine Lösung für diese Art der ersten Ausgabeverzögerung ist die Verwendung von Abfrageparallelisierungsmethoden (Partitionierung der Daten) oder das Hinzufügen weiterer Streamingeinheiten, um den Durchsatz zu verbessern, bis der Auftrag aufgeholt wird.  Weitere Informationen finden Sie unter [Überlegungen beim Erstellen von Stream Analytics-Aufträgen](stream-analytics-concepts-checkpoint-replay.md).

Diese Faktoren haben Auswirkungen auf die Schnelligkeit der ersten Ausgabe, die erzeugt wird:

1. Verwenden von Aggregaten im Fenstermodus (GROUP BY von rollierenden, springenden und gleitenden Fenstern)
   - Für Aggregate von rollierenden oder springenden Fenstern werden die Ergebnisse am Ende des Fensterzeitraums generiert.
   - Für ein gleitendes Fenster werden die Ergebnisse generiert, wenn ein Ereignis in das gleitende Fenster eintritt oder dieses verlässt.
   - Wenn Sie eine große Fenstergröße (> 1 Stunde) planen, ist es am besten, wenn Sie sich für ein springendes oder gleitendes Fenster entscheiden, damit Sie die Ausgabe häufiger sehen können.

2. Verwenden von temporalen Joins (JOIN mit DATEDIFF-Funktion)
   - Übereinstimmungen werden generiert, sobald beide Seiten der übereinstimmenden Ereignisse eintreten.
   - Daten ohne Übereinstimmung (LEFT OUTER JOIN) werden am Ende des DATEDIFF-Fensters für jedes Ereignis auf der linken Seite generiert.

3. Verwenden von temporalen Analysefunktionen (ISFIRST, LAST und LAG mit LIMIT DURATION-Funktion)
   - Bei analytischen Funktionen wird die Ausgabe für jedes Ereignis generiert. Es gibt keine Verzögerung.

### <a name="output-falls-behind"></a>Zurückfallende Ausgabe

Wenn Sie während der normalen Ausführung des Auftrags feststellen, dass die Ausgabe des Auftrags hinterherhinkt (immer längere Wartezeit), können Sie die Grundursachen ermitteln, indem Sie diese Faktoren daraufhin untersuchen:
- Ob die Downstreamsenke gedrosselt wird.
- Ob die Upstreamquelle gedrosselt wird.
- Ob die Verarbeitungslogik in der Abfrage rechenintensiv ist.

Um diese Details anzuzeigen, wählen Sie im Azure-Portal den Streamingauftrag und das **Auftragsdiagramm** aus. Für jede Eingabe gibt es eine Backlogereignismetrik pro Partition. Wenn die Backlogereignismetrik weiter ansteigt, ist dies ein Indikator für begrenzte Systemressourcen. Gründe hierfür können eine Drosselung der Ausgabesenke oder eine hohe CPU-Auslastung sein. Weitere Informationen zur Verwendung des Auftragsdiagramms finden Sie unter [Datengesteuertes Debuggen mithilfe des Auftragsdiagramms](stream-analytics-job-diagram-with-metrics.md).

## <a name="key-violation-warning-with-azure-sql-database-output"></a>Warnung vor Schlüsselverletzungen bei der Ausgabe von Azure SQL-Datenbank

Wenn Sie Azure SQL-Datenbank als Ausgabe an einen Stream Analytics-Auftrag konfigurieren, fügt dieser die Datensätze als Masseneintrag in der Zieltabelle hinzu. Im Allgemeinen garantiert Azure Stream Analytics [mindestens eine Übermittlung](https://docs.microsoft.com/stream-analytics-query/event-delivery-guarantees-azure-stream-analytics) an die Ausgabesenke. Sie können jedoch auch eine [genau einmalige Übermittlung]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) an die SQL-Ausgabe erreichen, wenn für die SQL-Tabelle eine UNIQUE-Einschränkung definiert wurde.

Sobald eindeutige Schlüsseleinschränkungen in der SQL-Tabelle festgelegt sind und doppelte Datensätze in die SQL-Tabelle eingefügt werden, entfernt Azure Stream Analytics den doppelten Datensatz. Es teilt die Daten in Batches auf und fügt die Batches rekursiv ein, bis ein einziger doppelter Datensatz gefunden wird. Wenn der Streamingauftrag eine beträchtliche Anzahl von doppelten Zeilen hat, muss dieser Teilen-und-Einfügen-Prozess die Duplikate einzeln ignorieren. Dies ist weniger effizient und zeitaufwendig. Wenn Sie im Aktivitätsprotokoll mehrere Warnmeldungen zu Schlüsselverstößen innerhalb der letzten Stunde finden, ist es äußerst wahrscheinlich, dass Ihre SQL-Ausgabe den gesamten Auftrag verlangsamt.

Um dieses Problem zu beheben, sollten Sie [den Index konfigurieren]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql), der die Schlüsselverstöße verursacht, indem Sie die Option IGNORE_DUP_KEY aktivieren. Durch Aktivieren dieser Option wird bei Masseneinfügungen das Ignorieren doppelter Werte durch SQL zugelassen. SQL Azure generiert anstelle eines Fehlers einfach eine Warnung. Azure Stream Analytics gibt daraufhin keine Fehler zu Primärschlüsselverstößen mehr zurück.

Beachten Sie die folgenden Beobachtungen, wenn Sie IGNORE_DUP_KEY für mehrere Typen von Indizes konfigurieren:

* IGNORE_DUP_KEY kann nicht für eine PRIMARY KEY- oder UNIQUE-Einschränkung festgelegt werden, die ALTER INDEX verwendet. Sie müssen stattdessen den Index löschen und neu erstellen.  
* Sie können die Option IGNORE_DUP_KEY mit ALTER INDEX für einen eindeutigen Index festlegen, wenn sich dieser von der PRIMARY KEY-/UNIQUE-Einschränkung unterscheidet und mit einer CREATE INDEX- oder INDEX-Definition erstellt wurde.  

* IGNORE_DUP_KEY gilt nicht für Spaltenspeicherindizes, da für solche Indizes keine Eindeutigkeit erzwungen werden kann.  

## <a name="column-names-are-lower-cased-by-azure-stream-analytics"></a>Spaltennamen werden von Azure Stream Analytics in Kleinbuchstaben umgewandelt
Bei Verwendung des ursprünglichen Kompatibilitätsgrads (1.0) wurden Spaltennamen von Azure Stream Analytics in Kleinbuchstaben geändert. Dieses Verhalten wurde bei höheren Kompatibilitätsgraden behoben. Um die Groß-/Kleinschreibung beizubehalten, empfehlen wir Kunden, zum Kompatibilitätsgrad 1.1 und höher zu wechseln. Weitere Informationen finden Sie unter [Kompatibilitätsgrad für Azure Stream Analytics-Aufträge](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-compatibility-level).

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
