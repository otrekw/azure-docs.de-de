---
title: Aktivieren der AD DS-Authentifizierung für Dateifreigaben in Azure
description: In diesem Artikel erhalten Sie Informationen zum Aktivieren der Active Directory Domain Services-Authentifizierung über SMB für Azure-Dateifreigaben. Ihre in die Domäne eingebundenen virtuellen Windows-Computer können dann mit AD DS-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 05/29/2020
ms.author: rogarana
ms.openlocfilehash: 5592a3c53a57e9cd96468bfca187e02faef28b05
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2020
ms.locfileid: "84268398"
---
# <a name="part-one-enable-ad-ds-authentication-for-your-azure-file-shares"></a>Teil 1: Aktivieren der AD DS-Authentifizierung für Ihre Dateifreigaben in Azure 

Bevor Sie die Active Directory Domain Services-Authentifizierung (AD DS) aktivieren, sollten Sie den [Übersichtsartikel](storage-files-identity-auth-active-directory-enable.md) gelesen haben, um die unterstützten Szenarios und Anforderungen zu kennen.

In diesem Artikel wird der Prozess erläutert, der für das Aktivieren der Active Directory Domain Services-Authentifizierung (AD DS) für Ihr Speicherkonto erforderlich ist. Nachdem Sie das Feature aktiviert haben, müssen Sie Ihr Speicherkonto und Ihre AD DS-Instanz konfigurieren, damit die AD DS-Anmeldeinformationen für die Authentifizierung bei Ihrer Dateifreigabe in Azure verwendet werden. Sie müssen Ihr Speicherkonto bei AD DS registrieren und dann die erforderlichen Domäneneigenschaften für das Speicherkonto festlegen, um die AD DS-Authentifizierung über SMB für Azure-Dateifreigaben zu aktivieren. Wenn das Feature für das Speicherkonto aktiviert ist, gilt es für alle neuen und vorhandenen Dateifreigaben im Konto.

## <a name="option-one-recommended-use-the-script"></a>Option 1 (empfohlen): Verwenden des Skripts

Das Skript in diesem Artikel nimmt die erforderlichen Änderungen vor und aktiviert das Feature für Sie. Da Teile des Skripts mit Ihrer lokalen AD DS-Instanz interagieren, wird hier erläutert, was das Skript genau tut, damit Sie bestimmen können, ob die Änderungen mit Ihren Compliance- und Sicherheitsrichtlinien konform sind, und dafür sorgen können, dass Sie über die erforderlichen Berechtigungen verfügen, um das Skript auszuführen. Es wird zwar empfohlen, das Skript zu verwenden, wenn Sie dies jedoch nicht können, finden Sie hier die Schritte, die für eine manuelle Ausführung erforderlich sind.

### <a name="script-prerequisites"></a>Skriptvoraussetzungen

- [Herunterladen und Entpacken des Moduls AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases)
- Installieren Sie das Modul auf einem Gerät mit Domäneneinbindung in die lokale AD DS-Umgebung mithilfe von AD DS-Anmeldeinformationen, die zum Erstellen eines Dienstanmeldekontos oder eines Computerkontos in der Ziel-AD-Umgebungen berechtigt sind, und führen Sie das Modul dann aus.
-  Führen Sie das Skript mit lokalen AD DS-Anmeldeinformationen aus, die mit Ihrer Azure AD-Umgebung synchronisiert sind. Die lokalen AD DS-Anmeldeinformationen müssen über die Rolle „Speicherkontobesitzer“ oder „Mitwirkender RBAC“ verfügen.

### <a name="offline-domain-join"></a>Offlinedomänenbeitritt

