---
title: Überwachen von Python-Anwendungen mit Azure Monitor (Vorschau) | Microsoft-Dokumentation
description: Enthält eine Anleitung zum Verknüpfen von OpenCensus Python mit Azure Monitor.
ms.topic: conceptual
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.reviewer: mbullwin
ms.openlocfilehash: a2b66cdc7a0704cd3560c0776a0ca5302dc689d2
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78250766"
---
# <a name="set-up-azure-monitor-for-your-python-application-preview"></a>Einrichten von Azure Monitor für Ihre Python-Anwendung (Vorschau)

Azure Monitor unterstützt durch die Integration mit [OpenCensus](https://opencensus.io) die verteilte Ablaufverfolgung, Metrikerfassung und Protokollierung für Python-Anwendungen. In diesem Artikel werden der Prozess zur Einrichtung von OpenCensus für Python und das Senden Ihrer Überwachungsdaten an Azure Monitor beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.
- Python-Installation. In diesem Artikel wird [Python 3.7.0](https://www.python.org/downloads/) verwendet, aber auch frühere Versionen können mit kleineren Änderungen wahrscheinlich verwendet werden.

## <a name="sign-in-to-the-azure-portal"></a>Melden Sie sich auf dem Azure-Portal an.

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

## <a name="create-an-application-insights-resource-in-azure-monitor"></a>Erstellen einer Application Insights-Ressource in Azure Monitor

Zuerst müssen Sie eine Application Insights-Ressource in Azure Monitor erstellen, die einen Instrumentierungsschlüssel (ikey) generiert. Der ikey wird zum Konfigurieren des OpenCensus SDK für das Senden von Telemetriedaten an Azure Monitor verwendet.

1. Klicken Sie auf **Ressource erstellen** > **Entwicklertools** > **Application Insights**.

   ![Hinzufügen einer Application Insights-Ressource](./media/opencensus-python/0001-create-resource.png)

1. Ein Konfigurationsfeld wird angezeigt. Füllen Sie die Eingabefelder anhand der Informationen in der folgenden Tabelle aus.

   | Einstellung        | value           | BESCHREIBUNG  |
   | ------------- |:-------------|:-----|
   | **Name**      | Global eindeutiger Wert | Der Name, der die zu überwachende App identifiziert. |
   | **Ressourcengruppe**     | myResourceGroup      | Der Name der neuen Ressourcengruppe, die Application Insights-Daten hosten soll. |
   | **Location** | East US | Ein Standort in Ihrer Nähe oder in der Nähe des Standorts, an dem Ihre App gehostet wird. |

1. Klicken Sie auf **Erstellen**.

## <a name="instrument-with-opencensus-python-sdk-for-azure-monitor"></a>Instrumentieren mit dem OpenCensus Python SDK für Azure Monitor

Installieren Sie OpenCensus Azure Monitor Exporters:

```console
python -m pip install opencensus-ext-azure
```

Eine vollständige Liste der Pakete und Integrationen finden Sie unter [OpenCensus-Pakete](https://docs.microsoft.com/azure/azure-monitor/app/nuget#common-packages-for-python-using-opencensus).

> [!NOTE]
> Beim Befehl `python -m pip install opencensus-ext-azure` wird davon ausgegangen, dass Sie eine `PATH`-Umgebungsvariable für die Python-Installation festgelegt haben. Wenn Sie diese Variable nicht konfiguriert haben, müssen Sie den vollständigen Verzeichnispfad zum Speicherort Ihrer ausführbaren Python-Datei angeben. Das Ergebnis ist ein Befehl, der Folgendem ähnelt: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`

Das SDK verwendet drei Azure Monitor-Exportprogramme, um unterschiedliche Arten von Telemetriedaten an Azure Monitor zu senden: Ablaufverfolgung, Metriken und Protokolle. Weitere Informationen zu diesen Telemetrietypen finden Sie in der [Übersicht über die Datenplattform](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform). Befolgen Sie die nachstehenden Anweisungen, um diese Telemetrietypen über die drei Exportprogramme zu senden.

## <a name="telemetry-type-mappings"></a>Zuordnungen von Telemetriedatentypen

Im Folgenden finden Sie die Zuordnungen der von OpenCensus bereitgestellten Exportprogramme zu den in Azure Monitor angezeigten Typen von Telemetriedaten.

![Screenshot der Zuordnungen von Telemetrietypen von OpenCensus zu Azure Monitor](./media/opencensus-python/0012-telemetry-types.png)

### <a name="trace"></a>Trace

> [!NOTE]
> `Trace` in OpenCensus bezieht sich auf die [verteilte Ablaufverfolgung](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing). `AzureExporter` sendet Telemetrie zu `requests` und `dependency` an Azure Monitor.

1. Zuerst generieren wir lokal einige Ablaufverfolgungsdaten. Geben Sie in Python IDLE oder einem Editor Ihrer Wahl den unten angegebenen Code ein.

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Durch Ausführen des Codes werden Sie wiederholt aufgefordert, einen Wert einzugeben. Mit jedem Eintrag wird der Wert in die Shell gedruckt, und vom OpenCensus Python-Modul wird ein entsprechendes `SpanData`-Element generiert. Das OpenCensus-Projekt definiert eine [Ablaufverfolgung als Struktur von Spannen (Spans)](https://opencensus.io/core-concepts/tracing/).
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Das Eingeben von Werten ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich `SpanData` für Azure Monitor ausgeben. Übergeben Sie die Verbindungszeichenfolge direkt an das Exportprogramm, oder geben Sie sie in einer `APPLICATIONINSIGHTS_CONNECTION_STRING`-Umgebungsvariablen an. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Wenn Sie das Python-Skript jetzt ausführen, sollten Sie weiterhin zur Eingabe von Werten aufgefordert werden, doch wird nur der Wert in die Shell gedruckt. Das erstellte `SpanData`-Element wird an Azure Monitor gesendet. Sie finden die ausgegebenen Span-Daten unter `dependencies`. Weitere Details zu ausgehenden Anforderungen finden Sie unter [Nachverfolgen von Abhängigkeiten mit OpenCensus Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-dependency).
Weitere Details zu eingehenden Anforderungen finden Sie unter [Nachverfolgen von Anforderungen mit OpenCensus Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python-request).

#### <a name="sampling"></a>Stichproben

Informationen zur Stichprobenerstellung in OpenCensus finden Sie unter [Stichprobenerstellung in OpenCensus](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

#### <a name="trace-correlation"></a>Korrelation der Ablaufverfolgung

Ausführliche Informationen zur Telemetriekorrelation in den Ablaufverfolgungsdaten finden Sie unter [Telemetriekorrelation in OpenCensus Python](https://docs.microsoft.com/azure/azure-monitor/app/correlation#telemetry-correlation-in-opencensus-python).

#### <a name="modify-telemetry"></a>Ändern der Telemetrie

Einzelheiten dazu, wie Sie nachverfolgte Telemetrie modifizieren können, bevor sie an Azure Monitor gesendet wird, finden Sie unter [OpenCensus Python-Telemetrieprozessoren](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors).

### <a name="metrics"></a>Metriken

1. Zuerst generieren wir einige lokale Metrikdaten. Wir erstellen eine einfache Metrik, um nachzuverfolgen, wie häufig der Benutzer die EINGABETASTE betätigt.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Bei Ausführung des Codes werden Sie wiederholt aufgefordert, die EINGABETASTE zu drücken. Es wird eine Metrik erstellt, um die Anzahl von Betätigungen der EINGABETASTE nachzuverfolgen. Bei jedem Eintrag wird der Wert erhöht, und die Metrikinformationen werden in der Konsole angezeigt. Die Informationen umfassen den aktuellen Wert und den aktuellen Zeitstempel für den Aktualisierungszeitpunkt der Metrik.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Das Eingeben von Werten ist zu Demonstrationszwecken zwar hilfreich, aber wir möchten eigentlich die Metrikdaten für Azure Monitor ausgeben. Übergeben Sie die Verbindungszeichenfolge direkt an das Exportprogramm, oder geben Sie sie in einer `APPLICATIONINSIGHTS_CONNECTION_STRING`-Umgebungsvariablen an. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')

    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. Das Exportprogramm sendet die Metrikdaten in einem festen Intervall an Azure Monitor. Die Standardeinstellung ist alle 15 Sekunden. Wir verfolgen nur eine einzelne Metrik. Diese Metrikdaten werden also mit dem jeweiligen Wert und Zeitstempel in jedem Intervall gesendet. Sie finden die Daten unter `customMetrics`.

#### <a name="standard-metrics"></a>Standardmetriken

Standardmäßig sendet das Tool zum Exportieren von Metriken eine Gruppe von Standardmetriken an Azure Monitor. Sie können dies deaktivieren, indem Sie das Flag `enable_standard_metrics` im Konstruktor des Tools zum Exportieren von Metriken auf `False` festlegen.

    ```python
    ...
    exporter = metrics_exporter.new_metrics_exporter(
      enable_standard_metrics=False,
      connection_string='InstrumentationKey=<your-instrumentation-key-here>')
    ...
    ```
Es folgt eine Liste von Standardmetriken, die derzeit gesendet werden:

- Verfügbarer Arbeitsspeicher (Bytes)
- CPU – Prozessorzeit (%)
- Rate eingehender Anforderungen (pro Sekunde)
- Durchschnittliche Ausführungsdauer eingehender Anforderungen (Millisekunden)
- Rate ausgehender Anforderungen (pro Sekunde)
- Prozess – CPU-Auslastung (%)
- Prozess – Private Bytes (Bytes)

Diese Metriken sollten in `performanceCounters` angezeigt werden. Die Rate eingehender Anforderungen (pro Sekunde) läge unter `customMetrics`.
#### <a name="modify-telemetry"></a>Ändern der Telemetrie

Einzelheiten dazu, wie Sie nachverfolgte Telemetrie modifizieren können, bevor sie an Azure Monitor gesendet wird, finden Sie unter [OpenCensus Python-Telemetrieprozessoren](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors).

### <a name="logs"></a>Protokolle

1. Zuerst generieren wir einige lokale Protokolldaten.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  Der Code fordert fortlaufend zur Eingabe eines Werts auf. Es wird ein Protokolleintrag für jeden eingegebenen Wert ausgegeben.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Das Eingeben von Werten ist zu Demonstrationszwecken zwar hilfreich, wir möchten jedoch eigentlich die Protokolldaten für Azure Monitor ausgeben. Übergeben Sie die Verbindungszeichenfolge direkt an das Exportprogramm, oder geben Sie sie in einer `APPLICATIONINSIGHTS_CONNECTION_STRING`-Umgebungsvariablen an. Ändern Sie Ihren Code aus dem vorherigen Schritt basierend auf dem folgenden Codebeispiel:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. Das Exportprogramm sendet Protokolldaten an Azure Monitor. Sie finden die Daten unter `traces`. 

> [!NOTE]
> `traces` in diesem Kontext ist nicht identisch mit `Tracing`. `traces` bezieht sich auf den Typ der Telemetrie, der bei der Verwendung von `AzureLogHandler` in Azure Monitor angezeigt wird. `Tracing` bezieht sich auf ein Konzept in OpenCensus und betrifft die [verteilte Ablaufverfolgung](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing).

5. Zum Formatieren der Protokollmeldungen können Sie `formatters` in der integrierten Python-[Protokollierungs-API](https://docs.python.org/3/library/logging.html#formatter-objects) verwenden.

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    format_str = '%(asctime)s - %(levelname)-8s - %(message)s'
    date_format = '%Y-%m-%d %H:%M:%S'
    formatter = logging.Formatter(format_str, date_format)
    # TODO: replace the all-zero GUID with your instrumentation key.
    handler = AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    handler.setFormatter(formatter)
    logger.addHandler(handler)
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

6. Sie können Ihren Protokollnachrichten auch benutzerdefinierte Eigenschaften im Schlüsselwortargument *extra* hinzufügen, indem Sie das Feld „custom_dimensions“ verwenden. Diese werden als Schlüssel-Wert-Paare in `customDimensions` in Azure Monitor angezeigt.
> [!NOTE]
> Damit dieses Feature funktioniert, müssen Sie im Feld „custom_dimensions“ ein Wörterbuch übergeben. Wenn Sie Argumente eines anderen Typs übergeben, werden diese von der Protokollierung ignoriert.

    ```python
    import logging
    
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )

    properties = {'custom_dimensions': {'key_1': 'value_1', 'key_2': 'value_2'}}

    # Use properties in logging statements
    logger.warning('action', extra=properties)

    # Use properties in exception logs
    try:
        result = 1 / 0  # generate a ZeroDivisionError
    except Exception:
    logger.exception('Captured an exception.', extra=properties)
    ```
#### <a name="sampling"></a>Stichproben

Informationen zur Stichprobenerstellung in OpenCensus finden Sie unter [Stichprobenerstellung in OpenCensus](sampling.md#configuring-fixed-rate-sampling-for-opencensus-python-applications).

#### <a name="log-correlation"></a>Protokollkorrelation

Ausführliche Informationen dazu, wie Sie die Protokolle um Daten im Ablaufverfolgungskontext erweitern können, finden Sie unter [Integration von Protokollen](https://docs.microsoft.com/azure/azure-monitor/app/correlation#log-correlation) für OpenCensus Python.

#### <a name="modify-telemetry"></a>Ändern der Telemetrie

Einzelheiten dazu, wie Sie nachverfolgte Telemetrie modifizieren können, bevor sie an Azure Monitor gesendet wird, finden Sie unter [OpenCensus Python-Telemetrieprozessoren](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#opencensus-python-telemetry-processors).

## <a name="view-your-data-with-queries"></a>Anzeigen Ihrer Daten mit Abfragen

Sie können die von Ihrer Anwendung gesendeten Telemetriedaten über die Registerkarte **Protokolle (Analytics)** anzeigen.

![Screenshot des Übersichtsbereichs mit „Protokolle (Analytics)“ in einem roten Kasten](./media/opencensus-python/0010-logs-query.png)

In der Liste unter **Aktiv** ist Folgendes angegeben:

- Für Telemetriedaten, die mit dem Exportprogramm der Azure Monitor-Ablaufverfolgung gesendet werden, werden eingehende Anforderungen unter `requests` angezeigt. Ausgehende bzw. in Bearbeitung befindliche Anforderungen werden unter `dependencies` angezeigt.
- Für Telemetriedaten, die mit dem Exportprogramm für Azure Monitor-Metriken gesendet werden, werden die gesendeten Metriken unter `customMetrics` angezeigt.
- Für Telemetriedaten, die mit dem Exportprogramm für Azure Monitor-Protokolle gesendet werden, werden die Protokolle unter `traces` angezeigt. Ausnahmen werden unter `exceptions` angezeigt.

Ausführlichere Informationen zur Verwendung von Abfragen und Protokollen finden Sie unter [Protokolle in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs).

## <a name="learn-more-about-opencensus-for-python"></a>Weitere Informationen zu OpenCensus für Python

* [OpenCensus Python auf GitHub](https://github.com/census-instrumentation/opencensus-python)
* [Anpassung](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Django-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL-Integration](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Nächste Schritte

* [Anwendungszuordnung](./../../azure-monitor/app/app-map.md)
* [End-to-End-Leistungsüberwachung](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Alerts

* [Verfügbarkeitstests](../../azure-monitor/app/monitor-web-app-availability.md): Erstellen Sie Tests, um sicherzustellen, dass Ihre Website im Web sichtbar ist.
* [Intelligente Diagnose](../../azure-monitor/app/proactive-diagnostics.md): Diese Tests werden automatisch ausgeführt, sodass Sie keinerlei Einrichtungsschritte ausführen müssen. Sie werden darüber benachrichtigt, ob für Ihre App eine ungewöhnlich hohe Zahl von Anforderungen mit Fehlern vorliegt.
* [Metrikwarnungen](../../azure-monitor/app/alerts.md): Richten Sie Warnungen ein, damit Sie gewarnt werden, wenn für eine Metrik ein Schwellenwert überschritten wird. Sie können diese für benutzerdefinierte Metriken festlegen, die Sie in Ihrer App codieren.
