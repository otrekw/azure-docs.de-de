---
title: Diagnose und Problembehandlung einer Preview-Umgebung – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Diagnose und Problembehandlung in einer Azure Time Series Insights Preview-Umgebung durchführen.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: seodec18
ms.openlocfilehash: 1fc3aa6caa6266d2cd42e4783e8e39d5cc92c220
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2020
ms.locfileid: "75861573"
---
# <a name="diagnose-and-troubleshoot-a-preview-environment"></a>Diagnose und Problembehandlung einer Preview-Umgebung

In diesem Artikel werden einige häufig auftretende Probleme zusammengefasst, die auftreten können, wenn Sie mit Ihrer Azure Time Series Insights Preview-Umgebung arbeiten. Darüber hinaus werden mögliche Ursachen und Lösungen für jedes der Probleme beschrieben.

## <a name="problem-i-cant-find-my-environment-in-the-preview-explorer"></a>Problem: Ich kann meine Umgebung im Preview-Explorer nicht finden.

Dieses Problem kann auftreten, wenn Sie nicht über die Berechtigung für den Zugriff auf die Time Series Insights-Umgebung verfügen. Benutzer benötigen eine Zugriffsrolle auf Leser-Ebene, um ihre Time Series Insights-Umgebung anzuzeigen. Um die aktuellen Zugriffsebenen zu überprüfen und zusätzlichen Zugriff zu gewähren, wechseln Sie zum Abschnitt **Datenzugriffsrichtlinien** in der Time Series Insights-Ressource im [Azure-Portal](https://portal.azure.com/).

  [![Überprüfen der Datenzugriffsrichtlinien](media/preview-troubleshoot/verify-data-access-policies.png)](media/preview-troubleshoot/verify-data-access-policies.png#lightbox)

## <a name="problem-no-data-is-seen-in-the-preview-explorer"></a>Problem: Im Preview-Explorer werden keine Daten angezeigt.

Es gibt verschiedene mögliche Gründe, aus denen Ihre Daten im [Azure Time Series Insights Preview-Explorer](https://insights.timeseries.azure.com/preview) nicht angezeigt werden.

- Ihre Ereignisquelle empfängt möglicherweise keine Daten.

    Stellen Sie sicher, dass Ihre Ereignisquelle, bei der es sich um einen Event Hub oder einen IoT-Hub handelt, Daten von Ihren Tags oder Instanzen empfängt. Um dies zu überprüfen, wechseln Sie zur Übersichtsseite Ihrer Ressourcen im Azure-Portal.

    [![Übersicht über das Überprüfen von Dashboardmetriken](media/preview-troubleshoot/verify-dashboard-metrics.png)](media/preview-troubleshoot/verify-dashboard-metrics.png#lightbox)

- Ihre Ereignisquelldaten sind nicht im JSON-Format.

    Time Series Insights unterstützt nur JSON-Daten. JSON-Beispiele finden Sie unter [Unterstützte JSON-Formen](./how-to-shape-query-json.md).

- Dem Schlüssel Ihrer Ereignisquelle fehlt eine erforderliche Berechtigung.

  * Für IoT Hub müssen Sie einen Schlüssel mit der Berechtigung **Dienstverbindung** bereitstellen.

    [![Überprüfen der IoT Hub-Berechtigungen](media/preview-troubleshoot/verify-correct-permissions.png)](media/preview-troubleshoot/verify-correct-permissions.png#lightbox)

    * Die Richtlinien **iothubowner** und **service** funktionieren beide, da sie über die Berechtigung **Dienstverbindung** verfügen.

  * Für einen Event Hub müssen Sie einen Schlüssel mit der Berechtigung **Lauschen** bereitstellen.
  
    [![Überprüfen der Event Hub-Berechtigungen](media/preview-troubleshoot/verify-eh-permissions.png)](media/preview-troubleshoot/verify-eh-permissions.png#lightbox)

    * Die Richtlinien **read** und **write** funktionieren beide, da sie über die Berechtigung **Lauschen** verfügen.

- Ihre bereitgestellte Consumergruppe ist für Time Series Insights nicht exklusiv.

    Während der Registrierung eines IoT-Hubs oder eines Event Hubs geben Sie die Consumergruppe an, die zum Lesen der Daten verwendet wird. Diese Consumergruppe muss pro Umgebung eindeutig sein. Wenn die Consumergruppe geteilt wird, trennt der zugrunde liegende Event Hub automatisch einen der Leser nach dem Zufallsprinzip. Stellen Sie eine eindeutige Consumergruppe bereit, aus der Time Series Insights lesen soll.

- Ihre Time Series-ID-Eigenschaft, die zum Zeitpunkt der Bereitstellung angegeben wurde, ist falsch, fehlt oder ist null.

    Dieses Problem kann auftreten, wenn die Time Series-ID-Eigenschaft zum Zeitpunkt der Bereitstellung der Umgebung fehlerhaft konfiguriert ist. Weitere Informationen finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](./time-series-insights-update-how-to-id.md). Zu diesem Zeitpunkt können Sie eine vorhandene Time Series Insights-Umgebung nicht so aktualisieren, dass sie eine andere Time Series-ID verwendet.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Problem: Einige Daten werden angezeigt, andere hingegen fehlen.

Möglicherweise werden Sie Daten ohne die Time Series-ID.

- Dieses Problem kann auftreten, wenn Sie Ereignisse ohne das Feld „Time Series-ID“ in der Nutzlast senden. Weitere Informationen finden Sie unter [Unterstützte JSON-Formen](./how-to-shape-query-json.md).
- Dieses Problem kann auftreten, weil Ihre Umgebung gedrosselt wird.

    > [!NOTE]
    > Zurzeit unterstützt Time Series Insights eine maximale Datenerfassungsrate von 6 Mbit/s.

## <a name="problem-my-event-sources-timestamp-property-name-doesnt-work"></a>Problem: Die Timestamp-Eigenschaft für den Namen meiner Ereignisquelle funktioniert nicht.

Stellen Sie sicher, dass Name und Wert den folgenden Regeln entsprechen:

* Der Name der Timestamp-Eigenschaft beachtet Groß-/Kleinschreibung.
* Der Wert für die Timestamp-Eigenschaft, der als JSON-Zeichenfolge von Ihrer Ereignisquelle eingeht, hat das Format `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Ein Beispiel für eine solche Zeichenfolge ist `“2008-04-12T12:53Z”`.

Sie können am einfachsten sicherstellen, dass Ihr Name der Timestamp-Eigenschaft erfasst wird und richtig funktioniert, indem Sie den Time Series Insights-Explorer verwenden. Wählen Sie im Time Series Insights-Explorer mithilfe des Diagramms einen Zeitraum aus, nachdem Sie den Namen der Timestamp-Eigenschaft angegeben haben. Klicken Sie mit der rechten Maustaste auf die Auswahl, und wählen Sie die Option **Ereignisse untersuchen** aus. Die erste Spaltenüberschrift ist der Name Ihrer Timestamp-Eigenschaft. Er sollte neben dem Wort `Timestamp` die Zeichenfolge `($ts)` aufweisen, statt:

* `(abc)`, was anzeigt, dass Time Series Insights die Datenwerte als Zeichenfolgen liest.
* Das **Kalender**symbol, das anzeigt, dass Time Series Insights die Datenwerte als Datum/Uhrzeit-Wert liest.
* `#`, was anzeigt, dass Time Series Insights die Datenwerte als ganze Zahl liest.

Wenn die Timestamp-Eigenschaft nicht explizit angegeben ist, wird der Zeitpunkt der Einreihung eine Ereignisses in die Warteschlange für einen IoT-Hub oder Event Hub als Standardzeitstempel verwendet.

## <a name="problem-i-cant-view-data-from-my-warm-store-in-the-explorer"></a>Problem: Ich kann keine Daten aus meinem warmen Speicher im Explorer anzeigen.

- Möglicherweise haben Sie Ihren warmen Speicher erst vor Kurzem bereitgestellt, und die Daten werden noch übertragen.
- Möglicherweise haben Sie Ihren warmen Speicher gelöscht. in diesem Fall wären die Daten verloren.

## <a name="problem-i-cant-view-or-edit-my-time-series-model"></a>Problem: Ich kann mein Zeitreihenmodell nicht anzeigen oder bearbeiten.

- Möglicherweise greifen Sie auf eine Time Series Insights S1- oder S2-Umgebung zu.

   Zeitreihenmodelle werden nur in Umgebungen mit nutzungsbasierter Bezahlung unterstützt. Weitere Informationen zum Zugriff auf Ihre S1- oder S2-Umgebung über den Time Series Insights Preview-Explorer finden Sie unter [Visualisieren von Daten im Explorer](./time-series-insights-update-explorer.md).

   [![Keine Ereignisse in der Umgebung](media/preview-troubleshoot/troubleshoot-no-events.png)](media/preview-troubleshoot/troubleshoot-no-events.png#lightbox)

- Möglicherweise besitzen Sie keine Berechtigungen zum Anzeigen und Bearbeiten des Modells.

   Benutzer benötigen Zugriff auf Mitwirkender-Ebene, um ihre Zeitreihenmodelle zu bearbeiten und anzuzeigen. Um die aktuellen Zugriffsebenen zu überprüfen und zusätzlichen Zugriff zu gewähren, wechseln Sie zum Abschnitt **Datenzugriffsrichtlinien** in Ihrer Time Series Insights-Ressource im Azure-Portal.

## <a name="problem-all-my-instances-in-the-preview-explorer-lack-a-parent"></a>Problem: Bei allen meinen Instanzen im Preview-Explorer fehlt ein übergeordnetes Element.

Dieses Problem kann auftreten, wenn in Ihrer Umgebung keine Zeitreihenmodell-Hierarchie definiert ist. Weitere Informationen finden Sie unter [Arbeiten mit Zeitreihenmodellen](./time-series-insights-update-how-to-tsm.md).

  [![Bei Instanzen ohne übergeordnetes Element wird eine Warnung angezeigt.](media/preview-troubleshoot/unparented-instances.png)](media/preview-troubleshoot/unparented-instances.png#lightbox)

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie [Arbeiten mit Zeitreihenmodellen](./time-series-insights-update-how-to-tsm.md).

- Weitere Informationen zu [unterstützten JSON-Formen](./how-to-shape-query-json.md).

- Lesen Sie [Planung und Limits](./time-series-insights-update-plan.md) in Azure Time Series Insights Preview.
