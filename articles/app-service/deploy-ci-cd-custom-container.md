---
title: CI/CD für benutzerdefinierte Container
description: Hier erfahren Sie, wie Sie Continuous Deployment für einen benutzerdefinierten Windows- oder Linux-Container in Azure App Service einrichten.
keywords: Azure App Service, Linux, Docker, ACR, OSS
author: msangapu-msft
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.topic: article
ms.date: 03/12/2021
ms.author: msangapu
ms.custom: seodec18, devx-track-azurecli
zone_pivot_groups: app-service-containers-windows-linux
ms.openlocfilehash: 6519f3fe7335ed41f4d5ef67771aaa738a33e4a8
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107782601"
---
# <a name="continuous-deployment-with-custom-containers-in-azure-app-service"></a>Continuous Deployment mit benutzerdefinierten Containern in Azure App Service

In diesem Tutorial konfigurieren Sie Continuous Deployment für ein benutzerdefiniertes Containerimage aus verwalteten [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)-Repositorys oder aus [Docker Hub](https://hub.docker.com).

## <a name="1-go-to-deployment-center"></a>1. Wechseln zum Bereitstellungscenter

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Verwaltungsseite Ihrer App Service-App.

Klicken Sie im Menü auf der linken Seite auf **Deployment Center** > **Einstellungen**. 

::: zone pivot="container-linux"
## <a name="2-choose-deployment-source"></a>2. Bereitstellungsquelle auswählen

Die **Auswahl** der Bereitstellungsquelle hängt von Ihrem Szenario ab:
- Die **Containerregistrierung** richtet CI/CD zwischen Ihrer Containerregistrierung und App Service ein.
- **GitHub Actions** ist die richtige Wahl für Sie, wenn Sie den Quellcode für das Containerimage in GitHub hosten. Die neue Bereitstellungsaktion wird durch neue Commits in Ihrem GitHub-Repository ausgelöst und kann `docker build` sowie `docker push` direkt in Ihrer Containerregistrierung ausführen. Anschließend aktualisiert sie die App Service-App, um das neue Image auszuführen. Weitere Informationen finden Sie unter [Funktionsweise von CI/CD mit GitHub Actions](#how-cicd-works-with-github-actions).
- Informationen zum Einrichten von CI/CD mit **Azure Pipelines** finden Sie unter [Bereitstellen eines Azure-Web-App-Containers aus Azure Pipelines](/azure/devops/pipelines/targets/webapp-on-container-linux).

> [!NOTE]
> Wählen Sie für eine Docker Compose-App die **Containerregistrierung** aus.

Wenn Sie sich für GitHub Actions entscheiden, **klicken** Sie auf **Autorisieren**, und befolgen Sie die Eingabeaufforderungen zur Autorisierung. Wenn Sie die Autorisierung schon einmal mit GitHub durchgeführt haben, können Sie die Bereitstellung über das Repository eines anderen Benutzers ausführen, indem Sie auf **Konto ändern** klicken.

Nachdem Sie Ihr Azure-Konto bei GitHub autorisiert haben, **wählen** Sie das **Unternehmen**, das **Repository** und den **Branch** aus, über die Sie die Bereitstellung vornehmen möchten.
::: zone-end  

::: zone pivot="container-windows"
## <a name="2-configure-registry-settings"></a>2. Konfigurieren der Registrierungseinstellungen
::: zone-end  
::: zone pivot="container-linux"
## <a name="3-configure-registry-settings"></a>3. Konfigurieren der Registrierungseinstellungen

**Wählen** Sie zum Bereitstellen einer App mit mehreren Containern (Docker Compose) unter **Containertyp** die Option **Docker Compose** aus.

Wird das Dropdownmenü **Containertyp** nicht angezeigt, müssen Sie zurück zu **Quelltext** scrollen und **Container Registry** **auswählen**.
::: zone-end

**Wählen** Sie unter **Registrierungsquelle** aus, wo Ihre Containerregistrierung eingerichtet werden soll. Wenn Sie sich weder für Azure Container Registry noch für Docker Hub entscheiden, **wählen** Sie eine **private Registrierung** aus.

::: zone pivot="container-linux"
> [!NOTE]
> Wenn Ihre App mit mehreren Containern (Docker Compose) mehr als ein privates Image verwendet, müssen Sie sicherstellen, dass sich die privaten Images in derselben privaten Registrierung befinden und mit denselben Benutzeranmeldeinformationen zugänglich sind. Wenn Ihre App mit mehreren Containern nur öffentliche Images verwendet, **wählen** Sie **Docker Hub** aus, selbst wenn einige Images nicht in Docker Hub gehostet werden.
::: zone-end  

Klicken Sie auf die Registerkarte Ihrer Lösung, und führen Sie die beschriebenen Schritte aus.

# <a name="azure-container-registry"></a>[Azure Container Registry](#tab/acr)

Im Dropdownmenü **Registrierung** werden die Registrierungen angezeigt, die sich im gleichen Abonnement wie Ihre App befinden. **Wählen** Sie die gewünschte Registrierung aus.

> [!NOTE]
> **Wählen** Sie für Bereitstellungen aus einer Registrierung, die sich in einem anderen Abonnement befindet, unter **Registrierungsquelle** stattdessen **Private Registrierung** aus.

::: zone pivot="container-windows"
**Wählen** Sie das bereitzustellende **Image** und **Tag** aus. Wenn Sie möchten, können Sie unter **Startdatei** den Startbefehl **eingeben**. 
::: zone-end
::: zone pivot="container-linux"
Führen Sie als Nächstes den Schritt für Ihren **Containertyp** aus:
- **Wählen** Sie für **Docker Compose** die Registrierung für Ihre privaten Images aus. **Klicken** Sie auf **Datei auswählen**, um Ihre [Docker Compose-Datei](https://docs.docker.com/compose/compose-file/) hochzuladen, oder **fügen** Sie den Inhalt Ihrer Docker Compose-Datei in die **Konfiguration** ein.
- **Wählen** Sie für **einzelne Container** das bereitzustellende **Image** und **Tag** aus. Wenn Sie möchten, können Sie unter **Startdatei** den Startbefehl **eingeben**. 
::: zone-end

App Service fügt die Zeichenfolge unter **Startdatei** an [das Ende des Befehls „`docker run`“ (als `[COMMAND] [ARG...]`-Segment)](https://docs.docker.com/engine/reference/run/) an, wenn Ihr Container gestartet wird.

# <a name="docker-hub"></a>[Docker Hub](#tab/dockerhub)

::: zone pivot="container-windows"
**Wählen** Sie unter **Repositoryzugriff** aus, ob das bereitzustellende Image öffentlich oder privat sein soll.
::: zone-end
::: zone pivot="container-linux"
**Wählen** Sie unter **Repositoryzugriff** aus, ob das bereitzustellende Image öffentlich oder privat sein soll. **Wählen** Sie für eine Docker Compose-App mit mindestens einem privaten Image **Private** aus.
::: zone-end

Wenn Sie ein privates Image auswählen, **geben** Sie den **Anmeldenamen** (Benutzername) und das **Kennwort** für das Docker-Konto an.

::: zone pivot="container-windows"
**Geben** Sie unter **Vollständiger Imagename und Tag** das Image und den Tagnamen getrennt durch einen `:` an (z. B. `nginx:latest`). Wenn Sie möchten, können Sie unter **Startdatei** den Startbefehl **eingeben**. 
::: zone-end
::: zone pivot="container-linux"
Führen Sie als Nächstes den Schritt für Ihren **Containertyp** aus:
- **Wählen** Sie für **Docker Compose** die Registrierung für Ihre privaten Images aus. **Klicken** Sie auf **Datei auswählen**, um Ihre [Docker Compose-Datei](https://docs.docker.com/compose/compose-file/) hochzuladen, oder **fügen** Sie den Inhalt Ihrer Docker Compose-Datei in die **Konfiguration** ein.
- **Geben** Sie für einen **einzelnen Container** unter **Vollständiger Imagename und Tag** das Image und den Tagnamen getrennt durch einen `:` an (z. B. `nginx:latest`). Wenn Sie möchten, können Sie unter **Startdatei** den Startbefehl **eingeben**. 
::: zone-end

App Service fügt die Zeichenfolge unter **Startdatei** an [das Ende des Befehls „`docker run`“ (als `[COMMAND] [ARG...]`-Segment)](https://docs.docker.com/engine/reference/run/) an, wenn Ihr Container gestartet wird.

# <a name="private-registry"></a>[Private Registrierung](#tab/private)

**Geben** Sie unter **Server-URL** die URL des Servers beginnend mit **https://** ein.

**Geben** Sie unter **Anmeldename** und **Kennwort** die Anmeldeinformationen für Ihre private Registrierung ein.

::: zone pivot="container-windows"
**Geben** Sie unter **Vollständiger Imagename und Tag** das Image und den Tagnamen getrennt durch einen `:` an (z. B. `nginx:latest`). Wenn Sie möchten, können Sie unter **Startdatei** den Startbefehl **eingeben**. 
::: zone-end
::: zone pivot="container-linux"
Führen Sie als Nächstes den Schritt für Ihren **Containertyp** aus:
- **Wählen** Sie für **Docker Compose** die Registrierung für Ihre privaten Images aus. **Klicken** Sie auf **Datei auswählen**, um Ihre [Docker Compose-Datei](https://docs.docker.com/compose/compose-file/) hochzuladen, oder **fügen** Sie den Inhalt Ihrer Docker Compose-Datei in die **Konfiguration** ein.
- **Geben** Sie für einen **einzelnen Container** unter **Vollständiger Imagename und Tag** das Image und den Tagnamen getrennt durch einen `:` an (z. B. `nginx:latest`). Wenn Sie möchten, können Sie unter **Startdatei** den Startbefehl **eingeben**. 
::: zone-end

App Service fügt die Zeichenfolge unter **Startdatei** an [das Ende des Befehls „`docker run`“ (als `[COMMAND] [ARG...]`-Segment)](https://docs.docker.com/engine/reference/run/) an, wenn Ihr Container gestartet wird.

-----

::: zone pivot="container-windows"
## <a name="3-enable-cicd"></a>3. Aktivieren von CI/CD
::: zone-end
::: zone pivot="container-linux"
## <a name="4-enable-cicd"></a>4. Aktivieren von CI/CD
::: zone-end

App Service unterstützt die CI/CD-Integration mit Azure Container Registry und Docker Hub. **Klicken** Sie unter **Dauerhafte Bereitstellung** auf **Ein**, um die Integration zu aktivieren.

::: zone pivot="container-linux"
> [!NOTE]
> Wenn Sie unter **Quelle** die Option **GitHub Actions** auswählen, gibt es keine Integration, weil CI/CD direkt von GitHub Actions verwaltet wird. Stattdessen wird der Bereich **Workflowkonfiguration** angezeigt, in dem Sie auf **Dateivorschau anzeigen** **klicken** können, um sich die Workflowdatei anzusehen. Azure committet diese Datei auf Ihr ausgewähltes GitHub-Repository, damit Build- und Bereitstellungstasks dort verwaltet werden. Weitere Informationen finden Sie unter [Funktionsweise von CI/CD mit GitHub Actions](#how-cicd-works-with-github-actions).
::: zone-end

Wenn Sie diese Option aktivieren, fügt App Service Ihrem Repository in Azure Container Registry oder Docker Hub einen Webhook hinzu. Ihr Repository veröffentlicht immer an diesen Webhook, wenn Ihr ausgewähltes Image mit `docker push` aktualisiert wird. Der Webhook bewirkt, dass Ihre App Service-App neu gestartet wird und `docker pull` ausführt, um das aktualisierte Image abzurufen.

**Bei anderen privaten Registrierungen** können Sie den Webhook manuell oder als Schritt einer CI/CD-Pipeline veröffentlichen. **Klicken** Sie unter **Webhook-URL** auf **Kopieren**, um die Webhook-URL zu kopieren.

::: zone pivot="container-linux"
> [!NOTE]
> Die Unterstützung für Apps mit mehreren Containern (Docker Compose) ist eingeschränkt: 
> - Für Azure Container Registry erstellt App Service einen Webhook in der ausgewählten Registrierung, der nur für diese Registrierung gilt. Ein `docker push`-Befehl für eines der Repositorys in der Registrierung (einschließlich derjenigen, auf die nicht in Ihrer Docker Compose-Datei verwiesen wird) löst einen Neustart der App aus. Daher sollten Sie den Geltungsbereich des [Webhooks eingrenzen](../container-registry/container-registry-webhook.md).
> - Docker Hub unterstützt keine Webhooks auf Registrierungsebene. Sie müssen Webhooks manuell zu den in der Docker Compose-Datei angegebenen Images **hinzufügen**.
::: zone-end

::: zone pivot="container-windows"
## <a name="4-save-your-settings"></a>4. Speichern der Einstellungen
::: zone-end
::: zone pivot="container-linux"
## <a name="5-save-your-settings"></a>5. Speichern der Einstellungen
::: zone-end

**Klicken** Sie auf **Speichern**.

::: zone pivot="container-linux"

## <a name="how-cicd-works-with-github-actions"></a>Funktionsweise von CI/CD mit GitHub Actions

Wenn Sie unter **Quelltext** die Option **GitHub Actions** auswählen (siehe [Auswählen der Bereitstellungsquelle](#2-choose-deployment-source)), richtet App Service CI/CD auf folgende Weise ein:

- Legt eine GitHub Actions-Workflowdatei in Ihrem GitHub-Repository ab, um Build- und Bereitstellungsaufgaben für App Service zu verarbeiten.
- Fügt die Anmeldeinformationen für Ihre private Registrierung als GitHub-Geheimnisse hinzu. Die generierte Workflowdatei führt die Aktion [Azure/docker-login](https://github.com/Azure/docker-login) aus, um sich bei Ihrer privaten Registrierung anzumelden, und führt dann `docker push` für die Bereitstellung aus.
- Fügt das Veröffentlichungsprofil für Ihre App als GitHub-Geheimnis hinzu. Die generierte Workflowdatei verwendet dieses Geheimnis für die Authentifizierung bei App Service und führt dann die Aktion [Azure/webapps-deploy](https://github.com/Azure/webapps-deploy) aus, um das aktualisierte Image zu konfigurieren. Dadurch wird ein App-Neustart ausgelöst, um das aktualisierte Image abzurufen.
- Erfasst Informationen aus den [Workflowausführungsprotokollen](https://docs.github.com/actions/managing-workflow-runs/using-workflow-run-logs) und zeigt sie auf der Registerkarte **Protokolle** im **Deployment Center** Ihrer App an.

Sie können den GitHub Actions-Buildanbieter auf folgende Weise anpassen:

- Anpassen der Workflowdatei, nachdem Sie in Ihrem GitHub-Repository generiert wurde. Weitere Informationen finden Sie unter [Workflowsyntax für GitHub Actions](https://docs.github.com/actions/reference/workflow-syntax-for-github-actions). Stellen Sie unbedingt sicher, dass der Workflow mit der Aktion [Azure/webapps-deploy](https://github.com/Azure/webapps-deploy) endet, um einen App-Neustart auszulösen.
- Wenn der ausgewählte Branch geschützt ist, können Sie trotzdem eine Vorschau der Workflowdatei anzeigen, ohne die Konfiguration zu speichern, und sie dann zusammen mit den erforderlichen GitHub-Geheimnissen manuell zu Ihrem Repository hinzufügen. Diese Methode verschafft Ihnen nicht die Protokollintegration in das Azure-Portal.
- Anstatt ein Veröffentlichungsprofil zu verwenden, stellen Sie mithilfe eines [Dienstprinzipals](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) in Azure Active Directory bereit.

#### <a name="authenticate-with-a-service-principal"></a>Authentifizieren mit einem Dienstprinzipal

Diese optionale Konfiguration ersetzt die Standardauthentifizierung mit Veröffentlichungsprofilen in der generierten Workflowdatei.

**Generieren** Sie in der [Azure CLI](/cli/azure/) mit dem Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) einen Dienstprinzipal. Ersetzen Sie im folgenden Beispiel *\<subscription-id>* , *\<group-name>* und *\<app-name>* durch Ihre eigenen Werte. **Speichern** Sie die gesamte JSON-Ausgabe für den nächsten Schritt, einschließlich der `{}`-Objekte der obersten Ebene.

```azurecli-interactive
az ad sp create-for-rbac --name "myAppDeployAuth" --role contributor \
                            --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> \
                            --sdk-auth
```

> [!IMPORTANT]
> Erteilen Sie aus Sicherheitsgründen dem Dienstprinzipal den minimal erforderlichen Zugriff. Der Bereich im vorherigen Beispiel ist auf die spezifische App Service-App und nicht auf die gesamte Ressourcengruppe beschränkt.

**Navigieren** Sie in [GitHub](https://github.com/) zu Ihrem Repository, und **klicken** Sie auf **Einstellungen > Geheimnisse > Add a new secret** (Neues Geheimnis hinzufügen). **Fügen** Sie die gesamte JSON-Ausgabe aus dem Azure CLI-Befehl in das Wertfeld des Geheimnisses ein. **Geben** Sie dem Geheimnis einen Namen wie `AZURE_CREDENTIALS`.

**Ändern** Sie in der Workflowdatei, die vom **Bereitstellungscenter** generiert wurde, den Schritt `azure/webapps-deploy` mit Code wie im folgenden Beispiel:

```yaml
- name: Sign in to Azure 
# Use the GitHub secret you added
- uses: azure/login@v1
    with:
    creds: ${{ secrets.AZURE_CREDENTIALS }}
- name: Deploy to Azure Web App
# Remove publish-profile
- uses: azure/webapps-deploy@v2
    with:
    app-name: '<app-name>'
    slot-name: 'production'
    images: '<registry-server>/${{ secrets.AzureAppService_ContainerUsername_... }}/<image>:${{ github.sha }}'
    - name: Sign out of Azure
    run: |
    az logout
```

::: zone-end

## <a name="automate-with-cli"></a>Automatisieren mithilfe der Befehlszeilenschnittstelle

**Führen** Sie [az webapp config container set](/cli/azure/webapp/config/container#az_webapp_config_container_set) aus, um die Containerregistrierung und das Docker-Image zu konfigurieren.

# <a name="azure-container-registry"></a>[Azure Container Registry](#tab/acr)

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name '<image>:<tag>' --docker-registry-server-url 'https://<registry-name>.azurecr.io' --docker-registry-server-user '<username>' --docker-registry-server-password '<password>'
```

# <a name="docker-hub"></a>[Docker Hub](#tab/dockerhub)

```azurecli-interactive
# Public image
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <image-name>

# Private image
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name <image-name> --docker-registry-server-user <username> --docker-registry-server-password <password>
```

# <a name="private-registry"></a>[Private Registrierung](#tab/private)

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group <group-name> --docker-custom-image-name '<image>:<tag>' --docker-registry-server-url <private-repo-url> --docker-registry-server-user <username> --docker-registry-server-password <password>
```

-----

::: zone pivot="container-linux"
**Bereiten** Sie zum Konfigurieren einer App mit mehreren Containern eine Docker Compose-Datei vor, und **führen** Sie [az webapp config container set](/cli/azure/webapp/config/container#az_webapp_config_container_set) mit dem `--multicontainer-config-file`-Parameter aus. Wenn Ihre Docker Compose-Datei private Images enthält, **fügen** Sie, wie im vorherigen Beispiel gezeigt, `--docker-registry-server-*`-Parameter hinzu.

```azurecli-interactive
az webapp config container set --resource-group <group-name> --name <app-name> --multicontainer-config-file <docker-compose-file>
```
::: zone-end

**Führen** Sie [az webapp deployment container config](/cli/azure/webapp/deployment/container#az_webapp_deployment-container-config) mit dem `--enable-cd`-Parameter aus, um CI/CD von der Containerregistrierung zu Ihrer App zu konfigurieren. Der Befehl gibt die Webhook-URL aus. Sie müssen den Webhook jedoch in einem separaten Schritt manuell in der Registrierung erstellen. Im folgenden Beispiel wird CI/CD in Ihrer App aktiviert. Anschließend wird die Webhook-URL in der Ausgabe verwendet, um den Webhook in Azure Container Registry zu erstellen.

```azurecli-interactive
ci_cd_url=$(az webapp deployment container config --name <app-name> --resource-group <group-name> --enable-cd true --query CI_CD_URL --output tsv)

az acr webhook create --name <webhook-name> --registry <registry-name> --resource-group <group-name> --actions push --uri $ci_cd_url --scope '<image>:<tag>'
```

## <a name="more-resources"></a>Weitere Ressourcen

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Erstellen einer .NET Core-Web-App in App Service unter Linux](quickstart-dotnetcore.md)
* [Schnellstart: Ausführen eines benutzerdefinierten Containers in App Service](quickstart-custom-container.md)
* [App Service unter Linux – häufig gestellte Fragen](faq-app-service-linux.md)
* [Konfigurieren eines benutzerdefinierten Containers](configure-custom-container.md)
* [Aktionsworkflows für das Bereitstellen in Azure](https://github.com/Azure/actions-workflow-samples)
