---
title: Konfigurieren von Anmeldeinformationen für die Bereitstellung
description: Erfahren Sie, welche Arten von Anmeldeinformationen für die Bereitstellung in Azure App Service verfügbar sind und wie Sie diese konfigurieren und verwenden.
ms.topic: article
ms.date: 02/11/2021
ms.reviewer: byvinyal
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: b77a26f61e1168846156de990806bbed2f7c41e3
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107789535"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Konfigurieren von Anmeldeinformationen für die Azure App Service-Bereitstellung
Um die App-Bereitstellung von einem lokalen Computer aus zu schützen, unterstützt [Azure App Service](./overview.md) zwei Arten von Anmeldeinformationen für die [lokale Git-Bereitstellung](deploy-local-git.md) und [FTP/S-Bereitstellungen](deploy-ftp.md). Diese Anmeldeinformationen sind nicht identisch mit den Anmeldeinformationen Ihres Azure-Abonnements.

[!INCLUDE [app-service-deploy-credentials](../../includes/app-service-deploy-credentials.md)]

> [!NOTE]
> Die Seite **Development Center (klassisch)** im Azure-Portal, bei der es sich um die alte Bereitstellung handelt, wird im März 2021 eingestellt. Diese Änderung wirkt sich nicht auf vorhandene Bereitstellungseinstellungen in Ihrer APP aus, und Sie können die APP-Bereitstellung weiterhin auf der Seite **Bereitstellungscenter** verwalten.

