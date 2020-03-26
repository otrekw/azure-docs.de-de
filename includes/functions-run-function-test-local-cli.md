---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: d53c41752d57a27ebea9bd60f7e723dab1e7308a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "78190850"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Führen Sie Ihre Funktion aus, indem Sie den lokalen Azure Functions-Runtimehost im Ordner *LocalFunctionProj* starten:

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-javascript,programming-language-python"
```
func start
```
::: zone-end

::: zone pivot="programming-language-typescript"
```
npm install
npm start
```
::: zone-end

Gegen Ende der Ausgabe sollten die folgenden Zeilen angezeigt werden: 

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...

</pre>

>[!NOTE]  
> Sollte „HttpExample“ nicht wie unten dargestellt angezeigt werden, haben Sie den Host wahrscheinlich im Ordner *HttpExample* gestartet. Verwenden Sie in diesem Fall **STRG**+**C**, um den Host zu beenden. Navigieren Sie anschließend zum übergeordneten Ordner *LocalFunctionProj*, und führen Sie den vorherigen Befehl erneut aus.

Kopieren Sie die URL Ihrer `HttpExample`-Funktion aus dieser Ausgabe in einen Browser, und fügen Sie die Abfragezeichenfolge `?name=<your-name>` an. Die vollständige URL lautet dann wie folgt: `http://localhost:7071/api/HttpExample?name=Functions`. Im Browser sollte eine Meldung wie `Hello Functions` angezeigt werden:

![Ergebnis der lokal im Browser ausgeführten Funktion](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

Im Terminal, in dem Sie `func start` ausgeführt haben, wird beim Senden von Anforderungen auch die Protokollausgabe angezeigt.

Wenn Sie so weit sind, drücken Sie **STRG**+**C**, und wählen Sie `y` aus, um den Funktionshost zu beenden.