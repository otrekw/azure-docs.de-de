---
title: Konfigurieren von CI/CD mit GitHub-Aktionen
description: Erfahren Sie, wie Sie Azure App Service Codes über eine CI/CD-Pipeline mit GitHub-Aktionen bereitstellen. Passen Sie die Buildaufgaben an, und führen Sie komplexe Bereitstellungen aus.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.custom: devx-track-python
ms.openlocfilehash: 713f4228bc2ba968fc96668d4d5c568f33b7e786
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88080282"
---
# <a name="deploy-to-app-service-using-github-actions"></a>Bereitstellen in App Service mithilfe von GitHub Actions

[GitHub Actions](https://help.github.com/en/articles/about-github-actions) bietet Ihnen die Flexibilität, einen automatisierten Workflow für den Softwareentwicklungs-Lebenszyklus zu erstellen. Mit den Azure App Service-Aktionen für GitHub können Sie Ihren Workflow automatisieren, um mithilfe von GitHub Actions in [Azure App Service](overview.md) bereitzustellen.

> [!IMPORTANT]
> GitHub Actions liegt zurzeit in einer Betaversion vor. Sie müssen sich zunächst mit Ihrem GitHub-Konto [registrieren, um die Vorschau nutzen zu können](https://github.com/features/actions).
> 

Ein Workflow wird durch eine YAML-Datei im Pfad `/.github/workflows/` in Ihrem Repository definiert. Diese Definition enthält die verschiedenen Schritte und Parameter, die den Workflow bilden.

Bei einem Azure App Service-Workflow umfasst die Datei drei Abschnitte:

|`Section`  |Aufgaben  |
|---------|---------|
|**Authentifizierung** | 1. Definieren eines Dienstprinzipals. <br /> 2. Erstellen eines GitHub-Geheimnisses. |
|**Build** | 1. Einrichten der Umgebung. <br /> 2. Erstellen der Web-App. |
|**Bereitstellen** | 1. Bereitstellen der Web-App. |

## <a name="generate-deployment-credentials"></a>Generieren von Anmeldeinformationen für die Bereitstellung

# <a name="user-level-credentials"></a>[Anmeldeinformationen auf Benutzerebene](#tab/userlevel)

Sie können mit dem Befehl [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) in der [Azure CLI](https://docs.microsoft.com/cli/azure/) einen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) erstellen. Sie können diesen Befehl mit [Azure Cloud Shell](https://shell.azure.com/) im Azure-Portal oder durch Auswählen der Schaltfläche **Ausprobieren** ausführen.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> \
                            --sdk-auth
```

Ersetzen Sie im obigen Beispiel die Platzhalter durch Ihre Abonnement-ID, den Ressourcengruppennamen und den App-Namen. Die Ausgabe ist ein JSON-Objekt mit den Anmeldeinformationen für die Rollenzuweisung, die ähnlich wie unten Zugriff auf Ihre App Service-App gewähren. Kopieren Sie dieses JSON-Objekt zur späteren Verwendung.

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> Es ist immer empfehlenswert, den minimalen Zugriff zu gewähren. Der Bereich im vorherigen Beispiel ist auf die spezifische App Service-App und nicht auf die gesamte Ressourcengruppe beschränkt.

# <a name="app-level-credentials"></a>[Anmeldeinformationen auf App-Ebene](#tab/applevel)

Sie können Anmeldeinformationen auf App-Ebene verwenden, indem Sie das Veröffentlichungsprofil für Ihre App verwenden. Wechseln Sie zur Verwaltungsseite Ihrer App im Portal. Klicken Sie auf der Seite **Übersicht** auf die Option **Veröffentlichungsprofil abrufen**.

Sie benötigen den Inhalt der Datei später noch.

---

## <a name="configure-the-github-secret"></a>Konfigurieren des GitHub-Geheimnisses

# <a name="user-level-credentials"></a>[Anmeldeinformationen auf Benutzerebene](#tab/userlevel)

Navigieren Sie in [GitHub](https://github.com/) zu Ihrem Repository, und wählen Sie **Einstellungen > Geheimnisse > Neues Geheimnis hinzufügen** aus.

Um [Anmeldeinformationen auf Benutzerebene](#generate-deployment-credentials) zu verwenden, fügen Sie die gesamte JSON-Ausgabe aus dem Azure CLI-Befehl in das Wertfeld des Geheimnisses ein. Geben Sie dem Geheimnis einen Namen wie `AZURE_CREDENTIALS`.

Wenn Sie die Workflowdatei später konfigurieren, verwenden Sie das Geheimnis für die Eingabe `creds` der Azure-Anmeldeaktion. Beispiel:

```yaml
- uses: azure/login@v1
  with:
    creds: ${{ secrets.AZURE_CREDENTIALS }}
```

# <a name="app-level-credentials"></a>[Anmeldeinformationen auf App-Ebene](#tab/applevel)

Navigieren Sie in [GitHub](https://github.com/) zu Ihrem Repository, und wählen Sie **Einstellungen > Geheimnisse > Neues Geheimnis hinzufügen** aus.

Um [Anmeldeinformationen auf App-Ebene](#generate-deployment-credentials) zu verwenden, fügen Sie den Inhalt der heruntergeladenen Veröffentlichungsprofildatei in das Wertfeld des Geheimnisses ein. Geben Sie dem Geheimnis einen Namen wie `azureWebAppPublishProfile`.

Wenn Sie die Workflowdatei später konfigurieren, verwenden Sie das Geheimnis für das Eingabe `publish-profile` der Azure-Web-App-Bereitstellungsaktion. Beispiel:
    
```yaml
- uses: azure/webapps-deploy@v2
  with:
    publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

---

## <a name="set-up-the-environment"></a>Einrichten der Umgebung

Das Einrichten der Umgebung kann mithilfe einer der Setupaktionen erfolgen.

|**Sprache**  |**Setupaktion**  |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**     | `actions/setup-java` |
|**JavaScript** | `actions/setup-node` |
|**Python**     | `actions/setup-python` |

Die folgenden Beispiele zeigen den Teil des Workflows, der die Umgebung für die verschiedenen unterstützten Sprachen einrichtet:

**JavaScript**

```yaml
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```
**Python**

```yaml
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-web-app"></a>Erstellen der Web-App.

Dies hängt von der Sprache und den von Azure App Service unterstützten Sprachen ab. Dieser Abschnitt sollte die Standardbuildschritte der einzelnen Sprachen beinhalten.

Die folgenden Beispiele zeigen den Teil des Workflows, der die Web-App in den verschiedenen unterstützten Sprachen erstellt.

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

**Python**

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

**.NET**

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - uses: actions/checkout@v1
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```
## <a name="deploy-to-app-service"></a>Bereitstellen für App Service

Verwenden Sie die Aktion `azure/webapps-deploy@v2`, um Ihren Code in einer App Service-App bereitzustellen. Diese Aktion umfasst vier Parameter:

| **Parameter**  | **Erklärung**  |
|---------|---------|
| **app-name** | (Erforderlich): Name der App Service-App. | 
| **publish-profile** | (Optional) Veröffentlichungsprofil-Dateiinhalte mit Web Deploy-Geheimnissen. |
| **package** | (Optional) Pfad zum Paket oder Ordner. *.zip, *.war, *.jar oder ein Ordner für die Bereitstellung |
| **slot-name** | (Optional): Geben Sie einen vorhandene Slot ein, bei dem es sich nicht um den Produktionsslot handelt. |

# <a name="user-level-credentials"></a>[Anmeldeinformationen auf Benutzerebene](#tab/userlevel)

Nachstehend wird ein Beispielworkflow zum Erstellen und Bereitstellen einer Node.js-App in Azure mithilfe eines Azure-Dienstprinzipals angezeigt. Beachten Sie, wie die `creds`-Eingabe auf das `AZURE_CREDENTIALS`-Geheimnis verweist, das Sie zuvor erstellt haben.

```yaml
on: [push]

name: Node.js

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
   
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
               
    # deploy web app using Azure credentials
    - uses: azure/webapps-deploy@v2
      with:
        app-name: 'node-rn'

    # Azure logout 
    - name: logout
      run: |
        az logout
```

# <a name="app-level-credentials"></a>[Anmeldeinformationen auf App-Ebene](#tab/applevel)

Nachstehend wird ein Beispielworkflow zum Erstellen und Bereitstellen einer Node.js-App in Azure mithilfe des Veröffentlichungsprofils der App angezeigt. Beachten Sie, wie die `publish-profile`-Eingabe auf das `azureWebAppPublishProfile`-Geheimnis verweist, das Sie zuvor erstellt haben.

```yaml
# File: .github/workflows/workflow.yml

on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
       
    - name: 'Run Azure webapp deploy action using publish profile credentials'
          uses: azure/webapps-deploy@v2
          with: 
            app-name: node-rn
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

---

## <a name="next-steps"></a>Nächste Schritte

Die verfügbaren Aktionen sind auf verschiedene GitHub-Repositorys verteilt, jeweils mit Dokumentation und Beispielen, um Sie bei der Verwendung von GitHub für CI/CD und der Bereitstellung Ihrer Apps in Azure zu unterstützen.

- [Aktionsworkflows für das Bereitstellen in Azure](https://github.com/Azure/actions-workflow-samples)

- [Azure-Anmeldung](https://github.com/Azure/login)

- [Azure-Web-App](https://github.com/Azure/webapps-deploy)

- [Azure-Web-App für Container](https://github.com/Azure/webapps-container-deploy)

- [Docker-Anmeldung/-Abmeldung](https://github.com/Azure/docker-login)

- [Ereignisse zum Auslösen von Workflows](https://help.github.com/en/articles/events-that-trigger-workflows)

- [Kubernetes-Bereitstellung](https://github.com/Azure/k8s-deploy)

- [Workflows zum Einstieg](https://github.com/actions/starter-workflows)
