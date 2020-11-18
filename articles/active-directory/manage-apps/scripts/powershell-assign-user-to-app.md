---
title: 'PowerShell-Beispiel: Zuweisen eines Benutzers zu einer Anwendungsproxy-App'
description: Hier finden Sie ein PowerShell-Beispiel, in dem einer Anwendungsproxyanwendung von Azure Active Directory (Azure AD) ein Benutzer zugewiesen wird.
services: active-directory
author: kenwith
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: eb8375c4e6ef3af90d5f533bc608046802eec736
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658841"
---
# <a name="assign-a-user-to-a-specific-azure-ad-application-proxy-application"></a>Zuweisen eines Benutzers zu einer bestimmten Azure AD-Anwendungsproxyanwendung

In diesem Beispiel für ein PowerShell-Skript können Sie einer bestimmten Azure AD-Anwendungsproxyanwendung einen Benutzer zuweisen.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/assign-user-to-app.ps1 "Assign a user to an application")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
| [New-AzureADUserAppRoleAssignment](/powershell/module/AzureAD/new-azureaduserapproleassignment?view=azureadps-2.0) | Weist einer Anwendungsrolle einen Benutzer zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview?view=azureadps-2.0).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).