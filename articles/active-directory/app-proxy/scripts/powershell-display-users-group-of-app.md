---
title: 'PowerShell-Beispiel: Auflisten von Benutzern und Gruppen für eine Azure Active Directory-Anwendungsproxy-App'
description: Hier finden Sie ein PowerShell-Beispiel, das alle einer bestimmten Anwendungsproxyanwendung von Azure Active Directory (Azure AD) zugewiesenen Benutzer und Gruppen auflistet.
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: sample
ms.date: 04/29/2021
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 309a9e5c7dc3a766751af235c75e25bd3d94cc3c
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108293697"
---
# <a name="display-users-and-groups-assigned-to-an-application-proxy-application"></a>Anzeigen der einer Anwendungsproxyanwendung zugewiesenen Benutzer und Gruppen

Mit diesem PowerShell-Beispielskript werden die einer bestimmten Anwendungsproxyanwendung von Azure Active Directory (Azure AD) zugewiesenen Benutzer und Gruppen aufgelistet.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/display-users-group-of-an-app.ps1 "Display users and groups assigned to an Application Proxy application")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
| [Get-AzureADUser](/powershell/module/AzureAD/get-azureaduser)| Ruft einen Benutzer ab. |
| [Get-AzureADGroup](/powershell/module/AzureAD/get-azureadgroup)| Ruft eine Gruppe ab. |
| [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Ruft einen Dienstprinzipal ab. |
| [Get-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/get-azureaduserapproleassignment) | Hiermit rufen Sie die Anwendungsrollenzuweisung eines Benutzers ab. |
| [Get-AzureADGroupAppRoleAssignment](/powershell/module/AzureAD/get-azureadgroupapproleassignment) | Hiermit rufen Sie die Anwendungsrollenzuweisung einer Gruppe ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).
