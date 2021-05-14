---
title: Leistungsbenchmarks
titleSuffix: Azure Cognitive Search
description: Erfahren Sie mehr über die Leistung von Azure Cognitive Search anhand verschiedener Leistungsvergleichstests
author: dereklegenzoff
manager: luisca
ms.author: delegenz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: 5f7617fdfea68c8005c8a33ec782edc2bbe01f4b
ms.sourcegitcommit: bd1a4e4df613ff24e954eb3876aebff533b317ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2021
ms.locfileid: "107930771"
---
# <a name="azure-cognitive-search-performance-benchmarks"></a>Azure Cognitive Search-Leistungsvergleichstests

Die Leistung von Azure Cognitive Search hängt von einer [Vielzahl von Faktoren](search-performance-tips.md) ab, beispielsweise der Größe Ihres Suchdiensts und den Abfragetypen, die Sie senden. Um abzuschätzen, wie groß der Suchdienst für Ihre Workload sein muss, wurden mehrere Vergleichstests zum Dokumenten der Leistung für verschiedene Suchdienste und Konfigurationen durchgeführt. Diese Vergleichstests garantieren in keiner Weise ein bestimmtes Leistungsniveau Ihres Diensts, können Ihnen aber eine Vorstellung von der zu erwartenden Leistung geben.

Um eine Reihe verschiedener Anwendungsfälle abzudecken, haben wir Vergleichstests für zwei Hauptszenarien verwendet:

