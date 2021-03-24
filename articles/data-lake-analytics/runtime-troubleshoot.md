---
title: Problembehandlung für U-SQL-Laufzeitfehler in Azure Data Lake Analytics
description: Erfahren Sie, wie Sie die Problembehandlung für U-SQL-Laufzeitfehler durchführen.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: troubleshooting
ms.date: 10/10/2019
ms.openlocfilehash: 1236b83b410057e55015391772e37bd461a448d0
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102030612"
---
# <a name="learn-how-to-troubleshoot-u-sql-runtime-failures-due-to-runtime-changes"></a>Problembehandlung für U-SQL-Laufzeitfehler aufgrund von Laufzeitänderungen

Mit der Azure Data Lake-U-SQL-Laufzeit, einschließlich Compiler, Optimierer und Auftrags-Manager, wird Ihr U-SQL-Code verarbeitet.

## <a name="choosing-your-u-sql-runtime-version"></a>Auswählen der U-SQL-Laufzeitversion

Wenn Sie U-SQL-Aufträge über Visual Studio, das ADL SDK oder das Azure Data Lake Analytics Portal übermitteln, wird für Ihren Auftrag die aktuell verfügbare Standardlaufzeit verwendet. Es werden regelmäßig neue Versionen der U-SQL-Laufzeit veröffentlicht, die sowohl kleinere Updates als auch Sicherheitsfixes enthalten.

Sie können auch eine benutzerdefinierte Laufzeitversion auswählen. Der Grund dafür kann sein, dass Sie ein neues Update ausprobieren möchten, bei einer älteren Version einer Laufzeit verbleiben müssen oder Ihnen ein Hotfix für ein gemeldetes Problem bereitgestellt wurde, bei dem Sie nicht auf das reguläre neue Update warten können.

> [!CAUTION]
> Wenn Sie eine andere Laufzeit als die Standardversion auswählen, können Ihre U-SQL-Aufträge möglicherweise unterbrochen werden. Verwenden Sie diese anderen Versionen nur zu Testzwecken.

In seltenen Fällen kann der Microsoft-Support eine andere Version einer Laufzeit als Standardversion für Ihr Konto anheften. Stellen Sie sicher, dass Sie diese Anheftung so schnell wie möglich zurücksetzen. Wenn diese Version angeheftet bleibt, läuft sie zu einem späteren Zeitpunkt ab.

### <a name="monitoring-your-jobs-u-sql-runtime-version"></a>Überwachen der U-SQL-Laufzeitversion Ihrer Aufträge

Sie können den Verlauf der Laufzeitversionen, die von Ihren bisherigen Aufträgen verwendet wurden, im Auftragsverlauf Ihres Kontos über den Auftragsbrowser von Visual Studio oder den Auftragsverlauf im Azure-Portal anzeigen.

1. Gehen Sie im Azure-Portal zu Ihrem Data Lake Analytics-Konto.
2. Wählen Sie **Alle Aufträge anzeigen** aus. Eine Liste mit allen aktiven und kürzlich abgeschlossenen Aufträge im Konto wird angezeigt.
3. Klicken Sie optional auf **Filtern**, um die Aufträge nach den Werten **Zeitbereich**, **Auftragsname** und **Ersteller** zu suchen.
4. Die Laufzeit, die bei den abgeschlossenen Aufträgen verwendet wurde, wird angezeigt.

![Anzeigen der Laufzeitversion eines früheren Auftrags](./media/runtime-troubleshoot/prior-job-usql-runtime-version-.png)

Die verfügbaren Laufzeitversionen ändern sich mit der Zeit. Die Standardlaufzeit wird immer als „Standard“ bezeichnet. Es bleibt mindestens die vorherige Laufzeit für einige Zeit verfügbar. Außerdem werden aus einer Reihe von Gründen auch spezielle Laufzeiten zur Verfügung gestellt. Explizit benannte Laufzeiten weisen im Allgemeinen das folgende Format auf (Kursivschrift wird für variable Teile und [] für optionale Teile verwendet):

release_YYYYMMDD_adl_buildno[_modifier]

Beispielsweise bezeichnet „release_20190318_adl_3394512_2“ die zweite Version des Builds 3394512 des Laufzeitrelease vom 18. März 2019 and „release_20190318_adl_3394512_private“ bezeichnet einen privaten Build desselben Release. Hinweis: Das Datum bezieht sich auf den Zeitpunkt, zu dem der letzte Eincheckvorgang für dieses Release erfolgte, und nicht unbedingt auf das offizielle Veröffentlichungsdatum.


