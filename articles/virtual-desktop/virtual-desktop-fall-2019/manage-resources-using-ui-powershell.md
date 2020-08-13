---
title: Bereitstellen eines Verwaltungstools für Windows Virtual Desktop (klassisch) mithilfe des Dienstprinzipals – Azure
description: Erfahren Sie, wie Sie das Verwaltungstool für Windows Virtual Desktop (klassisch) mithilfe von PowerShell bereitstellen.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 08366735308e8b3c6fb79efd1adca469272dd053
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "88005770"
---
# <a name="deploy-a-windows-virtual-desktop-classic-management-tool-with-powershell"></a>Bereitstellen eines Windows Virtual Desktop-Verwaltungstools (klassisch) mit PowerShell

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop (klassisch). Der Dienst unterstützt keine Windows Virtual Desktop-Objekte in Azure Resource Manager.

In diesem Artikel wird gezeigt, wie das Bereitstellungstool mithilfe von PowerShell bereitgestellt wird.

## <a name="important-considerations"></a>Wichtige Hinweise

Das Verwaltungstool muss für jedes Abonnement des Azure Active Directory-Mandanten (Azure AD) separat bereitgestellt werden. Das Tool unterstützt keine Azure AD Business-to-Business (B2B)-Szenarien.

Dieses Verwaltungstool ist ein Beispiel. Microsoft stellt wichtige Sicherheits- und Qualitätsupdates zur Verfügung. [Der Quellcode steht auf GitHub zur Verfügung](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy). Egal, ob Sie ein Kunde oder Partner sind, sollten Sie das Tool an Ihre geschäftlichen Anforderungen anpassen.

Die folgenden Browser sind mit dem Verwaltungstool kompatibel:

- Google Chrome 68 oder höher
- Microsoft Edge 40.15063 oder höher
- Mozilla Firefox 52.0 oder höher
- Safari 10 oder höher (nur macOS)

## <a name="what-you-need-to-deploy-the-management-tool"></a>Was Sie zum Bereitstellen des Verwaltungstools benötigen

Bevor Sie das Verwaltungstool bereitstellen, benötigen Sie einen Azure Active Directory (Azure AD)-Benutzer, um eine App-Registrierung zu erstellen und die Verwaltungsbenutzeroberfläche bereitzustellen. Für diesen Benutzer gilt Folgendes:

- Er muss über die Berechtigung zum Erstellen von Ressourcen in Ihrem Azure-Abonnement verfügen.
- Er muss über die Berechtigung zum Erstellen einer Azure AD-Anwendung verfügen. Führen Sie die folgenden Schritte aus, um zu überprüfen, ob der Benutzer über die erforderlichen Berechtigungen verfügt, indem Sie die Anweisungen unter [Erforderliche Berechtigungen](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app) befolgen.

Nachdem Sie das Verwaltungstool bereitgestellt und konfiguriert haben, sollten Sie einen Benutzer auffordern, die Verwaltungsbenutzeroberfläche aufzurufen, um sicherzustellen, dass alles funktioniert. Der Benutzer, der die Verwaltungsbenutzeroberfläche aufruft, muss über eine Rollenzuweisung verfügen, die es ihm erlaubt, den Windows Virtual Desktop-Mandanten anzuzeigen oder zu bearbeiten.

## <a name="set-up-powershell"></a>Einrichten von PowerShell

Melden Sie sich zunächst bei den PowerShell-Modulen „Az“ und „Azure AD“ an. Gehen Sie folgendermaßen vor, um sich anzumelden:

1. Öffnen Sie PowerShell als Administrator, und navigieren Sie zu dem Verzeichnis, in das Sie die PowerShell-Skripts heruntergeladen haben.
2. Melden Sie sich bei Azure mit einem Konto an, das die Berechtigung „Besitzer“ oder „Mitwirkender“ für das Azure-Abonnement besitzt, das Sie zum Erstellen des Verwaltungstools verwenden möchten, indem Sie das folgende Cmdlet ausführen:

    ```powershell
    Login-AzAccount
    ```

3. Führen Sie das folgende Cmdlet aus, um sich bei Azure AD mit demselben Konto anzumelden, das Sie für das PowerShell-Modul „Az“ verwendet haben:

    ```powershell
    Connect-AzureAD
    ```

