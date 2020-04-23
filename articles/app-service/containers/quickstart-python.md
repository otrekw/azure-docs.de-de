---
title: 'Schnellstart: Erstellen einer Linux-Python-App'
description: Führen Sie erste Schritte mit Linux-Apps in Azure App Service aus, indem Sie Ihre erste Python-App in einem Linux-Container in App Service bereitstellen.
ms.topic: quickstart
ms.date: 04/03/2020
ms.custom: seo-python-october2019, cli-validate
experimental: true
experiment_id: 01a9132f-eaab-4c
ms.openlocfilehash: 7f8d21cbccc1a6eec7f62ab5d129f3f0637b8d13
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085126"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Schnellstart: Erstellen einer Python-App in Azure App Service für Linux

In diesem Schnellstart stellen Sie eine Python-Web-App für [App Service für Linux](app-service-linux-intro.md) bereit. Dies ist der überaus skalierbare Webhostingdienst von Azure mit automatisiertem Patchen. Sie verwenden auf einem Mac-, Linux- oder Windows-Computer die lokale [Azure-Befehlszeilenschnittstelle (CLI)](/cli/azure/install-azure-cli). Die Web-App, die Sie konfigurieren, nutzt einen kostenlosen App-Service-Tarif, sodass Ihnen im Laufe dieses Artikels keine Kosten entstehen.

