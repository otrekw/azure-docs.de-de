---
title: Verwalten von Benutzer- und Administratorberechtigungen – Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Berechtigungen für Ihre Anwendung in Azure AD überprüfen und verwalten. Dies gilt für das Szenario, dass Sie beispielsweise alle Berechtigungen, die einer Anwendung erteilt wurden, widerrufen möchten.
services: active-directory
author: mimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 7/10/2020
ms.author: mimart
ms.reviewer: luleonpla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00d878c7b2f78d037e89235f3bb30c02fd11a7ae
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2020
ms.locfileid: "86277598"
---
# <a name="take-action-on-overpriviledged-or-suspicious-application-in-azure-active-directory"></a>Maßnahmen gegenüber verdächtigen Anwendungen oder Anwendungen mit zu vielen Berechtigungen in Azure Active Directory

Hier erfahren Sie, wie Sie Anwendungsberechtigungen überprüfen und verwalten. Basierend auf dem Szenario werden in diesem Artikel verschiedene Aktionen beschrieben, die Sie zum Schutz Ihrer Anwendung ausführen können. Dies gilt für alle Anwendungen, die Ihrem Azure Active Directory-Mandanten (Azure AD) per Benutzer- oder Administratoreinwilligung hinzugefügt wurden.

Weitere Informationen zur Einwilligung für Anwendungen finden Sie unter [Azure Active Directory-Zustimmungsframework](../develop/consent-framework.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen sich als globaler Administrator, Anwendungsadministrator oder Cloudanwendungsadministrator anmelden, um die folgenden Aktionen ausführen zu können.

Um den Zugriff auf Anwendungen zu beschränken, benötigen Sie die Benutzerzuweisung, und weisen Sie dann Benutzer oder Gruppen der Anwendung zu.  Weitere Informationen finden Sie unter [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Azure Active Directory](methods-for-assigning-users-and-groups.md).

Über das Azure AD-Portal können Sie kontextbezogene PowerShell-Skripts abrufen, um die Aktionen auszuführen.
 
1. Melden Sie sich globaler Administrator, als Anwendungsadministrator oder als Cloudanwendungsadministrator beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus.
3. Wählen Sie die Anwendung aus, deren Zugriffsberechtigungen Sie einschränken möchten.
4. Wählen Sie **Berechtigungen** aus. Klicken Sie in der Befehlsleiste auf **Berechtigungen überprüfen**.

![Berechtigungen überprüfen](./media/manage-application-permissions/review-permissions.png)

## <a name="i-want-to-control-access-to-an-application"></a>Steuern des Zugriffs auf eine Anwendung

Wenn Sie steuern möchten, wer auf eine Anwendung zugreifen kann, aktivieren Sie die Einstellung „Benutzerzuweisung“.

1. Melden Sie sich globaler Administrator, als Anwendungsadministrator oder als Cloudanwendungsadministrator beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus.
3. Wählen Sie die Anwendung aus, deren Zugriffsberechtigungen Sie einschränken möchten.
4. Klicken Sie auf **Eigenschaften**, und legen Sie die Einstellung „Benutzerzuweisung erforderlich?“ auf „Ja“ fest.
5. Klicken Sie auf **Benutzer und Gruppen**, und entfernen Sie unerwünschte Benutzer.
6. Weisen Sie der Anwendung Benutzer oder Gruppen zu.

Optional können Sie auch mithilfe von PowerShell alle Benutzer entfernen, die der Anwendung zugewiesen sind.

## <a name="i-want-to-revoke-all-permissions-for-an-application"></a>Widerrufen aller Berechtigungen für eine Anwendung

Mithilfe von PowerShell können Sie alle Berechtigungen widerrufen, die der Anwendung erteilt wurden.

> [!NOTE]
> Durch das Widerrufen aktueller Berechtigungen wird nicht verhindert, dass Benutzer einer Anwendung erneut zustimmen. Wenn Sie verhindern möchten, dass Benutzer einer Anwendung zustimmen, lesen Sie [Konfigurieren der Art und Weise, wie Benutzer Anwendungen zustimmen können](configure-user-consent.md).

Optional können Sie die Anwendung deaktivieren, um zu verhindern, dass Benutzer auf die App und die App auf Ihre Daten zugreifen können.

1. Melden Sie sich globaler Administrator, als Anwendungsadministrator oder als Cloudanwendungsadministrator beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus.
3. Wählen Sie die Anwendung aus, deren Zugriffsberechtigungen Sie einschränken möchten.
4. Klicken Sie auf **Eigenschaften**, und legen Sie die Einstellung „Aktiviert für die Benutzeranmeldung?“ auf „Nein“ fest.

## <a name="application-is-suspicious-and-i-want-to-investigate"></a>Untersuchen verdächtiger Anwendungen

Bei einer verdächtigen Anwendung sollten Sie den Zugriff darauf einschränken, indem Sie die Einstellung „Benutzerzuweisung“ aktivieren. Zudem sollten Sie die Berechtigungen überprüfen, die Benutzer und Administratoren der Anwendung erteilt haben.

1. Melden Sie sich globaler Administrator, als Anwendungsadministrator oder als Cloudanwendungsadministrator beim [Azure-Portal](https://portal.azure.com) an.
3. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus.
5. Wählen Sie die Anwendung aus, deren Zugriffsberechtigungen Sie einschränken möchten.
6. Klicken Sie auf **Eigenschaften**, und legen Sie die Einstellung „Benutzerzuweisung erforderlich?“ auf „Ja“ fest.
7. Klicken Sie auf **Berechtigungen**, und überprüfen Sie die per Administrator- und Benutzereinwilligung erteilten Berechtigungen.

Optional können Sie folgende Aktionen ausführen:

- Entfernen Sie mithilfe von PowerShell alle zugewiesenen Benutzer, um eine erneute Anmeldung bei der Anwendung zu verhindern.
- Annullieren Sie mithilfe von PowerShell die Aktualisierungstoken für Benutzer, die auf die Anwendung zugreifen können.
- Widerrufen Sie mithilfe von PowerShell alle Berechtigungen für diese Anwendung.
- Deaktivieren Sie die Anwendung, um zu verhindern, dass Benutzer auf die Anwendung und die Anwendung auf Ihre Daten zugreifen.


## <a name="application-is-malicious-and-im-compromised"></a>Vorgehen bei bösartigen Anwendungen

Wurde die Anwendung kompromittiert, sollten Sie sie deaktivieren, um zu verhindern, dass Benutzer auf die App und die App auf Ihre Daten zugreifen können. Wenn Sie stattdessen die Anwendung löschen, können Benutzer der Anwendung erneut zustimmen und Zugriff auf Ihre Daten gewähren.

1. Melden Sie sich globaler Administrator, als Anwendungsadministrator oder als Cloudanwendungsadministrator beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus.
3. Wählen Sie die Anwendung aus, deren Zugriffsberechtigungen Sie einschränken möchten.
4. Klicken Sie auf **Eigenschaften**, und kopieren Sie die Objekt-ID.

### <a name="powershell-commands"></a>PowerShell-Befehle


Rufen Sie die Objekt-ID des Dienstprinzipals ab.

1. Melden Sie sich globaler Administrator, als Anwendungsadministrator oder als Cloudanwendungsadministrator beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** aus.
3. Wählen Sie die Anwendung aus, deren Zugriffsberechtigungen Sie einschränken möchten.
4. Klicken Sie auf **Eigenschaften**, und kopieren Sie die Objekt-ID.

```powershell
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $sp.ObjectId
```
Entfernen Sie alle Benutzer, die der Anwendung zugewiesen sind.
 ```powershell
    Connect-AzureAD

    # Get Service Principal using objectId
    $sp = Get-AzureADServicePrincipal -ObjectId "<ServicePrincipal objectID>"

    # Get Azure AD App role assignments using objectId of the Service Principal
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -All $true

    # Remove all users and groups assigned to the application
    $assignments | ForEach-Object {
        if ($_.PrincipalType -eq "User") {
            Remove-AzureADUserAppRoleAssignment -ObjectId $_.PrincipalId -AppRoleAssignmentId $_.ObjectId
        } elseif ($_.PrincipalType -eq "Group") {
            Remove-AzureADGroupAppRoleAssignment -ObjectId $_.PrincipalId -AppRoleAssignmentId $_.ObjectId
        }
    }
 ```

Widerrufen Sie die Berechtigungen, die der Anwendung erteilt wurden.

```powershell
    Connect-AzureAD

    # Get Service Principal using objectId
    $sp = Get-AzureADServicePrincipal -ObjectId "<ServicePrincipal objectID>"

    # Get all delegated permissions for the service principal
    $spOAuth2PermissionsGrants = Get-AzureADOAuth2PermissionGrant -All $true| Where-Object { $_.clientId -eq $sp.ObjectId }

    # Remove all delegated permissions
    $spOAuth2PermissionsGrants | ForEach-Object {
        Remove-AzureADOAuth2PermissionGrant -ObjectId $_.ObjectId
    }

    # Get all application permissions for the service principal
    $spApplicationPermissions = Get-AzureADServiceAppRoleAssignedTo -ObjectId $sp.ObjectId -All $true | Where-Object { $_.PrincipalType -eq "ServicePrincipal" }

    # Remove all delegated permissions
    $spApplicationPermissions | ForEach-Object {
        Remove-AzureADServiceAppRoleAssignment -ObjectId $_.PrincipalId -AppRoleAssignmentId $_.objectId
    }
```
Annullieren Sie Aktualisierungstoken.
```powershell
        Connect-AzureAD

        # Get Service Principal using objectId
        $sp = Get-AzureADServicePrincipal -ObjectId "<ServicePrincipal objectID>"

        # Get Azure AD App role assignments using objectID of the Service Principal
        $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -All $true | Where-Object {$_.PrincipalType -eq "User"}

        # Revoke refresh token for all users assigned to the application
        $assignments | ForEach-Object {
            Revoke-AzureADUserAllRefreshToken -ObjectId $_.PrincipalId
        }
```
## <a name="next-steps"></a>Nächste Schritte
- [Verwalten der Einwilligung zu Anwendungen und Auswerten von Einwilligungsanforderungen](manage-consent-requests.md)
- [Konfigurieren der Benutzereinwilligung](configure-user-consent.md)
- [Konfigurieren des Workflows für die Administratoreinwilligung](configure-admin-consent-workflow.md)