## <a name="configure-user-scope-credentials"></a><a name="userscope"></a>Konfigurieren der Anmeldeinformationen für den Benutzerbereich

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Führen Sie den Befehl [az webapp deployment user set](/cli/azure/webapp/deployment/user#az_webapp_deployment_user_set) aus. Ersetzen Sie \<username> und \<password> durch Ihren Benutzernamen bzw. Ihr Kennwort für die Bereitstellung. 

- Der Benutzername muss in Azure eindeutig sein und darf bei lokalen Git-Pushes nicht das Symbol „@“ enthalten. 
- Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen und Symbole. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

In der JSON-Ausgabe wird das Kennwort als `null` angezeigt.

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Die Anmeldeinformationen für den Benutzerbereich können nicht mit Azure PowerShell konfiguriert werden. Verwenden Sie eine andere Methode, oder ziehen Sie die Verwendung von [Anmeldeinformationen für den Anwendungsbereich](#appscope) in Erwägung. 

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Sie können die Anmeldeinformationen für den Benutzerbereich auf der [Ressourcenseite](../azure-resource-manager/management/manage-resources-portal.md#manage-resources) einer App konfigurieren. Unabhängig von der App, in der Sie diese Anmeldeinformationen konfigurieren, gelten sie für alle Apps und alle Abonnements in Ihrem Azure-Konto. 

Im [Azure-Portal](https://portal.azure.com) muss mindestens eine App vorhanden sein, bevor Sie auf die Seite mit den Anmeldeinformationen für die Bereitstellung zugreifen können. So konfigurieren Sie die Anmeldeinformationen für Ihren Benutzerbereich

1. Wählen Sie im linken Menü Ihrer App **Bereitstellungscenter** > **FTPS-Anmeldeinformationen** oder **Lokale Git-/FTPS-Anmeldeinformationen** aus.

    ![Zeigt, wie Sie das FTP-Dashboard im Bereitstellungscenter in Azure App Services auswählen.](./media/app-service-deployment-credentials/access-no-git.png)

2. Scrollen Sie nach unten zu **Benutzerbereich**, konfigurieren Sie **Benutzername** und **Kennwort**, und wählen Sie dann **Speichern** aus.

Wenn Sie Anmeldeinformationen für die Bereitstellung festgelegt haben, finden Sie den *Git*-Benutzernamen für Bereitstellungen auf der Seite **Übersicht** Ihrer App.

![Zeigt, wie Sie den Benutzernamen der Git-Bereitstellung auf der Übersichtsseite Ihrer App finden.](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Wenn die Git-Bereitstellung konfiguriert ist, wird auf der Seite ein **Git-/Bereitstellungsbenutzername** angezeigt, andernfalls ein **FTP-/Bereitstellungsbenutzername**.

> [!NOTE]
> Ihr Kennwort für den Benutzerbereich für die Bereitstellung wird von Azure nicht angezeigt. Wenn Sie das Kennwort vergessen, können Sie Ihre Anmeldeinformationen über die Schritte in diesem Abschnitt zurücksetzen.
>
> 

-----

## <a name="use-user-scope-credentials-with-ftpftps"></a>Verwenden von Anmeldeinformationen für den Benutzerbereich mit FTP/FTPS

Für die Authentifizierung an einem FTP-/FTPS-Endpunkt mit Anmeldeinformationen für den Benutzerbereich ist ein Benutzername im folgenden Format erforderlich: `<app-name>\<user-name>`

Da Anmeldeinformationen für den Benutzerbereich mit dem Benutzer und nicht einer bestimmten Ressource verknüpft sind, muss der Benutzername dieses Format aufweisen, damit die Anmeldeaktion an den richtigen App-Endpunkt weitergeleitet wird.

## <a name="get-application-scope-credentials"></a><a name="appscope"></a>Abrufen von Anmeldeinformationen für den Anwendungsbereich

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Verwenden Sie den Befehl [az webapp deployment list-publishing-profiles](/cli/azure/webapp/deployment#az_webapp_deployment_list_publishing_profiles), um die Anmeldeinformationen für den Anwendungsbereich abzurufen. Beispiel:

```azurecli-interactive
az webapp deployment list-publishing-profiles --resource-group <group-name> --name <app-name>
```

Bei einer [lokalen Git-Bereitstellung](deploy-local-git.md) können Sie auch den Befehl [az webapp deployment list-publishing-credentials](/cli/azure/webapp/deployment#az_webapp_deployment_list_publishing_credentials) verwenden, um einen Git-Remote-URI für Ihre App abzurufen. Darin sind die Anmeldeinformationen für den Anwendungsbereich bereits eingebettet. Beispiel:

```azurecli-interactive
az webapp deployment list-publishing-credentials --resource-group <group-name> --name <app-name> --query scmUri
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Rufen Sie die Anmeldeinformationen für den Anwendungsbereich mithilfe des Befehls [Get-AzWebAppPublishingProfile](/powershell/module/az.websites/get-azwebapppublishingprofile) ab. Beispiel:

```azurepowershell-interactive
Get-AzWebAppPublishingProfile -ResourceGroupName <group-name> -Name <app-name>
```

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

1. Wählen Sie im linken Menü Ihrer App **Bereitstellungscenter** > **FTPS-Anmeldeinformationen** oder **Lokale Git-/FTPS-Anmeldeinformationen** aus.

    ![Zeigt, wie Sie das FTP-Dashboard im Bereitstellungscenter in Azure App Services auswählen.](./media/app-service-deployment-credentials/access-no-git.png)

2. Wählen Sie im Abschnitt **Anwendungsbereich** den Link **Kopieren** aus, um den Benutzernamen oder das Kennwort zu kopieren.

-----

## <a name="reset-application-scope-credentials"></a>Zurücksetzen von Anmeldeinformationen für den Anwendungsbereich

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Sie setzen die Anmeldeinformationen für den Anwendungsbereich mit dem Befehl [az resource invoke-action](/cli/azure/resource#az_resource_invoke_action) zurück:

```azurecli-interactive
az resource invoke-action --action newpassword --resource-group <group-name> --name <app-name> --resource-type Microsoft.Web/sites
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Sie setzen die Anmeldeinformationen für den Anwendungsbereich mit dem Befehl [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction) zurück:

```azurepowershell-interactive
Invoke-AzResourceAction -ResourceGroupName <group-name> -ResourceType Microsoft.Web/sites -ResourceName <app-name> -Action newpassword
```

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

1. Wählen Sie im linken Menü Ihrer App **Bereitstellungscenter** > **FTPS-Anmeldeinformationen** oder **Lokale Git-/FTPS-Anmeldeinformationen** aus.

    ![Zeigt, wie Sie das FTP-Dashboard im Bereitstellungscenter in Azure App Services auswählen.](./media/app-service-deployment-credentials/access-no-git.png)

2. Wählen Sie im Abschnitt **Anwendungsbereich** die Option **Zurücksetzen** aus.

-----

## <a name="disable-basic-authentication"></a>Deaktivieren der Standardauthentifizierung

Einige Organisationen müssen Sicherheitsanforderungen erfüllen und möchten daher den Zugriff über FTP oder WebDeploy deaktivieren. Auf diese Weise können die Mitglieder der Organisation nur auf Ihre App Services über APIs zugreifen, die von Azure Active Directory (Azure AD) gesteuert werden.

### <a name="ftp"></a>FTP

Um den FTP-Zugriff auf die Website zu deaktivieren, führen Sie den folgenden CLI-Befehl aus. Ersetzen Sie die Platzhalter durch die Ressourcengruppe und den Websitenamen. 

```azurecli-interactive
az resource update --resource-group <resource-group> --name ftp --namespace Microsoft.Web --resource-type basicPublishingCredentialsPolicies --parent sites/<site-name> --set properties.allow=false
```

Um zu bestätigen, dass der FTP-Zugriff blockiert ist, können Sie versuchen, sich mit einem FTP-Client wie FileZilla zu authentifizieren. Zum Abrufen der Veröffentlichungsanmeldeinformationen navigieren Sie zum Übersichtsblatt Ihrer Website, und klicken Sie auf „Veröffentlichungsprofil herunterladen“. Verwenden Sie den FTP-Hostnamen, den Benutzernamen und das Kennwort für die Datei, um sich zu authentifizieren, und Sie erhalten eine 401-Fehlerantwort, die angibt, dass Sie nicht autorisiert sind.

### <a name="webdeploy-and-scm"></a>WebDeploy und SCM

Führen Sie den folgenden CLI-Befehl aus, um Standardauthentifizierungszugriff auf den WebDeploy-Port und die SCM-Website zu deaktivieren. Ersetzen Sie die Platzhalter durch die Ressourcengruppe und den Websitenamen. 

```azurecli-interactive
az resource update --resource-group <resource-group> --name scm --namespace Microsoft.Web --resource-type basicPublishingCredentialsPolicies --parent sites/<site-name> --set properties.allow=false
```

Um zu bestätigen, dass die Anmeldeinformationen für das Veröffentlichungsprofil für WebDeploy blockiert werden, versuchen Sie, eine [Web-App mit Visual Studio 2019 zu veröffentlichen](/visualstudio/deployment/quickstart-deploy-to-azure).

### <a name="disable-access-to-the-api"></a>Deaktivieren des Zugriffs auf die API

Die API im vorherigen Abschnitt basiert auf rollenbasierter Zugriffssteuerung von Azure (Azure RBAC). Das bedeutet, dass Sie eine [benutzerdefinierte Rolle erstellen](../role-based-access-control/custom-roles.md#steps-to-create-a-custom-role) und der Rolle Benutzer mit niedrigeren Berechtigungen zuweisen können, sodass diese keine Standardauthentifizierung für beliebige Websites aktivieren können. Um die benutzerdefinierte Rolle zu konfigurieren, [befolgen Sie diese Anweisungen](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#create-a-custom-rbac-role).

Sie können auch [Azure Monitor](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#audit-with-azure-monitor) verwenden, um alle erfolgreichen Authentifizierungsanforderungen zu überwachen, sowie [Azure Policy](https://azure.github.io/AppService/2020/08/10/securing-data-plane-access.html#enforce-compliance-with-azure-policy), um diese Konfiguration für alle Standorte in Ihrem Abonnement zu erzwingen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie diese Anmeldeinformationen zum Bereitstellen Ihrer App über [lokales Git](deploy-local-git.md) oder [FTP/S](deploy-ftp.md) verwenden.