Mit dem Cmdlet `Join-AzStorageAccountForAuth` wird die Entsprechung eines Offlinedomänenbeitritts im Auftrag des angegebenen Speicherkontos durchgeführt. Das Skript verwendet das Cmdlet, um ein Konto in Ihrer AD-Domäne zu erstellen, entweder ein [Computerkonto](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (Standardwert) oder ein [Dienstanmeldekonto](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts). Wenn Sie diesen Befehl manuell ausführen möchten, sollten Sie das Konto auswählen, das am besten für Ihre Umgebung geeignet ist.

Das vom Cmdlet erstellte AD DS-Konto repräsentiert das Speicherkonto. Wenn das AD DS-Konto unter einer AD-Organisationseinheit (OE) erstellt wird, die einen Kennwortablauf erzwingt, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters aktualisieren. Wenn Sie das Kontokennwort vor diesem Gate nicht aktualisieren, führt dies beim Zugriff auf Azure-Dateifreigaben zu Authentifizierungsfehlern. Weitere Informationen zum Aktualisieren des Kennworts finden Sie unter [Aktualisieren des Kennworts für das AD DS-Konto](storage-files-identity-ad-ds-update-password.md).

### <a name="use-the-script-to-enable-ad-ds-authentication"></a>Verwenden des Skripts zum Aktivieren der AD DS-Authentifizierung

Vergessen Sie nicht, die Platzhalterwerte in den Parametern unten durch ihre eigenen Werte zu ersetzen, bevor Sie das Skript in PowerShell ausführen.
> [!IMPORTANT]
> Mit dem Cmdlet für den Domänenbeitritt wird ein AD-Konto erstellt, das das Speicherkonto (die Dateifreigabe) in AD repräsentiert. Sie können sich wahlweise als Computerkonto oder Dienstanmeldekonto registrieren. Einzelheiten finden Sie unter [Häufig gestellte Fragen](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control). Für Computerkonten gilt eine Standardfrist für den Ablauf des Kennworts, die in AD auf 30 Tage festgelegt ist. Analog dazu kann für das Dienstanmeldekonto in der AD-Domäne oder Organisationseinheit (OU) eine Standardablauffrist für das Kennwort festgelegt sein.
> Für beide Kontotypen wir empfohlen, das in Ihrer AD-Umgebung konfigurierte Kenntwortablaufalter zu überprüfen und eine [Aktualisierung des Kennworts Ihrer Speicherkontoidentität](storage-files-identity-ad-ds-update-password.md) für das AD-Konto zu planen, bevor das maximale Kennwortalter erreicht ist. Sie können in Erwägung ziehen, [eine neue AD-Organisationseinheit (OE) in AD](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit?view=win10-ps) zu erstellen und die Richtlinie zum Ablauf des Kennworts für [Computerkonten](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj852252(v=ws.11)?redirectedfrom=MSDN) oder Dienstanmeldekonten entsprechend zu deaktivieren. 

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Define parameters
$SubscriptionId = "<your-subscription-id-here>"
$ResourceGroupName = "<resource-group-name-here>"
$StorageAccountName = "<storage-account-name-here>"

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId $SubscriptionId 

# Register the target storage account with your active directory environment under the target OU (for example: specify the OU with Name as "UserAccounts" or DistinguishedName as "OU=UserAccounts,DC=CONTOSO,DC=COM"). 
# You can use to this PowerShell cmdlet: Get-ADOrganizationalUnit to find the Name and DistinguishedName of your target OU. If you are using the OU Name, specify it with -OrganizationalUnitName as shown below. If you are using the OU DistinguishedName, you can set it with -OrganizationalUnitDistinguishedName. You can choose to provide one of the two names to specify the target OU.
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account, depends on the AD permission you have and preference. 
# Run Get-Help Join-AzStorageAccountForAuth for more details on this cmdlet.

Join-AzStorageAccountForAuth `
        -ResourceGroupName $ResourceGroupName `
        -Name $StorageAccountName `
        -DomainAccountType "<ComputerAccount|ServiceLogonAccount>" ` #Default set to "ComputerAccount" if parameter is omitted
        -OrganizationalUnitName "<ou-name-here>" #You can also use -OrganizationalUnitDistinguishedName "<ou-distinguishedname-here>" instead. If you don't provide the OU name as an input parameter, the AD identity that represents the storage account will be created under the root directory.

#You can run the Debug-AzStorageAccountAuth cmdlet to conduct a set of basic checks on your AD configuration with the logged on AD user. This cmdlet is supported on AzFilesHybrid v0.1.2+ version. For more details on the checks performed in this cmdlet, see Azure Files Windows troubleshooting guide.
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose
```

## <a name="option-2-manually-perform-the-script-actions"></a>Option 2: Manuelle Ausführung der Skriptaktionen

Wenn Sie das oben beschriebene Skript `Join-AzStorageAccountForAuth` bereits erfolgreich ausgeführt haben, fahren Sie mit dem nächsten Abschnitt [Überprüfen der Aktivierung des Features](#confirm-the-feature-is-enabled) fort. Die folgenden manuellen Schritte müssen Sie nicht ausführen.

### <a name="checking-environment"></a>Überprüfen der Umgebung

Sie müssen als ersten Schritt den Zustand der Umgebung überprüfen. Insbesondere müssen Sie überprüfen, ob [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) installiert ist und die Shell mit Administratorrechten ausgeführt wird. Überprüfen Sie dann, ob das [Modul „Az.Storage 2.0“](https://www.powershellgallery.com/packages/Az.Storage/2.0.0) installiert ist. Ist dies nicht der Fall, installieren Sie das Modul. Wenn Sie diese beiden Überprüfungen durchgeführt haben, überprüfen Sie, ob Ihre AD DS-Umgebung ein bereits vorhandenes [Computerkonto](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (Standardwert) oder ein [Dienstanmeldekonto](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) mit dem SPN/UPN „cifs/ihr-speicherkontoname.file.core.windows.net“ aufweist. Wenn das Konto nicht vorhanden ist, erstellen Sie wie im folgenden Abschnitt beschrieben ein Konto.

### <a name="creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>Manuelles Erstellen einer Identität, die das Speicherkonto in Ihrem AD darstellt

Um dieses Konto manuell zu erstellen, erstellen Sie mit `New-AzStorageAccountKey -KeyName kerb1` einen neuen Kerberos-Schlüssel für Ihr Speicherkonto. Verwenden Sie den Kerberos-Schlüssel dann als Kennwort für Ihr Konto. Dieser Schlüssel wird nur während der Einrichtung verwendet und kann nicht für Vorgänge auf Steuerungs- oder Datenebene für das Speicherkonto verwendet werden. Sobald Sie über diesen Schlüssel verfügen, erstellen Sie unter Ihrer Organisationseinheit ein Dienst- oder ein Computerkonto. Verwenden Sie die folgende Spezifikation, und denken Sie dabei daran, den Beispieltext durch den Namen Ihres Speicherkontos zu ersetzen:

SPN: „cifs/ihr-speicherkontoname.file.core.windows.net“ Kennwort: Kerberos-Schlüssel für Ihr Speicherkonto.

Wenn Ihre Organisationseinheit den Ablauf von Kennwörtern erzwingt, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters aktualisieren, um Authentifizierungsfehler beim Zugriff auf Azure-Dateifreigaben zu verhindern. Weitere Informationen finden Sie unter [Aktualisieren des Kennworts für Ihre Speicherkontoidentität in Active Directory](storage-files-identity-ad-ds-update-password.md).

Notieren Sie sich die SID für die neu erstellte Identität. Sie benötigen sie im nächsten Schritt. Die soeben erstellte Identität, die das Speicherkonto repräsentiert, muss nicht mit Azure AD synchronisiert werden.

### <a name="enable-the-feature-on-your-storage-account"></a>Aktivieren des Features für Ihr Speicherkonto

Sie können nun das Feature für Ihr Speicherkonto aktivieren. Geben Sie einige Konfigurationsdetails für die Domäneneigenschaften im folgenden Befehl ein, und führen Sie ihn dann aus. Die im folgenden Befehl erforderliche SID für das Speicherkonto ist die SID der Identität, die Sie im [vorherigen Abschnitt](#creating-an-identity-representing-the-storage-account-in-your-ad-manually) in Ihrer AD DS-Umgebung erstellt haben.

```PowerShell
# Set the feature flag on the target storage account and provide the required AD domain information
Set-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>" `
        -EnableActiveDirectoryDomainServicesForFile $true `
        -ActiveDirectoryDomainName "<your-domain-name-here>" `
        -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" `
        -ActiveDirectoryForestName "<your-forest-name-here>" `
        -ActiveDirectoryDomainGuid "<your-guid-here>" `
        -ActiveDirectoryDomainsid "<your-domain-sid-here>" `
        -ActiveDirectoryAzureStorageSid "<your-storage-account-sid>"
```

### <a name="debugging"></a>Debuggen

Sie können das Cmdlet Debug-AzStorageAccountAuth ausführen, um eine Reihe grundlegender Überprüfungen Ihrer AD-Konfiguration mit dem angemeldeten AD-Benutzer durchzuführen. Dieses Cmdlet wird von der AzFilesHybrid-Version 0.1.2 und höheren Versionen unterstützt. Weitere Informationen zu den in diesem Cmdlet ausgeführten Überprüfungen finden Sie im Troubleshootingleitfaden für Windows unter [Azure Files mit AD-Anmeldeinformationen kann nicht eingebunden werden](storage-troubleshoot-windows-file-connection-problems.md#unable-to-mount-azure-files-with-ad-credentials).

```PowerShell
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose
```

## <a name="confirm-the-feature-is-enabled"></a>Überprüfen der Aktivierung des Features

Sie können überprüfen, ob das Feature für Ihr Speicherkonto aktiviert ist, indem Sie das folgende Skript verwenden:

```PowerShell
# Get the target storage account
$storageaccount = Get-AzStorageAccount `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -Name "<your-storage-account-name-here>"

# List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

# List the directory domain information if the storage account has enabled AD DS authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben das Feature nun für Ihr Speicherkonto erfolgreich aktiviert. Zur Verwendung des Features sind eine Konfiguration sowie weitere Änderungen erforderlich. Sie können nun mit folgendem Artikel fortfahren:

[Teil 2: Zuweisen von Berechtigungen auf Freigabeebene für eine Identität](storage-files-identity-ad-ds-assign-permissions.md)