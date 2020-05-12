---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/11/2019
ms.author: rogara
ms.custom: include file
ms.openlocfilehash: 5fc106bfd97e8decd47ac7d43383907dcbbbda9c
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792977"
---
## <a name="2-assign-access-permissions-to-an-identity"></a>2 Zuweisen von Zugriffsberechtigungen zu einer Identität

Für den Zugriff auf Azure Files-Ressourcen mit identitätsbasierter Authentifizierung muss eine Identität (Benutzer, Gruppe oder Dienstprinzipal) über die erforderlichen Berechtigungen auf der Freigabeebene verfügen. Dieser Prozess ähnelt der Angabe von Windows-Freigabeberechtigungen, bei denen Sie die Art des Zugriffs eines bestimmten Benutzers auf eine Dateifreigabe angeben. Die Anleitung in diesem Abschnitt zeigt, wie Sie einer Identität Lese-, Schreib- oder Löschberechtigungen für eine Dateifreigabe zuweisen. 

Wir haben drei integrierte Azure-Rollen zum Gewähren von Berechtigungen auf der Freigabeebene für Benutzer eingeführt:

- **Speicherdateidaten-SMB-Freigabeleser** ermöglicht den Lesezugriff auf Azure Storage-Dateifreigaben über SMB.
- **Speicherdateidaten-SMB-Freigabemitwirkender** ermöglicht den Lese-, Schreib- und Löschzugriff auf Azure Storage-Dateifreigaben über SMB.
- **Speicherdateidaten-SMB-Freigabemitwirkender mit erhöhten Rechten** gewährt Lese-, Schreib-, Lösch- und Änderungs-NTFS-Berechtigungen auf Azure Storage-Dateifreigaben über SMB.

> [!IMPORTANT]
> Die vollständige administrative Kontrolle über eine Dateifreigabe, einschließlich der Möglichkeit, den Besitz einer Rolle zu übernehmen, erfordert die Verwendung des Speicherkontenschlüssels. Die administrative Kontrolle wird mit Azure AD-Anmeldeinformationen nicht unterstützt.

Sie können das Azure-Portal, PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um die integrierten Rollen der Azure AD-Identität eines Benutzers zuzuweisen und damit Berechtigungen auf Freigabeebene zu erteilen. Beachten Sie, dass die RBAC-Rollenzuweisung auf Freigabeebene einige Zeit in Anspruch nehmen kann. 

> [!NOTE]
> Denken Sie daran, [Ihre AD DS-Anmeldeinformationen mit Azure AD zu synchronisieren](../articles/active-directory/hybrid/how-to-connect-install-roadmap.md), wenn Sie Ihr lokales AD DS für die Authentifizierung verwenden möchten. Die Kennworthashsynchronisierung von AD DS mit Azure AD ist optional. Die Berechtigung auf Freigabeebene wird der von Ihrem lokalen AD DS synchronisierten Azure AD-Identität erteilt.

Im Allgemeinen wird empfohlen, Berechtigungen auf Freigabeebene für die Verwaltung des Zugriffs auf hoher Ebene für eine aus Benutzern und Identitäten bestehende AD-Gruppe zu verwenden und dann NTFS-Berechtigungen für eine differenzierte Zugriffssteuerung auf der Verzeichnis-/Dateiebene zu nutzen. 

#### <a name="azure-portal"></a>Azure-Portal
Um einer Azure AD-Identität eine RBAC-Rolle zuzuweisen, führen Sie im [Azure-Portal](https://portal.azure.com) die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrer Dateifreigabe, oder [erstellen Sie eine Dateifreigabe](../articles/storage/files/storage-how-to-create-file-share.md).
2. Wählen Sie **Access Control (IAM)** aus.
3. Auswählen von **Rollenzuweisung hinzufügen**
4. Wählen Sie auf dem Blatt **Rollenzuweisung hinzufügen** in der Liste **Rolle** die entsprechende integrierte Rolle aus (Speicherdateidaten-SMB-Freigabeleser, Speicherdateidaten-SMB-Freigabemitwirkender). Behalten Sie für **Zugriff zuweisen zu** die Standardeinstellung bei: **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal**. Wählen Sie die Azure AD-Zielidentität anhand des Namens oder der E-Mail-Adresse aus.
5. Wählen Sie abschließend **Speichern** aus, um den Vorgang der Rollenzuweisung abzuschließen.

#### <a name="powershell"></a>PowerShell

Das folgende PowerShell-Beispiel zeigt, wie einer Azure AD-Identität basierend auf dem Anmeldenamen eine RBAC-Rolle zugewiesen wird. Weitere Informationen zum Zuweisen von RBAC-Rollen mit PowerShell finden Sie unter [Verwalten des Zugriffs mit RBAC und Azure PowerShell](../articles/role-based-access-control/role-assignments-powershell.md).

Denken Sie vor dem Ausführen des folgenden Beispielskripts daran, Platzhalterwerte durch eigene Werte zu ersetzen (einschließlich der Klammern).

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
  
Der folgende CLI 2.0-Befehl zeigt, wie einer Azure AD-Identität basierend auf dem Anmeldenamen eine RBAC-Rolle zugewiesen wird. Weitere Informationen zum Zuweisen von RBAC-Rollen mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Verwalten des Zugriffs mit RBAC und der Azure-Befehlszeilenschnittstelle](../articles/role-based-access-control/role-assignments-cli.md). 

Denken Sie vor dem Ausführen des folgenden Beispielskripts daran, Platzhalterwerte durch eigene Werte zu ersetzen (einschließlich der Klammern).

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="3-configure-ntfs-permissions-over-smb"></a>3 Konfigurieren von NTFS-Berechtigungen über SMB 
Nachdem Sie mit RBAC Berechtigungen auf Freigabeebene zugewiesen haben, müssen Sie die richtigen NTFS-Berechtigungen auf Stamm-, Verzeichnis- oder Dateiebene zuweisen. Stellen Sie sich die Berechtigungen auf der Freigabeebene als allgemeinen Gatekeeper vor, der festlegt, ob ein Benutzer auf die Freigabe zugreifen kann. Die NTFS-Berechtigungen agieren hingegen auf einer detaillierteren Ebene und legen fest, welche Vorgänge der Benutzer auf der Verzeichnis- oder Dateiebene ausführen kann.

Azure Files unterstützt den vollständigen Satz an grundlegenden und erweiterten NTFS-Berechtigungen. Sie können NTFS-Berechtigungen für Verzeichnisse und Dateien in einer Azure-Dateifreigabe anzeigen und konfigurieren, indem Sie die Freigabe einbinden und dann den Windows-Datei-Explorer verwenden oder den Windows-Befehl [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) oder [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-acl) ausführen. 

Um NTFS mit Superuser-Berechtigungen zu konfigurieren, müssen Sie die Freigabe mit Ihrem Speicherkontenschlüssel von Ihrem domänengebundenen virtuellen Computer einbinden. Befolgen Sie die Anweisungen im nächsten Abschnitt, um eine Azure-Dateifreigabe über die Eingabeaufforderung einzubinden und die NTFS-Berechtigungen entsprechend zu konfigurieren.

