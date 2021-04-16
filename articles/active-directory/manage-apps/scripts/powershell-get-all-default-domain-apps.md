---
title: 'PowerShell-Beispiel: Anwendungsproxy-Apps, die Standarddomänen verwenden'
description: Hier finden Sie ein PowerShell-Beispiel, das alle Anwendungsproxyanwendungen von Azure Active Directory (Azure AD) auflistet, die Standarddomänen (.msappproxy.net) verwenden.
services: active-directory
author: kenwith
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 02216711efdc88f3a5255bcdc67074da3f768534
ms.sourcegitcommit: 2654d8d7490720a05e5304bc9a7c2b41eb4ae007
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107377954"
---
# <a name="get-all-application-proxy-apps-using-default-domains-msappproxynet"></a>Abrufen aller Anwendungsproxy-Apps, die Standarddomänen (.msappproxy.net) verwenden

Dieses PowerShell-Beispiel listet alle Anwendungsproxyanwendungen von Azure Active Directory (Azure AD) auf, die Standarddomänen (.msappproxy.net) verwenden.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-default-domain-apps.ps1 "Get all Application Proxy apps using default domains (.msappproxy.net)")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Ruft einen Dienstprinzipal ab. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Ruft eine Azure AD-Anwendung ab. |
|[Get-AzureADApplicationProxyApplication](/powershell/module/azuread/get-azureadapplicationproxyapplication) | Ruft eine für den Anwendungsproxy in Azure AD konfigurierte Anwendung ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).