* **E-Commerce-Suche**: Dieser Vergleichstests emuliert ein echtes E-Commerce-Szenario und basiert auf dem skandinavischen E-Commerce-Unternehmen [CDON.](https://cdon.com)
* **Dokumentsuche**: Dieses Szenario besteht aus der Schlüsselwortsuche über Volltextdokumente von [Semantic Scholar](http://s2-public-api-prod.us-west-2.elasticbeanstalk.com/corpus/download/). Es emuliert eine typische Lösung für die Dokumentsuche.

Obwohl diese Szenarien unterschiedliche Anwendungsfälle widerspiegeln, ist jedes Szenario anders, daher empfehlen wir immer, Leistungstests für Ihre individuelle Workload durchzuführen. Wir haben eine [Leistungstestlösung mit JMeter](https://github.com/Azure-Samples/azure-search-performance-testing) veröffentlicht, damit Sie ähnliche Tests für Ihren eigenen Dienst ausführen können.

## <a name="testing-methodology"></a>Testmethodik

Um die Leistung von Azure Cognitive Search zu messen, haben wir Tests für zwei verschiedene Szenarien auf verschiedenen Ebenen und Replikat/Partition-Kombinationen durchgeführt.

Zum Erstellen dieser Vergleichstests wurde die folgende Methodik verwendet:

1. Der Test beginnt bei `X` Abfragen pro Sekunde (QPS) für 180 Sekunden. Dies waren in der Regel 5 oder 10 QPS.
2. QPS wurde dann um `X` erhöht und für weitere 180 Sekunden ausgeführt.
3. Alle 180 Sekunden wurde der Test um `X` QPS erhöht, bis die durchschnittliche Wartezeit über 1.000 ms bzw. weniger als 99 % erfolgreiche Abfragen anstieg.

Das folgende Diagramm zeigt ein visuelles Beispiel für die Abfragelast des Tests:

![Beispieltest](./media/performance-benchmarks/example-test.png)

In jedem Szenario wurden mindestens 10.000 eindeutige Abfragen verwendet, um eine zu starke Verzerrung der Tests durch Zwischenspeichern zu vermeiden.

> [!IMPORTANT]
> Diese Tests umfassen nur Abfrageworkloads. Wenn Sie ein hohes Volumen an Indizierungsvorgängen erwarten, sollten Sie dies bei Ihren Schätzungen und Leistungstests berücksichtigen. Beispielcode zum Simulieren der Indizierung finden Sie in diesem [Tutorial](tutorial-optimize-indexing-push-api.md).

### <a name="definitions"></a>Definitionen

- **Maximale QPS** – Die unten angegebenen Zahlen für die maximale Anzahl von QPS basieren auf den höchsten QPS-Werten, die in einem Test erreicht wurden, bei dem 99  % der Abfragen ohne Drosselung erfolgreich abgeschlossen wurden und die durchschnittliche Wartezeit unter 1000 ms blieb.

- **Prozentanteil der max. QPS** – Prozentanteil der maximalen Anzahl von QPS, die für einen bestimmten Test erreicht wurden. Wenn ein bestimmter Test beispielsweise einen Maximalwert von 100 QPS erreicht hat, sind 20 % der maximalen QPS 20 QPS.

- **Wartezeit** – Wartezeit des Servers für eine Abfrage. In diesen ist die [Roundtripverzögerung (RTT)](https://en.wikipedia.org/wiki/Round-trip_delay) nicht enthalten. Die Werte unten sind in Millisekunden (ms) angegeben.

### <a name="disclaimer"></a>Haftungsausschluss

Der Code, den wir zum Ausführen dieser Vergleichstests verwendet haben, ist [hier](https://github.com/Azure-Samples/azure-search-performance-testing/tree/main/other_tools) verfügbar. Beachten Sie, dass wir mit der [JMeter-Leistungstestlösung](https://github.com/Azure-Samples/azure-search-performance-testing) etwas niedrigere QPS-Ebenen festgestellt haben als in den unten angegebenen Vergleichstests. Die Unterschiede können auf die unterschiedliche Art der Tests zurückzuführen sein. Dies zeigt, wie wichtig es ist, dass Ihre Leistungstests der Produktionsworkload so ähnlich wie möglich sind.

Diese Vergleichstests garantieren in keiner Weise ein bestimmtes Leistungsniveau Ihres Diensts, können Ihnen aber eine Vorstellung von der zu erwartenden Leistung basierend auf Ihrem Szenario geben.

Bei Fragen oder Bedenken können Sie sich gerne unter azuresearch_contact@microsoft.com an uns wenden.

## <a name="benchmark-1-e-commerce-search"></a>Vergleichstest 1: E-Commerce-Suche

:::row:::
   :::column span="1":::
      ![CDON-Logo](./media/performance-benchmarks/cdon-logo-160px2.png)
   :::column-end:::
   :::column span="3":::
      Dieser Vergleichstest wurde in Zusammenarbeit mit dem E-Commerce-Unternehmen [CDON](https://cdon.com) erstellt, dem größten Onlinemarktplatz Skandinaviens, mit Niederlassungen in Schweden, Finnland, Norwegen und Dänemark. Durch seine 1.500 Einzelhändler bietet CDON ein breites Spektrum von mehr als 8 Millionen Produkten. Im Jahr 2020 hatte CDON über 120 Millionen Besucher und 2 Millionen aktive Kunden. Weitere Informationen zur Verwendung von Azure Cognitive Search durch CDON finden Sie [in diesem Artikel](https://pulse.microsoft.com/transform/na/fa1-how-cdon-has-been-using-technology-to-become-the-leading-marketplace-in-the-nordics/).
   :::column-end:::
:::row-end:::

Zum Ausführen dieser Tests haben wir eine Momentaufnahme des CDON-Produktionssuchindexes und Tausende eindeutiger Abfragen von ihrer [Website](https://cdon.com) verwendet.

### <a name="scenario-details"></a>Szenariodetails

- **Dokumentanzahl**: 6.000.000 
- **Indexgröße**: 20 GB
- **Indexschema**: Ein breiter Index mit insgesamt 250 Feldern, 25 durchsuchbaren und 200 filterbaren/facettierbaren Feldern
- **Abfragetypen**: Volltextsuchabfragen einschließlich Facetten, Filtern, Sortierung und Bewertungsprofilen

### <a name="s1-performance"></a>S1-Leistung

#### <a name="queries-per-second"></a>Abfragen pro Sekunde

Das folgende Diagramm zeigt die höchste Abfragelast in Bezug auf Abfragen pro Sekunde (QPS), die ein Dienst über einen längeren Zeitraum verarbeiten konnte.

![Höchste beizubehaltender QPS-Wert, E-Commerce S1](./media/performance-benchmarks/s1-ecom-qps.png)

#### <a name="query-latency"></a>Abfragewartezeit

Die Abfragewartezeit variiert je nach Auslastung des Diensts, und Dienste mit höherer Belastung haben eine höhere durchschnittliche Abfragewartezeit. Die folgende Tabelle zeigt das 25., 50., 75., 90., 95. und 99. Perzentil der Abfragewartezeit für drei verschiedene Nutzungsebenen.

| Prozentanteil der max. QPS  | Durchschnittliche Latenz | 25 % | 75 % | 90% | 95 % | 99 %|
|---|---|---|---| --- | --- | --- | 
| 20%  | 104 ms  | 35 ms  | 115 ms   | 177 ms | 257 ms | 738 ms |
| 50%  | 140 ms  | 47 ms  | 144 ms   | 241 ms | 400 ms | 1175 ms |
| 80 %  | 239 ms  | 77 ms  | 248 ms   | 466 ms | 763 ms | 1752 ms | 


### <a name="s2-performance"></a>S2-Leistung

#### <a name="queries-per-second"></a>Abfragen pro Sekunde

Das folgende Diagramm zeigt die höchste Abfragelast in Bezug auf Abfragen pro Sekunde (QPS), die ein Dienst über einen längeren Zeitraum verarbeiten konnte.

![Höchste beizubehaltender QPS-Wert, E-Commerce S2](./media/performance-benchmarks/s2-ecom-qps.png)

#### <a name="query-latency"></a>Abfragewartezeit

Die Abfragewartezeit variiert je nach Auslastung des Diensts, und Dienste mit höherer Belastung haben eine höhere durchschnittliche Abfragewartezeit. Die folgende Tabelle zeigt das 25., 50., 75., 90., 95. und 99. Perzentil der Abfragewartezeit für drei verschiedene Nutzungsebenen.

| Prozentanteil der max. QPS  | Durchschnittliche Latenz | 25 % | 75 % | 90% | 95 % | 99 %|
|---|---|---|---| --- | --- | --- | 
| 20%  | 56 ms | 21 ms | 68 ms  | 106 ms  | 132 ms | 210 ms | 
| 50%  | 71 ms  | 26 ms  | 83 ms   | 132 ms | 177 ms | 329 ms |
| 80 %  | 140 ms  | 47 ms  | 153 ms   | 293 ms | 452 ms | 924 ms | 

### <a name="s3-performance"></a>S3-Leistung

#### <a name="queries-per-second"></a>Abfragen pro Sekunde

Das folgende Diagramm zeigt die höchste Abfragelast in Bezug auf Abfragen pro Sekunde (QPS), die ein Dienst über einen längeren Zeitraum verarbeiten konnte.

![Höchste beizubehaltender QPS-Wert, E-Commerce S3](./media/performance-benchmarks/s3-ecom-qps.png)

In diesem Fall erkennen wir, dass das Hinzufügen einer zweiten Partition die maximale QPS signifikant erhöht, das Hinzufügen einer dritten Partition aber abnehmende Grenzerträge liefert. Die geringere Verbesserung liegt wahrscheinlich darin begründet, dass bereits mit nur zwei Partitionen alle Daten in den aktiven Speicher von S3 gezogen werden.

#### <a name="query-latency"></a>Abfragewartezeit

Die Abfragewartezeit variiert je nach Auslastung des Diensts, und Dienste mit höherer Belastung haben eine höhere durchschnittliche Abfragewartezeit. Die folgende Tabelle zeigt das 25., 50., 75., 90., 95. und 99. Perzentil der Abfragewartezeit für drei verschiedene Nutzungsebenen.

| Prozentanteil der max. QPS  | Durchschnittliche Latenz | 25 % | 75 % | 90% | 95 % | 99 %|
|---|---|---|---| --- | --- | --- |
| 20%  | 50 ms  | 20 ms  | 64 ms   | 83 ms | 98 ms | 160 ms |
| 50%  | 62 ms  | 24 ms  | 80 ms   | 107 ms | 130 ms | 253 ms |
| 80 %  | 115 ms  | 38 ms  | 121 ms   | 218 ms | 352 ms | 828 ms |

## <a name="benchmark-2-document-search"></a>Vergleichstest 2: Dokumentsuche

### <a name="scenario-details"></a>Szenariodetails

- **Dokumentanzahl**: 7,5 Millionen
- **Indexgröße**: 22 GB
- **Indexschema**: 23 Felder; 8 durchsuchbar, 10 filterbar/facettierbar
- **Abfragetypen**: Schlüsselwortsuchen mit Facetten und Treffermarkierung

### <a name="s1-performance"></a>S1-Leistung

#### <a name="queries-per-second"></a>Abfragen pro Sekunde

Das folgende Diagramm zeigt die höchste Abfragelast in Bezug auf Abfragen pro Sekunde (QPS), die ein Dienst über einen längeren Zeitraum verarbeiten konnte.

![Höchste beizubehaltender QPS-Wert, Dokumentationssuche S1](./media/performance-benchmarks/s1-docsearch-qps.png)

#### <a name="query-latency"></a>Abfragewartezeit

Die Abfragewartezeit variiert je nach Auslastung des Diensts, und Dienste mit höherer Belastung haben eine höhere durchschnittliche Abfragewartezeit. Die folgende Tabelle zeigt das 25., 50., 75., 90., 95. und 99. Perzentil der Abfragewartezeit für drei verschiedene Nutzungsebenen.

| Prozentanteil der max. QPS  | Durchschnittliche Latenz | 25 % | 75 % | 90% | 95 % | 99 %|
|---|---|---|---| --- | --- | --- |
| 20%  | 67 ms  | 44 ms  | 77 ms   | 103 ms | 126 ms | 216 ms |
| 50%  | 93 ms  | 59 ms  | 110 ms   | 150 ms | 184 ms | 304 ms |
| 80 %  | 150 ms  | 96 ms  | 184 ms   | 248 ms | 297 ms | 424 ms |

### <a name="s2-performance"></a>S2-Leistung

#### <a name="queries-per-second"></a>Abfragen pro Sekunde

Das folgende Diagramm zeigt die höchste Abfragelast in Bezug auf Abfragen pro Sekunde (QPS), die ein Dienst über einen längeren Zeitraum verarbeiten konnte.

![Höchste beizubehaltender QPS-Wert, Dokumentationssuche S2](./media/performance-benchmarks/s2-docsearch-qps.png)

#### <a name="query-latency"></a>Abfragewartezeit

Die Abfragewartezeit variiert je nach Auslastung des Diensts, und Dienste mit höherer Belastung haben eine höhere durchschnittliche Abfragewartezeit. Die folgende Tabelle zeigt das 25., 50., 75., 90., 95. und 99. Perzentil der Abfragewartezeit für drei verschiedene Nutzungsebenen.

| Prozentanteil der max. QPS  | Durchschnittliche Latenz | 25 % | 75 % | 90% | 95 % | 99 %|
|---|---|---|---| --- | --- | --- |
| 20%  | 45 ms  | 31 ms  | 55 ms   | 73 ms | 84 ms | 109 ms |
| 50%  | 63 ms  | 39 ms  | 81 ms   | 106 ms | 123 ms | 163 ms |
| 80 %  | 115 ms  | 73 ms  | 145 ms   | 191 ms | 224 ms | 291 ms |

### <a name="s3-performance"></a>S3-Leistung

#### <a name="queries-per-second"></a>Abfragen pro Sekunde

Das folgende Diagramm zeigt die höchste Abfragelast in Bezug auf Abfragen pro Sekunde (QPS), die ein Dienst über einen längeren Zeitraum verarbeiten konnte.

![Höchster verarbeitbarer QPS-Wert, Dokumentationssuche S3](./media/performance-benchmarks/s3-docsearch-qps.png)

#### <a name="query-latency"></a>Abfragewartezeit

Die Abfragewartezeit variiert je nach Auslastung des Diensts, und Dienste mit höherer Belastung haben eine höhere durchschnittliche Abfragewartezeit. Die folgende Tabelle zeigt das 25., 50., 75., 90., 95. und 99. Perzentil der Abfragewartezeit für drei verschiedene Nutzungsebenen.

| Prozentanteil der max. QPS  | Durchschnittliche Latenz | 25 % | 75 % | 90% | 95 % | 99 %|
|---|---|---|---| --- | --- | --- |
| 20%  | 43 ms  | 29 ms  | 53 ms   | 74 ms | 86 ms | 111 ms |
| 50%  | 65 ms  | 37 ms  | 85 ms   | 111 ms | 128 ms | 164 ms |
| 80 %  | 126 ms  | 83 ms  | 162 ms   | 205 ms | 233 ms | 281 ms |

## <a name="takeaways"></a>Wesentliche Punkte

Anhand dieser Vergleichstests können Sie sich ein Bild von der Leistung machen, die Azure Cognitive Search bietet. Sie erkennen auch die Unterschiede zwischen Diensten in verschiedenen Tarifen.

Einige wichtige Erkenntnisse, die sich aus diesen Vergleichstests ergeben, sind:

* S2 kann in der Regel mindestens das Vierfache des Abfragevolumens wie S1 verarbeiten.
* S2 weist in der Regel eine geringere Wartezeit auf als S1, bei vergleichbaren Abfragevolumina.
* Wenn Sie Replikate hinzufügen, skalieren die QPS, die ein Dienst verarbeiten kann, in der Regel linear (wenn ein Replikat z. B. 10 QPS verarbeiten kann, können fünf Replikate normalerweise 50 QPS verarbeiten).
* Je höher die Last für den Dienst ist, desto höher ist die durchschnittliche Wartezeit.

Sie erkennen außerdem, dass die Leistung zwischen Szenarien erheblich variieren kann. Wenn die erwartete Leistung nicht erreicht wird, sehen Sie sich die [Tipps für eine bessere Leistung](search-performance-tips.md) an.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun die Leistungsvergleichstests angeschaut haben, können Sie mehr darüber erfahren, wie Sie die Leistung von Cognitive Search analysieren und welche Faktoren die Leistung beeinflussen.

> [!div class="nextstepaction"]
> [Analysieren der Leistung](search-performance-analysis.md)
> [Tipps für eine bessere Leistung](search-performance-tips.md)