## <a name="troubleshooting-u-sql-runtime-version-issues"></a>Behandeln von Problemen mit der U-SQL-Laufzeitversion

Es können zwei Probleme mit der Laufzeitversion auftreten:

1. Ein Skript oder ein Benutzercode ändert das Verhalten von einer Version zur nächsten. Solche wichtigen Änderungen werden normalerweise im Voraus durch die Veröffentlichung von Versionshinweisen mitgeteilt. Wenn Sie eine solche wichtige Änderung feststellen, melden Sie dieses geänderte Verhalten dem Microsoft-Support (falls noch nicht dokumentiert), und übergeben Sie Ihre Aufträge für die ältere Runtimeversion.

2. Sie haben eine nicht standardmäßige Laufzeit entweder explizit oder implizit verwendet, als diese an Ihr Konto angeheftet wurde, und diese Laufzeit wurde nach einiger Zeit entfernt. Wenn Runtimes fehlen, aktualisieren Sie Ihre Skripts so, dass sie mit der aktuellen Standardlaufzeit ausgeführt werden. Wenn Sie zusätzliche Zeit benötigen, wenden Sie sich an den Microsoft-Support.

## <a name="known-issues"></a>Bekannte Probleme

1. Das Verweisen auf die Datei „Newtonsoft.Json“ ab Version 12.0.3 in einem USQL-Skript führt zu folgendem Kompilierungsfehler:

    *„In Ihrem Data Lake Analytics-Konto ausgeführte Aufträge werden wahrscheinlich langsamer ausgeführt oder können nicht erfolgreich abgeschlossen werden. Ein unerwartetes Problem verhindert eine automatische Wiederherstellung dieser Funktionalität für Ihr Azure Data Lake Analytics-Konto. Die Azure Data Lake-Techniker wurden informiert, um das Problem zu untersuchen.“*  

    Die Aufrufliste enthält Folgendes:  
    `System.IndexOutOfRangeException: Index was outside the bounds of the array.`  
    `at Roslyn.Compilers.MetadataReader.PEFile.CustomAttributeTableReader.get_Item(UInt32 rowId)`  
    `...`

    **Lösung**: Verwenden Sie höchstens Version 12.0.2 der Datei „Newtonsoft.Json“.
2. Kunden sehen möglicherweise temporäre Dateien und Ordner in ihrem Speicher. Diese werden im Rahmen der normalen Auftragsausführung erstellt, aber normalerweise gelöscht, bevor sie von den Kunden gesehen werden. Unter bestimmten Umständen, die selten und zufällig sind, bleiben sie möglicherweise für einen bestimmten Zeitraum sichtbar. Sie werden schließlich gelöscht und nie als Teil des Benutzerspeichers gezählt und generieren keinerlei Gebühren. Abhängig von der Auftragslogik des Kunden kann dies zu Problemen führen. Wenn der Auftrag z. B. alle Dateien im Ordner aufzählt und dann Dateilisten vergleicht, kann dies möglicherweise fehlschlagen, weil unerwartete temporäre Dateien vorhanden sind. Wenn ein Downstreamauftrag für die weitere Verarbeitung alle Dateien aus einem bestimmten Ordner aufzählt, zählt er möglicherweise auch die temporären Dateien auf.  

    **Lösung**: Eine Korrektur wird in der Laufzeit identifiziert, in der die temporären Dateien in einem temporären Ordner auf Kontoebene gespeichert werden und nicht im aktuellen Ausgabeordner. Die temporären Dateien werden in diesen neuen temporären Ordner geschrieben und am Ende der Auftragsausführung gelöscht.  
    Da diese Korrektur die Kundendaten verarbeitet, ist es äußerst wichtig, diese Korrektur in MSFT vor der Veröffentlichung zu validieren. Es wird davon ausgegangen, dass diese Korrektur in der Mitte des Jahres 2021 als Beta-Laufzeit und in der zweiten Hälfte des Jahres 2021 als Standardlaufzeit verfügbar ist. 


## <a name="see-also"></a>Weitere Informationen

- [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)
- [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md)
- [Überwachen von Aufträgen in Azure Data Lake Analytics über das Azure-Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
