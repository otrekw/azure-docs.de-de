---
title: Veröffentlichen einer Funktion für Azure mithilfe von Java und Gradle
description: Hier erfahren Sie, wie Sie mit Java und Gradle eine einfache Funktion mit HTTP-Trigger erstellen und für Azure veröffentlichen.
author: KarlErickson
ms.author: karler
ms.topic: how-to
ms.date: 04/08/2020
ms.openlocfilehash: 5e18e035bd237fd489b715986e58d7ede726348d
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886601"
---
# <a name="use-java-and-gradle-to-create-and-publish-a-function-to-azure"></a>Erstellen und Veröffentlichen einer Funktion für Azure mithilfe von Java und Gradle

In diesem Artikel erfahren Sie, wie Sie mit dem Gradle-Befehlszeilentool ein Java-Funktionsprojekt erstellen und für Azure Functions veröffentlichen. Der fertige Funktionscode wird dann in Azure im Rahmen eines [serverlosen Hostingplans](functions-scale.md#consumption-plan) ausgeführt und durch eine HTTP-Anforderung ausgelöst. 

> [!NOTE]
> Sollte Gradle nicht Ihr bevorzugtes Entwicklungstool sein, stehen ähnliche Tutorials mit [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java), [IntelliJ IDEA](/azure/java/intellij/azure-toolkit-for-intellij-quickstart-functions) und [VS Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java) für Java-Entwickler zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Um Funktionen mit Java zu entwickeln, muss Folgendes installiert sein:

- [Java Developer Kit](https://aka.ms/azure-jdks), Version 8
- [Azure-Befehlszeilenschnittstelle]
- [Azure Functions Core Tools](./functions-run-local.md#v2), Version 2.6.666 oder höher
- [Gradle](https://gradle.org/) ab Version 4.10

Außerdem benötigen Sie ein aktives Azure-Abonnement. [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

> [!IMPORTANT]
> Damit Sie diesen Schnellstart durchführen können, muss die Umgebungsvariable JAVA_HOME auf den Installationsspeicherort des JDK festgelegt sein.

## <a name="prepare-a-functions-project"></a>Vorbereiten eines Functions-Projekts

Verwenden Sie den folgenden Befehl, um das Beispielprojekt zu klonen:

```bash
git clone https://github.com/Azure-Samples/azure-functions-samples-java.git
cd azure-functions-samples-java/
```

Öffnen Sie `build.gradle`, und ändern Sie den App-Namen (`appName`) im folgenden Abschnitt in einen eindeutigen Namen, um Domänennamenskonflikte bei der Bereitstellung in Azure zu vermeiden. 

```gradle
azurefunctions {
    resourceGroup = 'java-functions-group'
    appName = 'azure-functions-sample-demo'
    pricingTier = 'Consumption'
    region = 'westus'
    runtime {
      os = 'windows'
    }
    localDebug = "transport=dt_socket,server=y,suspend=n,address=5005"
}
```

Öffnen Sie die neue Datei „Function.java“ unter *src/main/java* in einem Text-Editor, und überprüfen Sie den generierten Code. Bei diesem Code handelt es sich um eine Funktion mit [HTTP-Trigger](functions-bindings-http-webhook.md), die den Text der Anforderung zurückgibt. 

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=generate-project)

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Führen Sie den folgenden Befehl aus, um das Funktionsprojekt zu erstellen und auszuführen:

```bash
gradle jar --info
gradle azureFunctionsRun
```
Wenn Sie das Projekt lokal ausführen, wird eine Ausgabe wie die folgende von Azure Functions Core Tools angezeigt:

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

    HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...
</pre>

Lösen Sie die Funktion über die Befehlszeile aus. Verwenden Sie dazu den folgenden cURL-Befehl in einem neuen Terminalfenster:

```bash
curl -w "\n" http://localhost:7071/api/HttpExample --data AzureFunctions
```

Erwartete Ausgabe:

<pre>
Hello AzureFunctions!
</pre>

Der [Funktionsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys) ist bei lokaler Ausführung nicht erforderlich.  
Verwenden Sie `Ctrl+C` im Terminal, um den Funktionscode anzuhalten.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=local-run)

## <a name="deploy-the-function-to-azure"></a>Bereitstellen der Funktion in Azure

Wenn Sie Ihre Funktions-App erstmals bereitstellen, werden in Azure eine Funktions-App und zugehörige Ressourcen erstellt. Vor der Bereitstellung müssen Sie sich zunächst mithilfe des Azure CLI-Befehls [az login](/cli/azure/authenticate-azure-cli) bei Ihrem Azure-Abonnement anmelden. 

```azurecli
az login
```

> [!TIP]
> Falls Ihr Konto auf mehrere Abonnements zugreifen kann, verwenden Sie [az account set](/cli/azure/account#az-account-set), um das Standardabonnement für diese Sitzung festzulegen. 

Verwenden Sie den folgenden Befehl, um Ihr Projekt für eine neue Funktions-App bereitzustellen. 

```bash
gradle azureFunctionsDeploy
```

Dadurch werden die folgenden Ressourcen in Azure erstellt, basierend auf den Werten in der Datei build.gradle:

+ Ressourcengruppe – benannt mit der angegebenen Ressourcengruppe (_resourceGroup_).
+ Speicherkonto – von Functions benötigt. Der Name wird nach dem Zufallsprinzip basierend auf den Anforderungen für den Speicherkontonamen generiert.
+ App Service-Plan. Serverloses Hosting mit Verbrauchsplan für Ihre Funktions-App in der angegebenen App-Region (_appRegion_). Der Name wird nach dem Zufallsprinzip generiert.
+ Funktions-App – die Bereitstellungs-und Ausführungseinheit für Ihre Funktionen. Der Name ist Ihr App-Name (_appName_) mit angefügter Zufallszahl. 

Bei der Bereitstellung werden mithilfe von [zip deployment](functions-deployment-technologies.md#zip-deploy) auch die Projektdateien gepackt und für die neue Funktions-App bereitgestellt (mit aktiviertem Modus für die paketbasierte Ausführung).

Da der von uns veröffentlichte HTTP-Trigger `authLevel = AuthorizationLevel.FUNCTION` verwendet, müssen Sie den Funktionsschlüssel abrufen, um den Funktionsendpunkt über HTTP aufrufen zu können. Den Funktionsschlüssel können Sie am einfachsten über das [Azure portal] abrufen.

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=deploy)

## <a name="get-the-http-trigger-url"></a>Abrufen der URL des HTTP-Triggers

Die erforderliche URL zum Auslösen Ihrer Funktion können Sie mit dem Funktionsschlüssel im Azure-Portal abrufen. 

1. Navigieren Sie zum [Azure portal], melden Sie sich an, geben Sie im oberen Seitenbereich unter **Suche** den App-Namen (_appName_) Ihrer Funktions-App ein, und drücken Sie die EINGABETASTE.
 
1. Erweitern Sie in Ihrer Funktions-App **Funktionen (schreibgeschützt)** , wählen Sie Ihre Funktion aus, und wählen Sie anschließend rechts oben die Option **</> Funktions-URL abrufen** aus. 

    ![Kopieren der URL der Funktion aus dem Azure-Portal](./media/functions-create-java-maven/get-function-url-portal.png)

1. Wählen Sie **default (Function key)** (Standard (Funktionsschlüssel)) und anschließend **Kopieren** aus. 

Nun können Sie mithilfe der kopierte URL auf Ihre Funktion zugreifen.

## <a name="verify-the-function-in-azure"></a>Überprüfen der Funktion in Azure

Die in Azure ausgeführte Funktions-App kann mithilfe von `cURL` überprüft werden. Ersetzen Sie hierzu die URL aus dem folgenden Beispiel durch die URL, die Sie aus dem Portal kopiert haben.

```console
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpExample?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Dadurch wird eine POST-Anforderung mit `AzureFunctions` an den Funktionsendpunkt gesendet. Die Antwort sieht wie folgt aus:

<pre>
Hello AzureFunctions!
</pre>

> [!div class="nextstepaction"]
> [Ich bin auf ein Problem gestoßen](https://www.research.net/r/javae2e?tutorial=functions-maven-quickstart&step=verify-deployment)

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein Java-Funktionsprojekt mit einer per HTTP ausgelösten Funktion erstellt, auf Ihrem lokalen Computer ausgeführt und für Azure bereitgestellt. Erweitern Sie nun Ihre Funktion durch...

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangen-Ausgabebindung](functions-add-output-binding-storage-queue-java.md)


[Azure-Befehlszeilenschnittstelle]: /cli/azure
[Azure portal]: https://portal.azure.com
