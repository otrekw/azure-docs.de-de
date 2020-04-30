---
title: 'Azure Time Series Insights: Planen Ihrer Vorschauumgebung | Microsoft-Dokumentation'
description: Bewährte Methoden für die Konfiguration, Verwaltung, Planung und Bereitstellung Ihrer Azure Time Series Insights Preview-Umgebung.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 04/27/2020
ms.custom: seodec18
ms.openlocfilehash: 4b61df52df45cb2ee01407390ce3e34d86350ef7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82189248"
---
# <a name="plan-your-azure-time-series-insights-preview-environment"></a>Planen Ihrer Azure Time Series Insights Preview-Umgebung

Dieser Artikel beschreibt bewährte Methoden zum Planen und schnellen Starten mit Azure Time Series Insights Preview.

> [!NOTE]
> Bewährte Methoden zum Planen einer Time Series Insights-Instanz für allgemeine Verfügbarkeit finden Sie unter [Planen Ihrer Azure Time Series Insights-GA-Umgebung](time-series-insights-environment-planning.md).

## <a name="best-practices-for-planning-and-preparation"></a>Bewährte Methoden für Planung und Vorbereitung

Bewährte Methoden zur Planung und Vorbereitung Ihrer Umgebung werden in den folgenden Artikeln ausführlicher beschrieben:

