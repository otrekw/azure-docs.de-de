---
title: Verwalten von Office 365-Diensten mit Azure Automation
description: Erfahren Sie, wie Sie Azure Automation zum Verwalten von Office 365-Abonnementdiensten verwenden.
services: automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 9cb505ced907b143fbd6a5f4f30c818092005bb8
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80576769"
---
# <a name="manage-office-365-services-using-azure-automation"></a>Verwalten von Office 365-Diensten mit Azure Automation

Sie können Azure Automation für die Verwaltung von Office 365-Abonnementdiensten und für Produkte wie Microsoft Word und Microsoft Outlook verwenden. Interaktionen mit Office 365 werden durch [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) ermöglicht. Siehe [Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure](automation-use-azure-ad.md).

>[!NOTE]
>Dieser Artikel wurde aktualisiert und beinhaltet jetzt das neue Az-Modul von Azure PowerShell. Sie können das AzureRM-Modul weiterhin verwenden, das bis mindestens Dezember 2020 weiterhin Fehlerbehebungen erhält. Weitere Informationen zum neuen Az-Modul und zur Kompatibilität mit AzureRM finden Sie unter [Introducing the new Azure PowerShell Az module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0) (Einführung in das neue Az-Modul von Azure PowerShell). Installationsanweisungen für das Az-Modul auf Ihrem Hybrid Runbook Worker finden Sie unter [Installieren des Azure PowerShell-Moduls](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). In Ihrem Automation-Konto können Sie die Module mithilfe der Informationen unter [Aktualisieren von Azure PowerShell-Modulen in Azure Automation](automation-update-azure-modules.md) auf die neueste Version aktualisieren.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes zum Verwalten von Office 365-Abonnementdiensten in Azure Automation.

