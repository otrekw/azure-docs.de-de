---
title: 'Tutorial: ASP.NET Core mit SQL-Datenbank'
description: Sie erfahren, wie Sie eine .NET Core-App in Azure App Service mit einer Verbindung mit einer SQL-Datenbank ausführen.
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/23/2020
ms.custom: mvc, cli-validate, seodec18
ms.openlocfilehash: 4b5c78313eddd50441dbd47f556d2dbef03feefc
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310375"
---
# <a name="tutorial-build-an-aspnet-core-and-sql-database-app-in-azure-app-service"></a>Tutorial: Erstellen einer ASP.NET Core- und SQL-Datenbank-App in Azure App Service

> [!NOTE]
> In diesem Artikel wird eine App in App Service unter Windows bereitgestellt. Informationen zur Bereitstellung in App Service unter _Linux_ finden Sie unter [Erstellen einer .NET Core- und SQL-Datenbank-Web-App in Azure App Service für Linux](./containers/tutorial-dotnetcore-sqldb-app.md).
>

[App Service](overview.md) bietet einen hochgradig skalierbaren Webhostingdienst mit Self-Patching in Azure. In diesem Tutorial wird gezeigt, wie Sie eine .NET Core-App erstellen und mit einer SQL-Datenbank verbinden. Wenn Sie diese Schritte ausgeführt haben, verfügen Sie über eine .NET Core MVC-App, die in App Service ausgeführt wird.

