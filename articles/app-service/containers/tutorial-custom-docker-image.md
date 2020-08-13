---
title: 'Tutorial: Erstellen und Ausführen eines benutzerdefinierten Images in Azure App Service'
description: In dieser Schritt-für-Schritt-Anleitung wird erläutert, wie Sie ein benutzerdefiniertes Linux-Image erstellen, das Image in Azure Container Registry pushen und es dann in Azure App Service bereitstellen.
keywords: Azure App Service, Web-App, Linux, Docker, Container
author: msangapu-msft
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.topic: tutorial
ms.date: 07/16/2020
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-python
ms.openlocfilehash: 1c1313c7333c20dac5e581025a571c6a733dafc6
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2020
ms.locfileid: "87850915"
---
# <a name="tutorial-run-a-custom-docker-image-in-app-service"></a>Tutorial: Ausführen eines benutzerdefinierten Docker-Images in App Service

Für Azure App Service wird die Containertechnologie von Docker genutzt, um sowohl integrierte als auch benutzerdefinierte Images zu hosten. Führen Sie zum Anzeigen einer Liste der integrierten Images den Azure CLI-Befehl [„az webapp list-runtimes --linux“](/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes) aus. Wenn diese Images Ihre Anforderungen nicht erfüllen, können Sie ein benutzerdefiniertes Image erstellen und bereitstellen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines benutzerdefinierten Images, wenn kein integriertes Image Ihre Anforderungen erfüllt
> * Pushen des benutzerdefinierten Images in eine private Containerregistrierung in Azure
> * Ausführen des benutzerdefinierten Images in App Service
> * Konfigurieren von Umgebungsvariablen
> * Aktualisieren und erneutes Bereitstellen des Images
> * Zugreifen auf Diagnoseprotokolle
> * Herstellen einer Verbindung mit dem Container per SSH

Bei diesem Tutorial fällt in Ihrem Azure-Konto eine geringfügige Gebühr für die Containerregistrierung an. Wird der Container länger als einen Monat gehostet, können weitere Kosten anfallen.

## <a name="set-up-your-initial-environment"></a>Einrichten der anfänglichen Umgebung