* Ein Azure-Abonnement. Siehe [Leitfaden zur Entscheidungsfindung für Abonnements](https://docs.microsoft.com/azure/cloud-adoption-framework/decision-guides/subscriptions/).
* Ein Automation-Objekt in Azure, das die Anmeldeinformationen des Benutzerkontos und Runbooks enthält. Siehe [Einführung in Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro).
* Azure AD. Siehe [Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure](automation-use-azure-ad.md).
* Einen Office 365-Mandanten mit einem Konto. Siehe [Einrichten Ihres Office 365-Mandanten](https://docs.microsoft.com/sharepoint/dev/spfx/set-up-your-developer-tenant).

## <a name="installing-the-msonline-and-msonlineext-modules"></a>Installieren der Module MSOnline und MSOnlineExt

Die Verwendung von Office 365 in Azure Automation erfordert Microsoft Azure Active Directory für Windows PowerShell (`MSOnline`-Modul). Außerdem benötigen Sie das Modul [`MSOnlineExt`](https://www.powershellgallery.com/packages/MSOnlineExt/1.0.35), das die Azure AD-Verwaltung in Umgebungen mit einem und mehreren Mandanten vereinfacht. Installieren Sie das Modul wie unter [Verwenden von Azure AD in Azure Automation für die Authentifizierung bei Azure](automation-use-azure-ad.md) beschrieben.

>[!NOTE]
>Um MSOnline PowerShell verwenden zu können, müssen Sie Mitglied von Azure AD sein. Gastbenutzer können das Modul nicht verwenden.

## <a name="creating-an-azure-automation-account"></a>Erstellen eines Azure Automation-Kontos

Für die Durchführung der in diesem Artikel aufgeführten Schritte benötigen Sie ein Konto in Azure Automation. Siehe [Erstellen eines Azure Automation-Kontos](automation-quickstart-create-account.md).
 
## <a name="adding-msonline-and-msonlineext-as-assets"></a>Hinzufügen von MSOnline und MSOnlineExt als Objekte

Fügen Sie nun die installierten Module MSOnline und MSOnlineExt hinzu, um die Funktionalität von Office 365 zu aktivieren. Informationen finden Sie unter [Verwalten von Modulen in Azure Automation](shared-resources/modules.md).

1. Wählen Sie im Azure-Portal **Automation-Konten** aus.
2. Wählen Sie Ihr Automation-Konto aus.
3. Wählen Sie unter **Freigegebene Ressourcen** die Option **Modulkatalog** aus.
4. Suchen Sie nach MSOnline.
5. Wählen Sie das PowerShell-Modul `MSOnline` aus, und klicken Sie auf **Importieren**, um das Modul als Objekt zu importieren.
6. Wiederholen Sie die Schritte 4 und 5, um das `MSOnlineExt`-Modul zu suchen und zu importieren. 

## <a name="creating-a-credential-asset-optional"></a>Erstellen eines Anmeldeinformationsobjekts (optional)

Es ist optional, ein Anmeldeinformationsobjekt für den Administratorbenutzer von Office 365 zu erstellen, der über Berechtigungen zum Ausführen Ihres Skripts verfügt. Dies kann jedoch hilfreich sein, um die Offenlegung von Benutzernamen und Kennwörtern innerhalb von PowerShell-Skripts zu verhindern. Anleitungen finden Sie unter [Erstellen eines Anmeldeinformationsobjekts](automation-use-azure-ad.md#creating-a-credential-asset).

## <a name="creating-an-office-365-service-account"></a>Erstellen eines Office 365-Dienstkontos

Um Office 365-Abonnementdienste ausführen zu können, benötigen Sie ein Office 365-Dienstkonto mit den für die Aktionen notwendigen Berechtigungen, die Sie ausführen möchten. Sie können ein globales Administratorkonto, dabei ein Konto pro Dienst, verwenden oder über eine Funktion oder ein Skript zum Ausführen verfügen. In jedem Fall benötigt das Dienstkonto ein komplexes und sicheres Kennwort. Siehe [Einrichten von Office 365 Business](https://docs.microsoft.com/microsoft-365/admin/setup/setup?view=o365-worldwide). 

## <a name="connecting-to-the-azure-ad-online-service"></a>Herstellen einer Verbindung mit dem Azure AD-Onlinedienst

>[!NOTE]
>Um die Cmdlets des MSOnline-Moduls verwenden zu können, müssen Sie sie über die Windows PowerShell ausführen. PowerShell Core unterstützt diese Cmdlets nicht.

Sie können das MSOnline-Modul verwenden, um eine Verbindung mit Azure AD aus dem Office 365-Abonnement herzustellen. Die Verbindung verwendet einen Benutzernamen und ein Kennwort von Office 365, oder sie verwendet mehrstufige Authentifizierung (Multi-Factor Authentication, MFA). Sie können eine Verbindung über das Azure-Portal oder eine Windows PowerShell-Eingabeaufforderung herstellen (benötigt keine erhöhten Rechte).

Unten sehen Sie ein PowerShell-Beispiel. Das [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7)-Cmdlet fordert zur Eingabe von Anmeldeinformationen auf und speichert diese in der Variablen `Msolcred`. Anschließend verwendet das [Connect-MsolService](https://docs.microsoft.com/powershell/module/msonline/connect-msolservice?view=azureadps-1.0)-Cmdlet die Anmeldeinformationen, um eine Verbindung mit dem Azure Active Directory-Onlinedienst herzustellen. Wenn Sie eine Verbindung mit einer bestimmten Azure-Umgebung herstellen möchten, verwenden Sie den `AzureEnvironment`-Parameter.

```powershell
$Msolcred = Get-Credential
Connect-MsolService -Credential $MsolCred -AzureEnvironment "AzureCloud"
```

Wenn Sie keine Fehler erhalten, haben Sie erfolgreich eine Verbindung hergestellt. Ein schneller Test besteht darin, ein Office 365-Cmdlet auszuführen, z. B. `Get-MsolUser`, und die Ergebnisse anzuzeigen. Wenn Sie Fehler erhalten, beachten Sie, dass ein häufiges Problem ein falsches Kennwort ist.

>[!NOTE]
>Sie können auch das AzureRM-Modul oder das Az-Modul verwenden, um eine Verbindung mit Azure AD aus dem Office 365-Abonnement herzustellen. Das Hauptverbindungs-Cmdlet ist [Connect-AzureAD](https://docs.microsoft.com/powershell/module/azuread/connect-azuread?view=azureadps-2.0). Dieses Cmdlet unterstützt den `AzureEnvironmentName`-Parameter für bestimmte Office 365-Umgebungen.

## <a name="creating-a-powershell-runbook-from-an-existing-script"></a>Erstellen eines PowerShell-Runbooks aus einem vorhandenen Skript

Auf Office 365-Funktionen greifen Sie über ein PowerShell-Skript zu. Im Folgenden finden Sie ein Beispiel für ein Skript für ein Anmeldeinformationsobjekt namens `Office-Credentials` mit dem Benutzernamen `admin@TenantOne.com`. Es verwendet `Get-AutomationPSCredential`, um das Office 365-Anmeldeinformationsobjekt zu importieren.

```powershell
$emailFromAddress = "admin@TenantOne.com" 
$emailToAddress = "servicedesk@TenantOne.com" 
$emailSMTPServer = "outlook.office365.com" 
$emailSubject = "Office 365 License Report" 

$credObject = Get-AutomationPSCredential -Name "Office-Credentials" 
Connect-MsolService -Credential $credObject

$O365Licenses = Get-MsolAccountSku | Out-String 
Send-MailMessage -Credential $credObject -From $emailFromAddress -To $emailToAddress -Subject $emailSubject -Body 

$O365Licenses -SmtpServer $emailSMTPServer -UseSSL
```

## <a name="running-the-script-in-a-runbook"></a>Ausführen des Skripts in einem Runbook

Sie können Ihr Skript in einem Azure Automation-Runbook verwenden. Zu Demonstrationszwecken verwenden wir den PowerShell-Runbooktyp.

1. Erstellen Sie ein neues PowerShell-Runbook. Informationen finden Sie und [Erstellen eines Azure Automation-Runbooks](https://docs.microsoft.com/azure/automation/automation-quickstart-create-runbook).
2. Wählen Sie in Ihrem Automation-Konto unter **Prozessautomatisierung** die Option **Runbooks** aus.
3. Wählen Sie das neue Runbook aus, und klicken Sie auf **Bearbeiten**.
4. Kopieren Sie Ihr Skript, und fügen Sie es in den Text-Editor für das Runbook ein.
5. Wählen Sie **OBJEKTE** aus, erweitern Sie dann **Anmeldeinformationen**, und überprüfen Sie, ob das Office 365-Anmeldeinformationsobjekt vorhanden ist.
6. Klicken Sie auf **Speichern**.
7. Wählen Sie **Testbereich** aus, und klicken Sie dann auf **Starten**, um das Runbook zu testen. Siehe [Verwalten von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/manage-runbooks).
8. Wenn die Tests fertig sind, beenden Sie den Testbereich.

## <a name="publishing-and-scheduling-the-runbook"></a>Veröffentlichen und Planen des Runbooks

Informationen zum Veröffentlichen und anschließenden Planen Ihres Runbooks finden Sie unter [Verwalten von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/manage-runbooks).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Automation-Anmeldeinformationsobjekten finden Sie unter [Anmeldeinformationsobjekte in Azure Automation](shared-resources/credentials.md).
* Informationen zum Arbeiten mit Automation-Modulen finden Sie unter [Verwalten von Modulen in Azure Automation](shared-resources/modules.md).
* Eine Übersicht der Runbookverwaltung finden Sie unter [Verwalten von Runbooks in Azure Automation](https://docs.microsoft.com/azure/automation/manage-runbooks).
* Weitere Informationen zu den Methoden zum Starten eines Runbooks in Azure Automation finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).
* Weitere Informationen zur PowerShell, einschließlich einer Sprachreferenz und Lernmodulen, finden Sie in der [PowerShell-Dokumentation](https://docs.microsoft.com/powershell/scripting/overview).