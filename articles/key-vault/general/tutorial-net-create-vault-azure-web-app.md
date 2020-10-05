---
title: 'Tutorial: Verwenden von Azure Key Vault mit einer Azure-Web-App in .NET | Microsoft-Dokumentation'
description: In diesem Tutorial konfigurieren Sie eine Azure-Web-App in einer ASP.NET Core-Anwendung zum Lesen eines Geheimnisses aus Ihrem Schlüsseltresor.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 05/06/2020
ms.author: mbaldwin
ms.custom: devx-track-csharp
ms.openlocfilehash: e537bb74655bce5c8438e22fb9b990b72eab73d7
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336682"
---
# <a name="tutorial-use-a-managed-identity-to-connect-key-vault-to-an-azure-web-app-with-net"></a>Tutorial: Verwenden einer verwalteten Identität, um eine Verbindung zwischen Key Vault und einer Azure-Web-App mit .NET herzustellen

Azure Key Vault ermöglicht das sichere Speichern von Anmeldeinformationen und anderen Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Dieses Problem können Sie anhand der Informationen unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../../active-directory/managed-identities-azure-resources/overview.md) lösen, indem Sie Azure-Diensten eine automatisch verwaltete Identität in Azure AD bereitstellen. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung, einschließlich Key Vault, unterstützt. Hierfür müssen im Code keine Anmeldeinformationen angezeigt werden.

In diesem Tutorial wird eine verwaltete Identität verwendet, um eine Azure-Web-App mit einer Azure Key Vault-Instanz zu authentifizieren. In den Schritten werden zwar die [Azure Key Vault v4-Clientbibliothek für .NET](/dotnet/api/overview/azure/key-vault?view=azure-dotnet) und die [Azure-Befehlszeilenschnittstelle](/cli/azure/get-started-with-azure-cli) verwendet, die gleichen grundlegenden Prinzipien gelten jedoch auch bei Verwendung der Entwicklungssprache Ihrer Wahl sowie bei Verwendung von Azure PowerShell und/oder des Azure-Portals.

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch:

