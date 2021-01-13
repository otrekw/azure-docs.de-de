---
title: Umsteigen von „Express.js“ auf Azure Functions
description: Erfahren Sie, wie Sie Express.js-Endpunkte in Azure Functions umgestalten.
author: craigshoemaker
ms.topic: conceptual
ms.date: 07/31/2020
ms.author: cshoe
ms.openlocfilehash: 266df5371ff5f47526fa9d6567c62e31d51ebb05
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87810223"
---
# <a name="shifting-from-expressjs-to-azure-functions"></a>Umsteigen von „Express.js“ auf Azure Functions

„Express.js“ gehört zu den beliebtesten Node.js-Frameworks für Webentwickler und bleibt weiterhin eine hervorragende Wahl zum Entwickeln von Apps, die API-Endpunkte bereitstellen.

Wenn Sie Code zu einer serverlosen Architektur migrieren, wirkt sich das Refactoring von Express.js-Endpunkten auf die folgenden Bereiche aus:

- **Middleware:** „Express.js“ bietet eine robuste Sammlung von Middleware. Viele Middlewaremodule sind nicht mehr für Azure Functions und Funktionen von [Azure API Management](../api-management/api-management-key-concepts.md) erforderlich. Stellen Sie vor der Migration von Endpunkten sicher, dass Sie die gesamte Logik, die von wichtiger Middleware verarbeitet wird, replizieren oder ersetzen können.

- **Abweichende APIs**: Die API, die zur Verarbeitung von Anforderungen und Antworten verwendet wird, unterscheidet sich zwischen Azure Functions und „Express.js“. Im folgenden Beispiel werden die erforderlichen Änderungen erläutert.

- **Standardroute**: Standardmäßig werden Azure Functions-Endpunkte unter der `api`-Route bereitgestellt. Routingregeln können über [`routePrefix` in der _Datei_ host.json](./functions-bindings-http-webhook-output.md#hostjson-settings) konfiguriert werden.

- **Konfiguration und Konventionen**: Eine Functions-App verwendet die Datei _function.json_, um HTTP-Verben sowie Sicherheitsrichtlinien zu definieren und kann die [Eingabe und Ausgabe](./functions-triggers-bindings.md) der Funktion konfigurieren. Standardmäßig definiert der Ordnername, der die Funktionsdateien enthält, den Endpunktnamen, aber Sie können den Namen über die Eigenschaft `route` in der Datei [function.json](./functions-bindings-http-webhook-trigger.md#customize-the-http-endpoint) ändern.

> [!TIP]
> Weitere Informationen finden Sie im interaktiven Tutorial [Refactoring von „Node.js“ und Express-APIs in serverlose APIs mit Azure Functions](/learn/modules/shift-nodejs-express-apis-serverless/).

## <a name="example"></a>Beispiel

### <a name="expressjs"></a>Express.js

Das folgende Beispiel zeigt einen typischen `GET`-Endpunkt von „Express.js“.

```javascript
// server.js
app.get('/hello', (req, res) => {
  try {
    res.send("Success!");
  } catch(error) {
    const err = JSON.stringify(error);
    res.status(500).send(`Request error. ${err}`);
  }
});
```

Wenn eine `GET`-Anforderung an `/hello` gesendet wird, wird eine `HTTP 200`-Antwort zurückgegeben, die `Success` enthält. Wenn für den Endpunkt ein Fehler auftritt, ist die Antwort `HTTP 500` mit den Fehlerdetails.

### <a name="azure-functions"></a>Azure Functions

Azure Functions organisiert Konfigurations- und Codedateien in einem einzelnen Ordner für jede Funktion. Standardmäßig gibt der Name des Ordners den Funktionsnamen vor.

Beispielsweise verfügt eine Funktion mit dem Namen `hello` über einen Ordner mit den folgenden Dateien.

``` files
| - hello
|  - function.json
|  - index.js
```

Im folgenden Beispiel wird das gleiche Ergebnis wie der Express.js-Endpunkt oben implementiert, jedoch mit Azure Functions.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// hello/index.js
module.exports = async function (context, req) {
  try {
    context.res = { body: "Success!" };
  } catch(error) {
    const err = JSON.stringify(error);
    context.res = {
      status: 500,
      body: `Request error. ${err}`
    };
  }
};
```

# <a name="typescript"></a>[TypeScript](#tab/typescript)

```typescript
// hello/index.ts
import { AzureFunction, Context, HttpRequest } from "@azure/functions";

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
  try {
    context.res = { body: "Success!" };
  } catch (error) {
    const err = JSON.stringify(error);
    context.res = {
      status: 500,
      body: `Request error. ${err}`,
    };
  }
};

export default httpTrigger;
```

---

Bei der Umstellung auf Functions werden die folgenden Änderungen vorgenommen:

- **Modul:** Der Funktionscode wird als JavaScript-Modul implementiert.

- **Kontext und Antwortobjekt**: [`context`](./functions-reference-node.md#context-object) ermöglicht Ihnen die Kommunikation mit der Laufzeit der Funktion. Aus dem Kontext können Sie Anforderungsdaten lesen und die Antwort der Funktion festlegen. Synchroner Code erfordert, dass Sie `context.done()` zum Abschluss der Ausführung aufrufen, während `asyc`-Funktionen die Anforderung implizit auflösen.

- **Benennungskonvention**: Der Ordnername, der zur Speicherung der Azure Functions-Dateien verwendet wird, wird standardmäßig als Endpunktname verwendet (dies kann in der Datei [function.json](./functions-bindings-http-webhook-trigger.md#customize-the-http-endpoint) überschrieben werden).

- **Konfiguration:** Sie definieren die HTTP-Verben in der Datei [function.json](./functions-bindings-http-webhook-trigger.md#customize-the-http-endpoint), z. B. mit `POST` oder `PUT`.

Die folgende Datei _function.json_ enthält Konfigurationsinformationen für die Funktion.

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

Wenn Sie `get` im `methods`-Array definieren, steht die Funktion für HTTP-`GET`-Anforderungen zur Verfügung. Wenn Ihre API `POST`-Supportanforderungen akzeptieren soll, können Sie dem Array auch `post` hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie im interaktiven Tutorial [Refactoring von „Node.js“ und Express-APIs in serverlose APIs mit Azure Functions](/learn/modules/shift-nodejs-express-apis-serverless/).