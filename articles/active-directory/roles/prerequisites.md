---
title: Voraussetzungen für die Verwendung von PowerShell oder Graph-Tester für Azure AD-Rollen – Azure Active Directory
description: In diesem Artikel finden Sie Voraussetzungen für die Verwendung von PowerShell oder Graph-Tester für Azure Active Directory-Rollen.
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.service: active-directory
ms.topic: how-to
ms.subservice: roles
ms.workload: identity
ms.date: 05/13/2021
ms.author: rolyon
ms.reviewer: anandy
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2b67f3514246bcac1185370393ac469f8c96cf3
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111890616"
---
# <a name="prerequisites-to-use-powershell-or-graph-explorer-for-azure-ad-roles"></a>In diesem Artikel finden Sie Voraussetzungen für die Verwendung von PowerShell oder Graph-Tester für Azure AD-Rollen

Wenn Sie Azure AD-Rollen (Azure Active Directory) mithilfe von PowerShell oder Graph-Tester verwalten möchten, müssen Sie die erforderlichen Voraussetzungen erfüllen. In diesem Artikel werden die PowerShell- und Graph-Tester-Voraussetzungen für verschiedene Azure AD-Rollenfeatures beschrieben.

## <a name="azuread-module"></a>AzureAD-Modul

PowerShell-Befehle für Folgendes:

- Auflisten der Rollenzuweisungen
- Erstellen einer Gruppe, der Rollen zugeordnet werden können
- Verwalten von Verwaltungseinheiten

Sie müssen über eine Installation des folgenden Moduls verfügen:

- [AzureAD](https://www.powershellgallery.com/packages/AzureAD), Version 2.0.2.130 oder höher


#### <a name="check-azuread-version"></a>Überprüfen der AzureAD-Version

Wenn Sie überprüfen möchten, welche AzureAD-Version installiert ist, verwenden Sie [Get-InstalledModule](/powershell/module/powershellget/get-installedmodule).

```powershell
Get-InstalledModule -Name AzureAD
```

Die Ausgabe sollte etwa folgendermaßen aussehen:

```powershell
Version    Name                                Repository           Description
-------    ----                                ----------           -----------
2.0.2.130  AzureAD                             PSGallery            Azure Active Directory V2 General Availability M...
```

#### <a name="install-azuread"></a>Installieren von AzureAD

Wenn AzureAD noch nicht installiert ist, verwenden Sie [Install-Module](/powershell/module/powershellget/install-module) zum Installieren von AzureAD.

```powershell
Install-Module -Name AzureAD
```

#### <a name="update-azuread"></a>Aktualisieren von AzureAD

Wenn Sie AzureAD auf die aktuelle Version aktualisieren möchten, führen Sie [Install-Module](/powershell/module/powershellget/install-module) noch mal aus.

```powershell
Install-Module -Name AzureAD
```

#### <a name="use-azuread"></a>Verwenden von AzureAD

Wenn Sie AzureAD verwenden möchten, befolgen Sie die folgenden Schritte, um sicherzustellen, dass AzureAD in die aktuelle Sitzung importiert wurde.

1. Verwenden Sie [Get-Module](/powershell/module/microsoft.powershell.core/get-module), um zu überprüfen, ob AzureAD in den Arbeitsspeicher geladen wurde.

    ```powershell
    Get-Module -Name AzureAD
    ```

1. Wenn beim vorherigen Schritt keine Ausgabe angezeigt wird, verwenden Sie [Import-Module](/powershell/module/microsoft.powershell.core/import-module), um AzureAD zu importieren. Der Parameter `-Force` entfernt das geladene Modul und importiert es anschließend noch mal.

    ```powershell
    Import-Module -Name AzureAD -Force
    ```

1. Führen Sie [Get-Module](/powershell/module/microsoft.powershell.core/get-module) noch mal aus.

    ```powershell
    Get-Module -Name AzureAD
    ```

    Die Ausgabe sollte etwa folgendermaßen aussehen:
    
    ```powershell
    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     2.0.2.130  AzureAD                             {Add-AzureADApplicationOwner, Add-AzureADDeviceRegisteredO...
    ```

## <a name="azureadpreview-module"></a>AzureADPreview-Modul

PowerShell-Befehle für Folgendes:

- Zuweisen von Rollen zu Benutzern oder Gruppen
- Entfernen einer Rollenzuweisung
- Berechtigen einer Gruppe für eine Rolle mithilfe von Privileged Identity Management
- Erstellen von benutzerdefinierten Rollen

Sie müssen über eine Installation des folgenden Moduls verfügen:

- [AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview), Version 2.0.2.129 oder höher


#### <a name="check-azureadpreview-version"></a>Überprüfen der AzureADPreview-Version

Wenn Sie überprüfen möchten, welche AzureADPreview-Version installiert ist, verwenden Sie [Get-InstalledModule](/powershell/module/powershellget/get-installedmodule).

```powershell
Get-InstalledModule -Name AzureADPreview
```

Die Ausgabe sollte etwa folgendermaßen aussehen:

```powershell
Version    Name                                Repository           Description
-------    ----                                ----------           -----------
2.0.2.129  AzureADPreview                      PSGallery            Azure Active Directory V2 Preview Module. ...
```

#### <a name="install-azureadpreview"></a>Installieren von AzureADPreview

Wenn AzureADPreview noch nicht installiert ist, verwenden Sie [Install-Module](/powershell/module/powershellget/install-module) zum Installieren von AzureADPreview.

```powershell
Install-Module -Name AzureADPreview
```

#### <a name="update-azureadpreview"></a>Aktualisieren von AzureADPreview

Wenn Sie AzureADPreview auf die aktuelle Version aktualisieren möchten, führen Sie [Install-Module](/powershell/module/powershellget/install-module) noch mal aus.

```powershell
Install-Module -Name AzureADPreview
```

#### <a name="use-azureadpreview"></a>Verwenden von AzureADPreview

Wenn Sie AzureADPreview verwenden möchten, befolgen Sie die folgenden Schritte, um sicherzustellen, dass AzureADPreview in die aktuelle Sitzung importiert wurde.

1. Verwenden Sie [Get-Module](/powershell/module/microsoft.powershell.core/get-module), um zu überprüfen, ob AzureADPreview in den Arbeitsspeicher geladen wurde.

    ```powershell
    Get-Module -Name AzureADPreview
    ```

1. Wenn beim vorherigen Schritt keine Ausgabe angezeigt wird, verwenden Sie [Import-Module](/powershell/module/powershellget/import-module), um AzureADPreview zu importieren. Der Parameter `-Force` entfernt das geladene Modul und importiert es anschließend noch mal.

    ```powershell
    Import-Module -Name AzureADPreview -Force
    ```

1. Führen Sie [Get-Module](/powershell/module/microsoft.powershell.core/get-module) noch mal aus.

    ```powershell
    Get-Module -Name AzureADPreview
    ```

    Die Ausgabe sollte etwa folgendermaßen aussehen:
    
    ```powershell
    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     2.0.2.129  AzureADPreview                      {Add-AzureADAdministrativeUnitMember, Add-AzureADApplicati...
    ```

## <a name="graph-explorer"></a>Graph-Explorer

Wenn Sie Azure AD-Rollen mithilfe der [Microsoft Graph-API](/graph/overview) und dem [Graph-Tester](/graph/graph-explorer/graph-explorer-overview) verwalten möchten, sind die folgenden Schritte erforderlich:

1. Öffnen Sie **Azure Active Directory** im Azure-Portal.

1. Klicken Sie auf **Unternehmensanwendungen**.

1. Suchen Sie in der Anwendungsliste nach **Graph-Tester**, und klicken Sie darauf.

1. Klicken Sie auf **Berechtigungen**.

1. Klicken Sie auf **Administratorzustimmung für Graph-Tester erteilen**.

    ![Screenshot: Link „Grant admin consent for Graph explorer“ (Administratoreinwilligung für Graph-Tester erteilen)](./media/prerequisites/select-graph-explorer.png)

1. Verwenden Sie das [Tool „Graph-Tester“](https://aka.ms/ge).

## <a name="next-steps"></a>Nächste Schritte

- [Installieren von Azure Active Directory PowerShell für Graph](/powershell/azure/active-directory/install-adv2)
- [AzureAD](/powershell/module/azuread/)
- [Graph-Explorer](/graph/graph-explorer/graph-explorer-overview)