* Was Sie erhalten, wenn Sie [eine Azure Time Series Insights Preview-Umgebung bereitstellen](#the-preview-environment).
* Wie die [Time Series-IDs und Zeitstempeleigenschaften lauten](#configure-time-series-ids-and-timestamp-properties).
* Was das neue [Time Series-Modell ist](#understand-the-time-series-model) und wie Sie ein eigenes erstellen.
* Wie Sie [Ereignisse effizient an JSON senden](#shape-your-events).
* [Business Disaster Recovery-Optionen](#business-disaster-recovery) mit Time Series Insights.

Für Azure Time Series Insights wird ein Geschäftsmodell mit nutzungsbasierte Bezahlung angewendet. Weitere Informationen zu Gebühren und Kapazitäten finden Sie auf der Seite [Azure Time Series Insights – Preise](https://azure.microsoft.com/pricing/details/time-series-insights/).

## <a name="the-preview-environment"></a>Die Preview-Umgebung

Wenn Sie eine Time Series Insights Preview-Umgebung bereitstellen, erstellen Sie zwei Azure-Ressourcen:

* Eine Azure Time Series Insights Preview-Umgebung
* Ein Azure-Speicherkonto vom Typ „Allgemein v1“

Im Rahmen des Bereitstellungsprozesses geben Sie an, ob Sie einen Warm Storage aktivieren möchten. Der Warm Storage bietet Ihnen ein mehrstufiges Abfrageerlebnis. Wenn diese Option aktiviert ist, müssen Sie einen Aufbewahrungszeitraum zwischen 7 und 30 Tagen angeben. Abfragen, die innerhalb des Warm Storage-Aufbewahrungszeitraums ausgeführt werden, bieten in der Regel schnellere Antwortzeiten. Wenn sich eine Abfrage über den Warm Storage-Aufbewahrungszeitraum erstreckt, wird sie aus dem Cold Storage bearbeitet.

Abfragen zum Warm Storage sind kostenlos, während Abfragen zum Cold Storage Kosten verursachen. Es ist wichtig, Ihre Abfragemuster zu verstehen und Ihre Warm Storage-Konfiguration entsprechend zu planen. Es wird empfohlen, dass sich die interaktiven Analysen der aktuellsten Daten in Ihrem Warm Storage und die Musteranalysen sowie langfristigen Trends im Cold Storage befinden.

> [!NOTE]
> Weitere Informationen zum Abfragen Ihrer warmen Daten finden Sie in der [API-Referenz](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#uri-parameters).

Um zu beginnen, benötigen Sie drei zusätzliche Elemente:

* Ein [Zeitreihenmodell](./time-series-insights-update-tsm.md)
* Eine [mit Time Series Insights verbundene Ereignisquelle](./time-series-insights-how-to-add-an-event-source-iothub.md)
* [In die Ereignisquelle fließende Ereignisse](./time-series-insights-send-events.md), die sowohl dem Modell zugeordnet als auch im gültigen JSON-Format sind

## <a name="review-preview-limits"></a>Überprüfen der Grenzwerte der Vorschau

[!INCLUDE [Review Time Series Insights Preview limits](../../includes/time-series-insights-preview-limits.md)]

## <a name="configure-time-series-ids-and-timestamp-properties"></a>Konfigurieren von Time Series-IDs und Zeitstempeleigenschaften

Um eine neue Time Series Insights-Umgebung zu erstellen, wählen Sie eine Time Series-ID aus. Diese Vorgehensweise fungiert als logische Partition für Ihre Daten. Wie bereits erwähnt, stellen Sie sicher, dass Sie Ihre Time Series-ID bereit haben.

> [!IMPORTANT]
> Zeitreihen-IDs können *später nicht mehr geändert werden*. Überprüfen Sie jede vor der endgültigen Auswahl und der ersten Verwendung.

Sie können bis zu drei Schlüssel auswählen, um Ihre Ressourcen eindeutig zu unterscheiden. Weitere Informationen finden Sie unter [Bewährte Methoden für die Auswahl einer Time Series-ID](./time-series-insights-update-how-to-id.md) und [Speicherung und Dateneingang](./time-series-insights-update-storage-ingress.md).

Die **Timestamp**-Eigenschaft ist ebenfalls wichtig. Sie können diese Eigenschaft festlegen, wenn Sie Ereignisquellen hinzufügen. Jede Ereignisquelle besitzt eine optionale Timestamp-Eigenschaft, die zum Nachverfolgen von Ereignisquellen im Laufe der Zeit verwendet wird. Zeitstempelwerte beachten die Groß- und Kleinschreibung und müssen gemäß der individuellen Spezifikation jeder Ereignisquelle formatiert sein.

> [!TIP]
> Überprüfen Sie die Formatierungs- und Analyseanforderungen für Ihre Ereignisquellen.

Wenn der Zeitstempel eines Ereignisses leer bleibt, wird der Zeitpunkt der Einreihung des Ereignisses in die Warteschlange einer Ereignisquelle verwendet. Wenn Sie historische Daten oder Ereignisse im Batch senden, ist es hilfreicher, die Timestamp-Eigenschaft anzupassen, statt den Zeitpunkt der Einreihung des Ereignisses in die Warteschlange zu verwenden. Weitere Informationen finden Sie unter der Vorgehensweise zum [Hinzufügen von Ereignisquellen in Azure IoT Hub](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="understand-the-time-series-model"></a>Grundlagen des Zeitreihenmodells

Sie können jetzt das Zeitreihenmodell Ihrer Time Series Insights-Umgebung konfigurieren. Das neue Modell erleichtert das Auffinden und Analysieren von IoT-Daten. Es ermöglicht die Zusammenstellung, Wartung und Anreicherung von Zeitreihendaten und hilft bei der Vorbereitung für Consumer bereiter Datasets. Das Modell verwendet Time Series-IDs, die einer Instanz zugeordnet werden, die die eindeutige Ressource mit Variablen, bekannt als Typen, und Hierarchien verknüpft. Erfahren Sie mehr über das neue [Zeitreihenmodell](./time-series-insights-update-tsm.md).

Das Modell ist dynamisch, damit es jederzeit erstellt werden kann. Für den schnellen Einstieg erstellen und laden Sie es hoch, bevor Sie Daten per Push in Time Series Insights übertragen. Informationen zum Erstellen Ihres Modells finden Sie unter [Datenmodellierung in Azure Time Series Insights Preview](./time-series-insights-update-how-to-tsm.md).

Bei vielen Kunden wird das Zeitreihenmodell einem vorhandenen Ressourcenmodell oder einem bereits eingerichteten ERP-System zugeordnet. Wenn Sie nicht über ein vorhandenes Modell verfügen, ist wird eine vordefinierte Erfahrung [bereitgestellt](https://github.com/Microsoft/tsiclient), um schnell den Betrieb aufnehmen zu können. Um einen Eindruck zu erhalten, wie Ihnen ein Modell helfen könnte, sehen Sie sich die [Beispieldemoumgebung](https://insights.timeseries.azure.com/preview/demo) an.

## <a name="shape-your-events"></a>Gestalten Ihrer Ereignisse

Sie können die Art überprüfen, in der Sie Ereignisse an Time Series Insights senden. Idealerweise werden Ihrer Ereignisse gut und effizient denormalisiert.

Eine gute Faustregel ist:

* Speichern Sie Metadaten in Ihrem Zeitreihenmodell.
* Stellen Sie sicher, dass Time Series-Modus, Instanzfelder und Ereignisse nur erforderliche Informationen enthalten, z. B. eine Time Series-ID oder eine Timestamp-Eigenschaft.

Weitere Informationen finden Sie unter [Unterstützte JSON-Formen](./time-series-insights-send-events.md#supported-json-shapes).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Nächste Schritte

- Planen Sie mithilfe des Artikels [Einführung in Azure Advisor](../advisor/advisor-overview.md) Ihre Konfigurationseinstellungen für die Geschäftswiederherstellung.
- Weitere Informationen zu [Speicherung und Dateneingang](./time-series-insights-update-storage-ingress.md) finden Sie in der Time Series Insights-Vorschauversion.
- Weitere Informationen zu [Datenmodellierung](./time-series-insights-update-tsm.md) finden Sie in der Time Series Insights-Vorschauversion.
