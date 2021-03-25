---
title: 'PowerShell-Beispiel: Auflisten aller Anwendungsproxy-Apps mit einer Richtlinie'
description: Hier finden Sie ein PowerShell-Beispiel, das alle in Ihrem Verzeichnis enthaltenen Anwendungsproxyanwendungen von Azure Active Directory (Azure AD) auflistet, die über eine Richtlinie für die Tokengültigkeitsdauer verfügen.
services: active-directory
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 12/05/2019
ms.author: kenwith
ms.reviewer: japere
ms.openlocfilehash: 67a521886f5ff7e7e68f82c80004e0cf2649e652
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102565651"
---
# <a name="get-all-application-proxy-apps-with-a-token-lifetime-policy"></a>Abrufen aller Anwendungsproxy-Apps mit einer Richtlinie für die Tokengültigkeitsdauer

Mit diesem PowerShell-Beispielskript werden alle in Ihrem Verzeichnis enthaltenen Anwendungsproxyanwendungen von Azure Active Directory (Azure AD) mit einer Richtlinie für die Tokengültigkeitsdauer sowie Details zur Richtlinie aufgelistet.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-with-policy.ps1 "Get all Application Proxy apps with a token lifetime policy")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Ruft einen Dienstprinzipal ab. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Ruft eine Azure AD-Anwendung ab. |
|[Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview) | Ruft eine Richtlinie in Azure AD ab. |
|[Get-AzureADServicePrincipalPolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview) | Ruft die Richtlinie eines Dienstprinzipals in Azure AD ab. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).
