---
title: Aktivieren der Active Directory-Authentifizierung über SMB für Azure Files
description: Erfahren Sie, wie Sie identitätsbasierte Authentifizierung über SMB für Azure-Dateifreigaben über Active Directory aktivieren. Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) können dann mit AD-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: rogarana
ms.openlocfilehash: febb796a47b9f5e78906d513c115b62b35c7c7d5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196504"
---
# <a name="enable-on-premises-active-directory-domain-services-authentication-over-smb-for-azure-file-shares"></a>Aktivieren der lokalen Active Directory Domain Services-Authentifizierung über SMB für Azure-Dateifreigaben

[Azure Files](storage-files-introduction.md) unterstützt identitätsbasierte Authentifizierung über Server Message Block (SMB) durch zwei Typen von Domänendiensten: Azure Active Directory Domain Services (Azure AD DS) und lokale Active Directory Domain Services (AD DS) (Vorschau). Dieser Artikel konzentriert sich auf die neu eingeführte Unterstützung (Vorschau) der Nutzung von Active Directory Domain Services für die Authentifizierung bei Azure-Dateifreigaben. Wenn Sie Azure AD DS-Authentifizierung (GA) für Azure-Dateifreigaben aktivieren möchten, lesen Sie [unserem Artikel zu diesem Thema](storage-files-identity-auth-active-directory-domain-service-enable.md).

> [!NOTE]
> Azure-Dateifreigaben unterstützen nur die Authentifizierung bei einem Domänendienst, entweder bei Azure Active Directory Domain Services (Azure AD DS) oder einer lokalen Active Directory Domain Services-Instanz (AD DS). 
>
> AD DS-Identitäten, die für die Azure-Dateifreigabeauthentifizierung verwendet werden, müssen mit Azure AD synchronisiert sein. Kennworthashsynchronisierung ist optional. 
> 
> Die lokale AD DS-Authentifizierung unterstützt keine Authentifizierung für Computerkonten, die in AD DS erstellt werden. 
> 
> Die lokale AD DS-Authentifizierung kann nur für eine AD-Gesamtstruktur unterstützt werden, in der das Speicherkonto registriert ist. Sie können auf Azure-Dateifreigaben standardmäßig nur mit den AD DS-Anmeldeinformationen einer einzelnen Gesamtstruktur zugreifen. Wenn Sie von einer anderen Gesamtstruktur aus auf Ihre Azure-Dateifreigabe zugreifen müssen, stellen Sie sicher, dass Sie die richtige Gesamtstrukturvertrauensstellung konfiguriert haben. Weitere Informationen finden Sie unter [FAQ](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control).  
> 
> AD DS-Authentifizierung für SMB-Zugriff und persistente ACLs werden für Azure-Dateifreigaben unterstützt, die über die Azure-Dateisynchronisierung verwaltet werden.
>
> Azure Files unterstützt Kerberos-Authentifizierung mit AD mit RC4-HMAC-Verschlüsselung. AES-Kerberos-Verschlüsselung wird noch nicht unterstützt.

Wenn Sie AD DS für Azure-Dateifreigaben über SMB aktivieren, können Ihre in AD DS eingebundenen Computer Azure-Dateifreigaben mithilfe Ihrer vorhandenen AD-Anmeldeinformationen bereitstellen. Diese Funktion kann für eine AD DS-Umgebung aktiviert werden, die entweder mit lokalen Computern oder in Azure gehostet wird.

Die für den Zugriff auf Azure-Dateifreigaben verwendeten Identitäten müssen mit Azure AD synchronisiert werden, um über das Modell für die [rollenbasierte Zugriffssteuerung (RBAC)](../../role-based-access-control/overview.md) Dateiberechtigungen auf Freigabeebene zu erzwingen. [DACLs im Windows-Stil](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) für Dateien/Verzeichnisse, die von vorhandenen Dateiservern übertragen werden, werden beibehalten und erzwungen. Diese Funktion bietet nahtlose Integration in die AD DS-Umgebung Ihres Unternehmens. Wenn Sie lokale Dateiserver durch Azure-Dateifreigaben ersetzen, können vorhandene Benutzer ohne Änderungen an den verwendeten Anmeldeinformationen auf Azure-Dateifreigaben von ihren aktuellen SSO-Clients aus zugreifen.  

