---
title: 'PowerShell-Beispiel: Auflisten von zusätzlichen Informationen zu Anwendungsproxy-Apps'
description: Hier finden Sie ein PowerShell-Beispiel, mit dem alle Azure Active Directory-Anwendungsproxyanwendungen (Azure AD) zusammen mit der Anwendungs-ID (AppId), dem Namen (DisplayName), der externen URL (ExternalUrl), der internen URL (InternalUrl) und dem Authentifizierungstyp (ExternalAuthenticationType) aufgelistet werden.
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
ms.openlocfilehash: cd1120874ca9ee0d3ef65417d6a200ffd0a40fdb
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659113"
---
# <a name="get-all-application-proxy-apps-and-list-extended-information"></a>Abrufen aller Anwendungsproxy-Apps und Auflisten zusätzlicher Informationen

Dieses PowerShell-Beispielskript listet Informationen zu allen Azure Active Directory-Anwendungsproxyanwendungen (Azure AD) einschließlich der Anwendungs-ID (AppId), des Namens (DisplayName), der externen URL (ExternalUrl), der internen URL (InternalUrl) und des Authentifizierungstyps (ExternalAuthenticationType) auf.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-extended.ps1 "Get all Application Proxy apps")]

## <a name="script-explanation"></a>Erläuterung des Skripts

| Get-Help | Notizen |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal?view=azureadps-2.0) | Ruft einen Dienstprinzipal ab. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication?view=azureadps-2.0) | Ruft eine Azure AD-Anwendung ab. |
|[Get-AzureADApplicationProxyApplication](/powershell/module/azuread/get-azureadapplicationproxyapplication?view=azureadps-2.0) | Ruft eine für den Anwendungsproxy in Azure AD konfigurierte Anwendung ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview?view=azureadps-2.0).

Weitere PowerShell-Beispiele für den Anwendungsproxy finden Sie unter [Azure AD PowerShell-Beispiele für Azure AD-Anwendungsproxy](../application-proxy-powershell-samples.md).