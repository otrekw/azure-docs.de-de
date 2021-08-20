---
title: 'Schnellstart: Metrics Advisor-Webportal'
titleSuffix: Azure Cognitive Services
description: Hier finden Sie Informationen zu den ersten Schritten mit dem Metrics Advisor-Webportal.
services: cognitive-services
author: mrbullwinkle
ms.author: mbullwin
manager: nitinme
ms.date: 09/30/2020
ms.topic: quickstart
ms.service: applied-ai-services
ms.subservice: metrics-advisor
ms.custom:
- mode-portal
ms.openlocfilehash: 324b84255bd5939940a7819c521a452c1c49c8e8
ms.sourcegitcommit: 192444210a0bd040008ef01babd140b23a95541b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114341834"
---
# <a name="quickstart-monitor-your-first-metric-using-the-web-portal"></a>Schnellstart: Überwachen Ihrer ersten Metrik mithilfe des Webportals

Wenn Sie eine Metrics Advisor-Instanz bereitstellen, können Sie den Dienst über die APIs sowie über den webbasierten Arbeitsbereich verwenden. Der webbasierte Arbeitsbereich ermöglicht einen einfachen und schnellen Einstieg in die Verwendung des Diensts. Darüber hinaus können Sie hier visuell Einstellungen konfigurieren, Ihr Modell anpassen und Ursachenanalysen durchführen. 

* Durchführen des Onboardings für Ihre Metrikdaten
* Anzeigen Ihrer Metriken und Visualisierungen
* Optimieren von Erkennungskonfigurationen
* Untersuchen von Diagnoseerkenntnissen
* Erstellen und Abonnieren von Anomaliewarnungen

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services)
* Wenn Sie über ein Azure-Abonnement verfügen, <a href="https://go.microsoft.com/fwlink/?linkid=2142156"  title="Erstellen einer Metrics Advisor-Ressource"  target="_blank">erstellen Sie eine Metrics Advisor-Ressource </a> im Azure-Portal, um Ihre Metrics Advisor-Instanz bereitzustellen.  

    
> [!TIP]
> * Die Bereitstellung Ihrer Metrics Advisor-Ressource kann zwischen zehn und 30 Minuten dauern. Wählen Sie nach der erfolgreichen Bereitstellung die Option **Zu Ressource wechseln** aus.
> * Wenn Sie über die REST-API mit dem Dienst interagieren möchten, benötigen Sie den Schlüssel und den Endpunkt der erstellten Ressource. Diese finden Sie in der erstellten Ressource auf der Registerkarte **Schlüssel und Endpunkte**.


In diesem Dokument wird eine SQL-Datenbank-Instanz als Beispiel für die Erstellung Ihres ersten Monitors verwendet.

## <a name="sign-in-to-your-workspace"></a>Anmelden in Ihrem Arbeitsbereich