4. Navigieren Sie anschließend zu dem Ordner, in dem Sie die beiden PowerShell-Skripts aus dem GitHub-Repository „RDS-Templates“ gespeichert haben.

Lassen Sie das PowerShell-Fenster, das Sie für die Anmeldung geöffnet haben, geöffnet, um weitere PowerShell-Cmdlets auszuführen, während Sie angemeldet sind.

## <a name="create-an-azure-active-directory-app-registration"></a>Erstellen einer Azure Active Directory-App-Registrierung

Um das Verwaltungstool erfolgreich bereitzustellen und zu konfigurieren, müssen Sie zunächst die folgenden PowerShell-Skripts aus dem [GitHub-Repository „RDS-Templates“](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy/scripts) herunterladen.

```powershell
Set-Location -Path "c:\temp"
$uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-management-ux/deploy/scripts/createWvdMgmtUxAppRegistration.ps1"
Invoke-WebRequest -Uri $uri -OutFile ".\createWvdMgmtUxAppRegistration.ps1"
$uri = "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-management-ux/deploy/scripts/updateWvdMgmtUxApiUrl.ps1"
Invoke-WebRequest -Uri $uri -OutFile ".\updateWvdMgmtUxApiUrl.ps1"
```

Führen Sie die folgenden Befehle aus, um die App-Registrierung mit erforderlichen API-Berechtigungen zu erstellen:

```powershell
$appName = Read-Host -Prompt "Enter a unique name for the management tool's app registration. The name can't contain spaces or special characters."
$azureSubscription = Get-AzSubscription | Out-GridView -PassThru
$subscriptionId = $azureSubscription.Id
Get-AzSubscription -SubscriptionId $subscriptionId | Select-AzSubscription

.\createWvdMgmtUxAppRegistration.ps1 -AppName $appName -SubscriptionId $subscriptionId
```

Nachdem Sie die Registrierung der Azure AD-App abgeschlossen haben, können Sie das Verwaltungstool bereitstellen.

## <a name="deploy-the-management-tool"></a>Bereitstellen des Verwaltungstools

Führen Sie die folgenden PowerShell-Befehle aus, um das Verwaltungstool bereitzustellen und es dem soeben erstellten Dienstprinzipal zuzuordnen:

```powershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateParameters = @{}
$templateParameters.Add('isServicePrincipal', $true)
$templateParameters.Add('azureAdminUserPrincipalNameOrApplicationId', $ServicePrincipalCredentials.UserName)
$templateParameters.Add('azureAdminPassword', $servicePrincipalCredentials.Password)
$templateParameters.Add('applicationName', $appName)

Get-AzSubscription -SubscriptionId $subscriptionId | Select-AzSubscription
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/RDS-Templates/master/wvd-templates/wvd-management-ux/deploy/mainTemplate.json" `
    -TemplateParameterObject $templateParameters `
    -Verbose
```

Nachdem Sie die Web-App erstellt haben, müssen Sie der Azure AD Anwendung einen Umleitungs-URI hinzufügen, damit sich Benutzer anmelden können.

## <a name="set-the-redirect-uri"></a>Festlegen des Umleitungs-URIs

Führen Sie die folgenden PowerShell-Befehle aus, um die URL der Web-App abzurufen und als Umleitungs-URI für die Authentifizierung (auch als Antwort-URL bezeichnet) festzulegen:

```powershell
$webApp = Get-AzWebApp -ResourceGroupName $resourceGroupName -Name $appName
$redirectUri = "https://" + $webApp.DefaultHostName + "/"
Get-AzureADApplication -All $true | where { $_.AppId -match $servicePrincipalCredentials.UserName } | Set-AzureADApplication -ReplyUrls $redirectUri
```

Nachdem Sie nun einen Umleitungs-URI hinzugefügt haben, müssen Sie als nächstes die API-URL aktualisieren, damit eine Interaktion des Verwaltungstools mit dem API-Back-End-Dienst möglich ist.

## <a name="update-the-api-url-for-the-web-application"></a>Aktualisieren der API-URL für die Webanwendung

Führen Sie das folgende Skript aus, um die API-URL-Konfiguration im Front-End der Webanwendung zu aktualisieren:

```powershell
.\updateWvdMgmtUxApiUrl.ps1 -AppName $appName -SubscriptionId $subscriptionId
```

