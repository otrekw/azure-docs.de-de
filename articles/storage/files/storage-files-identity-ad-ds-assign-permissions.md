---
title: 'Steuern des Zugriffs auf Dateifreigaben in Azure: lokale AD DS-Authentifizierung'
description: In diesem Artikel erfahren Sie, wie Sie einer Active Directory Domain Services-Identität, die Ihr Speicherkonto darstellt, Berechtigungen zuweisen. So können Sie den Zugriff über die identitätsbasierte Authentifizierung steuern.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: af88f0b3403fb80acbb7dacebe293ac583e35799
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91716039"
---
# <a name="part-two-assign-share-level-permissions-to-an-identity"></a>Teil 2: Zuweisen von Berechtigungen auf Freigabeebene für eine Identität

Bevor Sie sich diesen Artikel ansehen, sollten Sie den vorherigen Artikel, [Teil 1: Aktivieren der AD DS-Authentifizierung für Ihre Dateifreigaben in Azure](storage-files-identity-ad-ds-enable.md), bereits gelesen haben.

Sobald Sie die Active Directory Domain Services-Authentifizierung (AD DS) für Ihr Speicherkonto aktiviert haben, müssen Sie Berechtigungen auf Freigabeebene konfigurieren, um Zugriff auf Ihre Dateifreigaben zu erhalten. Bei der Identität, mit der Sie auf Ihre Dateifreigaberessourcen in Azure zugreifen möchten, muss es sich um eine hybride Identität handeln, die sowohl in AD DS als auch in Azure AD vorhanden ist. Angenommen, Sie verfügen beispielsweise über den Benutzer user1@onprem.contoso.com in Ihrer Active Directory Domain Services-Instanz. Außerdem haben Sie diesen mithilfe der Azure AD Connect-Synchronisierung mit Azure AD synchronisiert, wo er nun als user1@contoso.com vorhanden ist. Damit dieser Benutzer auf Azure Files zugreifen kann, müssen Sie user1@contoso.com Berechtigungen auf Freigabeebene zuweisen. Dasselbe Konzept gilt für Gruppen oder Dienstprinzipale. Aus diesem Grund müssen Sie die Benutzer und Gruppen in Ihrer AD DS-Instanz mit Azure AD mithilfe der Azure AD Connect-Synchronisierung synchronisieren. 

Der Azure AD-Identität, die für denselben Benutzer oder dieselbe Gruppe in Ihrer AD DS-Instanz steht, müssen Berechtigungen auf Freigabeebene zugewiesen werden, um die AD DS-Authentifizierung für Ihre Dateifreigabe in Azure zu unterstützen. Die Authentifizierung und Autorisierung mit Identitäten, die nur in Azure AD vorhanden sind, z. B. verwaltete Identitäten in Azure, wird bei der AD DS-Authentifizierung nicht unterstützt. In diesem Artikel wird gezeigt, wie einer Identität für eine Dateifreigabe Berechtigungen auf Freigabeebene zugewiesen werden.


## <a name="share-level-permissions"></a>Berechtigungen auf Freigabeebene

Im Allgemeinen wird empfohlen, Berechtigungen auf Freigabeebene für die allgemeine Verwaltung des Zugriffs für eine aus Benutzern und Identitäten bestehende Azure AD-Gruppe zu verwenden und dann Zugriffssteuerungslisten für eine differenzierte Zugriffssteuerung auf der Verzeichnis-/Dateiebene zu nutzen. 

Es gibt drei in Azure integrierte Rollen zum Gewähren von Berechtigungen auf Freigabeebene für Benutzer:

- **Speicherdateidaten-SMB-Freigabeleser** ermöglicht den Lesezugriff auf Azure Storage-Dateifreigaben über SMB.
- **Speicherdateidaten-SMB-Freigabemitwirkender** ermöglicht den Lese-, Schreib- und Löschzugriff auf Azure Storage-Dateifreigaben über SMB.
- **Mitwirkender mit erhöhten Rechten für Speicherdateidaten-SMB-Freigabe** gewährt über Windows-Zugriffssteuerunglisten die Berechtigungen für Lese-, Schreib-, Lösch- und Änderungsvorgänge für Azure Storage-Dateifreigaben über SMB.

> [!IMPORTANT]
> Die vollständige administrative Kontrolle über eine Dateifreigabe, einschließlich der Möglichkeit, den Besitz einer Rolle zu übernehmen, erfordert die Verwendung des Speicherkontenschlüssels. Die administrative Kontrolle wird mit Azure AD-Anmeldeinformationen nicht unterstützt.

Sie können das Azure-Portal, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, um die integrierten Rollen der Azure AD-Identität eines Benutzers zuzuweisen und damit Berechtigungen auf Freigabeebene zu erteilen.

## <a name="assign-an-azure-role"></a>Zuweisen einer Azure-Rolle

### <a name="azure-portal"></a>Azure-Portal

Um einer Azure AD-Identität eine Azure-Rolle zuzuweisen, führen Sie im [Azure-Portal](https://portal.azure.com) die folgenden Schritte aus:

1. Navigieren Sie im Azure-Portal zu Ihrer Dateifreigabe, oder [erstellen Sie eine Dateifreigabe](storage-how-to-create-file-share.md).
1. Wählen Sie **Access Control (IAM)** aus.
1. Auswählen von **Rollenzuweisung hinzufügen**
1. Wählen Sie auf dem Blatt **Rollenzuweisung hinzufügen** in der Liste **Rolle** die entsprechende integrierte Rolle aus (Speicherdateidaten-SMB-Freigabeleser, Speicherdateidaten-SMB-Freigabemitwirkender). Behalten Sie für **Zugriff zuweisen zu** die Standardeinstellung bei: **Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal**. Wählen Sie die Azure AD-Zielidentität anhand des Namens oder der E-Mail-Adresse aus. **Bei der ausgewählten Azure AD-Identität muss es sich um eine hybride Identität handeln. Es kann sich um keine nur in der Cloud verfügbare Identität handeln.** Das bedeutet, dieselbe Identität ist auch in AD DS vorhanden.
1. Wählen Sie abschließend **Speichern** aus, um den Vorgang der Rollenzuweisung abzuschließen.

### <a name="powershell"></a>PowerShell

Das folgende PowerShell-Beispiel zeigt, wie einer Azure AD-Identität basierend auf dem Anmeldenamen eine Azure-Rolle zugewiesen wird. Weitere Informationen zum Zuweisen von Azure-Rollen mit PowerShell finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe des Azure PowerShell-Moduls](../../role-based-access-control/role-assignments-powershell.md).

Denken Sie vor dem Ausführen des folgenden Beispielskripts daran, Platzhalterwerte durch eigene Werte zu ersetzen (einschließlich der Klammern).

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)
  
Der folgende CLI 2.0-Befehl zeigt, wie einer Azure AD-Identität basierend auf dem Anmeldenamen eine Azure-Rolle zugewiesen wird. Weitere Informationen zum Zuweisen von Azure-Rollen mit der Azure CLI finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen mithilfe der Azure CLI](../../role-based-access-control/role-assignments-cli.md). 

Denken Sie vor dem Ausführen des folgenden Beispielskripts daran, Platzhalterwerte durch eigene Werte zu ersetzen (einschließlich der Klammern).

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Berechtigungen auf Freigabeebene zugewiesen haben, müssen Sie Berechtigungen auf Verzeichnis- und Dateiebene konfigurieren. Fahren Sie mit dem nächsten Artikel fort.

[Teil 3: Konfigurieren von Berechtigungen auf Verzeichnis- und Dateiebene über SMB](storage-files-identity-ad-ds-configure-permissions.md)