Melden Sie sich nach der Erstellung Ihrer Ressource mit Ihrem Active Directory-Konto beim [Metrics Advisor-Portal](https://go.microsoft.com/fwlink/?linkid=2143774) an. Wählen Sie auf der Zielseite Ihr **Verzeichnis**, Ihr **Abonnement** und den soeben erstellten **Arbeitsbereich** und dann die Option **Erste Schritte** aus. Wählen Sie für das Onboarding von Zeitreihendaten im Menü auf der linken Seite die Option **Datenfeed hinzufügen** aus.

 
Derzeit können Sie in jeder verfügbaren Region eine Metrics Advisor-Ressource erstellen. Sie können die Arbeitsbereiche im Metrics Advisor-Portal jederzeit wechseln.


## <a name="onboard-time-series-data"></a>Durchführen des Onboardings für Zeitreihendaten

Metrics Advisor bietet Connectors für verschiedene Datenquellen wie SQL-Datenbank, Azure Data Explorer und Azure Table Storage. Die Schritte für die Verbindung von Daten sind für unterschiedliche Connectors zwar ähnlich, einige Konfigurationsparameter können jedoch variieren. Informationen zu den verschiedenen Datenverbindungseinstellungen finden Sie im Artikel zur [Verbindungsherstellung für unterschiedliche Datenfeedquellen](../data-feeds-from-different-sources.md).

In dieser Schnellstartanleitung wird als Beispiel eine SQL-Datenbank-Instanz verwendet. Mit den gleichen Schritten können Sie auch Ihre eigenen Daten erfassen.


### <a name="data-schema-requirements-and-configuration"></a>Datenschemaanforderungen und -konfiguration

[!INCLUDE [data schema requirements](../includes/data-schema-requirements.md)]

### <a name="configure-connection-settings-and-query"></a>Konfigurieren der Verbindungseinstellungen und der Abfrage

[Fügen Sie die Datenfeeds hinzu](../how-tos/onboard-your-data.md), indem Sie eine Verbindung mit Ihrer Zeitreihendatenquelle herstellen. Wählen Sie die folgenden Parameter aus:

* **Quelltyp**: Die Art der Datenquelle, in der Ihre Zeitreihendaten gespeichert sind.
* **Granularität**: Das Intervall zwischen aufeinanderfolgenden Datenpunkten in Ihren Zeitreihendaten (beispielsweise jährlich, monatlich oder täglich). Das kürzestmögliche Intervall für die Anpassung beträgt 60 Sekunden.
* **Ingest data since (UTC)** (Daten erfassen seit (UTC)): Die Startzeit für den ersten zu erfassenden Zeitstempel. 


<!-- Next, specify the **Connection string** with the credentials for your data source, and a custom **Query**, see [how to write a valid query](../tutorials/write-a-valid-query.md) for more information. -->

:::image type="content" source="../media/connection-settings.png" alt-text="Verbindungseinstellungen" lightbox="../media/connection-settings.png":::


### <a name="load-data"></a>Laden der Daten

Wählen Sie nach dem Eingeben der Verbindungs- und Abfragezeichenfolge die Option **Daten laden** aus. Bei diesem Vorgang wird von Metrics Advisor Folgendes überprüft: die Verbindung und die Berechtigung zum Laden von Daten, die erforderlichen Parameter (@IntervalStart und @IntervalEnd), die in der Abfrage verwendet werden müssen, und der Spaltenname aus der Datenquelle. 

Gehen Sie wie folgt vor, falls bei diesem Schritt ein Fehler auftritt:
1. Überprüfen Sie zunächst, ob die Verbindungszeichenfolge gültig ist. 
2. Vergewissern Sie sich anschließend, dass ausreichende Berechtigungen vorhanden sind und dass für die IP-Adresse des Erfassungsworkers der Zugriff gewährt wurde.
3. Überprüfen Sie als Nächstes, ob die erforderlichen Parameter (@IntervalStart und @IntervalEnd) in Ihrer Abfrage verwendet werden. 

### <a name="schema-configuration"></a>Schemakonfiguration

Wählen Sie die entsprechenden Felder aus, nachdem die Daten durch die Ausführung der Abfrage geladen wurden und wie unten dargestellt angezeigt werden.


|Auswahl  |Beschreibung  |Hinweise  |
|---------|---------|---------|
|**Timestamp**     | Der Zeitstempel eines Datenpunkts. Wenn kein Wert angegeben ist, wird von Metrics Advisor stattdessen der Zeitstempel der Erfassung des Datenpunkts verwendet. Pro Datenfeed kann maximal eine Spalte als Zeitstempel angegeben werden.        | Dies ist optional. Geben Sie maximal eine Spalte an.       |
|**Measure**     |  Die numerischen Werte im Datenfeed. Pro Datenfeed können mehrere Measures angegeben werden. Es muss jedoch mindestens eine Spalte als Measure ausgewählt werden.        | Geben Sie mindestens eine Spalte an.        |
|**Dimension**     | Enthält Kategoriewerte. Durch eine Kombination aus unterschiedlichen Werten wird eine bestimmte Zeitreihe mit einer einzelnen Dimension identifiziert (beispielsweise Land, Sprache, Mandant). Sie können beliebig viele Spalten (oder auch keine Spalten) als Dimensionen auswählen. Hinweis: Wenn Sie als Dimension eine Spalte auswählen, die keine Zeichenfolge enthält, müssen Sie darauf achten, dass es zu keiner Dimensionsexplosion kommt. | Dies ist optional.        |
|**Ignorieren**     | Die ausgewählte Spalte wird ignoriert.        | Dies ist optional. Für die Unterstützung der Nutzung einer Abfrage zum Abrufen von Daten für Datenquellen ist keine Option „Ignorieren“ vorhanden.       |


:::image type="content" source="../media/schema-configuration.png" alt-text="Schemakonfiguration" lightbox="../media/schema-configuration.png":::

Wählen Sie nach dem Konfigurieren des Schemas die Option **Schema überprüfen** aus. Bei diesem Vorgang werden von Metrics Advisor die folgenden Überprüfungen durchgeführt:
- Ob der Zeitstempel der abgefragten Daten nur in ein bestimmtes Intervall fällt 
- Ob für die gleiche Dimensionskombination innerhalb eines Metrikintervalls doppelte Werte zurückgegeben werden  

### <a name="automatic-roll-up-settings"></a>Einstellungen für automatisches Rollup

> [!IMPORTANT]
> Wenn Sie die **Ursachenanalyse** und andere Diagnosefunktionen aktivieren möchten, muss die Einstellung für automatisches Rollup konfiguriert werden. Nach der Aktivierung können die Einstellungen für automatisches Rollup nicht mehr geändert werden.

Metrics Advisor kann während der Erfassung für jede Dimension automatisch eine Aggregation durchführen (SUM, MAX, MIN, ...). Anschließend wird eine Hierarchie erstellt, die für die Ursachenanalyse und andere Diagnosefeatures verwendet wird. Ausführlichere Informationen finden Sie unter [Einstellungen für automatisches Rollup](../how-tos/onboard-your-data.md#automatic-roll-up-settings).

Geben Sie einen benutzerdefinierten Namen für den Datenfeed an. Dieser wird in Ihrem Arbeitsbereich angezeigt. Klicken Sie auf **Submit** (Senden). 

## <a name="tune-detection-configuration"></a>Optimieren der Erkennungskonfiguration

Nach dem Hinzufügen des Datenfeeds wird von Metrics Advisor versucht, Metrikdaten des angegebenen Startdatums zu erfassen. Es dauert einige Zeit, bis die Daten vollständig erfasst wurden. Wählen Sie zum Anzeigen des Erfassungsstatus im oberen Bereich der Datenfeedseite die Option **Ingestion progress** (Erfassungsstatus) aus. Wenn Daten erfasst wurden, wendet Metrics Advisor die Erkennung an und überwacht die Quelle weiter auf neue Daten.

Wählen Sie nach der Anwendung der Erkennung eine der Metriken aus, die im Datenfeed aufgelistet sind, um zur Seite **Metrikdetails** zu gelangen. Dort haben Sie folgende Möglichkeiten: 
- Anzeigen von Visualisierungen aller Zeitreihensegmente unter der betreffenden Metrik
- Aktualisieren der Erkennungskonfiguration, um die erwarteten Ergebnisse zu erhalten
- Einrichten von Benachrichtigungen für erkannte Anomalien

:::image type="content" source="../media/metric-details.png" alt-text="Metrikdetails" lightbox="../media/metric-details.png":::

## <a name="view-the-diagnostic-insights"></a>Anzeigen der Diagnoseerkenntnisse

Nach dem Optimieren der Erkennungskonfiguration sollte es sich bei ggf. gefundenen Anomalien um tatsächliche Anomalien in Ihren Daten handeln. Metrics Advisor führt eine Analyse für mehrdimensionale Metriken durch, um die Grundursache in einer bestimmten Dimension zu ermitteln, und eine metrikübergreifende Analyse per „Metrikgraph“. 

Wählen Sie zum Anzeigen der Diagnoseerkenntnisse in Zeitreihenvisualisierungen die roten Punkte aus. Diese stehen für die erkannten Anomalien. Daraufhin wird ein Fenster mit einem Link zur Seite für die Incidentanalyse angezeigt. 

:::image type="content" source="../media/incident-link.png" alt-text="Incidentlink" lightbox="../media/incident-link.png":::

Nach dem Auswählen dieses Links gelangen Sie zur Seite für die Incidentanalyse, auf der eine Gruppe mit verwandten Anomalien analysiert und eine Reihe von Diagnoseerkenntnissen angezeigt wird. Das Diagnostizieren eines Incidents umfasst drei Hauptschritte:

### <a name="check-summary-of-current-incident"></a>Überprüfen der Zusammenfassung des aktuellen Incidents

Oben befindet sich eine Zusammenfassung mit grundlegenden Informationen, Aktionen und Ablaufverfolgungen und einer analysierten Grundursache. Es werden grundlegende Informationen für die Bereiche „Wichtigste betroffene Reihen“ (mit Diagramm), „Start-/Endzeit der Beeinträchtigung“, „Schweregrad des Incidents“ und „Gesamte Anomalien“ angezeigt.

Bei der analysierten Grundursache handelt es sich um ein automatisch analysiertes Ergebnis. Von Metrics Advisor wird eine Analyse aller Anomalien durchgeführt, die für eine Zeitreihe innerhalb einer Metrik mit verschiedenen Dimensionswerten für denselben Zeitstempel erfasst wurden. Anschließend wird eine Korrelation durchgeführt, um verwandte Anomalien zu gruppieren, und es werden Informationen zur Grundursache generiert.

:::image type="content" source="../media/diagnostics/incident-summary.png" alt-text="Zusammenfassung der Incidentdiagnose" lightbox="../media/diagnostics/incident-summary.png":::

Basierend auf diesen Informationen können Sie sich bereits einen Überblick über den aktuellen Anomaliestatus sowie über die Auswirkungen des Incidents und die wahrscheinlichste Grundursache verschaffen. Dadurch können Sie umgehend Maßnahmen ergreifen, um den Incident schnellstmöglich aufzulösen. 

### <a name="view-cross-dimension-diagnostic-insights"></a>Anzeigen von dimensionsübergreifenden Diagnoseerkenntnissen

Nachdem Sie die grundlegenden Informationen und die Erkenntnisse der automatischen Analyse erhalten haben, können Sie umfassende ausführlichere Informationen zum Anomaliestatus für andere Dimensionen unter derselben Metrik erhalten, indem Sie die **Diagnosestruktur** verwenden.

Bei Metriken mit mehreren Dimensionen wird die Zeitreihe von Metrics Advisor als Hierarchie kategorisiert, die als „Diagnosestruktur“ bezeichnet wird. Es kann beispielsweise sein, dass die Metrik „Umsatz“ anhand von zwei Dimensionen überwacht wird: „Region“ und „Kategorie“. Auch wenn konkrete Dimensionswerte verwendet werden, muss ein **aggregierter** Dimensionswert vorhanden sein, z. B. **„SUM“** . Die Zeitreihen „Region“ = **„SUM“** und „Kategorie“ = **„SUM“** werden dann als Stammknoten der Struktur kategorisiert. Immer wenn unter der Dimension **„SUM“** eine Anomalie erfasst wird, kann hierfür ein Drilldown und eine Analyse ausgeführt werden, um zu ermitteln, welcher spezifische Dimensionswert am meisten zur Anomalie des übergeordneten Knotens beigetragen hat. Klicken Sie jeweils auf einen Knoten, um ihn zu erweitern und die ausführlichen Informationen anzuzeigen.

:::image type="content" source="../media/diagnostics/cross-dimension-diagnostic.png" alt-text="Incidentdiagnose: Dimensionsübergreifende Ansicht" lightbox="../media/diagnostics/cross-dimension-diagnostic.png":::

### <a name="view-cross-metrics-diagnostic-insights-using-metrics-graph"></a>Anzeigen von metrikübergreifenden Diagnoseerkenntnissen per „Metrikgraph“

Da es manchmal schwierig sein kann, ein Problem durch die Überprüfung des Anomaliestatus von nur einer Metrik zu analysieren, müssen Sie ggf. mehrere Metriken korrelieren. Kunden können einen „Metrikgraphen“ konfigurieren, mit dem die Beziehungen zwischen Metriken angegeben werden. Indem das obige Ergebnis der dimensionsübergreifenden Diagnose verwendet wird, ist die Grundursache auf einen bestimmten Dimensionswert beschränkt. Verwenden Sie dann den „Metrikgraphen“, und filtern Sie nach der Grundursache der analysierten Dimension, um den Anomaliestatus für andere Metriken zu überprüfen.
Durch Klicken auf diesen Link gelangen Sie zur Seite für die Incidentanalyse, auf der die entsprechende Anomalie analysiert und eine Reihe von Diagnoseerkenntnissen angezeigt wird. Die Seite mit den Incidentdetails enthält drei Abschnitte, die den drei Hauptschritten zum Diagnostizieren eines Incidents entsprechen. 

:::image type="content" source="../media/diagnostics/cross-metrics-analysis.png" alt-text="Incidentdiagnose: Metrikübergreifende Analyse" lightbox="../media/diagnostics/cross-metrics-analysis.png":::

Sie können sich aber auch noch weitere Diagnoseerkenntnisse ansehen und zusätzliche Features nutzen, um Detailinformationen zu Anomalien nach Dimensionen zu erhalten, ähnliche Anomalien anzuzeigen und metrikübergreifende Vergleiche anzustellen. Weitere Informationen finden Sie unter [Diagnostizieren eines Incidents](../how-tos/diagnose-an-incident.md). 

## <a name="get-notified-when-new-anomalies-are-found"></a>Erhalten einer Benachrichtigung, wenn neue Anomalien gefunden werden

Wenn Sie im Falle einer erkannten Anomalie in Ihren Daten eine Benachrichtigung erhalten möchten, können Sie ein Abonnement für einzelne oder mehrere Ihrer Metriken erstellen. Metrics Advisor verwendet Hooks, um Warnungen zu senden. Drei Arten von Hooks werden unterstützt: E-Mail-Hook, Webhook und Azure DevOps. Hier wird als Beispiel ein Webhook verwendet. 

### <a name="create-a-web-hook"></a>Erstellen eines Webhooks

Ein Webhook ist der Einstiegspunkt für die Anomalieerkennung durch eine programmgesteuerte Methode des Metrics Advisor-Diensts. Hierbei wird eine vom Benutzer bereitgestellte API aufgerufen, wenn eine Warnung ausgelöst wird. Ausführliche Informationen zur Erstellung von Hooks finden Sie im Abschnitt **Erstellen eines Hooks** des Artikels [Anleitung: Konfigurieren von Warnungen und Erhalten von Benachrichtigungen per Hook](../how-tos/alerts.md#create-a-hook). 

### <a name="configure-alert-settings"></a>Konfigurieren der Warnungseinstellungen

Nach Erstellung eines Hooks wird durch eine Warnungseinstellung bestimmt, wie und welche Warnbenachrichtigungen gesendet werden. Pro Metrik können jeweils mehrere Warnungseinstellungen festgelegt werden. Zwei wichtige Einstellungen sind **Warnung für** zum Angeben der einzuschließenden Anomalien und **Filter anomaly options** (Anomalieoptionen filtern), um zu definieren, welche Anomalien in die Warnung eingeschlossen werden sollen. Ausführlichere Informationen finden Sie im Abschnitt **Hinzufügen oder Bearbeiten von Warnungseinstellungen** der Anleitung [Konfigurieren von Warnungen und Erhalten von Benachrichtigungen per Hook](../how-tos/alerts.md#add-or-edit-alert-settings).


## <a name="next-steps"></a>Nächste Schritte

- [Durchführen des Onboardings für Ihre Datenfeeds](../how-tos/onboard-your-data.md)
    - [Verwalten von Datenfeeds](../how-tos/manage-data-feeds.md)
    - [Konfigurationen für unterschiedliche Datenquellen](../data-feeds-from-different-sources.md)
- [Verwenden von REST-API oder Clientbibliotheken](./rest-api-and-client-library.md)
- [Konfigurieren von Metriken und Optimieren der Erkennungskonfiguration](../how-tos/configure-metrics.md)