* Azure-Abonnement ([kostenloses Abonnement erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F))
* [.NET Core 3.1 SDK oder höher](https://dotnet.microsoft.com/download/dotnet-core/3.1)
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) oder [Azure PowerShell](/powershell/azure/)

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Erstellen Sie mithilfe des Befehls [az group create](/cli/azure/group?view=azure-cli-latest#az-group-create) eine Ressourcengruppe für Ihren Schlüsseltresor und Ihre Web-App:

```azurecli-interactive
az group create --name "myResourceGroup" -l "EastUS"
```

## <a name="set-up-your-key-vault"></a>Einrichten Ihres Schlüsseltresors

In diesem Abschnitt wird ein Schlüsseltresor erstellt und ein Geheimnis darin platziert. Dieses Geheimnis wird dann später in diesem Tutorial verwendet.

Verwenden Sie den Befehl [az keyvault create](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create), um einen Schlüsseltresor zu erstellen:

> [!Important]
> Jeder Schlüsseltresor muss einen eindeutigen Namen haben. Ersetzen Sie in den folgenden Beispielen „<your-keyvault-name>“ durch den Namen Ihres Schlüsseltresors.

```azurecli-interactive
az keyvault create --name "<your-keyvault-name>" -g "myResourceGroup"
```

Notieren Sie sich das zurückgegebene `vaultUri`-Element. Es hat das Format „https://&lt;your-keyvault-name&gt;.vault.azure.net/“. Er wird im Schritt [Aktualisieren des Codes](#update-the-code) benötigt.

[!INCLUDE [Create a secret](../../../includes/key-vault-create-secret.md)]

## <a name="create-a-net-web-app"></a>Erstellen einer .NET-Web-App

### <a name="create-a-local-app"></a>Erstellen einer lokalen App

Erstellen Sie in einem Terminalfenster auf Ihrem Computer ein Verzeichnis mit dem Namen `akvwebapp`, und wechseln Sie dorthin.

```bash
mkdir akvwebapp
cd akvwebapp
```

Erstellen Sie nun mithilfe des Befehls [dotnet new web](/dotnet/core/tools/dotnet-new) eine neue .NET Core-App:

```bash
dotnet new web
```

Führen Sie die Anwendung lokal aus, damit Sie sehen, wie sie beim Bereitstellen in Azure aussehen sollte. 

```bash
dotnet run
```

Öffnen Sie einen Webbrowser, und navigieren Sie zu der App auf `http://localhost:5000`.

Auf der Seite wird die Nachricht **Hello World** aus der Beispiel-App angezeigt.

### <a name="initialize-the-git-repository"></a>Initialisieren des Git-Repositorys

Drücken Sie in Ihrem Terminalfenster **STRG+C**, um den Webserver zu beenden.  Initialisieren Sie ein Git-Repository für das .NET Core-Projekt.

```bash
git init
git add .
git commit -m "first commit"
```

### <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers

Für die Bereitstellung in einer Azure-Web-App über FTP oder ein lokales Git kann ein *Bereitstellungsbenutzer* verwendet werden. Nach der Konfiguration des Bereitstellungsbenutzers können Sie ihn für alle Azure-Bereitstellungen verwenden. Der Benutzername und das Kennwort für die Bereitstellung auf Kontoebene unterscheiden sich von den Anmeldeinformationen für Ihr Azure-Abonnement. 

Führen Sie zum Konfigurieren des Bereitstellungsbenutzers den Befehl [az webapp deployment user set](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) aus. Wählen Sie einen Benutzernamen und ein Kennwort, der bzw. das den folgenden Richtlinien entspricht: 

- Der Benutzername muss in Azure eindeutig sein und darf bei lokalen Git-Pushes nicht das Symbol „@“ enthalten. 
- Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen und Symbole. 

```azurecli-interactive
az webapp deployment user set --user-name "<username>" --password "<password>"
```

In der JSON-Ausgabe wird das Kennwort als `null` angezeigt. Wenn Sie den Fehler `'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler `'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden. 

Notieren Sie Ihren Benutzernamen und Ihr Kennwort für die Bereitstellung Ihrer Web-Apps.

### <a name="create-an-app-service-plan"></a>Erstellen eines App Service-Plans

Erstellen Sie mithilfe des Azure CLI-Befehls [az appservice plan create](/cli/azure/appservice/plan?view=azure-cli-latest) einen App Service-Plan. Im folgenden Beispiel wird ein App Service-Plan namens `myAppServicePlan` im Tarif **Free** erstellt:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

<pre>
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
</pre>


### <a name="create-a-remote-web-app"></a>Erstellen einer Remote-Web-App

Erstellen Sie eine [Azure-Web-App](../../app-service/overview.md#app-service-on-linux) im App Service-Plan `myAppServicePlan`. 

> [!Important]
> Eine Azure-Web-App muss genau wie eine Key Vault-Instanz einen eindeutigen Namen haben. Ersetzen Sie in den folgenden Beispielen \<your-webapp-name\> durch den Namen Ihrer Web-App.


```azurecli-interactive
az webapp create --resource-group "myResourceGroup" --plan "myAppServicePlan" --name "<your-webapp-name>" --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure CLI eine Ausgabe wie im folgenden Beispiel an:

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.azurewebsites.net/&lt;ayour-webapp-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "clientCertExclusionPaths": null,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;your-webapp-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;your-webapp-name&gt;.scm.azurewebsites.net/&lt;your-webapp-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>


Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<your-webapp-name>.scm.azurewebsites.net/<your-webapp-name>.git` angezeigt. Speichern Sie diese URL für später.

Wechseln Sie zu Ihrer neu erstellten App. Ersetzen Sie _&lt;your-webapp-name>_ durch Ihren App-Namen.

```bash
https://<your-webapp-name>.azurewebsites.net
```

Die Standardwebseite für eine neu erstellte Azure-Web-App wird angezeigt.

### <a name="deploy-your-local-app"></a>Bereitstellen Ihrer lokalen App

Fügen Sie Ihrem lokalen Git-Repository über das lokale Terminalfenster einen Azure-Remotespeicherort hinzu. Ersetzen Sie dabei *\<deploymentLocalGitUrl-from-create-step>* durch die URL des Git-Remotespeicherorts, die Sie im Schritt [Erstellen einer Remote-Web-App](#create-a-remote-web-app) gespeichert haben.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Führen Sie einen Pushvorgang zum Azure-Remotespeicherort durch, um Ihre App mit dem folgenden Befehl bereitzustellen. Wenn Sie von Git Credential Manager zur Eingabe von Anmeldeinformationen aufgefordert werden, verwenden Sie die Anmeldeinformationen, die Sie im Schritt [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben.

```bash
git push azure master
```

Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen. Während der Ausführung werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:
<pre>
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 285 bytes | 95.00 KiB/s, done.
Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'd6b54472f7'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
remote:
remote: Oryx Version      : 0.2.20200114.13, Commit: 204922f30f8e8d41f5241b8c218425ef89106d1d, ReleaseTagName: 20200114.13
remote: Build Operation ID: |imoMY2y77/s=.40ca2a87_
remote: Repository Commit : d6b54472f7e8e9fd885ffafaa64522e74cf370e1
.
.
.
remote: Deployment successful.
remote: Deployment Logs : 'https://&lt;your-webapp-name&gt;.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/d6b54472f7e8e9fd885ffafaa64522e74cf370e1/log'
To https://&lt;your-webapp-name&gt;.scm.azurewebsites.net:443/&lt;your-webapp-name&gt;.git
   d87e6ca..d6b5447  master -> master
</pre>

Navigieren Sie in Ihrem Webbrowser zu der bereitgestellten Anwendung, oder aktualisieren Sie die Ansicht.

```bash
http://<your-webapp-name>.azurewebsites.net
```

Daraufhin wird die Nachricht „Hello World!“ angezeigt, die Sie zuvor beim Besuch von `http://localhost:5000`gesehen haben.

## <a name="create-and-assign-a-managed-identity"></a>Erstellen und Zuweisen einer verwalteten Identität

Führen Sie über die Azure-Befehlszeilenschnittstelle den Befehl [az webapp-identity assign](/cli/azure/webapp/identity?view=azure-cli-latest#az-webapp-identity-assign) aus, um die Identität für diese Anwendung zu erstellen:

```azurecli-interactive
az webapp identity assign --name "<your-webapp-name>" --resource-group "myResourceGroup"
```

Daraufhin wird der folgende JSON-Codeausschnitt zurückgegeben:

```json
{
  "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "SystemAssigned"
}
```

Übergeben Sie die Prinzipal-ID (principalID) an den Azure CLI-Befehl [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy), um Ihrer Web-App das Ausführen von Abruf- und Auflistungsvorgängen (**get** und **list**) für Ihren Schlüsseltresor zu ermöglichen:

```azurecli-interactive
az keyvault set-policy --name "<your-keyvault-name>" --object-id "<principalId>" --secret-permissions get list
```


## <a name="modify-the-app-to-access-your-key-vault"></a>Anpassen der App für den Zugriff auf Ihren Schlüsseltresor

### <a name="install-the-packages"></a>Installieren der Pakete

Installieren Sie über das Terminalfenster die Pakete der Azure Key Vault-Clientbibliothek für .NET:

```console
dotnet add package Azure.Identity
dotnet add package Azure.Security.KeyVault.Secrets
```

### <a name="update-the-code"></a>Aktualisieren des Codes

Navigieren Sie im Projekt „akvwebapp“ zur Datei „Startup.cs“, und öffnen Sie sie. 

Fügen Sie dem Header die beiden folgenden Zeilen hinzu:

```csharp
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
using Azure.Core;
```

Fügen Sie diese Zeilen vor dem Aufruf von `app.UseEndpoints` hinzu, und aktualisieren Sie den URI, sodass er dem Tresor-URI (`vaultUri`) Ihres Schlüsseltresors entspricht. Im folgenden Code wird ['DefaultAzureCredential()'](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet) für die Authentifizierung beim Schlüsseltresor verwendet. Dabei wird das Token der verwalteten Anwendungsidentität zur Authentifizierung herangezogen. Darüber hinaus wird das exponentielle Backoff für Wiederholungen verwendet, falls der Schlüsseltresor gedrosselt wird.

```csharp
SecretClientOptions options = new SecretClientOptions()
    {
        Retry =
        {
            Delay= TimeSpan.FromSeconds(2),
            MaxDelay = TimeSpan.FromSeconds(16),
            MaxRetries = 5,
            Mode = RetryMode.Exponential
         }
    };
var client = new SecretClient(new Uri("https://<your-unique-key-vault-name>.vault.azure.net/"), new DefaultAzureCredential(),options);

KeyVaultSecret secret = client.GetSecret("mySecret");

string secretValue = secret.Value;
```

Aktualisieren Sie die Zeile `await context.Response.WriteAsync("Hello World!");` wie folgt:

```csharp
await context.Response.WriteAsync(secretValue);
```

Speichern Sie Ihre Änderungen, bevor Sie mit dem nächsten Schritt fortfahren.

### <a name="redeploy-your-web-app"></a>Erneutes Bereitstellen Ihrer Web-App

Der aktualisierte Code kann mithilfe der folgenden Git-Befehle erneut in Azure bereitgestellt werden:

```bash
git add .
git commit -m "Updated web app to access my key vault"
git push azure master
```

## <a name="visit-your-completed-web-app"></a>Besuchen Ihrer fertigen Web-App

```bash
http://<your-webapp-name>.azurewebsites.net
```

Anstelle der zuvor angezeigten Nachricht **Hello World** sollte nun der Wert Ihres Geheimnisses angezeigt werden: **Erfolg!**

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu verwalteten Identitäten für Azure-Ressourcen finden Sie [hier](../../active-directory/managed-identities-azure-resources/overview.md).
- Weitere Informationen zu verwalteten Identitäten für App Service finden Sie [hier](../../app-service/overview-managed-identity.md?tabs=dotnet).
- Die API-Referenz für die Azure Key Vault-Clientbibliothek für .NET finden Sie [hier](/dotnet/api/overview/azure/key-vault?view=azure-dotnet).
- Den Quellcode für die Azure Key Vault-Clientbibliothek für .NET finden Sie [hier](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault).
- Das NuGet-Paket der Azure Key Vault-Clientbibliothek für .NET (v4) finden Sie [hier](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/).


