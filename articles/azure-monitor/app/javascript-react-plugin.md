---
title: React-Plug-In für das Application Insights-JavaScript-SDK
description: Hier erfahren Sie, wie Sie das React-Plug-In für das Application Insights-JavaScript-SDK installieren und verwenden.
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: 3a11f77384c520bed9824841269be4ad998adba4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90056199"
---
# <a name="react-plugin-for-application-insights-javascript-sdk"></a>React-Plug-In für das Application Insights-JavaScript-SDK

Das React-Plug-In für das Application Insights-JavaScript-SDK ermöglicht Folgendes:

- Nachverfolgung von Routenänderungen
- Nutzungsstatistiken für React-Komponenten

## <a name="getting-started"></a>Erste Schritte

Installieren Sie das npm-Paket:

```bash

npm install @microsoft/applicationinsights-react-js

```

## <a name="basic-usage"></a>Grundlegende Verwendung

```javascript
import React from 'react';
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { ReactPlugin, withAITracking } from '@microsoft/applicationinsights-react-js';
import { createBrowserHistory } from "history";

const browserHistory = createBrowserHistory({ basename: '' });
var reactPlugin = new ReactPlugin();
var appInsights = new ApplicationInsights({
    config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [reactPlugin],
        extensionConfig: {
          [reactPlugin.identifier]: { history: browserHistory }
        }
    }
});
appInsights.loadAppInsights();

// To instrument various React components usage tracking, apply the `withAITracking` higher-order
// component function.

class MyComponent extends React.Component {
    ...
}

export default withAITracking(reactPlugin,appInsights, MyComponent);

```

## <a name="configuration"></a>Konfiguration

| Name    | Standard | BESCHREIBUNG                                                                                                    |
|---------|---------|----------------------------------------------------------------------------------------------------------------|
| history | NULL    | Hierbei handelt es sich um den React Router-Verlauf. Weitere Informationen finden Sie in der [Dokumentation für das react-router-Paket](https://reactrouter.com/web/api/history). Weitere Informationen zum Zugriff auf das Verlaufsobjekt außerhalb von Komponenten finden Sie in den [FAQ zu react-router](https://github.com/ReactTraining/react-router/blob/master/FAQ.md#how-do-i-access-the-history-object-outside-of-components).    |

### <a name="react-components-usage-tracking"></a>Nachverfolgung der Nutzung von React-Komponenten

Wenden Sie zum Instrumentieren der Nachverfolgung der Nutzung verschiedener React-Komponenten die Komponentenfunktion höherer Ordnung `withAITracking` an.

Diese misst die Zeit zwischen dem `ComponentDidMount`-Ereignis und dem `ComponentWillUnmount`-Ereignis. Sie zieht hiervon jedoch die Zeit ab, in der der Benutzer inaktiv war, um eine genauere Angabe zu erhalten: `React Component Engaged Time = ComponentWillUnmount timestamp - ComponentDidMount timestamp - idle time`.

Wenn Sie diese Metrik im Azure-Portal anzeigen möchten, müssen Sie zur Application Insights-Ressource navigieren, dort auf die Registerkarte „Metriken“ klicken und die leeren Diagramme so konfigurieren, dass der benutzerdefinierte Metrikname „React-Komponente Nutzungsdauer (Sekunden)“ angezeigt wird. Wählen Sie anschließend die Aggregation (Summe, Durchschnitt etc.) für Ihre Metrik aus und eine Aufteilung nach „Komponentenname“.

![Screenshot: Diagramm, das die benutzerdefinierte Metrik „React-Komponente Nutzungsdauer (Sekunden)“ aufgeteilt nach „Komponentenname“ zeigt](./media/javascript-react-plugin/chart.png)

Sie können auch benutzerdefinierte Abfragen ausführen, um Application Insights-Daten zu unterteilen, um Berichte und Visualisierungen gemäß Ihren Anforderungen zu generieren. Navigieren Sie im Azure-Portal zur Application Insights-Ressource, klicken Sie im oberen Menü der Registerkarte „Übersicht“ auf die Option „Analyse“, und führen Sie Ihre Abfrage aus.

![Screenshot: Ergebnisse der benutzerdefinierten Metrikabfrage](./media/javascript-react-plugin/query.png)

> [!NOTE]
> Es kann bis zu 10 Minuten dauern, bis neue benutzerdefinierte Metriken im Azure-Portal angezeigt werden.

## <a name="sample-app"></a>Beispiel-App

Sehen Sie sich die [Application Insights-React-Demo](https://github.com/Azure-Samples/application-insights-react-demo) an.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum JavaScript SDK finden Sie in der [Dokumentation zum Application Insights JavaScript SDK](javascript.md).
- Informationen zur Kusto-Abfragesprache und zum Abfragen von Daten in Log Analytics finden Sie in der [Übersicht über Protokollabfragen](../../azure-monitor/log-query/log-query-overview.md).
