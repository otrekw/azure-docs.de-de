---
title: Erstellen der ersten Funktion in Azure mit Kotlin und Maven
description: Hier erfahren Sie, wie Sie mit Kotlin und Maven eine einfache Funktion mit HTTP-Trigger erstellen und für Azure veröffentlichen.
author: dglover
ms.service: azure-functions
ms.topic: quickstart
ms.date: 03/25/2020
ms.author: dglover
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: acb4290a90532b38f44a957e33c69f7d2b3f11f8
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92104817"
---
# <a name="quickstart-create-your-first-function-with-kotlin-and-maven"></a>Schnellstart: Erstellen Ihrer ersten Funktion mit Kotlin und Maven

Dieser Artikel führt Sie durch die Verwendung des Maven-Befehlszeilentools zum Erstellen und Veröffentlichen eines Kotlin-Funktionsprojekts in Azure Functions. Wenn Sie fertig sind, wird Ihr Funktionscode für den [Verbrauchstarif](functions-scale.md#consumption-plan) in Azure ausgeführt und kann mithilfe einer HTTP-Anforderung ausgelöst werden.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Um Funktionen mit Kotlin entwickeln zu können, muss Folgendes installiert sein:

- [Java Developer Kit](/azure/developer/java/fundamentals/java-jdk-long-term-support), Version 8
- [Apache Maven](https://maven.apache.org), Version 3.0 oder höher
- [Azure-Befehlszeilenschnittstelle](/cli/azure)
- [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.6.666 oder höher

> [!IMPORTANT]
> Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.

## <a name="generate-a-new-functions-project"></a>Generieren eines neuen Functions-Projekts

Führen Sie in einem leeren Ordner den folgenden Befehl aus, um das Functions-Projekt über einen [Maven-Archetyp](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) zu generieren.

# <a name="bash"></a>[Bash](#tab/bash)
```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```

> [!NOTE]
> Sollten bei der Befehlsausführung Probleme auftreten, überprüfen Sie, welche Version von `maven-archetype-plugin` verwendet wird. Da Sie den Befehl in einem leeren Verzeichnis ohne Datei vom Typ `.pom` ausführen, wird möglicherweise versucht, ein Plug-In mit der älteren Version aus `~/.m2/repository/org/apache/maven/plugins/maven-archetype-plugin` auszuführen, wenn Sie ein Upgrade für eine ältere Maven-Version ausgeführt haben. Löschen Sie in diesem Fall das Verzeichnis `maven-archetype-plugin`, und führen Sie den Befehl erneut aus.

# <a name="powershell"></a>[PowerShell](#tab/powershell)
```powershell
mvn archetype:generate `
    "-DarchetypeGroupId=com.microsoft.azure" `
    "-DarchetypeArtifactId=azure-functions-kotlin-archetype"
```

# <a name="cmd"></a>[Cmd](#tab/cmd)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```
---

Maven fordert Sie zur Eingabe der Werte auf, die erforderlich sind, um die Generierung des Projekts abzuschließen. Informationen zu den Werten _groupId_, _artifactId_ und _version_ finden Sie in der Referenz [Maven-Benennungskonventionen](https://maven.apache.org/guides/mini/guide-naming-conventions.html). Der Wert _appName_ muss in Azure eindeutig sein. Deshalb generiert Maven standardmäßig einen App-Namen basierend auf den zuvor eingegebenen Wert _artifactId_. Der Wert _packageName_ bestimmt das Kotlin-Paket für den generierten Funktionscode.

Die weiter unten angegebenen Bezeichner `com.fabrikam.functions` und `fabrikam-functions` dienen als Beispiele und verbessern die Lesbarkeit der weiteren Schritte in dieser Schnellstartanleitung. Sie können in diesem Schritt aber auch gerne eigene Werte für Maven angeben.

<pre>
[INFO] Parameter: groupId, Value: com.fabrikam.function
[INFO] Parameter: artifactId, Value: fabrikam-function
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.fabrikam.function
[INFO] Parameter: packageInPathFormat, Value: com/fabrikam/function
[INFO] Parameter: appName, Value: fabrikam-function-20190524171507291
[INFO] Parameter: resourceGroup, Value: java-functions-group
[INFO] Parameter: package, Value: com.fabrikam.function
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: groupId, Value: com.fabrikam.function
[INFO] Parameter: appRegion, Value: westus
[INFO] Parameter: artifactId, Value: fabrikam-function
</pre>

Maven erstellt die Projektdateien in einem neuen Ordner mit dem Namen _artifactId_ (in diesem Beispiel: `fabrikam-functions`). Bei dem generierten ausführungsbereiten Code im Projekt handelt es sich um eine einfache, [durch HTTP ausgelöste](./functions-bindings-http-webhook.md) Funktion, die den Text der Anforderung ausgibt:

```kotlin
class Function {

    /**
     * This function listens at endpoint "/api/HttpTrigger-Java". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/HttpTrigger-Java&code={your function key}
     * 2. curl "{your host}/api/HttpTrigger-Java?name=HTTP%20Query&code={your function key}"
     * Function Key is not needed when running locally, it is used to invoke function deployed to Azure.
     * More details: https://aka.ms/functions_authorization_keys
     */
    @FunctionName("HttpTrigger-Java")
    fun run(
            @HttpTrigger(
                    name = "req",
                    methods = [HttpMethod.GET, HttpMethod.POST],
                    authLevel = AuthorizationLevel.FUNCTION) request: HttpRequestMessage<Optional<String>>,
            context: ExecutionContext): HttpResponseMessage {

        context.logger.info("HTTP trigger processed a ${request.httpMethod.name} request.")

        val query = request.queryParameters["name"]
        val name = request.body.orElse(query)

        name?.let {
            return request
                    .createResponseBuilder(HttpStatus.OK)
                    .body("Hello, $name!")
                    .build()
        }

        return request
                .createResponseBuilder(HttpStatus.BAD_REQUEST)
                .body("Please pass a name on the query string or in the request body")
                .build()
    }
}
```

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Ändern Sie das Verzeichnis in den neu erstellten Projektordner. Erstellen Sie dann mit Maven die Funktion, und führen Sie sie aus:

```
cd fabrikam-function
mvn clean package
mvn azure-functions:run
```

> [!NOTE]
> Sollte bei Ihnen mit Java 9 die Ausnahme `javax.xml.bind.JAXBException` auftreten, sehen Sie sich die Problemumgehung auf [GitHub](https://github.com/jOOQ/jOOQ/issues/6477) an.

Wenn die Funktion lokal auf Ihrem System ausgeführt wird und für HTTP-Anforderungen bereit ist, wird die folgende Ausgabe angezeigt:

<pre>
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpTrigger-Java: [GET,POST] http://localhost:7071/api/HttpTrigger-Java
</pre>

Lösen Sie die Funktion über die Befehlszeile aus. Verwenden Sie dazu cURL in einem neuen Terminalfenster:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/HttpTrigger-Java
```

<pre>
Hello LocalFunction!
</pre>

Verwenden Sie `Ctrl-C` im Terminal, um den Funktionscode anzuhalten.

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

Bei dem Bereitstellungsprozess in Azure Functions werden die Anmeldeinformationen aus der Azure CLI verwendet. [Melden Sie sich über die Azure CLI an](/cli/azure/authenticate-azure-cli?view=azure-cli-latest), ehe Sie fortfahren.

```azurecli
az login
```

Stellen Sie Ihren Code mit `azure-functions:deploy` als Maven-Ziel in einer neuen Funktionen-App bereit.

> [!NOTE]
> Wenn Sie Ihre Funktionen-App mit Visual Studio Code bereitstellen, vergessen Sie nicht, ein kostenpflichtiges Abonnement auszuwählen. Andernfalls tritt ein Fehler auf. Das Abonnement wird auf der linken Seite der IDE angezeigt.

```
mvn azure-functions:deploy
```

Wenn die Bereitstellung abgeschlossen ist, wird die URL angezeigt, mit der Sie auf Ihre Azure-Funktionen-App zugreifen können:

<pre>
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
</pre>

Testen Sie die in Azure ausgeführte Funktionen-App mithilfe von `cURL`. Ersetzen Sie die URL im folgenden Beispiel durch die bereitgestellte URL für Ihre eigene Funktions-App aus dem vorherigen Schritt.

> [!NOTE]
> Legen Sie die **Zugriffsrechte** auf `Anonymous` fest. Bei Verwendung der Standardebene `Function` müssen Sie in Anforderungen für den Zugriff auf den Funktionsendpunkt den [Funktionsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys) angeben.

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/HttpTrigger-Java -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Vornehmen von Änderungen und erneutes Bereitstellen

Bearbeiten Sie die Quelldatei `src/main.../Function.java` im erstellten Projekt, um den von Ihrer Funktions-App zurückgegebenen Text zu ändern. Ändern Sie diese Zeile:

```kotlin
return request
        .createResponseBuilder(HttpStatus.OK)
        .body("Hello, $name!")
        .build()
```

Geänderter Code:

```kotlin
return request
        .createResponseBuilder(HttpStatus.OK)
        .body("Hi, $name!")
        .build()
```

Speichern Sie die Änderungen, und führen Sie eine erneute Bereitstellung durch, indem Sie wie zuvor `azure-functions:deploy` über das Terminal ausführen. Die Funktions-App wird aktualisiert, und die folgende Anforderung:

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

Aktualisierte Ausgabe:

<pre>
Hi, AzureFunctionsTest
</pre>


## <a name="reference-bindings"></a>Verweisbindungen

Wenn Sie [Functions-Trigger und -Bindungen](functions-triggers-bindings.md) verwenden möchten, bei denen es sich nicht um HTTP-Trigger oder Zeitgebertrigger handelt, müssen Sie Bindungserweiterungen installieren. Dies ist in diesem Artikel nicht erforderlich. Bei Verwendung anderer Bindungstypen müssen Sie jedoch wissen, wie Sie Erweiterungen aktivieren.

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Kotlin-Funktions-App mit einem einfachen HTTP-Trigger erstellt und für Azure Functions bereitgestellt.

- Weitere Informationen zum Entwickeln von Java- und Kotlin-Funktionen finden Sie im [Java-Entwicklerhandbuch für Azure Functions](functions-reference-java.md).
- Fügen Sie mit `azure-functions:add` als Maven-Ziel zusätzliche Funktionen mit verschiedenen Triggern zu Ihrem Projekt hinzu.
- Schreiben und debuggen Sie Funktionen lokal mit [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) und [Eclipse](functions-create-maven-eclipse.md). 
- Debuggen Sie in Azure bereitgestellte Funktionen mit Visual Studio Code. Eine entsprechende Anleitung finden Sie in der Visual Studio Code-Dokumentation für [serverlose Java-Anwendungen](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud).