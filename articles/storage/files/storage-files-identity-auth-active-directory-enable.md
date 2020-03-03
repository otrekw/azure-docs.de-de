---
title: Aktivieren der Active Directory-Authentifizierung über SMB für Azure Files
description: Erfahren Sie, wie Sie identitätsbasierte Authentifizierung über SMB für Azure-Dateifreigaben über Active Directory aktivieren. Ihre in die Domäne eingebundenen virtuellen Windows-Computer (VMs) können dann mit AD-Anmeldeinformationen auf Azure-Dateifreigaben zugreifen.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 02/23/2020
ms.author: rogarana
ms.openlocfilehash: 9bda3331bc27f387bd05ea0fcd29e7badf6d6a02
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651082"
---
# <a name="enable-active-directory-authentication-over-smb-for-azure-file-shares"></a>Aktivieren der Active Directory-Authentifizierung über SMB für Azure-Dateifreigaben

[Azure Files](storage-files-introduction.md) unterstützt identitätsbasierte Authentifizierung über Server Message Block (SMB) durch zwei Typen von Domänendiensten: Azure Active Directory Domain Services (Azure AD DS) (GA) und Active Directory (AD) (Vorschau). Dieser Artikel konzentriert sich auf die neu eingeführte Unterstützung (Vorschau) der Nutzung von Active Directory Domain Services für die Authentifizierung bei Azure-Dateifreigaben. Wenn Sie Azure AD DS-Authentifizierung (GA) für Azure-Dateifreigaben aktivieren möchten, lesen Sie [unserem Artikel zu diesem Thema](storage-files-identity-auth-active-directory-domain-service-enable.md).

> [!NOTE]
> Azure-Dateifreigaben unterstützen nur die Authentifizierung bei einem Domänendienst, entweder Azure Active Directory Domain Services (Azure AD DS) oder Active Directory (AD). 
>
> AD-Identitäten, die für Azure-Dateifreigabeauthentifizierung verwendet werden, müssen mit Azure AD synchronisiert sein. Kennworthashsynchronisierung ist optional. 
> 
> AD-Authentifizierung unterstützt nicht die Authentifizierung für Computerkonten, die in AD erstellt werden. 
> 
> AD-Authentifizierung kann nur für eine AD-Gesamtstruktur unterstützt werden, in der das Speicherkonto registriert ist. Sie können auf Azure-Dateifreigaben standardmäßig nur mit den AD-Anmeldeinformationen aus einer einzelnen AD-Gesamtstruktur zugreifen. Wenn Sie von einer anderen Gesamtstruktur aus auf Ihre Azure-Dateifreigabe zugreifen müssen, stellen Sie sicher, dass Sie die richtige Gesamtstrukturvertrauensstellung konfiguriert haben. Weitere Informationen finden Sie unter [FAQ](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control).  
> 
> AD-Authentifizierung für SMB-Zugriff und persistente ACLs werden für Azure-Dateifreigaben, die von der Azure-Dateisynchronisierung verwaltet werden, nicht unterstützt.
>
> Azure Files unterstützt Kerberos-Authentifizierung mit AD mit RC4-HMAC-Verschlüsselung. AES-Kerberos-Verschlüsselung wird noch nicht unterstützt.

Wenn Sie AD für Azure-Dateifreigaben über SMB aktivieren, können Ihre in die AD-Domäne eingebundenen Computer Azure-Dateifreigaben mithilfe Ihrer vorhandenen AD-Anmeldeinformationen einbinden. Diese Funktion kann mit einer AD-Umgebung aktiviert werden, die auf lokalen Computern oder in Azure gehostet wird.

AD-Identitäten, die für den Zugriff auf Azure-Dateifreigaben verwendet werden, müssen mit Azure AD synchronisiert werden, um Dateiberechtigungen auf Freigabeebene über das Standardmodell für [rollenbasierte Zugriffssteuerung (RBAC)](../../role-based-access-control/overview.md) zu erzwingen. [DACLs im Windows-Stil](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) für Dateien/Verzeichnisse, die von vorhandenen Dateiservern übertragen werden, werden beibehalten und erzwungen. Diese Funktion bietet nahtlose Integration in die AD-Domäneninfrastruktur Ihres Unternehmens. Wenn Sie lokale Dateiserver durch Azure-Dateifreigaben ersetzen, können vorhandene Benutzer ohne Änderungen an den verwendeten Anmeldeinformationen auf Azure-Dateifreigaben von ihren aktuellen SSO-Clients aus zugreifen.  
 
## <a name="prerequisites"></a>Voraussetzungen 

Bevor Sie AD-Authentifizierung für Azure-Dateifreigaben aktivieren, sollten Sie sicherstellen, dass Sie die folgenden Voraussetzungen erfüllt haben: 

- Auswählen oder erstellen Ihrer AD-Umgebung, und synchronisieren der Umgebung mit Azure AD. 

    Sie können das Feature für eine neue oder vorhandene AD-Umgebung aktivieren. Die für den Zugriff verwendeten Identitäten müssen mit Azure AD synchronisiert werden. Der Azure AD-Mandant und die Dateifreigabe, auf die Sie zugreifen möchten, müssen dem gleichen Abonnement zugeordnet sein. 

    Informationen zum Einrichten einer AD-Domänenumgebung finden Sie unter [Active Directory Domain Services: Übersicht](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview). Wenn Sie Ihr AD nicht mit Ihrem Azure AD synchronisiert haben, befolgen Sie die Anweisungen unter [Was ist Hybrididentität mit Azure Active Directory?](../../active-directory/hybrid/whatis-hybrid-identity.md), um Ihre bevorzugte Authentifizierungsmethode und die Azure AD Connect-Setupoption zu bestimmen. 

- Domänenbeitritt: Verknüpfen Sie einen lokalen Computer oder einen virtuellen Azure-Computer mit AD (auch als AD DS bezeichnet). 

    Damit Sie mit AD-Anmeldeinformationen von einem Computer oder einer VM auf eine Dateifreigabe zugreifen können, muss Ihr Gerät in AD in eine Domäne eingebunden sein. Weitere Informationen zum Domänenbeitritt zu AD finden Sie unter [Domänenbeitritt eines Computers](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- Wählen Sie ein Azure-Speicherkonto in [einer unterstützten Region](#regional-availability) aus, oder erstellen Sie ein Konto. 

    Stellen Sie sicher, dass das Speicherkonto mit den Dateifreigaben nicht bereits für Azure AD DS-Authentifizierung konfiguriert ist. Wenn Azure Files Azure AD DS-Authentifizierung für das Speicherkonto aktiviert ist, muss eine Deaktivierung erfolgen, bevor Sie zur Verwendung von AD wechseln. Dies bedeutet, dass vorhandene ACLs, die in der Azure AD DS-Umgebung konfiguriert sind, für die ordnungsgemäße Berechtigungserzwingung neu konfiguriert werden müssen.
    
    Weitere Informationen zum Erstellen einer neuen Dateifreigabe finden Sie unter  [Erstellen einer Dateifreigabe in Azure Files](storage-how-to-create-file-share.md).
    
    Für optimale Leistung wird empfohlen, das Speicherkonto in derselben Region bereitzustellen wie die VM, von der aus Sie auf die Freigabe zugreifen möchten. 

- Überprüfen Sie die Konnektivität, indem Sie Azure-Dateifreigaben mit Ihrem Speicherkontoschlüssel einbinden. 

    Um sicherzustellen, dass Ihr Gerät und die Dateifreigabe ordnungsgemäß konfiguriert sind, versuchen Sie, die Dateifreigabe mit Ihrem Speicherkontoschlüssel einzubinden. Weitere Informationen finden Sie unter  [Verwenden einer Azure-Dateifreigabe mit Windows](storage-how-to-use-files-windows.md).

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Azure Files AD-Authentifizierung ist in den [meisten öffentlichen Regionen](https://azure.microsoft.com/global-infrastructure/regions/) verfügbar. Die Regionen, in denen dies noch nicht der Fall ist, lauten:

- USA (Westen)
- USA, Westen 2
- East US
- USA (Ost) 2
- USA Süd Mitte
- Europa, Westen
- Nordeuropa

## <a name="workflow-overview"></a>Übersicht über Workflow

Bevor Sie AD-Authentifizierung über SMB für Azure-Dateifreigaben aktivieren, empfiehlt es sich, die [Voraussetzungen](#prerequisites) zu durchlaufen und sicherzustellen, dass Sie alle Schritte abgeschlossen haben. Die Voraussetzungen überprüfen, ob Ihre AD-, Azure AD- und Azure Storage-Umgebungen ordnungsgemäß konfiguriert sind. 

Gewähren Sie dann Zugriff auf Azure Files-Ressourcen mit AD-Anmeldeinformationen: 

- Aktivieren Sie Azure Files AD-Authentifizierung für Ihr Speicherkonto.  

- Weisen Sie der Azure AD-Identität (Benutzer, Gruppe oder Dienstprinzipal). die mit der AD-Zielidentität synchronisiert wird, Zugriffsberechtigungen für eine Freigabe zu. 

- Konfigurieren Sie ACLs über SMB für Verzeichnisse und Dateien. 

- Binden Sie eine Azure-Dateifreigabe von einer domänengebundenen AD-VM ein. 

Die folgende Abbildung zeigt den vollständigen Workflow zur Aktivierung der Azure AD-Authentifizierung über SMB für Azure-Dateifreigaben. 

![Files AD-Workflowdiagramm](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> AD-Authentifizierung über SMB für Azure-Dateifreigaben wird nur auf Computern oder VMs unterstützt, die unter neueren Betriebssystemversionen als Windows 7 oder Windows Server 2008 R2 ausgeführt werden. 

## <a name="enable-ad-authentication-for-your-account"></a>Aktivieren der AD-Authentifizierung für Ihr Konto 

Um AD-Authentifizierung über SMB für Azure-Dateifreigaben zu aktivieren, müssen Sie zunächst Ihr Speicherkonto bei AD registrieren und dann die erforderlichen Domäneneigenschaften für das Speicherkonto festlegen. Wenn das Feature für das Speicherkonto aktiviert ist, gilt es für alle neuen und vorhandenen Dateifreigaben im Konto. Verwenden Sie `join-AzStorageAccountForAuth`, um das Feature zu aktivieren. Eine ausführliche Beschreibung des End-to-End-Workflows finden Sie im Abschnitt weiter unten. 

> [!IMPORTANT]
> Das Cmdlet `join-AzStorageAccountForAuth` führt Änderungen an Ihrer AD-Umgebung aus. Lesen Sie die folgende Erklärung, um besser zu verstehen, was es bewirkt, um sicherzustellen, dass Sie über die richtigen Berechtigungen zur Ausführung des Befehls verfügen und dass die angewandten Änderungen mit den Compliance- und Sicherheitsrichtlinien übereinstimmen. 

Mit dem-Cmdlet `join-AzStorageAccountForAuth` wird die Entsprechung eines Offlinedomänenbeitritts im Auftrag des angegebenen Speicherkontos durchgeführt. Es wird ein Konto in Ihrer AD-Domäne erstellt, entweder ein [Computerkonto](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) oder ein [Dienstanmeldekonto](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts). Das erstellte AD-Konto stellt das Speicherkonto in der AD-Domäne dar. Wenn das AD-Konto unter einer AD-Organisationseinheit (OU) erstellt wird, die den Ablauf des Kennworts erzwingt, müssen Sie das Kennwort vor dem Erreichen des maximalen Kennwortalters aktualisieren. Wenn Sie das Kennwort des AD-Kontos nicht aktualisieren, führt dies beim Zugriff auf Azure-Dateifreigaben zu Authentifizierungsfehlern. Weitere Informationen zum Aktualisieren des Kennworts finden Sie unter [Aktualisieren des Kennworts für das AD-Konto](#update-ad-account-password).

Sie können das folgende Skript verwenden, um die Registrierung durchzuführen und das Feature zu aktivieren. Alternativ können Sie auch manuell die Vorgänge ausführen, die das Skript ausführen würde. Diese Vorgänge werden im Abschnitt beschrieben, der auf das Skript folgt. Sie müssen nicht beide Vorgänge ausführen.

### <a name="1-check-prerequisites"></a>1. Überprüfen der Voraussetzungen
- [Herunterladen und Entpacken des Moduls AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases)
- Installieren Sie das Modul auf einem Gerät, das über einen Domänenbeitritt mit AD verknüpft ist und über AD-Anmeldeinformationen verfügt, die zum Erstellen eines Dienstanmeldekontos oder eines Computerkontos im Ziel-AD berechtigt sind, und führen Sie das Modul dann aus.
-  Führen Sie das Skript mit AD-Anmeldeinformationen aus, die mit Ihrem Azure AD synchronisiert sind. Die AD-Anmeldeinformationen müssen über die Rolle „Speicherkontobesitzer“ oder „Mitwirkender RBAC“ verfügen.
- Stellen Sie sicher, dass sich Ihr Speicherkonto in einer [unterstützten Region](#regional-availability) befindet.

### <a name="2-execute-ad-enablement-script"></a>2. Ausführen des AD-Aktivierungsskripts
Vergessen Sie nicht, die Platzhalterwerte in den Parametern unten durch ihre eigenen Werte zu ersetzen, bevor Sie das Skript in PowerShell ausführen.

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Currentuser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId "<your-subscription-id-here>"

#Register the target storage account with your active directory environment under the target OU
join-AzStorageAccountForAuth -ResourceGroupName "<resource-group-name-here>" -Name "<storage-account-name-here>" -DomainAccountType "<ServiceLogonAccount|ComputerAccount>" -OrganizationUnitName "<ou-name-here>"
```

In der folgenden Beschreibung werden alle Aktionen zusammengefasst, die bei der Ausführung des Cmdlets `join-AzStorageAccountForAuth` ausgeführt werden. Wenn Sie den Befehl nicht verwenden möchten, können Sie diese Schritte manuell ausführen:

> [!NOTE]
> Wenn Sie das oben beschriebene Skript join-AzStorageAccountForAuth bereits erfolgreich ausgeführt haben, fahren Sie mit dem nächsten Abschnitt „3. Bestätigen, dass die Funktion aktiviert ist“ fort. Sie müssen die nachfolgenden Vorgänge nicht erneut ausführen.

#### <a name="a-checking-environment"></a>a. Überprüfen der Umgebung

Zuerst wird Ihre Umgebung überprüft. Insbesondere wird überprüft, ob [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) installiert ist und ob die Shell mit Administratorrechten ausgeführt wird. Dann wird überprüft, ob das [Modul Az.Storage 1.11.1-preview](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) installiert ist. Es wird installiert, wenn dies nicht der Fall ist. Wenn diese Überprüfungen bestanden werden, wird in Ihrem AD überprüft, ob ein [Computerkonto](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (Standard) oder [Dienstanmeldekonto](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) vorhanden ist, das bereits mit SPN/UPN als „cifs/ihr-speicherkontoname.file.core.windows.net“ erstellt wurde. Wenn das Konto nicht vorhanden ist, wird ein Konto wie in Abschnitt b weiter unten beschrieben erstellt.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. Manuelles Erstellen einer Identität, die das Speicherkonto in Ihrem AD darstellt

Um dieses Konto manuell zu erstellen, erstellen Sie mit `New-AzStorageAccountKey -KeyName kerb1` einen neuen Kerberos-Schlüssel für Ihr Speicherkonto. Verwenden Sie den Kerberos-Schlüssel dann als Kennwort für Ihr Konto. Dieser Schlüssel wird nur während der Einrichtung verwendet und kann nicht für Vorgänge auf Steuerungs- oder Datenebene für das Speicherkonto verwendet werden.

Sobald Sie über diesen Schlüssel verfügen, erstellen Sie unter Ihrer Organisationseinheit ein Dienst- oder ein Computerkonto. Verwenden Sie die folgende Spezifikation: SPN: „cifs/ihr-speicherkontoname.file.core.windows.net“ Kennwort: Kerberos-Schlüssel für Ihr Speicherkonto.

Wenn Ihre Organisationseinheit den Ablauf von Kennwörtern erzwingt, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters aktualisieren, um Authentifizierungsfehler beim Zugriff auf Azure-Dateifreigaben zu verhindern. Weitere Einzelheiten finden Sie unter [Aktualisieren des Kennworts für das AD-Konto](#update-ad-account-password).

Notieren Sie sich die SID für das neu erstellte Konto. Sie benötigen sie im nächsten Schritt.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Aktivieren des Features für Ihr Speicherkonto

Das Skript aktiviert dann das Feature für Ihr Speicherkonto. Um dieses Setup manuell auszuführen, geben Sie einige Konfigurationsdetails für die Domäneneigenschaften im folgenden Befehl ein, und führen Sie ihn dann aus. Die im folgenden Befehl erforderliche SID für das Speicherkonto ist die SID der Identität, die Sie in AD (Abschnitt b oben) erstellt haben.

```PowerShell
#Set the feature flag on the target storage account and provide the required AD domain information

Set-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>" -EnableActiveDirectoryDomainServiesForFile $true -ActiveDirectoryDomainName "<your-domain-name-here>" -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" -ActiveDirectoryForestName "<your-forest-name-here>" -ActiveDirectoryDomainGuid "<your-guid-here>" -ActiveDirectoryDomainsid "<your-domain-sid-here>" -ActiveDirectoryAzureStorageSid "<your-storage-account-sid>"
```


### <a name="3-confirm-that-the-feature-is-enabled"></a>3. Bestätigen, dass die Funktion aktiviert ist

Sie können überprüfen, ob die Funktion für Ihr Speicherkonto aktiviert ist, indem Sie das folgende Skript verwenden:

```PowerShell
#Get the target storage account
$storageaccount = Get-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>"

#List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

#List the directory domain information if the storage account has enabled AD authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

Sie haben das Feature nun für Ihr Speicherkonto erfolgreich aktiviert. Obwohl das Feature aktiviert ist, müssen Sie noch weitere Aktionen ausführen, um die Funktion ordnungsgemäß verwenden zu können.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Sie haben jetzt erfolgreich AD-Authentifizierung über SMB aktiviert und eine benutzerdefinierte Rolle zugewiesen, die den Zugriff auf eine Azure-Dateifreigabe mit einer AD-Identität ermöglicht. Befolgen Sie die Anweisungen in den Abschnitten [Zuweisen von Zugriffsberechtigungen zum Verwenden einer Identität](#assign-access-permissions-to-an-identity) und [Konfigurieren von NTFS-Berechtigungen über SMB](#configure-ntfs-permissions-over-smb), um weiteren Benutzern den Zugriff auf Ihre Dateifreigabe zu gewähren.

## <a name="update-ad-account-password"></a>Aktualisieren des AD-Kontokennworts

Wenn Sie die AD-Identität bzw. das Konto, das Ihr Speicherkonto darstellt, unter einer Organisationseinheit registriert haben, die eine Ablaufzeit des Kennworts erzwingt, müssen Sie das Kennwort vor Erreichen des maximalen Kennwortalters rotieren. Wenn Sie das Kennwort des AD-Kontos nicht aktualisieren, führt dies beim Zugriff auf Azure-Dateifreigaben zu Authentifizierungsfehlern.  

Um die Kennwortrotation zu initiieren, können Sie den Befehl `Update-AzStorageAccountADObjectPassword` aus dem Modul AzFilesHybrid ausführen. Das Cmdlet führt Aktionen aus, die der Speicherkonto-Schlüsselrotation ähneln. Es ruft den zweiten Kerberos-Schlüssel des Speicherkontos ab und verwendet ihn zum Aktualisieren des Kennworts für das registrierte Konto in AD. Es generiert dann den Kerberos-Zielschlüssel des Speicherkontos neu und aktualisiert das Kennwort für das registrierte Konto in AD. Sie müssen dieses Cmdlet in einer mit einer AD-Domäne verbundenen Umgebung ausführen.

```PowerShell
#Update the password of the AD account registered for the storage account
Update-AzStorageAccountADObjectPassword -RotateToKerbKey kerb2 -ResourceGroupName "your-resource-group-name-here" -StorageAccountName "your-storage-account-name-here"
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Files und zur Verwendung von AD über SMB finden Sie in den folgenden Ressourcen:

- [ Übersicht über die Unterstützung der identitätsbasierten Authentifizierung mit Azure Files für SMB-Zugriff](storage-files-active-directory-overview.md)
- [Häufig gestellte Fragen](storage-files-faq.md)
