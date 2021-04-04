---
title: Durchführen des Metrics Advisor-Onboardings für Ihren Datenfeed
titleSuffix: Azure Cognitive Services
description: Enthält eine Beschreibung der ersten Schritte beim Durchführen des Metrics Advisor-Onboardings für Ihre Datenfeeds.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/14/2020
ms.author: mbullwin
ms.openlocfilehash: fe3b87c733f54d8bd52c4d973977e3c8cbfefe19
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "92043200"
---
# <a name="how-to-onboard-your-metric-data-to-metrics-advisor"></a>Gewusst wie: Durchführen des Metrics Advisor-Onboardings für Ihre Metrikdaten

Informieren Sie sich in diesem Artikel darüber, wie Sie das Metrics Advisor-Onboarding für Ihre Daten durchführen. 

## <a name="data-schema-requirements-and-configuration"></a>Datenschemaanforderungen und -konfiguration

[!INCLUDE [data schema requirements](../includes/data-schema-requirements.md)]

## <a name="avoid-loading-partial-data"></a>Vermeiden des Ladens von partiellen Daten

Partielle Daten werden durch Inkonsistenzen zwischen den in Metrics Advisor gespeicherten Daten und der Datenquelle verursacht. Dies kann passieren, wenn die Datenquelle aktualisiert wird, nachdem Metrics Advisor das Pullen der Daten abgeschlossen hat. Bei Metrics Advisor werden Daten nur einmal aus einer bestimmten Datenquelle gepullt.

Ein Beispiel hierfür ist der Fall, in dem für eine Metrik das Metrics Advisor-Onboarding aus Überwachungsgründen durchgeführt wurde. Metrics Advisor ruft die Metrikdaten erfolgreich an Zeitstempel A ab und führt dafür eine Anomalieerkennung durch. Falls die Metrikdaten dieses Zeitstempels A nach der Erfassung der Daten aber aktualisiert wurden, gilt Folgendes: Es wird kein neuer Datenwert abgerufen.

