---
title: Konfigurieren von Continuous Deployment
description: Hier erfahren Sie, wie Sie in Azure App Service CI/CD aus GitHub, BitBucket, Azure Repos und anderen Repositorys aktivieren. Wählen Sie die Buildpipeline aus, die Ihren Anforderungen entspricht.
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.topic: article
ms.date: 03/12/2021
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 633d62fc69c516b482d5749a07052337dc71f567
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107789481"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Continuous Deployment in Azure App Service

Der [Azure App Service](overview.md) ermöglicht die kontinuierliche Bereitstellung von [GitHub](https://help.github.com/articles/create-a-repo)-, [BitBucket](https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html)- und [Azure Repos](/azure/devops/repos/git/creatingrepo)-Repositorys durch Abrufen der neuesten Updates.

> [!NOTE]
> Die Seite **Development Center (klassisch)** im Azure-Portal, bei der es sich um die alte Bereitstellung handelt, wird im März 2021 eingestellt. Diese Änderung wirkt sich nicht auf vorhandene Bereitstellungseinstellungen in Ihrer APP aus, und Sie können die APP-Bereitstellung weiterhin auf der Seite **Deployment Center** verwalten.

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="configure-deployment-source"></a>Bereitstellungsquelle konfigurieren

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Verwaltungsseite Ihrer App Service-App.

1. Klicken Sie im Menü auf der linken Seite auf **Deployment Center** > **Einstellungen**. 

1. Wählen Sie unter **Quelle** eine der CI/CD-Optionen aus.

    ![Zeigt, wie die Bereitstellungsquelle im Deployment Center für Azure App Service ausgewählt wird.](media/app-service-continuous-deployment/choose-source.png)

Wählen Sie die Registerkarte aus, die Ihrer Auswahl für die Schritte entspricht.

# <a name="github"></a>[GitHub](#tab/github)

4. [GitHub Actions](#how-the-github-actions-build-provider-works) sind der Standardbuildanbieter. Um dies zu ändern, klicken Sie auf **Anbieter ändern**  >  **App Service-Builddienst** (Kudu) > **OK**.

    > [!NOTE]
    > Wenn Sie Azure Pipelines als Buildanbieter für Ihre App Service-App verwenden möchten, konfigurieren Sie ihn nicht in App Service. Konfigurieren Sie stattdessen CI/CD direkt aus Azure Pipelines. Die Option **Azure Pipelines** weist Sie lediglich in die richtige Richtung.

1. Wenn Sie zum ersten Mal von GitHub aus bereitstellen, klicken Sie auf **Autorisieren**, und befolgen Sie die Autorisierungsaufforderungen. Wenn Sie aus dem Repository eines anderen Benutzers bereitstellen möchten, klicken Sie auf **Konto ändern**.

1. Nachdem Sie Ihr Azure-Konto bei GitHub autorisiert haben, wählen Sie die **Organisation**, das **Repository** und den **Branch** aus, für die Sie CI/CD konfigurieren möchten.

1. Wenn GitHub Actions der ausgewählte Buildanbieter ist, können Sie die gewünschte Workflowdatei über die Dropdownlisten **Laufzeitstapel** und **Version** auswählen. Azure committet diese Workflowdatei in Ihr ausgewähltes GitHub-Repository, um Build- und Bereitstellungsaufgaben zu verarbeiten. Wenn Sie die Datei vor dem Speichern Ihrer Änderungen anzeigen möchten, klicken Sie auf **Dateivorschau anzeigen**.

    > [!NOTE]
    > App Service erkennt die [Sprachstapeleinstellung](configure-common.md#configure-language-stack-settings) Ihrer App und wählt die geeignetste Workflowvorlage aus. Wenn Sie eine andere Vorlage auswählen, wird möglicherweise eine App bereitgestellt, die nicht ordnungsgemäß ausgeführt wird. Weitere Informationen finden Sie unter [Funktionsweise des GitHub Actions-Buildanbieters](#how-the-github-actions-build-provider-works).

1. Klicken Sie auf **Speichern**.
   
    Neue Commits im ausgewählten Repository und Branch werden nun fortlaufend in Ihrer App Service-App bereitgestellt. Sie können die Commits und Bereitstellungen auf der Registerkarte **Protokolle** nachverfolgen.

# <a name="bitbucket"></a>[BitBucket](#tab/bitbucket)

Die BitBucket-Integration verwendet die App Service-Builddienste (Kudu) für die Buildautomatisierung.

4. Wenn Sie zum ersten Mal aus BitBucket bereitstellen, klicken Sie auf **Autorisieren**, und befolgen Sie die Autorisierungsaufforderungen. Wenn Sie aus dem Repository eines anderen Benutzers bereitstellen möchten, klicken Sie auf **Konto ändern**.

1. Wählen Sie für Bitbucket **„Team“** aus, dann **„Repository“** und die **„Branch“** , die Sie fortlaufend nutzen möchten.

1. Klicken Sie auf **Speichern**.
   
    Neue Commits im ausgewählten Repository und Branch werden nun fortlaufend in Ihrer App Service-App bereitgestellt. Sie können die Commits und Bereitstellungen auf der Registerkarte **Protokolle** nachverfolgen.
   
# <a name="local-git"></a>[Git (lokal)](#tab/local)

Siehe [Lokale Git-Bereitstellung in Azure App Service](deploy-local-git.md).

# <a name="azure-repos"></a>[Azure Repos](#tab/repos)

> [!NOTE]
> Azure Repos als Bereitstellungsquelle wird für Windows-Apps unterstützt.
>

4. App Service-Builddienst (Kudu) ist der Standardbuildanbieter.

    > [!NOTE]
    > Wenn Sie Azure Pipelines als Buildanbieter für Ihre App Service-App verwenden möchten, konfigurieren Sie ihn nicht in App Service. Konfigurieren Sie stattdessen CI/CD direkt aus Azure Pipelines. Die Option **Azure Pipelines** weist Sie lediglich in die richtige Richtung.

1. Wählen Sie die **Azure DevOps-Organisation**, das **Projekt**, das **Repository** und den **Branch** aus, die Sie fortlaufend bereitstellen möchten. 

    Wenn Ihre DevOps-Organisation nicht aufgeführt ist, ist sie noch nicht mit Ihrem Azure-Abonnement verknüpft. Weitere Informationen finden Sie unter [Erstellen einer Azure-Dienstverbindung](/azure/devops/pipelines/library/connect-to-azure).

-----

## <a name="disable-continuous-deployment"></a>Deaktivieren der fortlaufenden Bereitstellung

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zur Verwaltungsseite Ihrer App Service-App.

1. Klicken Sie im Menü auf der linken Seite auf **Bereitstellungscenter** > **Einstellungen** > **Trennen**. 

    ![Zeigt, wie Sie die Synchronisierung Ihres Cloudordners mit Ihrer App Service-App im Azure-Portal trennen.](media/app-service-continuous-deployment/disable.png)

1. Standardmäßig bleibt die GitHub Actions-Workflowdatei in Ihrem Repository erhalten, aber sie löst weiterhin die Bereitstellung für Ihre App aus. Um sie aus Ihrem Repository zu löschen, wählen Sie **Workflowdatei löschen** aus.

1. Klicken Sie auf **OK**.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="how-the-github-actions-build-provider-works"></a>Funktionsweise des GitHub Actions-Buildanbieters

Der GitHub Actions-Buildanbieter ist eine Option für [CI/CD von GitHub](#configure-deployment-source) und führt Folgendes aus, um CI/CD einzurichten:

- Legt eine GitHub Actions-Workflowdatei in Ihrem GitHub-Repository ab, um Build- und Bereitstellungsaufgaben für App Service zu verarbeiten.
- Fügt das Veröffentlichungsprofil für Ihre App als GitHub-Geheimnis hinzu. Die Workflowdatei verwendet dieses Geheimnis für die Authentifizierung bei App Service.
- Erfasst Informationen aus den [Workflowausführungsprotokollen](https://docs.github.com/actions/managing-workflow-runs/using-workflow-run-logs) und zeigt sie auf der Registerkarte **Protokolle** im **Deployment Center** Ihrer App an.

Sie können den GitHub Actions-Buildanbieter auf folgende Weise anpassen:

- Anpassen der Workflowdatei, nachdem Sie in Ihrem GitHub-Repository generiert wurde. Weitere Informationen finden Sie unter [Workflowsyntax für GitHub Actions](https://docs.github.com/actions/reference/workflow-syntax-for-github-actions). Stellen Sie lediglich sicher, dass der Workflow in App Service bereitgestellt wird, mit der Aktion [azure/webapps-deploy](https://github.com/Azure/webapps-deploy).
- Wenn der ausgewählte Branch geschützt ist, können Sie immer noch eine Vorschau der Workflowdatei anzeigen, ohne die Konfiguration zu speichern, und sie dann manuell in Ihrem Repository hinzufügen. Diese Methode verschafft Ihnen nicht die Protokollintegration in das Azure-Portal.
- Anstatt ein Veröffentlichungsprofil zu verwenden, stellen Sie mithilfe eines [Dienstprinzipals](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) in Azure Active Directory bereit.

#### <a name="authenticate-with-a-service-principal"></a>Authentifizieren mit einem Dienstprinzipal

Diese optionale Konfiguration ersetzt die Standardauthentifizierung mit Veröffentlichungsprofilen in der generierten Workflowdatei.

1. Generieren Sie mit dem Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#az_ad_sp_create_for_rbac) in der [Azure CLI](/cli/azure/) einen Dienstprinzipal. Ersetzen Sie im folgenden Beispielen *\<subscription-id>* , *\<group-name>* und *\<app-name>* durch Ihre eigenen Werte:

    ```azurecli-interactive
    az ad sp create-for-rbac --name "myAppDeployAuth" --role contributor \
                                --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> \
                                --sdk-auth
    ```
    
    > [!IMPORTANT]
    > Erteilen Sie aus Sicherheitsgründen dem Dienstprinzipal den minimal erforderlichen Zugriff. Der Bereich im vorherigen Beispiel ist auf die spezifische App Service-App und nicht auf die gesamte Ressourcengruppe beschränkt.
    
1. Speichern Sie die gesamte JSON-Ausgabe für den nächsten Schritt, einschließlich `{}` der obersten Ebene.

1. Navigieren Sie in [GitHub](https://github.com/) zu Ihrem Repository, und wählen Sie **Einstellungen > Geheimnisse > Neues Geheimnis hinzufügen** aus.

1. Fügen Sie die gesamte JSON-Ausgabe aus dem Azure CLI-Befehl in das Wertfeld des Geheimnisses ein. Geben Sie dem Geheimnis einen Namen wie `AZURE_CREDENTIALS`.

1. Überarbeiten Sie in der Workflowdatei, die vom **Deployment Center** generiert wurde, den `azure/webapps-deploy`-Schritt mit Code wie im folgenden Beispiel zu sehen (der geändert aus einer Node.js-Workflowdatei stammt):

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
        package: .
    - name: Sign out of Azure
      run: |
        az logout
    ```
    
## <a name="deploy-from-other-repositories"></a>Bereitstellen aus anderen Repositorys

Für Windows-Apps können Sie Continuous Deployment aus einem Git- oder Mecurial-Cloudrepository, das das Portal nicht direkt unterstützt (beispielsweise [GitLab](https://gitlab.com/)), manuell konfigurieren. Wählen Sie hierzu in der Dropdownliste **Quelle** die Option „Git (extern)“ (External Git) aus. Weitere Informationen finden Sie unter [Einrichten von Continuous Deployment mit manuellen Schritten](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="more-resources"></a>Weitere Ressourcen

* [Bereitstellen aus Azure Pipelines in Azure App Services](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps)
* [Untersuchen häufiger Probleme mit Continuous Deployment](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Verwenden von Azure PowerShell](/powershell/azure/)
* [Projekt Kudu](https://github.com/projectkudu/kudu/wiki)
