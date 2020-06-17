---
title: 'Tutorial: Bereitstellen von Python (Django) mit PostgreSQL'
description: Hier erfahren Sie, wie Sie eine Python-App mit einer PostgreSQL-Datenbank erstellen und in Azure App Service für Linux bereitstellen. In diesem Tutorial wird zu Demonstrationszwecken eine Django-Beispiel-App verwendet.
ms.devlang: python
ms.topic: tutorial
ms.date: 04/14/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
- tracking-python
ms.openlocfilehash: 4a2f80ea30fc68ae1dfea72983fd2b229d40c711
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84559287"
---
# <a name="tutorial-deploy-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Tutorial: Bereitstellen einer Python-Web-App (Django) mit PostgreSQL in Azure App Service

In diesem Tutorial wird veranschaulicht, wie Sie eine datengesteuerte Python-Web-App (Django) für [Azure App Service](app-service-linux-intro.md) bereitstellen und mit einer Azure Database for PostgreSQL-Datenbankinstanz verbinden. Von App Service wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt.

![Bereitstellen der Python Django-Web-App in Azure App Service](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Azure Database for PostgreSQL-Datenbankinstanz
> * Bereitstellen von Code für Azure App Service und Herstellen einer Verbindung mit Postgres
> * Aktualisieren Ihres Codes und erneutes Bereitstellen
> * Anzeigen von Diagnoseprotokollen
> * Verwalten der Web-App im Azure-Portal

Sie können die Schritte in diesem Artikel auch unter macOS, Linux oder Windows ausführen.

## <a name="install-dependencies"></a>Installieren von Abhängigkeiten

Stellen Sie Folgendes sicher, bevor Sie mit diesem Tutorial beginnen:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- Installieren Sie die [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).
- Installieren Sie [Git](https://git-scm.com/).
- Installieren Sie [Python 3](https://www.python.org/downloads/).

## <a name="clone-the-sample-app"></a>Klonen der Beispiel-App

Führen Sie in einem Terminalfenster die folgenden Befehle aus, um das Beispiel-App-Repository zu klonen, und wechseln Sie in den Repositorystamm:

```
git clone https://github.com/Azure-Samples/djangoapp
cd djangoapp
```

Das Beispielrepository „djangoapp“ enthält die datengesteuerte [Django](https://www.djangoproject.com/)-Umfrage-App, die Sie erhalten, indem Sie in der Django-Dokumentation die Schritte unter [Schreiben Ihrer ersten Django-App](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) ausführen. Sie wird Ihnen hier zur Verfügung gestellt.

## <a name="prepare-app-for-app-service"></a>Vorbereiten der App für App Service

Wie viele Python-Webframeworks erfordert Django [bestimmte Änderungen, bevor die Ausführung auf einem Produktionsserver möglich ist](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/), und das ist bei App Service nicht anders. Sie müssen einige Einstellungen in der Standarddatei *azuresite/settings.py* ändern und hinzufügen, damit die App nach der Bereitstellung für App Service funktioniert. 

Sehen Sie sich *azuresite/production.py* an, wo die erforderliche Konfiguration für App Service vorgenommen wird. Sie führt im Wesentlichen die folgenden Aktionen aus:

- Erben aller Einstellungen von *azuresite/settings.py*.
- Hinzufügen des voll qualifizierten Domänennamens der App Service-App zu den zulässigen Hosts. 
- Verwenden von [WhiteNoise](https://whitenoise.evans.io/en/stable/), um die Bereitstellung statischer Dateien in der Produktion zu aktivieren, da Django standardmäßig keine statischen Dateien in der Produktion bereitstellt. Das WhiteNoise-Paket ist bereits in *requirements.txt* enthalten.
- Hinzufügen einer Konfiguration für die PostgreSQL-Datenbank. Standardmäßig verwendet Django Sqlite3 als Datenbank, dies eignet sich jedoch nicht für Produktions-Apps. Das [psycopg2-binary](https://pypi.org/project/psycopg2-binary/)-Paket ist bereits in *requirements.txt* enthalten.
- Die Postgres-Konfiguration verwendet Umgebungsvariablen. Später erfahren Sie, wie Sie Umgebungsvariablen in App Service festlegen.

*azuresite/production.py* ist aus praktischen Gründen im Repository enthalten, wird aber noch nicht von der App verwendet. Um sicherzustellen, dass die Einstellungen in App Service verwendet werden, müssen Sie für den Zugriff darauf die zwei Dateien *manage.py* und *azuresite/wsgi.py* konfigurieren.

- Ändern Sie in *manage.py* die folgende Zeile:

    <pre>
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.settings')
    </pre>

    Geänderter Code:

    ```python
    if os.environ.get('DJANGO_ENV') == 'production':
        os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.production')
    else:
        os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.settings')
    ```

    Sie legen die Umgebungsvariable `DJANGO_ENV` später fest, wenn Sie Ihre App Service-App konfigurieren.

- Nehmen Sie in *azuresite/wsgi.py* die gleiche Änderung wie oben vor.

    In App Service verwenden Sie *manage.py* zum Ausführen von Datenbankmigrationen, und App Service verwendet *azuresite/wsgi.py*, um Ihre Django-App in der Produktionsumgebung auszuführen. Diese Änderung in beiden Dateien stellt sicher, dass die Produktionseinstellungen in beiden Fällen verwendet werden.

## <a name="sign-in-to-azure-cli"></a>Anmelden bei der Azure-Befehlszeilenschnittstelle

Die Azure-Befehlszeilenschnittstelle muss bereits installiert sein. Mit [Azure CLI](/cli/azure/what-is-azure-cli) können Sie über das Befehlszeilenterminal mit Azure-Ressourcen arbeiten. 

Verwenden Sie den [`az login`](/cli/azure/reference-index#az-login)-Befehl, um sich bei Azure anzumelden:

```azurecli
az login
```

Befolgen Sie die Anweisungen zum Anmelden bei Ihrem Azure-Konto im Terminal. Wenn Sie fertig sind, werden Ihre Abonnements im JSON-Format in der Terminalausgabe angezeigt.

## <a name="create-postgres-database-in-azure"></a>Erstellen einer Postgres-Datenbankinstanz in Azure

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) is available in your region. If your region doesn't support Gen4 hardware, change *--sku-name* in the following command line to a value that's supported in your region, such as B_Gen4_1.  -->

In diesem Abschnitt erstellen Sie eine Azure Database for PostgreSQL-Serverinstanz und eine Datenbank in Azure. Installieren Sie zu Beginn die `db-up`-Erweiterung mit folgendem Befehl:

```azurecli
az extension add --name db-up
```

Erstellen Sie die Postgres-Datenbankinstanz wie im folgenden Beispiel gezeigt mit dem Befehl [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) in Azure. Ersetzen Sie *\<postgresql-name>* durch einen *eindeutigen* Namen (der Serverendpunkt ist *https://\<postgresql-name>.postgres.database.azure.com*). Geben Sie für *\<admin-username>* und *\<admin-password>* Anmeldeinformationen ein, um einen Administratorbenutzer für diesen Postgres-Server zu erstellen.

<!-- Issue: without --location -->
```azurecli
az postgres up --resource-group myResourceGroup --location westus2 --server-name <postgresql-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

Dieser Befehl kann einige Zeit in Anspruch nehmen, da er Folgendes bewirkt:

- Er erstellt eine [Ressourcengruppe](../../azure-resource-manager/management/overview.md#terminology) namens `myResourceGroup`, wenn diese noch nicht vorhanden ist. Jede Azure-Ressource muss sich in einer dieser Ressourcengruppen befinden. `--resource-group` ist optional.
- Er erstellt eine Postgres-Serverinstanz mit dem Administratorbenutzer.
- Er erstellt eine `pollsdb`-Datenbank.
- Er ermöglicht den Zugriff über Ihre lokale IP-Adresse.
- Er ermöglicht den Zugriff über Azure-Dienste.
- Er erstellt einen neuen Datenbankbenutzer mit Zugriff auf die `pollsdb`-Datenbank.

Sie können alle Schritte separat mit anderen `az postgres`-Befehlen und `psql` ausführen, aber `az postgres up` führt alle in einem Schritt für Sie aus.

Wenn der Befehl abgeschlossen ist, suchen Sie die Ausgabezeilen, die mit `Ran Database Query:` beginnen. Sie zeigen den Datenbankbenutzer, der für Sie erstellt wurde, mit Benutzernamen `root` und Kennwort `Pollsdb1` an. Sie werden sie später verwenden, um die Verbindung Ihrer App mit der Datenbank herzustellen.

<!-- not all locations support az postgres up -->
> [!TIP]
> `--location <location-name>`: Kann auf eine beliebige [Azure-Region](https://azure.microsoft.com/global-infrastructure/regions/) festgelegt werden. Sie können die Regionen, die für Ihr Abonnement verfügbar sind, mit dem [`az account list-locations`](/cli/azure/account#az-account-list-locations)-Befehl abrufen. Legen Sie bei Produktions-Apps für Ihre Datenbank und Ihre App den gleichen Standort fest.

## <a name="deploy-the-app-service-app"></a>Bereitstellen der App Service-App

In diesem Abschnitt erstellen Sie die App Service-App. Sie stellen die Verbindung dieser App mit der Postgres-Datenbankinstanz her, die Sie erstellt haben, und stellen Ihren Code bereit.

### <a name="create-the-app-service-app"></a>Erstellen der App Service-App

<!-- validation error: Parameter 'ResourceGroup.location' can not be None. -->
<!-- --resource-group is not respected at all -->

Stellen Sie sicher, dass Sie sich wieder im Repositorystamm (`djangoapp`) befinden, da die App aus diesem Verzeichnis bereitgestellt wird.

Erstellen Sie mit dem Befehl [`az webapp up`](/cli/azure/webapp#az-webapp-up) eine App Service-App, wie im folgenden Beispiel gezeigt wird. Ersetzen Sie *\<app-name>* durch einen *eindeutigen* Namen (der Serverendpunkt ist *https://\<app-name>.azurewebsites.net*). Zulässige Zeichen für *\<app-name>* sind `A`-`Z`, `0`-`9` und `-`.

```azurecli
az webapp up --plan myAppServicePlan --location westus2 --sku B1 --name <app-name>
```
<!-- !!! without --sku creates PremiumV2 plan!! -->

Dieser Befehl kann einige Zeit in Anspruch nehmen, da er Folgendes bewirkt:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Er generiert automatisch eine [Ressourcengruppe](../../azure-resource-manager/management/overview.md#terminology).
- Er erstellt den [App Service-Plan](../overview-hosting-plans.md) *myAppServicePlan* im Basic-Tarif (B1), falls er nicht vorhanden ist. `--plan` und `--sku` sind optional.
- Er erstellt die App Service-App, wenn sie nicht vorhanden ist.
- Er aktiviert die Standardprotokollierung für die App, sofern sie nicht bereits aktiviert ist.
- Er lädt das Repository mithilfe der ZIP-Bereitstellung mit aktivierter Buildautomatisierung hoch.

Nachdem die Bereitstellung abgeschlossen ist, wird eine JSON-Ausgabe wie die folgende angezeigt:

<pre>
{
  "URL": "http://&lt;app-name&gt;.azurewebsites.net",
  "appserviceplan": "myAppServicePlan",
  "location": "westus",
  "name": "&lt;app-name&gt;",
  "os": "Linux",
  "resourcegroup": "&lt;app-resource-group&gt;",
  "runtime_version": "python|3.7",
  "runtime_version_detected": "-",
  "sku": "BASIC",
  "src_path": "//var//lib//postgresql//djangoapp"
}
</pre>

Kopieren Sie den Wert von *\<app-resource-group>* . Sie benötigen ihn, um die App später zu konfigurieren. 

> [!TIP]
> Die entsprechenden Einstellungen werden in einem ausgeblendeten Verzeichnis vom Typ *.azure* in Ihrem Repository gespeichert. Sie können den einfachen Befehl später zum erneuten Bereitstellen von Änderungen und zum sofortigen Aktivieren von Diagnoseprotokollen verwenden:
> 
> ```azurecli
> az webapp up
> ```

Der Beispielcode wird jetzt bereitgestellt, aber die App stellt noch keine Verbindung mit der Postgres-Datenbankinstanz in Azure her. Diesen Schritt führen Sie als Nächstes durch.

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Wenn Sie Ihre App lokal ausführen, können Sie die Umgebungsvariablen in der Terminalsitzung festlegen. In App Service legen Sie hierzu *App-Einstellungen* mit dem Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) fest.

Führen Sie den folgenden Befehl aus, um die Details der Datenbankverbindung als App-Einstellungen anzugeben. Ersetzen Sie *\<app-name>* , *\<app-resource-group>* und *\<postgresql-name>* durch Ihre eigenen Werte. Beachten Sie, dass die Benutzeranmeldeinformationen `root` und `Pollsdb1` von `az postgres up` für Sie erstellt wurden.

```azurecli
az webapp config appsettings set --name <app-name> --resource-group <app-resource-group> --settings DJANGO_ENV="production" DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="root@<postgresql-name>" DBPASS="Pollsdb1" DBNAME="pollsdb"
```

Informationen dazu, wie in Ihrem Code auf diese App-Einstellungen zugegriffen wird, finden Sie unter [Zugreifen auf Umgebungsvariablen](how-to-configure-python.md#access-environment-variables).

### <a name="run-database-migrations"></a>Ausführen von Datenbankmigrationen

Um Datenbankmigrationen in App Service auszuführen, öffnen Sie eine SSH-Sitzung im Browser, indem Sie zu *https://\<app-name>.scm.azurewebsites.net/webssh/host* navigieren:

<!-- doesn't work when container not started -->
<!-- ```azurecli
az webapp ssh --resource-group myResourceGroup --name <app-name>
``` -->

Führen Sie in der SSH-Sitzung folgende Befehle aus:

```bash
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
# Create the super user (follow prompts)
python manage.py createsuperuser
```

### <a name="browse-to-the-azure-app"></a>Navigieren zur Azure-App

Greifen Sie in einem Browser über die URL *http:\//\<app-name>.azurewebsites.net* auf die bereitgestellte App zu. Es sollte eine Meldung wie **No polls are available** (Keine Umfragen verfügbar) angezeigt werden. 

Navigieren Sie zu *http:\//\<app-name>.azurewebsites.net/admin*, und melden Sie sich als der Administratorbenutzer an, den Sie im letzten Schritt erstellt haben. Wählen Sie neben **Fragen** die Option **Hinzufügen** aus, und erstellen Sie eine Frage für eine Umfrage mit mehreren Auswahlmöglichkeiten.

Greifen Sie über die URL *http:\//\<app-name>.azurewebsites.net/admin* auf die bereitgestellte App zu, und erstellen Sie einige Fragen für die Umfrage. Die Fragen finden Sie unter *http:\//\<app-name>.azurewebsites.net/* . 

![Ausführen einer Python-Django-App in App Services in Azure](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

Greifen Sie über die URL *http:\//\<app-name>.azurewebsites.net* wieder auf die bereitgestellte App zu, um die Frage für die Umfrage anzuzeigen, und beantworten Sie die Frage.

App Service erkennt ein Django-Projekt in Ihrem Repository, indem in jedem Unterverzeichnis nach der Datei *wsgi.py* gesucht wird. Das Unterverzeichnis wird durch `manage.py startproject` standardmäßig erstellt. Wenn die Datei von App Service gefunden wird, wird die Django-Web-App geladen. Weitere Informationen dazu, wie App Service Python-Apps lädt, finden Sie unter [Konfigurieren Ihrer Python-App für Azure App Service unter Linux](how-to-configure-python.md).

**Glückwunsch!** Sie führen eine Python-Web-App (Django) in Azure App Service für Linux aus.

## <a name="develop-app-locally-and-redeploy"></a>Lokales Entwickeln der App und erneutes Bereitstellen

In diesem Abschnitt entwickeln Sie Ihre App in Ihrer lokalen Umgebung und stellen Ihren Code erneut für App Service bereit.

### <a name="set-up-locally-and-run"></a>Lokales Einrichten und Ausführen

Um Ihre lokale Entwicklungsumgebung einzurichten und die Beispiel-App zum ersten Mal auszuführen, führen Sie die folgenden Befehle aus:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
# Configure the Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# Configure the Python virtual environment
py -3 -m venv venv
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
venv\scripts\activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="cmd"></a>[Befehlszeile](#tab/cmd)

```CMD
:: Configure the Python virtual environment
py -3 -m venv venv
venv\scripts\activate

:: Install packages
pip install -r requirements.txt
:: Run Django migrations
python manage.py migrate
:: Create Django superuser (follow prompts)
python manage.py createsuperuser
:: Run the dev server
python manage.py runserver
```
---

Nachdem die Django-Web-App vollständig geladen wurde, wird in etwa die folgende Meldung zurückgegeben:

<pre>
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
</pre>

Navigieren Sie in einem Browser zu *http:\//localhost:8000*. Es sollte eine Meldung wie **No polls are available** (Keine Umfragen verfügbar) angezeigt werden. 

Navigieren Sie zu *http:\//localhost:8000/admin*, und melden Sie sich mit dem Administratorbenutzer an, den Sie im letzten Schritt erstellt haben. Wählen Sie neben **Fragen** die Option **Hinzufügen** aus, und erstellen Sie eine Frage für eine Umfrage mit mehreren Auswahlmöglichkeiten.

![Lokales Ausführen einer Python-Django-App in App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Navigieren Sie erneut zu *http:\//localhost:8000*, um die Frage der Umfrage anzuzeigen, und beantworten Sie die Frage. Die lokale Django-Beispielanwendung schreibt und speichert Benutzerdaten in eine lokale Sqlite3-Datenbank, sodass Sie sich nicht darum kümmern müssen, ihre Produktionsdatenbank zu bereinigen. Damit Ihre Entwicklungsumgebung der Azure-Umgebung entspricht, sollten Sie stattdessen eine Postgres-Datenbankinstanz lokal verwenden.

Um den Django-Server zu beenden, geben Sie STRG+C ein.

### <a name="update-the-app"></a>Aktualisieren der App

Um zu erfahren, wie App-Updates funktionieren, nehmen Sie eine kleine Änderung in `polls/models.py` vor. Suchen Sie die folgende Zeile:

<pre>
choice_text = models.CharField(max_length=200)
</pre>

Ändern Sie sie wie folgt:

```python
choice_text = models.CharField(max_length=100)
```

Wenn Sie das Datenmodell ändern, müssen Sie eine neue Django-Migration erstellen. Benutzen Sie dazu folgenden Befehl:

```
python manage.py makemigrations
```

Sie können Ihre Änderungen lokal testen, indem Sie Migrationen ausführen, den Entwicklungsserver ausführen und zu *http:\//localhost:8000/admin* navigieren:

```
python manage.py migrate
python manage.py runserver
```

### <a name="redeploy-code-to-azure"></a>Erneutes Bereitstellen von Code in Azure

Um die Änderungen erneut bereitzustellen, führen Sie den folgenden Befehl im Repositorystamm aus:

```azurecli
az webapp up
```

App Service erkennt, dass die App vorhanden ist, und stellt lediglich den Code bereit.

### <a name="rerun-migrations-in-azure"></a>Erneutes Ausführen von Migrationen in Azure

Da Sie Änderungen am Datenmodell vorgenommen haben, müssen Sie die Datenbankmigrationen in App Service erneut ausführen. Öffnen Sie eine SSH-Sitzung im Browser, indem Sie zu *https://\<app-name>.scm.azurewebsites.net/webssh/host* navigieren. Führen Sie die folgenden Befehle aus:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

### <a name="review-app-in-production"></a>Überprüfen der App in der Produktion

Navigieren Sie zu *http:\//\<app-name>.azurewebsites.net*, und sehen Sie sich die Ausführung der Änderungen live in der Produktion an. 

## <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

Sie können auf die Konsolenprotokolle zugreifen, die aus dem Container generiert werden.

> [!TIP]
> `az webapp up` aktiviert die Standardprotokollierung für Sie. Aus Leistungsgründen wird diese Protokollierung nach einiger Zeit deaktiviert, aber jedes Mal wieder eingeschaltet, wenn Sie `az webapp up` erneut ausführen. Führen Sie zur manuellen Aktivierung folgenden Befehl aus:
>
> ```azurecli
> az webapp log config --name <app-name> --resource-group <app-resource-group> --docker-container-logging filesystem
> ```

Führen Sie den folgenden Azure CLI-Befehl aus, um den Protokolldatenstrom anzuzeigen:

```azurecli
az webapp log tail --name <app-name> --resource-group <app-resource-group>
```

Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

> [!NOTE]
> Sie können die Protokolldateien auch im Browser unter `https://<app-name>.scm.azurewebsites.net/api/logs/docker` untersuchen.

Um das Protokollstreaming jederzeit zu beenden, geben Sie `Ctrl`+`C` ein.

## <a name="manage-your-app-in-the-azure-portal"></a>Überwachen Ihrer App im Azure-Portal

Suchen Sie im [Azure-Portal](https://portal.azure.com) nach der von Ihnen erstellten App, und wählen Sie sie aus.

![Navigieren zur Python-Django-App im Azure-Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben durchführen, z. B. Durchsuchen, Beenden, Neustarten und Löschen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![Verwalten der Python-Django-App auf der Seite „Übersicht“ im Azure-Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Ressourcen voraussichtlich nicht mehr benötigen, löschen Sie die Ressourcengruppen mit folgendem Befehl:

```azurecli
az group delete --name myResourceGroup
az group delete --name <app-resource-group>
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde Folgendes beschrieben:

> [!div class="checklist"]
> * Erstellen einer Azure Database for PostgreSQL-Datenbankinstanz
> * Bereitstellen von Code für Azure App Service und Herstellen einer Verbindung mit Postgres
> * Aktualisieren Ihres Codes und erneutes Bereitstellen
> * Anzeigen von Diagnoseprotokollen
> * Verwalten der Web-App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen:

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](../app-service-web-tutorial-custom-domain.md)

Oder sehen Sie sich weitere Ressourcen an:

> [!div class="nextstepaction"]
> [Konfigurieren einer Python-App](how-to-configure-python.md)
