---
title: Angular-Plug-in für das Application Insights-JavaScript-SDK
description: Hier erfahren Sie, wie Sie das Angular-Plug-In für das Application Insights-JavaScript-SDK installieren und verwenden.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: lagayhar
ms.openlocfilehash: 152ba4b1c8a4e09db0bce759f5b67f577ec5d584
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91843629"
---
# <a name="angular-plugin-for-application-insights-javascript-sdk"></a>Angular-Plug-in für das Application Insights-JavaScript-SDK

Das Angular-Plug-In für das Application Insights-JavaScript-SDK bietet folgende Möglichkeiten:

- Nachverfolgung von Routeränderungen
- Nutzungsstatistiken für Angular-Komponenten

> [!WARNING]
> Das Angular-Plug-In ist nicht kompatibel mit ECMAScript 3 (ES3).

## <a name="getting-started"></a>Erste Schritte

Installieren Sie das npm-Paket:

```bash
npm install @microsoft/applicationinsights-angularplugin-js
```

## <a name="basic-usage"></a>Grundlegende Verwendung

Richten Sie eine Application Insights-Instanz in der Eintragskomponente in Ihrer App ein:

```js
import { Component, OnInit } from '@angular/core';
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { AngularPlugin, AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
    private appInsights;
    constructor(
        private router: Router,
        private angularPluginService: AngularPluginService 
    ){
        var angularPlugin = new AngularPlugin();
        this.angularPluginService.init(angularPlugin, this.router);
        this.appInsights = new ApplicationInsights({ config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [angularPlugin],
        extensionConfig: {
            [angularPlugin.identifier]: { router: this.router }
        }
        } });
    }

    ngOnInit() {
        this.appInsights.loadAppInsights();
    }
}

```

Fügen Sie `AngularPluginService` als Anbieter zur Anbieterliste in der `app.module.ts`-Datei hinzu,um die `trackMetric`-Methode zur Nutzungsnachverfolgung der Angular-Komponente zu verwenden.

```js
import { AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';

@NgModule({
    ...
  providers: [ AngularPluginService ],
})
export class AppModule { }
```

Rufen Sie `trackMetric` in der `ngOnDestroy`-Methode dieser Komponente auf, um die Lebensdauer einer Komponente zu verfolgen. Wenn die Komponente zerstört wird, löst sie ein `trackMetric`-Ereignis aus, das die Zeit sendet, die der Benutzer auf der Seite verbracht hat, sowie den Komponentennamen.

```js
import { Component, OnDestroy, HostListener } from '@angular/core';
import { AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent implements OnDestroy {

  constructor(private angularPluginService: AngularPluginService) {}

  @HostListener('window:beforeunload')
  ngOnDestroy() {
    this.angularPluginService.trackMetric();
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum JavaScript SDK finden Sie in der [Dokumentation zum Application Insights JavaScript SDK](javascript.md).
- [Angular-Plug-in auf GitHub](https://github.com/microsoft/ApplicationInsights-JS/tree/master/extensions/applicationinsights-angularplugin-js)