Die folgenden Berechtigungsgruppen werden im Stammverzeichnis einer Dateifreigabe unterstützt:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\Users:(RX)
- BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
- NT AUTHORITY\Authenticated Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- CREATOR OWNER:(OI)(CI)(IO)(F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Bereitstellen einer Dateifreigabe über die Eingabeaufforderung

Verwenden Sie den Windows-Befehl **net use**, um die Azure-Dateifreigabe zu aktivieren. Denken Sie daran, die Platzhalterwerte im folgenden Beispiel durch Ihre eigenen Werte zu ersetzen. Weitere Informationen zum Einbinden von Dateifreigaben finden Sie unter [Verwenden einer Azure-Dateifreigabe mit Windows](../articles/storage/files/storage-how-to-use-files-windows.md). 

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> /user:Azure\<storage-account-name> <storage-account-key>
```

Wenn beim Herstellen einer Verbindung mit Azure Files Probleme auftreten, finden Sie weitere Informationen unter [dem Problembehandlungstool, das für Azure Files-Bereitstellungsfehler unter Windows veröffentlicht wurde](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5). Außerdem stehen [Anleitungen](https://docs.microsoft.com/azure/storage/files/storage-files-faq#on-premises-access) zur Umgehung von Szenarien bereit, wenn Port 445 blockiert ist. 


### <a name="configure-ntfs-permissions-with-windows-file-explorer"></a>Konfigurieren von NTFS-Berechtigungen mit dem Windows-Datei-Explorer
Verwenden Sie den Windows-Datei-Explorer, um allen Verzeichnissen und Dateien unter der Dateifreigabe, einschließlich des Stammverzeichnisses, vollständige Berechtigungen zu erteilen.

1. Öffnen Sie Windows-Explorer, klicken Sie mit der rechten Maustaste auf die Datei bzw. das Verzeichnis, und wählen Sie **Eigenschaften** aus.
2. Wählen Sie die Registerkarte **Sicherheit** .
3. Wählen Sie **Bearbeiten** aus, um die Berechtigungen zu ändern.
4. Sie können die Berechtigung bereits vorhandener Benutzer ändern oder **Hinzufügen** auswählen, um neuen Benutzern Berechtigungen zu gewähren.
5. Geben Sie im Eingabeaufforderungsfenster zum Hinzufügen neuer Benutzer im Feld **Geben Sie die Namen der auszuwählenden Objekte ein** den Namen des Zielbenutzers ein, dem Sie die Berechtigung gewähren möchten, und wählen Sie **Namen überprüfen** aus, um den vollständigen UPN-Namen des Zielbenutzers zu ermitteln.
7.    Klicken Sie auf **OK**.
8.    Wählen Sie auf der Registerkarte **Sicherheit** alle Berechtigungen aus, die Sie dem neuen Benutzer gewähren möchten.
9.    Wählen Sie **Übernehmen**.

### <a name="configure-ntfs-permissions-with-icacls"></a>Konfigurieren von NTFS-Berechtigungen mit icacls
Verwenden Sie den folgenden Windows-Befehl, um allen Verzeichnissen und Dateien unter der Dateifreigabe, einschließlich des Stammverzeichnisses, vollständige Berechtigungen zu erteilen. Denken Sie daran, die Platzhalterwerte in diesem Beispiel durch Ihre eigenen Werte zu ersetzen.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Weitere Informationen zur Verwendung von icacls zum Festlegen von NTFS-Berechtigungen und zu den verschiedenen Arten von unterstützten Berechtigungen finden Sie in der [Befehlszeilenreferenz für icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="4-mount-a-file-share-from-a-domain-joined-vm"></a>4 Bereitstellen einer Dateifreigabe von einem in eine Domäne eingebundenen virtuellen Computer

Mit dem folgenden Prozess wird überprüft, ob Ihre Dateifreigabe und Ihre Zugriffsberechtigungen ordnungsgemäß eingerichtet wurden und Sie von einem in die Domäne eingebundenen virtuellen Computer aus Zugriff auf eine Azure-Dateifreigabe haben. Beachten Sie, dass die RBAC-Rollenzuweisung auf Freigabeebene einige Zeit in Anspruch nehmen kann. 

Melden Sie sich mit der Azure AD-Identität, für die Sie Berechtigungen erteilt haben, beim virtuellen Computer an, wie in der folgenden Abbildung gezeigt. Wenn Sie die lokale AD DS Authentifizierung für Azure Files aktiviert haben, verwenden Sie Ihre AD DS-Anmeldeinformationen. Melden Sie sich für die Azure AD DS-Authentifizierung mit Azure AD-Anmeldeinformationen an.

![Screenshot mit dem Azure AD-Anmeldebildschirm für die Benutzerauthentifizierung](media/storage-files-aad-permissions-and-mounting/azure-active-directory-authentication-dialog.png)

Verwenden Sie den folgenden Befehl, um die Azure-Dateifreigabe einzubinden. Denken Sie daran, die Platzhalterwerte durch Ihre eigenen Werte zu ersetzen. Da Sie bereits authentifiziert wurden, müssen Sie weder den Speicherkontoschlüssel noch die lokalen AD DS-Anmeldeinformationen oder die Azure AD DS-Anmeldeinformationen angeben. Einmaliges Anmelden wird für die Authentifizierung mit lokalem AD DS oder Azure AD DS unterstützt. Wenn beim Einbinden von AD DS-Anmeldeinformationen Probleme auftreten, finden Sie Hilfestellung unter [Behandeln von Azure Files-Problemen unter Windows](https://docs.microsoft.com/azure/storage/files/storage-troubleshoot-windows-file-connection-problems).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```
