---
title: Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure
description: Erfahren Sie, wie Sie Azure AD in Azure Automation als Anbieter für die Authentifizierung bei Azure verwenden.
services: automation
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 90338a1ffa79e6c2347832cb2e74633db02ec72d
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548337"
---
# <a name="use-azure-ad-in-azure-automation-to-authenticate-to-azure"></a>Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure

Der [Azure Active Directory (AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)-Dienst ermöglicht eine Reihe von Verwaltungsaufgaben wie Benutzerverwaltung, Domänenverwaltung und Konfiguration des einmaligen Anmeldens. In diesem Artikel wird beschrieben, wie Sie Azure AD in Azure Automation als Anbieter für die Authentifizierung bei Azure verwenden. 

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="installing-azure-ad-modules"></a>Installieren von Azure AD-Modulen

Sie können Azure AD mithilfe der folgenden PowerShell-Module aktivieren:

* Azure Active Directory PowerShell für Graph (AzureRM und AZ-Module). Azure Automation enthält im Lieferumfang das AzureRM-Modul und sein jüngstes Upgrade, das Az-Modul. Die Funktionalität umfasst die nicht interaktive Authentifizierung bei Azure mithilfe der Azure auf den Anmeldeinformationen des AD-Benutzers (OrgId) basierenden Authentifizierung. Siehe [Azure AD 2.0.2.76](https://www.powershellgallery.com/packages/AzureAD/2.0.2.76).

* Microsoft Azure Active Directory für Windows PowerShell (MSOnline-Modul). Dieses Modul ermöglicht Interaktionen mit Microsoft Online, einschließlich Office 365.

>[!NOTE]
>PowerShell Core unterstützt das MSOnline-Modul nicht. Um die Module-Cmdlets verwenden zu können, müssen Sie sie über Windows PowerShell ausführen. Es wird empfohlen, dass Sie die neuere Azure Active Directory PowerShell für Graph-Module anstelle des MSOnline-Moduls verwenden. 

### <a name="preinstallation"></a>Vorinstallation

Vor dem Installieren der Azure AD-Module auf Ihrem Computer:

* Deinstallieren Sie alle früheren Versionen des AzureRM/Az-Moduls und des MSOnline-Moduls. 

* Deinstallieren Sie den Microsoft Online Services-Anmelde-Assistenten, um den ordnungsgemäßen Betrieb der neuen PowerShell-Module sicherzustellen.  

### <a name="install-the-azurerm-and-az-modules"></a>Installieren der AzureRM- und Az-Module

>[!NOTE]
>Um mit diesen Modulen arbeiten zu können, müssen Sie PowerShell, Version 5.1 oder höher, mit einer 64-Bit-Version von Windows verwenden. 

1. Installieren Sie Windows Management Framework 5.1 (WMF). Siehe [Installieren und Konfigurieren von WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure?view=powershell-7).

2. Verwenden Sie zum Installieren von AzureRM und/oder Az die Anweisungen unter [Installieren von Azure PowerShell unter Windows mit PowerShellGet](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-6.13.0).

### <a name="install-the-msonline-module"></a>Installieren des MSOnline-Moduls

>[!NOTE]
>Um das MSOnline-Modul zu installieren, müssen Sie Mitglied einer Office 365-Administratorrolle sein. Siehe [Informationen zu Administratorrollen](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles?view=o365-worldwide).

1. Stellen Sie sicher, dass die Microsoft .NET Framework 3.5.x-Funktion auf Ihrem Computer aktiviert ist. Es ist wahrscheinlich, dass auf Ihrem Computer eine neuere Version installiert ist, aber die Abwärtskompatibilität mit älteren Versionen des .NET Frameworks kann aktiviert oder deaktiviert werden. 

2. Installieren Sie die 64-Bit-Version des [Microsoft Online Services-Anmelde-Assistenten](https://www.microsoft.com/download/details.aspx?id=41950).

3. Führen Sie Windows PowerShell als Administrator aus, um eine Windows PowerShell-Eingabeaufforderung mit erhöhten Rechten zu erstellen.

4. Stellen Sie Azure Active Directory aus [MSOnline 1.0](http://www.powershellgallery.com/packages/MSOnline/1.0) bereit.

5. Wenn Sie zum Installieren des NuGet-Anbieters aufgefordert werden, wählen Sie „Y“ aus, und drücken Sie die EINGABETASTE.

6. Wenn Sie zum Installieren des Moduls aus der [PSgallery](https://www.powershellgallery.com/) aufgefordert werden, geben Sie „Y“ ein, und drücken Sie die EINGABETASTE.

### <a name="install-support-for-pscredential"></a>Installieren der Unterstützung für „PSCredential“"

Azure Automation verwendet die [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?view=pscore-6.2.0)-Klasse, um ein Anmeldeinformationsobjekt darzustellen. Ihr Skripts rufen `PSCredential`-Objekte mithilfe des `Get-AutomationPSCredential`-Cmdlets ab. Weitere Informationen finden Sie unter [Anmeldeinformationsobjekte in Azure Automation](shared-resources/credentials.md).

## <a name="assigning-a-subscription-administrator"></a>Zuweisen eines Abonnementadministrators

Sie müssen einen Administrator für das Azure-Abonnement zuweisen. Diese Person verfügt über die Rolle „Besitzer“" für den Abonnementbereich. Siehe [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md). 

## <a name="changing-the-azure-ad-users-password"></a>Ändern des Kennworts des Azure AD-Benutzers

So ändern Sie das Kennwort des Azure AD-Benutzers

1. Melden Sie sich bei Azure ab.

2. Lassen Sie den Administrator sich bei Azure als der gerade erstellte Azure AD-Benutzer anmelden, wobei der vollständige Benutzername (einschließlich Domäne) und ein temporäres Kennwort verwendet werden. 

3. Bitten Sie den Administrator, das Kennwort bei Aufforderung dazu zu ändern.

## <a name="configuring-azure-automation-to-use-the-azure-ad-user-to-manage-the-azure-subscription"></a>Konfigurieren von Azure Automation für die Verwendung des Azure AD Benutzers zum Verwalten des Azure-Abonnements

Damit Azure Automation mit Azure AD kommunizieren kann, müssen Sie die Anmeldeinformationen abrufen, die der Azure-Verbindung mit Azure AD zugeordnet sind. Beispiele für diese Anmeldeinformationen sind Mandanten-ID, Abonnement-ID und Ähnliches. Weitere Informationen zu der Verbindung zwischen Azure und Azure AD finden Sie unter [Verbinden Ihrer Organisation mit Azure Active Directory](https://docs.microsoft.com/azure/devops/organizations/accounts/connect-organization-to-azure-ad?view=azure-devops).

## <a name="creating-a-credential-asset"></a>Erstellen eines Anmeldeinformationsobjekts

Da die Azure-Anmeldeinformationen für Azure AD verfügbar sind, ist es jetzt an der Zeit, ein Azure Automation-Anmeldeinformationsobjekt zu erstellen, um die Azure AD-Anmeldeinformationen sicher zu speichern, damit Runbooks und DSC-Skripts (Desire State Configuration) darauf zugreifen können. Dies können Sie mithilfe des Azure-Portals oder mit PowerShell-Cmdlets durchführen.

### <a name="create-the-credential-asset-in-azure-portal"></a>Erstellen des Anmeldeinformationsobjekts im Azure-Portal

Sie können das Azure-Portal zum Erstellen des Anmeldeinformationsobjekts verwenden. Führen Sie diesen Vorgang in Ihrem Automation-Konto unter **Freigegebene Ressourcen** mithilfe der Option **Anmeldeinformationen** aus. Siehe [Anmeldeinformationsobjekte in Azure Automation](shared-resources/credentials.md).

### <a name="create-the-credential-asset-with-windows-powershell"></a>Erstellen des Anmeldeinformationsobjekts mithilfe von Windows PowerShell

Zum Vorbereiten eines neuen Anmeldeinformationsobjekts in Windows PowerShell erstellt Ihr Skript zunächst ein `PSCredential`-Objekt mithilfe des zugewiesenen Benutzernamens und Kennworts. Das Skript verwendet dann dieses Objekt, um das Objekt durch einen Aufruf des Cmdlets [New-AzureAutomationCredential](https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azureautomationcredential?view=azuresmps-4.0.0) zu erstellen. Alternativ kann das Skript das Cmdlet [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7) aufrufen, um den Benutzer zur Eingabe eines Namens und Kennworts aufzufordern. Siehe [Anmeldeinformationsobjekte in Azure Automation](shared-resources/credentials.md). 

## <a name="managing-azure-resources-from-an-azure-automation-runbook"></a>Verwalten von Azure-Ressourcen aus einem Azure Automation-Runbook

Sie können Azure-Ressourcen unter Verwendung des Anmeldeinformationsobjekts aus Azure Automation-Runbooks heraus verwalten. Im Folgenden finden Sie ein PowerShell-Beispielrunbook, das Anmeldeinformationsobjekt erfasst, um es zum Beenden und Starten virtueller Computer in einem Azure-Abonnement zu verwenden. Dieses Runbook verwendet zunächst `Get-AutomationPSCredential`, um die Anmeldeinformationen abzurufen, die für die Authentifizierung bei Azure verwendet werden sollen. Danach ruft es das Cmdlet [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.6.1) auf, um eine Verbindung mit Azure mit diesen Anmeldeinformationen herzustellen. Das Skript verwendet das Cmdlet [Select-AzureSubscription](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0), um das Abonnement auszuwählen, mit dem Sie arbeiten möchten. 

```azurepowershell
Workflow Stop-Start-AzureVM 
{ 
    Param 
    (    
        [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()] 
        [String] 
        $AzureSubscriptionId, 
        [Parameter(Mandatory=$true)][ValidateNotNullOrEmpty()] 
        [String] 
        $AzureVMList="All", 
        [Parameter(Mandatory=$true)][ValidateSet("Start","Stop")] 
        [String] 
        $Action 
    ) 
     
    $credential = Get-AutomationPSCredential -Name 'AzureCredential' 
    Connect-AzAccount -Credential $credential 
    Select-AzureSubscription -SubscriptionId $AzureSubscriptionId 
 
    if($AzureVMList -ne "All") 
    { 
        $AzureVMs = $AzureVMList.Split(",") 
        [System.Collections.ArrayList]$AzureVMsToHandle = $AzureVMs 
    } 
    else 
    { 
        $AzureVMs = (Get-AzVM).Name 
        [System.Collections.ArrayList]$AzureVMsToHandle = $AzureVMs 
 
    } 
 
    foreach($AzureVM in $AzureVMsToHandle) 
    { 
        if(!(Get-AzVM | ? {$_.Name -eq $AzureVM})) 
        { 
            throw " AzureVM : [$AzureVM] - Does not exist! - Check your inputs " 
        } 
    } 
 
    if($Action -eq "Stop") 
    { 
        Write-Output "Stopping VMs"; 
        foreach -parallel ($AzureVM in $AzureVMsToHandle) 
        { 
            Get-AzVM | ? {$_.Name -eq $AzureVM} | Stop-AzVM -Force 
        } 
    } 
    else 
    { 
        Write-Output "Starting VMs"; 
        foreach -parallel ($AzureVM in $AzureVMsToHandle) 
        { 
            Get-AzVM | ? {$_.Name -eq $AzureVM} | Start-AzVM 
        } 
    } 
}
```  

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Automation-Anmeldeinformationsobjekten finden Sie unter [Anmeldeinformationsobjekte in Azure Automation](shared-resources/credentials.md).
* Informationen zum Arbeiten mit Automation-Modulen finden Sie unter [Verwalten von Modulen in Azure Automation](shared-resources/modules.md).
* Weitere Informationen zu den Methoden zum Starten eines Runbooks in Azure Automation finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).
* Weitere Informationen zur PowerShell, einschließlich einer Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).