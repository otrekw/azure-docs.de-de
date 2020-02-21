---
title: HTTP-Ausgabebindungen in Azure Functions
description: Erfahren Sie, wie Sie HTTP-Antworten in Azure Functions zurückgeben.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: a25658677e436edf4d001599bb4981f527016596
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210683"
---
# <a name="azure-functions-http-output-bindings"></a>HTTP-Ausgabebindungen in Azure Functions

Verwenden Sie die HTTP-Ausgabebindung, um eine Antwort an den Absender der HTTP-Anforderung zu senden. Diese Bindung erfordert einen HTTP-Trigger und ermöglicht Ihnen, die Antwort, die der Anforderung des Triggers zugeordnet ist, benutzerdefiniert anzupassen.

Der Standardrückgabewert für eine HTTP-ausgelöste Funktion ist:

- `HTTP 204 No Content` bei leerem Hauptteil in Functions 2.x und höher
- `HTTP 200 OK` bei leerem Hauptteil in Functions 1.x

## <a name="configuration"></a>Konfiguration

Die folgende Tabelle gibt Aufschluss über die Bindungskonfigurationseigenschaften, die Sie in der Datei *function.json* festlegen. Für C#-Klassenbibliotheken gibt es keine Attributeigenschaften, die den folgenden Eigenschaften der Datei *function.json* entsprechen.

|Eigenschaft  |Beschreibung  |
|---------|---------|
| **type** |Muss auf `http` festgelegt sein. |
| **direction** | Muss auf `out` festgelegt sein. |
| **name** | Der Variablenname, der im Funktionscode für die Antwort verwendet wird, oder `$return` für die Verwendung des Rückgabewerts. |

## <a name="usage"></a>Verwendung

Verwenden Sie zum Senden einer HTTP-Antwort die Antwortmuster des Sprachstandards. Legen Sie in C# oder im C#-Skript den Funktionsrückgabetyp auf `IActionResult` oder `Task<IActionResult>` fest. In C# ist kein Attribut des Rückgabewerts erforderlich.

