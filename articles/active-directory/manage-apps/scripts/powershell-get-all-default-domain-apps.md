---
title: 'PowerShell-Beispiel: Anwendungsproxy-Apps, die Standarddomänen verwenden'
description: Hier finden Sie ein PowerShell-Beispiel, das alle Anwendungsproxyanwendungen von Azure Active Directory (Azure AD) auflistet, die Standarddomänen (.msappproxy.net) verwenden.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40a9f7ce86981d6f03b25237a7a73869fd5fd4ba
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75476028"
---
# <a name="get-all-application-proxy-apps-using-default-domains-msappproxynet"></a>Abrufen aller Anwendungsproxy-Apps, die Standarddomänen (.msappproxy.net) verwenden

Dieses PowerShell-Beispiel listet alle Anwendungsproxyanwendungen von Azure Active Directory (Azure AD) auf, die Standarddomänen (.msappproxy.net) verwenden.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) oder das [Azure AD PowerShell V2-Modul für Graph in der Vorschauversion](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-default-domain-apps.ps1 "Get all Application Proxy apps using default domains (.msappproxy.net)")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](https://docs.microsoft.com/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Ruft einen Dienstprinzipal ab. |
|[Get-AzureADApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Ruft eine Azure AD-Anwendung ab. |
|[Get-AzureADApplicationProxyApplication](https://docs.microsoft.com/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | Ruft eine für den Anwendungsproxy in Azure AD konfigurierte Anwendung ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).