Wenn Sie Apps lieber über eine IDE bereitstellen möchten, lesen Sie [Bereitstellen von Python-Apps in App Service aus Visual Studio Code](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="prerequisites"></a>Voraussetzungen

- Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- <a href="https://www.python.org/downloads/" target="_blank">Python 3.7</a> (Python 3.6 wird ebenfalls unterstützt)
- <a href="https://git-scm.com/downloads" target="_blank">Git-Client</a>
- <a href="https://docs.microsoft.com/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.0.80 oder höher. Führen Sie `az --version` aus, um Ihre Version zu überprüfen.

## <a name="download-the-sample"></a>Herunterladen des Beispiels

Führen Sie in einem Terminalfenster den folgenden Befehl aus, um die Beispielanwendung auf Ihren lokalen Computer zu klonen. 

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Wechseln Sie dann zu diesem Ordner:

```terminal
cd python-docs-hello-world
```

Das Repository enthält die Datei *application.py*, die App Service informiert, dass der Code eine Flask-App enthält. Weitere Informationen finden Sie unter [Startprozess und Anpassungen für Container](how-to-configure-python.md).

## <a name="run-the-sample"></a>Ausführen des Beispiels

Öffnen Sie ein Terminalfenster, und verwenden Sie die folgenden Befehle (entsprechend Ihrem Betriebssystem), um die erforderlichen Abhängigkeiten zu installieren und den integrierten Entwicklungsserver zu starten. 

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
export FLASK_APP=application.py
flask run
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
SET FLASK_APP=application.py
flask run
```

---

Öffnen Sie einen Webbrowser, und navigieren Sie zu der Beispiel-App unter `http://localhost:5000/`. Die App zeigt die Meldung **Hello World!** .

![Lokales Ausführen einer Python-Beispiel-App](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)

Drücken Sie in Ihrem Terminalfenster **STRG**+**C**, um den Webserver zu beenden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Die Azure CLI bietet viele praktische Befehle, die Sie über ein lokales Terminal ausführen, um Azure-Ressourcen über die Befehlszeile bereitzustellen und zu verwalten. Sie können mithilfe von Befehlen die gleichen Aufgaben wie über das Azure-Portal in einem Browser ausführen. Sie können auch CLI-Befehle in Skripts verwenden, um Verwaltungsprozesse zu automatisieren.

Um Azure-Befehle in der Azure CLI auszuführen, müssen Sie sich zunächst mit dem Befehl `az login` anmelden. Dieser Befehl öffnet einen Browser zum Erfassen Ihrer Anmeldeinformationen.

```azurecli
az login
```

## <a name="deploy-the-sample"></a>Bereitstellen des Beispiels

Mit dem Befehl [`az webapp up`](/cli/azure/webapp#az-webapp-up) wird die Web-App in App Service erstellt und Ihr Code bereitgestellt.

Führen Sie im Ordner *python-docs-hello-world*, der den Beispielcode enthält, den folgenden Befehl `az webapp up` aus. Ersetzen Sie `<app-name>` durch einen global eindeutigen App-Namen (*gültige Zeichen sind `a-z`, `0-9` und `-`* ).


```azurecli
az webapp up --sku F1 -n <app-name>
```

Mit dem Argument `--sku F1` wird die Web-App im Tarif „Free“ erstellt. Sie können dieses Argument weglassen, um stattdessen einen Premium-Tarif zu verwenden. Dieser verursacht jedoch stündlich Kosten.

Außerdem können Sie optional das Argument `-l <location-name>` einschließen. `<location_name>` ist hierbei eine Azure-Region wie **centralus**, **eastasia**, **westeurope**, **koreasouth**, **brazilsouth**, **centralindia** und so weiter. Sie können eine Liste der zulässigen Regionen für Ihr Azure-Konto abrufen, indem Sie den Befehl [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) ausführen.

Die vollständige Ausführung des Befehls `az webapp up` kann mehrere Minuten dauern. Während der Ausführung werden Informationen wie im folgenden Beispiel angezeigt. `<app-name>` ist hierbei der zuvor angegebene Name:

<pre>
Creating Resource group 'appsvc_rg_Linux_centralus' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_centralus' ...
App service plan creation complete
Creating app '&lt;app-name&gt;' ....
Configuring default logging for the app, if not already enabled
Creating zip with contents of dir D:\Examples\python-docs-hello-world ...
Getting scm site credentials for zip deployment
Starting zip deployment. This operation can take a while to complete ...
Deployment endpoint responded with status code 202
You can launch the app at http://&lt;app-name&gt;.azurewebsites.net
{
  "URL": "http://&lt;app-name&gt;.net",
  "appserviceplan": "appsvc_asp_Linux_centralus",
  "location": "eastus",
  "name": "&lt;app-name&gt;",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_centralus",
  "runtime_version": "python|3.7",
  "runtime_version_detected": "-",
  "sku": "FREE",
  "src_path": "D:\\Examples\\python-docs-hello-world"
}
</pre>

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Navigieren zur App

Navigieren Sie in Ihrem Webbrowser zur an der URL `http://<app-name>.azurewebsites.net` bereitgestellten Anwendung.

Der Python-Beispielcode führt einen Linux-Container in App Service mit einem integrierten Image aus.

![Ausführen einer Python-Beispiel-App in Azure](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

**Glückwunsch!** Sie haben Ihre Python-App in App Service unter Linux bereitgestellt.

## <a name="redeploy-updates"></a>Erneutes Bereitstellen von Updates

Öffnen Sie *application.py* in Ihrem bevorzugten Code-Editor, und aktualisieren Sie die Funktion `hello` wie folgt. Durch diese Änderung wird eine Anweisung vom Typ `print` hinzugefügt, um die Protokollierungsausgabe für den nächsten Abschnitt zu generieren. 

```python
def hello():
    print("Handling request to home page.")
    return "Hello Azure!"
```

Speichern Sie Ihre Änderungen, und beenden Sie den Editor. 

Stellen Sie die App mithilfe des Befehls `az webapp up` erneut bereit:

```azurecli
az webapp up
```

In diesem Befehl werden zwischengespeicherte Werte aus der Datei *.azure/config* verwendet (einschließlich App-Name, Ressourcengruppe und App Service-Plan).

Wechseln Sie nach Abschluss der Bereitstellung zurück zum Browserfenster, in dem `http://<app-name>.azurewebsites.net` geöffnet ist, und aktualisieren Sie die Seite, auf der die geänderte Meldung angezeigt werden sollte:

![Ausführen einer aktualisierten Python-Beispiel-App in Azure](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code bietet leistungsstarke Erweiterungen für Python und Azure App Service, die die Bereitstellung von Python-Web-Apps in App Service vereinfachen. Weitere Informationen finden Sie unter [Bereitstellen von Python-Apps in App Service aus Visual Studio Code](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="stream-logs"></a>Streaming von Protokollen

Sie können auf die Konsolenprotokolle zugreifen, die innerhalb der App und des Containers, in dem sie ausgeführt wird, generiert wurden. Protokolle enthalten alle Ausgaben, die mit `print`-Anweisungen generiert werden.

Führen Sie zum Streamen von Protokollen den folgenden Befehl aus:

```azurecli
az webapp log tail
```

Aktualisieren Sie die App im Browser, um Konsolenprotokolle zu erstellen, die Zeilen ähnlich dem folgenden Text enthalten sollten. Wenn Sie die Ausgabe nicht sofort sehen, versuchen Sie es nach 30 Sekunden erneut.

<pre>
2020-04-03T22:54:04.236405938Z Handling request to home page.
2020-04-03T22:54:04.236497641Z 172.16.0.1 - - [03/Apr/2020:22:54:04 +0000] "GET / HTTP/1.1" 200 12 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.83 Safari/537.36 Edg/81.0.416.41"
</pre>

Sie können die Protokolldateien auch im Browser unter `https://<app-name>.scm.azurewebsites.net/api/logs/docker` untersuchen.

Um das Protokollstreaming jederzeit zu beenden, geben Sie `Ctrl`+`C` ein.

## <a name="manage-the-azure-app"></a>Verwalten der Azure-App

Wechseln Sie zum <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>, um die erstellte App zu verwalten. Suchen Sie nach **App Services**, und wählen Sie diese Option aus.

![Navigieren zu App Services im Azure-Portal](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Wählen Sie den Namen Ihrer Azure-App aus.

![Navigieren zu Ihrer Python-App in App Services im Azure-Portal](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

Die Übersichtsseite Ihrer App wird angezeigt. Hier können Sie einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen.

![Verwalten Ihrer Python-App auf der Seite „Übersicht“ im Azure-Portal](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

Im Menü „App Service“ werden verschiedene Seiten für die Konfiguration Ihrer App angezeigt.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Die Ressourcengruppe hat abhängig von Ihrem Standort einen Namen wie appsvc_rg_Linux_CentralUS. Wenn Sie eine andere App Service-SKU als den kostenlosen Tarif „F1“ verwenden, fallen laufende Kosten für diese Ressourcen an (siehe [App Service – Preise](https://azure.microsoft.com/pricing/details/app-service/linux/)).

Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, löschen Sie die Ressourcengruppe, indem Sie den folgenden Befehl ausführen. Ersetzen Sie `<resource-group-name>` durch die in der Ausgabe des Befehls `az webapp up` gezeigte Ressourcengruppe, wie z. B. appsvc_rg_Linux_centralus. Die Ausführung des Befehls kann eine Minute in Anspruch nehmen.

```azurecli
az group delete -n <resource-group-name>
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Python-Web-App (Django) mit PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Python-Web-App](../../active-directory/develop/quickstart-v2-python-webapp.md)

> [!div class="nextstepaction"]
> [Konfigurieren einer Python-App](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Tutorial: Ausführen einer Python-App in einem benutzerdefinierten Container](tutorial-custom-docker-image.md)