> [!NOTE]
> Um Sie beim Einrichten der Azure Files-AD-Authentifizierung für gängige Anwendungsfälle zu unterstützen, haben wir [zwei Videos](https://docs.microsoft.com/azure/storage/files/storage-files-introduction#videos) mit Schrittanleitungen zu diesen Themen veröffentlicht:
> - Ersetzen lokaler Dateiserver durch Azure Files (einschließlich Einrichtung eines privaten Links für Dateien und AD-Authentifizierung)
> - Verwenden von Azure Files als Profilcontainer für Windows Virtual Desktop (einschließlich Einrichtung der AD-Authentifizierung und FsLogix-Konfiguration)

## <a name="prerequisites"></a>Voraussetzungen 

Bevor Sie die AD DS-Authentifizierung für Azure-Dateifreigaben aktivieren, müssen Sie sicherstellen, dass die folgenden Voraussetzungen erfüllt sind: 

- Sie müssen Ihre AD DS-Umgebung auswählen und erstellen und [mit Azure AD synchronisieren](../../active-directory/hybrid/how-to-connect-install-roadmap.md). 

    Sie können das Feature für eine neue oder eine vorhandene AD DS-Umgebung aktivieren. Die für den Zugriff verwendeten Identitäten müssen mit Azure AD synchronisiert werden. Der Azure AD-Mandant und die Dateifreigabe, auf die Sie zugreifen möchten, müssen dem gleichen Abonnement zugeordnet sein. 

    Informationen zum Einrichten einer AD-Domänenumgebung finden Sie unter [Active Directory Domain Services: Übersicht](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview). Wenn Sie keine Synchronisierung Ihrer AD-Umgebung mit Azure AD durchgeführt haben, folgen Sie den Anweisungen unter [Installationsübersicht: Azure AD Connect und Azure AD Connect Health](../../active-directory/hybrid/how-to-connect-install-roadmap.md), um Azure AD Connect zu konfigurieren und einzurichten. 

- Binden Sie einen lokalen Computer oder eine Azure-VM in eine lokale AD DS-Domäne ein. 

    Damit Sie mit AD-Anmeldeinformationen von einem Computer oder einer VM aus auf eine Dateifreigabe zugreifen können, muss Ihr Gerät in eine AD DS-Domäne eingebunden sein. Weitere Informationen zum Domänenbeitritt finden Sie unter [Hinzufügen eines Computers zu einer Domäne](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- Wählen Sie ein Azure-Speicherkonto, oder erstellen Sie eines.  Für optimale Leistung wird empfohlen, das Speicherkonto in derselben Region bereitzustellen wie die VM, von der aus Sie auf die Freigabe zugreifen möchten.

    Stellen Sie sicher, dass das Speicherkonto mit den Dateifreigaben nicht bereits für Azure AD DS-Authentifizierung konfiguriert ist. Wenn die Azure Files-Azure AD DS-Authentifizierung für das Speicherkonto aktiviert ist, muss diese vor der Umstellung auf eine lokale AD DS-Authentifizierung deaktiviert werden. Dies bedeutet, dass vorhandene ACLs, die in der Azure AD DS-Umgebung konfiguriert sind, für die ordnungsgemäße Berechtigungserzwingung neu konfiguriert werden müssen.
    
    Weitere Informationen zum Erstellen einer neuen Dateifreigabe finden Sie unter  [Erstellen einer Dateifreigabe in Azure Files](storage-how-to-create-file-share.md).

- Überprüfen Sie die Konnektivität, indem Sie Azure-Dateifreigaben mit Ihrem Speicherkontoschlüssel einbinden. 

    Um sicherzustellen, dass Ihr Gerät und die Dateifreigabe ordnungsgemäß konfiguriert sind, versuchen Sie, die [Dateifreigabe mit Ihrem Speicherkontoschlüssel einzubinden](storage-how-to-use-files-windows.md). Wenn beim Herstellen einer Verbindung mit Azure Files Probleme auftreten, finden Sie weitere Informationen unter [dem Problembehandlungstool, das für Azure Files-Einbindungsfehler unter Windows veröffentlicht wurde](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5). Außerdem stehen [Anleitungen](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) zur Umgehung von Szenarien bereit, wenn Port 445 blockiert ist. 

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Die Azure Files-Authentifizierung mit AD DS (Vorschau) ist [in allen öffentlichen Regionen und Azure Gov-Regionen](https://azure.microsoft.com/global-infrastructure/locations/) verfügbar.

## <a name="overview"></a>Übersicht

Wenn Sie Netzwerkkonfigurationen für Ihre Dateifreigabe aktivieren möchten, lesen Sie den Abschnitt [Netzwerküberlegungen](https://docs.microsoft.com/azure/storage/files/storage-files-networking-overview), und nehmen Sie entsprechende Konfiguration vor, bevor Sie die AD DS-Authentifizierung aktivieren.

Wenn Sie die AD DS-Authentifizierung für Ihre Azure-Dateifreigaben aktivieren, können Sie sich mit Ihren lokalen AD DS-Anmeldeinformationen bei ihren Azure-Dateifreigaben authentifizieren. Außerdem ermöglicht es eine bessere Verwaltung der Berechtigungen, sodass Sie eine präzise Zugriffssteuerung erzielen. Hierzu müssen die Identitäten mithilfe von AD Connect aus den lokalen AD DS mit Azure AD synchronisiert werden. Sie steuern den Zugriff auf Freigabeebene mit Identitäten, die mit Azure AD synchronisiert werden, während Sie den Zugriff auf Datei-/Freigabebene mit lokalen AD DS-Anmeldeinformationen verwalten.

Führen Sie anschließend die folgenden Schritte aus, um Azure Files für die AD DS-Authentifizierung einzurichten: 

1. [Aktivieren Sie die Azure Files-AD DS-Authentifizierung für Ihr Speicherkonto.](#1-enable-ad-ds-authentication-for-your-account)

1. [Weisen Sie der Azure AD-Identität (Benutzer, Gruppe oder Dienstprinzipal), die mit der AD-Zielidentität synchronisiert wird, Zugriffsberechtigungen für eine Freigabe zu.](#2-assign-access-permissions-to-an-identity)

1. [Konfigurieren Sie ACLs über SMB für Verzeichnisse und Dateien.](#3-configure-ntfs-permissions-over-smb)
 
1. [Binden Sie eine Azure-Dateifreigabe für eine VM ein, die Mitglied Ihrer AD DS-Umgebung ist.](#4-mount-a-file-share-from-a-domain-joined-vm)

1. [Aktualisieren Sie das Kennwort für Ihre Speicherkontoidentität in AD DS.](#5-update-the-password-of-your-storage-account-identity-in-ad-ds)

Die folgende Abbildung zeigt den vollständigen Workflow zur Aktivierung der Azure AD-Authentifizierung über SMB für Azure-Dateifreigaben. 

![Files AD-Workflowdiagramm](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> Die AD DS-Authentifizierung über SMB für Azure-Dateifreigaben wird nur für Computer oder VMs unterstützt, auf denen neuere Betriebssystemversionen als Windows 7 oder Windows Server 2008 R2 ausgeführt werden. 

## <a name="1-enable-ad-ds-authentication-for-your-account"></a>1 Aktivieren der AD DS-Authentifizierung für Ihr Konto 

Um die AD DS-Authentifizierung über SMB für Azure-Dateifreigaben zu aktivieren, müssen Sie zunächst Ihr Speicherkonto bei AD DS registrieren und dann die erforderlichen Domäneneigenschaften für das Speicherkonto festlegen. Wenn das Feature für das Speicherkonto aktiviert ist, gilt es für alle neuen und vorhandenen Dateifreigaben im Konto. Laden Sie das PowerShell-Modul AzFilesHybrid herunter, und aktivieren Sie das Feature mit `join-AzStorageAccountForAuth`. Eine ausführliche Beschreibung des End-to-End-Workflows finden Sie im Skript in diesem Abschnitt. 

> [!IMPORTANT]
> Das Cmdlet `Join-AzStorageAccountForAuth` führt Änderungen an Ihrer AD-Umgebung aus. Lesen Sie die folgende Erklärung, um besser zu verstehen, was es bewirkt, um sicherzustellen, dass Sie über die richtigen Berechtigungen zur Ausführung des Befehls verfügen und dass die angewandten Änderungen mit den Compliance- und Sicherheitsrichtlinien übereinstimmen. 

Mit dem-Cmdlet `Join-AzStorageAccountForAuth` wird die Entsprechung eines Offlinedomänenbeitritts im Auftrag des angegebenen Speicherkontos durchgeführt. Das Skript verwendet das Cmdlet, um ein Konto in Ihrer AD-Domäne zu erstellen, entweder ein [Computerkonto](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (Standardwert) oder ein [Dienstanmeldekonto](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts). Wenn Sie diesen Vorgang manuell ausführen, sollten Sie das Konto auswählen, das am besten für Ihre Umgebung geeignet ist.

Das vom Cmdlet erstellte AD DS-Konto repräsentiert das Speicherkonto in der AD-Domäne. Wenn das AD DS-Konto unter einer AD-Organisationseinheit (OE) erstellt wird, die einen Kennwortablauf erzwingt, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters aktualisieren. Wenn Sie das Kontokennwort nicht aktualisieren, führt dies beim Zugriff auf Azure-Dateifreigaben zu Authentifizierungsfehlern. Weitere Informationen zum Aktualisieren des Kennworts finden Sie unter [Aktualisieren des Kennworts für das AD DS-Konto](#5-update-the-password-of-your-storage-account-identity-in-ad-ds).

Sie können das folgende Skript verwenden, um die Registrierung durchzuführen und das Feature zu aktivieren. Alternativ können Sie auch manuell die Vorgänge ausführen, die das Skript ausführen würde. Diese Vorgänge werden im Abschnitt beschrieben, der auf das Skript folgt. Sie müssen nicht beide Vorgänge ausführen.

### <a name="11-script-prerequisites"></a>1.1 Skriptvoraussetzungen
- [Herunterladen und Entpacken des Moduls AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases)
- Installieren Sie das Modul auf einem Gerät mit Domäneneinbindung in die lokale AD DS-Umgebung mithilfe von AD DS-Anmeldeinformationen, die zum Erstellen eines Dienstanmeldekontos oder eines Computerkontos in der Ziel-AD-Umgebungen berechtigt sind, und führen Sie das Modul dann aus.
-  Führen Sie das Skript mit lokalen AD DS-Anmeldeinformationen aus, die mit Ihrer Azure AD-Umgebung synchronisiert sind. Die lokalen AD DS-Anmeldeinformationen müssen über die Rolle „Speicherkontobesitzer“ oder „Mitwirkender RBAC“ verfügen.
- Stellen Sie sicher, dass sich Ihr Speicherkonto in einer [unterstützten Region](#regional-availability) befindet.

### <a name="12-domain-join-your-storage-account"></a>1.2 Domänenbeitritt Ihres Speicherkontos
Vergessen Sie nicht, die Platzhalterwerte in den Parametern unten durch ihre eigenen Werte zu ersetzen, bevor Sie das Skript in PowerShell ausführen.
> [!IMPORTANT]
> Mit dem Cmdlet für den Domänenbeitritt wird ein AD-Konto erstellt, das das Speicherkonto (die Dateifreigabe) in AD repräsentiert. Sie können sich wahlweise als Computerkonto oder Dienstanmeldekonto registrieren. Einzelheiten finden Sie unter [Häufig gestellte Fragen](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control). Für Computerkonten gilt eine Standardfrist für den Ablauf des Kennworts, die in AD auf 30 Tage festgelegt ist. Analog dazu kann für das Dienstanmeldekonto in der AD-Domäne oder Organisationseinheit (OU) eine Standardablauffrist für das Kennwort festgelegt sein.
> Für beide Kontotypen empfehlen wir nachdrücklich, das in Ihrer AD-Umgebung konfigurierte Kenntwortablaufalter zu überprüfen und eine [Aktualisierung des Kennworts Ihrer Speicherkontoidentität in AD](#5-update-the-password-of-your-storage-account-identity-in-ad-ds) für das nachstehende AD-Konto zu planen, bevor das maximale Kennwortalter erreicht ist. Sie können in Erwägung ziehen, [ eine neue AD-Organisationseinheit (OU) in AD](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit?view=win10-ps) zu erstellen und die Richtlinie zum Ablauf des Kennworts für [Computerkonten](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj852252(v=ws.11)?redirectedfrom=MSDN) oder Dienstanmeldekonten entsprechend zu deaktivieren. 

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
# You can choose to create the identity that represents the storage account as either a Service Logon Account or Computer Account (default parameter value), depends on the AD permission you have and preference. 
# You can run Get-Help Join-AzStorageAccountForAuth to find more details on this cmdlet.

Join-AzStorageAccountForAuth `
        -ResourceGroupName $ResourceGroupName `
        -Name $StorageAccountName `
        -DomainAccountType "<ComputerAccount|ServiceLogonAccount>" `
        -OrganizationalUnitName "<ou-name-here>" #You can also use -OrganizationalUnitDistinguishedName "<ou-distinguishedname-here>" instead. If you don't provide the OU name as an input parameter, the AD identity that represents the storage account will be created under the root directory.

#You can run the Debug-AzStorageAccountAuth cmdlet to conduct a set of basic checks on your AD configuration with the logged on AD user. This cmdlet is supported on AzFilesHybrid v0.1.2+ version. For more details on the checks performed in this cmdlet, go to Azure Files FAQ.
Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose

```

In der folgenden Beschreibung werden alle Aktionen zusammengefasst, die bei der Ausführung des Cmdlets `Join-AzStorageAccountForAuth` ausgeführt werden. Wenn Sie den Befehl nicht verwenden möchten, können Sie diese Schritte manuell ausführen:

> [!NOTE]
> Wenn Sie das oben beschriebene Skript `Join-AzStorageAccountForAuth` bereits erfolgreich ausgeführt haben, fahren Sie mit dem nächsten Abschnitt „1.3. Bestätigen, dass die Funktion aktiviert ist“ fort. Sie müssen die nachfolgenden Vorgänge nicht erneut ausführen.

#### <a name="a-checking-environment"></a>a. Überprüfen der Umgebung

Zuerst überprüft das Skript Ihre Umgebung. Insbesondere wird überprüft, ob [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) installiert ist und die Shell mit Administratorrechten ausgeführt wird. Dann wird überprüft, ob das [Modul Az.Storage 1.11.1-preview](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) installiert ist. Es wird installiert, wenn dies nicht der Fall ist. Wenn diese Überprüfungen erfolgreich sind, wird Ihre AD DS-Umgebung auf ein bereits vorhandenes [Computerkonto](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (Standardwert) oder ein [Dienstanmeldekonto](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) mit dem SPN/UPN „cifs/ihr-speicherkontoname.file.core.windows.net“ überprüft. Wenn das Konto nicht vorhanden ist, wird ein Konto wie in Abschnitt b weiter unten beschrieben erstellt.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. Manuelles Erstellen einer Identität, die das Speicherkonto in Ihrem AD darstellt

Um dieses Konto manuell zu erstellen, erstellen Sie mit `New-AzStorageAccountKey -KeyName kerb1` einen neuen Kerberos-Schlüssel für Ihr Speicherkonto. Verwenden Sie den Kerberos-Schlüssel dann als Kennwort für Ihr Konto. Dieser Schlüssel wird nur während der Einrichtung verwendet und kann nicht für Vorgänge auf Steuerungs- oder Datenebene für das Speicherkonto verwendet werden.

Sobald Sie über diesen Schlüssel verfügen, erstellen Sie unter Ihrer Organisationseinheit ein Dienst- oder ein Computerkonto. Verwenden Sie die folgende Spezifikation: SPN: „cifs/ihr-speicherkontoname.file.core.windows.net“ Kennwort: Kerberos-Schlüssel für Ihr Speicherkonto.

Wenn Ihre Organisationseinheit den Ablauf von Kennwörtern erzwingt, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters aktualisieren, um Authentifizierungsfehler beim Zugriff auf Azure-Dateifreigaben zu verhindern. Einzelheiten finden Sie unter [Aktualisieren des Kennworts Ihrer Speicherkontoidentität in AD DS](#5-update-the-password-of-your-storage-account-identity-in-ad-ds).

Notieren Sie sich die SID für das neu erstellte Konto. Sie benötigen sie im nächsten Schritt. Die soeben erstellte Identität, die das Speicherkonto repräsentiert, muss nicht mit Azure AD synchronisiert werden.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Aktivieren des Features für Ihr Speicherkonto

Das Skript aktiviert dann das Feature für Ihr Speicherkonto. Um dieses Setup manuell auszuführen, geben Sie einige Konfigurationsdetails für die Domäneneigenschaften im folgenden Befehl ein, und führen Sie ihn dann aus. Die im folgenden Befehl erforderliche SID für das Speicherkonto ist die SID der Identität, die Sie im [vorherigen Abschnitt](#b-creating-an-identity-representing-the-storage-account-in-your-ad-manually) in Ihrer AD DS-Umgebung erstellt haben.

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


### <a name="13-confirm-that-the-feature-is-enabled"></a>1.3 Bestätigen, dass die Funktion aktiviert ist

Sie können überprüfen, ob die Funktion für Ihr Speicherkonto aktiviert ist, indem Sie das folgende Skript verwenden:

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

Sie haben das Feature nun für Ihr Speicherkonto erfolgreich aktiviert. Nun, da das Feature aktiviert ist, müssen weitere Schritte zur Verwendung des Features ausgeführt werden.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Sie haben die AD DS-Authentifizierung über SMB erfolgreich aktiviert und eine benutzerdefinierte Rolle zugewiesen, die den Zugriff auf eine Azure-Dateifreigabe mit einer AD DS-Identität ermöglicht. Befolgen Sie die Anweisungen in den Abschnitten [Zuweisen von Zugriffsberechtigungen zum Verwenden einer Identität](#2-assign-access-permissions-to-an-identity) und [Konfigurieren von NTFS-Berechtigungen über SMB](#3-configure-ntfs-permissions-over-smb), um weiteren Benutzern den Zugriff auf Ihre Dateifreigabe zu gewähren.

## <a name="5-update-the-password-of-your-storage-account-identity-in-ad-ds"></a>5 Aktualisieren des Kennworts für Ihre Speicherkontoidentität in AD DS

Wenn Sie die AD DS-Identität bzw. das Konto, das Ihr Speicherkonto repräsentiert, für eine Organisationseinheit mit Erzwingung des Kennwortablaufs registriert haben, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters rotieren. Wenn Sie das Kennwort des AD DS-Kontos nicht aktualisieren, führt dies beim Zugriff auf Azure-Dateifreigaben zu Authentifizierungsfehlern.  

Um die Kennwortrotation zu initiieren, können Sie den Befehl `Update-AzStorageAccountADObjectPassword` aus dem Modul AzFilesHybrid ausführen. Das Cmdlet führt Aktionen aus, die der Speicherkonto-Schlüsselrotation ähneln. Es ruft den zweiten Kerberos-Schlüssel des Speicherkontos ab und verwendet ihn zum Aktualisieren des Kennworts für das registrierte Konto in AD DS. Anschließend generiert es den Kerberos-Zielschlüssel des Speicherkontos neu und aktualisiert das Kennwort für das registrierte Konto in AD DS. Sie müssen dieses Cmdlet in einer lokalen AD DS-Umgebung mit Domäneneinbindung ausführen.

```PowerShell
# Update the password of the AD DS account registered for the storage account
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Files und zur Verwendung von AD über SMB finden Sie in den folgenden Ressourcen:

- [ Übersicht über die Unterstützung der identitätsbasierten Authentifizierung mit Azure Files für SMB-Zugriff](storage-files-active-directory-overview.md)
- [Häufig gestellte Fragen](storage-files-faq.md)