* Sie benötigen ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Installieren Sie [Docker](https://docs.docker.com/get-started/#setup). Mit diesem Tool erstellen Sie Docker-Images. Zum Installieren von Docker ist möglicherweise ein Neustart des Computers erforderlich.
* Installieren Sie die <a href="/cli/azure/install-azure-cli" target="_blank">Azure CLI</a> 2.0.80 oder höher, mit der Sie Befehle in einer beliebigen Shell ausführen, um Azure-Ressourcen bereitzustellen und zu konfigurieren.

Öffnen Sie nach der Installation von Docker und der Azure CLI ein Terminalfenster, und vergewissern Sie sich, dass Docker installiert ist:

```bash
docker --version
```

Vergewissern Sie sich außerdem, dass Sie mindestens Version 2.0.80 der Azure CLI verwenden:

```azurecli
az --version
```

Melden Sie sich anschließend über die CLI bei Azure an:

```azurecli
az login
```

Der Befehl `az login` öffnet einen Browser zum Erfassen Ihrer Anmeldeinformationen. Wenn der Befehl abgeschlossen ist, wird eine JSON-Ausgabe mit Informationen zu Ihren Abonnements angezeigt.

Nachdem Sie sich angemeldet haben, können Sie Azure-Befehle mit der Azure CLI ausführen, um Ressourcen in Ihrem Abonnement zu verwenden.

## <a name="clone-or-download-the-sample-app"></a>Klonen oder Herunterladen der Beispiel-App

Das Beispiel für dieses Tutorial können Sie herunterladen oder mithilfe von „Git Clone“ klonen.

### <a name="clone-with-git"></a>Klonen mit Git

Klonen Sie das Beispielrepository:

```terminal
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
```

Fügen Sie unbedingt das Argument `--config core.autocrlf=input` mit ein, um ordnungsgemäße Zeilenenden in Dateien zu gewährleisten, die im Linux-Container verwendet werden:

Wechseln Sie dann zu diesem Ordner:

```terminal
cd docker-django-webapp-linux
```

### <a name="download-from-github"></a>Von GitHub herunterladen

Statt „Git Clone“ zu verwenden, können Sie zu [https://github.com/Azure-Samples/docker-django-webapp-linux](https://github.com/Azure-Samples/docker-django-webapp-linux) navigieren und dann **Klonen** und **ZIP herunterladen** auswählen. 

Entpacken Sie die ZIP-Datei in einem Ordner mit dem Namen *docker-django-webapp-linux*. 

Öffnen Sie dann ein Terminalfenster im Ordner *docker-django-webapp-linux*.

## <a name="optional-examine-the-docker-file"></a>(Optional) Untersuchen der Docker-Datei

Die Datei im Beispiel mit dem Namen _Dockerfile_, die das Docker-Image beschreibt und Konfigurationsanweisungen enthält:

```Dockerfile
FROM tiangolo/uwsgi-nginx-flask:python3.6

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt --no-cache-dir
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222

#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

* Mit der ersten Gruppe von Befehlen werden die erforderlichen Komponenten der App in der Umgebung installiert.
* Mit der zweiten Gruppe von Befehlen wird ein [SSH](https://www.ssh.com/ssh/protocol/)-Server für eine sichere Kommunikation zwischen dem Container und dem Host erstellt.
* Mit der letzten Zeile (`ENTRYPOINT ["init.sh"]`) wird `init.sh` aufgerufen, um den SSH-Dienst und den Python-Server zu starten.

## <a name="build-and-test-the-image-locally"></a>Lokales Erstellen und Testen des Images

1. Führen Sie den folgenden Befehl aus, um das Image zu erstellen:

    ```bash
    docker build --tag appsvc-tutorial-custom-image .
    ```
    
1. Testen Sie, ob der Build funktioniert, indem Sie den Docker-Container lokal ausführen:

    ```bash
    docker run -p 8000:8000 appsvc-tutorial-custom-image
    ```
    
    Mit diesem [`docker run`](https://docs.docker.com/engine/reference/commandline/run/)-Befehl wird mit dem Argument `-p` der Port gefolgt vom Namen des Images angegeben. 
    
    > [!TIP]
    > Wenn Sie Windows verwenden und der Fehler „*standard_init_linux.go:211: exec user process caused "Datei oder Verzeichnis nicht vorhanden"* “ angezeigt wird, enthält die Datei *init.sh* CRLF-Zeilenenden anstelle der erwarteten LF-Zeilenenden. Dieser Fehler tritt auf, wenn Sie Git zum Klonen des Beispielrepositorys verwendet, aber den Parameter `--config core.autocrlf=input` ausgelassen haben. In diesem Fall klonen Sie das Repository erneut mit dem Argument „--config“. Der Fehler wird möglicherweise auch angezeigt, wenn Sie *init.sh* bearbeitet und mit CRLF-Zeilenenden gespeichert haben. Speichern Sie in diesem Fall die Datei nur mit LF-Zeilenenden erneut.

1. Rufen Sie `http://localhost:8000` auf, um zu überprüfen, ob Web-App und Container ordnungsgemäß funktionieren.

    ![Lokales Testen der Web-App](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In diesem Abschnitt und den nachfolgenden Abschnitten stellen Sie Ressourcen in Azure bereit, in die Sie das Image pushen, und stellen anschließend einen Container für Azure App Service bereit. Sie erstellen zunächst eine Ressourcengruppe, in der alle diese Ressourcen gesammelt werden.

Führen Sie den Befehl [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) aus, um eine Ressourcengruppe zu erstellen:

```azurecli-interactive
az group create --name AppSvc-DockerTutorial-rg --location westus2
```

Sie können den Wert `--location` ändern, um eine Region in Ihrer Nähe anzugeben.

## <a name="push-the-image-to-azure-container-registry"></a>Übertragen des Images an Azure Container Registry per Push

In diesem Abschnitt pushen Sie das Image in Azure Container Registry. Von dort kann es von App Service bereitgestellt werden.

1. Führen Sie den Befehl [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) aus, um eine Azure Container Registry-Instanz zu erstellen:

    ```azurecli-interactive
    az acr create --name <registry-name> --resource-group AppSvc-DockerTutorial-rg --sku Basic --admin-enabled true
    ```
    
    Ersetzen Sie `<registry-name>` durch einen geeigneten Namen für die Registrierung. Der Name darf nur Buchstaben und Ziffern enthalten und muss in Azure eindeutig sein.

1. Führen Sie den Befehl [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az-acr-show) aus, um Anmeldeinformationen für die Registrierung abzurufen:

    ```azurecli-interactive
    az acr credential show --resource-group AppSvc-DockerTutorial-rg --name <registry-name>
    ```
    
    Die JSON-Ausgabe dieses Befehls enthält zwei Kennwörter sowie den Benutzernamen der Registrierung.
    
1. Verwenden Sie den Befehl `docker login`, um sich bei der Containerregistrierung anzumelden:

    ```bash
    docker login <registry-name>.azurecr.io --username <registry-username>
    ```
    
    Ersetzen Sie `<registry-name>` und `<registry-username>` durch die Werte aus den vorherigen Schritten. Geben Sie bei entsprechender Aufforderung eines der Kennwörter aus dem vorherigen Schritt ein.

    Sie verwenden in allen verbleibenden Schritten dieses Abschnitts denselben Registrierungsnamen.

1. Wenn die Anmeldung erfolgreich ist, markieren Sie das lokale Docker-Image für die Registrierung:

    ```bash
   docker tag appsvc-tutorial-custom-image <registry-name>.azurecr.io/appsvc-tutorial-custom-image:latest
    ```    

1. Pushen Sie das Image mithilfe des Befehls `docker push` in die Registrierung:

    ```bash
    docker push <registry-name>.azurecr.io/appsvc-tutorial-custom-image:latest
    ```

    Das erste Hochladen des Images kann einige Minuten dauern, da es das Basisimage enthält. Nachfolgende Uploads sind in der Regel schneller.

    Während Sie warten, können Sie die Schritte im nächsten Abschnitt ausführen, um App Service für die Bereitstellung über die Registrierung zu konfigurieren.

1. Überprüfen Sie mithilfe des Befehls `az acr repository list`, ob der Pushvorgang erfolgreich war:

    ```azurecli-interactive
    az acr repository list -n <registry-name>
    ```
    
    In der Ausgabe sollte der Name des Images angezeigt werden.


## <a name="configure-app-service-to-deploy-the-image-from-the-registry"></a>Konfigurieren von App Service für die Bereitstellung des Images über die Registrierung

Zum Bereitstellen eines Containers in Azure App Service erstellen Sie zunächst eine Web-App in App Service und verbinden dann die Web-App mit der Containerregistrierung. Beim Starten der Web-App pullt App Service automatisch das Image aus der Registrierung.

1. Erstellen Sie mithilfe des Befehls [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) einen App Service-Plan:

    ```azurecli-interactive
    az appservice plan create --name AppSvc-DockerTutorial-plan --resource-group AppSvc-DockerTutorial-rg --is-linux
    ```

    Ein App Service-Plan entspricht dem virtuellen Computer, der die Web-App hostet. Standardmäßig verwendet der vorherige Befehl den preisgünstigen [B1-Tarif](https://azure.microsoft.com/pricing/details/app-service/linux/), bei dem der erste Monat kostenlos ist. Sie können den Tarif mit dem Parameter `--sku` steuern.

1. Erstellen Sie die Web-App mithilfe des Befehls [`az webpp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create):

    ```azurecli-interactive
    az webapp create --resource-group AppSvc-DockerTutorial-rg --plan AppSvc-DockerTutorial-plan --name <app-name> --deployment-container-image-name <registry-name>.azurecr.io/appsvc-tutorial-custom-image:latest
    ```
    
    Ersetzen Sie `<app-name>` durch einen Namen für die Web-App, der innerhalb von Azure eindeutig sein muss. Ersetzen Sie außerdem `<registry-name>` durch den Namen Ihrer Registrierung aus dem vorherigen Abschnitt.

1. Verwenden Sie [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set), um die Umgebungsvariable `WEBSITES_PORT` wie vom App-Code erwartet festzulegen: 

    ```azurecli-interactive
    az webapp config appsettings set --resource-group AppSvc-DockerTutorial-rg --name <app-name> --settings WEBSITES_PORT=8000
    ```

    Ersetzen Sie `<app-name>` durch den Namen, den Sie im vorherigen Schritt verwendet haben.
    
    Weitere Informationen zu dieser Umgebungsvariablen finden Sie in der [Infodatei im GitHub-Repository des Beispiels](https://github.com/Azure-Samples/docker-django-webapp-linux).

1. Aktivieren Sie mithilfe des Befehls [`az webapp identity assign`](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) eine [verwaltete Identität](/azure/app-service/overview-managed-identity) für die Web-App:

    ```azurecli-interactive
    az webapp identity assign --resource-group AppSvc-DockerTutorial-rg --name <app-name> --query principalId --output tsv
    ```

    Ersetzen Sie `<app-name>` durch den Namen, den Sie im vorherigen Schritt verwendet haben. Die Ausgabe des Befehls (gefiltert nach den Argumenten `--query` und `--output`) ist der Dienstprinzipal der zugewiesenen Identität, den Sie in Kürze verwenden.

    Die verwaltete Identität ermöglicht es Ihnen, der Web-App Berechtigungen für den Zugriff auf andere Azure-Ressourcen zu erteilen, ohne dass bestimmte Anmeldeinformationen erforderlich sind.

1. Rufen Sie Ihre Abonnement-ID mit dem Befehl [`az account show`](/cli/azure/account?view=azure-cli-latest#az-account-show) ab, die Sie im nächsten Schritt benötigen:

    ```azurecli-interactive
    az account show --query id --output tsv
    ``` 

1. Erteilen Sie der Web-App die Berechtigung für den Zugriff auf die Containerregistrierung:

    ```azurecli-interactive
    az role assignment create --assignee <principal-id> --scope /subscriptions/<subscription-id>/resourceGroups/AppSvc-DockerTutorial-rg/providers/Microsoft.ContainerRegistry/registries/<registry-name> --role "AcrPull"
    ```

    Ersetzen Sie die folgenden Werte:
    - `<principal-id>` durch die Dienstprinzipal-ID aus dem Befehl `az webapp identity assign`
    - `<registry-name>` durch den Namen Ihrer Containerregistrierung
    - `<subscription-id>` durch die vom Befehl `az account show` abgerufenen Abonnement-ID

Weitere Informationen zu diesen Berechtigungen finden Sie unter [Was ist die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) von Azure?](/azure/role-based-access-control/overview). 

## <a name="deploy-the-image-and-test-the-app"></a>Bereitstellen des Images und Testen der App

Sie können diese Schritte ausführen, sobald das Image in die Containerregistrierung gepusht und App Service vollständig bereitgestellt wurde.

1. Verwenden Sie den Befehl [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set), um die für die Web-App bereitzustellende Containerregistrierung und das dazugehörige Image anzugeben:

    ```azurecli-interactive
    az webapp config container set --name <app-name> --resource-group AppSvc-DockerTutorial-rg --docker-custom-image-name <registry-name>.azurecr.io/appsvc-tutorial-custom-image:latest --docker-registry-server-url https://<registry-name>.azurecr.io
    ```
    
    Ersetzen Sie `<app_name>` durch den Namen Ihrer Web-App und `<registry-name>` an zwei Stellen durch den Namen Ihrer Registrierung. 

    - Bei Verwendung einer anderen Registrierung als Docker Hub (wie in diesem Beispiel gezeigt) muss `--docker-registry-server-url` das Format `https://` haben, gefolgt vom vollqualifizierten Domänennamen der Registrierung.
    - Eine Meldung mit dem Hinweis, dass für den Zugriff auf Azure Container Registry keine Anmeldeinformationen angegeben wurden und dass versucht wird, diese nachzuschlagen, bedeutet, dass Azure die verwaltete Identität der App für die Authentifizierung bei der Containerregistrierung verwendet, anstelle nach Benutzername und Kennwort zu fragen.
    - Wird der Fehler „AttributeError: 'NoneType' object has no attribute 'reserved'“ (AttributeError: Das NoneType-Objekt hat kein Attribut 'reserved'.) angezeigt, vergewissern Sie sich, ob `<app-name>` korrekt ist.

    > [!TIP]
    > Sie können die Containereinstellungen der Web-App jederzeit mit dem Befehl `az webapp config container show --name <app-name> --resource-group AppSvc-DockerTutorial-rg`abrufen. Das Image wird in der Eigenschaft `DOCKER_CUSTOM_IMAGE_NAME` angegeben. Wenn die Web-App über Azure DevOps oder Azure Resource Manager-Vorlagen bereitgestellt wird, kann das Image auch in einer Eigenschaft mit dem Namen `LinuxFxVersion` angezeigt werden. Beide Eigenschaften dienen demselben Zweck. Sind beide in der Web-App-Konfiguration vorhanden, hat `LinuxFxVersion` Vorrang.

1. Nach Abschluss des Befehls `az webapp config container set` sollte die Web-App im Container in App Service ausgeführt werden.

    Navigieren Sie zum Testen der App zu `http://<app-name>.azurewebsites.net`, und ersetzen Sie `<app-name>` durch den Namen Ihrer Web-App. Beim ersten Zugriff kann es einige Zeit dauern, bis die App antwortet, da App Service das gesamte Image aus der Registrierung abrufen muss. Aktualisieren Sie einfach die Seite, wenn für den Browser eine Zeitüberschreitung eintritt. Nach dem Pullen des anfänglichen Images werden nachfolgende Tests wesentlich schneller ausgeführt.

    ![Erfolgreicher Test der Web-App in Azure](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="modify-the-app-code-and-redeploy"></a>Ändern des App-Codes und erneutes Bereitstellen

In diesem Abschnitt ändern Sie den Web-App-Code, erstellen den Container neu und pushen den Container anschließend in die Registrierung. App Service pullt das aktualisierte Image dann automatisch aus der Registrierung, um die ausgeführte Web-App zu aktualisieren.

1. Öffnen Sie im lokalen Ordner *docker-django-webapp-linux* die Datei *app/templates/app/index.html*.

1. Ändern Sie das erste HTML-Element entsprechend dem folgenden Code.

    ```html
    <nav class="navbar navbar-inverse navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>
        </div>
      </div>
    </nav>
    ```
    
1. Speichern Sie die Änderungen.

1. Navigieren Sie zum Ordner *docker-django-webapp-linux*, und erstellen Sie das Image neu:

    ```bash
    docker build --tag appsvc-tutorial-custom-image .
    ```

1. Aktualisieren Sie die Versionsnummer im Tag des Images auf v1.0.1:

    ```bash
    docker tag appsvc-tutorial-custom-image <registry-name>.azurecr.io/appsvc-tutorial-custom-image:latest
    ```

    Ersetzen Sie `<registry-name>` durch den Namen Ihrer Registrierung.

1. Übertragen Sie das Image per Push in die Registrierung:

    ```bash
    docker push <registry-name>.azurecr.io/appsvc-tutorial-custom-image:latest
    ```

1. Starten Sie die Web-App neu:

    ```azurecli-interactive
    az webapp restart --name <app_name> --resource-group AppSvc-DockerTutorial-rg
    ```

    Ersetzen Sie `<app_name>` durch den Namen Ihrer Web-App. Beim Neustart ruft App Service das aktualisierte Image aus der Containerregistrierung ab.

1. Überprüfen Sie, ob das Update bereitgestellt wurde, indem Sie zu `http://<app-name>.azurewebsites.net` navigieren.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

1. Aktivieren Sie die Containerprotokollierung:

    ```azurecli-interactive
    az webapp log config --name <app-name> --resource-group AppSvc-DockerTutorial-rg --docker-container-logging filesystem
    ```
    
1. Aktivieren Sie den Protokolldatenstrom:

    ```azurecli-interactive
    az webapp log tail --name <app-name> --resource-group AppSvc-DockerTutorial-rg
    ```
    
    Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

    Sie können die Protokolldateien auch im Browser unter `https://<app-name>.scm.azurewebsites.net/api/logs/docker` untersuchen.

1. Zum Beenden des Protokollstreamings können Sie jederzeit **STRG**+**C** drücken.

## <a name="connect-to-the-container-using-ssh"></a>Herstellen einer Verbindung mit dem Container per SSH

SSH ermöglicht die sichere Kommunikation zwischen einem Container und einem Client. Zum Aktivieren der SSH-Verbindung mit Ihrem Container muss Ihr benutzerdefiniertes Image dafür konfiguriert werden. Sobald der Container ausgeführt wird, können Sie eine SSH-Verbindung öffnen.

### <a name="configure-the-container-for-ssh"></a>Konfigurieren des Containers für SSH

Die in diesem Tutorial verwendete Beispiel-App verfügt bereits über die erforderliche Konfiguration im *Dockerfile*, die den SSH-Server installiert und außerdem die Anmeldeinformationen festlegt. Dieser Abschnitt dient nur zu Informationszwecken. Fahren Sie mit dem nächsten Abschnitt fort, um eine Verbindung mit dem Container herzustellen.

```Dockerfile
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
  && apt-get install -y --no-install-recommends openssh-server \
  && echo "$SSH_PASSWD" | chpasswd 
```

> [!NOTE]
> Diese Konfiguration erlaubt keine externen Verbindungen zum Container. SSH ist nur über die Kudu/SCM-Website verfügbar. Die Authentifizierung für die Kudu/SCM-Website wird über Ihr Azure-Konto durchgeführt.

Das *Dockerfile* kopiert außerdem die Datei *sshd_config* in den Ordner */etc/ssh/* und macht Port 2222 für den Container verfügbar:

```Dockerfile
COPY sshd_config /etc/ssh/

# ...

EXPOSE 8000 2222
```

Port 2222 ist ein interner Port, auf den nur von Containern innerhalb des Brückennetzwerks eines privaten virtuellen Netzwerks zugegriffen werden kann. 

Mit dem Eingangsskript *init.sh* wird der SSH-Server schließlich gestartet.

```bash
#!/bin/bash
service ssh start
```

### <a name="open-ssh-connection-to-container"></a>Öffnen einer SSH-Verbindung mit einem Container

1. Navigieren Sie zu `https://<app-name>.scm.azurewebsites.net/webssh/host`, und melden Sie sich mit Ihrem Azure-Konto an. Ersetzen Sie `<app-name>` durch den Namen Ihrer Web-App.

1. Nachdem Sie sich angemeldet haben, werden Sie auf eine Informationsseite für die Web-App umgeleitet. Wählen Sie oben auf der Seite **SSH** aus, um die Shell zu öffnen und Befehle zu verwenden.

    Beispielsweise können Sie mithilfe des Befehls `top` die Prozesse untersuchen, die darin ausgeführt werden.
    
## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Artikel erstellte Ressourcen können laufende Kosten verursachen. Zum Bereinigen der Ressourcen müssen Sie nur die Ressourcengruppe löschen, in der die Ressourcen enthalten sind:

```azurecli
az group delete --name AppSvc-DockerTutorial-rg
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellen eines benutzerdefinierten Images in einer privaten Containerregistrierung
> * Bereitstellen des benutzerdefinierten Images in App Service
> * Aktualisieren und erneutes Bereitstellen des Images
> * Zugreifen auf Diagnoseprotokolle
> * Herstellen einer Verbindung mit dem Container per SSH

Im nächsten Tutorial erfahren Sie, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen:

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](../app-service-web-tutorial-custom-domain.md)

Oder sehen Sie sich weitere Ressourcen an:

> [!div class="nextstepaction"]
> [Konfigurieren eines benutzerdefinierten Containers](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Tutorial: WordPress-App mit mehreren Containern](tutorial-multi-container-app.md)
