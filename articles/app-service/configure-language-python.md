---
title: Konfigurieren von Linux-Python-Apps
description: Es wird beschrieben, wie Sie den Python-Container konfigurieren, in dem Web-Apps ausgeführt werden, indem Sie sowohl das Azure-Portal als auch die Azure CLI verwenden.
ms.topic: quickstart
ms.date: 10/06/2020
ms.reviewer: astay; kraigb
ms.custom: mvc, seodec18, devx-track-python, devx-track-azurecli
ms.openlocfilehash: 935baef209811146d0b60f4fc02986818fd103a7
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92743790"
---
# <a name="configure-a-linux-python-app-for-azure-app-service"></a>Konfigurieren einer Linux-Python-App für Azure App Service

In diesem Artikel erfahren Sie, wie Python-Apps mit [Azure App Service](overview.md) ausgeführt werden und wie Sie das Verhalten von App Service bei Bedarf anpassen können. Python-Apps müssen mit allen erforderlichen [pip](https://pypi.org/project/pip/)-Modulen bereitgestellt werden.

Die App Service-Bereitstellungs-Engine aktiviert automatisch eine virtuelle Umgebung und führt `pip install -r requirements.txt` für Sie aus, wenn Sie ein [Git-Repository](deploy-local-git.md) oder ein [ZIP-Paket](deploy-zip.md) bereitstellen.

Dieser Leitfaden enthält wichtige Konzepte und Anleitungen für Python-Entwickler, die in App Service einen integrierten Linux-Container verwenden. Falls Sie Azure App Service noch nicht verwendet haben, ist es ratsam, zunächst die [Python-Schnellstartanleitung](quickstart-python.md) und das [Tutorial „Python mit PostgreSQL“](tutorial-python-postgresql-app.md) durchzuarbeiten.

Sie können für die Konfiguration entweder das [Azure-Portal](https://portal.azure.com) oder die Azure CLI verwenden:

- **Azure-Portal** : Verwenden Sie die Seite **Einstellungen** > **Konfiguration** der App wie unter [Konfigurieren einer App Service-App im Azure-Portal](configure-common.md) beschrieben.

- **Azure CLI** : Sie haben zwei Möglichkeiten.

    - Führen Sie Befehle im Dienst [Azure Cloud Shell](../cloud-shell/overview.md) aus, den Sie jeweils mit der Schaltfläche **Jetzt testen** in der rechten oberen Ecke der Codeblöcke öffnen können.
    - Führen Sie Befehle lokal aus, indem Sie die aktuelle Version der [Azure CLI](/cli/azure/install-azure-cli) installieren und sich dann mit dem Befehl [az login](/cli/azure/reference-index#az-login) bei Azure anmelden.
    
> [!NOTE]
> Linux ist derzeit die empfohlene Option zum Ausführen von Python-Apps in App Service. Informationen zur Windows-Option finden Sie unter [Einrichten einer Python-Umgebung in Azure App Service (Windows)](/visualstudio/python/managing-python-on-azure-app-service).

## <a name="configure-python-version"></a>Konfigurieren der Python-Version

- **Azure-Portal** : Verwenden Sie die Registerkarte **Allgemeine Einstellungen** auf der Seite **Konfiguration** . Dies ist unter [Konfigurieren allgemeiner Einstellungen](configure-common.md#configure-general-settings) für Linux-Container beschrieben.

- **Azure CLI** :

    -  Zeigen Sie die aktuelle Python-Version mit [az webapp config show](/cli/azure/webapp/config#az_webapp_config_show) an:
    
        ```azurecli-interactive
        az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
        ```
        
        Ersetzen Sie `<resource-group-name>` und `<app-name>` durch die entsprechenden Namen für Ihre Web-App.
    
    - Legen Sie die Python-Version mit [az webapp config set](/cli/azure/webapp/config#az_webapp_config_set) fest.
        
        ```azurecli-interactive
        az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "PYTHON|3.7"
        ```
    
    - Zeigen Sie alle Python-Versionen an, die in Azure App Service unterstützt werden, indem Sie [az webapp list-runtimes](/cli/azure/webapp#az_webapp_list_runtimes) verwenden:
    
        ```azurecli-interactive
        az webapp list-runtimes --linux | grep PYTHON
        ```
    
Sie können auch eine nicht unterstützte Version von Python ausführen, indem Sie stattdessen Ihr eigenes Containerimage erstellen. Weitere Informationen finden Sie unter [Verwenden eines benutzerdefinierten Docker-Images für Web-App für Container](tutorial-custom-container.md?pivots=container-linux).

<!-- <a> element here to preserve external links-->
<a name="access-environment-variables"></a>

## <a name="customize-build-automation"></a>Anpassen der Buildautomatisierung

Mit dem Buildsystem von App Service (Bezeichnung: Oryx) werden die folgenden Schritte ausgeführt, wenn Sie Ihre App per Git oder mit ZIP-Paketen bereitstellen:

1. Führen Sie ein benutzerdefiniertes Präbuildskript aus, sofern dies über die Einstellung `PRE_BUILD_COMMAND` angegeben ist.
1. Führen Sie `pip install -r requirements.txt` aus. Die Datei *requirements.txt* muss im Stammordner des Projekts enthalten sein. Andernfalls wird vom Buildprozess ein Fehler der folgenden Art gemeldet: „setup.py oder requirements.txt nicht gefunden. pip-Installation wird nicht ausgeführt.“
1. Wenn sich *manage.py* im Stammverzeichnis des Repositorys befindet (Hinweis auf Django-App), wird *manage.py collectstatic* ausgeführt. Wenn die Einstellung `DISABLE_COLLECTSTATIC` aber auf `true` festgelegt ist, wird dieser Schritt übersprungen.
1. Führen Sie ein benutzerdefiniertes Postbuildskript aus, sofern dies über die Einstellung `POST_BUILD_COMMAND` angegeben ist.

Standardmäßig sind die Einstellungen für `PRE_BUILD_COMMAND`, `POST_BUILD_COMMAND` und `DISABLE_COLLECTSTATIC` leer. 

- Wenn Sie bei der Erstellung von Django-Apps die Ausführung von „collectstatic“ deaktivieren möchten, müssen Sie `DISABLE_COLLECTSTATIC` auf „true“ festlegen.

- Legen Sie zum Ausführen von Präbuildbefehlen die Einstellung `PRE_BUILD_COMMAND` so fest, dass sie entweder einen Befehl, z. B. `echo Pre-build command`, oder einen Pfad zu einer Skriptdatei, z. B. `scripts/prebuild.sh`, relativ zu Ihrem Projektstamm enthält. Für alle Befehle müssen relative Pfade zum Stammordner des Projekts verwendet werden.

- Legen Sie zum Ausführen von Postbuildbefehlen die Einstellung `POST_BUILD_COMMAND` so fest, dass sie entweder einen Befehl, z. B. `echo Post-build command`, oder einen Pfad zu einer Skriptdatei, z. B. `scripts/postbuild.sh`, relativ zu Ihrem Projektstamm enthält. Für alle Befehle müssen relative Pfade zum Stammordner des Projekts verwendet werden.

Weitere Einstellungen zum Anpassen der Buildautomatisierung finden Sie unter [Oryx-Konfiguration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md). 

Weitere Informationen zur Ausführung und Erstellung von Python-Apps unter Linux durch App Service finden Sie unter [Erkennung und Erstellung von Python-Apps durch Oryx](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/python.md).

> [!NOTE]
> Die Einstellungen `PRE_BUILD_SCRIPT_PATH` und `POST_BUILD_SCRIPT_PATH` sind mit `PRE_BUILD_COMMAND` und `POST_BUILD_COMMAND` identisch und werden aus Legacygründen unterstützt.
> 
> Eine Einstellung mit dem Namen `SCM_DO_BUILD_DURING_DEPLOYMENT` löst (sofern sie `true` oder 1 enthält) einen Oryx-Buildvorgang während der Bereitstellung aus. Die Einstellung ist auf „true“ festgelegt, wenn die Bereitstellung mit git, dem Azure CLI-Befehl `az webapp up` und Visual Studio Code durchgeführt wird.

> [!NOTE]
> Verwenden Sie in allen Prä- und Postbuildskripts immer relative Pfade, da sich der Buildcontainer, in dem Oryx ausgeführt wird, von dem Runtimecontainer unterscheidet, in dem die App ausgeführt wird. Verlassen Sie sich niemals auf die genaue Platzierung Ihres App-Projektordners im Container (z. B. die Platzierung unter *site/wwwroot* ).

## <a name="production-settings-for-django-apps"></a>Produktionseinstellungen für Django-Apps

Für eine Produktionsumgebung wie Azure App Service sollten bei Django-Apps die Vorgaben in der [Bereitstellungsprüfliste](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/) von Django (djangoproject.com) erfüllt werden.

In der folgenden Tabelle sind die Produktionseinstellungen beschrieben, die für Azure relevant sind. Diese Einstellungen werden in der Datei *setting.py* der App definiert.

| Django-Einstellung | Anleitung für Azure |
| --- | --- |
| `SECRET_KEY` | Speichern Sie den Wert in einer App Service-Einstellung, wie dies unter [Zugreifen auf App-Einstellungen als Umgebungsvariablen](#access-app-settings-as-environment-variables) beschrieben ist. Alternativ können Sie den [Wert in Azure Key Vault als „Geheimnis“ speichern](/azure/key-vault/secrets/quick-create-python). |
| `DEBUG` | Erstellen Sie in App Service die Einstellung `DEBUG` mit dem Wert 0 (false), und laden Sie den Wert dann als Umgebungsvariable. Erstellen Sie in Ihrer Entwicklungsumgebung die Umgebungsvariable `DEBUG` mit dem Wert 1 (true). |
| `ALLOWED_HOSTS` | In der Produktion ist es für Django erforderlich, dass Sie die URL der App im Array `ALLOWED_HOSTS` von *settings.py* einfügen. Sie können diese URL zur Laufzeit mit dem Code `os.environ['WEBSITE_HOSTNAME']` abrufen. Von App Service wird die Umgebungsvariable `WEBSITE_HOSTNAME` automatisch auf die URL der App festgelegt. |
| `DATABASES` | Definieren Sie Einstellungen in App Service für die Datenbankverbindung, und laden Sie sie als Umgebungsvariablen, um das Wörterbuch [`DATABASES`](https://docs.djangoproject.com/en/3.1/ref/settings/#std:setting-DATABASES) aufzufüllen. Sie können diese Werte (vor allem Benutzername und Kennwort) auch als [Azure Key Vault-Geheimnisse](/azure/key-vault/secrets/quick-create-python) speichern. |

## <a name="container-characteristics"></a>Eigenschaften von Containern

Bei der Bereitstellung in App Service werden Python-Apps in einem Linux-Docker-Container ausgeführt, der im [GitHub-Repository für Python in App Service](https://github.com/Azure-App-Service/python) definiert ist. Sie finden Imagekonfigurationen in den versionsspezifischen Verzeichnissen.

Dieser Container verfügt über die folgenden Eigenschaften:

- Apps werden per [Gunicorn WSGI HTTP Server](https://gunicorn.org/) mit den zusätzlichen Argumenten `--bind=0.0.0.0 --timeout 600` ausgeführt.
    - Sie können Konfigurationseinstellungen für Gunicorn über die Datei *gunicorn.conf.py* im Projektstamm angeben. Dies ist in der [Übersicht zur Gunicorn-Konfiguration](https://docs.gunicorn.org/en/stable/configure.html#configuration-file) (docs.gunicorn.org) beschrieben. Alternativ können Sie den [Startbefehl anpassen](#customize-startup-command).

    - Um Ihre Web-App vor versehentlichen oder absichtlichen DDoS-Angriffen zu schützen, wird Gunicorn hinter einem Nginx-Reverseproxy ausgeführt. Dies ist unter [Bereitstellen von Gunicorn](https://docs.gunicorn.org/en/latest/deploy.html) (docs.gunicorn.org) beschrieben.

- Standardmäßig enthält das Basiscontainerimage nur das Flask-Webframework, aber der Container unterstützt auch andere Frameworks, die WSGI-konform und mit Python 3.6 oder höher kompatibel sind, z. B. Django.

- Erstellen Sie zum Installieren von zusätzlichen Paketen, z. B. Django, im Stamm Ihres Projekts die Datei [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files), in der Ihre direkten Abhängigkeiten angegeben sind. App Service installiert diese Abhängigkeiten dann automatisch, wenn Sie Ihr Projekt bereitstellen.

    Die Datei *requirements.txt* *muss* sich im Projektstamm befinden, damit die Abhängigkeiten installiert werden. Andernfalls wird vom Buildprozess ein Fehler der folgenden Art gemeldet: „setup.py oder requirements.txt nicht gefunden. pip-Installation wird nicht ausgeführt.“ Wenn dieser Fehler auftritt, sollten Sie den Speicherort Ihrer Anforderungsdatei überprüfen.

- App Service definiert automatisch eine Umgebungsvariable mit dem Namen `WEBSITE_HOSTNAME` und der URL der Web-App, z. B. `msdocs-hello-world.azurewebsites.net`. Außerdem wird `WEBSITE_SITE_NAME` mit dem Namen Ihrer App definiert, z. B. `msdocs-hello-world`. 
   
## <a name="container-startup-process"></a>Startprozess des Containers

Beim Starten führt der Container vom Typ „App Service für Linux“ die folgenden Schritte aus:

1. Verwenden eines [benutzerdefinierten Startbefehls](#customize-startup-command) (sofern vorhanden)
2. Überprüfen, ob eine [Django-App](#django-app) vorhanden ist, und Starten von Gunicorn für die ggf. erkannte App
3. Überprüfen, ob eine [Flask-App](#flask-app) vorhanden ist, und Starten von Gunicorn für die ggf. erkannte App
4. Wenn keine andere App gefunden wird, können Sie eine Standard-App starten, die im Container integriert ist.

Die folgenden Abschnitte enthalten weitere Details zu den einzelnen Optionen.

### <a name="django-app"></a>Django-App

Für Django-Apps sucht App Service in Ihrem App-Code nach der Datei `wsgi.py` und führt dann mit dem folgenden Befehl Gunicorn aus:

```bash
# <module> is the name of the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Gehen Sie wie folgt vor, falls Sie eine präzisere Kontrolle über den Startbefehl benötigen: Verwenden Sie einen [benutzerdefinierten Startbefehl](#customize-startup-command), ersetzen Sie `<module>` durch den Namen des Ordners, der *wsgi.py* enthält, und fügen Sie das Argument `--chdir` hinzu, falls sich dieses Modul nicht im Projektstamm befindet. Verwenden Sie beispielsweise die Argumente `--chdir knboard/backend config.wsgi`, wenn sich die Datei *wsgi.py* bei Ihnen unter *knboard/backend/config* im Projektstamm befindet.

Fügen Sie zum Aktivieren der Produktionsprotokollierung die Parameter `--access-logfile` und `--error-logfile` so hinzu, wie dies in den Beispielen für [benutzerdefinierte Startbefehle](#customize-startup-command) veranschaulicht wird.

### <a name="flask-app"></a>Flask-App

Für Flask sucht App Service nach der Datei *application.py* oder *app.py* und startet Gunicorn:

```bash
# If application.py
gunicorn --bind=0.0.0.0 --timeout 600 application:app

# If app.py
gunicorn --bind=0.0.0.0 --timeout 600 app:app
```

Falls Ihr App-Hauptmodul in einer anderen Datei enthalten ist, sollten Sie einen anderen Namen für das App-Objekt verwenden. Wenn Sie zusätzliche Argumente für Gunicorn angeben möchten, können Sie einen [benutzerdefinierten Startbefehl](#customize-startup-command) nutzen.

### <a name="default-behavior"></a>Standardverhalten

Wenn App Service keinen benutzerdefinierten Befehl und keine Django-App oder Flask-App findet, wird eine schreibgeschützte Standard-App ausgeführt, die sich im Ordner _opt/defaultsite_ befindet. Die Standard-App wird wie folgt angezeigt:

![Standardwebseite für App Service für Linux](media/configure-language-python/default-python-app.png)

## <a name="customize-startup-command"></a>Anpassen des Startbefehls

Wie in diesem Artikel bereits erwähnt, können Sie Konfigurationseinstellungen für Gunicorn über die Datei *gunicorn.conf.py* im Projektstamm angeben. Dies ist in der [Übersicht zur Gunicorn-Konfiguration](https://docs.gunicorn.org/en/stable/configure.html#configuration-file) beschrieben.

Wenn diese Konfiguration nicht ausreicht, können Sie das Startverhalten des Containers steuern, indem Sie entweder einen benutzerdefinierten Startbefehl oder mehrere Befehle in einer Datei mit Startbefehlen angeben. Für eine Datei mit Startbefehlen können Sie einen beliebigen Namen wählen, z. B. *startup.sh* , *startup.cmd* , *startup.txt* usw.

Für alle Befehle müssen relative Pfade zum Stammordner des Projekts verwendet werden.

Geben Sie einen Startbefehl oder eine Befehlsdatei wie folgt an:

- **Azure-Portal** : Wählen Sie die Seite **Konfiguration** der App und dann die Option **Allgemeine Einstellungen** aus. Geben Sie im Feld **Startbefehl** entweder den vollständigen Text Ihres Startbefehls oder den Namen Ihrer Startbefehlsdatei an. Klicken Sie anschließend auf **Speichern** , um die Änderungen zu übernehmen. Weitere Informationen finden Sie unter [Konfigurieren von allgemeinen Einstellungen](configure-common.md#configure-general-settings) für Linux-Container.

- **Azure CLI** : Verwenden Sie den Befehl [az webapp config set](/cli/azure/webapp/config#az_webapp_config_set) mit dem Parameter `--startup-file`, um den Startbefehl bzw. die Datei festzulegen:

    ```azurecli-interactive
    az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
    ```
        
    Ersetzen Sie `<custom-command>` entweder durch den vollständigen Text Ihres Startbefehls oder durch den Namen Ihrer Startbefehlsdatei.
        
App Service ignoriert alle Fehler, die beim Verarbeiten eines benutzerdefinierten Startbefehls oder einer Befehlsdatei auftreten, und setzt den Startprozess dann fort, indem nach Django- und Flask-Apps gesucht wird. Falls das erwartete Verhalten nicht eintritt, sollten Sie überprüfen, ob Ihr Startbefehl bzw. die Datei fehlerfrei ist und ob zusammen mit Ihrem App-Code eine Startbefehlsdatei für App Service bereitgestellt wird. Weitere Informationen finden Sie auch in den [Diagnoseprotokollen](#access-diagnostic-logs). Überprüfen Sie auch die Seite **Diagnose und Problembehandlung** der App im [Azure-Portal](https://portal.azure.com).

### <a name="example-startup-commands"></a>Beispiele für Startbefehle

- **Hinzugefügte Gunicorn-Argumente** : Im folgenden Beispiel wird `--workers=4` einer Gunicorn-Befehlszeile hinzugefügt, um eine Django-App zu starten: 

    ```bash
    # <module-path> is the relative path to the folder that contains the module
    # that contains wsgi.py; <module> is the name of the folder containing wsgi.py.
    gunicorn --bind=0.0.0.0 --timeout 600 --workers=4 --chdir <module_path> <module>.wsgi
    ```    

    Weitere Informationen finden Sie unter [Ausführen von Gunicorn](https://docs.gunicorn.org/en/stable/run.html) (docs.gunicorn.org).

- **Aktivieren der Produktionsprotokollierung für Django** : Fügen Sie der Befehlszeile die Argumente `--access-logfile '-'` und `--error-logfile '-'` hinzu:

    ```bash    
    # '-' for the log files means stdout for --access-logfile and stderr for --error-logfile.
    gunicorn --bind=0.0.0.0 --timeout 600 --workers=4 --chdir <module_path> <module>.wsgi --access-logfile '-' --error-logfile '-'
    ```    

    Diese Protokolle werden im [App Service-Protokolldatenstrom](#access-diagnostic-logs) angezeigt.

    Weitere Informationen finden Sie unter [Gunicorn-Protokollierung](https://docs.gunicorn.org/en/stable/settings.html#logging) (docs.gunicorn.org).
    
- **Benutzerdefiniertes Flask-Hauptmodul** : Standardmäßig wird von App Service angenommen, dass das Hauptmodul einer Flask-App *application.py* oder *app.py* lautet. Falls für Ihr Hauptmodul ein anderer Name verwendet wird, müssen Sie den Startbefehl anpassen. Wenn Sie beispielsweise über eine Flask-App verfügen, deren Hauptmodul *hello.py* lautet, und das Flask-App-Objekt in dieser Datei den Namen `myapp` hat, lautet der Befehl wie folgt:

    ```bash
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```
    
    Wenn Ihr Hauptmodul in einem Unterordner, z. B. `website` enthalten ist, können Sie diesen Ordner mit dem Argument `--chdir` angeben:
    
    ```bash
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
    ```
    
- **Verwenden eines anderen Servers als eines Gunicorn-Servers** : Verwenden Sie für die Nutzung eines anderen Webservers, z. B. [aiohttp](https://aiohttp.readthedocs.io/en/stable/web_quickstart.html), den entsprechenden Befehl als Startbefehl oder in der Startbefehlsdatei:

    ```bash
    python3.7 -m aiohttp.web -H localhost -P 8080 package.module:init_func
    ```

## <a name="access-app-settings-as-environment-variables"></a>Zugreifen auf App-Einstellungen als Umgebungsvariablen

App-Einstellungen sind Werte, die in der Cloud speziell für Ihre App gespeichert werden. Dies ist unter [Konfigurieren von App-Einstellungen](configure-common.md#configure-app-settings) beschrieben. Diese Einstellungen sind für Ihren App-Code als Umgebungsvariablen verfügbar, und der Zugriff darauf erfolgt mit dem Standardmuster [os.environ](https://docs.python.org/3/library/os.html#os.environ).

Wenn Sie beispielsweise eine App-Einstellung mit dem Namen `DATABASE_SERVER` erstellt haben, wird der Wert dieser Einstellung mit dem folgenden Code abgerufen:

```python
db_server = os.environ['DATABASE_SERVER']
```
    
## <a name="detect-https-session"></a>Erkennen einer HTTPS-Sitzung

In App Service erfolgt die [SSL-Terminierung](https://wikipedia.org/wiki/TLS_termination_proxy) (wikipedia.org) in den Modulen für den Netzwerklastenausgleich, sodass alle HTTPS-Anforderungen Ihre App als unverschlüsselte HTTP-Anforderungen erreichen. Wenn Ihre App-Logik überprüfen muss, ob Benutzeranforderungen verschlüsselt sind, können Sie dazu den Header `X-Forwarded-Proto` untersuchen.

```python
if 'X-Forwarded-Proto' in request.headers and request.headers['X-Forwarded-Proto'] == 'https':
# Do something when HTTPS is used
```

Gängige Webframeworks ermöglichen den Zugriff auf die Information `X-Forwarded-*` in Ihrem App-Standardmuster. In [CodeIgniter](https://codeigniter.com/) überprüft [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) standardmäßig den Wert von `X_FORWARDED_PROTO`.

## <a name="access-diagnostic-logs"></a>Zugreifen auf Diagnoseprotokolle

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

Wählen Sie im Menü auf der linken Seite für Ihre App die Option **Überwachung** > **Protokolldatenstrom** aus, um über das Azure-Portal auf Protokolle zuzugreifen.

## <a name="open-ssh-session-in-browser"></a>Öffnen einer SSH-Sitzung im Browser

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Problembehandlung

- **Die Standard-App wird angezeigt, nachdem Sie Ihren eigenen App-Code bereitgestellt haben.** Die Standard-App wird angezeigt, da Sie entweder Ihren App-Code nicht in App Service bereitgestellt haben oder App Service Ihren App-Code nicht finden konnte und stattdessen die Standard-App ausgeführt hat.

    - Starten Sie App Service neu, warten Sie 15 bis 20 Sekunden, und prüfen Sie die App dann erneut.
    
    - Stellen Sie sicher, dass Sie App Service für Linux verwenden (keine Windows-basierte Instanz). Führen Sie über die Azure CLI den Befehl `az webapp show --resource-group <resource-group-name> --name <app-name> --query kind` aus, und ersetzen Sie jeweils `<resource-group-name>` und `<app-service-name>`. `app,linux` sollte als Ausgabe angezeigt werden. Erstellen Sie andernfalls die App Service-Instanz neu, und wählen Sie Linux aus.
    
    - Verwenden Sie SSH oder die Kudu-Konsole, um eine direkte Verbindung mit App Service herzustellen, und stellen Sie sicher, dass Ihre Dateien unter *site/wwwroot* vorhanden sind. Falls Ihre Dateien nicht vorhanden sind, sollten Sie Ihren Bereitstellungsprozess überprüfen und die App erneut bereitstellen.
    
    - Wenn Ihre Dateien vorhanden sind, konnte App Service Ihre spezifische Startdatei nicht identifizieren. Überprüfen Sie, ob Ihre App so strukturiert ist, wie App Service dies für [Django](#django-app) oder [Flask](#flask-app) erwartet, oder verwenden Sie einen [benutzerdefinierten Startbefehl](#customize-startup-command).

- **Im Browser wird die Meldung „Dienst nicht verfügbar“ angezeigt.** Für den Browser ist ein Timeout aufgetreten, während auf eine Antwort von App Service gewartet wurde. Dies weist darauf hin, dass App Service den Gunicorn-Server gestartet hat, aber die Argumente, mit denen der App-Code angegeben wird, fehlerhaft sind.

    - Aktualisieren Sie den Browser. Dies gilt besonders, wenn Sie in Ihrem App Service-Plan die niedrigsten Tarife nutzen. Bei Verwendung von Free-Tarifen kann das Starten der App beispielsweise länger dauern. Sie reagiert dann wieder, nachdem Sie den Browser aktualisiert haben.

    - Überprüfen Sie, ob Ihre App so strukturiert ist, wie App Service dies für [Django](#django-app) oder [Flask](#flask-app) erwartet, oder verwenden Sie einen [benutzerdefinierten Startbefehl](#customize-startup-command).

    - Untersuchen Sie den [Protokolldatenstrom](#access-diagnostic-logs) auf Fehlermeldungen.

- **Für den Protokolldatenstrom wird eine Fehlermeldung der Art „setup.py oder requirements.txt nicht gefunden. pip-Installation wird nicht ausgeführt.“ angezeigt** : Ihre Datei *requirements.txt* konnte während des Oryx-Buildprozesses nicht gefunden werden.

    - Verwenden Sie SSH oder die Kudu-Konsole, um eine direkte Verbindung mit App Service herzustellen, und stellen Sie sicher, dass *requirements.txt* direkt unter *site/wwwroot* vorhanden ist. Falls nicht, sollen Sie sicherstellen, dass sich die Datei in Ihrem Repository befindet und Teil Ihrer Bereitstellung ist. Verschieben Sie sie in den Stamm, wenn sie in einem separaten Ordner enthalten ist.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Python-App mit PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Tutorial: Bereitstellen aus privatem Containerrepository](tutorial-custom-container.md?pivots=container-linux)

> [!div class="nextstepaction"]
> [Häufig gestellte Fragen (FAQ) zu Azure App Service unter Linux](faq-app-service-linux.md)
