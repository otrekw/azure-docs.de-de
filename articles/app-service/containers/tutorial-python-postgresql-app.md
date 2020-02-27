---
title: 'Tutorial: Linux-Python-App mit PostgreSQL'
description: Hier erfahren Sie, wie Sie eine Linux-Python-App in Azure App Service ausführen, die mit einer PostgreSQL-Datenbank in Azure verbunden ist. In diesem Tutorial wird Django verwendet.
ms.devlang: python
ms.topic: tutorial
ms.date: 01/23/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
ms.openlocfilehash: 13431b62e64774a4c31cf95200def3ba77f973d7
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2020
ms.locfileid: "77523932"
---
# <a name="tutorial-run-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Tutorial: Ausführen einer Python-Web-App (Django) mit PostgreSQL in Azure App Service

Von [Azure App Service](app-service-linux-intro.md) wird ein hochgradig skalierbarer Webhostingdienst mit Self-Patching bereitgestellt. In diesem Tutorial wird veranschaulicht, wie Sie eine datengesteuerte Python Django-Web-App mit einer Azure Database for PostgreSQL-Datenbank verbinden und die App unter Azure App Service bereitstellen und ausführen.

![Python Django-Web-App in Azure App Service](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Azure Database for PostgreSQL-Datenbank und Herstellen einer Verbindung mit einer Web-App
> * Bereitstellen der Web-App in Azure App Service
> * Anzeigen von Diagnoseprotokollen
> * Verwalten der Web-App im Azure-Portal

Sie können die Schritte in diesem Artikel auch unter macOS, Linux oder Windows ausführen. Die Schritte ähneln sich meist, und die Unterschiede werden in diesem Tutorial nicht ausführlich beschrieben. In den meisten der folgenden Beispiele wird ein `bash`-Terminalfenster unter Linux verwendet. 

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie mit diesem Tutorial beginnen:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- Installieren Sie [Git](https://git-scm.com/).
- Installieren Sie [Python 3](https://www.python.org/downloads/).
- Installieren Sie [PostgreSQL](https://www.postgresql.org/download/), und führen Sie das System aus.

## <a name="test-postgresql-installation-and-create-a-database"></a>Testen der PostgreSQL-Installation und Erstellen einer Datenbank

Stellen Sie zunächst eine Verbindung mit Ihrem lokalen PostgreSQL-Server her, und erstellen Sie eine Datenbank: 

Führen Sie in einem Terminalfenster `psql` aus, um als integrierter `postgres`-Benutzer eine Verbindung mit Ihrem lokalen PostgreSQL-Server herzustellen.

```bash
sudo su - postgres
psql
```
oder
```PowerShell
psql -U postgres
```

Wenn die Verbindung erfolgreich ist, wird die PostgreSQL-Datenbank ausgeführt. Stellen Sie andernfalls sicher, dass die lokale PostgreSQL-Datenbank gestartet wurde, indem Sie die Anweisungen für Ihr Betriebssystem unter [Downloads: PostgreSQL Core Distribution](https://www.postgresql.org/download/) befolgen.

Erstellen Sie eine neue Datenbank mit dem Namen *pollsdb*, und richten Sie einen separaten Datenbankbenutzer mit dem Namen *manager* mit dem Kennwort *supersecretpass* ein:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

<a name="step2"></a>
## <a name="create-and-run-the-local-python-app"></a>Erstellen und Ausführen der lokalen Python-App

Richten Sie als Nächstes die Python Django-Beispiel-Web-App ein, und führen Sie sie aus.

Das Beispielrepository [djangoapp](https://github.com/Azure-Samples/djangoapp) enthält die datengesteuerte [Django](https://www.djangoproject.com/)-Umfrage-App, die Sie erhalten, indem Sie in der Django-Dokumentation die Schritte unter [Schreiben Ihrer ersten Django-App](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) ausführen.

### <a name="clone-the-sample-app"></a>Klonen der Beispiel-App

Führen Sie in einem Terminalfenster die folgenden Befehle aus, um das Beispiel-App-Repository zu klonen, und wechseln Sie in das neue Arbeitsverzeichnis:

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

### <a name="configure-the-python-virtual-environment"></a>Konfigurieren der virtuellen Python-Umgebung

Erstellen und aktivieren Sie eine virtuelle Python-Umgebung, um Ihre App auszuführen.

```bash
python3 -m venv venv
source venv/bin/activate
```
oder
```PowerShell
py -3 -m venv venv
venv\scripts\activate
```

Führen Sie in der `venv`-Umgebung *env.sh* oder *env.ps1* aus, um die Umgebungsvariablen festzulegen, die von *azuresite/settings.py* für die Einstellungen der Datenbankverbindung verwendet werden sollen.

```bash
source ./env.sh
```
oder
```PowerShell
.\env.ps1
```

Installieren Sie die erforderlichen Pakete aus *requirements.txt*, führen Sie [Django-Migrationen](https://docs.djangoproject.com/en/2.1/topics/migrations/) aus, und [erstellen Sie einen Administratorbenutzer](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user):

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

### <a name="run-the-web-app"></a>Ausführen der Web-App

Führen Sie den Django-Server aus, nachdem Sie den Administratorbenutzer erstellt haben.

```bash
python manage.py runserver
```

Nachdem die Django-Web-App vollständig geladen wurde, wird in etwa die folgende Meldung zurückgegeben:

```bash
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

Navigieren Sie in einem Browser zu *http:\//localhost:8000*. Es sollte eine Meldung wie **No polls are available** (Keine Umfragen verfügbar) angezeigt werden. 

Navigieren Sie zu *http:\//localhost:8000/admin*, und melden Sie sich mit dem Administratorbenutzer an, den Sie im letzten Schritt erstellt haben. Wählen Sie neben **Fragen** die Option **Hinzufügen** aus, und erstellen Sie eine Frage für eine Umfrage mit mehreren Auswahlmöglichkeiten.

![Lokales Ausführen einer Python-Django-App in App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Navigieren Sie erneut zu *http:\//localhost:8000*, um die Frage der Umfrage anzuzeigen, und beantworten Sie die Frage. Von der lokalen Django-Beispielanwendung werden Benutzerdaten in die lokale PostgreSQL-Datenbank geschrieben und darin gespeichert.

Sie können den Django-Server beenden, indem Sie im Terminal STRG+C eingeben.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

In den meisten verbleibenden Schritten dieses Artikels werden Azure CLI-Befehle in der Azure Cloud Shell verwendet. 

## <a name="create-and-connect-to-azure-database-for-postgresql"></a>Erstellen einer Azure Database for PostgreSQL-Instanz und Herstellen einer Verbindung

In diesem Abschnitt erstellen Sie einen Azure Database for PostgreSQL-Server und eine zugehörige Datenbank und stellen dafür eine Verbindung mit Ihrer Web-App her. Wenn Sie Ihre Web-App in Azure App Service bereitstellen, wird diese Clouddatenbank von der App verwendet. 

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Sie können eine neue Ressourcengruppe für Ihren Azure Database for PostgreSQL-Server erstellen oder eine vorhandene Ressourcengruppe verwenden. 

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen einer Azure-Datenbank für PostgreSQL-Server

Sie erstellen mit dem Befehl [az postgres server create](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) einen PostgreSQL-Server in der Cloud Shell.

> [!NOTE]
> Überprüfen Sie, welche [Computegeneration](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) in Ihrer Region verfügbar ist, bevor Sie einen Azure Database for PostgreSQL-Server erstellen. Falls Gen4-Hardware in Ihrer Region nicht unterstützt wird, müssen Sie *--sku-name* in der folgenden Befehlszeile in einen Wert ändern, der für Ihre Region unterstützt wird (z. B. Gen5). 

Ersetzen Sie im folgenden Befehl *\<postgresql-name>* durch einen eindeutigen Servernamen. Da der Servername Teil Ihres PostgreSQL-Endpunkts *https://\<postgresql-name>.postgres.database.azure.com* ist, muss er für alle Server in Azure eindeutig sein. 

Ersetzen Sie *\<resourcegroup-name>* und *\<region>* durch den Namen und die Region der Ressourcengruppe, die Sie verwenden möchten. Erstellen Sie für *\<admin-username>* und *\<admin-password>* Benutzeranmeldeinformationen, die für das Datenbankadministrator-Konto verwendet werden können. Notieren Sie sich *\<admin-username>* und *\<admin-password>* , damit Sie diese Angaben später zum Anmelden am PostgreSQL-Server und den Datenbanken verwenden können.

```azurecli-interactive
az postgres server create --resource-group <resourcegroup-name> --name <postgresql-name> --location "<region>" --admin-user <admin-username> --admin-password <admin-password> --sku-name B_Gen5_1
```

Nach der Erstellung des Azure Database for PostgreSQL-Servers gibt die Azure CLI JSON-Code wie im folgenden Beispiel zurück:

```json
{
  "administratorLogin": "myusername",
  "earliestRestoreDate": "2020-01-22T19:02:15.727000+00:00",
  "fullyQualifiedDomainName": "myservername.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/myservername",
  "location": "westeurope",
  "masterServerId": "",
  "name": "myservername",
  "replicaCapacity": 5,
  "replicationRole": "None",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 1,
    "family": "Gen5",
    "name": "B_Gen5_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

### <a name="create-firewall-rules-for-the-azure-database-for-postgresql-server"></a>Erstellen von Firewallregeln für den Azure Database for PostgreSQL-Server

Führen Sie den Befehl [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az-postgres-server-firewall-rule-create) aus, um den Zugriff auf die Datenbank über Azure-Ressourcen zuzulassen. Ersetzen Sie die Platzhalter *\<postgresql-name>* und *\<resourcegroup-name>* durch Ihre Werte.

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Die obige Einstellung ermöglicht Netzwerkverbindungen von allen IP-Adressen im Azure-Netzwerk. Versuchen Sie für den Produktivbetrieb möglichst restriktive Firewallregeln zu konfigurieren, indem Sie [nur die ausgehenden IP-Adressen zulassen, die von Ihrer App verwendet werden](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).

Führen Sie den Befehl `firewall-rule create` erneut aus, um den Zugriff über Ihren lokalen Computer zuzulassen. Ersetzen Sie *\<your-ip-address>* durch [Ihre lokale IPv4-IP-Adresse](https://www.whatsmyip.org/). Ersetzen Sie die Platzhalter *\<postgresql-name>* und *\<resourcegroup-name>* durch Ihre eigenen Werte.

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address> --name AllowLocalClient
```

### <a name="create-and-connect-to-the-azure-database-for-postgresql-database"></a>Erstellen einer Azure Database for PostgreSQL-Datenbank und Herstellen einer Verbindung

Stellen Sie eine Verbindung mit Ihrem Azure Database for PostgreSQL-Server her, indem Sie den folgenden Befehl ausführen. Verwenden Sie Ihre eigenen Angaben für *\<postgresql-name>* und *\<admin-username>* , und melden Sie sich mit dem von Ihnen erstellten Kennwort an.

```bash
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

Erstellen Sie auf dem Azure Database for PostgreSQL-Server eine Datenbank und einen Benutzer (genauso wie auf Ihrem lokalen PostgreSQL-Server):

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

> [!NOTE]
> Es empfiehlt sich, Datenbankbenutzer mit eingeschränkten Berechtigungen für bestimmte Apps zu erstellen, anstatt den Administratorbenutzer zu verwenden. Der Benutzer `manager` verfügt *nur* für die Datenbank `pollsdb` über vollständige Berechtigungen.

Geben Sie `\q` ein, um den PostgreSQL-Client zu beenden.

### <a name="test-app-connectivity-to-the-azure-postgresql-database"></a>Testen der App-Konnektivität mit der Azure PostgreSQL-Datenbank

Bearbeiten Sie Ihre lokale Datei *env.sh* oder *env.ps1* so, dass sie auf Ihre PostgreSQL-Clouddatenbank verweist, indem Sie *\<postgresql-name>* durch den Namen Ihres Azure Database for PostgreSQL-Servers ersetzen.

```bash
export DBHOST="<postgresql-name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql-name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"
```
oder
```powershell
$Env:DBHOST = "<postgresql-name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql-name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Führen Sie in der `venv`-Umgebung in Ihrem lokalen Terminalfenster die bearbeitete Datei *env.sh* bzw. *env.ps1* aus. 
```bash
source ./env.sh
```
oder
```PowerShell
.\env.ps1
```

Führen Sie die Django-Migration in die Azure-Datenbank aus, und erstellen Sie einen Administratorbenutzer.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Führen Sie den Django-Server aus, nachdem der Administratorbenutzer erstellt wurde.

```bash
python manage.py runserver
```

Wenn Sie in einem Browser zu *http:\//localhost:8000* navigieren, sollte wieder die Meldung **No polls are available** (Keine Umfragen verfügbar) angezeigt werden. 

Navigieren Sie zu *http:\//localhost:8000/admin*, und melden Sie sich mit dem von Ihnen erstellten Administratorbenutzer an. Erstellen Sie dann wie zuvor eine Frage für eine Umfrage.

![Lokales Ausführen einer Python-Django-App in App Services](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Navigieren Sie erneut zu *http:\//localhost:8000*, um die Frage der Umfrage anzuzeigen. Von Ihrer App werden jetzt Daten in die Azure Database for PostgreSQL-Datenbank geschrieben.

Sie können den Django-Server beenden, indem Sie im Terminal STRG+C eingeben.

## <a name="deploy-the-web-app-to-azure-app-service"></a>Bereitstellen der Web-App in Azure App Service

In diesem Schritt stellen Sie die mit der Azure Database for PostgreSQL verbundene Python-App in Azure App Service bereit.

### <a name="configure-repository"></a>Konfigurieren des Repositorys

Da in diesem Tutorial ein Django-Beispiel verwendet wird, müssen Sie in Ihrer Datei *djangoapp/azuresite/settings.py* einige Einstellungen ändern und hinzufügen, damit sie für Azure App Service funktioniert. 

1. Django überprüft den `HTTP_HOST`-Header in eingehenden Anforderungen. Damit Ihre Django-Web-App in App Service funktioniert, müssen Sie den vollqualifizierten Domänennamen der App den zulässigen Hosts hinzufügen. 
   
   Bearbeiten Sie *azuresite/settings.py*, um die Zeile `ALLOWED_HOSTS` wie folgt zu ändern:
   
   ```python
   ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
   ```
   
1. Für Django wird das [Bereitstellen von statischen Dateien in der Produktion](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/) nicht unterstützt. Verwenden Sie für dieses Tutorial [WhiteNoise](https://whitenoise.evans.io/en/stable/), um die Bereitstellung der Dateien zu ermöglichen. Das WhiteNoise-Paket wurde bereits mit *requirements.txt* installiert. 
   
   Suchen Sie zum Konfigurieren von Django für die Verwendung von WhiteNoise in *azuresite/settings.py* nach der Einstellung für `MIDDLEWARE`, und fügen Sie der Liste direkt nach der Zeile `django.middleware.security.SecurityMiddleware` den Eintrag `whitenoise.middleware.WhiteNoiseMiddleware` hinzu. Ihre `MIDDLEWARE`-Einstellung sollte wie folgt aussehen:
   
   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'whitenoise.middleware.WhiteNoiseMiddleware',
       ...
   ]
   ```
   
1. Fügen Sie am Ende von *azuresite/settings.py* die folgenden Zeilen hinzu:
   
   ```python
   STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
   STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
   ```
   
   Weitere Informationen zum Konfigurieren von WhiteNoise finden Sie in der [Dokumentation zu WhiteNoise](https://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> Im Abschnitt mit den Datenbankeinstellungen wird bereits die bewährte Sicherheitsmethode verwendet, bei der Umgebungsvariablen genutzt werden. Alle Empfehlungen zur Bereitstellung finden Sie unter [Django-Dokumentation: Checkliste zur Bereitstellung](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).

Committen Sie Ihre Änderungen in Ihren Fork des Repositorys *djangoapp*:

```bash
git commit -am "configure for App Service"
```

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-app-service-plan"></a>App Service-Plan erstellen

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Weiter oben in diesem Tutorial haben Sie die Umgebungsvariablen für die Verbindung mit der PostgreSQL-Datenbank definiert.

In Azure App Service legen Sie Umgebungsvariablen als *App-Einstellungen* fest, indem Sie den Befehl [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) verwenden.

Führen Sie in der Azure Cloud Shell den folgenden Befehl aus, um die Details der Datenbankverbindung als App-Einstellungen anzugeben. Ersetzen Sie *\<app-name>* , *\<resourcegroup-name>* und *\<postgresql-name>* durch Ihre eigenen Werte.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resourcegroup-name> --settings DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="manager@<postgresql-name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

Informationen dazu, wie in Ihrem Code auf diese App-Einstellungen zugegriffen wird, finden Sie unter [Zugreifen auf Umgebungsvariablen](how-to-configure-python.md#access-environment-variables).

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 60, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (51/51), done.
Writing objects: 100% (60/60), 15.37 KiB | 749.00 KiB/s, done.
Total 60 (delta 9), reused 0 (delta 0)
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '06f3f7c0cb'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
. 
. 
. 
remote: Done in 100 sec(s).
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: Deployment successful.
remote: Deployment Logs : 'https://<app-name>.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/06f3f7c0cb52ce3b4aff85c2b5099fbacb65ab94/log'
To https://<app-name>.scm.azurewebsites.net/<app-name>.git 
 * [new branch]      master -> master
```  

Der App Service-Bereitstellungsserver sieht *requirements.txt* im Stammverzeichnis des Repositorys und führt die Python-Paketverwaltung automatisch nach `git push` aus.

### <a name="browse-to-the-azure-app"></a>Navigieren zur Azure-App

Greifen Sie über die URL *http:\//\<app-name>.azurewebsites.net* auf die bereitgestellte App zu. Das Starten dauert einen Moment, da bei der ersten Anforderung der App der Container heruntergeladen und ausgeführt werden muss. Wenn bei der Seite ein Timeout auftritt oder eine Fehlermeldung angezeigt wird, warten Sie einige Minuten, und aktualisieren Sie die Seite.

Die weiter oben erstellten Fragen der Umfrage sollten angezeigt werden. 

App Service erkennt ein Django-Projekt in Ihrem Repository, indem in jedem Unterverzeichnis nach der Datei *wsgi.py* gesucht wird. Das Unterverzeichnis wird durch `manage.py startproject` standardmäßig erstellt. Wenn die Datei von App Service gefunden wird, wird die Django-Web-App geladen. Weitere Informationen dazu, wie App Service Python-Apps lädt, finden Sie unter [Konfigurieren Ihrer Python-App für Azure App Service unter Linux](how-to-configure-python.md).

Navigieren Sie zu *http:\//\<app-name>.azurewebsites.net/admin*, und melden Sie sich mit dem von Ihnen erstellten Administratorbenutzer an. Erstellen Sie bei Bedarf einige weitere Fragen für die Umfrage.

![Ausführen einer Python-Django-App in App Services in Azure](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

**Glückwunsch!** Sie führen eine Python-Web-App (Django) in Azure App Service für Linux aus.

## <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-your-app-in-the-azure-portal"></a>Überwachen Ihrer App im Azure-Portal

Suchen Sie im [Azure-Portal](https://portal.azure.com) nach der von Ihnen erstellten App, und wählen Sie sie aus.

![Navigieren zur Python-Django-App im Azure-Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben durchführen, z. B. Durchsuchen, Beenden, Neustarten und Löschen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![Verwalten der Python-Django-App auf der Seite „Übersicht“ im Azure-Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen:

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](../app-service-web-tutorial-custom-domain.md)

Oder sehen Sie sich weitere Ressourcen an:

> [!div class="nextstepaction"]
> [Konfigurieren einer Python-App](how-to-configure-python.md)