Sie können versuchen, einen [Abgleich](manage-data-feeds.md#backfill-your-data-feed) für die Verlaufsdaten durchzuführen (weiter unten beschrieben), um Inkonsistenzen zu beseitigen. Dies führt aber nicht zur Auslösung neuer Anomaliewarnungen, falls bereits Warnungen für diese Zeitpunkte ausgelöst wurden. Bei diesem Prozess kommt es im System ggf. zu einer Erhöhung der Workload, und er wird nicht automatisch durchgeführt.

Wir empfehlen Ihnen zwei Ansätze, um das Laden von partiellen Daten zu vermeiden:

* Generieren von Daten in einer Transaktion:

    Stellen Sie sicher, dass die Metrikwerte für alle Dimensionskombinationen desselben Zeitstempels mit nur einer Transaktion in der Datenquelle gespeichert werden. Warten Sie beim obigen Beispiel, bis Daten aus allen Datenquellen bereit sind, und laden Sie sie dann mit nur einer Transaktion in Metrics Advisor. Mit Metrics Advisor kann der Datenfeed regelmäßig abgefragt werden, bis die Daten vollständig (bzw. partiell) erfolgreich abgerufen wurden.

* Verzögern Sie die Datenerfassung, indem Sie einen geeigneten Wert für den Parameter **Ingestion time offset** (Erfassungszeit-Offset) festlegen:

    Legen Sie den Parameter **Ingestion time offset** (Erfassungszeit-Offset) für Ihren Datenfeed fest, um die Erfassung zu verzögern, bis die Aufbereitung der Daten vollständig abgeschlossen ist. Dies kann für einige Datenquellen hilfreich sein, die keine Transaktionen unterstützen, z. B. Azure Table Storage. Weitere Informationen finden Sie unter [Erweiterte Einstellungen](manage-data-feeds.md#advanced-settings).

## <a name="add-a-data-feed-using-the-web-based-workspace"></a>Hinzufügen eines Datenfeeds mit dem webbasierten Arbeitsbereich

Klicken Sie auf **Erste Schritte**, nachdem Sie sich an Ihrem Metrics Advisor-Portal angemeldet und Ihren Arbeitsbereich ausgewählt haben. Klicken Sie anschließend auf der Hauptseite des Arbeitsbereichs im linken Menü auf **Add data feed** (Datenfeed hinzufügen).

### <a name="add-connection-settings"></a>Hinzufügen von Verbindungseinstellungen

Als Nächstes geben Sie die Parameter für die Verbindungsherstellung mit Ihrer Zeitreihen-Datenquelle ein. 
* **Quelltyp**: Der Typ der Datenquelle, auf der Ihre Zeitreihendaten gespeichert werden.
* **Granularität**: Das Intervall zwischen aufeinanderfolgenden Datenpunkten in Ihren Zeitreihendaten. Derzeit wird von Metrics Advisor Folgendes unterstützt: „Jährlich“, „Monatlich“, „Wöchentlich“, „Täglich“, „Stündlich“ und „Benutzerdefiniert“. Das kürzeste Intervall, das von der Anpassungsoption unterstützt wird, beträgt 60 Sekunden.
  * **Sekunden**: Gibt die Anzahl von Sekunden an, wenn *granularityName* auf *Customize* (Anpassen) festgelegt ist.
* **Ingest data since (UTC)** (Daten erfassen seit (UTC)): Die Baselinestartzeit für die Datenerfassung. *startOffsetInSeconds* wird häufig zum Hinzufügen eines Versatzwerts verwendet, um die Datenkonsistenz zu fördern.

Als Nächstes müssen Sie die Verbindungsinformationen für die Datenquelle und die benutzerdefinierten Abfragen angeben, mit denen die Daten in das erforderliche Schema konvertiert werden. Ausführliche Informationen zu den anderen Feldern und zum Verbinden unterschiedlicher Arten von Datenquellen finden Sie unter [Hinzufügen von Datenfeeds aus verschiedenen Datenquellen](../data-feeds-from-different-sources.md).

[!INCLUDE [query requirements](../includes/query-requirements.md)]

### <a name="verify-and-get-schema"></a>Überprüfen und Abrufen des Schemas

Nachdem die Verbindungs- und Abfragezeichenfolge festgelegt wurde, sollten Sie die Option **Verify and get schema** (Schema überprüfen und abrufen) auswählen, um die Verbindung zu überprüfen und die Abfrage auszuführen, mit der Ihr Datenschema aus der Datenquelle abgerufen wird. Je nach Datenquellenverbindung dauert dies normalerweise einige Sekunden. Stellen Sie Folgendes sicher, falls bei diesem Schritt ein Fehler auftritt:

* Ihre Verbindungszeichenfolge und die Abfrage sind korrekt.
* Mit Ihrer Metrics Advisor-Instanz kann eine Verbindung mit der Datenquelle hergestellt werden, falls Firewalleinstellungen vorhanden sind.

### <a name="schema-configuration"></a>Schemakonfiguration

Wählen Sie die entsprechenden Felder aus, nachdem das Datenschema geladen wurde.

Wenn der Zeitstempel eines Datenpunkts weggelassen wurde, wird von Metrics Advisor stattdessen der Zeitstempel verwendet, zu dem der Datenpunkt erfasst wurde. Sie können für jeden Datenfeed maximal eine Spalte als Zeitstempel angeben. Gehen Sie wie folgt vor, falls Sie eine Meldung erhalten, dass eine Spalte nicht als Zeitstempel angegeben werden kann: Überprüfen Sie Ihre Abfrage bzw. Datenquelle und das Vorhandensein mehrerer Zeitstempel im Abfrageergebnis (nicht nur in den Vorschaudaten). Beim Durchführen der Datenerfassung kann von Metrics Advisor für die jeweilige Quelle nur jeweils ein Teil (z. B. ein Tag oder eine Stunde, je nach Granularität) der Zeitreihendaten genutzt werden.

|Auswahl  |BESCHREIBUNG  |Hinweise  |
|---------|---------|---------|
| **Anzeigename** | Name, der in Ihrem Arbeitsbereich anstelle des ursprünglichen Spaltennamens angezeigt werden soll. | |
|**Timestamp**     | Der Zeitstempel eines Datenpunkts. Wenn kein Wert angegeben ist, wird von Metrics Advisor stattdessen der Zeitstempel der Datenpunkterfassung verwendet. Sie können für jeden Datenfeed maximal eine Spalte als Zeitstempel angeben.        | Dies ist optional. Es sollte maximal eine Spalte angegeben werden. Falls ein Fehler der Art **Spalte kann nicht als Zeitstempel angegeben werden** angezeigt wird, sollten Sie Ihre Abfrage oder Datenquelle auf doppelte Zeitstempel überprüfen.      |
|**Measure**     |  Die numerischen Werte im Datenfeed. Sie können für jeden Datenfeed mehrere Measures angeben, aber mindestens eine Spalte sollte als Measure ausgewählt werden.        | Es sollte mindestens eine Spalte angegeben werden.        |
|**Dimension**     | Enthält Kategoriewerte. Mit einer Kombination unterschiedlicher Wert wird eine bestimmte Zeitreihe mit nur einer Dimension identifiziert, z. B. Land, Sprache, Mandant. Sie können null oder mehr Spalten als Dimensionen auswählen. Hinweis: Gehen Sie mit Bedacht vor, wenn Sie eine Spalte mit einem anderen Typ als „Zeichenfolge“ als Dimension auswählen. | Dies ist optional.        |
|**Ignorieren**     | Die ausgewählte Spalte wird ignoriert.        | Dies ist optional. Siehe folgenden Text.       |

Falls Sie Spalten ignorieren möchten, empfehlen wir Ihnen, Ihre Abfrage oder Datenquelle zu aktualisieren, um diese Spalten auszuschließen. Sie können Spalten auch ignorieren, indem Sie die Option **Ignore columns** (Spalten ignorieren) und dann **Ignore** (Ignorieren) für die jeweilige Spalte verwenden. Wenn eine Spalte als Dimension genutzt werden soll und fälschlicherweise auf *Ignoriert* festgelegt wurde, werden von Metrics Advisor unter Umständen partielle Daten erfasst. Nehmen Sie beispielsweise an, dass Ihre Abfrage über die folgenden Daten verfügt:

| Zeilen-ID | Timestamp | Country | Sprache | Income |
| --- | --- | --- | --- | --- |
| 1 | 10.11.2019 | China | ZH-CN | 10000 |
| 2 | 10.11.2019 | China | EN-US | 1000 |
| 3 | 10.11.2019 | US | ZH-CN | 12000 |
| 4 | 11.11.2019 | US | EN-US | 23000 |
| ... | ...| ... | ... | ... |

Wenn *Land/Region* eine Dimension ist und *Sprache* auf *Ignoriert* festgelegt ist, verfügen die erste und die zweite Spalte über die gleichen Dimensionen. Metrics Advisor wählt willkürlich einen Wert aus diesen beiden Zeilen aus. In diesem Fall werden die Zeilen von Metrics Advisor nicht aggregiert.

### <a name="automatic-roll-up-settings"></a>Einstellungen für automatisches Rollup

> [!IMPORTANT]
> Wenn Sie die Fehlerursachenanalyse und andere Diagnosefunktionen aktivieren möchten, müssen die **Einstellungen für automatisches Rollup** konfiguriert werden. Nach der Aktivierung können die Einstellungen für automatisches Rollup nicht mehr geändert werden.

Metrics Advisor kann während der Erfassung für jede Dimension automatisch eine Aggregation durchführen (z. B. SUM, MAX, MIN). Anschließend wird eine Hierarchie erstellt, die für die Fehlerursachenanalyse und andere Diagnosefeatures verwendet wird. 

Stellen Sie sich die folgenden Szenarien vor:

* *Ich muss die Rollupanalyse für meine Daten nicht einbinden.*

    Sie müssen das Metrics Advisor-Rollup nicht nutzen.

* *Für meine Daten wurde das Rollup bereits ausgeführt, und der Dimensionswert wird wie folgt dargestellt: NULL oder leer (Standardeinstellung), Nur NULL, Sonstiges.*

    Diese Option bedeutet, dass von Metrics Advisor kein Rollup für die Daten ausgeführt werden muss, weil die Zeilen bereits summiert wurden. Wenn Sie beispielsweise *Nur NULL* auswählen, wird die zweite Datenzeile im Beispiel unten als Aggregation aller Länder und der Sprache *EN-US* angesehen. Die vierte Datenzeile, die für *Land/Region* leer ist, wird dagegen als reguläre Zeile angesehen, und dies kann ein Hinweis auf unvollständige Daten sein.
    
    | Country | Sprache | Income |
    |---------|----------|--------|
    | China   | ZH-CN    | 10000  |
    | (NULL)  | EN-US    | 999.999 |
    | US      | EN-US    | 12000  |
    |         | EN-US    | 5.000   |

* *Mit Metrics Advisor soll ein Rollup für meine Daten ausgeführt werden, indem Sum/Max/Min/Avg/Count berechnet wird, und dies soll mit <some string> dargestellt werden*

    Für einige Datenquellen, z. B. Cosmos DB oder Azure Blob Storage, werden bestimmte Berechnungen wie *Gruppieren nach* oder *Dritte Potenz* nicht unterstützt. Die Rollupoption wird von Metrics Advisor bereitgestellt, um während der Erfassung automatisch einen Datencube zu generieren.
    Diese Option bedeutet, dass das Rollup von Metrics Advisor mit dem von Ihnen ausgewählten Algorithmus berechnet und die angegebene Zeichenfolge zum Darstellen des Rollups in Metrics Advisor verwendet werden soll. Hierbei werden auf Ihrer Datenquelle keine Daten geändert.
    Nehmen Sie beispielsweise an, dass Sie über einige Zeitreihen verfügen, die für „Umsatzmetriken mit der Dimension (Land, Region)“ stehen. Dies kann für einen Zeitstempel ggf. wie folgt aussehen:


    | Country       | Region           | Sales |
    |---------------|------------------|-------|
    | Canada        | Alberta          | 100   |
    | Canada        | British Columbia | 500   |
    | USA | Montana          | 100   |


    Nach dem Aktivieren des automatischen Rollups mit *Sum* werden von Metrics Advisor die Dimensionskombinationen berechnet und die Metriken während der Datenerfassung summiert. Das Ergebnis kann beispielsweise wie folgt lauten:

    | Country       | Region           | Sales |
    | ------------ | --------------- | ---- |
    | Canada        | Alberta          | 100   |
    | NULL          | Alberta          | 100   |
    | Canada        | British Columbia | 500   |
    | NULL          | British Columbia | 500   |
    | USA | Montana          | 100   |
    | NULL          | Montana          | 100   |
    | NULL          | NULL             | 700   |
    | Canada        | NULL             | 600   |
    | USA | NULL             | 100   |

    `(Country=Canada, Region=NULL, Sales=600)` bedeutet, dass sich für den Umsatz in Kanada (alle Regionen) die Summe 600 ergibt.

    Hier ist die Transformation in SQL-Sprache angegeben.

    ```mssql
    SELECT
        dimension_1,
        dimension_2,
        ...
        dimension_n,
        sum (metrics_1) AS metrics_1,
        sum (metrics_2) AS metrics_2,
        ...
        sum (metrics_n) AS metrics_n
    FROM
        each_timestamp_data
    GROUP BY
        CUBE (dimension_1, dimension_2, ..., dimension_n);
    ```

    Beachten Sie Folgendes, bevor Sie das Feature für automatische Rollups verwenden:

    * Falls Sie *SUM* zum Aggregieren Ihrer Daten verwenden möchten, sollten Sie sicherstellen, dass Ihre Metriken für jede Dimension additiv sind. Hier sind einige Beispiele für *nicht additive* Metriken angegeben:
      * Auf Brüchen basierende Metriken. Beispiele hierfür sind Verhältnis, Prozentsatz usw. Sie sollten beispielsweise nicht die Arbeitslosenquote jedes Bundesstaats hinzufügen, um die Arbeitslosenquote für das gesamte Land zu berechnen.
      * Dimensionsüberlappung. Sie sollten beispielsweise nicht die Anzahl von Personen für jeden Sport hinzufügen, um die Anzahl von Personen zu berechnen, die sportinteressiert sind. Der Grund ist, dass es hierbei zu einer Überlappung kommt, weil eine Person an mehreren Sportarten interessiert sein kann.
    * Die Größe des Cubes ist begrenzt, um die Integrität des Gesamtsystems sicherzustellen. Derzeit liegt der Grenzwert bei 1.000.000. Wenn Ihre Daten diesen Grenzwert überschreiten, ist der Erfassungsvorgang für diesen Zeitstempel nicht erfolgreich.

## <a name="advanced-settings"></a>Erweiterte Einstellungen

Es gibt mehrere erweiterte Einstellungen, um die Erfassung von Daten auf angepasste Weise zu ermöglichen, z. B. Angabe eines Offsetwerts für die Erfassung oder Parallelität. Weitere Informationen finden Sie im Artikel zur Datenfeedverwaltung im Abschnitt [Erweiterte Einstellungen](manage-data-feeds.md#advanced-settings).

## <a name="specify-a-name-for-the-data-feed-and-check-the-ingestion-progress"></a>Angeben eines Namens für den Datenfeed und Überprüfen des Erfassungsstatus
 
Geben Sie einen benutzerdefinierten Namen für den Datenfeed an, der in Ihrem Arbeitsbereich angezeigt werden soll. Klicken Sie anschließend auf **Absenden**. Auf der Seite mit den Datenfeeddetails können Sie die Statusanzeige für die Erfassung verwenden, um die Statusinformationen anzuzeigen.

:::image type="content" source="../media/datafeeds/ingestion-progress.png" alt-text="Statusanzeige für die Erfassung" lightbox="../media/datafeeds/ingestion-progress.png":::


Überprüfen Sie die Erfassungsfehlerdetails wie folgt: 

1. Klicken Sie auf **Details anzeigen**.
2. Klicken Sie auf **Status**, und wählen Sie anschließend **Fehlgeschlagen** oder **Fehler** aus.
3. Zeigen Sie auf eine fehlgeschlagene Erfassung, und sehen Sie sich die angezeigte Meldung mit den Details an.

:::image type="content" source="../media/datafeeds/check-failed-ingestion.png" alt-text="Überprüfen der fehlgeschlagenen Erfassung":::

Mit dem Status *Fehlgeschlagen* wird angegeben, dass der Erfassungsvorgang für diese Datenquelle zu einem späteren Zeitpunkt wiederholt wird.
Beim Status *Fehler* wird von Metrics Advisor für die Datenquelle kein weiterer Versuch durchgeführt. Um die Daten erneut zu laden, müssen Sie manuell einen Abgleich bzw. den erneuten Ladevorgang auslösen.

Sie können den Status einer Erfassung auch erneut laden, indem Sie auf **Refresh Progress** (Status aktualisieren) klicken. Nach Abschluss der Datenerfassung können Sie auf die Metriken klicken und die Ergebnisse der Anomalieerkennung überprüfen.

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten Ihrer Datenfeeds](manage-data-feeds.md)
- [Konfigurationen für unterschiedliche Datenquellen](../data-feeds-from-different-sources.md)
- [Konfigurieren von Metriken und Optimieren der Konfigurationserkennung](configure-metrics.md)