Nachdem Sie die Web-App des Verwaltungstools vollständig konfiguriert haben, muss die Azure AD Anwendung überprüft und Einwilligung erteilt werden.

## <a name="verify-the-azure-ad-application-and-provide-consent"></a>Überprüfen der Azure AD Anwendung und Erteilen der Einwilligung

So überprüfen Sie die Konfiguration der Azure AD Anwendung und erteilen Sie die Einwilligung:

1. Öffnen Sie Ihren Internetbrowser, und melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit Ihrem Administratorkonto an.
2. Suchen Sie in der Suchleiste am oberen Rand des Azure-Portals nach **App-Registrierungen**, und wählen Sie das Element unter **Dienste** aus.
3. Wählen Sie **Alle Anwendungen** aus, und suchen Sie den eindeutigen App-Namen, den Sie für das PowerShell-Skript in [Erstellen einer Azure Active Directory-App-Registrierung](#create-an-azure-active-directory-app-registration) eingegeben haben.
4. Wählen Sie im Bereich auf der linken Seite des Browsers **Authentifizierung** aus, und stellen Sie sicher, dass der Umleitungs-URI mit der Web-App-URL für das Verwaltungstool übereinstimmt (siehe folgende Abbildung).

   [ ![Die Authentifizierungsseite mit dem eingegebenen Umleitungs-URI](../media/management-ui-redirect-uri-inline.png) ](../media/management-ui-redirect-uri-expanded.png#lightbox)

5. Wählen Sie im linken Bereich **API-Berechtigungen** aus, um zu bestätigen, dass Berechtigungen hinzugefügt wurden. Wählen Sie als globaler Administrator die Schaltfläche **Einwilligung des Administrators erteilen für `tenantname`** aus, und befolgen Sie die Anweisungen im Dialogfeld, um die Administratoreinwilligung für Ihre Organisation anzugeben.

    [ ![Die Seite „API-Berechtigungen“](../media/management-ui-permissions-inline.png) ](../media/management-ui-permissions-expanded.png#lightbox)

Sie können das Verwaltungstool jetzt verwenden.

## <a name="use-the-management-tool"></a>Verwenden des Verwaltungstools

Nachdem Sie das Verwaltungstool eingerichtet haben, können Sie es jederzeit und überall aufrufen. So rufen Sie das Tool auf:

1. Öffnen Sie die URL der Web-App in einem Webbrowser. Wenn Sie die URL vergessen haben, können Sie sich bei Azure anmelden, den App Dienst suchen, den Sie für das Verwaltungstool bereitgestellt haben, und dann die URL auswählen.
2. Melden Sie sich mit Ihren Windows Virtual Desktop-Anmeldeinformationen an.

   > [!NOTE]
   > Wenn Sie beim Konfigurieren des Verwaltungstools keine Administratoreinwilligung erteilt haben, muss jeder Benutzer, der sich anmeldet, eigens einwilligen, um das Tool zu verwenden.

3. Wählen Sie bei der Aufforderung zum Auswählen einer Mandantengruppe in der Dropdownliste den Eintrag **Standardmandantengruppe** aus.
4. Wenn Sie eine **Standardmandantengruppe** auswählen, sollte links im Fenster ein Menü angezeigt werden. Suchen Sie in diesem Menü nach dem Namen Ihrer Mandantengruppe, und wählen Sie sie aus.

   > [!NOTE]
   > Wenn Sie eine benutzerdefinierte Mandantengruppe festgelegt haben, geben Sie den Namen manuell ein, anstatt eine Auswahl in der Dropdownliste zu treffen.

## <a name="report-issues"></a>Melden von Problemen

Falls Probleme mit dem Verwaltungstool oder mit anderen Windows Virtual Desktop-Tools auftreten, sollten Sie die Anleitung unter [Azure Resource Manager-Vorlagen für Remotedesktopdienste](https://github.com/Azure/RDS-Templates/blob/master/README.md) befolgen, um diese auf GitHub zu melden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfahren, wie Sie das Verwaltungstool bereitstellen und eine Verbindung damit herstellen. Im nächsten Schritt erfahren Sie nun, wie Sie mithilfe von Azure Service Health Dienstprobleme und Integritätsempfehlungen überwachen. Weitere Informationen finden Sie in unserem [Tutorial zum Einrichten von Dienstwarnungen](set-up-service-alerts-2019.md).