Beispielantworten finden Sie im [Triggerbeispiel](./functions-bindings-http-webhook-trigger.md#example).

## <a name="hostjson-settings"></a>Einstellungen für „host.json“

In diesem Abschnitt werden die verfügbaren globalen Konfigurationseinstellungen für diese Bindung in Version 2.x und höheren Versionen beschrieben. Die nachfolgende Beispieldatei „host.json“ enthält nur die Einstellungen für Version 2.x und höhere Versionen für diese Bindung. Weitere Informationen zu globalen Konfigurationseinstellungen in Version 2.x und höheren Versionen finden Sie unter [host.json-Referenz für Azure Functions 2.x](functions-host-json.md).

> [!NOTE]
> Eine Referenz für „host.json“ in Functions 1.x finden Sie unter [host.json-Referenz für Azure Functions 1.x](functions-host-json-v1.md#http).

```json
{
    "extensions": {
        "http": {
            "routePrefix": "api",
            "maxOutstandingRequests": 200,
            "maxConcurrentRequests": 100,
            "dynamicThrottlesEnabled": true,
            "hsts": {
                "isEnabled": true,
                "maxAge": "10"
            },
            "customHeaders": {
                "X-Content-Type-Options": "nosniff"
            }
        }
    }
}
```

|Eigenschaft  |Standard | BESCHREIBUNG |
|---------|---------|---------| 
| customHeaders|none|Ermöglicht das Festlegen benutzerdefinierter Header in der HTTP-Antwort. Im vorherigen Beispiel wird der Antwort der `X-Content-Type-Options`-Header hinzugefügt, um die Inhaltstypermittlung zu vermeiden. |
|dynamicThrottlesEnabled|true<sup>\*</sup>|Bei einer Aktivierung dieser Einstellung überprüft die Pipeline zur Anforderungsverarbeitung regelmäßig Leistungsindikatoren zur Systemleistung wie `connections/threads/processes/memory/cpu/etc`, und wenn einer dieser Leistungsindikatoren einen integrierten Schwellenwert (80 %) übersteigt, werden Anforderungen mit der Antwort `429 "Too Busy"` zurückgewiesen, bis die Leistungsindikatoren wieder ein normales Niveau erreichen.<br/><sup>\*</sup>Der Standardwert in einem Verbrauchstarif ist `true`. Der Standardwert im Tarif „Dedicated“ ist `false`.|
|hsts|Nicht aktiviert|Wenn `isEnabled` auf `true` festgelegt ist, wird das [HSTS-Verhalten (HTTP Strict Transport Security) von .NET Core](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=visual-studio#hsts) erzwungen, wie in der [`HstsOptions`-Klasse](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions?view=aspnetcore-3.0) definiert. Das Beispiel oben legt außerdem die [`maxAge`](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions.maxage?view=aspnetcore-3.0#Microsoft_AspNetCore_HttpsPolicy_HstsOptions_MaxAge)-Eigenschaft auf 10 Tage fest. Folgende Eigenschaften von `hsts` werden unterstützt: <table><tr><th>Eigenschaft</th><th>BESCHREIBUNG</th></tr><tr><td>excludedHosts</td><td>Ein Zeichenfolgenarray mit Hostnamen, für die der HSTS-Header nicht hinzugefügt wird.</td></tr><tr><td>includeSubDomains</td><td>Boolescher Wert, der angibt, ob der „includeSubDomain“-Parameter des „Strict-Transport-Security“-Headers aktiviert wurde.</td></tr><tr><td>maxAge</td><td>Eine Zeichenfolge, die den „max-age“-Parameter des „Strict-Transport-Security“-Headers definiert.</td></tr><tr><td>preload</td><td>Boolescher Wert, der angibt, ob der „preload“-Parameter des „Strict-Transport-Security“-Headers aktiviert ist.</td></tr></table>|
|maxConcurrentRequests|100<sup>\*</sup>|Die maximale Anzahl von HTTP-Funktionen, die parallel ausgeführt werden. Mit diesem Wert können Sie die Parallelität steuern und somit die Verwaltung der Ressourcenverwendung vereinfachen. Beispielsweise könnten Sie über eine HTTP-Funktion verfügen, die eine große Zahl von Systemressourcen (Speicher/CPU/Sockets) verbraucht und daher Probleme verursacht, wenn die Parallelität zu hoch ist. Oder eine Funktion führt ausgehende Anforderungen an einen Dienst eines Drittanbieters durch, und die Rate dieser Aufrufe muss eingeschränkt werden. In diesen Fällen kann eine Drosselung hilfreich sein. <br/><sup>*</sup>Der Standardwert für einen Verbrauchstarif ist 100. Der Standardwert im Tarif „Dedicated“ ist unbegrenzt (`-1`).|
|maxOutstandingRequests|200<sup>\*</sup>|Die maximale Anzahl ausstehender Anforderungen, die zu einem beliebigen Zeitpunkt gespeichert werden. Dieser Grenzwert umfasst Anforderungen in der Warteschlange, deren Ausführung aber noch nicht gestartet ist, sowie alle laufenden Ausführungen. Alle eingehenden Anforderungen über diesem Grenzwert werden mit der Antwort 429 „Ausgelastet“ zurückgewiesen. Das ermöglicht es dem Aufrufer zeitbasierte Strategien für Wiederholungsversuche einzusetzen, und Sie erhalten damit die Möglichkeit, die maximalen Wartezeiten für Anforderungen zu steuern. Damit wird nur das Queuing gesteuert, das innerhalb des Ausführungspfads des Skripthosts auftritt. Andere Warteschlangen, z.B. die ASP.NET-Anforderungswarteschlange, sind von dieser Einstellung nicht betroffen und werden weiterhin verwendet. <br/><sup>\*</sup>Der Standardwert für einen Verbrauchstarif ist 200. Der Standardwert im Tarif „Dedicated“ ist unbegrenzt (`-1`).|
|routePrefix|api|Das Routenpräfix, das für alle Routen gilt. Verwenden Sie eine leere Zeichenfolge, um das Standardpräfix zu entfernen. |

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion aus einer HTTP-Anforderung](./functions-bindings-http-webhook-trigger.md)