![In App Service ausgeführte App](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Erstellen einer Datenbank in Azure SQL-Datenbank
> * Herstellen einer Verbindung einer .NET Core-App mit SQL-Datenbank
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Diagnoseprotokollen aus Azure
> * Verwalten der App im Azure-Portal

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* <a href="https://git-scm.com/" target="_blank">Installation von Git</a>
* <a href="https://dotnet.microsoft.com/download/dotnet-core/3.1" target="_blank">Installation des aktuellen .NET Core 3.1 SDK</a>

## <a name="create-local-net-core-app"></a>Erstellen einer lokalen .NET Core-App

In diesem Schritt richten Sie das lokale .NET Core-Projekt ein.

### <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Wechseln Sie im Terminalfenster mit `cd` in ein Arbeitsverzeichnis.

Führen Sie die folgenden Befehle aus, um das Beispielrepository zu klonen und in seinen Stamm zu wechseln.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Das Beispielprojekt enthält eine einfache CRUD-App (create-read-update-delete, erstellen-lesen-aktualisieren-löschen), die auf [Entity Framework Core](https://docs.microsoft.com/ef/core/) basiert.

### <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie die folgenden Befehle aus, um die erforderlichen Pakete zu installieren, Datenbankmigrationen auszuführen und die Anwendung zu starten.

```bash
dotnet tool install -g dotnet-ef
dotnet ef database update
dotnet run
```

Navigieren Sie in einem Browser zu `http://localhost:5000`. Wählen Sie den Link **Neu erstellen**, und erstellen Sie einige _Aufgaben_-Elemente.

![Erfolgreiche Verbindung mit SQL-Datenbank](./media/app-service-web-tutorial-dotnetcore-sqldb/local-app-in-browser.png)

Sie können .NET Core jederzeit beenden, indem Sie im Terminal `Ctrl+C` drücken.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-database-in-azure-sql-database"></a>Erstellen einer Datenbank in Azure SQL-Datenbank

In diesem Schritt erstellen Sie eine Datenbank in [Azure SQL-Datenbank](/azure/sql-database/). Wenn Ihre App in Azure bereitgestellt ist, nutzt sie diese Datenbank.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-server-in-azure-sql-database"></a>Erstellen eines Servers in Azure SQL-Datenbank

Erstellen Sie in der Cloud Shell einen [Server](../azure-sql/database/logical-servers.md) in Azure SQL-Datenbank mit dem Befehl [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create). Ein Server ist ein logisches Konstrukt, das eine Gruppe von Datenbanken enthält, die als Gruppe verwaltet werden.

Ersetzen Sie den Platzhalter *\<server-name>* durch einen *eindeutigen* Namen. Dieser Name wird als Teil des global eindeutigen SQL-Datenbank-Endpunkts `<server-name>.database.windows.net`verwendet. Gültige Zeichen sind `a`-`z`, `0`-`9` und `-`. Ersetzen Sie auch *\<db-username>db-username>* und *\<db-password>* durch einen Benutzernamen und ein Kennwort Ihrer Wahl.

```azurecli-interactive
az sql server create --name <server-name> --resource-group myResourceGroup --location "West Europe" --admin-user <db-username> --admin-password <db-password>
```

Nach dem Erstellen des Servers zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

<pre>
{
  "administratorLogin": "&lt;db-username&gt;",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "&lt;server-name&gt;.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/&lt;server-name&gt;",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "&lt;server-name&gt;",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
</pre>

### <a name="configure-a-server-firewall-rule"></a>Konfigurieren einer Serverfirewallregel

Erstellen Sie eine [Firewallregel auf Serverebene](../azure-sql/database/firewall-configure.md), indem Sie den Befehl [`az sql server firewall create`](/cli/azure/sql/server/firewall-rule?view=azure-cli-latest#az-sql-server-firewall-rule-create) verwenden. Wenn sowohl Start- als auch End-IP auf 0.0.0.0 festgelegt ist, wird die Firewall nur für andere Azure-Ressourcen geöffnet.

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server-name> --name AllowAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Sie können Ihre Firewallregel auch noch restriktiver gestalten und [nur die ausgehenden IP-Adressen verwenden, die Ihre App verwendet](overview-inbound-outbound-ips.md#find-outbound-ips).

Führen Sie den Befehl in Cloud Shell erneut aus, um den Zugriff vom lokalen Computer zuzulassen. Ersetzen Sie dabei *\<your-ip-address>* durch [Ihre lokale IPv4-IP-Adresse](https://www.whatsmyip.org/).

```azurecli-interactive
az sql server firewall-rule create --name AllowLocalClient --server <server_name> --resource-group myResourceGroup --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address>
```

### <a name="create-a-database-in-azure-sql-database"></a>Erstellen einer Datenbank in Azure SQL-Datenbank

Erstellen Sie mit dem Befehl [`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create) eine Datenbank mit der [Leistungsstufe „S0“](../azure-sql/database/service-tiers-dtu.md) auf dem Server.

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server-name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Erstellen der Verbindungszeichenfolge

Rufen Sie die Verbindungszeichenfolge mit dem Befehl [`az sql db show-connection-string`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-show-connection-string) ab.

```azurecli-interactive
az sql db show-connection-string --client ado.net --server <server-name> --name coreDB
```

Ersetzen Sie in der Befehlsausgabe *\<username>* und *\<password>* durch die zuvor verwendeten Anmeldeinformationen des Datenbankadministrators.

Dies ist die Verbindungszeichenfolge für Ihre .NET Core-App. Kopieren Sie sie für die spätere Verwendung.

### <a name="configure-app-to-connect-to-the-database-in-azure"></a>Konfigurieren der App zum Herstellen einer Verbindung mit der Datenbank in Azure

Öffnen Sie in Ihrem lokalen Repository „Startup.cs“, und suchen Sie den folgenden Code:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Ersetzen Sie ihn durch den folgenden Code:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
```

> [!IMPORTANT]
> Befolgen Sie bei Produktions-Apps, die aufskaliert werden müssen, die bewährten Methoden unter [Razor-Seiten mit EF Core in ASP.NET Core: Migrationen (4 von 8)](/aspnet/core/data/ef-rp/migrations#applying-migrations-in-production).

### <a name="run-database-migrations-to-the-database-in-azure"></a>Ausführen von Datenbankmigrationen zur Datenbank in Azure

Ihre App stellt derzeit eine Verbindung mit einer lokalen SQLite-Datenbank her. Sie haben eine Azure SQL-Datenbank konfiguriert. Erstellen Sie nun die erste Migration neu, um sie als Ziel festzulegen.

Führen Sie am Repositorystamm die folgenden Befehle aus. Ersetzen Sie *\<connection-string>* durch die zuvor erstellte Verbindungszeichenfolge.

```
# Delete old migrations
rm Migrations -r
# Recreate migrations
dotnet ef migrations add InitialCreate

# Set connection string to production database
# PowerShell
$env:ConnectionStrings:MyDbConnection="<connection-string>"
# CMD (no quotes)
set ConnectionStrings:MyDbConnection=<connection-string>
# Bash (no quotes)
export ConnectionStrings__MyDbConnection=<connection-string>

# Run migrations
dotnet ef database update
```

### <a name="run-app-with-new-configuration"></a>Ausführen der App mit der neuen Konfiguration

Die Datenbankmigration für die Produktionsdatenbank wird ausgeführt. Testen Sie daher nun Ihre App, indem Sie den folgenden Befehl ausführen:

```
dotnet run
```

Navigieren Sie in einem Browser zu `http://localhost:5000`. Wählen Sie den Link **Neu erstellen**, und erstellen Sie einige _Aufgaben_-Elemente. Ihre App liest und schreibt nun Daten in die Produktionsdatenbank.

Committen Sie Ihre lokalen Änderungen, und committen Sie sie anschließend in Ihr Git-Repository.

```bash
git add .
git commit -m "connect to SQLDB in Azure"
```

Sie können Ihren Code jetzt bereitstellen.

## <a name="deploy-app-to-azure"></a>Bereitstellen von Apps in Azure

In diesem Schritt stellen Sie die mit der SQL-Datenbank verbundene .NET Core-Anwendung für App Service bereit.

### <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Erstellen einer Web-App

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app-dotnetcore-win-no-h.md)]

### <a name="configure-connection-string"></a>Konfigurieren der Verbindungszeichenfolge

Verwenden Sie zum Festlegen von Verbindungszeichenfolgen für Ihre Azure-App den Befehl [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell. Ersetzen Sie im folgenden Befehl sowohl *\<app-name>* als auch den Parameter *\<connection-string>* durch die Verbindungszeichenfolge, die Sie zuvor erstellt haben.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app-name> --settings MyDbConnection="<connection-string>" --connection-string-type SQLAzure
```

In ASP.NET Core können Sie diese Verbindungszeichenfolge (`MyDbConnection`) genau wie eine Verbindungszeichenfolge in *appsettings.json* mit dem Standardmuster verwenden. In diesem Fall wird `MyDbConnection` ebenfalls in der Datei *appsettings.json* definiert. Bei der Ausführung in App Service hat die in App Service definierte Verbindungszeichenfolge Vorrang vor der in der Datei *appsettings.json* definierten Verbindungszeichenfolge. Im Rahmen der lokalen Entwicklung verwendet der Code den Wert *appsettings.json*. Bei der Bereitstellung verwendet der gleiche Code dann allerdings den App Service-Wert.

Informationen dazu, wie im Code auf die Verbindungszeichenfolge verwiesen wird, finden Sie unter [Konfigurieren der App zum Herstellen einer Verbindung mit der Produktionsdatenbank](#configure-app-to-connect-to-the-database-in-azure).

### <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

[!INCLUDE [push-to-azure-no-h](../../includes/app-service-web-git-push-to-azure-no-h.md)]

<pre>
Enumerating objects: 268, done.
Counting objects: 100% (268/268), done.
Compressing objects: 100% (171/171), done.
Writing objects: 100% (268/268), 1.18 MiB | 1.55 MiB/s, done.
Total 268 (delta 95), reused 251 (delta 87), pack-reused 0
remote: Resolving deltas: 100% (95/95), done.
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '64821c3558'.
remote: Generating deployment script.
remote: Project file path: .\DotNetCoreSqlDb.csproj
remote: Generating deployment script for ASP.NET MSBuild16 App
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment with MSBuild16.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: App container will begin restart within 10 seconds.
To https://&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git
 * [new branch]      master -> master
</pre>

### <a name="browse-to-the-azure-app"></a>Navigieren zur Azure-App

Navigieren Sie in Ihrem Webbrowser zur bereitgestellten App.

```bash
http://<app-name>.azurewebsites.net
```

Fügen Sie einige Aufgaben hinzu.

![In App Service ausgeführte App](./media/app-service-web-tutorial-dotnetcore-sqldb/azure-app-in-browser.png)

**Glückwunsch!** Sie führen eine datengesteuerte .NET Core-App in App Service aus.

## <a name="update-locally-and-redeploy"></a>Lokales Aktualisieren und erneutes Bereitstellen

In diesem Schritt nehmen Sie eine Änderung am Datenbankschema vor und veröffentlichen sie in Azure.

### <a name="update-your-data-model"></a>Aktualisieren Sie des Datenmodells

Öffnen Sie _Models\Todo.cs_ im Code-Editor. Fügen Sie der `ToDo`-Klasse die folgende Eigenschaft hinzu:

```csharp
public bool Done { get; set; }
```

### <a name="rerun-database-migrations"></a>Erneutes Ausführen der Datenbankmigration

Führen Sie einige Befehle aus, um Aktualisierungen an der Produktionsdatenbank vorzunehmen.

```bash
dotnet ef migrations add AddProperty
dotnet ef database update
```

### <a name="use-the-new-property"></a>Verwenden der neuen Eigenschaft

Nehmen Sie einige Änderungen an Ihrem Code vor, um die `Done`-Eigenschaft zu verwenden. Der Einfachheit halber ändern Sie in diesem Lernprogramm nur Ansichten `Index` und `Create`, um die Eigenschaft in Aktion zu sehen.

Öffnen Sie _Controllers\TodosController.cs_.

Suchen Sie die `Create([Bind("ID,Description,CreatedDate")] Todo todo)`-Methode, und fügen Sie `Done` zur Liste der Eigenschaften im `Bind`-Attribut hinzu. Anschließend sieht Ihre `Create()`-Methodensignatur wie im folgenden Code aus:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Öffnen Sie _Views\Todos\Create.cshtml_.

Im Razor-Code sollten Sie ein `<div class="form-group">`-Element für `Description` sowie ein weiteres `<div class="form-group">`-Element für `CreatedDate` sehen. Fügen Sie direkt nach diesen beiden Elementen ein weiteres `<div class="form-group">`-Element für `Done` hinzu:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Öffnen Sie _Views\Todos\Index.cshtml_.

Suchen Sie nach dem leeren `<th></th>`-Element. Fügen Sie direkt oberhalb dieses Elements den folgenden Razor-Code hinzu:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Suchen Sie nach dem `<td>`-Element, das die `asp-action`-Taghilfsprogramme enthält. Fügen Sie direkt oberhalb dieses Elements den folgenden Razor-Code hinzu:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

Mehr ist nicht erforderlich, um die Änderungen in den Ansichten `Index` und `Create` anzuzeigen.

### <a name="test-your-changes-locally"></a>Lokales Testen der Änderungen

Führen Sie die App lokal aus.

```bash
dotnet run
```

Navigieren Sie in Ihrem Browser zu `http://localhost:5000/`. Sie können jetzt eine Aufgabe hinzufügen und die Option **Fertig** aktivieren. Die Aufgabe sollte dann auf der Startseite als erledigt angezeigt werden. Beachten Sie, dass in der Ansicht `Edit` das Feld `Done` nicht angezeigt wird, da Sie die Ansicht `Edit` nicht geändert haben.

### <a name="publish-changes-to-azure"></a>Veröffentlichen von Änderungen in Azure

```bash
git add .
git commit -m "added done field"
git push azure master
```

Navigieren Sie nach Abschluss des `git push`-Vorgangs zu Ihrer App Service-App, versuchen Sie, eine Aufgabe hinzuzufügen, und aktivieren Sie **Fertig**.

![Azure-App nach Code First-Migration](./media/app-service-web-tutorial-dotnetcore-sqldb/this-one-is-done.png)

Ihre gesamten vorhandenen Aufgaben werden weiterhin angezeigt. Wenn Sie die ASP.NET Core-App erneut veröffentlichen, gehen vorhandene Daten in SQL-Datenbank nicht verloren. Außerdem wird durch Entity Framework Core-Migrationen nur das Datenschema geändert, die vorhandenen Daten bleiben unverändert.

## <a name="stream-diagnostic-logs"></a>Streamen von Diagnoseprotokollen

Wenn die ASP.NET Core-App in Azure App Service ausgeführt wird, können Sie die Konsolenprotokolle an Cloud Shell umleiten. Auf diese Weise erhalten Sie die gleichen Diagnosemeldungen, die Ihnen beim Debuggen von Anwendungsfehlern helfen.

Im Beispielprojekt wird die Anleitung unter [Protokollierung in ASP.NET Core.](https://docs.microsoft.com/aspnet/core/fundamentals/logging#azure-app-service-provider) mit zwei Konfigurationsänderungen bereits befolgt:

* Enthält einen Verweis auf `Microsoft.Extensions.Logging.AzureAppServices` in *DotNetCoreSqlDb.csproj*.
* Ruft `loggerFactory.AddAzureWebAppDiagnostics()` in *Program.cs* auf.

Zum Festlegen der [Protokollebene](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-level) für ASP.NET Core in App Service von der Standardebene `Error` auf `Information` verwenden Sie den Befehl [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) in Cloud Shell.

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --application-logging true --level information
```

> [!NOTE]
> Die Protokollebene des Projekts ist in *appsettings.json* bereits auf `Information` festgelegt.

Verwenden Sie zum Starten des Streamings von Protokolldateien den Befehl [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) in Cloud Shell.

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Nachdem das Protokollstreaming gestartet wurde, aktualisieren Sie die Azure-App im Browser, um Webdatenverkehr zu generieren. Sie können jetzt Konsolenprotokolle sehen, die auf das Terminal umgeleitet werden. Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

Um das Protokollstreaming jederzeit zu beenden, geben Sie `Ctrl`+`C` ein.

Weitere Informationen zum Anpassen der ASP.NET Core-Protokolle finden Sie unter [Protokollierung in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="manage-your-azure-app"></a>Verwalten der Azure-App

Um die von Ihnen erstellte App anzuzeigen, suchen Sie im [Azure-Portal](https://portal.azure.com) nach **App Services**, und wählen Sie diese Option aus.

![Auswählen von „App Services“ im Azure-Portal](./media/app-service-web-tutorial-dotnetcore-sqldb/app-services.png)

Wählen Sie auf der Seite **App Services** den Namen Ihrer Azure-App aus.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-dotnetcore-sqldb/access-portal.png)

Standardmäßig wird im Portal die Seite **Übersicht** Ihrer App angezeigt. Diese Seite bietet einen Überblick über den Status Ihrer App. Hier können Sie auch einfache Verwaltungsaufgaben wie Durchsuchen, Beenden, Neustarten und Löschen durchführen. Die Registerkarten links auf der Seite zeigen die verschiedenen Konfigurationsseiten, die Sie öffnen können.

![App Service-Seite im Azure-Portal](./media/app-service-web-tutorial-dotnetcore-sqldb/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
>
> * Erstellen einer Datenbank in Azure SQL-Datenbank
> * Herstellen einer Verbindung einer .NET Core-App mit SQL-Datenbank
> * Bereitstellen der Anwendung in Azure
> * Aktualisieren des Datenmodells und erneutes Bereitstellen der App
> * Streamen von Protokollen von Azure auf Ihr Terminal
> * Verwalten der App im Azure-Portal

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihrer App einen benutzerdefinierten DNS-Namen zuordnen.

> [!div class="nextstepaction"]
> [Tutorial: Zuordnen eines benutzerdefinierten DNS-Namens zu Ihrer App](app-service-web-tutorial-custom-domain.md)
